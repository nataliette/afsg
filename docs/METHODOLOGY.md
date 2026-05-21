# METHODOLOGY.md — Equipment Gym Tracker

Captures patterns in how you direct AI-assisted builds.

**This is observational** — written by Claude, about your working style. **Never rewritten, only added to.** Patterns documented here feed into how Claude approaches future sessions.

---

## Observed patterns (updated per session)

### Pattern: Quality verification before presentation

Test UI features internally before reporting completion. Don't prompt the user to check preview links — they'll navigate to the server themselves.

**Why:** Respects their time and flow. Testing is part of the work, not a handoff step.

### Pattern: Prefer semantic, flexible markup

Use CSS Grid layouts (divs) over HTML tables for all result displays. When you say "table," you mean the grid layout, not the HTML element.

**Why:** Better semantics, responsive flexibility, easier styling.

### Pattern: Code safety before adding features

When adding new gyms to equipment.json, immediately add GYM_INITIALS entries. Missing entries crash the app at runtime.

**Why:** Prevents silent failures and the need for debugging later.

### Pattern: Preference for structured rituals and consistency

You use explicit, repeatable workflows (session close ceremony, git commit conventions, structured documentation). This suggests you value clarity, maintainability, and the ability to retrospect on decisions over time. When proposing approaches, favor explicit structure and consistency over ad-hoc or flexible solutions.

**Why:** Enables better retrospection and pattern discovery across projects.

---

## How Claude uses this

After each session, Claude reflects on how you:
- Make decisions (data-driven? intuition? constraints-first?)
- Prioritize work (scope-first? quality-first? velocity-first?)
- Give feedback (high-level direction? specific interactions? code review?)
- Handle tradeoffs (which constraints matter most?)
- Communicate (talk-through? written briefs? live feedback?)

These patterns help Claude anticipate your preferences and work more naturally with your style across sessions.

---

## Examples of patterns to capture:
- How you scope work (breadth vs. depth, MVP focus, etc.)
- How you validate changes (user test, visual review, code inspection?)
- What "done" means to you (feature-complete, polished, shipped?)
- Your decision-making style (explicit vs. iterative, risk-averse vs. pragmatic?)
- How you communicate constraints (hard deadlines, aesthetic non-negotiables, technical requirements?)
