# AZ-400 Practical Modules: Azure DevOps Organization and User Management

## Lab Environment

### Required Accounts

* Microsoft account
* Azure DevOps account
* Azure subscription for service-connection practice
* Microsoft Entra ID tenant
* At least one test user account

### Required Tools

* Azure CLI
* Azure DevOps CLI extension
* Git
* Visual Studio Code
* PowerShell or Bash

---

# Module 1: Azure DevOps CLI Installation and Configuration

## Objective

Install Azure CLI, add the Azure DevOps extension, sign in, and configure default values.

## Step 1: Check Azure CLI

```bash
az version
```

## Step 2: Sign in to Azure

```bash
az login
```

To use a specific tenant:

```bash
az login --tenant <TENANT-ID>
```

Display the active account:

```bash
az account show --output table
```

List all accessible subscriptions:

```bash
az account list --output table
```

Select a subscription:

```bash
az account set --subscription "<SUBSCRIPTION-ID-OR-NAME>"
```

## Step 3: Install the Azure DevOps Extension

```bash
az extension add --name azure-devops
```

Check the installed extension:

```bash
az extension list --output table
```

Upgrade the extension:

```bash
az extension update --name azure-devops
```

## Step 4: Verify Azure DevOps Commands

```bash
az devops --help
```

```bash
az devops project --help
```

```bash
az devops team --help
```

## Step 5: Define Environment Variables

### Bash or macOS

```bash
export AZDO_ORG="https://dev.azure.com/myorganization"
export AZDO_PROJECT="CloudProject"
```

### PowerShell

```powershell
$env:AZDO_ORG = "https://dev.azure.com/myorganization"
$env:AZDO_PROJECT = "CloudProject"
```

## Step 6: Configure Default Organization and Project

```bash
az devops configure --defaults \
organization="$AZDO_ORG" \
project="$AZDO_PROJECT"
```

View the configuration:

```bash
az devops configure --list
```

## Points to Remember

* The Azure DevOps commands are provided through the `azure-devops` Azure CLI extension.
* The organization URL format is:

```text
https://dev.azure.com/organization-name
```

* Azure login and Azure DevOps organization access are related to identity but are managed separately.
* Configuring default values avoids repeatedly entering `--organization` and `--project`.

---

# Module 2: Create an Azure DevOps Organization

## Objective

Create and explore an Azure DevOps organization.

## Important Limitation

A new Azure DevOps organization is normally created through the Azure DevOps web portal. The standard Azure DevOps CLI does not provide a regular command for creating a new cloud organization.

## Practical Steps

1. Open:

```text
https://dev.azure.com
```

2. Sign in with a Microsoft account.
3. Select **New organization**.
4. Enter a globally unique organization name.
5. Select the hosting geography.
6. Complete the verification process.
7. Open the organization.

## Example

```text
Organization name: cloudnautic-devops
Organization URL: https://dev.azure.com/cloudnautic-devops
```

## Verify Organization Access

```bash
az devops project list \
--organization "https://dev.azure.com/cloudnautic-devops" \
--output table
```

## Open the Organization in a Browser

```bash
az devops project list \
--organization "https://dev.azure.com/cloudnautic-devops" \
--open
```

## Organization Contains

* Projects
* Users
* Security groups
* Billing
* Extensions
* Policies
* Agent pools
* Audit logs
* Microsoft Entra ID connection

## Points to Remember

* An organization is the highest Azure DevOps management container.
* One organization can contain multiple projects.
* An Azure DevOps organization is not the same as an Azure subscription.
* Deleting an organization can affect all its projects, repositories, pipelines and work items.

---

# Module 3: Create and Manage Azure DevOps Projects

## Objective

Create private and public Azure DevOps projects using the CLI.

## Step 1: Create a Private Agile Project

```bash
az devops project create \
--name "CloudProject" \
--description "Azure DevOps training project" \
--visibility private \
--source-control git \
--process agile \
--organization "$AZDO_ORG"
```

## Step 2: Create a Scrum Project

```bash
az devops project create \
--name "ScrumProject" \
--description "Scrum-based application project" \
--visibility private \
--source-control git \
--process scrum \
--organization "$AZDO_ORG"
```

## Step 3: List Projects

```bash
az devops project list \
--organization "$AZDO_ORG" \
--output table
```

## Step 4: Show Project Details

```bash
az devops project show \
--project "CloudProject" \
--organization "$AZDO_ORG" \
--output json
```

Display selected properties:

