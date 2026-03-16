# Claude Remote Control Skill

A Claude Code skill + `rc` CLI for managing `claude remote-control` sessions as background processes.

## Quick Install

### 1. Install the `rc` command

```bash
curl -fsSL "https://raw.githubusercontent.com/moregatest/claude-remote-controll-skill/main/rc?$(date +%s)" | sudo tee /usr/local/bin/rc > /dev/null && sudo chmod +x /usr/local/bin/rc
```

> `?$(date +%s)` bypasses GitHub's CDN cache to ensure you always get the latest version.

### 2. Install the Claude Code skill

```bash
cp -r remote-control ~/.claude/skills/
```

Or clone and install:

```bash
git clone https://github.com/moregatest/claude-remote-controll-skill.git
cp -r claude-remote-controll-skill/remote-control ~/.claude/skills/
```

## Usage

### CLI (`rc`)

```bash
rc start <dir>   # Start session in ~/Codes/<dir>
rc list          # List all sessions
rc stop <dir>    # Stop a session
```

### Via Claude Code (natural language)

- **Start**: "rc start tungTest" / "開一個 rc session 在 tungTest"
- **List**: "rc list" / "列出 rc"
- **Stop**: "rc stop tungTest" / "停掉 rc tungTest"

## How it works

- Sessions run as background processes (no tmux)
- Working directory: `~/Codes/<dir>`
- PID files: `~/.claude/rc-pids/<dir>.pid`
- Log files: `~/.claude/rc-pids/<dir>.log`
- Claude Code skill auto-detects if `rc` is installed and prompts installation if missing
