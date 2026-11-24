---
sidebar_position: 8
title: "Tool Reference"
---

# Tool reference

Every tool DronaHQ MCP exposes, what it does, and the minimum access it needs. **Read-only** tools never change anything. Your AI client may show this as a read-only or destructive hint.

Owners and admins have Owner access to every app and connector, so the app and connector requirements below apply to creators. See [Permissions](./permissions.md).

## Apps and files

| Tool | What it does | Read-only | Needs |
| --- | --- | --- | --- |
| `vibe_list_apps` | Lists the Vibe apps you can see | Yes | Shows only apps you can access |
| `vibe_create_app` | Creates a Vibe app | No | Owner or admin, or creator with Editor |
| `vibe_get_app` | Gets an app's details | Yes | Preview Only on the app |
| `vibe_list_files` | Lists an app's files | Yes | Preview Only on the app |
| `vibe_read_file` | Reads a file | Yes | Preview Only on the app |
| `vibe_write_file` | Creates or overwrites a file | No | Editor on the app |
| `vibe_delete_file` | Deletes a file | No | Editor on the app |
| `vibe_save_app` | Saves written files to the app | No | Editor on the app |
| `vibe_preview_url` | Gets a preview link | Yes | Preview Only on the app |
| `vibe_read_lowcode_source` | Reads a low-code app's screens and queries, for conversion | Yes | Preview Only on that app |

## Connectors and data

| Tool | What it does | Read-only | Needs |
| --- | --- | --- | --- |
| `vibe_list_connectors` | Lists connectors | Yes | Shows only connectors you can use |
| `vibe_get_connector_accounts` | Lists a connector's accounts | Yes | Use Only |
| `vibe_get_db_schema` | Gets tables and columns | Yes | Use Only |
| `vibe_run_db_query` | Runs read-only SQL | Yes | Use Only |
| `vibe_run_db_write` | Runs SQL that changes data or schema, or NoSQL actions | No | Use Only |
| `vibe_request_connector_setup` | Starts setting up a new connector and returns a Studio link | No | Owner or admin |
| `vibe_get_connector_job` | Checks whether connector setup has finished | Yes | The person who started setup |
| `vibe_bind_connectors` | Connects connectors to an app | No | Editor on the app and Use Only on each connector |
| `vibe_list_subcats` | Lists saved operations | Yes | Use Only |
| `vibe_get_subcat` | Gets a saved operation | Yes | Use Only |
| `vibe_get_subcat_input_schema` | Gets a saved operation's inputs | Yes | Use Only |
| `vibe_run_subcat` | Runs a saved operation | No | Use Only |
| `vibe_create_subcat` | Creates a saved database query | No | Editor on the connector |
| `vibe_create_http_read_subcat` | Creates a saved HTTP `GET`/`HEAD` operation | No | Editor on the connector |
| `vibe_create_http_write_subcat` | Creates a saved HTTP `POST`/`PUT`/`PATCH`/`DELETE` operation | No | Editor on the connector |

## Data Agents

| Tool | What it does | Read-only | Needs |
| --- | --- | --- | --- |
| `vibe_list_data_agents` | Lists Data Agents | Yes | Shows only agents for connectors you can use |
| `vibe_get_data_agent_context` | Gets a connector's Data Agent context | Yes | Use Only |
| `vibe_prepare_sql_query_instructions` | Gets context, live schema and SQL dialect notes before writing SQL | Yes | Use Only |
| `vibe_create_data_agent` | Creates a connector's Data Agent | No | Editor on the connector |
| `vibe_update_data_agent` | Updates a Data Agent | No | Editor on the connector |

## Publishing and app access

| Tool | What it does | Read-only | Needs |
| --- | --- | --- | --- |
| `vibe_check_in` | Saves a version snapshot without publishing | No | Editor on the app |
| `vibe_publish` | Publishes a new version | No | Publisher on the app |
| `vibe_set_access` | Sets whether the public link needs sign-in | No | Publisher on the app, and the secure embed licence |
| `vibe_get_app_permissions` | Shows who has access to an app | Yes | Owner on the app |
| `vibe_set_app_permissions` | Replaces who has access to an app | No | Owner on the app |

## Catalogues, groups and users

| Tool | What it does | Read-only | Needs |
| --- | --- | --- | --- |
| `vibe_list_catalogues` | Lists catalogues | Yes | Anyone who can connect |
| `vibe_get_catalogue_users` | Shows a catalogue's groups and users | Yes | Anyone who can connect |
| `vibe_create_catalogue` | Creates a catalogue | No | Owner or super admin |
| `vibe_add_catalogue_groups` | Adds groups to a catalogue | No | Owner or admin |
| `vibe_set_catalogue_groups` | Replaces a catalogue's groups | No | Owner or admin |
| `vibe_list_groups` | Lists groups | Yes | Owner or admin |
| `vibe_create_group` | Creates a group | No | Owner or admin |
| `vibe_check_users` | Checks whether emails exist and can be invited | Yes | Owner or admin |
| `vibe_add_user_to_group` | Invites a user or adds them to groups | No | Owner or admin |
| `vibe_deactivate_user` | Deactivates a user | No | Owner or admin |
| `vibe_reactivate_user` | Reactivates a user | No | Owner or admin |
| `vibe_delete_user` | Permanently deletes a deactivated user | No | Owner or super admin |

## Automations

All automation tools need the owner, admin or creator role.

| Tool | What it does | Read-only |
| --- | --- | --- |
| `automation_list` | Lists automations | Yes |
| `automation_get` | Gets an automation | Yes |
| `automation_create` | Creates an automation draft | No |
| `automation_rename` | Renames an automation | No |
| `automation_duplicate` | Copies an automation | No |
| `automation_delete` | Deletes an automation | No |
| `automation_set_trigger` | Sets the Scheduler or WebHook trigger | No |
| `automation_get_task_schema` | Gets a task type's schema | Yes |
| `automation_get_tasks` | Lists an automation's tasks | Yes |
| `automation_add_tasks` | Adds tasks | No |
| `automation_update_task` | Updates a task | No |
| `automation_move_tasks` | Moves tasks | No |
| `automation_remove_tasks` | Removes tasks | No |
| `automation_validate` | Checks the automation for errors | Yes |
| `automation_test_task` | Tests one task, skipping changes to data | No |
| `automation_test_task_write` | Tests one task, including changes to data | No |
| `automation_test_run` | Tests the whole automation, skipping changes to data | No |
| `automation_test_run_write` | Tests the whole automation, including changes to data | No |
| `automation_publish` | Publishes the automation | No |
| `automation_set_active` | Turns a published automation on or off | No |

## Resources and prompts

The server also exposes guidance documents as MCP resources, under `dronahq://vibe/...` (for example `dronahq://vibe/connector-setup-checklist`), and ready-made prompts named `dronahq_vibe_*`. Your AI client uses these automatically. You don't need to load them yourself.