```bash
az devops project show \
--project "CloudProject" \
--organization "$AZDO_ORG" \
--query "{Name:name,ID:id,State:state,Visibility:visibility}" \
--output table
```

## Step 5: Store the Project ID

### Bash

```bash
PROJECT_ID=$(az devops project show \
--project "CloudProject" \
--organization "$AZDO_ORG" \
--query id \
--output tsv)

echo "$PROJECT_ID"
```

### PowerShell

```powershell
$PROJECT_ID = az devops project show `
  --project "CloudProject" `
  --organization $env:AZDO_ORG `
  --query id `
  --output tsv

Write-Output $PROJECT_ID
```

## Step 6: Delete a Test Project

```bash
az devops project delete \
--id "$PROJECT_ID" \
--organization "$AZDO_ORG" \
--yes
```

## Points to Remember

* Every project receives a default team.
* A project usually includes Boards, Repos, Pipelines, Test Plans and Artifacts.
* Project visibility can be private or public, depending on organizational policy.
* Use separate projects only when strong isolation is required.
* Too many projects can create administrative overhead.

---

# Module 4: Create and Manage Azure DevOps Teams

## Objective

Create teams and view team membership.

## Step 1: Create a Team

```bash
az devops team create \
--name "Cloud Team" \
--description "Cloud application development team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

## Step 2: Create Additional Teams

```bash
az devops team create \
--name "Development Team" \
--description "Application development team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

```bash
az devops team create \
--name "DevOps Team" \
--description "CI/CD and infrastructure team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

```bash
az devops team create \
--name "Testing Team" \
--description "Quality assurance team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

## Step 3: List Teams

```bash
az devops team list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--output table
```

## Step 4: Show Team Details

```bash
az devops team show \
--team "Cloud Team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--output json
```

## Step 5: List Team Members

```bash
az devops team list-member \
--team "Cloud Team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--output table
```

## Step 6: Update a Team

```bash
az devops team update \
--team "Cloud Team" \
--name "Cloud Platform Team" \
--description "Cloud and platform engineering team" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

## Points to Remember

* A project can contain multiple teams.
* Each team can have its own backlog, board, sprint and dashboard.
* A default team is created automatically with every project.
* Teams should represent actual product or delivery groups.
* Do not create a separate team for every individual user.

---

# Module 5: Add and Manage Azure DevOps Users

## Objective

Invite users and assign access levels.

## Access Levels

| Access level             | Typical user                                     |
| ------------------------ | ------------------------------------------------ |
| Stakeholder              | Business user or reviewer                        |
| Basic                    | Developer, administrator or DevOps engineer      |
| Basic + Test Plans       | Manual tester or test manager                    |
| Visual Studio Subscriber | User with an eligible Visual Studio subscription |

## Step 1: Add a Basic User

```bash
az devops user add \
--email-id "developer@company.com" \
--license-type express \
--send-email-invite true \
--organization "$AZDO_ORG"
```

Here, `express` represents Basic access.

## Step 2: Add a Stakeholder User

```bash
az devops user add \
--email-id "manager@company.com" \
--license-type stakeholder \
--send-email-invite true \
--organization "$AZDO_ORG"
```

## Step 3: Add a Basic + Test Plans User

```bash
az devops user add \
--email-id "tester@company.com" \
--license-type advanced \
--send-email-invite true \
--organization "$AZDO_ORG"
```

## Step 4: List Organization Users

```bash
az devops user list \
--organization "$AZDO_ORG" \
--output table
```

## Step 5: Search for a Particular User

```bash
az devops user list \
--organization "$AZDO_ORG" \
--query "items[?user.mailAddress=='developer@company.com']" \
--output table
```

## Step 6: Update a User’s Access Level

```bash
az devops user update \
--user "developer@company.com" \
--license-type stakeholder \
--organization "$AZDO_ORG"
```

Change the user back to Basic:

```bash
az devops user update \
--user "developer@company.com" \
--license-type express \
--organization "$AZDO_ORG"
```

## Step 7: Remove a User

```bash
az devops user remove \
--user "developer@company.com" \
--organization "$AZDO_ORG" \
--yes
```

## Points to Remember

* Adding a user to Microsoft Entra ID does not automatically grant Azure DevOps access.
* Adding a user to Azure DevOps does not automatically grant Azure subscription access.
* Access level defines feature availability.
* Permissions define what actions the user can perform.
* License availability should be checked before assigning paid access levels.

---

# Module 6: Azure DevOps Security Groups

## Objective

Create security groups and manage group memberships.

## Step 1: List All Groups

```bash
az devops security group list \
--organization "$AZDO_ORG" \
--output table
```

## Step 2: List Project-Level Groups

```bash
az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--output table
```

Common default groups include:

* Contributors
* Readers
* Project Administrators
* Build Administrators
* Release Administrators

## Step 3: Create a Custom Group

```bash
az devops security group create \
--name "Cloud Developers" \
--description "Developers working on cloud applications" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

