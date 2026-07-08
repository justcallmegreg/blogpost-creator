---
title: "Turning \"Make It Sound Good\" Into Rules a Machine Can Follow"
subtitle: "A promo-writing skill kept producing generic text from instructions like \"make it professional.\" The fix: replace vague tone words with rules you can actually check."
date: "2026-07-08"
audience: "People who build with LLMs or write technical content"
tags: [llm, prompting, technical-writing, skills]
repository: https://github.com/justcallmegreg/blogpost-creator
---

*A promo-writing skill kept producing generic text from instructions like "make it professional." The fix: replace vague tone words with rules you can actually check.*

## Problem Statement

The skill writes a blog post, then two short promos to announce it. The promo instructions were vague — "professional," "relaxed," "tasteful emojis." Vague words don't constrain a model, so the output kept drifting to the same flat announcement. Anyone who has told an AI to "make it sound good" knows the result.

## Solution

Replace the adjectives with rules you can check:

- **Ban the tells.** No hype words like "amazing" or "game-changer" — the energy comes from the problem, not from praise.
- **Use numbers, not adjectives.** "0–2 emojis," not "tasteful." "120–200 words," not "a bit longer."
- **Frame it as a small investigation** — here was a puzzle, here is what worked — without naming the steps.
- **End with a plain-language pass** — state the one core idea in everyday terms, then strip the jargon.

## Considerations Behind the Solution

Every rule had to pass one test: can you check it? "Professional" can't be verified. "0–2 emojis" and "no hype words" can. The investigation framing stays a mindset, not a template, so it bends to any project instead of forcing one shape.

## Conclusions

The move is transferable: turn subjective quality into constraints you can count or search for. It works for any prompt where the real requirement is just "good."

## Why Should You Adopt It?

Checkable rules give steadier output and faster review. They also make disagreements concrete — you argue about a number, not a vibe.

## How Should You Adopt It?

Take one vague instruction, like "make it engaging." Write down what it bans, what it rewards, and one number that bounds it. Add a check a reader could verify. Repeat for each fuzzy word.

Source: https://github.com/justcallmegreg/blogpost-creator
