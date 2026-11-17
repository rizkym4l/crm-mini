# Workflow: how I build crm-mini

Related: [PRD.md](./PRD.md), [DESIGN.md](./DESIGN.md), [TASKS.md](./TASKS.md)

I work alone, with AI. AI writes most of the code. I run it, check it, and make sure I understand it. I type code myself when I experiment.

## The steps

```
1. Pick a task → 2. Design doc → 3. Self-review → 4. Critical review → 5. Build and test → 6. PR and merge
                      ↑______________↓    ↑______________↓                               ↑______↓
```

### 1. Pick a task

Take the next unchecked task in [TASKS.md](./TASKS.md). One task at a time.

### 2. Design doc

The design for the MVP already exists: PRD (what and why) and DESIGN (how). For a new feature that is not in DESIGN yet, ask AI to add a short section to DESIGN.md first: the solution, the files it touches, and the data it changes.

### 3. Self-review

Read the design. Ask AI to fix anything unclear or wrong. Repeat until I can explain the solution in my own words.

### 4. Critical review (instead of a senior)

I have no senior, so I use reviewers that do not simply agree with me:

- **A fresh AI session** that did not write the doc. Ask it to find weak points and to ask me questions, not to approve.
- **A one-day pause** for big designs. Read it again the next day.
- Answer every question. Update the doc. Repeat until there are no open questions.

### 5. Build and test

AI writes the code for **one task**. Then:

1. **I run it** and check the task's "Done when" myself.
2. **Explain-back test:** AI asks me 1–2 short questions about the code. If I cannot answer, we go over it before moving on.
3. **Break it on purpose:** I change one thing by hand (remove a tool description, change temperature, send bad input) and see what happens.
4. **Dev testing:**
   - Logic and tools: `bun test`
   - Backend API (`/api/*`): Postman or `curl`
   - Web pages (Phase 3+): Playwright

### 6. PR and merge

- One branch per phase, for example `phase-0-setup`.
- Open a PR to my own repo.
- Run `/code-review` on the diff. Fix what is real, write down why I skip the rest.
- Merge when the phase's "Done when" checks pass.

Small doc changes can go straight to `main`.

## Who types what

| Part | Who writes it | My job |
|---|---|---|
| Setup, config, database, web pages | AI | Read it quickly, run it |
| AI core: `llm.ts`, `tools.ts`, `loop.ts`, prompt | AI | Explain it back, experiment with it |
| Experiments | Me | Change one thing at a time |

## Rules

- Small steps. Never a whole phase in one go.
- "It runs" is not enough. I must be able to explain why it works.
- Prices, stock, and order status are enforced by code, never by the model.