## Step 4: Store the Group Descriptor

```bash
GROUP_DESCRIPTOR=$(az devops security group show \
--group "Cloud Developers" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--query descriptor \
--output tsv)

echo "$GROUP_DESCRIPTOR"
```

## Step 5: Get the User Descriptor

```bash
USER_DESCRIPTOR=$(az devops user show \
--user "developer@company.com" \
--organization "$AZDO_ORG" \
--query user.descriptor \
--output tsv)

echo "$USER_DESCRIPTOR"
```

## Step 6: Add the User to the Group

```bash
az devops security group membership add \
--group-id "$GROUP_DESCRIPTOR" \
--member-id "$USER_DESCRIPTOR" \
--organization "$AZDO_ORG"
```

## Step 7: List Group Members

```bash
az devops security group membership list \
--id "$GROUP_DESCRIPTOR" \
--relationship members \
--organization "$AZDO_ORG" \
--output table
```

## Step 8: Remove the User from the Group

```bash
az devops security group membership remove \
--group-id "$GROUP_DESCRIPTOR" \
--member-id "$USER_DESCRIPTOR" \
--organization "$AZDO_ORG" \
--yes
```

## Step 9: Delete the Custom Group

```bash
az devops security group delete \
--id "$GROUP_DESCRIPTOR" \
--organization "$AZDO_ORG" \
--yes
```

## Points to Remember

* Use groups instead of assigning permissions directly to users.
* Group-based access is easier to audit and maintain.
* Users can inherit permissions from several groups.
* A user may receive a permission through direct assignment and group membership.

---

# Module 7: Add Users to Default Project Groups

## Objective

Add users to Contributors, Readers or Project Administrators.

## Step 1: Find the Contributors Group

```bash
CONTRIBUTORS_DESCRIPTOR=$(az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--query "graphGroups[?contains(displayName, 'Contributors')].descriptor | [0]" \
--output tsv)

echo "$CONTRIBUTORS_DESCRIPTOR"
```

## Step 2: Get the User Descriptor

```bash
USER_DESCRIPTOR=$(az devops user show \
--user "developer@company.com" \
--organization "$AZDO_ORG" \
--query user.descriptor \
--output tsv)
```

## Step 3: Add the User to Contributors

```bash
az devops security group membership add \
--group-id "$CONTRIBUTORS_DESCRIPTOR" \
--member-id "$USER_DESCRIPTOR" \
--organization "$AZDO_ORG"
```

## Add a User to Readers

```bash
READERS_DESCRIPTOR=$(az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--query "graphGroups[?contains(displayName, 'Readers')].descriptor | [0]" \
--output tsv
)
```

```bash
az devops security group membership add \
--group-id "$READERS_DESCRIPTOR" \
--member-id "$USER_DESCRIPTOR" \
--organization "$AZDO_ORG"
```

## Add a User to Project Administrators

```bash
ADMIN_DESCRIPTOR=$(az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--query "graphGroups[?contains(displayName, 'Project Administrators')].descriptor | [0]" \
--output tsv
)
```

```bash
az devops security group membership add \
--group-id "$ADMIN_DESCRIPTOR" \
--member-id "$USER_DESCRIPTOR" \
--organization "$AZDO_ORG"
```

## Verify Membership

```bash
az devops security group membership list \
--id "$USER_DESCRIPTOR" \
--relationship memberof \
--organization "$AZDO_ORG" \
--output table
```

## Points to Remember

* Contributors can normally create and modify project content.
* Readers receive read-only project access.
* Project Administrators have broad project-level administrative access.
* Do not assign Project Administrator access to every developer.
* Use the principle of least privilege.

---

# Module 8: Azure DevOps Permissions

## Objective

Understand and inspect Azure DevOps security permissions.

## Permission States

* Allow
* Deny
* Not set
* Inherited allow
* Inherited deny

## Step 1: List Security Namespaces

```bash
az devops security permission namespace list \
--organization "$AZDO_ORG" \
--output table
```

## Step 2: Search for Git Repository Namespace

