---
sidebar_position: 4
title: "Connecting Data"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Connecting data

Vibe apps read and write data through DronaHQ **connectors**: databases, REST and GraphQL APIs, and accounts such as AWS S3, SMTP or Google Cloud Storage. Connector credentials are stored in the DronaHQ vault. They never pass through your AI client, so the agent can use a database without ever seeing its password.

## How the agent picks a connector

When an app needs data, the agent asks which kinds of connector you need. For each one, it asks whether to **use an existing connector** or **set up a new one**.

The agent works with only the connector you choose. It doesn't browse every connector in your workspace.

## Set up a new connector

Only **owners and admins** can set up new connectors. Creators can use the connectors they've been given access to. If you're a creator, ask an admin to create the connector and grant it to you or your group.

Setting up a connector happens in Studio, not in the chat:

1. The agent prepares the connector with the non-secret details, such as host, port, database name, username, region or bucket.
2. It gives you a Studio link. Open it in a browser where you're signed in to the same workspace.
3. Studio opens the connector form with a banner: "… setup ready — enter credentials and save. The MCP agent is waiting."
4. Enter the password, API key or other credentials, and save.
5. The agent notices that setup is finished and continues. It can now propose a schema, create tables and connect the app to the new connector.

The link works only for you and **expires after 30 minutes**. If it expired, Studio says "This setup link is expired. Request a new magic link from MCP." Ask the agent for a new link.

<!-- SCREENSHOT: connector-setup-banner.png — connector form opened from the setup link, with the "setup ready" banner -->

:::note
Some connectors can't be set up from a link: **DronaHQ Database**, managed **File Storage** and **Google Sheets**. Set these up in Studio (Google Sheets signs in with Google), then ask the agent to use the existing connector.
:::

## Use an existing connector

The agent lists the connectors you can use. Pick one by name, and the agent maps your tables and connects the app to it.

To use a connector, you need at least **Use Only** access to it. See [Permissions](./permissions.md#connector-permissions).

## Query data

| Tool | Use it for |
| --- | --- |
| `vibe_get_db_schema` | Inspect tables and columns |
| `vibe_run_db_query` | **Read-only** SQL: one `SELECT`, `WITH`, `SHOW`, `DESCRIBE` or `EXPLAIN` statement per query. You can send a batch of separate queries in one call |
| `vibe_run_db_write` | Changes: `CREATE`, `ALTER`, `INSERT`, `UPDATE`, `DELETE`, and actions on NoSQL connectors |

`vibe_run_db_query` refuses anything that changes data. It also refuses a query that joins several statements with semicolons, such as `SELECT 1; SELECT 2`. Send them as separate queries in a batch instead. `vibe_run_db_write` refuses read-only statements. Both tools need **Use Only** access to the connector.

:::caution Review changes before you approve them
`vibe_run_db_write` runs against your real database. Approve schema changes and bulk updates only after reading what the agent proposes.
:::

## Saved operations

Saved operations are named, reusable queries stored on a connector. Apps call them by name, and inputs are passed as placeholders.

| Tool | Creates |
| --- | --- |
| `vibe_create_subcat` | A database query |
| `vibe_create_http_read_subcat` | An HTTP `GET` or `HEAD` operation |
| `vibe_create_http_write_subcat` | An HTTP `POST`, `PUT`, `PATCH` or `DELETE` operation, with a body |

Rules for saved operations:

* Use `{{name}}` for inputs. Quote text, date and `LIKE` values: `'{{name}}'`.
* Names start with a letter and use only letters, digits and underscores.
* Authentication isn't stored on the operation. It's set on the connector in Studio.
* Server-side variables such as `{{user.email}}` work only in saved queries, not in ad-hoc queries.

Creating a saved operation needs **Editor** access to the connector. Listing and running one needs **Use Only**.

## Data Agents

A Data Agent gives the AI context about one connector's data: what the tables mean, how they relate, and which SQL dialect to use. Each connector has at most one Data Agent.

Before writing SQL, the agent loads the connector's Data Agent context together with the live schema. If the connector has no Data Agent yet, the agent stops and asks whether to create one.

| Tool | Needs |
| --- | --- |
| `vibe_list_data_agents` | Shows only Data Agents for connectors you can use |
| `vibe_get_data_agent_context` | **Use Only** on the connector |
| `vibe_create_data_agent`, `vibe_update_data_agent` | **Editor** on the connector |
