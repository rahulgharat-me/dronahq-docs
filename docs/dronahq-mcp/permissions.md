---
sidebar_position: 5
title: "Permissions"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Permissions

Your AI client acts as you. Every tool call is checked against your current role and your Studio permissions, using the same App Permissions and Connector Permissions you manage in Studio. If your access changes, the client's access changes on its next request.

## Workspace roles

| Role | Through MCP |
| --- | --- |
| **Owner** | Full access to every app and connector. Can do everything below |
| **Admin** | Full access to every app and connector. Manages users, groups, catalogue groups and connector access, and MCP settings. Creating a catalogue needs an owner or super admin. Limited admins manage only the groups and catalogues assigned to them |
| **Creator** | Creates apps and automations. Works with the apps and connectors they've been granted |
| **End user** | Can't connect |

A creator is never treated as an admin.

## App permissions

| Level | Code | Through MCP you can |
| --- | --- | --- |
| Owner | `1000` | Everything, including viewing and changing who has access to the app |
| Publisher | `0100` | Publish the app and change its public link access |
| Editor | `0010` | Write, delete and save files, connect connectors, check in versions |
| Preview Only | `0001` | Open the app, read its files and get a preview link |
| None | `0000` | Nothing. The app isn't listed |

Each level includes the ones below it. Where you have no explicit permission on an app, your workspace's default permission applies.

Owners and admins always have Owner access to every app.

### Creating apps

Owners and admins can always create apps. Creators can create apps when their effective app permission is at least **Editor**. Creators can't add apps to the **Master Template** catalogue.

## Connector permissions

| Level | Code | Through MCP you can |
| --- | --- | --- |
| Owner | `1000` | Everything |
| Editor | `0010` | Create saved operations, and create or update the connector's Data Agent |
| Use Only | `0001` | Inspect the schema, run read and write queries, run saved operations, connect the connector to an app |
| None | `0000` | Nothing. The connector isn't listed |

Owners and admins always have Owner access to every connector.

**Setting up new connectors** is limited to owners and admins, whatever their connector permissions. See [Set up a new connector](./connecting-data.md#set-up-a-new-connector).

### Choose which creators can use a connector

Owners and admins can open the **Connector access** tab in the Connect DronaHQ MCP drawer. Pick a connector and click **Edit permission** to choose which creators and groups can use it through MCP. This opens the same connector permission panel as Studio.

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-connector-access.png" alt="The Connector access tab" />
  <figcaption align="center"><i>The Connector access tab</i></figcaption>
</figure>

## User and workspace management

| Action | Who |
| --- | --- |
| List groups, check emails, create groups, invite users | Owners and admins |
| Add or replace a catalogue's groups | Owners and admins (limited admins: only their catalogues) |
| Create a catalogue | Owners and super admins |
| Deactivate or reactivate a user | Owners and admins. See [User accounts](./publishing-and-users.md#user-accounts) for the rules |
| Automations | Owners, admins and creators |

## When something is denied

A denied call returns an error that names the tool, the permission it needs and the permission you have:

```
Permission denied: vibe_write_file requires Editor (0010) on app 123; you have Preview/Use Only (0001)
```

Other examples:

```
Permission denied: DronaHQ MCP requires a channel owner, admin, or creator account
Permission denied: user account is deactivated
Permission denied: vibe_request_connector_setup — only workspace owners and admins can add connectors. Ask an admin to create the connector and grant it to you (or your group) in Studio permissions.
```

The agent shows you the message. To fix it, ask an owner or admin to change your permission in Studio.
