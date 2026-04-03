---
title: "Bản đề xuất"
date: 2026-03-26
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# GuardScript — Nền tảng bảo vệ mã nguồn
## Đề xuất dự án

**Thông tin nhóm thực hiện:**

| Họ tên | MSSV | Vai trò |
|---|---|---|
| Võ Tấn Phát | SE194484 | Nhóm trưởng |
| Bùi Minh Hiển | SE190829 | Thành viên |
| Dương Nguyên Bình | SE194067 | Thành viên |
| Trần Vinh | SE193927 | Thành viên |
| Nguyễn Duy Tùng | SE196572 | Thành viên |
| Nguyễn Đức Trí | SE194091 | Thành viên |

**AWS Services:** `Lambda` · `DynamoDB` · `S3` · `CloudFront` · `CloudWatch` · `API Gateway WebSocket` · `SES` · `SNS` · `WAF` · `ACM`

---

### 1. Tóm tắt điều hành

**GuardScript** là nền tảng phân phối script đa ngôn ngữ theo cơ chế loader có kiểm soát truy cập. Người dùng cuối không nhận source code theo cách phát hành truyền thống, mà lấy script qua endpoint có xác thực chữ ký, kiểm tra thời gian yêu cầu, license, HWID và access policy.

Hệ thống được triển khai theo kiến trúc serverless trên AWS nhằm giảm chi phí vận hành, dễ mở rộng, và phù hợp với mục tiêu workshop cloud của FCJ.

---

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Trong thực tế phát triển script, nhóm gặp ba vấn đề chính:

1. Mã nguồn rất dễ bị rò rỉ hoặc sao chép khi được chia sẻ thủ công.
2. Thiếu cơ chế phân quyền chi tiết cho từng người dùng hoặc từng nhóm.
3. Khó theo dõi phiên bản, lịch sử truy cập và quá trình sử dụng.
4. Thiếu cơ chế quản lý tập trung, dẫn đến dữ liệu bị phân mảnh.
5. Hạn chế về khả năng kiểm toán, giám sát và các cơ chế bảo vệ.
6. Hệ thống triển khai và bảo mật thường phức tạp và tốn kém.

#### Giải pháp

Hệ thống đề xuất giải quyết bằng:

1. Luồng phân phối script qua loader thay vì phát hành source trực tiếp.
2. Hai giao thức tải code: v2 (XOR + HMAC verification) và v3 (X25519 ECDH + AES-GCM).
3. License management với HWID lock/reset, access list blacklist/whitelist.
4. Monitoring dựa trên CloudWatch alarms/dashboard và logging nghiệp vụ.

#### Lợi ích và ROI

1. Tăng kiểm soát truy cập script theo license và thiết bị.
2. Giảm rủi ro lạm dụng endpoint nhờ chữ ký request, timestamp/nonce, rate-limit.
3. Duy trì chi phí phù hợp môi trường demo/sinh viên với kiến trúc pay-per-use.

---

### 3. Kiến trúc giải pháp

Hệ thống triển khai theo mô hình serverless với hai lớp chính: phân phối frontend qua edge và xử lý API trên Lambda.

**Luồng request tiêu biểu:**
```
Client (browser / loader)
  → CloudFront Distribution (SSL termination, cache layer)
    → Static assets: S3 bucket (frontend)
    → API /api/*, /files/*: Lambda Function URL origin
      → DynamoDB (users, workspaces, projects, licenses, logs, rate_limits, ...)
      → S3 (nội dung script/object)
  → API Gateway WebSocket (real-time)
  → CloudWatch Logs / Alarms / Dashboard
```
![Kiến trúc hệ thống GuardScript](/images/2-Proposal/1.jpg)

#### Dịch vụ AWS sử dụng

