---
title : "RBAC và kiểm chứng hệ thống phân quyền"
date : 2026-03-30
weight : 4
chapter : false
pre : " <b> 4.4. </b> "
---

# RBAC và kiểm chứng hệ thống phân quyền

Trong phần này, chúng ta sẽ tổng hợp toàn bộ các bước đã thực hiện trong workshop và phân tích cách hệ thống phân quyền hoạt động trong AWS IAM theo mô hình **Role-Based Access Control (RBAC)**.

## Mục tiêu

Sau khi hoàn thành phần này, bạn sẽ:

- Hiểu mối quan hệ giữa **User**, **Group**, **Policy** và **Resource**
- Giải thích cách user kế thừa quyền từ group
- Phân biệt các hành động được phép (**Allow**) và bị từ chối (**Deny**)
- Hiểu cách AWS IAM áp dụng nguyên tắc **Least Privilege**

---

## Tổng quan hệ thống đã xây dựng

Trong workshop này, chúng ta đã triển khai một mô hình phân quyền cơ bản gồm các thành phần sau:

- **IAM User**: `dev-2`
- **User Group**: `test-policy`
- **Policy**: `AmazonS3ReadOnlyAccess`
- **Resource**: `Amazon S3`

Luồng hoạt động của hệ thống:

```text
User (dev-2) → Group (test-policy) → Policy (AmazonS3ReadOnlyAccess) → Resource (S3)
````

Điều này có nghĩa là user `dev-2` không được gán quyền trực tiếp, mà sẽ nhận quyền thông qua group `test-policy`.

---

## Nguyên lý hoạt động của hệ thống

AWS IAM trong bài thực hành này hoạt động theo mô hình:

* Tạo **User** để đại diện cho người sử dụng
* Tạo **Group** để đại diện cho một vai trò
* Gán **Policy** vào Group
* Thêm **User** vào Group
* User sẽ kế thừa toàn bộ quyền từ Group

Cách làm này có ý nghĩa quan trọng vì:

* Quyền truy cập được quản lý tập trung
* Có thể thêm nhiều user vào cùng một group mà không cần cấu hình lại policy
* Giảm rủi ro gán sai quyền cho từng user riêng lẻ
* Phù hợp với mô hình phân quyền theo vai trò (**RBAC**)

---

## Kiểm chứng quyền truy cập

Sau khi hoàn tất cấu hình, chúng ta tiến hành kiểm tra hành vi thực tế của user `dev-2`.

### Trường hợp 1: Hành động được cho phép (Allow)

User `dev-2` đã được thêm vào group `test-policy`, và group này được gán policy `AmazonS3ReadOnlyAccess`.

Khi đăng nhập bằng `dev-2`, kết quả kiểm tra cho thấy:

* User có thể truy cập **Amazon S3**
* User có thể xem danh sách bucket
* User có thể mở bucket và xem dữ liệu

Điều này chứng minh rằng:

* Policy đã được áp dụng thành công
* User đã kế thừa đúng quyền từ group
* Quyền **read-only** đang hoạt động đúng như mong đợi

---

### Trường hợp 2: Hành động bị từ chối (Deny)

Tiếp theo, chúng ta kiểm tra một hành động yêu cầu quyền ghi dữ liệu:

* Thử upload file lên S3

Kết quả:

* Hệ thống trả về lỗi **Access Denied**

Điều này cho thấy:

* User không có quyền `s3:PutObject`
* Policy hiện tại chỉ cho phép đọc dữ liệu
* Các hành động vượt quá quyền được cấp sẽ bị AWS IAM chặn lại

Đây là một bước kiểm chứng rất quan trọng vì nó xác nhận hệ thống không chỉ cho phép truy cập đúng, mà còn từ chối các hành động không hợp lệ.

---

## Phân tích quyền truy cập

Trong workshop này, policy `AmazonS3ReadOnlyAccess` cho phép:

* Xem danh sách bucket
* Xem nội dung bucket
* Đọc object trong S3

Policy này không cho phép:

* Upload file
* Xóa object
* Chỉnh sửa dữ liệu
* Thực hiện các thao tác quản trị

Từ đó, chúng ta có thể kết luận:

* User `dev-2` có quyền **Read**
* User `dev-2` không có quyền **Write**

---

## Nguyên tắc Least Privilege

Hệ thống trong workshop tuân theo nguyên tắc:

```text
Chỉ cấp quyền tối thiểu cần thiết cho người dùng
```

Trong trường hợp này:

* User cần truy cập S3 để xem dữ liệu
* User không cần upload, xóa hoặc chỉnh sửa dữ liệu

Vì vậy, việc sử dụng `AmazonS3ReadOnlyAccess` là phù hợp.

Ý nghĩa của nguyên tắc này:

* Giảm thiểu rủi ro bảo mật
* Hạn chế hậu quả nếu user thao tác nhầm
* Giúp hệ thống an toàn và dễ kiểm soát hơn

---

## RBAC trong AWS IAM

RBAC là mô hình phân quyền trong đó:

* Quyền được gán cho **vai trò**
* Người dùng được gán vào **vai trò**
* Người dùng kế thừa quyền từ vai trò đó

Trong AWS IAM, mô hình này được ánh xạ như sau:

| Thành phần RBAC | AWS IAM                |
| --------------- | ---------------------- |
| User            | IAM User               |
| Role            | User Group             |
| Permission      | Policy                 |
| Resource        | AWS Service / Resource |

Luồng hoạt động:

```text
User → Group → Policy → Resource
```

Như vậy, trong workshop này:

* `dev-2` là **User**
* `test-policy` là **Role**
* `AmazonS3ReadOnlyAccess` là **Permission**
* `Amazon S3` là **Resource**

---

## Ý nghĩa của workshop

Workshop này không chỉ dừng lại ở việc tạo user hay gán policy, mà còn mô phỏng một hệ thống kiểm soát truy cập thực tế.

Thông qua bài thực hành, chúng ta đã:

* Tạo user để đại diện cho người dùng trong hệ thống
* Tạo group để quản lý quyền theo vai trò
* Gán policy để kiểm soát hành động được phép
* Đăng nhập bằng user thật để kiểm tra quyền
* Xác minh cả hai trường hợp:

  * **Allow**: xem được S3
  * **Deny**: upload bị từ chối

Điều này giúp chứng minh rằng hệ thống IAM đã được cấu hình đúng và hoạt động đúng nguyên tắc bảo mật.

---

## Kết luận

Sau khi hoàn thành workshop, chúng ta có thể rút ra các kết luận sau:

* User không cần được gán quyền trực tiếp
* Group là nơi phù hợp để quản lý quyền tập trung
* Policy quyết định rõ user được làm gì và không được làm gì
* AWS IAM áp dụng đúng nguyên tắc **Least Privilege**
* Mô hình **RBAC** giúp việc quản lý truy cập trở nên rõ ràng, dễ mở rộng và an toàn hơn

Tóm lại, hệ thống đã được triển khai thành công theo mô hình:

```text
User → Group → Policy → Resource
```

Và hành vi thực tế của user đã xác nhận rằng hệ thống phân quyền hoạt động chính xác trong AWS IAM.

````

---
