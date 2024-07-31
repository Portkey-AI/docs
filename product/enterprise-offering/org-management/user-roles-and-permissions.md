---
description: >-
  Learn about the different user roles and their associated permissions within
  Organizations and Workspaces.
---

# User Roles & Permissions

## User Roles and Permissions

Portkey implements a hierarchical role-based access control system to manage permissions at both the [organization](organizations.md) and [workspace](../../../portkey-endpoints/admin/workspaces/) levels. This system ensures that users have appropriate access to resources based on their responsibilities.

### Organization Level

At the organization level, there are two primary roles:

1. **Owner**
   * Has full control over the organization
   * Can manage all aspects of the organization, including creating and deleting workspaces
   * Can assign and revoke org admin roles
2. **Org Admin**
   * Has administrative access to all workspaces within the organization
   * Can create new workspaces
   * Can manage admin API keys
   * Has the ability to invite users to the organization

### Workspace Level

Within each workspace, there are two roles:

1. **Manager**
   * Has administrative control within the workspace
   * Can add and remove team members
   * Can create and manage workspace API keys
   * Has access to all workspace features and data
2. **Member**
   * Has standard access to workspace resources
   * Can use workspace features as permitted by the manager
   * Cannot add or remove team members or manage API keys

### Permissions Matrix

| Action                       | Owner | Org Admin | Workspace Manager | Workspace Member |
| ---------------------------- | ----- | --------- | ----------------- | ---------------- |
| Manage Organization          | ✅     | ✅         | ❌                 | ❌                |
| Create Workspaces            | ✅     | ✅         | ❌                 | ❌                |
| Manage Admin API Keys        | ✅     | ✅         | ❌                 | ❌                |
| Invite Users to Organization | ✅     | ✅         | ❌                 | ❌                |
| Manage Workspace             | ✅     | ✅         | ✅                 | ❌                |
| Add/Remove Workspace Members | ✅     | ✅         | ✅                 | ❌                |
| Create Workspace API Keys    | ✅     | ✅         | ✅                 | ❌                |
| Access Workspace Features    | ✅     | ✅         | ✅                 | ✅                |

This permissions structure ensures that access to sensitive operations and data is properly controlled, while still allowing for efficient management of resources within your organization and workspaces.

### Related Topics

* Organizations
* Workspaces
* API Keys
