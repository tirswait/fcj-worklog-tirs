---
title : "Access Denied and RBAC Validation"
date : 2024-01-01 
weight : 4
chapter : false
pre : " <b> 4.4. </b> "
---

# Access Denied and RBAC Validation

In this section, we verify permission restrictions.

Steps:
1. Attempt to upload file to S3
2. System returns Access Denied

Result:
- User does not have write permission
- IAM policy is enforced correctly

Conclusion:
RBAC is working as expected:
- Permissions are restricted based on roles
- Unauthorized actions are blocked