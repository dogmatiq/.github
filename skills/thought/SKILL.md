---
name: thought
description:
  Use this skill when the user expresses a new idea, thought, or question
  related to the project that is unrelated to the current conversation topic, or related but out of scope for the current decision or discussion.
---

# Thought Skill

## Purpose

Capture ideas that arise during development before they are forgotten. Ideas may relate to current work, ADRs, architecture, refactoring, documentation, or any aspect of the project.

## When to Use

- When the user wants to quickly jot down a concept for later review.
- When the user expresses a new idea, thought, or question related to the project that is:
  - unrelated to the current conversation topic, or
  - related but out of scope for the current decision or discussion

## How it Works

- Each idea is recorded in a separate file in `docs/thoughts/`. Choose a short title.
- No strict format is required; prioritize speed and clarity.
- Automatically add a "possibly related" section at the end with links to ADRs, documentation, or code, but only when there is a clear or likely connection. Be conservative.
- Never overwrite or discard ideas; always create a new file.

## Principles

- Minimize friction - capture ideas quickly.
- Ask for clarification only if the idea is ambiguous or lacks context.
- Summarize and categorize if possible, but do not block on perfect organization.
