---
name: handoff
description: Write a compact context-handoff brief before clearing or compacting a session. Use when the user says /handoff, "hand off", "wrap up this session", or is about to run /clear and wants to continue the same work in a fresh session without losing the goal, decisions, and next step. Produces a paste-ready brief, not a running narration.
---

# handoff

Produce a tight handoff brief so work can continue in a **fresh** session after `/clear`
with almost none of the current context. The brief is the ONLY thing that survives, so it
must carry intent, not history.

## When to run

The user is about to `/clear` (or the context is getting expensive) and wants to keep going
in a clean session. Invoke this, hand them the brief, they copy it, `/clear`, and paste it as
the first message of the new session.

## What to gather (read the session, don't ask)

Reconstruct these from the conversation and the working tree — only ask the user if something
critical is genuinely unknowable:

1. **Goal** — the one-sentence objective of this work, in the user's terms.
2. **Current state** — what's done and verified vs. what's in progress right now.
3. **Key decisions** — choices already made and *why*, so the next session doesn't relitigate
   them (e.g. "using X not Y because Z").
4. **Changed files** — the files touched this session and, in a few words, what changed in each.
   Prefer `git status --short` + `git diff --stat` over memory.
5. **Next step** — the single most immediate action to take, concrete enough to start on.
6. **Gotchas** — anything that will bite the next session: a failing test, a flaky command, a
   thing that looks safe but isn't, a pinned version, an env requirement.

## Output format (this exact shape)

Output ONLY the brief, in a single fenced block the user can copy whole. No preamble, no
"here's your handoff". Keep it under ~300 words — a brief, not a transcript.

```md
# Handoff — <goal in one line>

## State
- Done: <verified-complete items>
- In progress: <what's mid-flight right now>

## Decisions
- <decision> — <why>
- <decision> — <why>

## Changed files
- `<path>` — <what changed>
- `<path>` — <what changed>

## Next step
<the single most immediate concrete action>

## Gotchas
- <thing that will bite the next session>

## To resume
Read the files above, confirm the state, then do the next step. Don't redo the decisions.
```

## Rules

- **Decisions over narration.** Record what was decided and why, not the play-by-play of how
  you got there. The next session doesn't need the journey.
- **Verify file claims.** Base "changed files" on `git status`/`git diff`, not recollection.
- **One next step, not a plan.** Name the single next action. The fresh session can plan from
  there.
- **Don't dump code.** Reference files by path; the next session can open them. Paste a snippet
  only if it's a specific value that would otherwise be lost (a chosen config line, an ID).
- **No secrets.** Never copy API keys, tokens, or `.env` values into the brief.
- **Self-contained.** Assume the reader has zero prior context. Spell out names the way the
  first message of a cold session would need them.
