---
sidebar_position: 3
title: "Files and Code"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Files and code

Choose **Code** in the top bar to see the app's files in the left sidebar, and **Code View** to edit them.

## Files

`index.html`, marked with a star, is the app's entry point. Generated apps usually keep the rest under `src`: screens and UI in `src/components`, and the code that calls your connectors in `src/lib`.

| Button | What it does |
| --- | --- |
| **+** | **New File**, **New Folder**, **Upload Files** or **Upload Folder** |
| **Pull files from storage** | Loads the latest saved files. See [Pull files from storage](#pull-files-from-storage) |
| **Download bundle** | Downloads the app's source as a ZIP, set up as a Vite project |

<figure>
  <Thumbnail src="/img/vibe-apps/files.png" alt="The Files buttons, with the + menu open" width="70%" />
  <figcaption align="center"><i>The Files buttons, with the + menu open</i></figcaption>
</figure>

Right-click a file or folder for more actions:

| On a file | On a folder |
| --- | --- |
| **Rename**, **Duplicate**, **Delete** | **Rename**, **New File Here**, **New Folder Here**, **Delete** |

Deleting a folder deletes every file in it.

### Upload files

* Upload text files, such as code, CSS, JSON and SVG. Don't upload binary files, such as PNG images or fonts, here. They're read as text and won't work.
* If a file with the same path exists, you're asked whether to replace it.
* **Upload Folder** puts the folder's contents at the root of the app. The folder itself isn't kept.

### Find files and text

| To | Use |
| --- | --- |
| Filter the list by file name or content | The search box above the list |
| Go to a file by name | Ctrl+P (Cmd+P on Mac) |
| Search inside every file | Ctrl+Shift+F (Cmd+Shift+F on Mac). Tick **Aa** to match case |

## Edit code

Click a file to open it. A dot next to the file name means it has unsaved changes.

Files aren't saved as you type. Click **Save** on the file, or press Ctrl+S, to save it and rebuild the app. If you switch files with unsaved changes, choose **Save**, **Discard** or **Cancel**.

Markdown files show a live preview next to the code.

| Shortcut | Action |
| --- | --- |
| Ctrl+S / Cmd+S | Save the file |
| Ctrl+F / Cmd+F | Find |
| Ctrl+H / Cmd+H | Find and replace |
| F3, Shift+F3 | Next match, previous match |
| Ctrl+G / Cmd+G | Go to line |
| Ctrl+/ or Cmd+/ | Comment or uncomment |
| Ctrl+Space | Autocomplete |
| Ctrl+Q | Fold or unfold a block |

## Fix a syntax error

If the code doesn't build, you see **Syntax error — preview not updated**, and the preview keeps the last working build.

* **Copy** copies the error
* **Fix with AI** sends the error to the chat as a prompt

You can also fix the code yourself and save the file again.

## Pull files from storage

Use this when the app's files may have changed somewhere else, for example through MCP, the code editor extension or a teammate. Click **Pull files from storage** above the file list to load the latest saved version of every file.

:::caution Pulling replaces unsaved edits
If you have unsaved edits, you're asked to confirm, and continuing overwrites them. Save the files you want to keep first.
:::

## Edit in VS Code, Cursor or Windsurf

Save the app, then click **Open in VS Code** in the footer. To use Cursor or Windsurf, click the arrow next to it. Your choice is remembered. You need the [DronaHQ code editor extension](../code-editor-extension/overview.md).

To let your editor's AI agent build the app instead, choose **Connect via MCP** from the same menu. See [DronaHQ MCP](../dronahq-mcp/getting-started.md).
