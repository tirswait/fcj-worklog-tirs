---
title: "Workshop"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# Authentication and Access Control using AWS IAM

---

#### Overview

AWS Identity and Access Management (IAM) is a service that enables secure control of access to AWS resources.

In this workshop, you will implement a complete access control workflow including:

- Creating IAM users
- Creating user groups and attaching policies
- Assigning users to groups (RBAC)
- Enabling console access
- Logging in with IAM users
- Accessing Amazon S3
- Testing permissions (read vs write)
- Observing Access Denied errors

---

#### Objectives

This workshop simulates a real-world **authentication and authorization system**:

- **Authentication**: identifies who can access the system (IAM login)
- **Authorization**: defines what actions they can perform (IAM policies)

You will also implement:
**Role-Based Access Control (RBAC)**

Where:
- Users are assigned to Groups
- Groups are assigned Policies
- Users inherit permissions from Groups

---

#### Hands-on Steps

1. Create IAM Users (dev-1, dev-2)
2. Create User Group (Developers)
3. Attach AmazonS3ReadOnlyAccess policy to the group
4. Add users to the group
5. Enable console access and log in
6. Access S3 bucket
7. Attempt file upload → Access Denied
8. Verify RBAC behavior

---

#### Outcomes

After completing this workshop, you will understand:

- How AWS IAM manages access control
- How RBAC is implemented in practice
- The difference between read and write permissions
- How to troubleshoot Access Denied errors

---

#### Content

1. [Introduction](4.1-Workshop-overview/)
2. [IAM User and RBAC](4.2-Authentication%20and%20Role-Based%20Access%20Control%20using%20AWS%20IAM/)

---