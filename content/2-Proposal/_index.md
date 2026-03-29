---
title: "Project Proposal"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# GuardScript — Code Protector Platform

## Project Proposal

### Student / Team Information

- **Võ Tấn Phát (Team Leader)** – Student ID: **SE194484**
- **Bùi Minh Hiển** – Student ID: **SE190829**
- **Dương Nguyên Bình** – Student ID: **SE194067**
- **Vinh Trần** – Student ID: **SE193927**
- **Nguyễn Duy Tùng** – Student ID: **SE196572**
- **Nguyễn Đức Trí** – Student ID: **SE194091**

---

## Multi-language Secure Source Code Protection and Script Distribution Platform

### Deployed on AWS Cloud Infrastructure

**AWS Lambda** · **Amazon S3** · **Amazon DynamoDB** · **Amazon CloudFront** · **Amazon CloudWatch** · **Amazon SES**

---

# I. PROJECT INTRODUCTION

## 1.1. Background and Problem

In the modern software development environment, **source code protection** is a critical challenge. When developers distribute **Python** or **JavaScript** scripts to end users, they face risks such as source code being copied, modified, or redistributed illegally without effective control mechanisms.

Existing solutions such as simple **obfuscation** or packaging into `.exe` files still have many limitations: they are vulnerable to **reverse engineering**, cannot control who is running the code, and cannot revoke access when necessary. The market lacks an integrated solution that both protects source code and manages distribution and access in a comprehensive way.

## 1.2. Proposed Solution

**Code Protector** (system name: **IrisAuth**) is a **SaaS platform** that allows developers to upload source code to a server and distribute it to users through an **encrypted loader mechanism**. Instead of sharing source code directly, end users only receive a small loader of 1–2 lines. The server returns encrypted code only when the user passes all security checks, and the code exists only in **RAM** during execution — never written to a file.

The system is deployed on **AWS Cloud**, leveraging a **serverless architecture** and **managed services** to ensure high scalability, low global latency, and operation without manual infrastructure management.

---

# II. PROJECT OBJECTIVES

## 2.1. General Objective

Build a complete web platform that provides **secure source code protection and distribution services** for **Python** and **JavaScript**, with a multi-layer authentication system, an intuitive administration interface, and globally scalable **AWS Cloud** infrastructure.

## 2.2. Specific Objectives

- Design and implement a code distribution system via **encrypted loaders**, ensuring that source code is never exposed to end users in plaintext form.
- Build a **ECDH X25519 key exchange** mechanism combined with **AES-256-GCM encryption** to secure data transmission.
- Develop a **license management system** with features such as **HWID Lock**, expiration date, and execution limits.
- Build a **workspace** and **team collaboration** system with multiple user roles.
- Develop a responsive web admin interface distributed via **Amazon CloudFront** with low global latency.
- Integrate monitoring and alerting via **Amazon CloudWatch**, centralized logging, and notifications via **Discord Webhook**.
- Deploy a **serverless architecture** on **AWS Lambda**, store objects on **Amazon S3**, and structured data on **Amazon DynamoDB**.
- Integrate **Amazon SES** to send workspace member invitations and automated system notifications.

---

# III. PROJECT SCOPE

## 3.1. In Scope

### Backend & API

- **RESTful API** and **WebSocket** using **Node.js / Express.js** running on **AWS Lambda** via **API Gateway** or **Lambda Function URL**
- Handle **ECDH Handshake**, **AES-256-GCM encryption**, and **XOR protocol** for loader distribution
- **Rate limiting**, **IP access control**, **request signature validation**

### Frontend

- 5 interface pages: **Landing**, **Login**, **Register**, **Dashboard**, **Workspace**
- Static assets distributed globally via **Amazon CloudFront CDN**
- **Vanilla HTML5/CSS3/JavaScript**, no dependency on heavy frameworks
- Real-time synchronization via **AWS API Gateway WebSocket API**

### AWS Infrastructure

