---
title: "Proposal"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Nền tảng Bảo vệ và Phân phối Mã nguồn (IrisAuth)

## 1. Tổng quan đề xuất

Nền tảng Code Protector (IrisAuth) được xây dựng nhằm cung cấp một giải pháp an toàn và có khả năng mở rộng cho việc bảo vệ và phân phối mã nguồn đến người dùng cuối mà không làm lộ mã nguồn gốc.

Hệ thống cho phép developer tải mã nguồn lên nền tảng và phân phối thông qua cơ chế loader được mã hóa. Thay vì chia sẻ trực tiếp source code, việc thực thi sẽ được kiểm soát và cung cấp một cách an toàn tại thời điểm runtime.

Nền tảng tập trung vào **bảo vệ mã nguồn, kiểm soát truy cập và phân phối an toàn**, phục vụ cả nhu cầu kỹ thuật và định hướng sản phẩm.

---

## 2. Vấn đề và động lực

### Thách thức hiện tại

Trong môi trường phát triển phần mềm hiện nay:

* Mã nguồn dễ bị sao chép, chỉnh sửa hoặc tái phân phối trái phép  
* Các phương pháp truyền thống (obfuscation, đóng gói .exe) không còn hiệu quả  
* Developer không kiểm soát được ai đang chạy code và trong điều kiện nào  

Những hạn chế này tạo ra rủi ro lớn đối với việc bảo vệ tài sản trí tuệ và phân phối phần mềm.

### Giải pháp đề xuất

Hệ thống được đề xuất nhằm:

* Bảo vệ mã nguồn bằng cách **không bao giờ cung cấp plaintext cho người dùng cuối**  
* Phân phối code thông qua loader mã hóa và thực thi tại runtime  
* Áp dụng kiểm soát truy cập bằng license, HWID và xác thực request  

Giải pháp này cung cấp một **mô hình thực thi an toàn**, thay vì chỉ dựa vào các kỹ thuật bảo vệ tĩnh.

---

## 3. Phạm vi và nguyên tắc triển khai

Hệ thống được xây dựng dựa trên các nguyên tắc:

* Mã nguồn **không được phân phối trực tiếp** cho người dùng  
* Mọi hoạt động thực thi đều thông qua **xác thực phía server**  
* Áp dụng các cơ chế mã hóa mạnh cho lưu trữ và truyền dữ liệu  

Các thành phần chính bao gồm:

* Phân phối code an toàn qua loader mã hóa  
* Hệ thống license (HWID, expiration)  
* Kiểm soát truy cập (IP filtering, rate limiting)  
* Hệ thống logging và monitoring  

Đảm bảo hệ thống **an toàn, có khả năng mở rộng và sẵn sàng triển khai thực tế**.

---

## 4. Thiết kế hệ thống

### Kiến trúc tổng thể

Nền tảng sử dụng kiến trúc **cloud-native serverless**:

* Backend chạy trên AWS Lambda  
* Dữ liệu lưu trữ trên DynamoDB  
* Nội dung mã nguồn lưu trên Amazon S3  
* Frontend phân phối qua CloudFront CDN  

Hệ thống hướng tới:

* Khả năng mở rộng cao (auto-scaling)  
* Độ trễ thấp toàn cầu  
* Giảm thiểu quản lý hạ tầng  

### Vai trò của AWS

AWS đóng vai trò hạ tầng để:

* Triển khai hệ thống backend serverless  
* Lưu trữ dữ liệu và mã nguồn đã mã hóa  
* Giám sát và theo dõi hoạt động hệ thống  
* Phân phối nội dung toàn cầu  

Các dịch vụ sử dụng:

* AWS Lambda  
* Amazon S3  
* Amazon DynamoDB  
* Amazon CloudFront  
* Amazon CloudWatch  
* Amazon SES  

---

## 5. Sản phẩm đầu ra

Hệ thống cung cấp:

* Web platform quản lý và upload mã nguồn  
* Cơ chế loader mã hóa để thực thi code an toàn  
* Hệ thống license với HWID và thời hạn  
* Dashboard theo dõi và logging  
* Hạ tầng cloud serverless trên AWS  

Kết quả đạt được:

* Luồng thực thi code an toàn  
* Kiểm soát truy cập và theo dõi sử dụng  
* Dữ liệu log và monitoring hệ thống  

---

## 6. Giá trị mang lại

### Đối với tổ chức

* Bảo vệ hiệu quả tài sản trí tuệ (source code)  
* Kiểm soát chặt chẽ việc phân phối và thực thi code  
* Giảm rủi ro bị reverse engineering hoặc sử dụng trái phép  
* Xây dựng nền tảng SaaS có khả năng mở rộng  

### Đối với nhóm thực hiện

* Kinh nghiệm thực tế với kiến trúc serverless trên AWS  
* Áp dụng các kỹ thuật mã hóa hiện đại (ECDH, AES-GCM)  
* Xây dựng hệ thống SaaS mang tính thực tiễn cao  
* Nâng cao tư duy thiết kế hệ thống và sản phẩm  

---

## 7. Kết luận

Đề xuất này hướng tới xây dựng một **nền tảng bảo vệ mã nguồn an toàn và có khả năng mở rộng**, giải quyết các vấn đề thực tế trong phân phối phần mềm.

Hệ thống tập trung vào:

* Thiết kế ưu tiên bảo mật  
* Kiểm soát thực thi thay vì bảo vệ tĩnh  
* Khả năng mở rộng dựa trên cloud AWS  

IrisAuth là một giải pháp **thực tiễn, hiện đại và sẵn sàng triển khai**, phù hợp với nhu cầu của developer trong môi trường phần mềm ngày nay.