---
sidebar_position: 7
title: "Versions and Publishing"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Versions and publishing

Saving, adding a version and publishing are separate steps:

| Step | What it does | Who sees the result |
| --- | --- | --- |
| **Save** | Keeps your work | People who can open the app in Studio |
| **Add Version** | Keeps a snapshot you can go back to | People who can open the app in Studio |
| **Publish** | Releases a new version | Your users |

## Add a version

1. Save the app, then click **Versioning** in the top bar.
2. Choose the **Release type**: **Major**, **Minor** or **Patch**. The next version number is shown with each.
3. Enter **Release notes**. They're required.
4. Click **Add Version**.

Adding a version doesn't publish it. **Last live version**, in the same panel, shows the last version you published.

<figure>
  <Thumbnail src="/img/vibe-apps/versioning.png" alt="The Versioning panel" width="60%" />
  <figcaption align="center"><i>The Versioning panel</i></figcaption>
</figure>

## Go back to an earlier version

1. Click **Versioning** and find the version under **Version history**.
2. Click the **Rollback** icon, then **Revert**.
3. Check the app in Preview.

Deleting a version removes its stored files. A version marked **Files deleted** can't be restored.

To undo only a recent AI change, use [Revert in the chat](./chat.md#undo-an-ai-change).

## Publish

1. Save the app and click **Publish & Share**.
2. Choose the **Release Type** and enter **Release notes**. Both are required. Release notes can't contain special characters.
3. To release only to users tagged as Beta, turn on **Release in Beta Only**. Everyone else keeps the version published before. See [Releasing in Beta](../releasing-in-beta.md).
4. Under **Select Catalogue**, choose where the app appears for your users.
5. Click **Publish**.

You need **Publisher** access to the app. See [App permissions](../building-apps-concepts/app-permissions.md).

<figure>
  <Thumbnail src="/img/vibe-apps/publish-and-share.png" alt="The Publish & Share panel" width="90%" />
  <figcaption align="center"><i>The Publish & Share panel</i></figcaption>
</figure>

## Choose who can open the app

Under **Access** in **Publish & Share**:

| Access | Who can open it |
| --- | --- |
| **Private** | Only users you choose. Use **Add Users** to add someone by name and email, or **Manage Users** to open user management |
| **Public** | Anyone with the link |

For a public app, you can also:

* Copy the **App URL**, or **Set friendly name** to make it easier to read
* Copy the **Embed Code** to show the app on another site
* Use **Setup message passing** for an embedded app
* Turn on **Allow only verified/logged in users to access this link (including secure embed)** to require sign-in

The URL, friendly name, embed code and message passing options appear after the first publish.

## End user portal

**End User Portal**, in the same panel, has links to open your apps as an end user on **Web**, **iOS** and **Android**.

To publish from an AI client, see [Publishing and users](../dronahq-mcp/publishing-and-users.md).
