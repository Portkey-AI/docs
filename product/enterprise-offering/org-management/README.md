---
description: >-
  A high-level introduction to Portkey's organization management structure and
  key concepts.
---

# Org Management

Portkey's organization management structure provides a hierarchical system for managing teams, resources, and access within your AI development environment. This structure is designed to offer flexibility and security for enterprises of various sizes.

The account hierarchy in Portkey is organized as follows:

<figure><img src="../../../.gitbook/assets/Pasted image 20240727063717.png" alt=""><figcaption></figcaption></figure>

This hierarchy allows for efficient management of resources and access control across your organization. At the top level, you have your Account, which can contain one or more Organizations. Each Organization can have multiple Workspaces, providing a way to separate teams, projects, or departments within your company.

{% hint style="info" %}
**Enterprise Feature**\
Workspaces is currently a feature only enabled on the Enterprise Plans. \
\
If you're looking to add this feature to your organisation, please reach out to us on our [Discord Community](https://portkey.ai/community) or via email on [support@portkey.ai](mailto:support@portkey.ai)
{% endhint %}

Organizations contain User Invites & Users, Admin API Keys, and Workspaces. Workspaces, in turn, have their own Team structure (with Managers and Members), Workspace API Keys, and various features like Virtual Keys, Configs, Prompts, and more.

This structure enables you to:

* Maintain overall control at the Organization level
* Delegate responsibilities and access at the Workspace level
* Ensure data separation and project scoping
* Manage teams efficiently across different projects or departments

{% content-ref url="organizations.md" %}
[organizations.md](organizations.md)
{% endcontent-ref %}

{% content-ref url="../../../portkey-endpoints/admin/workspaces/" %}
[workspaces](../../../portkey-endpoints/admin/workspaces/)
{% endcontent-ref %}

{% content-ref url="user-roles-and-permissions.md" %}
[user-roles-and-permissions.md](user-roles-and-permissions.md)
{% endcontent-ref %}

{% content-ref url="api-keys-authn-and-authz.md" %}
[api-keys-authn-and-authz.md](api-keys-authn-and-authz.md)
{% endcontent-ref %}
