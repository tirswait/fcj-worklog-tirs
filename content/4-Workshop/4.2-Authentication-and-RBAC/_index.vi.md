---
title : "Xác thực và RBAC"
date : 2026-03-30 
weight : 2 
chapter : false
pre : " <b> 4.2. </b> "
---

# Xác thực và Phân quyền theo vai trò

Trong phần này, bạn triển khai RBAC bằng AWS IAM.

Các bước:
1. Tạo group (Developers) & Gán policy AmazonS3ReadOnlyAccess
2. Tạo IAM user (dev-1, dev-2) & Thêm user vào group 

Kết quả:
- User kế thừa quyền từ group
- Quản lý quyền tập trung

Mô hình:
User → Group → Policy

![diagram](/images/4-Workshop/4.1-Workshop-overview/1.jpg)