---
sidebar_position: 2
title: "Chat"
---
import Thumbnail from '@site/src/components/Thumbnail';

# Chat

Use the **Chat** view to build and change a Vibe app by describing what you want.

## Write a prompt

Type in the prompt box and press Enter. Press Shift+Enter for a new line. The agent can ask questions and propose user stories and a data schema for you to approve before it writes the code.

<!-- SCREENSHOT: vibe-chat.png — the Chat view with a prompt, user stories and the model picker -->

| To | Do this |
| --- | --- |
| Refer to a connector or service | Type `@` and pick it from the list |
| Start from a design | Attach or paste an image with your first prompt: JPG, PNG, TIFF or WebP, up to 10 MB each |
| Point at something on screen | Select it in Preview and choose **Add to chat**. See [Select an element](./preview.md#select-an-element) |
| Stop the agent | Click the stop button |

:::note Images work only in the first prompt
Once the app has code, you can't attach or paste more images.
:::

## Choose the AI model

The picker at the bottom of the prompt box sets what the chat runs on:

| Option | What it uses |
| --- | --- |
| **DronaHQ** | Your workspace's DronaHQ credits. DronaHQ chooses the model |
| **Bring Your Own Key** | A model from OpenAI, Anthropic or Google, with your own API key |
| **Other** | Your own endpoint. Enter its completion URL, model name and API key |

<figure>
  <Thumbnail src="/img/vibe-apps/model-picker.png" alt="The model picker in the prompt box" width="60%" />
  <figcaption align="center"><i>The model picker in the prompt box</i></figcaption>
</figure>

After you choose a model of your own, use the settings icon next to the picker to add or update its key. **Configure AI** has three steps: pick a provider, pick a model, then paste your key. If a key is already saved for that provider, you can choose **Use existing key**.

## Undo an AI change

After the AI changes your code, the chat can show **Version saved to history** with two buttons:

* **Revert** returns the app to that saved point, then reloads the page
* **Delete** removes the saved point

These saved points are for quick undo only. Your browser keeps the last two, so they aren't available on another computer, and starting a new thread clears them. To keep a version for longer, [add a version](./versions-and-publishing.md#add-a-version).

## Start a new thread

Click **+** in the top bar and confirm with **New Thread**. The current chat is cleared. Your app's code stays as it is.

## Credits and tasks

**Credits** pay for AI use when the model picker is set to **DronaHQ**. To see what's left, click the app name in the top bar.

**Tasks** apply on plans that measure usage in tasks. If they run out, the chat shows "Limit reached - Tasks exhausted" and a **Go to Billing** button. In the builder, these actions use tasks:

* Sending a prompt
* Saving a file
* Reloading the preview, including when it reloads by itself after you edit code

Building from an AI client uses tasks too. See [Usage](../dronahq-mcp/building-apps.md#usage).
