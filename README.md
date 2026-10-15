# SVM+ ADVANCED — Local AI Infrastructure Agent (Powered by AnkitCoder)

Discord VPS platform (LXC/KVM nodes, IP pools, port forwarding, Razorpay/Stripe/UPI payments, WebSSH) with a LocalAI-powered agent.

## Quick start
```bash
sudo bash install.sh            # install / upgrade, keeps .env and vps.db
sudo bash install.sh ai-install # LocalAI + AI wizard
svm doctor
```

Docs: INSTALL.md • CONFIGURATION.md • AI_AGENT.md • TROUBLESHOOTING.md • SECURITY.md • ARCHITECTURE.md

## Web Admin + WebSSH
- WebSSH `http://<IPv4>:5000/` - Admin panel `http://<IPv4>:5000/admin` (`svm admin-url`)
- `install.sh` generates the admin password on first install and prints it once; reset with `svm admin-password`.
- New admin commands: `!nodes`, `!nat`, `!ipool detail`, `!sshfix`. Full details: **WEB_ADMIN.md**.
