# Meduseld

Our web control panel for managing the Icarus game server. Instead of SSHing into the box every time we want to restart the server, we can just hit a button in the browser.

## What It Does

This is basically a remote control for our Icarus dedicated server:

- Start/stop/restart the server with one click
- See if it's actually running or if it crashed again
- Check how many people are online
- Monitor CPU, RAM, and disk usage
- Read server logs without SSHing in
- Update the game when new patches drop

It's all browser-based, so anyone in the group can manage the server from their phone or laptop.

## Features

- **Server Control**: Start, stop, restart, or force kill the Icarus server
- **Live Stats**: Real-time graphs showing CPU, RAM, disk usage over the last 30 minutes
- **Server Logs**: View game logs directly in the browser - see who's connecting, what's breaking, etc.
- **Update Checker**: Automatically checks Steam for new Icarus updates
- **Web Terminal**: Full SSH access through the browser if you need to run commands

## How to Access

1. Go to https://meduseld.io
2. Click through to the service menu
3. Pick what you need (Game Panel, SSH Terminal, Jellyfin, etc.)
4. Log in with Discord
5. Do your thing

If you can't get in, bug whoever's managing the Cloudflare Access list to add your Discord account.

## Server Status

The panel shows different states:

- **Offline** - Server's not running, hit Start
- **Starting** - Booting up, give it a minute
- **Running** - We're live, people can join
- **Stopping** - Shutting down
- **Restarting** - Usually means it's updating
- **Crashed** - Something went wrong, check logs or restart

## Quick Guide

**Starting the server**: Click the green Start button, wait 30-60 seconds

**Checking if it's up**: Status will show "Running" in green, plus you'll see the player count

**Server's frozen**: Use Force Stop to kill it, then start it again

**Updating the game**: Click "Check for Updates" - if there's a new version, hit Restart and it'll update automatically

**Reading logs**: Click the Logs tab to see what's happening in real-time

**Understanding the graphs**:
- CPU: How hard the processor is working
- Memory: How much RAM is being used
- Disk: Storage space used

If any of these hit 100%, the server's probably gonna have a bad time.

## Contributors

- [Add your names here]

## For Developers

Want to change something or add a feature? Here's how:

### Making Changes

1. Clone the repo and make a branch:
```bash
git clone <repo-url>
cd meduseld
git checkout -b feature/whatever-youre-doing
```

2. Edit stuff in the `app/` folder:
   - `app/webserver.py` - Main Flask app
   - `app/config.py` - Settings
   - `app/templates/panel.html` - Control panel UI
   - `app/static/css/style.css` - Styles

3. Test locally if you want:
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python app/webserver.py
```
Then visit http://localhost:5000

4. Push your branch:
```bash
git add .
git commit -m "what you changed"
git push -u origin feature/whatever-youre-doing
```

5. Open a PR on GitHub

6. After it's merged, deploy to the server:
```bash
ssh vertebra@meduseld.io
cd /srv/meduseld
git pull
sudo systemctl restart icarus-panel
```

### How It Works

```
Browser → Cloudflare (auth + HTTPS) → Cloudflare Tunnel → Flask App (port 5000) → Icarus Server
```

The Flask app monitors the Icarus process and collects stats every few seconds. The UI polls the API every 5 seconds to update the display.

### Key Files

- `app/webserver.py` - Flask routes, API endpoints, server monitoring
- `app/config.py` - All the settings (paths, timeouts, etc.)
- `app/templates/panel.html` - Control panel UI (Bootstrap + Chart.js)
- `app/templates/terminal.html` - SSH terminal wrapper (uses ttyd)

### Common Tasks

**Restart the panel**:
```bash
ssh vertebra@meduseld.io
sudo systemctl restart icarus-panel
```

**View panel logs**:
```bash
ssh vertebra@meduseld.io
tail -f /srv/meduseld/logs/webserver.log
```

**Manually start/stop Icarus** (bypassing the panel):
```bash
ssh vertebra@meduseld.io
cd /srv/games/icarus
./start.sh              # Start
pkill -9 IcarusServer   # Stop
```

**Check what's running**:
```bash
ssh vertebra@meduseld.io
sudo systemctl status icarus-panel
sudo systemctl status ttyd
sudo systemctl status cloudflared
```

### Troubleshooting

**"Server shows offline but it's actually running"**
The process name might've changed. Check with `ps aux | grep -i icarus` and update `PROCESS_NAME` in `app/config.py` if needed.

**"Graphs aren't updating"**
Stats thread probably crashed. Restart the panel: `sudo systemctl restart icarus-panel`

**"Can't access the site"**
1. Check if Cloudflare Tunnel is running: `sudo systemctl status cloudflared`
2. Make sure your email is in the Access list
3. Try incognito mode

**"My changes aren't showing up"**
Did you restart the panel after pulling? `sudo systemctl restart icarus-panel`

### API Endpoints

If you want to script stuff:

```bash
# Control
curl -X POST https://panel.meduseld.io/start
curl -X POST https://panel.meduseld.io/stop
curl -X POST https://panel.meduseld.io/restart
curl -X POST https://panel.meduseld.io/kill

# Stats
curl https://panel.meduseld.io/api/stats | jq
curl https://panel.meduseld.io/api/logs | jq
curl https://panel.meduseld.io/api/history | jq
curl https://panel.meduseld.io/api/check-update | jq
```

## Tech Stack

- Python 3.12 + Flask
- Bootstrap 5 + Chart.js
- ttyd (web terminal)
- Cloudflare Tunnel + Access
- Ubuntu Server 24.04

## Adding Someone New

1. Add their email to Cloudflare Access (Zero Trust dashboard → Applications → Meduseld)
2. Add them to GitHub if they're gonna code (Repo Settings → Collaborators)
3. Send them the URL: https://panel.meduseld.io

## Version

Current: **0.3.0-alpha**

Still in alpha, so expect some rough edges. Report bugs in the group chat or open an issue.
