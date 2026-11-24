---
sidebar_position: 4
title: "Connectors"
---

# Connectors

The **Connectors** section, below **Files** in the **Code** view, lists the connectors this app uses, not every connector in your workspace. If there are none, it says "No connectors bound to this app".

<!-- SCREENSHOT: vibe-connectors.png — the Connectors section with one connector expanded -->

## What's in the list

Click a connector to expand or collapse it. What's underneath depends on how the app uses it:

| Item | What happens when you click it |
| --- | --- |
| A query or API operation | Opens it in the connector's query editor |
| A database table | Nothing. Tables are listed for reference |

You can't open the queries of a shared connector that you don't have permission to edit.

## Manage the account

Point to a connector and click the person icon (**Manage account**) to see or change the account it connects with.

An account can hold different details for each [data environment](../datasource-concepts/data-environments.md). The environment chosen in the footer decides which ones the app uses. See [Switch environment](./preview.md#switch-environment).

## Add a connector to the app

Ask for it in the chat: type `@`, pick the connector and say what it's for. If you don't name one, the agent can ask you to choose or set up a connector when the app needs data. From an AI client, see [Connecting data](../dronahq-mcp/connecting-data.md).

## Refresh the list

The list updates when you close the connector editor. If a connector was added somewhere else, click **Refresh connectors**.
