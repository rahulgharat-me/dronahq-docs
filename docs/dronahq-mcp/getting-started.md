---
sidebar_position: 1
title: "Overview"
pagination_prev: null
---
import VideoEmbed from "@site/src/components/VideoEmbed";
import Thumbnail from '@site/src/components/Thumbnail';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# DronaHQ MCP

DronaHQ MCP connects your DronaHQ workspace to an AI client such as Claude Code, Claude, Cursor, VS Code or Windsurf, over the Model Context Protocol (MCP). Your AI client gets more than a place to write frontend code. It gets a production backend it can build and ship on: authentication, role-based access control (RBAC), a secrets vault, database and API connectors, environments, hosting and audit logging.

There's no server to stand up, no CI/CD pipeline to configure, no SSL certificate to manage and no middleware to write. Building through MCP uses your own AI client, so it doesn't use DronaHQ AI credits. Writing and saving app files does count as Vibe tasks toward your plan's usage. See [Usage](./building-apps.md#usage).

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
| Admin | Yes | Full access to every app and connector. Manages users, groups, catalogue groups, connector access and MCP settings. Creating a catalogue needs an owner or super admin |
| Creator | Yes | Works with the apps and connectors they've been given access to |
| End user | No | End users use published apps. They can't connect an AI client. Ask an admin to make you a creator |

Your AI client always acts as you, with your role. It can never do more than you can do in Studio. See [Permissions](./permissions.md).

## Getting started

Connecting takes about two minutes. You add your workspace's MCP server URL to your AI client, then sign in to DronaHQ when the client asks. You don't need to copy a token.


### Step 1: Open the Connect DronaHQ MCP drawer

You can open it from several places in Studio:

* **Apps home:** the **Build from your IDE via MCP** card → **Connect DronaHQ MCP**
* **Apps tile view:** **Create app using MCP**
* **Inside a Vibe app:** the editor menu (**Open in Cursor/Windsurf…**) → **Connect via MCP**
* **AI chat in the builder:** **Connect via MCP** → **Connect DronaHQ MCP**

<figure>
  <Thumbnail src="/img/dronahq-mcp/apps-home-mcp-card.png" alt="The DronaHQ MCP card on the Apps home" />
  <figcaption align="center"><i>The DronaHQ MCP card on the Apps home</i></figcaption>
</figure>

#### First time in DronaHQ?

New accounts, and people joining a workspace for the first time, start on a **Connect your AI client to DronaHQ** screen. It walks through the same three steps as this page: copy your MCP URL, add it to your client, and sign in when the client asks. When your client connects, the screen shows **Connected** and a **Start building** button. Use **Build in Studio instead** or **Skip for now** to go straight to Studio.

<figure>
  <Thumbnail src="/img/dronahq-mcp/welcome-connect-ai-client.png" alt="The first-run Connect your AI client screen" />
  <figcaption align="center"><i>The first-run Connect your AI client screen</i></figcaption>
</figure>

### Step 2: Create or pick an MCP server

Each workspace needs at least one MCP server. It's the address your AI clients connect to.

* If **Your MCP servers** already lists a server, select it.
* If not, fill in **Create MCP server** and click **Create server**. The defaults (**DronaHQ MCP**, "Tools to create and manage apps via DronaHQ MCP") are fine.

