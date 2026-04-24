# Next Steps

Come here after a week or two of using Claude Code. Each section below answers a specific "I wish it could just..." you'll hit in real work.

## "I wish I didn't have to re-explain my project every session"

**Use memory.** Claude Code has a persistent memory system at `~/.claude/projects/<project-path>/memory/`. You write markdown files there, and Claude can read them in future sessions.

Good uses: how you like to work, gotchas about the codebase, decisions you keep having to re-explain. Bad uses: things derivable from `git log` or the current file tree, and personal identifiers (these files are plaintext on disk — treat them like notes, not a secrets store).

Start with a single `MEMORY.md` index file. Add entries as you notice yourself repeating instructions.

## "I find myself typing the same prompt over and over"

**Make a slash command.** Create `~/.claude/commands/<name>.md` with the prompt inside. Now typing `/<name>` in Claude Code runs that prompt.

Example — `~/.claude/commands/refresh.md`:

```markdown
Catch me up on this codebase. Summarize what it does, the main files,
and what's been changed in the last 10 commits.
```

Now `/refresh` does that every time, in every project.

## "I want Claude to do X every time Y happens"

**Write another hook.** The `stop-slash-text-guard.sh` in this kit is a `Stop` hook — runs on turn end. Other events:

- **PreToolUse** — before Claude calls a tool (gate permissions, add confirmations).
- **PostToolUse** — after a tool runs (audit what happened, warn on patterns).
- **UserPromptSubmit** — when you submit a prompt (inject context, run pre-checks).
- **Stop** — end of Claude's turn (log, notify, audit).

Wire them in `settings.json` under `hooks.<event>`. See the Anthropic docs for the full spec.

## "I wish Claude could use my Gmail / Google Drive / Stripe / etc."

**Install an MCP server.** MCP (Model Context Protocol) is the standard way Claude talks to external services. Servers exist for Gmail, Google Calendar, Stripe, Supabase, PostHog, Notion, Linear, and dozens more.

Start with one. Pick the service you use most. Follow its install instructions (usually a single `claude mcp add` command). Expect OAuth flows the first time.

Then update your `settings.json`:
- Add the tools to `allow` if they're read-only.
- Add destructive tools (sends, deletes, charges) to `ask`.
- Never put anything that costs money in `allow`.

## "I want a specialist to handle this subtask"

**Use the Agent tool.** Claude Code ships with specialized agents (UI Designer, Code Reviewer, QA, etc.). Claude can delegate a bounded subtask to one of them and get back a focused result without cluttering the main session.

Two ways to invoke: ask in plain English ("use the code-reviewer agent on this file") and Claude will dispatch it, or use an `@agent-name` mention to invoke one explicitly. Subagent details: https://code.claude.com/docs/en/sub-agents

## "I want all of this, already set up"

**Look at gstack** (github.com/obra/gstack). It's an opinionated bundle of skills, hooks, and commands that wraps Claude Code. It's heavier than this starter kit and will override some things, so don't install it on day one — install it when you've built enough muscle memory to know what you'd customize.

Other curated collections exist. Search GitHub for `claude-code` topics and star counts sorted high.

## "I want to go deeper"

Read, in this order:
1. The official Claude Code docs (https://code.claude.com/docs/en/overview).
2. The MCP spec (https://modelcontextprotocol.io) — short, worth understanding.
3. The source code of the hooks you care about. Most are <100 lines of bash.

The pattern is always the same: small, composable shell/markdown files that hook into a few well-defined events. Nothing magic.
