---
sidebar_position: 3
title: "Building Apps"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Building apps

Once your AI client is [connected](./getting-started.md), you build by describing what you want. Mention DronaHQ in your prompt so the agent uses DronaHQ tools rather than writing a standalone project.

## How a build goes

### 1. The agent asks before it builds

The agent's first reply to a new app request is clarifying questions, not code. It then writes **user stories** for you to confirm. If it's unclear whether you want a new app or a change to an existing one, it asks.

<figure>
  <Thumbnail src="/img/dronahq-mcp/user-stories.png" alt="Agent proposing user stories" />
  <figcaption align="center"><i>The agent proposes user stories before building</i></figcaption>
</figure>

**Tips for better results:**

* Confirm or correct the user stories before the agent creates the app
* Name the connector to use, if you already have one
* Review the proposed table schema before the agent creates tables

### 2. Choose where the data lives

If the app needs data, the agent asks whether to use an existing connector or set up a new one, and proposes a schema for you to approve. See [Connecting data](./connecting-data.md).

### 3. The agent creates the app and writes its files

The agent creates a Vibe app and writes its source files. Vibe apps are React apps that load their libraries from a CDN. There's no `package.json` or bundler.

| Tool | What it does |
| --- | --- |
| `vibe_create_app` | Creates the app and returns its `pluginId` |
| `vibe_write_file` | Writes a file such as `index.html`, `src/main.jsx` or a component |
| `vibe_bind_connectors` | Connects the app to the connectors it uses |
| `vibe_save_app` | Saves the files to the app in Studio |
| `vibe_preview_url` | Returns a link to preview the app |

:::note Saving is a separate step
Files the agent writes aren't part of the app until it calls `vibe_save_app`. Preview always shows the last saved version.
:::

<figure>
  <Thumbnail src="/img/dronahq-mcp/code.png" alt="The generated app in Studio" />
  <figcaption align="center"><i>The generated app in Studio</i></figcaption>
</figure>

### 4. Review the code in Studio

Open the app in Studio and switch to **Code View** to see what the agent wrote:

* `index.html`: loads React, ReactDOM and Tailwind from a CDN
* `src/main.jsx`: the app's entry point
* `src/components/`: the screens and UI
* `src/lib/`: the data layer that calls your connectors

Use **Open in VS Code** in the footer to edit locally with the DronaHQ extension. See [Files and code](../vibe-apps/files-and-code.md) for everything you can do in Code View.

### 5. Preview the app

Switch to **Preview** and check that:

* The **Connected** badge is shown, meaning the app is using live data through its connector
* The UI matches what you asked for
* Data loads from your database, not placeholder data

<figure>
  <Thumbnail src="/img/dronahq-mcp/support-ticket.png" alt="Preview of the running app" />
  <figcaption align="center"><i>Preview of the running app</i></figcaption>
</figure>

#### Preview as another user

To check what a specific person sees, use the user selector in the preview footer. Search for a user and pick them. The preview then runs with that person's name, email and groups, so you can check permission-based screens without signing in as them. See [Preview](../vibe-apps/preview.md) for this, switching environments and selecting elements.

<figure>
  <Thumbnail src="/img/vibe-apps/preview-as-user.png" alt="Choosing a user to preview as" width="70%" />
  <figcaption align="center"><i>Choosing a user to preview as</i></figcaption>
</figure>

### 6. Iterate

Keep prompting in the same conversation, or refer to the app by name or `pluginId`:

> Update TaskFlow (pluginId 7465):
> * Add an `update_link` column and a quarterly frequency
> * Rebuild the dashboard as dealer tiles with a pending popup

The agent edits files with `vibe_write_file`, saves with `vibe_save_app`, and changes tables with `vibe_run_db_write`.

## Fixing bugs with the preview run log

Every time you run Preview, Studio records the console output and network calls in a run log the agent can read (`logs/preview-run.json`). When you report a bug, the agent reads this log first, so it sees the same errors you do.

To get the most useful log:

1. Open **Preview** and reproduce the problem once.
2. Tell the agent what went wrong.

Passwords, tokens and other sensitive headers are removed from the log, and long responses are shortened. The log isn't shown in the app's file list. To read the same activity yourself, see [Console and network logs](../vibe-apps/console-and-network.md).

## Save versions and publish

| Step | Tool | What it does |
| --- | --- | --- |
| **Check in** | `vibe_check_in` | Saves a version snapshot you can go back to. Doesn't publish. The agent asks before checking in. |
| **Publish** | `vibe_publish` | Releases a new version to your users. See [Publishing and users](./publishing-and-users.md). |

## Start from existing code or apps

### Import a frontend project

To bring an existing frontend into DronaHQ, go to **Apps → Create app → Import app** and choose the project folder.

* **Supported:** React, Next.js (pages with an entry point), Vue 3, Svelte 4 and 5, plain HTML and JavaScript, Vite, Create React App, Tailwind v3 and v4, Sass and Less, browser-compatible npm packages, and `.env` files.
* **Not supported:** Angular, Nuxt, SvelteKit, Vue 2 and build output folders. Calls to the project's own backend aren't imported. Use DronaHQ connectors for data instead.

After importing, you can keep working on the app from your AI client.

<!-- SCREENSHOT: import-app.png — the Import app dialog -->

### Convert a low-code app

To rebuild an existing DronaHQ low-code app as a Vibe app, ask the agent to convert it by name. The agent reads the low-code app's screens and data queries (`vibe_read_lowcode_source`) and uses them as the starting point. If several apps share the name, it asks which one you mean.

## Usage

Writing a file (`vibe_write_file`) and saving the app (`vibe_save_app`) each count as one Vibe task toward your plan's usage. Reading a file (`vibe_read_file`) or getting a preview link (`vibe_preview_url`) doesn't. Some actions in Studio also use tasks, such as reloading the preview. See [Credits and tasks](../vibe-apps/chat.md#credits-and-tasks).