```bash
az devops security permission namespace list \
--organization "$AZDO_ORG" \
--query "[?contains(name, 'Git')]" \
--output json
```

## Step 3: Search for Project Namespace

```bash
az devops security permission namespace list \
--organization "$AZDO_ORG" \
--query "[?contains(name, 'Project')]" \
--output table
```

## Step 4: Show Permission Details

```bash
az devops security permission show \
--id "<SECURITY-NAMESPACE-ID>" \
--subject "$GROUP_DESCRIPTOR" \
--token "<SECURITY-TOKEN>" \
--organization "$AZDO_ORG"
```

## Permission Update Pattern

```bash
az devops security permission update \
--id "<SECURITY-NAMESPACE-ID>" \
--subject "$GROUP_DESCRIPTOR" \
--token "<SECURITY-TOKEN>" \
--allow-bit <PERMISSION-BIT> \
--organization "$AZDO_ORG"
```

To explicitly deny a permission:

```bash
az devops security permission update \
--id "<SECURITY-NAMESPACE-ID>" \
--subject "$GROUP_DESCRIPTOR" \
--token "<SECURITY-TOKEN>" \
--deny-bit <PERMISSION-BIT> \
--organization "$AZDO_ORG"
```

## Important Warning

Permission namespace IDs, security tokens and permission bits differ between:

* Projects
* Git repositories
* Branches
* Pipelines
* Agent pools
* Service connections
* Variable groups

Do not copy a permission bit from one namespace and apply it to another without verification.

## Recommended UI Lab

1. Open the project.
2. Go to **Project settings**.
3. Select **Permissions**.
4. Open the custom group.
5. Configure selected permissions.
6. Sign in as a test user.
7. verify the effective access.

## Points to Remember

* Explicit Deny generally overrides Allow.
* Avoid using Deny unless there is a clear security requirement.
* Prefer leaving permissions as **Not set** when access is not required.
* Always test permissions using a non-administrator test account.

---

# Module 9: Create Microsoft Entra ID Users

## Objective

Create or invite Microsoft Entra ID users before granting Azure DevOps access.

## Step 1: Display Tenant Information

```bash
az account show \
--query "{TenantID:tenantId,Subscription:name,User:user.name}" \
--output table
```

## Step 2: Create an Internal Entra ID User

```bash
az ad user create \
--display-name "DevOps Developer" \
--user-principal-name "devopsdeveloper@yourtenant.onmicrosoft.com" \
--password "Use-A-Secure-Temporary-Password" \
--force-change-password-next-sign-in true
```

## Step 3: Verify the User

```bash
az ad user show \
--id "devopsdeveloper@yourtenant.onmicrosoft.com" \
--output table
```

## Step 4: List Users

```bash
az ad user list \
--query "[].{Name:displayName,UPN:userPrincipalName,ID:id}" \
--output table
```

## External Guest User

Guest users are commonly invited through:

```text
Azure portal
→ Microsoft Entra ID
→ Users
→ New user
→ Invite external user
```

After the guest accepts the invitation, add the same identity to Azure DevOps.

## Step 5: Add the Entra User to Azure DevOps

```bash
az devops user add \
--email-id "devopsdeveloper@yourtenant.onmicrosoft.com" \
--license-type express \
--send-email-invite true \
--organization "$AZDO_ORG"
```

## Points to Remember

* Creating an Entra user and adding an Azure DevOps user are two separate operations.
* The email address or user principal name should identify the same person.
* Guest users must normally accept their Microsoft Entra invitation.
* Use temporary passwords securely and never include real passwords in scripts or repositories.

---

# Module 10: Create and Manage Microsoft Entra Security Groups

## Objective

Create an Entra group and manage user membership.

## Step 1: Create an Entra Security Group

```bash
az ad group create \
--display-name "AzureDevOpsDevelopers" \
--mail-nickname "AzureDevOpsDevelopers"
```

## Step 2: Get the Group Object ID

```bash
ENTRA_GROUP_ID=$(az ad group show \
--group "AzureDevOpsDevelopers" \
--query id \
--output tsv)

echo "$ENTRA_GROUP_ID"
```

## Step 3: Get the User Object ID

```bash
ENTRA_USER_ID=$(az ad user show \
--id "devopsdeveloper@yourtenant.onmicrosoft.com" \
--query id \
--output tsv)

echo "$ENTRA_USER_ID"
```

## Step 4: Add the User to the Group

```bash
az ad group member add \
--group "$ENTRA_GROUP_ID" \
--member-id "$ENTRA_USER_ID"
```

