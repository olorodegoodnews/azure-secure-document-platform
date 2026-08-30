# Azure Storage Account RBAC Review

**Least-Privilege Access Model — Final Recommendation**

| | |
|---|---|
| **Storage account** | `securedoceast01` |
| **Resource group** | `rg-secure-document-platform` |
| **Document status** | Final RBAC Recommendation — Pending Implementation |
| **Prepared** | August 2026 |
| **Classification** | Internal — Security Review |

---

## Table of Contents

1. [Scope](#1-scope)
2. [Current IAM Findings](#2-current-iam-findings)
3. [Least-Privilege Principles](#3-least-privilege-principles)
4. [Final RBAC Assignment List](#4-final-rbac-assignment-list)
5. [Document Admins](#5-document-admins)
6. [Contributors](#6-contributors)
7. [Readers](#7-readers)
8. [Application Access](#8-application-access)
9. [Networking & Monitoring Access](#9-networking--monitoring-access)
10. [Identity & Access Management](#10-identity--access-management)
11. [Container Engineering](#11-container-engineering)
12. [Existing Assignment Remediation](#12-existing-assignment-remediation)
13. [Implementation Sequence](#13-implementation-sequence)
14. [Validation Checklist](#14-validation-checklist)
15. [Final Security Assessment](#15-final-security-assessment)

---

## 1. Scope

This document reviews the current Azure role-based access control (RBAC) configuration for the Azure Storage Account `securedoceast01`.

The objective is to identify excessive, overlapping, or unnecessarily broad permissions and establish a least-privilege RBAC model for:

- Document Administrators
- Storage Contributors
- Readers
- Application access
- Networking and monitoring access

The recommended model uses Microsoft Entra ID groups and managed identities where appropriate, with permissions assigned at the narrowest practical scope.

---

## 2. Current IAM Findings

The current environment contains 26 role assignments, with most assignments inherited from the Resource Group. The review identified the following findings.

### 2.1 Subscription Owner

**Assigned to:** Quadri Ahmed — Owner (subscription scope)

> **Assessment:** Highly privileged. Subscription-level Owner exceeds what is required for day-to-day project participation.

**Recommendation:** Retain only if required for authorized subscription-level administration. The Owner role should not be assigned to normal project members.

### 2.2 Storage Account Contributor

**Assigned to:** Ayomide, Collins, cyber_guy, Joseph, Kenny, Natasha, Temitope Collins, Winner — 8 users, inherited from the Resource Group

> **Assessment:** Broad management-plane access to storage account configuration. This is not required simply for users who need to upload, download, or manage blob data.

**Recommendation:** Review each assignment against the user's actual responsibility. Users who genuinely administer the storage account should receive the role through the `Contributors` group, scoped directly to `securedoceast01`. Unnecessary direct assignments should be removed after replacement access has been validated.

### 2.3 Storage Blob Data Owner

**Assigned to:** cyber_guy, Joseph

> **Assessment:** Higher privilege than required for normal document management.

**Recommendation:** Replace with `Storage Blob Data Contributor` where the user only needs to upload, modify, or delete blob data. Access should preferably be assigned through the `Document Admins` group and scoped to the required container(s). Group membership must be validated against actual job responsibilities before assignment.

### 2.4 Reader Assignments

**Assigned to:** Multiple users and groups, inherited from the Resource Group

> **Assessment:** Reader is read-only, but Resource Group scope may provide visibility into resources beyond what an individual role requires.

**Recommendation:** Retain Reader only where read-only visibility is required, and reduce the scope to the storage account or required resource where practical.

### 2.5 Network Contributor

**Assigned to:** Ayomide, Kenny — Resource Group scope

> **Assessment:** Aligned with networking responsibilities, but currently mixed with storage-scoped assignments rather than managed as a distinct function.

**Recommendation:** Networking permissions should be managed through the `Networking & Monitoring` group where possible, kept separate from storage administration.

---

## 3. Least-Privilege Principles

The following principles govern the final RBAC model:

1. **Least privilege** — users receive only the permissions required for their responsibilities.
2. **Group-based access** — permissions should preferably be assigned to Microsoft Entra ID groups rather than individual users.
3. **Narrow scope** — assignments should use the smallest practical scope.
4. **Separation of duties** — storage administration, document data access, identity management, networking, and application access should remain logically separated.
5. **Data roles instead of management roles** — users who only need to manage blob data should receive a Blob Data role rather than a Storage Account management role.
6. **Managed identity for applications** — application access should use a dedicated Managed Identity rather than human credentials.
7. **Privileged access restriction** — subscription-level Owner access should be restricted to authorized administrators.

---

## 4. Final RBAC Assignment List

| Principal | Recommended Role | Scope | Purpose |
|---|---|---|---|
| Document Admins | Storage Blob Data Contributor | Required blob container(s) | Upload, modify, and delete documents |
| Contributors | Storage Account Contributor | `securedoceast01` | Manage storage account configuration |
| Readers | Reader | `securedoceast01` or required resource | Read-only visibility |
| Application Managed Identity | Storage Blob Data Contributor | Required application container(s) | Application read/write access to blob data |
| Networking & Monitoring | Network Contributor | Required networking resources | Manage networking resources |
| Identity Engineer | No storage RBAC by default | None | Identity and authentication responsibilities |
| Container Engineers | No storage RBAC by default | None | Container deployment responsibilities |
| Subscription Administrator | Owner | Subscription | Authorized subscription-level administration |

---

## 5. Document Admins

**Recommended role:** Storage Blob Data Contributor
**Recommended scope:** Required blob container(s)

Document Administrators require the ability to manage document data, including uploading, modifying, and deleting documents.

They do not require permission to manage the storage account configuration. Therefore, `Storage Blob Data Contributor` is preferred over `Storage Account Contributor` or `Storage Blob Data Owner`.

The `Document Admins` group should be used for this access. Current users with `Storage Blob Data Owner`, including `cyber_guy` and `Joseph`, should be reviewed against their actual responsibilities before being assigned to this group.

---

## 6. Contributors

**Recommended role:** Storage Account Contributor
**Recommended scope:** `securedoceast01`

Contributors are responsible for storage-account administration and configuration.

The current eight `Storage Account Contributor` assignments should be reviewed individually. Only users whose responsibilities require storage-account administration should become members of the `Contributors` group.

The role should be assigned at the Storage Account scope rather than the Resource Group scope whenever possible.

---

## 7. Readers

**Recommended role:** Reader
**Recommended scope:** `securedoceast01` or another required resource scope

Readers require visibility without modification privileges.

Existing Resource Group-level Reader assignments should be reviewed to determine whether Resource Group visibility is actually required.

Where only storage-account visibility is necessary, Reader should be scoped to `securedoceast01`.

---

## 8. Application Access

**Recommended role:** Storage Blob Data Contributor
**Recommended scope:** Required application container(s)

The application should use a dedicated Microsoft Entra Managed Identity rather than a human user's credentials to access storage.

The Managed Identity should receive only the permissions necessary for the application's documented storage operations.

---

## 9. Networking & Monitoring Access

**Recommended role:** Network Contributor
**Recommended scope:** Required networking resources

Networking permissions should remain separate from storage administration.

The role should be limited to resources required for:

- Virtual Network management
- Subnet management
- Network Security Groups
- Network connectivity
- Related networking configuration

Network permissions should not automatically provide Storage Account Contributor access.

---

## 10. Identity & Access Management

The Identity Engineer is responsible for Microsoft Entra ID, users, groups, authentication, and identity-related configuration.

No storage RBAC should be assigned by default solely because a user is an Identity Engineer. Storage access should only be granted when a specific storage-related responsibility requires it.

The IAM/Security team should review and manage authorization through the appropriate administrative permissions rather than granting broad storage permissions unnecessarily.

---

## 11. Container Engineering

Container Engineers are responsible for Docker, Azure Container Registry, container deployment, and application deployment.

No storage RBAC should be assigned by default solely because a user belongs to the Container Engineers group.

If the application requires storage access, the preferred design is for the application's Managed Identity to receive the required Blob Data role.

---

## 12. Existing Assignment Remediation

The current assignments should be migrated carefully rather than removed immediately.

### Storage Account Contributor
Review all eight current assignments. Users requiring storage administration should be moved to the `Contributors` group. The assignment should be scoped to `securedoceast01`. Unnecessary Resource Group-level assignments should be removed after the replacement access has been created and tested.

### Storage Blob Data Owner
Review the assignments for `cyber_guy` and `Joseph`. Where their responsibilities only require document data management, replace `Storage Blob Data Owner` with `Storage Blob Data Contributor`, scoped to the required container(s).

### Reader
Review Resource Group-level Reader assignments. Reduce them to the storage account or required resource where Resource Group visibility is unnecessary.

### Network Contributor
Review the current individual Network Contributor assignments. Where appropriate, move the authorization model to the `Networking & Monitoring` group and scope it to the required networking resources.

### Owner
Subscription-level Owner should remain restricted to authorized administrators.

---

## 13. Implementation Sequence

1. Confirm the final group membership with the project team.
2. Create or verify the `Document Admins`, `Contributors`, and `Readers` groups.
3. Create or verify the application's Managed Identity.
4. Assign the required roles to the appropriate groups or Managed Identity.
5. Apply the narrowest practical scope.
6. Test the required permissions.
7. Confirm users can perform their assigned tasks.
8. Confirm users cannot perform unnecessary administrative tasks.
9. Remove redundant or excessive existing assignments.
10. Re-check the final RBAC configuration.

> Existing permissions should not be removed before replacement access has been validated.

---

## 14. Validation Checklist

- [ ] Document Admins can manage required blob data.
- [ ] Document Admins cannot unnecessarily manage the Storage Account.
- [ ] Contributors can manage `securedoceast01`.
- [ ] Contributors do not receive unnecessary Resource Group permissions.
- [ ] Readers have read-only access.
- [ ] Application Managed Identity can access only the required container(s).
- [ ] Networking & Monitoring can manage required networking resources.
- [ ] Identity Engineer does not receive unnecessary storage privileges.
- [ ] Container Engineers do not receive unnecessary storage privileges.
- [ ] Subscription Owner access is restricted to authorized administrators.
- [ ] Redundant direct user assignments have been removed after validation.

---

## 15. Final Security Assessment

The current RBAC configuration contains several broad and overlapping permissions that do not consistently follow least privilege.

The recommended model separates:

- Storage administration
- Document data management
- Read-only access
- Application access
- Networking access
- Identity administration

**Target authorization model:**

```
Microsoft Entra Group / Managed Identity  →  Azure RBAC Role  →  Narrow Resource Scope
```

This reduces unnecessary privilege, improves separation of duties, and makes access easier to manage and audit.

*The final RBAC configuration should be implemented only after the proposed group memberships and assignments have been reviewed and approved by the project team.*
