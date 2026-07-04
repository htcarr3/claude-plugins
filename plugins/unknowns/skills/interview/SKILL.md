---
name: Interview Me
description: Interview the user one question at a time to resolve ambiguity before implementation, prioritizing questions whose answers would change the architecture. Use when the user asks to be interviewed, or to pin down an underspecified feature before building.
when_to_use: >-
  After brainstorming, when unknowns remain, or when a feature is underspecified
  and you need the user's intent before writing code. Trigger phrases include
  "interview me", "ask me questions", "help me pin this down".
disable-model-invocation: true
argument-hint: "[feature or problem to pin down]"
---

# Interview Me

Interview the user to convert their **known unknowns** and hidden assumptions
into a clear spec. The subject: **$ARGUMENTS**

## How to run it

- **One question at a time.** Ask, wait for the answer, then ask the next. Never
  batch a numbered list — the point is to let each answer steer the next
  question.
- **Prioritize ruthlessly.** Lead with questions where a different answer would
  change the architecture, the data model, or the user-facing shape. Skip
  anything you can safely assume with an industry default (state the default
  instead of asking).
- **Follow the surprises.** When an answer reveals a new ambiguity or
  contradicts an earlier assumption, chase it before moving on.
- **Surface unknown knowns.** Ask the "obvious" questions the user would never
  write down but would have strong opinions on once asked.

Before starting, take one line to state what you already understand, so you
don't ask what's settled.

## When to stop

Stop when the remaining questions are only mechanical details you can decide
yourself. Then produce a short spec: the decisions made, the assumptions you're
proceeding on, and any deliberately-deferred questions. Offer to hand that spec
to `plan-unknowns`.
