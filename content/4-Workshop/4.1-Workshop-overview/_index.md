---
title : "Introduction"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 4.1. </b> "
---

#### AWS Identity and Access Management (IAM)
+ **AWS Identity and Access Management (IAM)** is a service that allows you to securely control access to AWS resources. It enables you to create and manage users, groups, and permissions.
+ IAM supports **authentication** (who can access) and **authorization** (what actions they can perform) through policies such as AWS managed policies or custom policies.

#### Workshop overview
In this workshop, you will work with AWS IAM to simulate a basic authentication and authorization system.  
+ You will create an **IAM user** with access to the AWS Management Console.  
+ You will assign permissions using an AWS managed policy such as **AdministratorAccess**.  
+ You will verify user permissions through the IAM dashboard to understand how access control is implemented.  

This workshop demonstrates how **role-based access control (RBAC)** can be applied using AWS IAM. It is a simplified version of the authentication and permission system described in the proposed GuardScript platform.

![overview](/images/4-Workshop/4.1-Workshop-overview/1.png)