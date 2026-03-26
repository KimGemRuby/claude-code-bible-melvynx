# Bible Melvynx — OpenClaw Workflow Complet

## Concept
OpenClaw = controler son ordinateur a distance depuis Telegram.
Lance des agents Claude Code en arriere-plan, notifie quand termine.

## Workflow feature depuis Telegram
1. Envoyer message/audio avec description de la feature
2. Skill `code_task` : clone worktree > npm install > cree GitHub Issue > lance apex --pr
3. Schedule un cron watcher chaque minute pour surveiller l'avancement
4. Notification Telegram avec lien de la PR quand termine
5. "Merge la PR et delete le worktree"

## Commande directe (sans Telegram)
```bash
claude -p --dangerously-skip-permissions apex --pr <issue_number> <description>
ps -p <PID> -o stat,etime   # verifier statut et temps ecoule
```

## Installation VPS Docker
```bash
npx openclaw-vps-setup
# Installe : Node, OpenClaw, Claude Code, Bun, Cloudflared, GCloud, UFW, Fail2Ban, SSH Hardening
```

## VPS recommande
Hetzner CAX21/CAX31 ARM ~$7.59/mois

## Modeles recommandes pour OpenClaw
| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2 input / $12 output par M |
| Cron jobs | Gemini 3 Flash | Encore moins cher |
| Agents de code | Claude Opus | Meilleure qualite |

## Securite VPS
- UFW + Fail2Ban (whitelister IPs AVANT activation)
- Desactiver password SSH (uniquement cle SSH)
- Docker pour sandboxing
- `SANDBOX=1 claude` pour bypass en sandbox
- Ne jamais laisser le dashboard OpenClaw ouvert en permanence

## Fichiers de config OpenClaw
- `soul.md` : identite/personnalite du bot
- `memory/` : memoire quotidienne
- `canvas/index.html` : interface web locale
- `.env` : variables d'environnement (tokens, cles API)
