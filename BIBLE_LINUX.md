# Bible Claude Code — Linux

> Guide complet pour configurer et utiliser Claude Code sur Linux (Ubuntu, Debian, Arch, etc.) et VPS
> Basee sur la methode Melvynx (CodeLynx) — Masterclass 4h + Setup 20min + Formation Complete
> Voir README.md pour la methode complete (workflows, prompting, context engineering)

---

## Table des matieres

1. [Pre-requis](#pre-requis)
2. [Installation](#installation)
3. [Terminaux recommandes](#terminaux-recommandes)
4. [Configuration settings.json (Linux)](#configuration-settingsjson-linux)
5. [Hooks Linux](#hooks-linux)
6. [Sons et notifications](#sons-et-notifications)
7. [Deny-list Linux](#deny-list-linux)
8. [MCP recommandes](#mcp-recommandes)
9. [Status Line](#status-line)
10. [tmux sur Linux](#tmux-sur-linux)
11. [Structure du dossier .claude/](#structure-du-dossier-claude)
12. [VPS — Configuration pour Claude Code](#vps--configuration-pour-claude-code)
13. [OpenClaw sur VPS](#openclaw-sur-vps)
14. [Docker et sandboxing](#docker-et-sandboxing)
15. [Securite VPS](#securite-vps)
16. [Astuces Linux](#astuces-linux)
17. [Troubleshooting Linux](#troubleshooting-linux)

---

## Pre-requis

### Node.js
```bash
# Ubuntu/Debian
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs

# Arch
sudo pacman -S nodejs npm

# Via nvm (recommande pour gerer les versions)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install --lts
```

### Bun (necessaire pour StatusLine et command-validator)
```bash
curl -fsSL https://bun.sh/install | bash
```

### VS Code
```bash
# Ubuntu/Debian
sudo snap install code --classic
# Ou via le .deb officiel de code.visualstudio.com

# Arch
yay -S visual-studio-code-bin
```

---

## Installation

### Claude Code
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Apres installation, relancer le terminal ou `source ~/.bashrc` / `source ~/.zshrc`.
Verifier : `claude` puis taper "qui suis-je ?"

### Config Melvynx OG (gratuite)
```bash
npx ig-blueprint-cli
```

Installe automatiquement :
- Backup config existante dans `backups/`
- settings.json (bypass + deny list)
- Hooks (command validator, notifications)
- Skills (/apex, /oneshot, /commit, /fix-errors, /fix-grammar, etc.)
- Status Line (necessite bun)

### Regle fondamentale
**JAMAIS lancer Claude Code a la racine du systeme** (`/`). Toujours `cd` dans un dossier projet d'abord.

---

## Terminaux recommandes

| Terminal | Description | Recommandation |
|----------|-------------|----------------|
| **Ghostty** | Terminal rapide et configurable (disponible sur Linux) | **★★★ Meilleur choix** |
| **Kitty** | Terminal GPU-accelere, tres configurable | ★★ Excellent |
| **Alacritty** | Terminal GPU, minimaliste | ★★ Performant |
| **GNOME Terminal** | Terminal par defaut GNOME | ★ Basique |
| **Konsole** | Terminal par defaut KDE | ★ Basique |
| **tmux** | Multiplexeur (indispensable sur VPS sans GUI) | **★★★ Obligatoire VPS** |

**Note** : CEMUX (recommande par Melvynx) est macOS-only (Swift/AppKit). Ghostty est le meilleur equivalent Linux.

---

## Configuration settings.json (Linux)

Fichier : `~/.claude/settings.json`

```json
{
  "$schema": "https://claude.ai/schemas/claude-code-settings.json",
  "defaultMode": "bypassPermissions",
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "toolSearchMode": "auto",
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo rm *)",
      "Bash(sudo rm -rf *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force*)",
      "Bash(git push -f *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git reset --hard*)",
      "Bash(mkfs*)",
      "Bash(dd if=*)",
      "Bash(curl *| bash*)",
      "Bash(curl *| sh*)",
      "Bash(wget *| bash*)",
      "Bash(wget *| sh*)",
      "Bash(shutdown*)",
      "Bash(reboot*)",
      "Bash(init 0*)",
      "Bash(systemctl poweroff*)"
    ]
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bun ~/.claude/scripts/command-validator/src/cli.ts"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "paplay /usr/share/sounds/freedesktop/stereo/complete.oga 2>/dev/null || aplay /usr/share/sounds/sound-icons/glass-water-1.wav 2>/dev/null || echo -e '\\a'",
            "async": true
          }
        ]
      }
    ]
  }
}
```

---

## Hooks Linux

### 3 hooks recommandes

| Hook | Declencheur | Commande Linux | Usage |
|------|------------|----------------|-------|
| **PreToolUse** | Avant execution outil | `bun command-validator/src/cli.ts` | Valide les commandes (AST-level) |
| **Stop** | Fin de tache | `paplay` ou `aplay` (voir ci-dessous) | Son "tache terminee" |
| **Notification** | Claude a besoin d'attention | Son different | Son "question en attente" |

### Sons sur Linux (alternatives a afplay macOS)

| Outil | Backend | Commande |
|-------|---------|----------|
| `paplay` | PulseAudio/PipeWire | `paplay /usr/share/sounds/freedesktop/stereo/complete.oga` |
| `aplay` | ALSA | `aplay /usr/share/sounds/sound-icons/glass-water-1.wav` |
| `mpv` | Universel | `mpv --no-video /chemin/vers/son.wav` |
| `play` | SoX | `play /chemin/vers/son.wav` |
| `echo -e '\a'` | Terminal bell | Son basique (fallback) |

### Hook Stop (avec fallback)
```json
{
  "type": "command",
  "command": "paplay /usr/share/sounds/freedesktop/stereo/complete.oga 2>/dev/null || aplay /usr/share/sounds/sound-icons/glass-water-1.wav 2>/dev/null || echo -e '\\a'",
  "async": true
}
```

### Hook Notification (son different)
```json
{
  "type": "command",
  "command": "paplay /usr/share/sounds/freedesktop/stereo/bell.oga 2>/dev/null || echo -e '\\a'",
  "async": true
}
```

### Sur VPS sans son
Sur un VPS headless, les sons ne fonctionnent pas. Alternatives :
- Notification Telegram (via curl + bot API)
- Notification push (via ntfy.sh)
- Email (via sendmail/msmtp)

```json
{
  "type": "command",
  "command": "curl -s -X POST 'https://api.telegram.org/bot<TOKEN>/sendMessage' -d 'chat_id=<ID>&text=Claude Code : tache terminee'",
  "async": true
}
```

---

## Sons et notifications

### Sons freedesktop (standard Linux)
```bash
ls /usr/share/sounds/freedesktop/stereo/
# alarm-clock-elapsed.oga  bell.oga  complete.oga
# dialog-error.oga  message.oga  trash-empty.oga
# ... et plus
```

### Installer les sons si absents
```bash
# Ubuntu/Debian
sudo apt install sound-theme-freedesktop

# Arch
sudo pacman -S sound-theme-freedesktop
```

### Installer les outils audio
```bash
# PulseAudio
sudo apt install pulseaudio-utils  # pour paplay

# ALSA
sudo apt install alsa-utils  # pour aplay

# SoX (universel)
sudo apt install sox
```

---

## Deny-list Linux

Commandes dangereuses specifiques a Linux :

```json
"permissions": {
  "deny": [
    "Bash(rm -rf *)",
    "Bash(sudo rm *)",
    "Bash(sudo rm -rf *)",
    "Bash(chmod 777 *)",
    "Bash(mkfs*)",
    "Bash(dd if=*)",
    "Bash(fdisk*)",
    "Bash(parted*)",
    "Bash(shutdown*)",
    "Bash(reboot*)",
    "Bash(init 0*)",
    "Bash(systemctl poweroff*)",
    "Bash(systemctl reboot*)",
    "Bash(iptables -F*)",
    "Bash(ufw disable*)",
    "Bash(systemctl stop fail2ban*)",
    "Bash(git push --force*)",
    "Bash(git reset --hard*)",
    "Bash(curl *| bash*)",
    "Bash(curl *| sh*)",
    "Bash(wget *| bash*)",
    "Bash(npm publish*)"
  ]
}
```

### Double securite (CLAUDE.md)
```
- JAMAIS utiliser rm -rf
- JAMAIS chmod 777
- JAMAIS sudo rm
- JAMAIS desactiver ufw ou fail2ban
- JAMAIS curl | bash
- Utiliser trash-cli au lieu de rm
```

### Installer trash-cli
```bash
# Ubuntu/Debian
sudo apt install trash-cli
# Utiliser : trash-put fichier.txt (au lieu de rm)

# Arch
sudo pacman -S trash-cli
```

---

## MCP recommandes

```bash
claude mcp add --scope user context7
claude mcp add --scope user exa
```

JAMAIS plus de 3 MCP simultanement.

---

## Status Line

### Installation
```bash
# Prerequis : bun installe
bun --version

# Installe automatiquement par Blueprint CLI
npx ig-blueprint-cli
```

### Si ne fonctionne pas
- Verifier que bun est installe : `bun --version`
- `/debug` dans Claude Code pour auto-reparer

---

## tmux sur Linux

### Installation
```bash
# Ubuntu/Debian
sudo apt install tmux

# Arch
sudo pacman -S tmux

# Fedora
sudo dnf install tmux
```

### Raccourcis essentiels

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `Ctrl+A C` | Nouveau terminal (window) |
| `Shift+fleches` | Switcher entre windows |
| `Ctrl+A S` | Split en deux |
| `Ctrl+A D` | Detacher (quitter sans fermer) |
| `tmux attach` / `tmux a` | Rattacher la session |
| `Ctrl+A ,` | Renommer la window |
| `Ctrl+A $` | Renommer la session |

### tmux sur VPS (indispensable)
Sur un VPS, tmux est **obligatoire** :
- Permet de detacher la session sans tuer Claude Code
- Permet de reconnecter apres deconnexion SSH
- Permet de lancer plusieurs Claude Code en parallele

```bash
# Lancer Claude Code dans tmux
tmux new -s claude
claude

# Detacher : Ctrl+A D
# Reconnecter : tmux a -t claude
```

---

## Structure du dossier .claude/

```
~/.claude/
├── CLAUDE.md             # Memoire globale
├── settings.json         # Config partagee
├── settings.local.json   # Config locale
├── rules/                # Regles persistantes (.md)
├── skills/               # Skills (SKILL.md entry point)
│   └── disabled/         # Deplacer ici pour desactiver
├── agents/               # Agents custom
├── scripts/              # command-validator, statusline
├── projects/             # Historique sessions
├── plans/                # Plans generes
├── tasks/                # Taches executees
└── teams/                # Equipes (JSON)
```

---

## VPS — Configuration pour Claude Code

### VPS recommande par Melvynx
**Hetzner CAX21/CAX31** (ARM, ~$7.59/mois). IPv4 obligatoire.

### Setup initial VPS
```bash
# Mettre a jour
apt update && apt upgrade -y

# Installer les outils de base
apt install -y curl git tmux htop

# Installer Node.js
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt install -y nodejs

# Installer bun
curl -fsSL https://bun.sh/install | bash

# Installer Claude Code
curl -fsSL https://claude.ai/install.sh | bash
```

### Bypass permission sur VPS (hack Melvynx)
```bash
# Claude croit etre dans une sandbox et accepte le bypass
SANDBOX=1 claude

# Mettre dans .bashrc pour permanent
echo 'export SANDBOX=1' >> ~/.bashrc
```

---

## OpenClaw sur VPS

### Concept
Controler Claude Code a distance via Telegram (texte + audio). Lance des agents en arriere-plan.

### Installation
```bash
# Sur le VPS avec Docker
npx openclaw-vps-setup
```

### Workflow OpenClaw
1. Message/audio Telegram avec description de la feature
2. Clone worktree → npm install → GitHub Issue → lance apex --pr
3. Notification Telegram avec lien PR
4. "Merge la PR et delete le worktree"

### Modeles recommandes

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2/$12 |
| Cron jobs | Gemini 3 Flash | Tres faible |
| Agents code | Claude Opus | Abonnement |

### Skills OpenClaw (remplacent les MCP)
Un skill = SKILL.md + execute.sh. Plus leger que MCP.
Skills : Cloudflare, CodeLine, Dub, FlightData, Front, Google Image, LinkedIn Post, Lumail, Mercury, Porkbun, SaveIt, TypeFully, Vercel, YouTube Comments

### Fichiers de configuration
- `soul.md` : identite/personnalite du bot
- `memory/` : memoire quotidienne
- `canvas/index.html` : interface web locale
- `.env` : variables d'environnement

---

## Docker et sandboxing

### Pourquoi Docker sur VPS ?
- Isole Claude Code du systeme hote
- Limite les degats en cas de commande dangereuse
- Facile a reconstruire si probleme

### Docker basique pour Claude Code
```dockerfile
FROM node:lts
RUN curl -fsSL https://claude.ai/install.sh | bash
RUN curl -fsSL https://bun.sh/install | bash
WORKDIR /workspace
CMD ["claude"]
```

### Docker Compose pour OpenClaw
```yaml
version: "3.8"
services:
  openclaw:
    build: .
    environment:
      - SANDBOX=1
    volumes:
      - ./workspace:/workspace
      - ./config:/root/.claude
    restart: unless-stopped
```

---

## Securite VPS

### Checklist securite obligatoire

```bash
# 1. UFW (firewall)
apt install ufw
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh
ufw allow 22/tcp
ufw enable

# 2. Fail2Ban
apt install fail2ban
systemctl enable fail2ban

# 3. Desactiver password SSH
# Dans /etc/ssh/sshd_config :
# PasswordAuthentication no
# PubkeyAuthentication yes
systemctl restart sshd

# 4. Whitelister vos IPs AVANT d'activer le firewall
ufw allow from <VOTRE_IP> to any port 22
```

### Commandes d'audit securite
```bash
# Audit OpenClaw
claudebot security-audit

# Verifier les connexions
ss -tlnp
netstat -tlnp

# Verifier les processus
htop
```

### Precautions
- Ne PAS activer 1Password sur le VPS (acces a toutes les cles)
- Telegram est plus securise que l'interface web d'OpenClaw
- Le gateway OpenClaw ouvre le port localhost 18789 — verifier qu'il n'est pas expose

---

## Astuces Linux

### Installer trash-cli (alternative a rm)
```bash
# Ubuntu/Debian
sudo apt install trash-cli

# Arch
sudo pacman -S trash-cli

# Usage
trash-put fichier.txt     # Au lieu de rm
trash-list                 # Voir le contenu de la corbeille
trash-restore              # Restaurer un fichier
trash-empty                # Vider la corbeille
```

### Dossier "CC" — Fourre-tout Claude Code
```bash
mkdir ~/cc
echo 'alias cdcc="cd ~/cc"' >> ~/.bashrc
source ~/.bashrc
```

### Lancer Claude Code au demarrage (systemd)
```ini
# ~/.config/systemd/user/claude-agent.service
[Unit]
Description=Claude Code Agent
After=network.target

[Service]
Type=simple
Environment=SANDBOX=1
ExecStart=/usr/local/bin/claude --session-name "agent"
Restart=on-failure
WorkingDirectory=/home/user/projet

[Install]
WantedBy=default.target
```

```bash
systemctl --user enable claude-agent
systemctl --user start claude-agent
```

### Notifications push sur VPS (ntfy.sh)
```bash
# Notification gratuite via ntfy.sh
curl -d "Claude Code : tache terminee" ntfy.sh/mon-topic-secret

# Dans un hook Stop :
"command": "curl -s -d 'Tache terminee' ntfy.sh/mon-topic"
```

---

## Troubleshooting Linux

### Claude Code ne trouve pas la commande `claude`
```bash
source ~/.bashrc
# Ou verifier le PATH :
echo $PATH | tr ':' '\n' | grep claude
which claude
```

### Bun ne fonctionne pas
```bash
# Reinstaller
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
bun --version
```

### Sons ne fonctionnent pas
```bash
# Verifier PulseAudio/PipeWire
pactl info

# Installer les outils
sudo apt install pulseaudio-utils sound-theme-freedesktop

# Tester
paplay /usr/share/sounds/freedesktop/stereo/complete.oga
```

### Permissions refusees sur .claude/
```bash
chmod -R u+rw ~/.claude/
ls -la ~/.claude/
```

### VPS : Claude Code tue par OOM Killer
```bash
# Verifier la RAM
free -h

# Augmenter le swap
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```

### tmux : session perdue apres reboot
```bash
# tmux ne persiste pas les sessions apres reboot
# Solution : tmux-resurrect plugin
git clone https://github.com/tmux-plugins/tmux-resurrect ~/.tmux/plugins/tmux-resurrect
# Ajouter dans ~/.tmux.conf :
# run-shell ~/.tmux/plugins/tmux-resurrect/resurrect.tmux
```

---

## Credits

Methode basee sur la Masterclass de [Melvynx (CodeLynx)](https://codelynx.dev).
Voir README.md pour la methode complete (workflows, prompting, context engineering, anti-patterns).