- **Amazon S3**: store encrypted source code, replacing local filesystem storage
- **Amazon DynamoDB**: **NoSQL database** replacing **SQLite**, storing users, workspaces, projects, licenses, logs, rate limits, and configurations
- **AWS Lambda**: runtime for **Express.js API**, supporting **serverless auto-scaling**
- **Amazon CloudFront**: distribute static files, cache S3 assets, SSL termination, custom domain
- **Amazon CloudWatch**: monitoring, log aggregation, metric alarms, system dashboards
- **Amazon SES**: send workspace invitation emails via token

### Loader & Client

- **Python loader** using `urllib` for **Python 3.7+**
- **Node.js loader**
- **Browser Userscript** using **Tampermonkey**
- Two distribution mechanisms: **ECDH handshake v3** and **XOR execute v2**

### Business Features

- **License system**: single creation, bulk creation, **HWID binding**, **CSV export**
- **IP Blacklist**, **IP Whitelist**, **Rate Limiting**, **Workspace PIN**
- Team management: invite members via email token with 3 roles `owner`, `editor`, `viewer`
- Multi-file project management with **bundle generation** for **Python** and **Node.js**
- **Python AST Obfuscator** as an independent module
- Real-time synchronization via **WebSocket**, notifications via **Discord Webhook**

## 3.2. Out of Scope

- **Native mobile applications**
- Support for programming languages other than **Python** and **JavaScript**
- Integration of **payment gateway**
- **CI/CD automation** beyond basic deployment using **AWS SAM** or **AWS CDK**

---

# IV. SYSTEM ARCHITECTURE & TECHNOLOGY

## 4.1. Technology Stack

| Layer | Service / Technology | Details |
|------|----------------------|----------|
| Runtime API | **Node.js v18+** on **AWS Lambda** | ES Module, Express.js |
| Database | **Amazon DynamoDB** | NoSQL key-value, on-demand capacity, automatic TTL for `rate_limits` and `sessions` |
| Object Storage | **Amazon S3** | Store encrypted scripts compressed with gzip, versioning enabled, server-side encryption |
| CDN & Edge | **Amazon CloudFront** | Distribute static frontend, cache S3 assets, SSL termination, custom domain |
| Compute | **AWS Lambda** | Auto-scale, serverless, reduced operational cost |
| Email | **Amazon SES** | Send workspace invitation emails, configured with DKIM and SPF |
| Monitoring | **Amazon CloudWatch** | Log Groups, metric alarms, dashboard |
| Real-time | **API Gateway WebSocket API** | Broadcast updates per workspace |
| Frontend | **HTML5, CSS3, Vanilla JS** | 5 static pages deployed to S3 bucket |
| Compression | **Pako (Gzip)** | Compress code before storing in S3 |

## 4.2. AWS System Architecture Overview

The **IrisAuth** system is deployed using a **serverless architecture** on **AWS**, completely eliminating the need for managing physical servers. A typical request flow is as follows:

- Client (**browser** or **Python loader**) sends requests to **CloudFront Distribution**
- Static assets such as HTML, CSS, JavaScript are served directly from **Amazon S3** via **CloudFront**
- API requests at path ` /api/* ` are forwarded to **API Gateway**, then to **AWS Lambda**
- **Lambda functions** process business logic, read/write metadata in **Amazon DynamoDB** and script content in **Amazon S3**
- Email invitations are sent via **Amazon SES**
- All Lambda logs are streamed to **Amazon CloudWatch Logs**
- Real-time connections go through **API Gateway WebSocket API**

## 4.3. Authentication System

The system does not use external **JWT** libraries but implements a custom authentication mechanism.

- Tokens are signed using **HMAC-SHA256**, with a randomly generated 48-byte secret stored in **DynamoDB** in the `app_config` table
- Token has a **TTL of 7 days**
- When a user changes their password, all existing tokens are immediately invalidated by comparing with `password_changed_at`
- Tokens are sent via **HttpOnly Cookie** named `token`, and also supported via `Authorization` header
- Passwords are hashed using **PBKDF2-SHA256** with **210,000 iterations** and a 16-byte random salt
- Legacy accounts using plain **SHA-256** are upgraded upon login
- **Workspace PIN** also uses **PBKDF2** and generates a separate session token stored in the `pin_verifications` table

## 4.4. Two Code Distribution Protocols

