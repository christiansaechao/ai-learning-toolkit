# AI Learning Toolkit

Turns your AI coding assistant into a tutor: it asks questions and gives hints instead of writing the answer. You try first, predict what should happen, then get help one small step at a time.

Two versions, same behavior:

| Folder | Use with |
|---|---|
| `chris-claude-ai-toolkit/` | Claude Code |
| `chris-copilot-ai-toolkit/` | GitHub Copilot Chat |

## Setup (30 seconds)

1. Open the folder matching your tool (above) in your editor.
2. That's it — commands are already wired up (`.claude/commands` or `.github/prompts`).

## How to use it

Type a slash command below into chat. Answer what it asks — that's where the learning happens. When you just want working code, say **"ship this"** and it'll switch to normal mode.

## Commands — what each one does, and when to use it

| Command | Use when... | What happens |
|---|---|---|
| `/learn` | You don't know where to start | Picks the right command for you (defaults to `/hint` if unclear) |
| `/hint` | You're stuck on a problem or bug | You predict first, then get hints one level at a time — question → concept → strategy → pseudocode → code |
| `/debug` | Something's broken and you want to understand why | Walks you through hypothesis → evidence → smallest next check, instead of handing you the fix |
| `/autopsy` | You just fixed a non-trivial bug | You fill in what you believed vs. what really happened; it classifies the mistake and logs it |
| `/read` | You're reading unfamiliar code | Asks you to trace execution step by step instead of explaining it to you |
| `/code-review` | You want your code reviewed | You spot the weakest part first, then it reviews and labels issues by severity — no auto-rewrite |
| `/test` | You're about to build something | You define the contract and edge cases before writing any implementation |
| `/explore` | You have a working solution and want to stress-test it | You defend your design, then it adds one constraint at a time (scale, concurrency, failure, etc.) |
| `/arch` | You're designing a non-trivial feature | One question at a time on requirements, interfaces, failure modes — before any code |
| `/explain` | You think you understand something | You teach it back in your own words; it finds the gaps |
| `/retrieve` | Starting a session, want a warm-up | Quizzes you on topics that are due for review, based on your `learning/` logs |
| `/api` | You're learning a new library/API | 7 guided questions on purpose, assumptions, tradeoffs, and failure modes |

Everything else (learning-log format, adaptive difficulty, the enforcement rules Claude/Copilot follow) is documented inside each tool's own folder — this page is just the fast path to get going.
