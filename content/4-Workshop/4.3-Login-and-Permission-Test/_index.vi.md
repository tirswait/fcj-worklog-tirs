---
title : "Đăng nhập và kiểm tra quyền"
date : 2026-03-30 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---

## Bước 1: Đăng nhập bằng IAM User

- Mở **Sign-in URL**
- Đăng nhập với user **dev-2**

![login](/images/4-Workshop/4.3-User-and-Test/1.png)

---

## Bước 2: Truy cập Amazon S3

- Sau khi đăng nhập, truy cập **Amazon S3**
- User có thể:
  - Xem danh sách bucket
  - Mở bucket

![s3](/images/4-Workshop/4.3-User-and-Test/2.png)

---

## Bước 3: Kiểm tra hành động bị hạn chế (Access Denied)

Thực hiện:

- Thử upload file lên S3

Kết quả:

- Hệ thống trả về lỗi **Access Denied**

![deny](/images/4-Workshop/4.3-User-and-Test/3.png)

---