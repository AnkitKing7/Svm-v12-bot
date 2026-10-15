# SVM+ Web Admin + WebSSH

## URLs
- WebSSH: `http://<SERVER_IPv4>:5000/`
- Admin panel: `http://<SERVER_IPv4>:5000/admin`  (`svm admin-url` prints it)

## Login
`install.sh` generates `ADMIN_WEB_PASSWORD` and `ADMIN_WEB_SECRET` in `.env` and shows the password once.
Forgot it? `svm admin-password` creates a new one and restarts the bot.
`WEB_ADMIN_*` spellings from older docs are still accepted. A `CHANGE_THIS...` placeholder counts as *not set*.

```env
ADMIN_WEB_ENABLED=true
ADMIN_WEB_PASSWORD=...        # long random value
ADMIN_WEB_SECRET=...          # session signing key (optional, derived from the password if empty)
ADMIN_SESSION_TTL=3600
ADMIN_COOKIE_SECURE=false     # true only behind HTTPS
```

## Panel tabs
Overview (stats + live SVG card) • Nodes & VPS (live status, host load, every VPS, *Repair SSH* button) •
IP Pool & NAT (per-pool allocations, NAT health, re-apply rules) • Ports • SSH Diagnose • Sessions • Admins • Config.

## Admin API (all need the session cookie; POST/DELETE also need `X-SVM-CSRF`)
`/api/admin/login|logout|me|overview|vps|nodes|ipam|nat|ports|status|config|sessions|admins`
`POST /api/admin/nat/sync` • `POST /api/admin/ssh/diagnose` • `POST /api/admin/vps/<name>/sshfix` •
`POST /api/admin/sessions/<id>/disconnect` • `GET /api/admin/svg/<template>`
Root passwords and API keys are never returned.

## WebSSH
Endpoints: `POST /api/ssh/connect|read|write|resize|disconnect`, `GET /api/health`.
Before opening a session the server probes the port. If the port answers but sshd gives no banner
(`...did not return an SSH banner`), and the port belongs to a VPS on this server, the bot automatically:
starts the VPS if stopped -> (re)installs/restarts sshd -> rebuilds the LXD port forward if it is broken ->
connects through the VPS private IP when the public forward is still dead. The error shown to the user says what was done.

## Discord commands added in this build
| Command | What it does |
|---|---|
| `!nodes [text\|svg\|png]` | every node: live status, host CPU/RAM/disk, **all VPS** per node + SVG/PNG card |
| `!ipool detail [pool] [all]` | full per-pool report (free/used/reserved, owners, private IPs) + SVG |
| `!ipool free [pool]` / `find <ip\|vps>` / `nat` | free addresses / lookup / NAT health |
| `!nat [status\|sync\|fix]` | NAT health card, re-apply rules, enable `ip_forward` |
| `!sshfix <vps\|id\|public-port>` | repair sshd / start VPS / rebuild port forward |
