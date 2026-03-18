<p align="center">
  <img src="https://github.com/user-attachments/assets/70fa6367-afd1-4d40-b923-ede9fe351957" alt="Meduseld" width="250">
</p>

# Meduseld

We build tools for self-hosted server management. Our public project is Herugrim — everything else is internal tooling for our gaming group.

## Open Source

### [Herugrim](https://github.com/meduseld-io/herugrim)

A Cloudflare Worker that lets you authenticate with Cloudflare Access using Discord. It wraps OIDC around Discord's OAuth2 API so Cloudflare Access can use Discord as an identity provider — no separate identity service needed.

Fork of [Erisa/discord-oidc-worker](https://github.com/Erisa/discord-oidc-worker) with several improvements:

- **Admin role detection** — checks for a specific Discord role and includes `is_admin` in the JWT, so your app can do role-based access control without a separate admin system
- **Rich user claims** — the ID token includes a `discord_user` object with full profile data (ID, username, display name, avatar, discriminator) instead of just email
- **No KV dependency** — signing keys are generated in-memory, so there's no Cloudflare KV namespace to set up
- **No config file** — everything is configured as constants in the worker, no external `config.json` to manage
- **Optional guild restriction** — limit access to members of specific Discord servers
- **Simplified setup** — clone, edit a few constants, deploy

If you're using Cloudflare Access and want Discord login, this is what you need. Check the [repo README](https://github.com/meduseld-io/herugrim#readme) for setup instructions.

**Tech**: Cloudflare Workers, Hono, Jose · **License**: MIT

## Internal Projects

The rest of our repos are private tools we use to manage a dedicated game server, media streaming, and system monitoring for our friend group.

- **meduseld-backend** — Flask API and control panel for managing a dedicated game server, Jellyfin SSO, system monitoring, and user management
- **meduseld-site** — Static frontend pages served via Cloudflare Pages (services hub, admin panel, system monitor)

## Contributors

| | |
|---|---|
| [@quietarcade](https://github.com/quietarcade) | [@Hier0g1yphiK](https://github.com/Hier0g1yphiK) |
| [@glenwinters859](https://github.com/glenwinters859) | [@ThomasDrennan](https://github.com/ThomasDrennan) |
