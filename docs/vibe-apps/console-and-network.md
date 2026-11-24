---
sidebar_position: 6
title: "Console and Network Logs"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Console and network logs

In Preview, click **Console | Network** in the footer to see what the app is doing as it runs. The button is highlighted when an error is logged while the panel is closed. Drag the top edge of the panel to resize it.

## Console

The **Console** tab shows what the app writes with `console.log`, `info`, `warn`, `error` and `debug`, along with uncaught errors and unhandled promise rejections. Each entry has its type and time, and errors include a stack trace when there is one.

* **Filter:** tick the types you want: **Log**, **Error**, **Warn**, **Info**, **Debug** or **All**
* **Search:** type in **Search logs...** to match messages and stack traces
* **Clear:** removes the current logs

The console also clears each time you switch from Code View to Preview.

<!-- SCREENSHOT: vibe-console.png — the Console tab with a log, a warning and an error -->

## Network

The **Network** tab lists the connector calls the app makes, such as database queries and API requests. **Logs** shows each call with its status and time, and you can open a call to see its request and response. **Timeline** and **Watchlist** are covered in [Connector logs](../debugging-apps/connector-logs.md).

* **Search** by name or initiator
* **Filter by** all, successful, failed or pending calls
* **Clear Log** empties the list
* **Download Log** saves **All Logs** or only **Error Logs**

<figure>
  <Thumbnail src="/img/vibe-apps/network.png" alt="A failed connector call in the Network tab" width="100%" />
  <figcaption align="center"><i>A failed connector call in the Network tab</i></figcaption>
</figure>

## Preview run log

While Preview is open, Studio also saves the console and network activity to a run log that AI clients connected through [DronaHQ MCP](../dronahq-mcp/building-apps.md#fixing-bugs-with-the-preview-run-log) can read. To get help with a bug, reproduce it once in Preview, then tell the agent what went wrong.

* Each run replaces the last one
* Values stored under names such as `password`, `token`, `api_key`, `cookie` or `authorization` are replaced with `[redacted]`
* Long responses are shortened
* The log isn't listed under **Files**, and writing it doesn't use a task

:::caution Other data isn't hidden
Only values that look like credentials are removed. Records the app loads, such as names and email addresses, can appear in the log.
:::

## Find the cause of a problem

| What you see | Where to look |
| --- | --- |
| A blank screen | **Console**, with only **Error** ticked |
| Missing or wrong data | **Network**: check that the call succeeded, then its response. Check the [environment](./preview.md#switch-environment) too |
| The wrong screens for a user | [Preview as that user](./preview.md#preview-as-another-user) |
| Your latest change doesn't appear | Check that the file is saved and has no [syntax error](./files-and-code.md#fix-a-syntax-error) |
| Changes made through MCP or a code editor don't appear | [Pull files from storage](./files-and-code.md#pull-files-from-storage), then open Preview again |
