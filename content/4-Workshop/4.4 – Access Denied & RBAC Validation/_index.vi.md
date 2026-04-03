---
title : "Access Denied và kiểm chứng RBAC"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 4.4. </b> "
---

# Access Denied và kiểm chứng RBAC

Trong phần này, bạn kiểm tra giới hạn quyền.

Các bước:
1. Thử upload file lên S3
2. Hệ thống báo lỗi Access Denied

Kết quả:
- User không có quyền ghi (write)
- Policy được áp dụng đúng

Kết luận:
RBAC hoạt động chính xác:
- Quyền được kiểm soát theo vai trò
- Hành động không hợp lệ bị chặn