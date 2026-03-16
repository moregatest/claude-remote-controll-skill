# Claude Remote Control Skill

A Claude Code skill + `rc` CLI for managing `claude remote-control` sessions as background processes.

## Install

### 1. Install the `rc` command

Clone the repo, review the script, then install:

```bash
git clone https://github.com/moregatest/claude-remote-controll-skill.git
cat claude-remote-controll-skill/rc   # review before running as root
sudo cp claude-remote-controll-skill/rc /usr/local/bin/rc
sudo chmod +x /usr/local/bin/rc
```

Alternatively install to a user-writable location without sudo:

```bash
mkdir -p ~/.local/bin
cp claude-remote-controll-skill/rc ~/.local/bin/rc
chmod +x ~/.local/bin/rc
# make sure ~/.local/bin is in your PATH
```

### 2. Install the Claude Code skill

```bash
cp -r claude-remote-controll-skill/remote-control ~/.claude/skills/
```

## Configuration

By default `rc start <name>` resolves relative names against `~/Codes`. Override with:

```bash
export RC_CODES_DIR=/path/to/your/projects
```

Add to `~/.zshrc` or `~/.bashrc` to persist.

## Usage

### CLI

```bash
rc start <dir>        # ~/Codes/<dir>
rc start ~/other/dir  # absolute or ~/ path
rc list
rc stop <dir>
```

### Via Claude Code (natural language)

- "rc start tungTest" / "開一個 rc session 在 tungTest"
- "rc list" / "列出 rc"
- "rc stop tungTest" / "停掉 rc tungTest"

## How it works

- Sessions run as background processes (no tmux)
- PID files: `~/.claude/rc-pids/<name>.pid`
- Log files: `~/.claude/rc-pids/<name>.log`
- Claude Code skill detects if `rc` is installed and shows install instructions if missing
