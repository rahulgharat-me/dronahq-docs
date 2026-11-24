---
sidebar_position: 5
title: "Preview"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Preview

Choose **Preview** in the top bar to run the app inside the builder. **Preview** becomes available when the app's files have loaded and any save has finished.

<figure>
  <Thumbnail src="/img/dronahq-mcp/support-ticket.png" alt="A Vibe app running in Preview" />
  <figcaption align="center"><i>A Vibe app running in Preview</i></figcaption>
</figure>

:::caution Preview uses real data
The app runs against the selected [environment](#switch-environment). Adding, changing or deleting records in Preview changes them in that environment.
:::

## Reload

Click the reload icon to run the app again from the start. If you edit code and come back to Preview, it reloads by itself. On plans that measure usage in tasks, each reload [uses a task](./chat.md#credits-and-tasks).

## Desktop and mobile

Use the desktop and mobile icons in the footer to change the preview size. Mobile is 414 pixels wide.

## Switch environment

The environment selector, at the left of the footer, sets which [data environment](../datasource-concepts/data-environments.md) the app's connectors use, such as **Production**, **Staging** or **Development**.

1. Click the environment name in the footer.
2. Choose an environment. You see "Environment Updated".

If Preview is open, the app restarts with the new environment. If you're in Code View, it reloads the next time you open Preview.

The environment is part of the app's settings, so save the app to keep your choice.

<figure>
  <Thumbnail src="/img/vibe-apps/environment.png" alt="The environment selector in the footer" width="60%" />
  <figcaption align="center"><i>The environment selector in the footer</i></figcaption>
</figure>

## Preview as another user

Check what a particular person sees, for example to test screens that depend on their groups.

1. In Preview, click your name in the footer.
2. Search for a user and choose them.

The app restarts with that person's name, email and groups. The network log is cleared, so you see only the calls made after the switch.

To go back, choose yourself from the list. Opening the app again also resets it.

Some things to know:

* This changes who the *app* thinks is using it. It doesn't sign you in as that person or change your own access in Studio.
* If you're not a super admin, you see only users in the groups assigned to you.
* The list shows up to 20 users. Search to find others.

<figure>
  <Thumbnail src="/img/vibe-apps/preview-as-user.png" alt="Choosing a user to preview as" width="70%" />
  <figcaption align="center"><i>Choosing a user to preview as</i></figcaption>
</figure>

## Select an element

Use the pointer icon next to the reload icon to go from something on screen to its code, or to ask the AI to change it.

1. Click the pointer icon. Elements are outlined as you move over them.
2. Click an element.
3. Choose one:
   * **Open in code** switches to Code View and finds the element's code
   * **Add to chat** adds the element to your prompt, so you can write, for example, "make this button red"

To leave this mode without choosing, click the pointer icon again.