### Protocol v2 — `GET /api/v5/execute`

- Client sends parameters including `id`, license key, HWID, timestamp, nonce, and **HMAC-SHA256 signature**
- Server validates the signature and checks timestamp within ±5 minutes to prevent **replay attacks**
- Response is encrypted using **XOR** with a key derived from **SHA-256**
- Script content is retrieved from **Amazon S3**

### Protocol v3 — `POST /api/v5/handshake`

- Client generates an **X25519 key pair**, sends the public key to the server along with a verification signature
- **AWS Lambda** generates its own key pair and computes the shared secret using `diffieHellman()`
- The **AES key** is derived using **HKDF-SHA256**
- Script is encrypted using **AES-256-GCM**, returning ciphertext along with the server public key
- Each session has a unique session key, supporting **perfect forward secrecy**

## 4.5. Security Algorithms and Standards

| Algorithm | Application in System |
|-----------|----------------------|
| **PBKDF2-SHA256** | Hash user passwords and Workspace PIN |
| **HMAC-SHA256** | Sign auth tokens, loader requests, and responses |
| **ECDH X25519** | Key exchange for protocol v3 |
| **HKDF-SHA256** | Derive AES key from shared secret |
| **AES-256-GCM** | Encrypt stored and transmitted scripts |
| **XOR + SHA-256** | Encryption for protocol v2 |
| **Gzip** | Compress content before storing in **Amazon S3** |
| `crypto.timingSafeEqual` | Secure comparison of tokens and signatures |
| Nonce + Timestamp | Prevent replay attacks |
| **S3 Server-Side Encryption** | Encrypt data at rest in S3 |
| **TLS 1.2+** | Secure all client-server connections |

## 4.6. Database — Amazon DynamoDB

Replacing **SQLite** with **Amazon DynamoDB**, a fully managed **NoSQL database**. The data design can follow either **single-table** or **multi-table** models depending on access patterns.

| DynamoDB Table | Partition Key / Sort Key | Function |
|--------------|---------------------------|-----------|
| `users` | PK: `userId` | Manage user accounts |
| `workspaces` | PK: `workspaceId` | Manage workspace, loader key, language, encryption keys |
| `projects` | PK: `workspaceId`, SK: `projectId` | Manage projects and security settings |
| `project_files` | PK: `projectId`, SK: `fileId` | Manage files and folders in project |
| `licenses` | PK: `workspaceId`, SK: `licenseKey` | Manage license keys, HWID, expiration |
| `access_lists` | PK: `workspaceId`, SK: `ip#type` | Manage blacklist and whitelist |
| `workspace_members` | PK: `workspaceId`, SK: `userId` | Manage members and roles |
| `workspace_invitations` | PK: `token` | Store email invitation tokens |
| `pin_verifications` | PK: `sessionToken` | Manage sessions after PIN verification |
| `logs` | PK: `workspaceId`, SK: `timestamp#uuid` | System activity logs |
| `app_config` | PK: `configKey` | System configuration |
| `rate_limits` | PK: `rateLimitKey` | Request rate limiting data |

**TTL** is enabled for tables `workspace_invitations`, `pin_verifications`, and `rate_limits` to automatically delete expired data.

## 4.7. Amazon S3 — Object Storage for Script Content

- Completely replaces local filesystem storage
- Each script file is stored with key format `{workspaceId}/{uuid}_{timestamp}_{hash}.gz`
- Content is compressed using **Gzip** and encrypted with **AES-256-GCM** before storage
- S3 bucket enables **versioning**, **server-side encryption**, and **Block Public Access**
- **AWS Lambda** accesses S3 via **IAM Role** following the **least privilege principle**
- Static frontend assets are also stored in a separate **S3 bucket** and distributed via **CloudFront**

## 4.8. Amazon CloudFront — CDN & Edge

- Uses a single **CloudFront Distribution** to serve both static frontend and API proxy
- Behavior configuration:
  - ` /api/* ` → routes to **API Gateway**
  - ` /* ` → routes to **S3 static bucket**
