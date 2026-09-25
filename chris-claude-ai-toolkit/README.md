# Chris's Claude Code Learning Toolkit

A customization that turns Claude Code into a **learning companion** instead of a code generator. It makes you think, predict, and reason — while Claude guides, questions, and diagnoses.

**Core idea:** You try first. Claude helps only after you commit to an answer.

> This is the Claude Code port of [chris-copilot-ai-toolkit](../chris-copilot-ai-toolkit), adapted from GitHub Copilot's `.github/prompts` + `.github/skills` format to Claude Code's `.claude/commands` + `.claude/skills` format. The two folders are independent copies — changes to one don't propagate to the other.

---

## How It Works

```
You try → You predict → Claude gives hints → You implement → You test → You explain → Claude reviews → You retrieve later
```

Claude never writes the solution for you. It asks questions, gives small hints, and tests your understanding. The harder you think, the more you learn.

---

## Quick Start

1. **Open this folder in Claude Code** (or copy it into your project).
2. **Type a slash command** like `/hint`, `/debug`, `/explain`, etc.
3. **Answer the questions** Claude asks you. Don't skip them — that's where the learning happens.
4. **Write the code yourself.** Claude will guide, not write.

---

## The 12 Commands

Each command is a workflow under `.claude/commands/`. Invoke it with `/command-name` in Claude Code.

### `/learn`
**The entry point.** Tell it what you want to learn, and it picks the right workflow for you. Use this when you're not sure which command to use. If it still can't tell, it defaults to `/hint`.

**Use when:** You're starting a session and don't know where to begin.

---

### `/hint`
**Your main problem-solving tool.** First, you predict what should happen. Then, Claude gives you the smallest hint needed — one level at a time, from a question all the way up to full code (only if you ask for it).

**Use when:** You're stuck on a problem, bug, or concept and want incremental help without being handed the answer.

---

### `/debug`
**Bug diagnosis through hypothesis testing.** Claude asks what you expected, what happened, and what you think is wrong — then guides you to find the bug yourself.

**Use when:** Your code doesn't work and you want to understand *why*, not just get a fix.

---

### `/autopsy`
**Post-mortem after fixing a bug.** You fill in what happened, what you believed, what actually happened, and what you missed. Claude classifies the bug and suggests prevention.

**Use when:** You just fixed a non-trivial bug and want to learn from it so it doesn't happen again.

---

### `/read`
**Understand unfamiliar code.** Claude asks you concrete questions one at a time: "What does this input produce?" "What happens after iteration 3?" You reconstruct the mental model yourself.

**Use when:** You're reading someone else's code, old code, framework internals, or library source.

---

### `/code-review`
**Educational code review.** Before Claude reviews your code, you identify the weakest part yourself. Then Claude labels issues by severity and asks discovery questions instead of rewriting your code.

**Use when:** You've written something and want a senior-developer-style review that teaches you.

---

### `/test`
**Design tests before implementing.** You state the contract, identify the smallest passing/failing inputs, and derive test cases. Claude exposes ambiguities in your spec.

**Use when:** You're about to implement a feature and want to think through edge cases first.

---

### `/explore`
**Explore design space.** You defend your current solution, Claude offers conceptually different alternatives, then introduces constraints one at a time to stress-test your design.

**Use when:** You have a working solution and want to understand tradeoffs, or when you want to practice handling real-world constraints like scale, concurrency, or failure.

---

### `/arch`
**Architecture interview.** Claude asks one focused question at a time about requirements, state, interfaces, failure modes, etc. You sketch the design; Claude challenges your assumptions.

**Use when:** You're designing a non-trivial feature and want to think through it before coding.

---

### `/explain`
**Teach-back test.** You explain a concept in your own words, as if teaching a beginner. Claude probes for gaps, vague terms, and contradictions — then gives a concise assessment.

**Use when:** You think you understand something and want to verify it.

---

### `/retrieve`
**Spaced retrieval practice.** Claude reads `learning/review.md`, prioritizes topics that are due, and asks you a short mix of prediction, debugging, and application questions from past material.

**Use when:** You're starting a coding session and want a quick warm-up to reinforce what you've learned.

---

### `/api`
**Learn an API deeply.** Claude walks you through 7 questions: what problem it solves, what assumptions it makes, what alternatives exist, when *not* to use it, and its failure modes. Then it points you to official docs.

**Use when:** You encounter a new library, framework, or API and want real understanding, not just syntax.

---

## The 3 Skills

Skills live under `.claude/skills/` and are reusable behaviors Claude can invoke automatically when relevant — you don't invoke them directly.

| Skill | What it does |
|---|---|
| **debugging** | Separates expected vs actual behavior, requires a hypothesis before suggesting causes, picks the smallest next experiment. |
| **examination** | Tests understanding through prediction, explanation, application, and transfer. Corrects the smallest misconception first. |
| **retrieval** | Mixes recent and older material, prefers prediction over definitions, adapts difficulty to your performance. |

There is no separate `code-review` skill — its behavior lives entirely in the `/code-review` command to avoid duplicating the same rules in two places.

---

## Learning Logs (Optional but wired in)

A lightweight folder for recording meaningful learning events.

```
learning/
├── mistakes.md    # Bugs, misconceptions, recurring patterns
├── concepts.md    # Durable understanding worth keeping
├── questions.md   # Unresolved questions to revisit
└── review.md      # Retrieval scheduling: difficulty, confidence, next-review date
```

Every entry across all four files carries a **Topic** tag, reused across files so `/retrieve` can cross-reference related mistakes, concepts, and open questions for the same idea. Commands that surface something worth remembering (`/hint`, `/debug`, `/autopsy`, `/code-review`, `/explain`, `/test`) draft the log entry themselves and ask you to confirm it — logging isn't left as homework.

`/retrieve` reads `review.md` to decide what's due and schedules the next review after each session.

---

## Adaptive Behavior

The toolkit adapts to you, **per topic** — struggling with recursion doesn't make Claude go easy on you for loops, and vice versa:

- **If you're solving a topic easily:** Claude asks deeper "why" questions, introduces harder constraints, and reduces unnecessary prompting — for that topic.
- **If you're struggling with a topic:** Claude lowers the hint level, revisits prerequisites, and creates targeted practice — without just giving you the answer.
- **When a topic comes back after a gap:** difficulty resets to moderate rather than picking up wherever it left off, so understanding gets re-established before ramping up again.

---

## Enforcement

Unlike a plain system prompt, `CLAUDE.md` includes an explicit gate Claude checks before every response in a learning workflow: has a prediction been stated, is the hint level being escalated by more than one step, is a full implementation being shown without being earned. If any check fails, Claude asks the missing question instead of answering.

---

## Shipping Mode

This toolkit is for learning. When you need to ship:

> Just say **"ship this"** or ask for a direct implementation.

Claude will switch to normal engineering mode. The learning rules only apply when you invoke a learning command.

---

## How It's Organized

```
CLAUDE.md                      # Global learning philosophy, enforcement gate, and learning-log rules
.claude/
├── commands/                  # 12 user-facing workflows (slash commands)
└── skills/                    # 3 reusable Claude behaviors
learning/                      # Learning logs
```

---

## Requirements

- **Claude Code** in your terminal, IDE extension, or desktop app
- No other dependencies, scripts, or setup needed

---

## Limitations

- **Learning logs** need `learning/` to exist with the templates in this repo; they're not auto-created.
- This is a prompt-based toolkit, not an app. It works through Claude Code conversations and slash commands.

---

## Philosophy

> Make AI reduce the friction around learning programming **without reducing the amount of thinking the learner has to do.**

If you're not thinking, you're not learning.
