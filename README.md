# Claude Remote Control Skill

A Claude Code skill for managing `claude remote-control` sessions as background processes.

## Features

- Start / List / Stop remote-control sessions
- Auto-detect if `claude` CLI is installed
- PID-based process management (no tmux)
- Working directory: `~/Codes/<dir>`

## Installation

Copy the `remote-control/` folder into your Claude Code skills directory:

```bash
cp -r remote-control ~/.claude/skills/
```

## Usage

- **Start**: "rc start tungTest" or "開一個 rc session 在 tungTest"
- **List**: "rc list" or "列出 rc"
- **Stop**: "rc stop tungTest" or "停掉 rc tungTest"
