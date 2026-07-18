# Introduction to AZ-400
```
1. Azure DevOps Overview
2. Benefits of Using Azure DevOps
3. How to Create a Free Azure DevOps Account
4. Azure DevOps Organization
5. Azure DevOps Project
6. Azure DevOps Team
7. Azure DevOps Users
8. Azure DevOps Access Levels
9. Azure DevOps Permissions
10. Azure DevOps Organization Settings
11. Azure Active Directory (Microsoft Entra ID) Integration with Azure DevOps
12. Difference Between Azure DevOps Accounts and Azure Portal Accounts
13. How to Add Existing Azure DevOps Organization Users to Microsoft Entra ID (Azure AD)
14. Managing Azure DevOps Access Using Microsoft Entra ID
15. Scenario: Azure DevOps Account and Azure Portal Account Are Different
16. Best Practices for Azure DevOps Organization and User Management
```
---

# Module 1: Azure DevOps Overview

## Theory

Azure DevOps is Microsoft's cloud-based platform that helps teams plan, develop, test, deploy, and monitor applications throughout the Software Development Lifecycle (SDLC).

It provides integrated services for:

* Project Management
* Source Code Management
* CI/CD
* Package Management
* Testing

Azure DevOps supports:

* Agile
* Scrum
* Kanban
* Waterfall

Works with

* Azure
* AWS
* Google Cloud
* Kubernetes
* Docker
* GitHub
* Jenkins
* Terraform
* Ansible

---

## Azure DevOps Services

| Service          | Purpose                    |
| ---------------- | -------------------------- |
| Azure Boards     | Project Planning           |
| Azure Repos      | Git Repository             |
| Azure Pipelines  | CI/CD                      |
| Azure Test Plans | Manual & Automated Testing |
| Azure Artifacts  | Package Management         |

---

## Practical

### Explore Azure DevOps Portal

Tasks

* Login to Azure DevOps
* Explore Dashboard
* Open Organization
* Open Project
* View Boards
* View Repos
* View Pipelines
* View Artifacts
* View Test Plans

---

## Points to Remember

Azure DevOps is SaaS.

No infrastructure management required.

Supports Git and TFVC.

Supports Windows, Linux and Mac.

Supports multiple programming languages.

---

# Module 2: Benefits of Azure DevOps

## Theory

Major Benefits

* Centralized project management
* Version control
* Automated CI/CD
* Work Item Tracking
* Collaboration
* Security
* Scalability
* Integration with Microsoft products
* Third-party integrations
* Multi-cloud deployment

---

## Practical

Explore Marketplace Extensions

Install one extension.

Examples

* SonarCloud
* Terraform
* Docker
* Slack

---

## Points to Remember

Azure DevOps reduces manual work.

Supports DevSecOps.

Supports Infrastructure as Code.

---

# Module 3: Create Free Azure DevOps Account

## Theory

Requirements

* Microsoft Account
* GitHub Account (optional)

Free Tier Includes

* Unlimited private Git repositories
* Five basic users
* Azure Pipelines
* Azure Boards

---

## Practical

### Lab

Step 1

Open

