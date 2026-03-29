---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 4.1. </b> "
---

#### AWS Identity and Access Management (IAM)
+ **AWS Identity and Access Management (IAM)** là một dịch vụ cho phép bạn kiểm soát quyền truy cập vào các tài nguyên AWS một cách an toàn. Dịch vụ này giúp bạn tạo và quản lý người dùng, nhóm và các quyền truy cập.
+ IAM hỗ trợ **xác thực (authentication)** (ai được phép truy cập) và **phân quyền (authorization)** (được phép thực hiện những hành động gì) thông qua các chính sách (policy) như AWS managed policies hoặc custom policies.

#### Tổng quan workshop
Trong workshop này, bạn sẽ làm việc với AWS IAM để mô phỏng một hệ thống xác thực và phân quyền cơ bản.  
+ Bạn sẽ tạo một **IAM user** có quyền truy cập vào AWS Management Console.  
+ Bạn sẽ gán quyền bằng cách sử dụng một AWS managed policy như **AdministratorAccess**.  
+ Bạn sẽ kiểm tra quyền của người dùng thông qua IAM dashboard để hiểu cách hệ thống kiểm soát truy cập hoạt động.  

Workshop này minh họa cách **kiểm soát truy cập dựa trên vai trò (RBAC)** có thể được áp dụng bằng AWS IAM. Đây là phiên bản đơn giản hóa của hệ thống xác thực và phân quyền được mô tả trong nền tảng GuardScript đã đề xuất.

![overview](/images/4-Workshop/4.1-Workshop-overview/1.png)
