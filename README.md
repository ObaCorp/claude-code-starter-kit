# Claude Code Starter Kit

A small, opinionated starter pack for someone who just installed Claude Code and doesn't know where to start.

It doesn't try to be comprehensive. It gives you four things that actually move the needle on day one:

1. A **CLAUDE.md** with 7 principles that reduce the most common ways Claude makes a mess of your code.
2. A **settings.json** template with tight permissions — safe reads are auto-allowed, any real `Bash` command prompts you, and truly dangerous patterns are denied outright.
3. A **working hook** you can watch fire in real time, so "hooks" stop being an abstract idea.
4. Two short guides: one for your first hour, one for when you're ready to go deeper.

## Who this is for

You just installed Claude Code. You've maybe run one or two prompts. You know it can write code. You don't yet know what a *skill*, *hook*, *agent*, or *MCP* is — or why you'd care.

That's exactly the right starting point.

## What's in here

```
claude-code-starter-kit/
├── CLAUDE.md                    # Drop this into ~/.claude/CLAUDE.md
├── settings.json.example        # Rename to settings.json, drop in ~/.claude/
├── hooks/
│   └── stop-slash-text-guard.sh # A real, working audit-only hook
├── GETTING_STARTED.md           # Do this on day one
└── NEXT_STEPS.md                # Read this in week two
```

## Install in 30 seconds

```bash
# 1. Install Claude Code if you haven't already
npm install -g @anthropic-ai/claude-code

# 2. Clone this kit
git clone <this-repo> ~/claude-code-starter-kit

# 3. Back up any existing Claude Code config before overwriting
[ -f ~/.claude/CLAUDE.md ] && cp ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.bak
[ -f ~/.claude/settings.json ] && cp ~/.claude/settings.json ~/.claude/settings.json.bak

# 4. Copy the files into your Claude Code config
mkdir -p ~/.claude/hooks
cp ~/claude-code-starter-kit/CLAUDE.md ~/.claude/CLAUDE.md
cp ~/claude-code-starter-kit/settings.json.example ~/.claude/settings.json
cp ~/claude-code-starter-kit/hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 5. Open Claude Code in any project folder
cd your-project
claude
```

Then read [GETTING_STARTED.md](./GETTING_STARTED.md).

## What each part of settings.json does

- **`permissions.allow`** — tools that run silently. Default list: reads, edits, web search, and a narrow slice of read-only `Bash` (git status/diff/log, ls, cat, head, tail). Anything else — including `Bash(npm install)`, `Bash(rm ...)`, `Bash(python3 script.py)` — will prompt you.
- **`permissions.deny`** — patterns that are never allowed. Not a complete sandbox — treat it as defense in depth, not a safety net. Real safety comes from the `ask` prompts.
- **`permissions.ask`** — tools that prompt every time. Starts empty. Add entries here once you install MCP servers with destructive actions (e.g. `mcp__gmail__send_email`, `mcp__stripe__*`).
- **`hooks`** — shell scripts wired to Claude Code events. The one wired up by default is an audit-only hook that logs a common mistake pattern.

Rule of thumb: boring and reversible → `allow`. Destructive or externally visible → `ask`. Dangerous or impossible to undo → `deny`. You'll see a lot of Bash prompts in your first week — that's intentional, not a bug. It's how you learn what Claude is actually trying to run.

## Why these files and not more

Claude Code has a lot of surface area: skills, hooks, agents, plugins, MCP servers, slash commands, permissions, memory. Dropping all of it on a beginner produces paralysis, not productivity.

Start with one good rulebook (CLAUDE.md), safe defaults (settings.json), and one concrete example of a hook. Everything else is best learned when you hit the specific problem it solves.

## License

MIT. Take what's useful, ignore what isn't.
