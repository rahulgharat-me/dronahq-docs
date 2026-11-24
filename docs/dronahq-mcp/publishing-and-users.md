---
sidebar_position: 6
title: "Publishing and Users"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Publishing and users

From your AI client you can publish an app, decide where it appears for your users, and manage who can sign in.

## Publish an app

Ask the agent to publish. It calls `vibe_publish`, which:

* Releases the next version (the patch number goes up automatically) and marks the app live
* Adds the app to a catalogue, so your users can find it
* Can turn on the app's public link, and require sign-in on it

| Option | What it does |
| --- | --- |
| Release notes | A note saved with the version |
| Catalogue | Where the app appears. Owners and admins default to **Master Template**. When a creator publishes, the app stays in its current catalogue |
| Public link | Turns the app's public link on or off |
| Require login | Only signed-in, verified users can open the public link. Matches **Allow only verified/logged in users to access this link** in Studio. Needs the secure embed licence. Without it, the app still publishes, but this option isn't applied |

You need **Publisher** access to the app. Publishing fails with "App publish license limit reached" if your plan's published app limit is reached.

### Change public link access later

`vibe_set_access` changes who can open an already published app's public link:

* **Public** (`isPublic: true`): anyone with the link can open it
* **Not public** (`isPublic: false`): visitors must sign in

It doesn't turn the public link itself on or off. Ask the agent to publish with the public link option for that. `vibe_set_access` needs **Publisher** access to the app and the secure embed licence.

### Check in a version without publishing

`vibe_check_in` saves a snapshot of the app as a version (Studio's **Versioning → Add Version**), without publishing it. Choose a major, minor or patch version. The default is patch. The agent asks before checking in. Save the app first, or the check-in fails with "App form data not found".

## Catalogues

Catalogues decide which groups of users see which published apps.

* New workspaces start with **Master Template** plus **Sales**, **HR**, **Marketing** and **Finance**, each assigned to its matching group. Older workspaces may have only Master Template.
* The agent lists the catalogues you can see, with the groups assigned to each, before choosing one.

| Action | Tool | Who |
| --- | --- | --- |
| List catalogues | `vibe_list_catalogues` | Everyone who can connect |
| See a catalogue's groups and users | `vibe_get_catalogue_users` | Everyone who can connect |
| Create a catalogue | `vibe_create_catalogue` | Owners and super admins |
| Add groups to a catalogue | `vibe_add_catalogue_groups` | Owners and admins |
| Replace a catalogue's groups | `vibe_set_catalogue_groups` | Owners and admins |

When you create a catalogue, DronaHQ checks whether one with the same set of groups already exists. If it does, it uses that catalogue instead of creating a duplicate. Removing every group from a catalogue needs an explicit confirmation.

## Groups and inviting people

Owners and admins can manage groups and invite people from their AI client:

| Tool | What it does |
| --- | --- |
| `vibe_list_groups` | Lists the groups you manage |
| `vibe_create_group` | Creates a group |
| `vibe_check_users` | Checks whether email addresses already exist and can be invited |
| `vibe_add_user_to_group` | Invites a new user, or adds an existing user to groups |

When you add an existing user to groups, their current groups are kept unless you ask to replace them. New users get an invite email unless you ask the agent not to send one. Limited admins can only use the groups assigned to them.

## User accounts

### Deactivate and reactivate

Owners and admins can deactivate and reactivate accounts with `vibe_deactivate_user` and `vibe_reactivate_user`.

* **Deactivating** signs the person out and blocks their access, including any AI client they connected. They keep their groups and data, and can be reactivated later.
* **Reactivating** restores access and uses one user licence. If none is free, it fails with "User license limit reached".

Rules:

* You can't change your own account or the workspace owner's.
* Only the owner or a super admin can deactivate or reactivate an admin or a creator.
* Limited admins can only change end users in the groups they manage.

### Delete permanently

Owners and super admins can permanently delete a user with `vibe_delete_user`. This is available only through MCP. There's no button for it in Studio.

:::danger This can't be undone
The agent must confirm the email with you, and the call must include `confirm: true`. The user must already be deactivated.
:::

When a user is deleted:

| | What happens |
| --- | --- |
| **Moves to the workspace owner** | Apps, forms, automations, connectors, AI and Data Agents, MCP servers and API keys they owned |
| **Removed** | Their workspace membership, groups, app and connector permissions, admin role, sessions, MCP access, and unsent form drafts |
| **Kept** | Submitted form responses, login history and audit logs |
| **Their login** | Deleted only if they don't belong to any other workspace |

You can't delete yourself or the workspace owner. If anything fails, nothing is changed. The deletion is recorded in the audit log.
