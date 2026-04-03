---
title : "Tạo Group và gán quyền với AWS IAM"
date : 2026-03-30
weight : 1
chapter : false
pre : " <b> 4.2.1 </b> "
---

Trong phần này, chúng ta sẽ tạo một **IAM User Group** và gán quyền truy cập phù hợp cho group đó.  
Thay vì gán quyền trực tiếp cho từng user, AWS khuyến nghị quản lý quyền thông qua **Group** để dễ mở rộng và tuân theo mô hình **Role-Based Access Control (RBAC)**.

## Mục tiêu

Sau khi hoàn thành bước này, bạn sẽ:

- Tạo được một **IAM User Group**
- Gán policy **AmazonS3ReadOnlyAccess** cho group
- Hiểu cách group quản lý quyền tập trung cho nhiều user

---

## Bước 1: Tạo User Group & Gán policy cho group

Truy cập:

- **IAM**
- **User groups**
- **Create user group**

Tại đây, nhập tên group. Trong bài thực hành này, group được đặt là:
**test-policy**

- Tìm policy: **AmazonS3ReadOnlyAccess**

![policy](/images/4-Workshop/4.2-Authentication-and-RBAC/1.png)

## Bước 2: Tạo group
- Sau khi tạo thành công, group **test-policy** sẽ xuất hiện trong danh sách.
  
![policy](/images/4-Workshop/4.2-Authentication-and-RBAC/2.png)

## Bước 3: Kiểm tra quyền của group
- Truy cập: IAM > User groups > test-policy > Permissions
- Kiểm tra policy đã được gán:
  
![policy](/images/4-Workshop/4.2-Authentication-and-RBAC/3.png)
