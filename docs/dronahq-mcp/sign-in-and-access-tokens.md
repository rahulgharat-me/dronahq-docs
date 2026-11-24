---
sidebar_position: 2
title: "Sign-in and Access Tokens"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Sign-in and access tokens

An AI client can connect to DronaHQ MCP in two ways. Both give the client exactly your role and permissions, nothing more.

| | Sign-in (OAuth) | Access token |
| --- | --- | --- |
| **How you connect** | Add the server URL, then sign in to DronaHQ in your browser | Use the setup from the drawer, which already contains the token |
| **Clients** | All: Claude, Claude Code, Cursor, VS Code, Windsurf and any MCP client that supports OAuth | Cursor, Claude Code, VS Code and Windsurf |
| **Expiry** | Renews itself. You sign in again after 30 days without use, and at least every 90 days | Never expires |
| **Disconnect one client** | Yes, from **Connected clients** | No. Only by rotating the org secret or turning tokens off |
| **Recommended for** | Everyone, and required when tokens are off | Quick setup on your own machine |

## Sign-in (OAuth)

Sign-in uses the standard MCP authorization flow. Your client opens a DronaHQ page in your browser, you sign in and allow access, and the client receives short-lived credentials that renew automatically.

### The sign-in page

The page reads **Connect {client} to DronaHQ** and **{client} wants to build in {workspace} as you**. It shows:

* The account and workspace you're signing in with, and a **Switch** link to use another workspace
* An **MCP server** list, if your workspace has more than one server
* What the client will be able to do:
  * Create, edit and publish Vibe apps and automations
  * Use the connectors and data you already have access to
  * Act with your role, never more than you can do in Studio
* Where you'll be sent back to after you allow access

Click **Allow access** to connect, or **Cancel**.

:::caution
Only click **Allow access** if you just started connecting from that client yourself.
:::

You're asked to allow access only the first time a client connects to a server. Later sign-ins skip this page.

<figure>
  <Thumbnail src="/img/dronahq-mcp/consent-page.png" alt="The DronaHQ sign-in page" />
  <figcaption align="center"><i>The DronaHQ sign-in page</i></figcaption>
</figure>

### Other screens you may see

| Screen | What it means |
| --- | --- |
| **Switch workspace to continue** | You're signed in to a different workspace than the one the client asked for. Click **Sign in to {workspace}**. |
| "This workspace doesn't have a DronaHQ MCP server yet" | Allowing access creates a server for you. |
| **You're connected** | Shown for desktop apps such as Cursor. Click **Open {client}** to go back. |
| **You can't use DronaHQ MCP here** | Your account is an end user. Ask an admin to make you a creator. |
| **This sign-in request expired** | Start connecting again from your client. |

<!-- SCREENSHOT: switch-workspace.png — the "Switch workspace to continue" screen -->

### How long sign-in lasts

The client's access is renewed automatically every hour while you use it. If you don't use the client for 30 days, or 90 days after you first signed in, you're asked to sign in again.

## Access tokens

For Cursor, Claude Code, VS Code and Windsurf, the drawer adds an access token to the setup it shows you, so the client connects without a sign-in step. The token:

* Belongs to you, and the client acts with your role
* **Never expires**
* Can't be revoked on its own. The only ways to stop it are to [rotate the org secret](#rotate-the-org-secret) (which disconnects everyone) or to [turn access tokens off](#turn-access-tokens-off)

Treat a token like a password. Don't commit it to a repository or paste it into chats. Every client stores it in its own MCP config file, so treat those files as sensitive.

To get a fresh token, open **Token settings** in the drawer and click **Generate token**.

<figure>
  <Thumbnail src="/img/dronahq-mcp/drawer-token-settings.png" alt="Access token options in the drawer" />
  <figcaption align="center"><i>Access token options in the drawer</i></figcaption>
</figure>

## Manage connected clients

**Connected clients** in the drawer lists every client connected to the selected server: the client, who connected it, and when it was last used. Clients that use an access token have an **Access token** badge.

* Everyone sees their own clients. Owners and admins see everyone's.
* Click **Disconnect** to remove a client that signed in. It stops working right away and has to sign in again to reconnect. Owners and admins can disconnect anyone's client.
* Clients using an access token can't be disconnected one at a time.

Deleting an MCP server also disconnects every client using it.

## Admin settings

Owners and admins have two extra controls in **Token settings**.

### Turn access tokens off

Clear **Allow access tokens in this workspace** to require sign-in for everyone. Clients using a pasted token stop working right away. Clients that signed in aren't affected. The drawer then shows "Access tokens are turned off for this workspace. Connect with the server URL and sign in."

We recommend turning tokens off once everyone in your workspace has switched to sign-in.

### Rotate the org secret

**Rotate org secret** disconnects **every** client in the workspace right away. Signed-in clients must sign in again, and every access token stops working. This also disconnects your workspace's other MCP connections, such as custom MCP servers. Use it if a token may have leaked.

Both actions are recorded in your workspace's audit log.

## Security notes

* Every request is checked against your current role and permissions. If your role changes or your account is deactivated, the client's access changes immediately.
* Credentials for databases and APIs never pass through your AI client. See [Connecting data](./connecting-data.md).
* Prefer sign-in for shared or managed machines. Disconnect clients you no longer use.
