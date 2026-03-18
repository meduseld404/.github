<p align="center">
  <img src="https://github.com/user-attachments/assets/a992766f-24d7-4271-88b4-62333265a1bf" alt="Meduseld" width="250">
</p>

# Meduseld

A self-hosted server management platform for our gaming group. Web-based control panel, media streaming, system monitoring, and Discord-based authentication — all accessible from a browser.

## What It Does

Instead of SSHing into the box every time we want to restart the game server, we just hit a button. The whole thing is browser-based, so anyone in the group can manage things from their phone or laptop.

- Start/stop/restart the game server with one click
- See if it's running, crashed, or updating
- Check who's online and monitor system resources
- Stream media through Jellyfin with automatic account provisioning
- View server logs and system stats without SSH
- Manage users and permissions through Discord roles

## Repositories

### [herugrim](https://github.com/meduseld-io/herugrim) (Public)

Discord OIDC identity provider for Cloudflare Access. A Cloudflare Worker that bridges Discord OAuth to Cloudflare Access, enabling Discord-based login across all Meduseld services. Fork of [Erisa/discord-oidc-worker](https://github.com/Erisa/discord-oidc-worker) with added admin role detection and richer user profile claims.

**Tech**: Cloudflare Workers, Hono, Jose

### meduseld-backend (Private)

Flask backend powering the control panel, health dashboard, and API. Handles game server process management, system monitoring, Jellyfin SSO, calendar events, user management, and backup/reboot services.

**Tech**: Python, Flask, PostgreSQL, SQLAlchemy

### meduseld-site (Private)

Static frontend pages served via Cloudflare Pages. The services hub, system monitor, admin panel, Edoras media management page, and landing page.

**Tech**: HTML, CSS, JavaScript, Bootstrap 5, Chart.js

## How It Works

```
Browser → Cloudflare Access (Discord login via herugrim)
       → Cloudflare Pages (static site)
       → Cloudflare Tunnel → Flask backend (API + panel)
                           → Game server / Jellyfin / SSH
```

Users log in with Discord. Cloudflare Access handles auth using herugrim as the OIDC provider. Admin status is determined by a Discord role — no manual promotion needed.

## Services

| Service | URL | Description |
|---------|-----|-------------|
| Landing | meduseld.io | Splash page |
| Services Hub | services.meduseld.io | Main navigation, status checks, calendar, game news |
| Game Panel | panel.meduseld.io | Server control, logs, stats, charts |
| System Monitor | system.meduseld.io | Server logs, resource monitoring, backup/reboot (admin) |
| Admin Panel | admin.meduseld.io | User management (admin) |
| Edoras | edoras.meduseld.io | Media service management (admin) |
| Jellyfin | jellyfin.meduseld.io | Media streaming with auto-provisioned accounts |
| SSH Terminal | ssh.meduseld.io | Browser-based SSH (admin) |
| Health | health.meduseld.io | Public service status dashboard |

## How to Access

1. Go to https://meduseld.io
2. Click through to the services menu
3. Log in with Discord
4. Pick what you need

If you can't get in, ask whoever manages the Cloudflare Access list to add your Discord account.

## Server Hardware

AMD Ryzen 7 2700 • 32GB DDR4 3600 • RTX 3060 • 1TB NVMe SSD • Ubuntu Server 24.04

## Contributors

| | |
|---|---|
| [@quietarcade](https://github.com/quietarcade) | [@Hier0g1yphiK](https://github.com/Hier0g1yphiK) |
| [@glenwinters859](https://github.com/glenwinters859) | [@ThomasDrennan](https://github.com/ThomasDrennan) |
