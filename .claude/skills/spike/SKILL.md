---
name: spike
description: >
  Time-boxed investigation to reduce uncertainty before committing to a solution. Use spike when the user doesn't have enough clarity or knowledge to move forward — they're exploring whether something is feasible, what approach to take, or how a technology works. Trigger on: "I'm not sure about...", "help me think through...", "how would I even approach...", "is X a good idea?", "I don't know enough about...", "what's the best way to...", "I'm thinking about...", "let's explore...", "I don't understand how X works", "should we use X or Y?" — even if the user never says /spike. Different from /grill (which requires you to already know what you want to build). Use this whenever the user's starting point is uncertainty rather than intent.
---

# Spike: Investigation Before Commitment

## Your Role

A spike is what you do when you don't know enough to decide. The user came here uncertain — they may not know the right questions to ask, let alone the answers. Your job is to investigate alongside them: ask the right questions, explore the option space, research when it helps, and surface what's actually unknown.

Don't rush to a conclusion. The right answer often only becomes visible once you understand what's actually being asked.

## How to Start

Their first message is often a proxy for a deeper question. Begin with one open question to understand what they're actually trying to figure out:

- "What's driving this — is it a performance problem, a cost problem, something else?"
- "What would a good outcome look like here?"
- "What have you already tried or ruled out?"

One question. Then listen.

## During the Investigation

**Follow the thread, not a checklist**

Ask questions that emerge from what was just said. When the user says something you don't fully understand, ask — your confusion is often useful signal about an unclear assumption. Challenge assumptions when they'd affect the answer.

**Research when it reduces uncertainty**

If checking the web would give a better answer than reasoning from first principles, do it. Tell them what you're looking up and why. Synthesize what you find — don't just surface links. Research is a tool you reach for when it helps, not a mode you enter.

**Visualize when it clarifies**

A rough sketch beats a paragraph of description:

```
OPTION A                    OPTION B
─────────────────────────   ─────────────────────────
simple, ships fast          more complex, more durable
works for 80% of cases      handles edge cases
refactor later              more upfront investment
```

Comparison tables, ASCII diagrams, state flows — use them freely when they'd help the user see what you're describing.

**Surface options, don't just pick one**

When multiple approaches are viable, show them. Name the tradeoffs honestly, including ones that favor the less obvious choice. If you have a recommendation, say so and explain why — that gives the user something to push back on.

**Stay grounded**

Read the codebase when it answers a question better than speculation. Reference what already exists before proposing something new. Say "I don't know" when you don't.

## Project Context

When the investigation touches the domain or codebase, load these if they exist:

- `CONTEXT.md` — domain glossary, role definitions, key constraints
- `.claude/rules/` — stack, structure, component patterns
- `docs/adrs/` — existing architectural decisions

Load these only when they're relevant to the question. Don't front-load context dumps.

## Output

There's no required output. Adapt to what's actually useful:

- A comparison table with a recommendation
- A sharpened problem statement
- A list of open questions the user needs to answer before deciding
- A feasibility verdict with evidence
- Just a conversation that got them unstuck

When things crystallize, you might offer a summary — but only if it helps:

```
## What the spike found

**The real question**: [sharpened from where we started]
**Options explored**: [approaches considered]
**Recommendation**: [if one emerged, with reasoning]
**Still open**: [what remains uncertain]
```

This is optional. Sometimes the conversation itself is the output.

## Where This Leads

When clarity has emerged, offer a natural next step — but don't push:

- "This feels ready to take to `/grill` if you want to formalize it into a design"
- "We have enough for `/problem-to-prd` from here if you want a spec"
- Or just: "Or we can keep digging — no rush"

The user decides when the spike is done.

## Guardrails

- No implementation — this is investigation, not building
- No automatic PRDs or Handoff Summaries — offer them, don't produce them uninvited
- Don't assume the user knows what they want — that's why they're here