## Step 5: List Group Members

```bash
az ad group member list \
--group "$ENTRA_GROUP_ID" \
--query "[].{Name:displayName,UPN:userPrincipalName,ID:id}" \
--output table
```

## Step 6: Verify Membership

```bash
az ad group member check \
--group "$ENTRA_GROUP_ID" \
--member-id "$ENTRA_USER_ID"
```

## Step 7: Remove the User

```bash
az ad group member remove \
--group "$ENTRA_GROUP_ID" \
--member-id "$ENTRA_USER_ID"
```

## Points to Remember

* Entra group membership does not automatically provide Azure DevOps permissions unless the group is added to Azure DevOps.
* Group-based management is recommended for larger organizations.
* Membership changes may take time to appear across connected services.
* Use separate groups for developers, testers, administrators and auditors.

---

# Module 11: Connect Azure DevOps to Microsoft Entra ID

## Objective

Connect the Azure DevOps organization to the appropriate Microsoft Entra tenant.

## Prerequisites

* You must be an Azure DevOps organization owner.
* You need sufficient access to the target Microsoft Entra tenant.
* The organization owner should exist in the target directory.
* A recovery administrator should be available.
* Existing users should be reviewed before switching directories.

## Practical Steps

1. Open the Azure DevOps organization.
2. Select **Organization settings**.
3. Select **Microsoft Entra ID**.
4. Select **Connect directory**.
5. Select the target directory.
6. Complete the connection.
7. Sign out.
8. Sign in using the identity from the connected tenant.
9. Verify projects, repositories and organization settings.

Microsoft documents the connection through **Organization settings → Microsoft Entra ID → Connect directory**.

## Display Current Azure Tenant

```bash
az account show \
--query "{TenantID:tenantId,User:user.name}" \
--output table
```

## List Accessible Tenants

```bash
az account tenant list --output table
```

## Sign in to the Correct Tenant

```bash
az login --tenant "<TARGET-TENANT-ID>"
```

## Important Safety Checklist

Before changing directories:

* Confirm the organization owner exists in the target tenant.
* Add a second organization administrator.
* Export the user list.
* Record current access levels.
* Record important security groups.
* Verify the target tenant ID.
* Do not perform the switch using an unknown guest identity.

## Points to Remember

* One Azure DevOps organization can be connected to one Microsoft Entra tenant at a time.
* Changing the directory does not automatically move an Azure subscription.
* Existing projects and repositories remain in the organization when the directory connection changes.
* Incorrect identity mapping can cause users to lose access.

---

# Module 12: Add an Existing Entra Group to Azure DevOps

## Objective

Import an existing Microsoft Entra group into Azure DevOps.

## Prerequisite

The Azure DevOps organization must be connected to the Microsoft Entra tenant containing the group.

## Step 1: Confirm the Entra Group

```bash
az ad group show \
--group "AzureDevOpsDevelopers" \
--output table
```

## Step 2: Add the Entra Group to Azure DevOps

Where the Entra group has a valid mail-enabled identifier:

```bash
az devops security group create \
--email-id "AzureDevOpsDevelopers@company.com" \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG"
```

Microsoft’s CLI supports adding an existing Entra group to Azure DevOps by using its email identifier.

## Alternative Portal Method

1. Open the Azure DevOps project.
2. Go to **Project settings**.
3. Select **Permissions**.
4. Select **New group** or add members to an existing group.
5. Search for the Microsoft Entra group.
6. Add the group.
7. Assign the required project permissions.

## Step 3: Verify the Imported Group

```bash
az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--output table
```

## Step 4: Add the Entra Group to Contributors

Get the imported group descriptor:

```bash
ENTRA_AZDO_DESCRIPTOR=$(az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--query "graphGroups[?displayName=='AzureDevOpsDevelopers'].descriptor | [0]" \
--output tsv)
```

Get the Contributors descriptor:

```bash
CONTRIBUTORS_DESCRIPTOR=$(az devops security group list \
--project "$AZDO_PROJECT" \
--organization "$AZDO_ORG" \
--query "graphGroups[?contains(displayName, 'Contributors')].descriptor | [0]" \
--output tsv)
```

Add the Entra group to Contributors:

```bash
az devops security group membership add \
--group-id "$CONTRIBUTORS_DESCRIPTOR" \
--member-id "$ENTRA_AZDO_DESCRIPTOR" \
--organization "$AZDO_ORG"
```

## Points to Remember

