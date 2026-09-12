# Run R2: AI economy, model routing, and boilerplate instead of regeneration

## Decision to inform

Should Archetype add a convention on efficient AI usage: route task classes to model tiers (cheap for mechanical edits, tests, research sweeps, and summaries; strong for design, security, and audit), keep a session and daily token budget so the user is never locked out of their plan for hours, prefer stored boilerplate over regenerating the same files in every project, and show the user what the AI spent and did. The founder's words: "efficiency in AI usage since it is very important lately, making use of the plans, considering not to max out hourly tokens and locking the user out for a few hours just because the AI decided to develop something not necessary that could have been saved in the boilerplate" and "trying to make use of cheap models whenever possible for small tasks".

## What exists today (verify by reading)

- Nothing on model tiers, token budgets, rate limits, or cheap-first anywhere in dist/ (the Claude session grepped for these words and found none; confirm).
- dist/conventions/17-context.md: file size, session hygiene, lost-in-the-middle.
- dist/conventions/00-reusability.md: one source of truth when a shape appears twice.
- dist/scaffolding/*.md: scaffolding is prose that the AI executes afresh for every project, so identical files are regenerated each time, paying tokens each time.
- dist/development/CREATING-FORKABLE-TEMPLATES.md: exists but nothing links to it (an orphan).
- dist/templates/pulse-monitor-spec.md: a read-only dashboard with a versioned data contract; today it shows code state only, nothing about AI activity or spend.
- Founder directive recorded in shared memory: do the work directly by default, few agents, strong model for agents; never relay an agent report unread.

## Questions to answer

1. Durable principles for governing the cost of AI-assisted development, by analogy with cloud cost governance and with batch or build-farm scheduling: budgets, allocation to work items, showback, caps, and kill switches. Which carry over, which do not?
2. Model routing: what is the evidence that routing or cascading across model tiers works for coding tasks, where it fails (silent errors by a cheap tier), and what verification must scale up when a cheaper tier did the work? Express the classes of work (mechanical edit, test writing, research sweep, summarisation, design, security review, independent audit) and the properties a tier must have for each, never a model name outside Research Notes.
3. Plan limits: how can a framework that names no vendor still instruct the AI to respect hourly and weekly usage windows, keep headroom for the user's own interactive work, and pause instead of exhausting the allowance? What must the host map (the framework states the property, the host supplies the number)?
4. Boilerplate over regeneration: design the rule "if it is identical in every project of this shape, it lives in a starter, not in the AI's output". How do forkable starters per stack shape stay timeless (the framework knows the shape categories, the starters are dated and live outside the framework)? How does the scaffold phase choose between clone, generate, or a mix? Read dist/scaffolding/SCAFFOLD.md and dist/development/CREATING-FORKABLE-TEMPLATES.md first.
5. Measurement and visibility: what should be recorded per feature (tokens, wall time, model tier used, retries) and where; how would an "AI activity" section extend the pulse data contract (dist/templates/pulse-monitor-spec.md) without breaking the stability rule?
6. Plain-language preferences: how does a non-technical rider set the speed versus cost versus thoroughness balance in words they understand, and how does the AI translate that into routing and budget decisions?
7. Mechanical checks: what can a script verify about this convention (for example, a spend record exists per session, a budget cap is declared, a starter is referenced instead of regenerated)? Mark mechanical versus judgment.

## Extra output for this run

A "Tiny version" section: the smallest change (one to two days) that captures most of the value, versus the full design.
