---
title : "Introduction"
date : 2026-03-30
weight : 1 
chapter : false
pre : " <b> 4.1. </b> "
---

# Workshop Overview

In this workshop, you will learn how to control access to AWS resources using **AWS Identity and Access Management (IAM)**. IAM is a core AWS service that enables secure and fine-grained access control for users and services.

Instead of assigning permissions directly to individual users, this workshop demonstrates how to manage access more effectively using **User Groups** and **Policies**, following AWS best practices.

---

## Objectives

After completing this workshop, you will be able to:

- Create and manage **IAM Users**
- Create **User Groups** to represent roles
- Assign permissions using **IAM Policies**
- Understand how users inherit permissions from groups
- Validate access control using **Amazon S3**

---

## Main Activities

During this workshop, you will perform the following steps:

1. Create a new IAM User  
2. Create a User Group and assign a policy  
3. Add the user to the group  
4. Log in using the created IAM user  
5. Access the Amazon S3 service  
6. Test different actions:
   - Allowed actions (Allow)  
   - Denied actions (Access Denied)  

---

## Architecture Model

This workshop follows a simple access control model:

```text
User → Group → Policy → Resource
````

Where:

* **User** represents an individual identity
* **Group** represents a role
* **Policy** defines permissions
* **Resource** is the AWS service (e.g., S3 Bucket)

---

## RBAC in AWS IAM

This workshop applies the **Role-Based Access Control (RBAC)** model:

* Permissions are assigned to **groups (roles)**
* Users are assigned to groups
* Users inherit permissions from those groups

This approach provides:

* Centralized permission management
* Better scalability
* Reduced configuration errors
* Alignment with AWS security best practices

---

## Expected Outcomes

By the end of this workshop, you will:

* Understand how IAM works in real-world scenarios
* Build a basic access control system using AWS IAM
* Clearly distinguish between **Allow** and **Access Denied**
* Apply the **Least Privilege** principle in practice

---

This workshop not only covers theoretical concepts but also provides hands-on experience by validating access behavior directly on AWS services.
