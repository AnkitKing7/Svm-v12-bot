# Troubleshooting

- `svm doctor` / `install.sh ai-doctor` — first stop.
- LocalAI unreachable → `install.sh ai-status`, check `LOCALAI_BASE_URL`.
- Model not ready → `install.sh ai-models` and set `LOCALAI_MODEL` to an installed model.
- No PNG → install `cairosvg` or `librsvg2-bin`.
- Sandbox unavailable → install Docker and `docker pull` the sandbox image.
- Webhooks → `!gateways`, and your HTTPS proxy must reach the webhook port.
- WebSSH "did not return an SSH banner" -> the port is open but sshd inside the VPS is down. The bot now repairs this by itself;
  to do it by hand: `!sshfix <vps|port>` or Admin panel -> SSH Diagnose -> *Diagnose + Repair SSH*.
- `/admin` says "admin.html not found" -> re-run `sudo bash install.sh` (older installers did not deploy admin.html). Check `svm doctor`.
- `/admin` says "ADMIN_WEB_PASSWORD is not set" -> `svm admin-password`.
- Public IP not reachable in NAT mode -> `!nat` shows the exact problem (ip_forward, missing rules, IP not on host); `!nat fix` / `!nat sync`.
