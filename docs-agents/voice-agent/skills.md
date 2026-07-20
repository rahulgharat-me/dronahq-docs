---
sidebar_position: 7
title: "Skills"
pagination_prev: null
---
import VideoEmbed from "@site/src/components/VideoEmbed";
import Image from "@site/src/components/Image";
import VersionedLink from '@site/src/components/VersionedLink';
import ArrowRight from '@site/static/icons/arrow_right.svg';
import Thumbnail from '@site/src/components/Thumbnail';

# Overview

A Skill is a self-contained capability you attach to a Voice Agent, each Skill defines one specific job the agent can do (e.g. checking an order status, booking a slot, raising a ticket), the condition under which it should be used, the tools it's allowed to call to get it done, and when that job counts as finished.

Rather than writing one large prompt that tries to handle every situation, Skills let you break an agent's behavior into focused units. During a call, the agent picks the right Skill for what the caller is asking, uses only the tools relevant to that Skill, and exits cleanly once the goal is met, instead of the conversation drifting or the agent trying to do everything at once.

<figure align ="center">
  <Thumbnail src="/img/agent-voice-skills/skills.png" alt="Adding skills to the agent"  />
  <figcaption align ="center"><i>Adding Skills </i></figcaption>
</figure>

## Skill Fields

| Field | Description |
|-------|-------------|
| **Name** | The identifier for the Skill, used to reference it internally (e.g. `skill_1`). |
| **Trigger** | Defines when this Skill should be selected, based on what the user is asking for in the conversation. |
| **Goal** | Describes the task this Skill is meant to accomplish. |
| **Exit When** | Defines the condition under which the Skill is considered complete — e.g. once its objective is met and the required information has been given to the user. |
| **Outputs** | Data the skill collects before returning. Captured to agent attributes and replayed in chat history when the skill exits. Multiple outputs can be added. |
| **Tools** | External actions available only while this Skill is active. |
