<p align="center">
  <img src="https://github.com/user-attachments/assets/70fa6367-afd1-4d40-b923-ede9fe351957" alt="Meduseld" width="250">
</p>

# Meduseld

We're a group of friends building software for server management, media streaming, and other things we find useful. Some of it is open source, the rest is internal tooling for our gaming group.

## Public Projects

### [Herugrim](https://github.com/meduseld-io/herugrim) <img src="https://github.com/user-attachments/assets/a992766f-24d7-4271-88b4-62333265a1bf" alt="Herugrim" width="28" align="top">

A Cloudflare Worker that lets you authenticate with Cloudflare Access using Discord. It wraps OIDC around Discord's OAuth2 API so Cloudflare Access can use Discord as an identity provider — no separate identity service needed.

Fork of [Erisa/discord-oidc-worker](https://github.com/Erisa/discord-oidc-worker) with several improvements:

- **Environment-based config** — all settings via Wrangler env vars and secrets, no credentials in source code
- **Admin role detection** — checks for a specific Discord role and includes `is_admin` in the JWT, so your app can do role-based access control without a separate admin system
- **Rich user claims** — the ID token includes a `discord_user` object with full profile data (ID, username, display name, avatar, discriminator) instead of just email
- **Dynamic OAuth scopes** — only requests `guilds.members.read` when admin detection is enabled, reducing permissions for simpler setups
- **No KV dependency** — signing keys are generated in-memory, no Cloudflare KV namespace to set up
- **Error handling** — all Discord API calls are validated with descriptive error responses
- **Health endpoint** — `GET /health` for uptime monitoring
- **Optional guild restriction** — limit access to members of specific Discord servers
- **One-click deploy** — deploy button in the README for quick Cloudflare Workers setup

If you're using Cloudflare Access and want Discord login, this is what you need. Check the [repo README](https://github.com/meduseld-io/herugrim#readme) for setup instructions.

**Tech**: Cloudflare Workers, Hono, Jose · **License**: MIT

### [FellowSync](https://github.com/meduseld-io/fellowsync) <img src="https://github.com/user-attachments/assets/19b8237d-3e5e-47cb-98b6-063807a54262" alt="FellowSync" width="28" align="top"> — *Coming Soon*

A self-hosted Spotify listening party app. Create a room, invite friends, and listen to music together in sync. Everyone queues tracks, the host controls playback, and FellowSync keeps everyone's Spotify playing the same song at the same position.

**Tech**: Flask, React, Redis, Spotify Web API · **License**: AGPL-3.0

### [ExSpire](https://github.com/meduseld-io/exspire) <img src="https://github.com/user-attachments/assets/0f597755-d2a1-418d-bffa-b9e0ea4957ad" alt="ExSpire" width="28" align="top"> — *Coming Soon*

Track things before they expire. Add subscriptions, documents, warranties, and more with expiry dates, then get reminders before they lapse. Items are displayed in a tower layout sorted by urgency.

**Tech**: Express, React, SQLite · **License**: Source Available

## Internal Projects

The rest of our repos are private tools we use to manage a dedicated game server, media streaming, and system monitoring for our friend group.

- **meduseld-backend** — Flask API and control panel for managing a dedicated game server, Jellyfin SSO, system monitoring, and user management
- **meduseld-site** — Static frontend pages served via Cloudflare Pages (services hub, admin panel, system monitor)

## Contributors

[@quietarcade](https://github.com/quietarcade) · [@Hier0g1yphiK](https://github.com/Hier0g1yphiK) · [@glenwinters859](https://github.com/glenwinters859) · [@ThomasDrennan](https://github.com/ThomasDrennan)