* The Entra group must come from the tenant connected to Azure DevOps.
* Nested groups can make effective permissions difficult to troubleshoot.
* Use clearly named groups such as:

  * `AZDO-Developers`
  * `AZDO-Testers`
  * `AZDO-Project-Admins`
  * `AZDO-Readers`

---

# Module 13: Different Azure DevOps and Azure Portal Accounts

## Scenario

Azure DevOps account:

```text
developer@gmail.com
```

Azure Portal account:

```text
admin@company.com
```

The first account can access Azure DevOps, while the second account can access the Azure subscription.

## Problem

When creating an Azure Resource Manager service connection, the Azure DevOps user might not be able to view or authorize the subscription.

## Step 1: Check the Current Azure Identity

```bash
az account show \
--query "{User:user.name,Tenant:tenantId,Subscription:name}" \
--output table
```

## Step 2: List Subscriptions for the Current Identity

```bash
az account list --output table
```

## Step 3: Sign Out and Use the Correct Azure Account

```bash
az logout
```

```bash
az login --tenant "<AZURE-SUBSCRIPTION-TENANT-ID>"
```

## Step 4: Check Azure RBAC

```bash
az role assignment list \
--assignee "admin@company.com" \
--all \
--output table
```

## Step 5: Assign a Suitable Azure Role

Example for training:

```bash
az role assignment create \
--assignee "admin@company.com" \
--role "Contributor" \
--scope "/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>"
```

## Preferred Enterprise Resolution

Use a service connection with workload identity federation instead of depending on a trainer’s or developer’s personal account.

Microsoft recommends modern Entra authentication and workload identity federation for many automation scenarios instead of long-lived personal access tokens or personal credentials.

## Points to Remember

* Azure DevOps access is controlled by Azure DevOps permissions.
* Azure resource access is controlled by Azure RBAC.
* Access to Azure DevOps does not provide access to an Azure subscription.
* Access to an Azure subscription does not provide access to an Azure DevOps organization.
* The accounts may be different, but the correct permissions must exist in both systems.

---

# Module 14: Azure DevOps REST API Authentication

## Objective

Use the Azure DevOps REST API to list projects.

## Recommended Authentication

For modern applications, prefer Microsoft Entra ID authentication. Personal access tokens should be limited to controlled labs or legacy scenarios.

## Option 1: Use an Entra Access Token

Get an access token for Azure DevOps:

```bash
TOKEN=$(az account get-access-token \
--resource "499b84ac-1321-427f-aa17-267ca6975798" \
--query accessToken \
--output tsv)
```

Call the Projects API:

```bash
curl \
-H "Authorization: Bearer $TOKEN" \
-H "Content-Type: application/json" \
"https://dev.azure.com/myorganization/_apis/projects?api-version=7.1"
```

## Pretty-Print with jq

```bash
curl -s \
-H "Authorization: Bearer $TOKEN" \
"https://dev.azure.com/myorganization/_apis/projects?api-version=7.1" |
jq '.value[] | {name, id, state, visibility}'
```

## List Teams Using REST API

```bash
curl -s \
-H "Authorization: Bearer $TOKEN" \
"https://dev.azure.com/myorganization/_apis/projects/CloudProject/teams?api-version=7.1" |
jq '.value[] | {name, id, description}'
```

## Python Example

```python
import os
import sys
import requests

organization = "myorganization"
token = os.getenv("AZDO_ENTRA_TOKEN")

if not token:
    print("AZDO_ENTRA_TOKEN is not configured.", file=sys.stderr)
    sys.exit(1)

url = (
    f"https://dev.azure.com/{organization}"
    "/_apis/projects?api-version=7.1"
)

headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json",
}

try:
    response = requests.get(
        url,
        headers=headers,
        timeout=30,
    )
    response.raise_for_status()
except requests.RequestException as exc:
    print(f"Request failed: {exc}", file=sys.stderr)
    sys.exit(1)

data = response.json()

for project in data.get("value", []):
    print(
        project.get("name"),
        project.get("state"),
        project.get("visibility"),
    )
```

Run it:

```bash
export AZDO_ENTRA_TOKEN="$TOKEN"
python3 list_projects.py
```

## Points to Remember

* Never hardcode access tokens in source code.
* Keep tokens in environment variables or secret-management systems.
* Entra access tokens and Azure DevOps personal access tokens are not interchangeable.
* Use the minimum required permissions.
* Tokens must not be committed to Git.

---

# Module 15: Complete Organization Setup Script

## Objective

Automate project, team, user and group creation.

## Bash Script

