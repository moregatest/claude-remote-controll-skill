---
name: remote-control
description: Use when the user wants to start, list, or stop claude remote-control sessions. Triggers on keywords like remote-control, rc, 遠端控制, remote control session, 開一個 rc, 列出 rc, 停掉 rc, plus ~/Codes/ directory names.
---

# Remote Control Session Manager

Manage claude remote-control sessions as background processes. All sessions use `~/Codes/<dir>` as working directory. PID files stored in `~/.claude/rc-pids/`.

**禁止使用 tmux** — tmux 會造成手機端 remote control 異常。

## IMMEDIATE: 技能載入時立即執行

**當此技能被載入時，必須立刻檢查 `rc` 指令是否已安裝：**

```bash
which rc
```

- If found → 繼續處理使用者的請求
- If not found → **立即停止**，告訴使用者：
  > `rc` 指令尚未安裝，請執行以下指令安裝：
  > ```bash
  > curl -fsSL https://raw.githubusercontent.com/moregatest/claude-remote-controll-skill/main/rc -o /usr/local/bin/rc && chmod +x /usr/local/bin/rc
  > ```
  > 安裝完成後重新開啟 terminal 即可使用。

  **不繼續執行任何後續操作。**

## Operations

### Start a session

When the user says something like "開一個 rc session 在 tungTest" or "rc start tungTest":

1. Extract the directory name from the user's message
2. Verify the directory exists:
   ```bash
   ls ~/Codes/<dir>
   ```
3. Ensure PID directory exists:
   ```bash
   mkdir -p ~/.claude/rc-pids
   ```
4. Check no duplicate process:
   ```bash
   if [ -f ~/.claude/rc-pids/<dir>.pid ]; then
     pid=$(cat ~/.claude/rc-pids/<dir>.pid)
     if kill -0 $pid 2>/dev/null; then
       echo "ALREADY_RUNNING"
     fi
   fi
   ```
   If already running, tell the user and stop.
5. Launch claude remote-control as background process:
   ```bash
   cd ~/Codes/<dir> && nohup claude remote-control --permission-mode bypassPermissions --spawn same-dir --name "<dir>" > ~/.claude/rc-pids/<dir>.log 2>&1 &
   echo $! > ~/.claude/rc-pids/<dir>.pid
   ```
6. Confirm to the user: "已啟動 rc-<dir> (PID: xxx)"

### List sessions

When the user says something like "列出 rc" or "rc list":

1. Check all PID files and verify which are still running:
   ```bash
   for f in ~/.claude/rc-pids/*.pid 2>/dev/null; do
     name=$(basename "$f" .pid)
     pid=$(cat "$f")
     if kill -0 $pid 2>/dev/null; then
       echo "$name (PID: $pid) - running"
     else
       echo "$name (PID: $pid) - stopped"
       rm "$f"
     fi
   done
   ```
2. If no PID files found, say "目前沒有 rc session 在跑"

### Stop a session

When the user says something like "停掉 rc tungTest" or "rc stop tungTest":

1. Extract the directory name from the user's message
2. Check the PID file exists and process is running:
   ```bash
   if [ -f ~/.claude/rc-pids/<dir>.pid ]; then
     pid=$(cat ~/.claude/rc-pids/<dir>.pid)
     kill -0 $pid 2>/dev/null
   fi
   ```
   If not running, tell the user and stop.
3. Gracefully stop the process:
   ```bash
   kill $pid
   rm ~/.claude/rc-pids/<dir>.pid
   ```
4. Confirm to the user: "已停止 rc-<dir>"

## Rules

- Working directory is always `~/Codes/<dir>`
- **絕對不要用 tmux** — 會造成手機端 remote control 異常
- PID files 存在 `~/.claude/rc-pids/<dir>.pid`
- Log files 存在 `~/.claude/rc-pids/<dir>.log`
- Always verify before acting (directory exists, process exists/doesn't exist)
- Keep responses short — just confirm the action