- Enforces **HTTPS**, using **TLS 1.2** or higher
- Optimizes caching for static assets while limiting cache for API responses
- Supports custom domain with certificates from **AWS Certificate Manager**
- Can integrate **AWS WAF** to block bots, SQL injection, and XSS at the edge layer

## 4.9. Amazon CloudWatch — Monitoring & Observability

- Create **Log Groups** for each **Lambda function**
- Create **Metric Filters** to extract metrics such as execution count, handshake rate, error rate
- Configure **CloudWatch Alarms** when:
  - Lambda error rate exceeds threshold
  - p99 latency exceeds acceptable level
  - **DynamoDB** experiences throttle events
- Build **CloudWatch Dashboard** to monitor request volume, latency, error breakdown, and storage usage
- Use **structured JSON logging** for easier querying via **CloudWatch Logs Insights**

## 4.10. Amazon SES — Email Service

- Send workspace invitation emails containing tokens
- Can use **SES Templates** or inline HTML
- Configure **DKIM**, **SPF**, **DMARC** to improve deliverability
- Monitor **bounce rate** and **complaint rate** via integration with **CloudWatch**
- Move from **sandbox mode** to **production** after verifying sending domain

## 4.11. Role-Based Access Control

| Role | Project Management | License Management | View Logs | Workspace Settings | Invite Members |
|--------|------------------|------------------|---------|-------------------|----------------|
| `owner` | Full access | Full access | Full access | Full access | Full access |
| `admin` | Full access | Full access | Full access | Limited | Allowed |
| `editor` | CRUD | Create/View | View | Not allowed | Not allowed |
| `viewer` | Read-only | View only | View | Not allowed | Not allowed |

---

# V. DETAILED FEATURES

## 5.1. Core Workflow

- Developer logs in and creates a workspace, metadata is stored in **Amazon DynamoDB**
- Developer uploads or inputs source code, the code is compressed using **Gzip**, encrypted using **AES-256-GCM**, then stored in **Amazon S3**
- Developer receives a short loader snippet for **Python** or **JavaScript**
- End-user runs the loader, which generates **HWID** from device information and hashes it using **SHA-256**
- Loader calls `GET /api/v5/execute` or `POST /api/v5/handshake` via **CloudFront**
- **AWS Lambda** verifies signature, checks license, HWID, rate limits, and access list in **DynamoDB**
- If valid, Lambda retrieves the script from **S3**, decrypts it, and returns content according to the corresponding protocol
- Loader decrypts and executes directly in **RAM**, without writing to a file
- The system logs activity into the `logs` table, updates execution count, and broadcasts events via **WebSocket**
- **CloudWatch** collects logs and metrics in real time

## 5.2. License System

- Create single **license key** with format **[prefix]LIC-XXXXXXXX-XXXXXXXX[suffix]**, supporting custom keys
- Bulk creation up to **100 licenses per batch** using **DynamoDB batch write**
- **HWID Lock**: when end-user runs for the first time, HWID is stored in **DynamoDB**; subsequent runs with different HWID are rejected
- Enable or disable `hwid_lock` per license
- Set expiration via `expiration_date`, checked in real time on each execution
- Bind license to a specific project or entire workspace
- Support HWID reset when users change devices
- Export list to **CSV** including `key`, `note`, status, `HWID`, `OS`, usage count, and last used time

## 5.3. Access Control

- **IP Blacklist**: block specific IPs from accessing the workspace
- **IP Whitelist**: allow access only from specified IPs
- **CloudFront WAF**: adds an additional edge protection layer
- **Max executions**: limit total number of project executions
- **Rate limiting** using **sliding window model** on **DynamoDB**
  - Login: max 10 requests/min/IP
  - Register: max 5 requests/min/IP
  - Loader execute: max 120 requests/min/IP
  - ECDH handshake: max 60 requests/min/IP
  - License creation: max 30 requests/min
- **TTL** on `rate_limits` automatically clears expired data
- **Workspace PIN** is hashed using **PBKDF2**
- All loader requests must include a valid **HMAC-SHA256 signature**

## 5.4. Multi-file Project Management

- Organized using directory structure with `file` and `folder`
- Define **entry point** for multi-file projects
- **Bundle generation**:
  - **Python**: merge files, handle internal imports
  - **Node.js**: bundle modules according to project structure
