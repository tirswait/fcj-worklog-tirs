---
title : "IAM User and Access Control"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 4.2. </b> "
---

#### Overview

This section demonstrates how to create an IAM user and assign permissions in AWS.

This workflow includes:
- Creating an IAM user
- Assigning permissions
- Verifying access control

---

#### Step 1: Create IAM User

- Navigate to AWS Console → IAM → Users → Create user
- Enter username: **...**
- Enable console access

![Create User](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/1.png)

---

#### Step 2: Assign Permissions

- Select **Attach policies directly**
- Choose **AdministratorAccess**

![Assign Policy](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/2.png)

---

#### Step 3: Create User

- Review configuration
- Click **Create user**
- Confirm success

![User Created](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/3.png)

---

#### Step 4: Verify Access Control

- Open IAM user dashboard
- Go to **Permissions**
- Confirm **AdministratorAccess**

![Permissions](/images/4-Workshop/4.2-Authentication_and_Role-Based_Access_Control_using_AWS_IAM/4.png)

---

#### Result

- IAM user successfully created  
- Permissions assigned  
- Access control verified  

---

#### Discussion

This demonstrates how AWS IAM implements authentication and role-based access control (RBAC).

---

