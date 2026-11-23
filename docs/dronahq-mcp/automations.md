---
sidebar_position: 7
title: "Automations"
---

# Automations

You can build DronaHQ [automations](../automations/overview.md) from your AI client: scheduled jobs and webhook-triggered workflows. Owners, admins and creators can use the automation tools.

## How the agent builds an automation

1. **Create** the automation (`automation_create`). This creates a draft and returns its `automationId`.
2. **Set the trigger first** (`automation_set_trigger`):
   * **Scheduler**: runs on a schedule
   * **WebHook**: runs when a request arrives. The agent includes a sample payload (body, and optionally query and headers) so later tasks can use its fields
3. **Add tasks** (`automation_add_tasks`). The agent first reads the task's schema (`automation_get_task_schema`), then places each task after another task or inside a branch.
4. **Review** the flow with `automation_get_tasks` and `automation_validate`. To change it, the agent can update, move or remove tasks.
5. **Test** it (see below).
6. **Publish** it (`automation_publish`).
7. **Turn it on** (`automation_set_active`).

Edits stay in the draft until you publish. The first published version is `0.0.1`, and each publish after that increases the patch number unless you choose a version.

## Testing without side effects

There are two kinds of test run:

| Tools | Tasks that change data |
| --- | --- |
| `automation_test_run`, `automation_test_task` | **Skipped**. Safe to run any time |
| `automation_test_run_write`, `automation_test_task_write` | **Run**. They really write to databases, send emails or call APIs |

Ask the agent to test with writes only when you're ready for real changes. Test runs use your first non-production environment by default. Testing a whole automation needs the automations licence.

## Publishing

`automation_publish` checks the automation first. If validation finds errors, publishing stops and the agent shows you what's wrong. The agent can force a publish, but only do that when you understand the errors.

A published automation runs only when it's active. Use `automation_set_active` to turn it on or off.

## Other actions

| Tool | What it does |
| --- | --- |
| `automation_list` | Lists the workspace's automations |
| `automation_get` | Gets an automation's details |
| `automation_rename` | Renames it |
| `automation_duplicate` | Copies it |
| `automation_delete` | Deletes it permanently |
