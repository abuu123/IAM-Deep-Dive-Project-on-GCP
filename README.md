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
gcloud logging read "protoPayload.methodName=SetIamPolicy"## 🔎 IAM Threat Modeling

### Identified Risk Categories

1. Privilege Escalation  
2. Lateral Movement  
3. Service Account Impersonation  
4. Excessive Project-Level Permissions  
5. Policy Inheritance Misconfiguration  

---

### Example Privilege Escalation Scenario

If a user has:
- roles/iam.serviceAccountUser
- roles/iam.roleAdmin
##IAM Threat Modeling
- Impersonate service accounts
- Potentially gain Owner-level access indirectly

Mitigation Implemented:
- Separated IAM role management from service account usage
- Restricted high-privilege roles to security administrators only
- Monitored SetIamPolicy events via Cloud Logging
## 🔐 IAM Conditions (Context-Aware Access)

Implemented conditional access for sensitive roles:

Example:
- Developer role allowed only during business hours
- Access restricted to specific IP range
- Limited access to specific resource types

Purpose:
- Reduce risk of credential misuse
- Limit exposure window
- Enforce contextual Zero Trust controls
## 📊 IAM Monitoring & Detection Strategy

Configured logging to detect:

- SetIamPolicy changes
- Service account key creation
- Role binding modifications
- Privileged role assignments

Sample Log Query:

protoPayload.methodName="SetIamPolicy"

Security Benefit:
- Real-time detection of privilege changes
- Faster incident response
- Audit compliance tracking## 🧱 Blast Radius Reduction Strategy

Original State:
- Project-wide Editor roles
- Broad inherited permissions

Hardened State:
- Resource-level IAM bindings
- Custom roles with minimal permissions
- Restricted service account scope

Result:
Reduced potential impact of compromised identity from project-wide to resource-specific.
## 🎯 Enterprise IAM Design Principles Applied

- Principle of Least Privilege
- Separation of Duties
- Identity Boundary Segmentation
- Policy Inheritance Awareness
- Continuous Monitoring
- Guardrail Enforcement via Org Policies

This lab simulates how IAM would be structured in a mid-size enterprise GCP environment.







