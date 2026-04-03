---
title : "Login and Permission Testing"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---

# Login and Permission Testing

In this section, we test IAM user access.

Steps:
1. Enable console access for dev-2
2. Generate password and login URL
3. Log in using IAM user
4. Access Amazon S3

Observation:
- User can view S3 buckets
- User cannot modify resources

This confirms that read-only permissions are working correctly.