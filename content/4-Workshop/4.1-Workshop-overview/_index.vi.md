---
title : "Giới thiệu"
date : 2026-03-30
weight : 1 
chapter : false
pre : " <b> 4.1. </b> "
---

# Tổng quan Workshop

Trong workshop này, bạn sẽ làm quen với cách AWS kiểm soát truy cập thông qua dịch vụ **AWS Identity and Access Management (IAM)**. Đây là một thành phần quan trọng giúp đảm bảo hệ thống an toàn và chỉ cho phép người dùng thực hiện những hành động được cấp quyền.

Thay vì gán quyền trực tiếp cho từng user, workshop sẽ hướng dẫn bạn cách sử dụng **User Group** và **Policy** để quản lý quyền một cách tập trung và hiệu quả hơn.

---

## Mục tiêu

Sau khi hoàn thành workshop, bạn sẽ có thể:

- Tạo và quản lý **IAM User**
- Tạo **User Group** để gom các user có cùng vai trò
- Gán quyền thông qua **IAM Policy**
- Hiểu cách user kế thừa quyền từ group
- Kiểm tra và xác minh quyền truy cập trên dịch vụ AWS (Amazon S3)

---

## Nội dung chính

Trong quá trình thực hành, bạn sẽ thực hiện các bước sau:

1. Tạo một IAM User mới
2. Tạo User Group và gán policy phù hợp
3. Thêm user vào group
4. Đăng nhập bằng user vừa tạo
5. Truy cập dịch vụ Amazon S3
6. Kiểm tra các hành động:
   - Hành động được phép (Allow)
   - Hành động bị từ chối (Deny)

---

## Mô hình áp dụng

Workshop này mô phỏng một hệ thống phân quyền thực tế dựa trên mô hình:

```text
User → Group → Policy → Resource
````

Trong đó:

* **User**: đại diện cho người dùng trong hệ thống
* **Group**: đại diện cho vai trò (role)
* **Policy**: định nghĩa quyền truy cập
* **Resource**: tài nguyên AWS (ví dụ: S3 Bucket)

---

## RBAC trong AWS IAM

Workshop áp dụng mô hình **Role-Based Access Control (RBAC)**, trong đó:

* Quyền được gán cho **group (role)**
* User được thêm vào group
* User kế thừa quyền từ group

Cách tiếp cận này giúp:

* Quản lý quyền tập trung
* Dễ mở rộng khi có nhiều user
* Giảm sai sót khi cấu hình
* Tuân theo best practice của AWS

---

## Kết quả đạt được

Sau khi hoàn thành workshop, bạn sẽ:

* Hiểu cách AWS IAM hoạt động trong thực tế
* Thiết lập được một hệ thống phân quyền cơ bản
* Phân biệt rõ giữa **Allow** và **Access Denied**
* Áp dụng được nguyên tắc **Least Privilege** trong hệ thống

---

Workshop này không chỉ giúp bạn nắm lý thuyết mà còn cung cấp trải nghiệm thực tế thông qua việc kiểm tra quyền truy cập trực tiếp trên AWS.