```bash
#!/usr/bin/env bash

set -euo pipefail

ORGANIZATION="https://dev.azure.com/myorganization"
PROJECT="CloudProject"
TEAM="Cloud Platform Team"
GROUP="Cloud Developers"
USER_EMAIL="developer@company.com"

echo "Checking Azure CLI..."
az version >/dev/null

echo "Installing Azure DevOps extension..."
if ! az extension show --name azure-devops >/dev/null 2>&1; then
    az extension add --name azure-devops
fi

echo "Configuring Azure DevOps defaults..."
az devops configure --defaults \
    organization="$ORGANIZATION" \
    project="$PROJECT"

echo "Checking whether project exists..."
PROJECT_ID=$(
    az devops project show \
        --project "$PROJECT" \
        --organization "$ORGANIZATION" \
        --query id \
        --output tsv 2>/dev/null || true
)

if [[ -z "$PROJECT_ID" ]]; then
    echo "Creating project: $PROJECT"

    az devops project create \
        --name "$PROJECT" \
        --description "Cloud and Azure DevOps practical project" \
        --visibility private \
        --source-control git \
        --process agile \
        --organization "$ORGANIZATION"

    PROJECT_ID=$(
        az devops project show \
            --project "$PROJECT" \
            --organization "$ORGANIZATION" \
            --query id \
            --output tsv
    )
else
    echo "Project already exists."
fi

echo "Project ID: $PROJECT_ID"

echo "Checking whether team exists..."
TEAM_ID=$(
    az devops team show \
        --team "$TEAM" \
        --project "$PROJECT" \
        --organization "$ORGANIZATION" \
        --query id \
        --output tsv 2>/dev/null || true
)

if [[ -z "$TEAM_ID" ]]; then
    echo "Creating team: $TEAM"

    az devops team create \
        --name "$TEAM" \
        --description "Cloud platform engineering team" \
        --project "$PROJECT" \
        --organization "$ORGANIZATION"
else
    echo "Team already exists."
fi

echo "Checking whether user exists..."
USER_DESCRIPTOR=$(
    az devops user show \
        --user "$USER_EMAIL" \
        --organization "$ORGANIZATION" \
        --query user.descriptor \
        --output tsv 2>/dev/null || true
)

if [[ -z "$USER_DESCRIPTOR" ]]; then
    echo "Adding user: $USER_EMAIL"

    az devops user add \
        --email-id "$USER_EMAIL" \
        --license-type express \
        --send-email-invite true \
        --organization "$ORGANIZATION"

    USER_DESCRIPTOR=$(
        az devops user show \
            --user "$USER_EMAIL" \
            --organization "$ORGANIZATION" \
            --query user.descriptor \
            --output tsv
    )
else
    echo "User already exists."
fi

echo "Checking whether group exists..."
GROUP_DESCRIPTOR=$(
    az devops security group list \
        --project "$PROJECT" \
        --organization "$ORGANIZATION" \
        --query "graphGroups[?displayName=='$GROUP'].descriptor | [0]" \
        --output tsv
)

if [[ -z "$GROUP_DESCRIPTOR" ]]; then
    echo "Creating group: $GROUP"

    GROUP_DESCRIPTOR=$(
        az devops security group create \
            --name "$GROUP" \
            --description "Cloud application developers" \
            --project "$PROJECT" \
            --organization "$ORGANIZATION" \
            --query descriptor \
            --output tsv
    )
else
    echo "Group already exists."
fi

echo "Adding user to group..."
az devops security group membership add \
    --group-id "$GROUP_DESCRIPTOR" \
    --member-id "$USER_DESCRIPTOR" \
    --organization "$ORGANIZATION"

echo "Listing project teams..."
az devops team list \
    --project "$PROJECT" \
    --organization "$ORGANIZATION" \
    --output table

echo "Listing organization users..."
az devops user list \
    --organization "$ORGANIZATION" \
    --output table

echo "Listing group members..."
az devops security group membership list \
    --id "$GROUP_DESCRIPTOR" \
    --relationship members \
    --organization "$ORGANIZATION" \
    --output table

echo "Azure DevOps organization setup completed."
```

## Run the Script

```bash
chmod +x setup-azure-devops.sh
```

```bash
./setup-azure-devops.sh
```

## Points to Remember

* Replace all example organization names and email addresses.
* Test scripts in a non-production organization.
* Use `set -euo pipefail` to identify script failures.
* Add existence checks to make scripts reusable.
* Do not place credentials or tokens directly in scripts.

---