[https://dev.azure.com](https://dev.azure.com)

Step 2

Sign in using Microsoft Account

Step 3

Create Organization

Step 4

Choose Country

Step 5

Complete CAPTCHA

Step 6

Organization Created

---

## Verify

Organization Dashboard should open successfully.

---

## Points to Remember

One Microsoft account can create multiple organizations.

Organization names are globally unique.

---

# Module 4: Azure DevOps Organization

## Theory

Organization is the highest-level container.

Contains

* Users
* Projects
* Billing
* Security
* Policies

One organization can have multiple projects.

---

## Practical

Create Organization

Verify

* URL
* Region
* Organization Settings

---

## Points to Remember

URL Format

```
https://dev.azure.com/OrganizationName
```

---

# Module 5: Azure DevOps Project

## Theory

A Project contains

* Boards
* Repositories
* Pipelines
* Teams
* Artifacts
* Test Plans

---

## Practical

Create Project

Example

Name

```
CloudProject
```

Visibility

Private

Version Control

Git

Work Process

Agile

---

## Verify

Project Dashboard

---

## Points to Remember

One organization

↓

Many Projects

---

# Module 6: Azure DevOps Team

## Theory

A Team is a group of users working together.

Each team has

* Backlog
* Sprint
* Dashboard
* Iteration

---

## Practical

Create Team

Example

```
Cloud Team
```

Add Members

Verify Dashboard

---

## Points to Remember

One Project

↓

Many Teams

---

# Module 7: Azure DevOps Users

## Theory

Users are invited into the Organization.

Users can be

* Developers
* Testers
* Project Managers
* Administrators
* Stakeholders

---

## Practical

Invite User

Organization Settings

↓

Users

↓

Add User

Assign

* Email
* Access Level
* Project
* Group

---

## Verify

Invitation Email

---

## Points to Remember

Users must accept invitation.

---

# Module 8: Azure DevOps Access Levels

## Theory

Available Access Levels

| Access             | Purpose   |
| ------------------ | --------- |
| Stakeholder        | Free      |
| Basic              | Developer |
| Basic + Test Plans | Tester    |

---

## Practical

Change Access Level

Organization Settings

↓

Users

↓

Access Level

---

## Verify

User permissions updated.

---

## Points to Remember

Stakeholder has limited features.

Basic users consume licenses.

---

# Module 9: Azure DevOps Permissions

## Theory

Permission Levels

* Organization
* Project
* Repository
* Pipeline
* Branch
* Library

Permission Types

Allow

Deny

Inherited

---

## Practical

Navigate

Project Settings

↓

Repositories

↓

Security

Grant

Read

Contribute

Create Branch

Delete Branch

---

## Verify

Login as second user.

Test access.

---

## Points to Remember

Deny overrides Allow.

Always use groups instead of assigning permissions directly to individual users.

---

# Module 10: Organization Settings

## Theory

Contains

* Users
* Billing
* Extensions
* Policies
* Audit
* Security

---

## Practical

Explore

Organization Settings

View

Billing

Extensions

Security

Audit Logs

---

## Points to Remember

Only Organization Administrator can modify settings.

---

# Module 11: Microsoft Entra ID Integration

## Theory

Azure DevOps integrates with Microsoft Entra ID.

Benefits

* Single Sign-On (SSO)
* Centralized Identity Management
* MFA
* Conditional Access
* User Synchronization

---

## Practical

Navigate

Organization Settings

↓

Microsoft Entra ID

View Connection Status

---

## Points to Remember

One Organization

↓

One Microsoft Entra Tenant

---

# Module 12: Azure DevOps Account vs Azure Portal Account

## Theory

| Azure DevOps | Azure Portal     |
| ------------ | ---------------- |
| Source Code  | Cloud Resources  |
| Pipelines    | Azure Services   |
| Boards       | Resource Groups  |
| Git          | Virtual Machines |
| CI/CD        | Networking       |

---

## Practical

Login to

Azure Portal

Azure DevOps

Compare

Subscriptions

Organizations

Projects

---

## Points to Remember

Both use Microsoft identity, but they serve different purposes and are managed separately.

---

# Module 13: Add Existing Azure DevOps Users to Microsoft Entra ID

## Theory

Scenario

User already exists in Azure DevOps.

Needs centralized identity.

---

## Practical

Microsoft Entra ID

↓

Users

↓

New User

↓

Invite External User

↓

Accept Invitation

↓

Azure DevOps

↓

Organization Settings

↓

Connect User

---

## Verify

User signs in using Entra ID.

---

## Points to Remember

Email addresses should match to simplify identity mapping.

---

# Module 14: Managing Azure DevOps Access Using Microsoft Entra ID

## Theory

Access Management

* Groups
* MFA
* Conditional Access
* Privileged Identity Management (PIM)
* Dynamic Groups

---

## Practical

Create Security Group

```
AzureDevOpsDevelopers
```

Assign Azure DevOps permissions.

Add users to group.

Verify inherited permissions.

---

## Points to Remember

Manage permissions through Entra ID groups rather than individual user assignments for easier administration.

---

# Module 15: Scenario – Azure DevOps Account and Azure Portal Account Are Different

## Theory

Example

Azure Portal

```
admin@company.com
```

Azure DevOps

```
developer@gmail.com
```

Problems

* Subscription mismatch
* Permission issues
* Pipeline authentication failures
* Service connection errors

---

## Practical

Login with two different accounts.

Attempt to create an Azure Resource Manager service connection.

Observe authorization or subscription access issues.

Resolve by granting the correct Azure RBAC permissions or using the appropriate account.

---

## Points to Remember

Azure subscription access is controlled by Azure RBAC, while Azure DevOps organization access is controlled separately.

---

# Module 16: Best Practices for Azure DevOps Organization and User Management

## Best Practices

* Use Microsoft Entra ID integration.
* Use Security Groups instead of individual permissions.
* Enable Multi-Factor Authentication (MFA).
* Follow the Principle of Least Privilege.
* Use Git repositories for all source code.
* Create separate projects for different products or applications.
* Regularly review user access and permissions.
* Enable audit logs for compliance.
* Use branch policies to protect the main branch.
* Remove inactive users periodically.
* Standardize organization, project, and repository naming conventions.
* Use service principals or managed identities for automation instead of personal accounts.
* Separate development, testing, and production environments.

---

# Overall Hands-on Labs

| Lab    | Objective                                                                |
| ------ | ------------------------------------------------------------------------ |
| Lab 1  | Create an Azure DevOps account and organization                          |
| Lab 2  | Create a new project with Git and Agile process                          |
| Lab 3  | Create teams and invite users                                            |
| Lab 4  | Configure access levels and permissions                                  |
| Lab 5  | Explore organization settings and extensions                             |
| Lab 6  | Connect Azure DevOps with Microsoft Entra ID                             |
| Lab 7  | Add users through Entra ID groups and verify access                      |
| Lab 8  | Compare Azure DevOps identity with Azure Portal identity                 |
| Lab 9  | Troubleshoot different-account access scenarios                          |
| Lab 10 | Apply security and governance best practices for organization management |

This sequence progresses from basic concepts to enterprise identity and governance, making it well-suited as the opening module for an AZ-400 Azure DevOps Engineer course.
