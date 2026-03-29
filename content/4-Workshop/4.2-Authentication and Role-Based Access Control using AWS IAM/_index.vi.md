---
title : "IAM User and Access Control"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 4.2. </b> "
---

#### Tổng quan

Phần này trình bày cách tạo IAM user và gán quyền trong AWS.

Quy trình bao gồm:
- Tạo IAM user
- Gán quyền
- Kiểm tra phân quyền

---

#### Bước 1: Tạo IAM User

- Truy cập AWS Console → IAM → Users → Create user  
- Nhập username: **...**  
- Bật quyền đăng nhập  

![Create User](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/1.png)

---

#### Bước 2: Gán quyền

- Chọn **Attach policies directly**  
- Chọn **AdministratorAccess**  

![Assign Policy](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/2.png)

---

#### Bước 3: Tạo user

- Kiểm tra cấu hình  
- Bấm **Create user**  
- Xác nhận thành công  

![User Created](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/3.png)

---

#### Bước 4: Kiểm tra quyền truy cập

- Mở IAM user  
- Vào tab **Permissions**  
- Kiểm tra policy **AdministratorAccess**  

![Permissions](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/4.png)

---

#### Kết quả

- Tạo user thành công  
- Gán quyền hoàn tất  
- Kiểm tra phân quyền thành công  

---

#### Thảo luận

Bài này minh họa cách AWS IAM triển khai xác thực và phân quyền theo vai trò (RBAC).