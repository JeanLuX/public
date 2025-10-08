# Auto-Update Homebrew with Systemd

This guide shows how to set up automatic (monthly) Homebrew maintenance using systemd user services.

Homebrew must be installed before (https://docs.brew.sh/Homebrew-on-Linux)

Tested on Ubuntu 24.04 & Fedora 42, with Homebrew 4.6.16 & notify-send 0.8.7

## 1. Create the maintenance script

Create the file `~/.local/bin/brew-maintenance`:

```bash
#!/bin/bash
#description        Homebrew maintenance script with notifications

LOG=$(mktemp)

# Ensure brew is available
if ! command -v brew >/dev/null 2>&1; then
    echo "Error: Homebrew (brew) not found in PATH." >&2
    exit 1
fi

# Run update & upgrade
brew update 2>&1 | tee -a "$LOG"
brew upgrade 2>&1 | tee -a "$LOG"

# Count upgraded formulae
UPDATED=$(grep -cE "Upgrading |Upgraded " "$LOG" || true)

# Run cleanup
brew cleanup --prune=all 2>&1 | tee -a "$LOG"

# Count removed files (exclude cache directory)
REMOVED=$(grep -E "Pruned|Removing" "$LOG" | grep -v "$HOME/.cache/Homebrew/" | wc -l || true)

# Build message
TITLE="🍺 Homebrew maintenance"
if [ "$UPDATED" -eq 0 ] && [ "$REMOVED" -eq 0 ]; then
    MSG="Everything is already up to date ✅"
else
    MSG="Updated: ${UPDATED} package(s) • Removed: ${REMOVED} old version(s) ✅"
fi

# Print to stdout and log to syslog
echo "$TITLE - $MSG"
logger -t brew-maintenance "$TITLE - $MSG" 2>/dev/null || true

# Send desktop notification if available
if command -v notify-send >/dev/null 2>&1; then
    notify-send "$TITLE" "$MSG"
fi
```

Make it executable:
```bash
chmod +x ~/.local/bin/brew-maintenance
```

## 2. Create the systemd service

Create the file `~/.config/systemd/user/brew-maintenance.service`:

```ini
#description        Run Homebrew maintenance script

[Unit]
Description=Homebrew maintenance

[Service]
Type=oneshot
ExecStart=/bin/bash -lc 'brew-maintenance'
Environment="PATH=/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin"
```

## 3. Create the systemd timer

Create the file `~/.config/systemd/user/brew-maintenance.timer`:

```ini
#description        Monthly Homebrew maintenance: update, upgrade, cleanup

[Unit]
Description=Homebrew monthly maintenance

[Timer]
OnCalendar=monthly
Persistent=true

[Install]
WantedBy=timers.target
```

## 4. Enable the timer

```bash
systemctl --user daemon-reload
systemctl --user enable --now brew-maintenance.timer
```

## 5. Test the setup

Run the maintenance script manually to verify it works:

```bash
systemctl --user start brew-maintenance.service
```

Check the timer status:

```bash
systemctl --user status brew-maintenance.timer
```
