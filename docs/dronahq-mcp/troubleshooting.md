---
sidebar_position: 9
title: "Troubleshooting"
---

# Troubleshooting

## Connecting

| Problem | Fix |
| --- | --- |
| DronaHQ tools don't appear in my client | Reconnect the server in your client: run `/mcp` in Claude Code, reload the MCP settings in Cursor or Windsurf, or restart the server in VS Code. Check that **Connected clients** in the drawer lists your client |
| The client never opens a sign-in page | Make sure you added the server URL from the drawer, not an old URL. Clients that use an access token don't sign in |
| "You can't use DronaHQ MCP here" | Your account is an end user. Ask an admin to make you a creator |
| "Switch workspace to continue" | You're signed in to another workspace. Click **Sign in to {workspace}** |
| "This sign-in request expired" | Start connecting again from your client |
| "Pasted access tokens are turned off for this workspace" (from your client), or "Access tokens are turned off for this workspace" (in the drawer) | Your admin requires sign-in. Remove the token from your client's config and connect with the server URL. See [Sign-in and access tokens](./sign-in-and-access-tokens.md) |
| My client stopped working for everyone at once | An admin may have rotated the org secret. Sign in again, or copy the new setup from the drawer |
| "Permission denied: user account is deactivated" | Your account was deactivated. Ask an admin to reactivate it |

## Building

| Problem | Fix |
| --- | --- |
| Preview doesn't show the latest changes | Ask the agent to save the app (`vibe_save_app`). Written files aren't live until they're saved |
| Preview shows placeholder data | Open the app in Studio's Preview, not as a raw HTML file, and check the **Connected** badge |
| The agent doesn't see the error I'm seeing | Open **Preview** and reproduce the problem once so the run log is up to date, then ask again |
| "Permission denied: … requires Editor (0010) on app …" | You need more access to that app. Ask the app's owner or an admin. See [Permissions](./permissions.md) |
| "App publish license limit reached" | Your plan's published app limit is reached. Unpublish an app or upgrade |
| "App version limit reached" | Your plan's version limit is reached |
| "Secure embed license required" | Requiring sign-in on public links needs the secure embed licence |

## Data

| Problem | Fix |
| --- | --- |
| The setup link says it's expired | Links last 30 minutes. Ask the agent for a new one |
| "Connector setup job not found" | Open the link while signed in as the person who asked the agent for it |
| "only workspace owners and admins can add connectors" | Ask an admin to create the connector and grant it to you or your group |
| The agent says a query isn't read-only | `vibe_run_db_query` only runs reads. Writes and schema changes go through `vibe_run_db_write` |
| Automation tools fail | The automation service may be unavailable. App building and data access still work |
