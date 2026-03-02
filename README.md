# IAM-Deep-Dive-Project-on-GCP
# 🔐 GCP IAM Deep Dive Security Lab

## 📌 Objective

Design and implement a secure Identity & Access Management (IAM) architecture in Google Cloud Platform (GCP) using least privilege principles, role segmentation, service account hardening, and organization-level policy controls.

This project simulates real-world enterprise IAM challenges and demonstrates mitigation strategies against privilege escalation and excessive permissions.

---

## 🧠 Why IAM Matters

Misconfigured IAM is the leading cause of cloud breaches.

This lab demonstrates:

- Role-based access control (RBAC)
- Custom role creation
- Privilege minimization
- Service account security
- IAM audit analysis
- Policy enforcement at project and organization levels

---

## 🏗️ Lab Architecture

Environment includes:

- 1 GCP Project
- 3 User Personas:
  - Developer
  - Security Analyst
  - Auditor
- 2 Service Accounts
- 1 Compute Engine instance
- 1 Cloud Storage bucket
- Custom IAM Roles
- Organization Policy Constraints

---

## 👥 User Persona Access Design

### 🔹 Developer
Access Requirements:
- Deploy VM instances
- Read logs
- Cannot modify IAM
- Cannot delete storage

Implemented:
- Custom Compute Operator Role
- Log Viewer Role
- Denied IAM Admin privileges

---

### 🔹 Security Analyst
Access Requirements:
- View IAM policies
- Review logs
- Analyze security findings
- No resource deployment rights

Implemented:
- Security Reviewer role
- Logging Viewer role
- No Compute or Storage admin access

---

### 🔹 Auditor
Access Requirements:
- Read-only access across project
- No write permissions

Implemented:
- Viewer role
- Restricted from modifying any resource

---

## 🔐 Service Account Hardening

Initial Misconfiguration (Intentional):
- Service account assigned Editor role
- Service account key created manually

Remediation Steps:
- Replaced Editor with minimal custom role
- Disabled service account key creation
- Removed unused keys
- Implemented Workload Identity principles

---

## 🚨 Privilege Escalation Risk Analysis

Simulated Risks:

- User with IAM Role Admin + Service Account User
- Overlapping permissions between roles
- Excessive inherited permissions

Mitigation:
- Role separation
- Reduced inherited project-wide permissions
- Scoped IAM at resource level
- Reviewed policy bindings via gcloud CLI

---

## 🛡️ Organization Policy Enforcement

Applied constraints:

- Disable service account key creation
- Restrict external IP creation
- Enforce uniform bucket-level access
- Restrict OS Login settings

---

## 📊 Before vs After Security Posture

Before:
- Broad Editor roles
- Excessive service account privileges
- No org-level enforcement
- Risk of privilege escalation

After:
- Custom least-privilege roles
- Reduced blast radius
- Enforced security guardrails
- Improved auditability

---

## 🧪 IAM Audit Commands Used

```bash
gcloud projects get-iam-policy PROJECT_ID
gcloud iam roles describe ROLE_NAME
gcloud iam service-accounts list
gcloud logging read "protoPayload.methodName=SetIamPolicy"
