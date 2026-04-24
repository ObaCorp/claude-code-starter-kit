# Getting Started

This is what to do in your first hour with Claude Code. Follow top to bottom.

## 1. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

You'll need Node 18+. If you don't have npm, install Node from nodejs.org first.

## 2. Sign in

Run `claude` in any folder. It'll walk you through signing in with your Anthropic account (or API key).

Run `claude --help` to see the CLI flags. You won't need most of them on day one.

## 3. Drop in the starter files

From the root of this repo:

```bash
# Back up anything that already exists
[ -f ~/.claude/CLAUDE.md ] && cp ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.bak
[ -f ~/.claude/settings.json ] && cp ~/.claude/settings.json ~/.claude/settings.json.bak

# Install
mkdir -p ~/.claude/hooks
cp CLAUDE.md ~/.claude/CLAUDE.md
cp settings.json.example ~/.claude/settings.json
cp hooks/stop-slash-text-guard.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/stop-slash-text-guard.sh
```

What each file does:
- `~/.claude/CLAUDE.md` — loaded into every session, globally. These are standing instructions for Claude.
- `~/.claude/settings.json` — permissions (what Claude can run without asking) and hook wiring.
- `~/.claude/hooks/*.sh` — scripts that run on Claude Code events. The simplest event is `Stop` (end of a turn). Tool events like running `Edit` or `Write` are wired through `PreToolUse` / `PostToolUse` with a matcher.

## 4. Check that hooks need jq

The included hook uses `jq`. Install it if you don't have it:

```bash
# macOS
brew install jq

# Linux
sudo apt install jq
```

## 5. Your first real session

Open Claude Code in any project folder (or a fresh empty folder):

```bash
mkdir ~/claude-hello && cd ~/claude-hello
claude
```

Try a simple task:

> Write a Python script that prints the first 10 Fibonacci numbers. Make a virtualenv, install nothing, just run it.

Watch what happens:
- Claude states a plan before touching files (that's your **CLAUDE.md** section 4 kicking in).
- It prompts you for permission before running `python3`, because this starter kit deliberately does *not* auto-allow arbitrary Bash. Answer yes when the prompt shows a command you recognize.
- It only touches the files it needs to (section 3).
- At the end, nothing should be over-engineered (section 2).

## 6. Watch the hook fire

The stop-slash-text-guard hook logs to `~/.claude/analytics/slash-text-violations.jsonl`.

At some point, Claude will end a message with something like `/something` — a dead slash-command reference. When that happens, you'll see a line appear in that file:

```bash
tail -f ~/.claude/analytics/slash-text-violations.jsonl
```

You now understand what a hook is: a shell script that runs on a Claude Code event. That's it. That's the whole thing.

## 7. Start customizing CLAUDE.md

The included CLAUDE.md is generic. Your real version should encode the quirks of **your** work:

- Which language/framework you use most → name it, so Claude stops asking.
- Your testing style → "use pytest, never unittest" or "write tests only for bugs."
- Your commit style → "short, no AI boilerplate in messages."
- Taboos → "never use Docker unless I explicitly ask."

Edit `~/.claude/CLAUDE.md` directly. Changes take effect the next time you start `claude`.

## 8. Know when to read NEXT_STEPS

Don't read [NEXT_STEPS.md](./NEXT_STEPS.md) yet. Come back when you've done real work for a week and find yourself thinking "this would be so much better if Claude could just..."

That "if it could just..." sentence is almost always answered by one of the topics in NEXT_STEPS (skills, memory, slash commands, MCP servers, specialized agents). But reading about them before you've felt the need is how you get paralysis.

Go use it for a week. Then come back.