| Tầng | Dịch vụ | Chi tiết |
|---|---|---|
| Runtime API | AWS Lambda (Node.js 20.x) | API backend dạng modular monolith |
| Database | Amazon DynamoDB | Multi-table, PAY_PER_REQUEST, có TTL cho dữ liệu tạm |
| Object Storage | Amazon S3 | Lưu frontend và content/script object |
| CDN & Edge | Amazon CloudFront | Route static + cache + behavior cho /api/* và /files/* |
| Edge Security | AWS WAF | Bảo vệ chống request độc hại tại CloudFront edge |
| TLS/SSL | AWS Certificate Manager | Quản lý chứng chỉ SSL/TLS cho HTTPS |
| Thông báo | Amazon SNS / SES | Gửi cảnh báo và email (mời thành viên, alarms) |
| Monitoring | Amazon CloudWatch | Alarms cho errors/throttles/p95, dashboard vận hành |
| Real-time | API Gateway WebSocket API | Đồng bộ sự kiện workspace/user/admin |
| CI/CD | GitHub Actions + SAM | Deploy hạ tầng và đồng bộ frontend |

#### Thiết kế thành phần

1. **Client Layer**: dashboard web và loader clients (Python/Node/Lua).
2. **Edge Layer**: CloudFront phân tuyến static và API/files.
3. **Compute Layer**: Lambda xử lý auth, workspace, project, license, loader protocols.
4. **Data Layer**: DynamoDB + S3.
5. **Observability Layer**: CloudWatch logs/alarms/dashboard và bảng logs nghiệp vụ.

---

### 4. Triển khai kỹ thuật

#### 4.1. Hệ thống xác thực

Token được tự implement theo cấu trúc `v2.<payload_base64url>.<signature_base64url>`:

1. Token ký bằng HMAC-SHA256 với secret lưu trong bảng app config.
2. Password hash dùng PBKDF2-SHA256 (210000 iterations).
3. So sánh chữ ký bằng timing-safe compare.
4. PIN workspace có session token riêng với TTL.

#### 4.2. Hai protocol phân phối code

**Protocol v2 — GET /api/v5/execute (XOR)**

1. Client gửi `id`, `license`, `HWID`, `timestamp`, `nonce`, `signature`.
2. Server kiểm tra timestamp ±300s, signature và rate-limit.
3. Script trả về được mã hóa XOR và có response signature.

**Protocol v3 — POST /api/v5/handshake (ECDH + AES-GCM)**

1. Client tạo public key X25519 và gửi lên endpoint handshake.
2. Server tạo keypair phiên, derive shared secret, derive AES key, mã hóa nội dung bằng AES-GCM.
3. Response gồm server public key, encrypted payload, timestamp và chữ ký.

#### 4.3. Thuật toán và chuẩn bảo mật

| Thuật toán | Ứng dụng cụ thể |
|---|---|
| PBKDF2-SHA256 (210.000 iter) | Băm mật khẩu người dùng, băm Workspace PIN |
| HMAC-SHA256 | Ký auth token và ký request/response loader |
| ECDH X25519 | Trao đổi khóa Protocol v3 (handshake) |
| HKDF-SHA256 | Dẫn xuất AES key từ ECDH shared secret (RFC 5869) |
| AES-256-GCM | Mã hóa script lưu S3 + mã hóa truyền tải Protocol v3 |
| XOR + SHA-256 | Mã hóa truyền tải Protocol v2 |
| crypto.timingSafeEqual | So sánh token/signature — chống timing attack |
| Nonce + Timestamp ±5 phút | Chống replay attack trên mọi request loader |
| S3 SSE-S3 (AES256) | Mã hóa object mặc định theo template hạ tầng |

#### 4.4. Cơ sở dữ liệu — Amazon DynamoDB

| Bảng DynamoDB | Partition Key | GSI | Ghi chú |
|---|---|---|---|
| users | id | EmailIndex(email) | Tài khoản người dùng, role, password_changed_at |
| workspaces | id | OwnerIndex(user_id), LoaderKeyIndex(loader_key) | Workspace, loader_key, PIN hash |
| projects | id | WorkspaceIndex(workspace_id), SecretKeyIndex(secret_key) | Project (script), cài đặt bảo mật, execution count |
| project_files | id | ProjectIndex(project_id), ParentIndex(parent_id) | File/folder trong project, entry point, sort_order |
| licenses | id | WorkspaceIndex(workspace_id), KeyIndex(key), ProjectIndex(project_id) | License key, HWID, ngày hết hạn, usage count |
| access_lists | id | WorkspaceIndex(workspace_id) | IP blacklist / whitelist theo workspace |
| workspace_members | id | WorkspaceIndex(workspace_id), UserIndex(user_id) | Thành viên được mời, vai trò |
| workspace_invitations | id | WorkspaceIndex, TokenIndex, EmailIndex — **TTL tự động** | Token mời thành viên |
| pin_verifications | token | WorkspaceIndex(workspace_id) — **TTL tự động** | Session token sau xác thực PIN |
| logs | id | WorkspaceIndex(workspace_id), WorkspaceTimestampIndex(workspace_id + timestamp) | Nhật ký sự kiện, hỗ trợ truy vấn theo khoảng thời gian |
| app_config | key | — | Cấu hình hệ thống (HMAC secret, loader secret…) |
| rate_limits | key | — **TTL tự động** | Sliding window rate limiting |
| websocket_connections | connection_id | UserIndex(user_id), WorkspaceIndex(workspace_id) — **TTL tự động** | Các kết nối WebSocket đang hoạt động |
| admin_audit | id | ActorIndex(actor_user_id), TargetIndex(target_id) | Nhật ký hành động admin |

> **Lưu ý thiết kế**: TTL được bật trên `workspace_invitations`, `pin_verifications`, `rate_limits`, và `websocket_connections` để tự động xóa records hết hạn/đã ngắt kết nối mà không cần cronjob. `WorkspaceTimestampIndex` trên bảng `logs` hỗ trợ truy vấn phân tích theo khoảng thời gian.

#### 4.5. Các giai đoạn phát triển

Dự án áp dụng phương pháp **Agile Scrum** với 6 sprint (mỗi sprint 1 tuần):

**Sprint 1 — Phân tích & Thiết kế kiến trúc**
- Xác định yêu cầu hệ thống
- Thiết kế kiến trúc AWS
- Thiết kế schema database và API

**Sprint 2 — Nền tảng Backend**
- Thiết lập cấu trúc project Lambda
- Triển khai Auth, User, Workspace APIs
- Tích hợp DynamoDB

**Sprint 3 — Quản lý Script & File**
- CRUD cho project, file và content
- Tích hợp S3 cho upload/download
- Cấu trúc file tree và bundling

**Sprint 4 — Bảo mật & Kiểm soát truy cập**
- Triển khai access control và licensing
- Tích hợp loader bảo vệ script (Protocol v2/v3)
- Xây dựng audit logging

**Sprint 5 — Frontend & Realtime**
- Phát triển dashboard UI
- Tích hợp APIs
- Cấu hình WebSocket cho cập nhật thời gian thực

**Sprint 6 — CI/CD & Triển khai**
- Cấu hình GitHub Actions pipeline
- Deploy qua AWS SAM/CloudFormation
- Thiết lập CloudWatch monitoring và alarms

---

### 5. Lộ trình & Mốc triển khai

| Giai đoạn | Nội dung | Thời gian |
|---|---|---|
| 1. Phân tích & Thiết kế kiến trúc | AWS onboarding, xác định scope GuardScript, thiết kế kiến trúc, schema DynamoDB, API design | Tuần 1–6 |
| 2. Nền tảng Backend | Khởi động dự án, auth, workspace, project, file, license APIs | Tuần 7–8 |
| 3. Script, File & Migrating AWS | Encryption module, tích hợp S3, setup Lambda/DynamoDB/CloudFront, loader protocols | Tuần 8–9 |
| 4. Bảo mật & Kiểm soát truy cập | Hardening loader v2/v3, HWID lock, access-list policies, rate-limit validation | Tuần 9–11 |
| 5. Frontend & Realtime | Dashboard/workspace UI, WebSocket, responsive design, kiểm thử tổng thể | Tuần 7–10 |
| 6. CI/CD & Tài liệu | Deploy SAM/CloudFormation, CloudWatch monitoring, validation checklist, báo cáo cuối | Tuần 11–12 |

---

### 6. Ước tính ngân sách

Chi phí hạ tầng hàng tháng điển hình (Free Tier / Quy mô nhỏ): **~$4.32/tháng**

| Dịch vụ | Chi phí ước tính | Ghi chú |
|---|---|---|
| AWS Lambda | ~$0.00/tháng | Free tier: 1M requests/tháng |
| Amazon DynamoDB | ~$0.60/tháng | On-demand; miễn phí 25 GB storage |
| Amazon S3 | ~$0.80/tháng | Lưu frontend và object content |
| Amazon CloudFront | ~$0.77/tháng | 1 TB data transfer free/tháng đầu |
| Amazon CloudWatch | ~$0.50/tháng | Log retention 30 ngày, alarms, dashboard |
| API Gateway WebSocket | ~$0.35/tháng | Kết nối WebSocket |
| Amazon SES | ~$0.09/tháng | Email thông báo và mời thành viên |
| Amazon SNS | ~$0.00/tháng | Cảnh báo (chủ yếu trong free tier) |
| AWS WAF | ~$0.21/tháng | Quy tắc bảo vệ edge |
| AWS ACM | ~$0.00/tháng | Chứng chỉ SSL/TLS (miễn phí cho CloudFront) |
| AWS IAM | ~$0.00/tháng | Không tính phí trực tiếp |
| **Tổng ước tính** | **~$4.32/tháng** | Pay-per-use, serverless |

> Lambda và DynamoDB chủ yếu nằm trong free tier ở mức sử dụng thấp.

---

### 7. Đánh giá rủi ro

| Rủi ro | Mức ảnh hưởng | Xác suất | Chiến lược giảm thiểu |
|---|---|---|---|
| Cold start Lambda làm tăng latency | Trung bình | Trung bình | Tối ưu handler, theo dõi p95 duration trên CloudWatch |
| DynamoDB throttling khi burst traffic | Cao | Thấp | On-demand capacity tự động scale; CloudWatch Alarm cảnh báo sớm |
| S3 object PUT/GET lỗi | Cao | Thấp | Retry logic trong Lambda; S3 versioning bật để phục hồi |
| Vượt ngân sách AWS | Trung bình | Thấp | AWS Budgets alert, tối ưu TTL và cache CloudFront |
| Replay attack trên loader | Cao | Thấp | Timestamp ±5 phút + nonce + HMAC signature bắt buộc |

**Kế hoạch dự phòng:**
- Nếu endpoint chính gặp sự cố: fallback sang Lambda Function URL trực tiếp.
- Nếu S3 không truy cập được: Lambda retry với exponential backoff, log lỗi vào CloudWatch.
- Dùng SAM/CloudFormation để tái tạo hạ tầng nhanh khi cần.

---

### 8. Security Considerations

1. Chữ ký request/response bằng HMAC-SHA256.
2. Xác minh timestamp và nonce để giảm replay.
3. Rate-limit theo IP/scope có TTL tự dọn dẹp.
4. RBAC theo owner/admin/editor/viewer.
5. HWID lock cho license khi cấu hình bật.
6. IAM role giới hạn theo tài nguyên stack.

---

### 9. Monitoring / Logging / Validation

1. CloudWatch alarms đang có sẵn cho `Errors`, `Throttles`, `Duration p95`.
2. CloudWatch dashboard đang có widget theo dõi invocations/errors/throttles/duration.
3. Bảng logs trên DynamoDB ghi nhận hành động theo workspace.
4. Checklist validation đề xuất:
  - Test login/register/rate-limit.
  - Test execute v2 với chữ ký/timestamp sai.
  - Test handshake v3 với public key sai.
  - Test blacklist/whitelist theo IP.
  - Kiểm tra metrics và alarms sau khi mô phỏng lỗi.

---

### 10. Deployment / Implementation Plan

1. Build và deploy stack bằng AWS SAM (`sam build`, `sam deploy`).
2. Sync frontend lên S3 host bucket.
3. Invalidate CloudFront sau khi cập nhật frontend.
4. Có thể dùng pipeline GitHub Actions để tự động hóa deploy.
5. Kiểm tra outputs sau deploy: CloudFront domain, Lambda URL, WebSocket endpoint, DynamoDB table names.

---

### 11. Kết quả kỳ vọng

**Kết quả kỹ thuật:**
1. Nền tảng deploy được end-to-end trên AWS serverless.
2. Luồng phân phối script có kiểm soát bằng signature, license, HWID, access policy.
3. Có monitoring cơ bản và tài liệu triển khai tái lập được.

**Giá trị dài hạn:**
1. Có thể mở rộng thêm ngôn ngữ loader và service governance.
2. Có nền tảng cho workshop cloud thực tế theo rubric FCJ.

---

### 12. Hướng phát triển

1. Tích hợp đầy đủ luồng gửi email production qua SES.
2. Bổ sung WAF rules cho edge protection.
3. Chuyển sang SSE-KMS với CMK riêng cho S3 object encryption.
4. Bổ sung AWS Budgets + Cost Anomaly Detection.
5. Tăng độ phủ test tự động (unit/integration/security tests).