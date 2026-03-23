# 🦞 Bubba

A persistent Claude Code agent team you steer from Telegram.

Built on [Agent Teams](https://code.claude.com/docs/en/agent-teams) and [Channels](https://code.claude.com/docs/en/channels) ([reference](https://code.claude.com/docs/en/channels-reference)).

One lead. Three subagents (Research, Design, Develop). Runs in tmux. Survives closing your laptop.

---

## Requirements

- Claude Code v2.1.80+ (`claude --version`)
- A [claude.ai](https://claude.ai) account — channels require this, API keys won't work
- [Bun](https://bun.sh)
- tmux
- Telegram

---

## Setup

### 1. Install dependencies

```bash
npm install -g @anthropic-ai/claude-code
curl -fsSL https://bun.sh/install | bash && source ~/.zshrc
brew install tmux
```

### 2. Create a Telegram bot

Open Telegram → find **BotFather** → `/newbot` → copy the token.

### 3. Install the Telegram plugin

Run Claude once in this directory (not in tmux):

```bash
claude
```

```
/plugin install telegram@claude-plugins-official
```

If not found:

```
/plugin marketplace add anthropics/claude-plugins-official
```

Then:

```
/reload-plugins
/telegram:configure YOUR_BOT_TOKEN_HERE
```

Exit Claude.

### 4. Pair your account

```bash
claude --channels plugin:telegram@claude-plugins-official
```

Send your bot any message in Telegram. It replies with a pairing code.

```
/telegram:access pair THE_CODE_HERE
/telegram:access policy allowlist
```

The allowlist step matters — without it, anyone who finds your bot can drive your session.

Exit Claude.

---

## Running

```bash
tmux new-session -s crew 'cd /path/to/bubba && claude --dangerously-skip-permissions --channels plugin:telegram@claude-plugins-official'
```

Attach:

```bash
tmux attach -t crew
```

Initialize the team by pasting this into the session:

```
Read CLAUDE.md.

Create an agent team with 3 teammates: Research, Develop, Design.
You are the lead. Delegate first.
When you need a decision, ask me on Telegram.
Keep updates short.
```

---

## Usage

Send your bot tasks from Telegram:

```
Build the auth flow
```

```
Research the best approach for X, then propose a plan
```

The lead coordinates and pings you when it needs a decision.

---

## tmux reference

| Command | Effect |
|---|---|
| `tmux attach -t crew` | Re-open the session |
| `Ctrl+B` then `D` | Detach, leave running |
| `tmux ls` | List sessions |
| `tmux kill-session -t crew` | Stop |

`Shift+Down` cycles between the lead and active subagents.

---

## Troubleshooting

**Bot silent** — Claude must be running with `--channels plugin:telegram@claude-plugins-official`.

**No subagents visible** — `Shift+Down`. Verify `.claude/settings.json` has `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS: "1"`.

**Stuck after resume** — Tell the lead: `Spawn fresh teammates and continue.`

**Lead hogging work** — Tell it: `Delegate first and wait for teammates to finish.`
