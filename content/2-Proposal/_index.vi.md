---
title: "Bản đề xuất"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Website Security Baseline Assessment Platform

## 1. Tổng quan đề xuất

Website Security Baseline Assessment Platform là một hệ thống hỗ trợ đánh giá nhanh mức độ an toàn cơ bản của website dựa trên các tiêu chí bảo mật công khai và an toàn.  
Hệ thống được thiết kế nhằm giúp doanh nghiệp hoặc đội kỹ thuật nội bộ nhận diện sớm các rủi ro bảo mật phổ biến trước khi quyết định thực hiện các hoạt động kiểm thử chuyên sâu như penetration testing.

Nền tảng tập trung vào việc cung cấp cái nhìn tổng quan về tình trạng bảo mật hiện tại của website, phục vụ cho cả mục đích kỹ thuật và quản lý.

---

## 2. Vấn đề và nhu cầu

### Vấn đề hiện tại

Trong thực tế, nhiều website:

* Chưa được đánh giá bảo mật định kỳ
* Thiếu công cụ sàng lọc rủi ro nhanh và hợp pháp
* Phụ thuộc vào các hoạt động pentest tốn thời gian và chi phí

Việc thiếu một bước đánh giá nền tảng (baseline) khiến doanh nghiệp khó xác định mức độ ưu tiên cho các hoạt động bảo mật tiếp theo.

### Giải pháp đề xuất

Hệ thống được xây dựng để:

* Đánh giá các cấu hình bảo mật cơ bản từ thông tin công khai
* Cung cấp báo cáo rủi ro ở mức độ tổng quan
* Hỗ trợ ra quyết định mà không cần can thiệp sâu vào hệ thống mục tiêu

Giải pháp không nhằm thay thế pentest chuyên sâu, mà đóng vai trò là bước tiền kiểm tra trong quy trình bảo mật.

---

## 3. Phạm vi và nguyên tắc triển khai

Hệ thống được triển khai theo các nguyên tắc sau:

* Chỉ thu thập và phân tích **thông tin công khai**
* Không thực hiện tấn công, khai thác hay vượt qua cơ chế bảo vệ
* Không brute-force, không fuzzing, không bypass authentication

Các nội dung kiểm tra chính bao gồm:

* HTTPS / SSL configuration
* HTTP Security Headers
* Server information exposure
* robots.txt và sitemap
* Nhận diện CMS phổ biến và phiên bản công khai

Cách tiếp cận này đảm bảo tính **hợp pháp**, an toàn và phù hợp để sử dụng trong môi trường doanh nghiệp.

---

## 4. Thiết kế hệ thống

### Thiết kế cốt lõi

Hệ thống được xây dựng theo hướng **portable và độc lập**:

* Core logic (scan, đánh giá rủi ro, sinh báo cáo) hoạt động độc lập
* Có thể triển khai trên:
  * Local environment
  * Server nội bộ
  * VPS
  * Cloud (AWS)

Phần core không phụ thuộc vào hạ tầng cloud cụ thể, giúp dễ dàng mở rộng hoặc di chuyển môi trường triển khai.

### Vai trò của AWS

AWS được sử dụng như một nền tảng hỗ trợ để:

* Triển khai hệ thống theo mô hình cloud-ready
* Lưu trữ báo cáo và dữ liệu kết quả
* Ghi log và theo dõi hoạt động hệ thống

Trong trường hợp không sử dụng AWS, hệ thống vẫn có thể vận hành đầy đủ chức năng.

---

## 5. Kết quả đầu ra

Hệ thống cung cấp các kết quả sau:

* Báo cáo đánh giá mức độ rủi ro (Low / Medium / High)
* Phần mô tả và giải thích dễ hiểu cho người không chuyên
* Danh sách khuyến nghị cải thiện bảo mật có thể hành động

Báo cáo được thiết kế để:

* Hỗ trợ đội kỹ thuật trong việc đánh giá nhanh
* Phù hợp cho quản lý hoặc người ra quyết định tham khảo

---

## 6. Giá trị kỳ vọng

### Đối với tổ chức sử dụng

* Có công cụ đánh giá bảo mật nội bộ
* Giảm thời gian và chi phí sàng lọc rủi ro ban đầu
* Là nền tảng cho các hoạt động bảo mật chuyên sâu sau này

### Đối với nhóm triển khai

* Thực hành đúng chuyên ngành IA / AI
* Rèn luyện tư duy thiết kế sản phẩm thực tế
* Làm việc với kiến trúc có khả năng mở rộng và tái sử dụng

---

## 7. Kết luận

Đề xuất hướng tới việc xây dựng một hệ thống đánh giá bảo mật **đơn giản, đúng phạm vi và có giá trị thực tiễn**, tập trung vào hiệu quả sử dụng thay vì độ phức tạp kỹ thuật không cần thiết.  
Việc sử dụng AWS giúp nâng cao khả năng vận hành và mở rộng, trong khi vẫn đảm bảo tính độc lập của hệ thống cốt lõi.
