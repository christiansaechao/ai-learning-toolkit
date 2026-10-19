# AI Learning Toolkit

Turns your AI coding assistant into a tutor: it asks questions and gives hints instead of writing the answer. You try first, predict what should happen, then get help one small step at a time.

Two versions, same behavior:

| Folder | Use with |
|---|---|
| `chris-claude-ai-toolkit/` | Claude Code |
| `chris-copilot-ai-toolkit/` | GitHub Copilot Chat |

## Setup

### What's in each folder

| Piece | Claude Code | Copilot | What it does |
|---|---|---|---|
| Rules / tutor personality | `CLAUDE.md` | `.github/copilot-instructions.md` | Tells the AI to coach instead of hand you answers |
| Slash commands | `.claude/commands/*.md` | `.github/prompts/*.prompt.md` | `/hint`, `/debug`, etc. |
| Skills | `.claude/skills/*/SKILL.md` | `.github/skills/*/SKILL.md` | Extra know-how the AI loads automatically when relevant |
| Your learning logs | `learning/*.md` | `learning/*.md` | Your mistakes, concepts, questions, review schedule |

The AI only finds these files if they're in the **right place**. You have two options:

### Option A — Per project (quickest, good for trying it out)

Copy the contents of your tool's folder into the **root** of the project you're working on (the folder you open in VS Code / start `claude` in):

```bash
# Claude Code — run from inside your project folder
cp -r path/to/learning-toolkit/chris-claude-ai-toolkit/{.claude,CLAUDE.md,learning} .

# Copilot — run from inside your project folder
cp -r path/to/learning-toolkit/chris-copilot-ai-toolkit/{.github,learning} .
```

> ⚠️ `cp -r folder/*` skips hidden folders like `.claude` and `.github`. Use the command above, or turn on "show hidden files" if dragging in a file explorer.

- ✅ Works immediately, nothing else to configure.
- ❌ You have to do it again for **every new project/repo**.
- ❌ If the project already has a `CLAUDE.md` or `copilot-instructions.md`, don't overwrite it — paste ours at the bottom instead.

### Option B — Global (install once, works in every project) — recommended

**Claude Code** — Claude reads `~/.claude/` in every project:

```bash
mkdir -p ~/.claude/commands ~/.claude/skills
cp path/to/learning-toolkit/chris-claude-ai-toolkit/.claude/commands/*.md ~/.claude/commands/
cp -r path/to/learning-toolkit/chris-claude-ai-toolkit/.claude/skills/* ~/.claude/skills/
cat path/to/learning-toolkit/chris-claude-ai-toolkit/CLAUDE.md >> ~/.claude/CLAUDE.md
```

**Copilot (VS Code)** — user-level prompts live in your VS Code profile folder:

| OS | Prompts folder |
|---|---|
| macOS | `~/Library/Application Support/Code/User/prompts/` |
| Windows | `%APPDATA%\Code\User\prompts\` |
| Linux | `~/.config/Code/User/prompts/` |

1. Copy every file from `.github/prompts/` into that folder.
2. Copy `.github/copilot-instructions.md` into that folder too, renamed to `learning-tutor.instructions.md`, and add this to the very top so it applies everywhere:
   ```
   ---
   applyTo: "**"
   ---
   ```
3. Copy `.github/skills/*` into `~/.copilot/skills/`.
4. Restart VS Code. In Copilot Chat, type `/` — you should see `/hint`, `/debug`, etc.

(Easier alternative: in Copilot Chat, run **"Chat: New Prompt File"** from the Command Palette and choose **User Data** to see exactly where your install keeps them.)

### Your learning logs (`learning/`)

The commands read and write `learning/*.md` **relative to the project you have open**. So even with a global install:

- Copy the `learning/` folder into each project where you want history tracked, **or**
- Keep one dedicated "notes" repo with `learning/` in it, and open that when you do `/retrieve` and `/autopsy`.

If `learning/` doesn't exist, the AI will just say there's no history yet — nothing breaks.

## FAQ

**Do I need to download this every time?**
No. Clone/download it **once**. With Option B (global) you never touch it again. With Option A you only re-copy files into each *new* project — not every time you open it.

**How do I get updates when Chris changes the toolkit?**
If you cloned it: `git pull` inside the toolkit folder, then re-run the copy commands (they overwrite the old versions). If you downloaded a zip, download it again.

**Will this mess with my other projects or my teammates?**
Global install only affects *your* machine. Per-project files get committed if you `git add` them — add `.claude/`, `CLAUDE.md`, `learning/` to `.gitignore` if you don't want them in a team repo.

**What if the project already has commands with the same name?**
Rename ours (e.g. `learn-debug.md` → `/learn-debug`) so the two don't collide.

**How do I turn it off when I just need to ship?**
Say **"ship this"** in chat. To disable completely, delete or rename the global files (e.g. `~/.claude/CLAUDE.md`).

**Copilot keeps auto-completing answers while I type.**
Turn off inline suggestions while learning: Settings → search `github.copilot.enable` → uncheck, or click the Copilot icon in the status bar → disable completions.

**I typed `/hint` and nothing happened.**
- Claude Code: make sure you started `claude` in the folder that contains `.claude/` (Option A) or that files are in `~/.claude/commands/` (Option B). Restart `claude`.
- Copilot: make sure you're in **Chat** (not inline), and that the files end in `.prompt.md`. Reload the window (`Ctrl/Cmd+Shift+P` → "Reload Window").

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
