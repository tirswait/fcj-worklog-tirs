---
title: "Lộ trình học AWS"
date: 2026-01-01
weight: 8
chapter: false
---

## Tháng 1–2: Nền tảng AWS

**Ngân sách dự kiến:** $50–70

### Mục tiêu học tập

* Hiểu kiến trúc hạ tầng toàn cầu của AWS
* Nắm vững các dịch vụ cốt lõi (EC2, S3, IAM, VPC)
* Hiểu các khái niệm cơ bản về bảo mật và mạng
* Làm quen với AWS CLI và SDK

---

### Nội dung học tập

#### Tuần 1 – Làm quen ban đầu

* Hoàn thành 5 nhiệm vụ tích lũy tín chỉ
* Thiết lập cảnh báo chi phí và ngân sách AWS
* Khám phá AWS Management Console
* Tạo EC2 instance đầu tiên
* Thực hành: Triển khai web server đơn giản

#### Tuần 2 – Lưu trữ và Cơ sở dữ liệu

* Amazon S3: bucket và lifecycle policy
* Amazon EBS: volume và snapshot
* Thiết lập và quản lý Amazon RDS
* Thực hành: Xây dựng hệ thống upload file

#### Tuần 3 – Mạng

* Khái niệm và cấu hình VPC
* So sánh Security Group và Network ACL
* Internet Gateway và NAT Gateway
* Thực hành: Kiến trúc mạng nhiều tầng

#### Tuần 4 – Định danh và Bảo mật

* IAM user, group và role
* Chính sách và phân quyền truy cập
* Thiết lập xác thực đa yếu tố (MFA)
* Thực hành: Xây dựng môi trường AWS nhiều người dùng an toàn

---

### Dự án

**Dự án 1: Website Blog Cá Nhân**

* EC2 + RDS + S3
* Chi phí ước tính: ~$25/tháng

**Dự án 2: Hệ thống Chia sẻ Tệp**

* S3 + Lambda + API Gateway
* Chi phí ước tính: ~$10/tháng

---

## Tháng 3–4: Dịch vụ AWS Trung cấp

**Ngân sách dự kiến:** $60–80

### Mục tiêu học tập

* Điện toán serverless với AWS Lambda
* Phát triển API với API Gateway
* Kiến thức cơ bản về container với Amazon ECS
* Giám sát và ghi log với CloudWatch

---

### Nội dung học tập

#### Tuần 5 – Serverless Computing

* Lambda function và cơ chế trigger
* Cấu hình API Gateway
* Thao tác với DynamoDB
* Thực hành: Backend REST API

#### Tuần 6 – Tích hợp ứng dụng

* SQS và SNS cho hệ thống message
* EventBridge và định tuyến sự kiện
* Workflow với Step Functions
* Thực hành: Kiến trúc hướng sự kiện

#### Tuần 7 – Container và Điều phối

* Kiến thức cơ bản về Docker
* ECS cluster và service
* ECR – kho lưu trữ container
* Thực hành: Ứng dụng web container hóa

#### Tuần 8 – Giám sát và Ghi log

* CloudWatch metric và alarm
* CloudTrail ghi log audit
* AWS X-Ray theo dõi phân tán
* Thực hành: Thiết lập hệ thống giám sát toàn diện

---

### Dự án

**Dự án 3: Serverless Todo API**

* Lambda + API Gateway + DynamoDB
* Chi phí ước tính: ~$15/tháng

**Dự án 4: Kiến trúc Microservices**

* ECS + ALB + RDS
* Chi phí ước tính: ~$35/tháng

---

## Tháng 5–6: Kiến trúc Nâng cao và Tối ưu

**Ngân sách dự kiến:** $70–90

### Mục tiêu học tập

* Thiết kế kiến trúc mở rộng và sẵn sàng cao
* Triển khai CI/CD pipeline
* Áp dụng các biện pháp bảo mật nâng cao
* Tối ưu chi phí và hiệu năng hệ thống

---

### Nội dung học tập

#### Tuần 9 – Mạng nâng cao

* VPC Peering và Transit Gateway
* Khái niệm AWS Direct Connect
* Cấu hình CloudFront CDN
* Thực hành: Kiến trúc ứng dụng toàn cầu

#### Tuần 10 – DevOps & CI/CD

* CodeCommit, CodeBuild, CodeDeploy
* Tự động hóa với CodePipeline
* Infrastructure as Code bằng CloudFormation
* Thực hành: Pipeline triển khai tự động

#### Tuần 11 – Bảo mật nâng cao

* AWS WAF và Shield
* Secrets Manager
* AWS Certificate Manager
* Thực hành: Môi trường production an toàn

#### Tuần 12 – Tối ưu và Best Practices

* Kỹ thuật tối ưu chi phí
* Tối ưu hiệu năng hệ thống
* Lập kế hoạch khôi phục thảm họa
* Thực hành: Kiến trúc sẵn sàng triển khai thực tế

---

### Dự án

**Dự án 5: Nền tảng Thương mại điện tử**

* Kiến trúc nhiều tầng
* Auto Scaling và Load Balancing
* Chi phí ước tính: ~$45/tháng

**Dự án 6: Pipeline Phân tích Dữ liệu**

* S3 + Glue + Athena + QuickSight
* Chi phí ước tính: ~$25/tháng

---

## Lộ trình Chứng chỉ

**AWS Certified Cloud Practitioner**

* Thời điểm: Cuối tháng 2
* Lệ phí thi: $100
* Thời gian ôn tập: 2–3 tuần

**AWS Solutions Architect – Associate**

* Thời điểm: Tháng 4–5
* Lệ phí thi: $150
* Thời gian ôn tập: 4–6 tuần

**AWS Solutions Architect – Professional**

* Thời điểm: Sau tháng 6
* Lệ phí thi: $300
* Thời gian ôn tập: 8–12 tuần

---

## Tài nguyên học tập

### Tài nguyên miễn phí

* AWS Skill Builder
* AWS Documentation
* AWS Architecture Center
* AWS re:Invent Video Library
* AWS Special Force Portal
* AWS Study Group (YouTube)
* Tài liệu hệ thống Lab

### Cộng đồng

* AWS Community Builders
* AWS User Groups
* Reddit: r/aws, r/AWSCertifications
* Discord: AWS Community Servers

---

