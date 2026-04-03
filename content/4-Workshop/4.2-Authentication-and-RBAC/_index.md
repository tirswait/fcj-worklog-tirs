---
title : "Authentication and RBAC"
date : 2026-03-30
weight : 2 
chapter : false
pre : " <b> 4.2. </b> "
---

# Authentication and Role-Based Access Control

In this section, we implement RBAC using AWS IAM.

Steps:
1. Create user group (Developers) & Attach AmazonS3ReadOnlyAccess policy
2. Create IAM users (dev-1, dev-2) & Add users to the group
 

Result:
- Users inherit permissions from the group
- Permissions are managed centrally

This demonstrates RBAC:
- Users → Groups → Policies


