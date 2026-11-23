---
sidebar_position: 1
title: "Overview"
pagination_prev: null
---
import VideoEmbed from "@site/src/components/VideoEmbed";
import Thumbnail from '@site/src/components/Thumbnail';

# DronaHQ MCP

DronaHQ MCP connects your DronaHQ workspace to an AI client such as Claude Code, Claude, Cursor, VS Code or Windsurf, over the Model Context Protocol (MCP). Your AI client gets more than a place to write frontend code. It gets a production backend it can build and ship on: authentication, role-based access control (RBAC), a secrets vault, database and API connectors, environments, hosting and audit logging.

There's no server to stand up, no CI/CD pipeline to configure, no SSL certificate to manage and no middleware to write. Building through MCP uses your own AI client, so it doesn't use DronaHQ AI credits.

<figure>
  <VideoEmbed host='youtube' videoId='21eyFKVqlPE' />
</figure>

## Why use DronaHQ MCP

A coding agent can generate a working admin panel or internal tool in minutes. Shipping it is where the time goes: deciding where it runs, who can access it, where the database credentials live, how it reaches services behind a VPN, what a rollback looks like and who signs off on the release. Multiply that by every internal tool your team needs and it adds up.

<figure>
  <Thumbnail src="/img/dronahq-mcp/thumbnail.png" alt="DronaHQ MCP overview" />
</figure>

### What you get

* **RBAC built into the app:** permissions are part of the app from the start, not added later
* **A secrets vault:** credentials never end up in your repository, your prompts or your AI client
* **Secure data access:** databases, REST and GraphQL APIs, internal services and SSH tunnels
* **No backend to write:** for CRUD apps, admin panels and internal tools there's nothing to stand up
* **No infrastructure to manage:** no server, deployment pipeline, certificate or DevOps ticket
* **SSO, sign-in and granular permissions:** available out of the box
* **Automations:** scheduled and webhook-triggered workflows, built and tested from the same chat
* **Version history and audit logs:** every change is attributed to a person

### What you can do from your AI client

| Area | Examples |
| --- | --- |
| Apps | Create a Vibe app, write and save its files, preview it, check in a version, publish it |
| Data | Set up a new connector, use an existing one, inspect a schema, run queries, create saved queries and HTTP operations, set up Data Agents |
| Access | Choose who can open an app, organise apps into catalogues, create groups, invite people, deactivate and reactivate accounts |
| Automations | Create an automation, set its trigger, add and test tasks, publish and turn it on |

See the [tool reference](./tool-reference.md) for every tool.

### When it's the right fit

* **A good fit:** internal apps, admin panels, dashboards, CRUD apps, field-force apps, workflows and automations for teams that need something live quickly.
* **Bring your own backend instead:** workloads with heavy proprietary logic or a public-facing API surface.
* **Deployment:** cloud, self-hosted or in your VPC. RBAC and audit logging are on by default, and it fits into your existing review process rather than replacing it.

## Who can use it

DronaHQ MCP is for the people who build in your workspace:

| Role | Can connect | Notes |
| --- | --- | --- |
| Workspace owner | Yes | Full access to every app and connector |
| Admin | Yes | Full access to every app and connector. Manages users, groups, catalogues, connector access and MCP settings |
| Creator | Yes | Works with the apps and connectors they've been given access to |
| End user | No | End users use published apps. They can't connect an AI client. Ask an admin to make you a creator |

Your AI client always acts as you, with your role. It can never do more than you can do in Studio. See [Permissions](./permissions.md).

## Next steps

1. [Connect your AI client](./getting-started.md)
2. Learn how [sign-in and access tokens](./sign-in-and-access-tokens.md) work
3. [Build your first app](./building-apps.md)
