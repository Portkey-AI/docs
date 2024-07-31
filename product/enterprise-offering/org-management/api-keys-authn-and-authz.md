---
description: >-
  Discover how Admin and Workspace API Keys are used to manage access and
  operations in Portkey.
---

# API Keys (AuthN and AuthZ)

## API Keys

Portkey uses two types of API keys to manage access to resources and operations: **Admin API Keys** and **Workspace API Keys**. These keys play crucial roles in authenticating and authorizing various operations within your [organization](organizations.md) and [workspaces](../../../portkey-endpoints/admin/workspaces/).

### Admin API Keys

Admin API Keys operate at the organization level and provide broad access across all workspaces within an organization.

Key features of Admin API Keys:

* Created and managed by organization owners and admins
* Provide access to organization-wide operations
* Can perform actions across all workspaces in the organization
* Used for administrative tasks and integrations that require broad access
* When making updates to entities, can specify a workspace\_id to target specific workspaces

Admin API Keys should be carefully managed and their use should be limited to necessary administrative operations due to their broad scope of access.



### Workspace API Keys

Workspace API Keys are scoped to a specific workspace and are used for operations within that workspace only.

Key features of Workspace API Keys:

* Two types: Service Account and User
  * Service Account: Used for automated processes and integrations
  * User: Associated with individual user accounts for personal access
* Scoped to a single workspace by default
* Can only execute actions within the workspace they belong to
* Used for most day-to-day operations and integrations within a workspace
* Completion APIs are always scoped by workspace and can only be accessed using Workspace API Keys
* Can be created and managed by workspace managers

Workspace API Keys provide a more granular level of access control, allowing you to manage permissions and resource usage at the project or team level.

Both types of API keys play important roles in Portkey's security model, enabling secure and efficient access to resources while maintaining proper separation of concerns between organization-wide administration and workspace-specific operations.

### Related Topics

* [Organizations](organizations.md)
* [Workspaces](../../../portkey-endpoints/admin/workspaces/)
* [User Roles and Permissions](user-roles-and-permissions.md)