You can skip this step only if you connect with the workspace-wide URL and sign in (see [Use one URL for every workspace](#use-one-url-for-every-workspace)). If your workspace has no server yet, DronaHQ creates one for you when you allow access. The client tabs in Step 3, and access tokens, need a server, so don't skip this step if you plan to use them.

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-servers.png" alt="Your MCP servers in the Connect DronaHQ MCP drawer" />
  <figcaption align="center"><i>Your MCP servers in the Connect DronaHQ MCP drawer</i></figcaption>
</figure>

### Step 3: Add the server to your AI client

Pick your client's tab in the drawer and follow the steps. The drawer fills in the URL of the server you selected in Step 2. If you skipped Step 2, use the [workspace-wide URL](#use-one-url-for-every-workspace) instead and sign in.

:::tip Sign-in or access token?
For Cursor, Claude Code, VS Code and Windsurf, the drawer includes an **access token** in the setup by default, so the client connects without a sign-in step. Claude and other clients always sign in with your DronaHQ account. If your admin has turned access tokens off, every client signs in. See [Sign-in and access tokens](./sign-in-and-access-tokens.md) for the differences.
:::

<Tabs>
<TabItem value="cursor" label="Cursor" default>

1. Click **Add to Cursor**. Cursor opens and adds the server.
2. If the setup uses sign-in, click **Connect** next to the server in Cursor's MCP settings and sign in.

If the button doesn't open Cursor, expand **Didn't open?** in the drawer and copy the config into `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "dronahq_mcp": {
      "url": "https://<your-studio>/api/vibe/mcp/<your-server-id>"
    }
  }
}
```

When the drawer includes an access token, the copied config also has a `headers` entry with `Authorization: Bearer <token>`.

</TabItem>
<TabItem value="claude-code" label="Claude Code">

Run the command from the drawer in your terminal:

```bash
claude mcp add --transport http dronahq_mcp -s user "https://<your-studio>/api/vibe/mcp/<your-server-id>"
```

* **With sign-in:** run `/mcp` in Claude Code, pick the server and choose **Authenticate**. Your browser opens so you can sign in.
* **With an access token:** the drawer's command already ends with `--header "Authorization: Bearer <token>"`. There's nothing else to do.

</TabItem>
<TabItem value="claude" label="Claude">

1. In Claude, open **Settings → Connectors**.
2. Click **Add custom connector** and paste the **Server URL** from the drawer.
3. Click **Connect** and sign in with your DronaHQ account.

This works anywhere Claude supports custom connectors. On Team and Enterprise plans, an owner may need to add the connector first.

<!-- SCREENSHOT: claude-add-custom-connector.png — Claude's "Add custom connector" dialog with the DronaHQ URL pasted -->

</TabItem>
<TabItem value="vscode" label="VS Code">

1. Click **Add to VS Code** (or **VS Code Insiders**).
2. If the setup uses sign-in, VS Code asks you to sign in when the server starts.

If the button doesn't open VS Code, expand **Didn't open?** and copy the server config into your MCP configuration:

```json
{
  "servers": {
    "dronahq_mcp": {
      "type": "http",
      "url": "https://<your-studio>/api/vibe/mcp/<your-server-id>"
    }
  }
}
```

</TabItem>
<TabItem value="windsurf" label="Windsurf">

Copy the config from the drawer into `~/.codeium/windsurf/mcp_config.json`, then refresh Windsurf's MCP servers:

```json
{
  "mcpServers": {
    "dronahq_mcp": {
      "serverUrl": "https://<your-studio>/api/vibe/mcp/<your-server-id>"
    }
  }
}
```

When the drawer includes an access token, the config already contains it. Otherwise, sign in when Windsurf asks.

</TabItem>
<TabItem value="other" label="Other clients">

Any client that supports remote MCP servers over HTTP can connect. Add the **Server URL** from the drawer. The client finds DronaHQ sign-in on its own (standard MCP authorization with OAuth), opens your browser and asks you to sign in.

</TabItem>
</Tabs>

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-client-tabs.png" alt="Client setup tabs in the drawer" />
  <figcaption align="center"><i>Client setup tabs in the drawer</i></figcaption>
</figure>

### Step 4: Sign in and allow access

If your client uses sign-in, your browser opens a DronaHQ page:

1. If you aren't signed in to Studio, sign in as usual (password, Google or SSO). You come straight back.
2. Check the account and workspace shown. To use a different workspace, click **Switch**.
3. If your workspace has more than one MCP server, pick one.
4. Click **Allow access**.

You're asked to allow access only the first time a client connects. After that, sign-in is silent.

<figure>
  <Thumbnail src="/img/dronahq-mcp/consent-page.png" alt="The DronaHQ sign-in page" />
  <figcaption align="center"><i>The DronaHQ sign-in page</i></figcaption>
</figure>

### Step 5: Check the connection

Back in the drawer, **Connected clients** lists your client with when it was last used. In your AI client, check that DronaHQ tools such as `vibe_create_app`, `vibe_write_file` and `vibe_save_app` are available.

On the Apps home, the MCP card now shows **Connected**, which client you used, and when. Its button changes to **Manage DronaHQ MCP**.

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-connected-clients.png" alt="Connected clients" />
  <figcaption align="center"><i>Connected clients</i></figcaption>
</figure>

You're ready to [build your first app](./building-apps.md).

### Use one URL for every workspace

The drawer gives you a URL for one MCP server: `https://<your-studio>/api/vibe/mcp/<your-server-id>`.

If you're sharing setup steps with people in different workspaces, use the workspace-wide URL instead:

```
https://<your-studio>/api/vibe/mcp
```

Each person picks their workspace when they sign in. This URL works only with sign-in, not with access tokens.

## Next steps

* Learn how [sign-in and access tokens](./sign-in-and-access-tokens.md) work
* [Build your first app](./building-apps.md)
* Find your way around the [Vibe app builder](../vibe-apps/overview.md) in Studio
