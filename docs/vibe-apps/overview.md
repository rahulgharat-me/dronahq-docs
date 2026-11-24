---
sidebar_position: 1
title: "Overview"
pagination_prev: null
---
import Thumbnail from '@site/src/components/Thumbnail';

# Vibe app builder

A Vibe app is a code-based app that you build by describing what you want. You can build it with the AI chat in Studio, from your own AI client through [DronaHQ MCP](../dronahq-mcp/getting-started.md), or by editing the code yourself. All three work on the same app, and you review, test and publish it in the Vibe app builder.

This page is a map of the builder. Each area links to the page that explains it.

<figure>
  <Thumbnail src="/img/dronahq-mcp/code.png" alt="The Vibe app builder in Code View, with files and connectors on the left" />
  <figcaption align="center"><i>The Vibe app builder in Code View</i></figcaption>
</figure>

## Top bar

| Control | What it does |
| --- | --- |
| App name | Opens a menu with **Back to Dashboard**, your remaining **Credits**, **Edit App** and **Help** |
| **+** (New Thread) | Starts a new chat and clears the current one. See [Start a new thread](./chat.md#start-a-new-thread) |
| **Chat** / **Code** | Switches the left sidebar between the AI chat and the app's files and connectors |
| Sidebar icon | Closes or opens the left sidebar |
| **Code View** / **Preview** | Switches the main area between the code editor and the running app |
| Reload icon | Reloads the preview. Shown only in Preview |
| Pointer icon | Selects an element in the preview. Shown only in Preview. See [Select an element](./preview.md#select-an-element) |
| Gear icon | Opens the [settings menu](#settings-menu) |
| **Versioning** | Saves a version or goes back to an earlier one. See [Versions and publishing](./versions-and-publishing.md) |
| **Publish & Share** | Publishes the app and controls who can open it. See [Publish](./versions-and-publishing.md#publish) |
| Save icon | Saves the app (Ctrl+S, or Cmd+S on Mac). A green tick means everything is saved |

## Left sidebar

| View | What you see |
| --- | --- |
| **Chat** | The AI chat and its prompt box. See [Chat](./chat.md) |
| **Code** | **Files**, the app's source files, with **Connectors**, the connectors the app uses, below. See [Files and code](./files-and-code.md) and [Connectors](./connectors.md) |

A new app opens on **Chat**. An app that already has code opens on **Code**.

## Footer

| Control | What it does |
| --- | --- |
| Environment | Chooses the data environment the app uses. See [Switch environment](./preview.md#switch-environment) |
| **Open in VS Code** | Opens the app in VS Code, Cursor or Windsurf. The arrow lists the editors and **Connect via MCP** |
| Desktop and mobile icons | Change the preview size. Shown only in Preview |
| User name | Previews the app as another user. Shown only in Preview. See [Preview as another user](./preview.md#preview-as-another-user) |
| **Console \| Network** | Shows the app's logs and connector calls. Shown only in Preview. See [Console and network logs](./console-and-network.md) |
| **?** | Documentation, support and keyboard shortcuts |

## Settings menu

The gear icon opens these app settings:

| Setting | Use it to |
| --- | --- |
| **Releases & History** | See the app's published releases |
| **Query String** | Define the query string values the app receives |
| **App Settings** | Change the app's properties |
| **App Export** | Export the app |
| **Connector Health** | Run a health check on the app's connectors |

## While the AI is writing code

The builder switches to Code View so you can watch the code being written. **Preview**, **Publish & Share** and the settings menu are unavailable until it finishes. To stop early, click the stop button in the chat.