- Supports recognition of over 30 file extensions
- Operations: create, rename, move, copy, delete, upload, search
- Large content is stored in **S3** and retrieved when needed

## 5.5. Workspace & Team Collaboration

- Each user can own multiple independent workspaces
- Each workspace has its own `loader_key` as a public identifier
- Invite members via email token using **Amazon SES**
- Invitation tokens are stored in `workspace_invitations`
- Supports 4 roles: `owner`, `admin`, `editor`, `viewer`
- Workspace changes are broadcast in real time via **API Gateway WebSocket API**
- Can configure **Discord Webhook** to send important event notifications
- Dashboard displays statistics such as total executions, licenses, and members

## 5.6. Logging & Monitoring

- Log events such as:
  - `LOAD_SCRIPT`
  - `ECDH_HANDSHAKE`
  - `BLOCK_ACCESS`
  - `INVALID_LICENSE`
  - `INVALID_HWID`
  - `INVALID_SIGNATURE`
  - `CREATE_LICENSE`
  - `BATCH_CREATE_LICENSE`
- Each log includes `workspace_id`, `action`, details, IP, country, and timestamp
- Business logs are stored in **DynamoDB**
- System logs from Lambda are stored in **CloudWatch Logs**
- Configure **CloudWatch Alarms** for early warnings
- Build **CloudWatch Dashboard** to monitor KPIs
- Support viewing logs per workspace in the admin interface

## 5.7. Python AST Obfuscator

File `obfuscator.py` is a Python code obfuscation tool operating independently at the **AST layer**.

- Parses syntax and checks for compatibility issues
- Renames variables, functions, and parameters into random strings
- Adds **dead code** to obfuscate logic
- Outputs compatibility reports with severity levels `error` or `warning`

---

# VI. IMPLEMENTATION PLAN

| Phase | Content | Estimated Time |
|----------|----------|-------------------|
| 1. Analysis & Design | Define requirements, design **DynamoDB schema**, API, and **CloudFront behavior** | Week 1–2 |
| 2. AWS Infrastructure | Provision **Lambda**, **DynamoDB**, **S3**, **CloudFront**, **SES**, **CloudWatch** using **AWS SAM** or **AWS CDK** | Week 3 |
| 3. Backend Core | Build authentication, workspace, project, file management APIs | Week 4–6 |
| 4. Security & Loader | Implement **ECDH handshake**, **AES encryption**, **Python** and **JavaScript loaders** | Week 7–8 |
| 5. License & Access | Complete license system, HWID, rate limiting, blacklist, whitelist | Week 9 |
| 6. Frontend & Email | Complete UI, integrate WebSocket and **Amazon SES** | Week 10–11 |
| 7. Obfuscator | Complete **Python AST Obfuscator** and bundle generation | Week 12 |
| 8. Testing & Deploy | Unit test, integration test, load test, production deployment | Week 13–14 |

---

# VII. EXPECTED RESULTS

After completion, the project will provide:

- A complete web platform running on **AWS Cloud** with **serverless architecture**
- A source code protection system with two protocols: **XOR execute v2** and **ECDH/AES-GCM v3**
- An intuitive admin interface distributed via **Amazon CloudFront**
- Loader clients compatible with **Python 3.7+**, **Node.js v14+**, and **Tampermonkey**
- Automated email invitation system via **Amazon SES**
- A full monitoring and alerting platform using **Amazon CloudWatch**
- Technical documentation including architecture, API specification, and deployment guide using **AWS SAM** or **AWS CDK**

---

# VIII. TECHNOLOGIES USED

**Node.js**, **Express.js**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudFront**, **Amazon CloudWatch**, **Amazon SES**, **AWS API Gateway**, **AWS WAF**, **AWS KMS**, **AWS IAM**, **ECDH X25519**, **AES-256-GCM**, **HMAC-SHA256**, **HKDF-SHA256**, **PBKDF2-SHA256**, **Python urllib**, **Python AST Manipulation**, **Gzip Compression**, **CORS**, **Rate Limiting**, **Role-Based Access Control**, **Discord Webhook**.