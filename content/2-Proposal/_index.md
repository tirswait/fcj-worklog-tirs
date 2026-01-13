---
title: "Proposal"
date: 2026-01-12
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Website Security Baseline Assessment Platform

## 1. Proposal Overview

The Website Security Baseline Assessment Platform is designed to support rapid evaluation of a website’s basic security posture based on publicly accessible and non-intrusive security indicators.  
The system aims to assist organizations and internal technical teams in identifying common security risks at an early stage, prior to deciding whether deeper security assessments such as penetration testing are necessary.

The platform focuses on providing a high-level view of the current security status of a website, serving both technical and managerial decision-making needs.

---

## 2. Problem Statement and Motivation

### Current Challenges

In practice, many websites:

* Are not assessed for security on a regular basis
* Lack a fast, lawful risk screening tool
* Rely heavily on time-consuming and costly penetration testing activities

The absence of a baseline security assessment step makes it difficult for organizations to prioritize security investments and remediation efforts.

### Proposed Solution

The proposed system aims to:

* Assess common security configurations using publicly available information
* Provide an overview-level risk report
* Support informed decision-making without deep system interaction

The platform is not intended to replace comprehensive penetration testing, but rather to act as a **baseline assessment** step within a broader security workflow.

---

## 3. Scope and Implementation Principles

The system is developed according to the following principles:

* Only **publicly accessible information** is collected and analyzed
* No attacks, exploitation, or circumvention of security controls are performed
* No brute-force, fuzzing, or authentication bypass techniques are used

The assessment focuses on:

* HTTPS / SSL configuration
* HTTP Security Headers
* Server information exposure
* robots.txt and sitemap analysis
* Detection of common CMS platforms and publicly disclosed versions

This approach ensures the system is **legally compliant** and suitable for use in enterprise environments.

---

## 4. System Design

### Core Architecture

The platform is designed to be **portable and independent**:

* Core logic (scanning, risk evaluation, report generation) operates independently
* Deployment options include:
  * Local environments
  * On-premise servers
  * VPS
  * Cloud platforms (e.g., AWS)

The core components are not tightly coupled to any specific cloud provider, enabling flexible deployment and scalability.

### Role of AWS

AWS is used as an enabling infrastructure to:

* Deploy the system in a cloud-ready architecture
* Store assessment reports and output data
* Log system activities and monitor operational status

If AWS is not used, the platform remains fully functional with no loss of core capabilities.

---

## 5. Deliverables

The platform produces the following outputs:

* Security risk assessment reports categorized by severity (Low / Medium / High)
* Clear and accessible explanations for non-technical users
* Actionable recommendations for improving website security

The reports are designed to:

* Support technical teams in rapid security evaluation
* Be understandable for management and decision-makers

---

## 6. Expected Value

### Organizational Benefits

* Availability of an internal security assessment tool
* Reduced time and cost for initial risk screening
* A foundation for more advanced security assessments

### Benefits for the Project Team

* Practical experience aligned with IA / AI disciplines
* Exposure to real-world product-oriented thinking
* Hands-on work with scalable and reusable system architectures

---

## 7. Conclusion

This proposal aims to deliver a **focused, well-scoped, and practically valuable** website security assessment platform.  
The system prioritizes effectiveness and legal safety over unnecessary technical complexity.  
AWS is utilized to enhance deployment and operational quality while preserving the independence of the core system.
