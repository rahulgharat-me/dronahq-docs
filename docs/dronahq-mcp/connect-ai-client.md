---
sidebar_position: 3
title: "Connect Your AI Client"
---
import Thumbnail from '@site/src/components/Thumbnail';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Connect your AI client

Connecting takes about two minutes. You add your workspace's MCP server URL to your AI client, then sign in to DronaHQ when the client asks. You don't need to copy a token.

:::note Before you start
You need a DronaHQ account with the **owner**, **admin** or **creator** role. End users can't connect an AI client. See [Who can use it](./overview.md#who-can-use-it).
:::

## Step 1: Open the Connect DronaHQ MCP drawer

You can open it from several places in Studio:

* **Apps home:** the **Build from your IDE via MCP** card → **Connect DronaHQ MCP**
* **Apps tile view:** **Create app using MCP**
* **Inside a Vibe app:** the editor menu (**Open in Cursor/Windsurf…**) → **Connect via MCP**
* **AI chat in the builder:** **Connect via MCP** → **Connect DronaHQ MCP**

<figure>
  <Thumbnail src="/img/dronahq-mcp/apps-home-mcp-card.png" alt="The DronaHQ MCP card on the Apps home" />
  <figcaption align="center"><i>The DronaHQ MCP card on the Apps home</i></figcaption>
</figure>

### First time in DronaHQ?

New accounts, and people joining a workspace for the first time, start on a **Connect your AI client to DronaHQ** screen. It walks through the same three steps as this page: copy your MCP URL, add it to your client, and sign in when the client asks. When your client connects, the screen shows **Connected** and a **Start building** button. Use **Build in Studio instead** or **Skip for now** to go straight to Studio.

<figure>
  <Thumbnail src="/img/dronahq-mcp/welcome-connect-ai-client.png" alt="The first-run Connect your AI client screen" />
  <figcaption align="center"><i>The first-run Connect your AI client screen</i></figcaption>
</figure>

## Step 2: Create or pick an MCP server

Each workspace needs at least one MCP server. It's the address your AI clients connect to.

* If **Your MCP servers** already lists a server, select it.
* If not, fill in **Create MCP server** and click **Create server**. The defaults (**DronaHQ MCP**, "Tools to create and manage apps via DronaHQ MCP") are fine.

You can also skip this step. If you sign in through the workspace-wide URL (see [Use one URL for every workspace](#use-one-url-for-every-workspace)) and your workspace has no server yet, DronaHQ creates one for you when you allow access.

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-servers.png" alt="Your MCP servers in the Connect DronaHQ MCP drawer" />
  <figcaption align="center"><i>Your MCP servers in the Connect DronaHQ MCP drawer</i></figcaption>
</figure>

## Step 3: Add the server to your AI client

Pick your client's tab in the drawer and follow the steps. The drawer fills in your server URL for you.

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

## Step 4: Sign in and allow access

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

## Step 5: Check the connection

Back in the drawer, **Connected clients** lists your client with when it was last used. In your AI client, check that DronaHQ tools such as `vibe_create_app`, `vibe_write_file` and `vibe_save_app` are available.

On the Apps home, the MCP card now shows **Connected**, which client you used, and when. Its button changes to **Manage DronaHQ MCP**.

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-connected-clients.png" alt="Connected clients" />
  <figcaption align="center"><i>Connected clients</i></figcaption>
</figure>

You're ready to [build your first app](./building-apps.md).

## Use one URL for every workspace

The drawer gives you a URL for one MCP server: `https://<your-studio>/api/vibe/mcp/<your-server-id>`.

If you're sharing setup steps with people in different workspaces, use the workspace-wide URL instead:

```
https://<your-studio>/api/vibe/mcp
```

Each person picks their workspace when they sign in. This URL works only with sign-in, not with access tokens.
