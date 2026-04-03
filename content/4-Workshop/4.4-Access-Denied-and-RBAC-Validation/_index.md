---
title : "Access Denied and RBAC Validation"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 4.4. </b> "
---

In this section, we summarize the entire workshop and explain how the access control system works in AWS IAM using the **Role-Based Access Control (RBAC)** model.

## Objectives

After completing this section, you will be able to:

- Understand the relationship between **User**, **Group**, **Policy**, and **Resource**
- Explain how users inherit permissions from groups
- Distinguish between allowed (**Allow**) and denied (**Deny**) actions
- Understand how AWS IAM applies the **Least Privilege** principle

---

## System Overview

In this workshop, we implemented a basic access control model with the following components:

- **IAM User**: `dev-2`
- **User Group**: `test-policy`
- **Policy**: `AmazonS3ReadOnlyAccess`
- **Resource**: `Amazon S3`

System flow:

```text
User (dev-2) → Group (test-policy) → Policy (AmazonS3ReadOnlyAccess) → Resource (S3)
````

This means that user `dev-2` is not assigned permissions directly. Instead, the user receives permissions through the group `test-policy`.

---

## How the System Works

AWS IAM in this workshop follows this model:

* Create a **User** to represent a person or account
* Create a **Group** to represent a role
* Attach a **Policy** to the Group
* Add the **User** to the Group
* The User inherits all permissions from the Group

This approach is important because:

* Access control is managed centrally
* Multiple users can share the same permissions through one group
* It reduces the risk of configuration mistakes
* It aligns with the **RBAC** model

---

## Access Validation

After finishing the configuration, we tested the real behavior of user `dev-2`.

### Case 1: Allowed Actions

User `dev-2` was added to group `test-policy`, and the group was assigned the policy `AmazonS3ReadOnlyAccess`.

After logging in as `dev-2`, the results showed:

* The user can access **Amazon S3**
* The user can view the bucket list
* The user can open a bucket and read its data

This confirms that:

* The policy was applied successfully
* The user inherited permissions from the group
* Read-only access is working as expected

---

### Case 2: Denied Actions

Next, we tested an action that requires write permission:

* Attempt to upload a file to S3

Result:

* The system returned **Access Denied**

This shows that:

* The user does not have `s3:PutObject` permission
* The current policy only allows read actions
* AWS IAM correctly blocks actions that exceed the granted permissions

This is an important validation step because it proves that the system not only allows the correct actions, but also denies unauthorized ones.

---

## Permission Analysis

In this workshop, the `AmazonS3ReadOnlyAccess` policy allows:

* Viewing the bucket list
* Viewing bucket contents
* Reading objects in S3

This policy does not allow:

* Uploading files
* Deleting objects
* Modifying data
* Performing administrative actions

Therefore, we can conclude:

* User `dev-2` has **Read** permission
* User `dev-2` does not have **Write** permission

---

## Least Privilege Principle

The system in this workshop follows the principle:

```text
Grant only the minimum permissions required
```

In this case:

* The user needs access to S3 in order to view data
* The user does not need to upload, delete, or modify data

Therefore, using `AmazonS3ReadOnlyAccess` is appropriate.

This principle is important because it:

* Reduces security risks
* Limits the impact of user mistakes
* Makes the system safer and easier to control

---

## RBAC in AWS IAM

RBAC is an access control model where:

* Permissions are assigned to **roles**
* Users are assigned to **roles**
* Users inherit permissions from those roles

In AWS IAM, this model maps as follows:

| RBAC Component | AWS IAM                |
| -------------- | ---------------------- |
| User           | IAM User               |
| Role           | User Group             |
| Permission     | Policy                 |
| Resource       | AWS Service / Resource |

Flow:

```text
User → Group → Policy → Resource
```

So, in this workshop:

* `dev-2` is the **User**
* `test-policy` is the **Role**
* `AmazonS3ReadOnlyAccess` is the **Permission**
* `Amazon S3` is the **Resource**

---

## Meaning of This Workshop

This workshop is not only about creating users or attaching policies. It also simulates a real access control system.

Through this hands-on lab, we have:

* Created a user to represent a real system user
* Created a group to manage permissions by role
* Attached a policy to control allowed actions
* Logged in with a real IAM user to test permissions
* Verified both scenarios:

  * **Allow**: access to S3
  * **Deny**: upload action blocked

This demonstrates that the IAM configuration is correct and that the system behaves according to security expectations.

---

## Conclusion

After completing this workshop, we can conclude that:

* Users do not need permissions attached directly
* Groups are an effective way to manage permissions centrally
* Policies clearly define what a user can and cannot do
* AWS IAM correctly applies the **Least Privilege** principle
* The **RBAC** model makes access control more scalable, organized, and secure

