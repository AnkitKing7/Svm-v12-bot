# Changelog

## 11.3-PLUS fixed + ops upgrade
**Fixed**
- Admin panel: `/admin` returned 404 after install/upgrade because `install.sh` never deployed `admin.html` (now deployed, backed up, rolled back).
- Admin panel: wrong field names (`user_id`, `session_id`, `main_admin_id`) and a wrong disconnect route made columns empty and "Disconnect" fail.
- Admin login: template placeholder `CHANGE_THIS...` was accepted as the real password; non-ASCII input crashed login; no brute-force limit.
  Now placeholders count as unset, `install.sh` generates strong secrets, 8 failures / 5 min lock.
- `admin.html` / `webssh.html` are read relative to `bot.py`, not the current directory.
- Duplicate / contradictory `.env.example` and docs (`ADMIN_WEB_*` vs `WEB_ADMIN_*`).
- WebSSH "did not return an SSH banner": port is now probed first; dead sshd is auto-repaired (start VPS, restart/install sshd, rebuild LXD proxy, fall back to the VPS private IP). The error message says exactly what is wrong.

**Added**
- `!nodes` (all nodes + live status + every VPS + SVG/PNG), `!nat`, `!ipool detail|free|find|nat`, `!sshfix`.
- Admin panel rewritten: Nodes & VPS, IP Pool & NAT, Ports, SSH Diagnose/Repair, live SVG cards, Config.
- New SVG templates: `nodes-vps`, `ipool-detail`, `nat-status`.
- `svm admin-password`, `svm admin-url`; `svm doctor` checks WebSSH, admin password and admin.html.

## AI + help fixes
- AI model name: if `LOCALAI_MODEL` is empty, mistyped or not installed the agent used to stay "NOT READY". It now auto-selects an installed chat model
  (`AI_MODEL_AUTO=true`), shows what it picked, and `!askai model <name|auto>` changes it at runtime (saved across restarts). `!askai models` lists installed models.
- `!help`: `!nodes`, `!nat`, `!sshfix` listed in Node Management; new "Nodes & Fleet" / "Network & IPs" auto groups; `!aiask`, `!ipool`, `!about`, `!vps-list` now have descriptions.
- AI identity: the model used to answer "who are you / which model" with its own raw name (Llama, GPT, a .gguf file name). Now the AI is **SVM+ AI (powered by AnkitCoder)**;
  "tum kaun ho / who are you / which model" gets a fixed answer with a friendly model label, and the system prompt forbids claiming to be ChatGPT/Meta/etc.
  Replies follow the user's language (English/Hindi/Hinglish). Env: `AI_DISPLAY_NAME`, `AI_SHOW_MODEL`.
