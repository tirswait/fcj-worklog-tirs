---
title : "Login and Permission Testing"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---

## Step 1: Login with IAM User

- Open the **Sign-in URL**
- Log in using user **dev-2**

![login](/images/4-Workshop/4.3-User-and-Test/1.png)

---

## Step 2: Access Amazon S3

- After logging in, navigate to **Amazon S3**
- The user can:
  - View bucket list
  - Open buckets

![s3](/images/4-Workshop/4.3-User-and-Test/2.png)

---

## Step 3: Test Restricted Actions (Access Denied)

Do:

- Try uploading a file to S3

Result:

- The system returns **Access Denied**

![deny](/images/4-Workshop/4.3-User-and-Test/3.png)

---