---
title : "Create a Group and Assign Permissions with AWS IAM"
date : 2026-03-30
weight : 1
chapter : false
pre : " <b> 4.2.1 </b> "
---

In this section, we will create an **IAM User Group** and assign appropriate access permissions to it.  
Instead of assigning permissions directly to individual users, AWS recommends managing permissions through **Groups** to improve scalability and follow the **Role-Based Access Control (RBAC)** model.

## Objectives

After completing this step, you will:

- Create an **IAM User Group**
- Attach the **AmazonS3ReadOnlyAccess** policy to the group
- Understand how groups centrally manage permissions for multiple users

---

## Step 1: Create a User Group & Attach Policy

Navigate to:

- **IAM**
- **User groups**
- **Create user group**

Here, enter the group name. In this lab, the group is named:
**test-policy**

- Search for the policy: **AmazonS3ReadOnlyAccess**

![policy](/images/4-Workshop/4.2-Authentication-and-RBAC/1.png)

## Step 2: Create the Group
- After successful creation, the group **test-policy** will appear in the list.

![policy](/images/4-Workshop/4.2-Authentication-and-RBAC/2.png)

## Step 3: Verify Group Permissions
- Navigate to: **IAM** > **User groups** > **test-policy** > **Permissions**
- Verify that the policy has been attached:

![policy](/images/4-Workshop/4.2-Authentication-and-RBAC/3.png)