# Module 16: PowerShell Automation Script

```powershell
$ErrorActionPreference = "Stop"

$Organization = "https://dev.azure.com/myorganization"
$Project = "CloudProject"
$Team = "Cloud Platform Team"
$Group = "Cloud Developers"
$UserEmail = "developer@company.com"

Write-Host "Checking Azure CLI..."

az version | Out-Null

Write-Host "Checking Azure DevOps extension..."

$Extension = az extension show `
    --name azure-devops `
    --query name `
    --output tsv 2>$null

if (-not $Extension) {
    az extension add --name azure-devops
}

Write-Host "Configuring defaults..."

az devops configure --defaults `
    organization=$Organization `
    project=$Project

Write-Host "Checking project..."

$ProjectId = az devops project show `
    --project $Project `
    --organization $Organization `
    --query id `
    --output tsv 2>$null

if (-not $ProjectId) {
    Write-Host "Creating project: $Project"

    az devops project create `
        --name $Project `
        --description "Cloud and Azure DevOps training project" `
        --visibility private `
        --source-control git `
        --process agile `
        --organization $Organization

    $ProjectId = az devops project show `
        --project $Project `
        --organization $Organization `
        --query id `
        --output tsv
}

Write-Host "Project ID: $ProjectId"

Write-Host "Checking team..."

$TeamId = az devops team show `
    --team $Team `
    --project $Project `
    --organization $Organization `
    --query id `
    --output tsv 2>$null

if (-not $TeamId) {
    az devops team create `
        --name $Team `
        --description "Cloud platform engineering team" `
        --project $Project `
        --organization $Organization
}

Write-Host "Checking user..."

$UserDescriptor = az devops user show `
    --user $UserEmail `
    --organization $Organization `
    --query user.descriptor `
    --output tsv 2>$null

if (-not $UserDescriptor) {
    az devops user add `
        --email-id $UserEmail `
        --license-type express `
        --send-email-invite true `
        --organization $Organization

    $UserDescriptor = az devops user show `
        --user $UserEmail `
        --organization $Organization `
        --query user.descriptor `
        --output tsv
}

Write-Host "Checking security group..."

$GroupDescriptor = az devops security group list `
    --project $Project `
    --organization $Organization `
    --query "graphGroups[?displayName=='$Group'].descriptor | [0]" `
    --output tsv

if (-not $GroupDescriptor) {
    $GroupDescriptor = az devops security group create `
        --name $Group `
        --description "Cloud application developers" `
        --project $Project `
        --organization $Organization `
        --query descriptor `
        --output tsv
}

Write-Host "Adding user to security group..."

az devops security group membership add `
    --group-id $GroupDescriptor `
    --member-id $UserDescriptor `
    --organization $Organization

Write-Host "Setup completed successfully."

az devops user list `
    --organization $Organization `
    --output table
```

---

# Recommended Lab Sequence

| Lab | Practical task                                           |
| --: | -------------------------------------------------------- |
|   1 | Install and configure the Azure DevOps CLI               |
|   2 | Create an Azure DevOps organization through the portal   |
|   3 | Create Agile and Scrum projects                          |
|   4 | Create Development, DevOps and Testing teams             |
|   5 | Add Basic and Stakeholder users                          |
|   6 | Create custom Azure DevOps security groups               |
|   7 | Add users to Contributors and Readers                    |
|   8 | Inspect security namespaces and permissions              |
|   9 | Create Microsoft Entra users                             |
|  10 | Create Microsoft Entra groups                            |
|  11 | Connect Azure DevOps to Microsoft Entra ID               |
|  12 | Import an Entra group into Azure DevOps                  |
|  13 | Troubleshoot different Azure and Azure DevOps identities |
|  14 | Access Azure DevOps through its REST API                 |
|  15 | Run the complete Bash automation script                  |
|  16 | Run the complete PowerShell automation script            |

# Final Points to Remember

1. An Azure DevOps organization and an Azure subscription are separate resources.
2. Azure DevOps access levels and permissions are different.
3. Microsoft Entra ID manages identities.
4. Azure DevOps security groups manage Azure DevOps permissions.
5. Azure RBAC manages Azure resource permissions.
6. Prefer group-based permission assignments.
7. Avoid granting administrator access unnecessarily.
8. Avoid explicit Deny unless absolutely required.
9. Keep at least two trusted organization administrators.
10. Use workload identity federation for production service connections.
11. Never store passwords, PATs or access tokens in Git.
12. Test directory changes and identity mappings carefully.
