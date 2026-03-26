# Bible Claude Code — Linux (Methode Melvynx)

> **Version** : 1.1.0
> **Date** : 2026-03-26
> **Auteur** : Soly Kim (kim13) — adapte de la Bible Melvynx macOS
> **Cible** : Toute distribution Linux (Debian/Ubuntu, Fedora/RHEL, Arch/Manjaro) et VPS
> **Prerequis** : Node.js 18+, Git, terminal moderne
> **Licence** : Usage personnel et educatif
> **Sources** : Bible Melvynx macOS, repo AIBlueprint, Masterclass 4h + Setup 20min + Formation Complete, documentation officielle Claude Code
> Voir README.md pour la methode complete (workflows, prompting, context engineering)

---

## Table des matieres

1. [Installation](#1-installation)
2. [Interface et Raccourcis](#2-interface-et-raccourcis)
3. [Memoire](#3-memoire)
4. [Settings et Permissions](#4-settings-et-permissions)
5. [Securite](#5-securite)
6. [Skills](#6-skills)
7. [Hooks](#7-hooks)
8. [MCP (Model Context Protocol)](#8-mcp-model-context-protocol)
9. [Sub-Agents](#9-sub-agents)
10. [Workflows](#10-workflows)
11. [Contexte](#11-contexte)
12. [Prompting](#12-prompting)
13. [Multi-Agents et Teams](#13-multi-agents-et-teams)
14. [Pricing et Limites](#14-pricing-et-limites)
15. [Outils Recommandes Linux](#15-outils-recommandes-linux)
16. [Status Line](#16-status-line)
17. [Structure .claude](#17-structure-claude)
18. [Plugins](#18-plugins)
19. [Deploiement](#19-deploiement)
20. [OpenClaw](#20-openclaw)
21. [Anti-Patterns](#21-anti-patterns)
22. [Niveaux de Maitrise](#22-niveaux-de-maitrise)
23. [Nouveautes et Roadmap](#23-nouveautes-et-roadmap)

---

## RÈGLE #0 — DOCUMENTER AVANT TOUTE MODIFICATION (PRIORITÉ MAXIMALE)

> **Cette règle prime sur TOUTES les autres. AUCUNE exception.**

AVANT toute modification, suppression ou écrasement de fichier, code, config ou donnée :

### Ordre obligatoire : DOCUMENTER → BACKUP → AGIR → VÉRIFIER

1. **IDENTIFIER** : quel fichier/dossier sera modifié, supprimé ou écrasé
2. **DOCUMENTER** : noter l'état actuel dans `~/.claude/logs/modifications.log`
3. **SNAPSHOT** : copier l'état actuel vers `~/.claude/backups/YYYYMMDD_HHMMSS/`
4. **COMMIT GITHUB** : `git commit -m "doc-guard: snapshot avant [action] — [fichier]"`
5. **AGIR** : seulement après les 4 étapes ci-dessus
6. **VÉRIFIER** : confirmer que l'action a produit le résultat attendu

### Zones critiques (protection renforcée Linux)
- `~/.claude/settings.json`
- `~/.claude/CLAUDE.md`
- `~/.claude/rules/`, `~/.claude/memory/`, `~/.claude/hooks/`, `~/.claude/skills/`
- `/etc/` (toute modification → `sudo tar czf /tmp/etc-backup-$(date +%Y%m%d).tar.gz /etc/`)
- `/boot/`, `/usr/`, `/var/` — modifications supervisées uniquement
- `fstab`, `shadow`, `passwd`, `sudoers` — backup obligatoire avant toute modification

---

## RÈGLE #1 — AUCUNE SUPPRESSION SANS TRIPLE SAUVEGARDE

> **Cet ordre prime sur TOUTE autre instruction. AUCUNE exception.**

### Étape 1 : JUSTIFICATION EXPLICITE
Avant de proposer ou exécuter une suppression :
- Expliquer CLAIREMENT pourquoi
- Décrire l'impact
- Proposer des alternatives (archivage, déplacement)
- Attendre la CONFIRMATION EXPLICITE de l'utilisateur

### Étape 2 : TRIPLE SAUVEGARDE OBLIGATOIRE

| # | Sauvegarde | Commande Linux | Vérification |
|---|-----------|---------------|--------------|
| 1 | **Poubelle** | `trash-put fichier.txt` ou `gio trash fichier.txt` (JAMAIS `rm`) | `trash-list` ou `ls ~/.local/share/Trash/files/` |
| 2 | **Git backup** | `git commit -m "backup: [fichier] avant suppression"` | `git log --oneline -1` |
| 3 | **Changelog** | Ajouter entrée datée dans le changelog | Lire le changelog |

**SI UNE DES 3 CONDITIONS N'EST PAS REMPLIE → REFUSER LA SUPPRESSION**

### Sauvegarde automatique du système Linux
- **Timeshift** (Ubuntu/Mint) ou **Snapper** (openSUSE/Fedora) : snapshots système automatiques
- **Config Claude** : `~/.claude/` doit être dans un repo git
- **Backup /etc/** : `sudo tar czf /tmp/etc-backup-$(date +%Y%m%d).tar.gz /etc/` avant modifications système
- **LVM snapshots** (si disponible) : `sudo lvcreate --size 1G --snapshot --name snap_root /dev/vg/root`

### Commandes safe Linux
```bash
# Option 1 : trash-cli via pip (RECOMMANDÉ — le plus complet)
pip install trash-cli
trash-put fichier.txt          # Mettre à la poubelle
trash-list                     # Lister la poubelle
trash-restore                  # Restaurer
trash-empty                    # Vider la poubelle

# Option 2 : gio trash (GNOME, pré-installé)
gio trash fichier.txt

# Option 3 : trash-cli via npm
npm install -g trash-cli
trash fichier.txt

# Alias de protection (ajouter dans ~/.bashrc)
alias rm='echo "INTERDIT: utiliser trash-put ou gio trash"; false'
```

### Ce qui est considéré comme "suppression"
- `rm`, `rm -rf`, `rm -f`, `unlink`, `shred`
- Écrasement d'un fichier existant
- `> fichier` (truncate)
- `git clean`, `git reset --hard`
- `systemctl disable/stop` sans backup de la config
- Toute action irréversible

---

## 1. Installation

### Installation de Claude Code

```bash
# Methode officielle (recommandee)
curl -fsSL https://claude.ai/install.sh | bash
```

Si `claude` n'est pas trouve apres installation :

```bash
# Recharger le shell
source ~/.bashrc
# ou
source ~/.zshrc

# Verifier le PATH
echo $PATH | tr ':' '\n' | grep -i claude

# Si absent, ajouter manuellement
echo 'export PATH="$HOME/.claude/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Apres installation, relancer le terminal ou `source ~/.bashrc` / `source ~/.zshrc`.
Verifier : `claude` puis taper "qui suis-je ?"

### Config Melvynx OG (gratuite — Blueprint CLI)

```bash
npx ig-blueprint-cli
```

Installe automatiquement :
- Backup config existante dans `backups/`
- settings.json (bypass + deny list)
- Hooks (command validator, notifications)
- Skills (/apex, /oneshot, /commit, /fix-errors, /fix-grammar, etc.)
- Status Line (necessite bun)

### Prerequis : Node.js

Claude Code necessite Node.js 18+. Plusieurs methodes d'installation :

```bash
# --- Debian/Ubuntu ---
sudo apt update && sudo apt install -y nodejs npm
# ou via NodeSource (recommande pour version recente)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# --- Fedora/RHEL ---
sudo dnf install -y nodejs npm
# ou via NodeSource
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
sudo dnf install -y nodejs

# --- Arch/Manjaro ---
sudo pacman -S nodejs npm

# --- Methode universelle via nvm (RECOMMANDEE) ---
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
nvm install 20
nvm use 20
nvm alias default 20
```

### Prerequis : Bun (pour les hooks command-validator et StatusLine)

```bash
# Installation officielle
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc

# Verification
bun --version
```

### Prerequis : Git

```bash
# Debian/Ubuntu
sudo apt install -y git

# Fedora/RHEL
sudo dnf install -y git

# Arch/Manjaro
sudo pacman -S git
```

### Prerequis : VS Code (optionnel)

```bash
# Ubuntu/Debian
sudo snap install code --classic
# Ou via le .deb officiel de code.visualstudio.com

# Arch
yay -S visual-studio-code-bin
```

### Regle fondamentale

**JAMAIS lancer Claude Code a la racine du systeme** (`/`). Toujours `cd` dans un dossier projet d'abord.

### Premier lancement

```bash
# Lancer Claude Code
claude

# Premiere chose : configurer le modele
/model
# Choisir claude-sonnet-4-20250514 (par defaut) ou claude-opus-4-0-20250514

# Verifier la version
claude --version
```

### Mise a jour

```bash
# Mise a jour automatique
claude update

# ou via npm si installe globalement
npm update -g @anthropic-ai/claude-code
```

---

## 2. Interface et Raccourcis

### Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `Alt+P` | Changer de modele (Sonnet/Opus/Haiku) |
| `Tab` | Accepter suggestion |
| `Shift+Tab` | Changer mode permissions (ask/auto/bypass) |
| `Ctrl+T` | Voir les taches en cours |
| `Ctrl+O` | Transcript verbose (mode debug) |
| `Ctrl+S` | Sauvegarder le prompt courant |
| `Ctrl+C` | Annuler commande en cours |
| `Ctrl+D` | Quitter Claude Code |
| `Escape` x2 | Clear input → historique des prompts |
| `Up/Down` | Naviguer dans l'historique |

> **Note Linux** : Sur Linux, la touche Meta correspond a `Alt`. Pas de touche `Cmd` comme sur macOS.
> Certains emulateurs de terminal (Ghostty, Kitty, Alacritty) peuvent capturer `Alt` pour leurs propres raccourcis. Si un raccourci ne fonctionne pas, verifier la config du terminal.

### Commandes slash

| Commande | Description |
|----------|-------------|
| `/clear` | Reset le contexte (OBLIGATOIRE entre taches majeures) |
| `/context` | Affiche l'utilisation des tokens en % |
| `/usage` | Limites et consommation API |
| `/model` | Changer de modele en cours de session |
| `/agent` | Gerer les agents custom |
| `/mcp` | Voir les MCP connectes |
| `/voice` | Mode vocal |
| `/loop` | Prompt a intervalle regulier |
| `/init` | Generer un CLAUDE.md pour le projet courant |

### Terminaux recommandes

| Terminal | Description | Recommandation |
|----------|-------------|----------------|
| **Ghostty** | Terminal rapide et configurable (disponible sur Linux) | **Meilleur choix** |
| **Kitty** | Terminal GPU-accelere, tres configurable | Excellent |
| **Alacritty** | Terminal GPU, minimaliste | Performant |
| **WezTerm** | Multiplexer integre, config Lua | Bon choix |
| **GNOME Terminal** | Terminal par defaut GNOME | Basique |
| **Konsole** | Terminal par defaut KDE | Basique |
| **tmux** | Multiplexeur (indispensable sur VPS sans GUI) | **Obligatoire VPS** |

> **Note** : CEMUX (recommande par Melvynx) est macOS-only (Swift/AppKit). Ghostty est le meilleur equivalent Linux.

### Modes de lancement

```bash
# Mode interactif (defaut)
claude

# Mode one-shot (pipe)
echo "explique ce code" | claude

# Mode avec fichier
claude < mon_prompt.txt

# Mode avec session nommee
claude --session-name "JARVIS: migration DB"

# Mode headless (CI/CD)
claude --headless --prompt "lint et fix tous les fichiers"

# Mode avec modele specifique
claude --model claude-opus-4-0-20250514
```

### References dans les prompts

```bash
# Taguer un fichier
@src/main.py

# Commande bash directe
!ls -la

# Taguer une URL
@https://docs.example.com/api
```

---

## 3. Memoire

### Architecture memoire (3 niveaux — Methode Melvynx)

#### Niveau 1 — Memoire courte (RAM)
- = La conversation en cours
- Se perd au `/clear`, au changement de contexte, a la fermeture
- **Rien d'important ne doit rester uniquement ici**

#### Niveau 2 — Memoire persistante (Disque)
- `~/.claude/CLAUDE.md` — memoire globale (tous les projets)
- `./CLAUDE.md` — memoire projet (racine du repo)
- `./src/components/CLAUDE.md` — memoire de dossier (charge quand l'IA lit un fichier du dossier)
- `~/.claude/rules/` — regles modulaires ciblees
- `~/.claude/memory/` — fichiers de reference detailles

#### Niveau 3 — Memoire contextuelle (chargee a la demande)
- Rules avec `globs:` — chargees uniquement quand un fichier correspondant est touche
- Exemples : regles JSON chargees quand on touche un `.json`, regles Python quand on touche un `.py`

### Chemins sur Linux

```
~/.claude/
  CLAUDE.md              # Memoire globale (identique macOS)
  settings.json          # Permissions et hooks
  rules/                 # Regles modulaires
    01-security.md
    02-memory.md
    20-json.md           # globs: ["*.json"]
    21-git.md            # globs: ["*.gitconfig", ".git/**"]
  memory/                # Fichiers de reference
    01_architecture.md
    02_lessons_log.md
    03_conventions.md
  skills/                # Skills custom
  agents/                # Agents custom
  hooks/                 # Scripts de hooks
  bible/                 # Bible Melvynx
  plugins/               # Plugins Claude Code
```

### CLAUDE.md — Structure recommandee

```markdown
# CLAUDE.md — [Nom du projet]

## Stack
- [Framework, langage, runtime, DB...]

## Architecture
- [Structure des dossiers, patterns utilises]

## Commandes
- `pnpm dev` — lancer le dev server
- `pnpm test` — lancer les tests
- `pnpm lint` — linter le code

## Conventions
- [Regles de code, nommage, style]

## Contraintes
- [Chemins proteges, services dependants]
```

### Regles conditionnelles (globs)

```markdown
---
globs: ["*.py"]
alwaysApply: false
---
# Regles Python
- Utiliser Ruff/Black pour le formatage
- 4 espaces d'indentation
- Type hints obligatoires
- Docstrings Google-style
```

### Commandes memoire

```bash
# Generer un CLAUDE.md pour le projet courant
/init

# Verifier la consommation contexte
/context
```

### Regle d'or
- **CLAUDE.md** : max 500 lignes (anti-pattern mega-CLAUDE.md)
- Les details longs vont dans `rules/` ou `memory/`
- `/clear` entre les taches majeures pour eviter l'autocompact

### Protocole de persistance mémoire (Règle #0 appliquée)

| Type d'information | Destination | Exemple |
|-------------------|-------------|---------|
| Règle comportementale | `~/.claude/rules/` | "JAMAIS rm sans backup" |
| Architecture/décision | `~/.claude/memory/` | "Service X dépend de Y" |
| Erreur corrigée | `~/.claude/memory/02_lessons_log.md` | "Bug X résolu par Y" |
| Préférence utilisateur | `~/.claude/rules/11-auto-learning-live.md` | "Toujours français" |

### Sauvegarde de la mémoire
- `~/.claude/` DOIT être dans un repo git
- Commit automatique après chaque modification de rules/ ou memory/
- JAMAIS supprimer un fichier de mémoire sans triple sauvegarde (Règle #1)
- JAMAIS écraser un fichier de mémoire — toujours fusionner (append)

### Vérification périodique
```bash
ls -la ~/.claude/memory/
git -C ~/.claude log --oneline -5  # Si repo git
timeshift --list 2>/dev/null || snapper list 2>/dev/null  # Snapshots système
```

---

## 4. Settings et Permissions

### Emplacement

```
~/.claude/settings.json
```

### Structure complete recommandee

```json
{
  "$schema": "https://claude.ai/schemas/claude-code-settings.json",
  "defaultMode": "bypassPermissions",
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "toolSearchMode": "auto",
  "permissions": {
    "allow": [
      "Bash(git status)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(ls:*)",
      "Bash(cat:*)",
      "Bash(head:*)",
      "Bash(tail:*)",
      "Bash(find:*)",
      "Bash(grep:*)",
      "Bash(rg:*)",
      "Bash(fd:*)",
      "Bash(wc:*)",
      "Bash(echo:*)",
      "Bash(pwd)",
      "Bash(whoami)",
      "Bash(date)",
      "Bash(uname:*)",
      "Bash(which:*)",
      "Bash(node:*)",
      "Bash(npm:*)",
      "Bash(npx:*)",
      "Bash(pnpm:*)",
      "Bash(bun:*)",
      "Bash(python3:*)",
      "Bash(pip:*)",
      "Read",
      "Write",
      "Edit",
      "Glob",
      "Grep",
      "WebFetch",
      "WebSearch"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(rm -rf *)",
      "Bash(rm -r:*)",
      "Bash(sudo rm:*)",
      "Bash(sudo rm *)",
      "Bash(sudo rm -rf *)",
      "Bash(rmdir:*)",
      "Bash(chmod 777:*)",
      "Bash(chmod 777 *)",
      "Bash(dd:*)",
      "Bash(dd if=*)",
      "Bash(mkfs:*)",
      "Bash(mkfs*)",
      "Bash(fdisk:*)",
      "Bash(fdisk*)",
      "Bash(parted:*)",
      "Bash(parted*)",
      "Bash(wipefs:*)",
      "Bash(shred:*)",
      "Bash(> /dev/sd:*)",
      "Bash(curl:*|bash)",
      "Bash(curl *| bash*)",
      "Bash(curl *| sh*)",
      "Bash(wget:*|bash)",
      "Bash(wget *| bash*)",
      "Bash(wget *| sh*)",
      "Bash(git push --force:*)",
      "Bash(git push --force*)",
      "Bash(git push -f *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git reset --hard:*)",
      "Bash(git reset --hard*)",
      "Bash(git clean -f:*)",
      "Bash(systemctl stop:*)",
      "Bash(systemctl disable:*)",
      "Bash(systemctl poweroff*)",
      "Bash(systemctl reboot*)",
      "Bash(systemctl stop fail2ban*)",
      "Bash(iptables -F:*)",
      "Bash(iptables -F*)",
      "Bash(ufw disable:*)",
      "Bash(ufw disable*)",
      "Bash(nft flush:*)",
      "Bash(shutdown*)",
      "Bash(reboot*)",
      "Bash(init 0*)",
      "Bash(npm publish*)"
    ]
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bun ~/.claude/hooks/command-validator/src/cli.ts \"$TOOL_INPUT\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "paplay /usr/share/sounds/freedesktop/stereo/complete.oga 2>/dev/null || aplay /usr/share/sounds/sound-icons/trumpet-12.wav 2>/dev/null || echo -e '\\a'",
            "async": true
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' \"$NOTIFICATION_MESSAGE\" --icon=dialog-information && paplay /usr/share/sounds/freedesktop/stereo/message.oga 2>/dev/null",
            "async": true
          }
        ]
      }
    ]
  },
  "mcpServers": {},
  "autoSearchThreshold": 10,
  "enableAllProjectMcpServers": true
}
```

### Modes de permissions

| Mode | Description | Raccourci |
|------|-------------|-----------|
| `ask` | Demande confirmation pour chaque action | Defaut |
| `auto` | Accepte automatiquement (allow-list) | `Shift+Tab` |
| `bypass` | Accepte TOUT (dangereux) | `Shift+Tab` x2 |

### Deny-list — commandes Linux a bloquer

```json
"deny": [
  "Bash(rm -rf:*)",
  "Bash(rm -rf *)",
  "Bash(rm -r:*)",
  "Bash(sudo rm:*)",
  "Bash(sudo rm *)",
  "Bash(sudo rm -rf *)",
  "Bash(rmdir:*)",
  "Bash(chmod 777:*)",
  "Bash(chmod 777 *)",
  "Bash(dd:*)",
  "Bash(dd if=*)",
  "Bash(mkfs:*)",
  "Bash(mkfs*)",
  "Bash(fdisk:*)",
  "Bash(fdisk*)",
  "Bash(parted:*)",
  "Bash(parted*)",
  "Bash(wipefs:*)",
  "Bash(shred:*)",
  "Bash(> /dev/sd:*)",
  "Bash(curl:*|bash)",
  "Bash(curl *| bash*)",
  "Bash(curl *| sh*)",
  "Bash(wget:*|bash)",
  "Bash(wget *| bash*)",
  "Bash(wget *| sh*)",
  "Bash(git push --force:*)",
  "Bash(git push --force*)",
  "Bash(git push -f *)",
  "Bash(git push --force-with-lease origin main*)",
  "Bash(git reset --hard:*)",
  "Bash(git reset --hard*)",
  "Bash(git clean -f:*)",
  "Bash(systemctl stop:*)",
  "Bash(systemctl disable:*)",
  "Bash(systemctl poweroff*)",
  "Bash(systemctl reboot*)",
  "Bash(systemctl stop fail2ban*)",
  "Bash(iptables -F:*)",
  "Bash(iptables -F*)",
  "Bash(ufw disable:*)",
  "Bash(ufw disable*)",
  "Bash(nft flush:*)",
  "Bash(shutdown*)",
  "Bash(reboot*)",
  "Bash(init 0*)",
  "Bash(npm publish*)"
]
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

### Trash au lieu de rm

Sur Linux, `rm` envoie directement au neant. Toujours utiliser une alternative :

```bash
# Option 1 : trash-cli (Python, le plus complet — RECOMMANDE)
pip install trash-cli
# Usage : trash-put fichier.txt
# Lister : trash-list
# Restaurer : trash-restore
# Vider : trash-empty

# Option 2 : trash-cli (npm, universel)
npm install -g trash-cli
# Usage : trash fichier.txt

# Option 3 : gio trash (GNOME, pre-installe sur Ubuntu/Fedora GNOME)
gio trash fichier.txt

# Option 4 : kioclient (KDE)
kioclient move fichier.txt trash:/

# Installation via le gestionnaire de paquets
# Ubuntu/Debian
sudo apt install trash-cli
# Arch
sudo pacman -S trash-cli

# Verification : le fichier va dans ~/.local/share/Trash/
ls ~/.local/share/Trash/files/
```

Wrapper recommande dans `~/.bashrc` :

```bash
# Protection anti-rm dans .bashrc
alias rm='echo "INTERDIT: utiliser trash-put ou gio trash"; false'
# Pour forcer rm en cas de besoin absolu : \rm ou command rm
```

---

## 5. Securite

### Hook PreToolUse : command-validator (Methode Melvynx)

Le hook principal de securite analyse les commandes AVANT execution via un AST (pas de regex naif).

```bash
# Installation
cd ~/.claude/hooks
git clone https://github.com/Melvynx/aiblueprint.git /tmp/aiblueprint
cp -r /tmp/aiblueprint/scripts/command-validator ~/.claude/hooks/
cd ~/.claude/hooks/command-validator
bun install
```

Configuration dans `settings.json` :

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bun ~/.claude/hooks/command-validator/src/cli.ts \"$TOOL_INPUT\""
          }
        ]
      }
    ]
  }
}
```

### Deny-list specifique Linux

Commandes a toujours bloquer :

| Commande | Raison |
|----------|--------|
| `rm -rf /` | Destruction totale du systeme |
| `dd if=/dev/zero of=/dev/sda` | Ecrase le disque entier |
| `mkfs.*` | Formate une partition |
| `:(){ :\|:& };:` | Fork bomb |
| `chmod -R 777 /` | Casse toutes les permissions |
| `iptables -F` | Supprime toutes les regles firewall |
| `nft flush ruleset` | Supprime toutes les regles nftables |
| `systemctl stop sshd` | Coupe l'acces SSH |
| `curl ... \| bash` | Execution de code distant non verifie |
| `wget ... \| bash` | Idem |
| `> /dev/sda` | Ecrase le MBR/GPT |
| `shred` | Destruction irreversible |
| `shutdown` / `reboot` / `init 0` | Arret/redemarrage non controle |
| `npm publish` | Publication non autorisee |

### Protection des chemins critiques

Chemins a JAMAIS modifier sans backup prealable :

```
/etc/                    # Configuration systeme
/boot/                   # Noyau et bootloader
/usr/                    # Binaires systeme
~/.ssh/                  # Cles SSH
~/.gnupg/                # Cles GPG
~/.config/               # Config utilisateur
/etc/passwd              # Comptes utilisateurs
/etc/shadow              # Mots de passe
/etc/sudoers             # Privileges sudo
/etc/fstab               # Points de montage
/etc/ssh/sshd_config     # Config SSH serveur
```

### Commandes de verification avant action

```bash
# Tester la config SSH avant de redemarrer sshd
sudo sshd -t

# Tester la config nginx avant reload
sudo nginx -t

# Tester la config Apache avant reload
sudo apachectl configtest

# Verifier les regles firewall avant de les appliquer
sudo iptables -L -n --line-numbers
sudo ufw status verbose
sudo nft list ruleset

# Verifier l'espace disque avant operation lourde
df -h
du -sh /chemin/cible
```

### Backup avant toute modification systeme

```bash
# Backup d'un fichier de config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak.$(date +%Y%m%d_%H%M%S)

# Backup complet /etc
sudo tar czf /tmp/etc-backup-$(date +%Y%m%d).tar.gz /etc/

# Snapshot LVM (si disponible)
sudo lvcreate --size 1G --snapshot --name snap_root /dev/vg/root
```

---

## 6. Skills

### Principe

Les Skills sont des prompts reutilisables stockes dans `~/.claude/skills/` ou `.claude/skills/` (projet).
Chaque skill est un dossier contenant un `SKILL.md` (point d'entree) et optionnellement des `steps/`.

### Structure

```
~/.claude/skills/
  apex/
    SKILL.md
    steps/
      step1-init.md
      step2-analyse.md
      step3-plan.md
      step4-execute.md
      step5-verify.md
  commit/
    SKILL.md
  oneshot/
    SKILL.md
  debug/
    SKILL.md
  ...
```

### Skills Melvynx (15 officiels)

| Skill | Usage | Description |
|-------|-------|-------------|
| `apex` | `/apex` | Workflow complet feature (explore > plan > code > test) |
| `oneshot` | `/oneshot` | Quick fix en une passe |
| `debug` | `/debug` | Debug methodique avec logs |
| `commit` | `/commit` | Commit propre Conventional Commits |
| `create-pr` | `/create-pr` | Pull Request avec description |
| `fix-errors` | `/fix-errors` | Correction erreurs build/lint |
| `fix-grammar` | `/fix-grammar` | Correction orthographe/grammaire |
| `fix-pr-comments` | `/fix-pr-comments` | Corriger les commentaires de PR |
| `merge` | `/merge` | Merge propre avec verifications |
| `claude-memory` | `/memorize` | Gestion memoire CLAUDE.md + rules |
| `prompt-creator` | `/prompt_creator` | Generer des prompts optimises |
| `ralph-loop` | `/ralph-loop` | Boucle de feedback iterative |
| `skill-creator` | `/skill-creator` | Creer de nouveaux skills |
| `subagent-creator` | `/subagent-creator` | Creer des sub-agents |
| `ultrathink` | `/ultrathink` | Reflexion profonde (2+ min) |

### Creer un skill custom

```bash
mkdir -p ~/.claude/skills/mon-skill
```

Fichier `~/.claude/skills/mon-skill/SKILL.md` :

```markdown
---
name: mon-skill
description: Description courte du skill
---

# Mon Skill

## Etapes
1. Lire le fichier cible
2. Analyser le probleme
3. Proposer une solution
4. Implementer
5. Verifier

## Regles
- Toujours lire 3 fichiers similaires avant de modifier
- Tester apres chaque changement
```

### Desactiver un skill temporairement

```bash
# Deplacer le skill dans le dossier disabled/
mkdir -p ~/.claude/skills/disabled
mv ~/.claude/skills/mon-skill ~/.claude/skills/disabled/mon-skill

# Pour reactiver : le remettre dans skills/
mv ~/.claude/skills/disabled/mon-skill ~/.claude/skills/mon-skill
```

### Prompt Discovery (anti "Lost in the Middle")

Pour les skills complexes, decouper en steps :

```
steps/
  step1-init.md       # Charge le contexte initial
  step2-analyse.md    # Analyse le probleme
  step3-plan.md       # Planifie la solution
  step4-execute.md    # Execute le plan
  step5-verify.md     # Verifie le resultat
```

Chaque step est lu juste avant execution = toujours "recent" dans le contexte du modele.

### Skill de sauvegarde automatique

```
# ~/.claude/skills/auto-backup/SKILL.md
---
name: auto-backup
description: Sauvegarde automatique de la config Claude et du système
---

## Fonctionnalités
1. Backup quotidien de ~/.claude/ (settings, rules, memory, skills, agents)
2. Vérification Timeshift/Snapper actif
3. Snapshot avant toute modification de zone critique
4. Commit git automatique des changements de config

## Commandes
- `backup-claude` : sauvegarde complète ~/.claude/
- `backup-check` : vérifie que toutes les sauvegardes sont à jour
- `backup-restore [date]` : restaure une config depuis un backup

## Automatisation Linux
- Systemd timer : `claude-auto-backup.timer` (quotidien)
- Cron : `0 3 * * * cd ~/.claude && git add -A && git commit -m "auto-backup $(date)"`
- Timeshift/Snapper : vérifier `timeshift --list` ou `snapper list` avant opérations risquées
```

---

## 7. Hooks

### Types de hooks

| Type | Declencheur | Usage |
|------|-------------|-------|
| `PreToolUse` | Avant chaque appel d'outil | Validation, securite |
| `PostToolUse` | Apres chaque appel d'outil | Logging, verification |
| `Stop` | Fin de la reponse de Claude | Son, notification |
| `Notification` | Notification envoyee | Alerte systeme |

### 3 hooks recommandes (Melvynx)

| Hook | Declencheur | Commande Linux | Usage |
|------|------------|----------------|-------|
| **PreToolUse** | Avant execution outil | `bun command-validator/src/cli.ts` | Valide les commandes (AST-level) |
| **Stop** | Fin de tache | `paplay` ou `aplay` (voir ci-dessous) | Son "tache terminee" |
| **Notification** | Claude a besoin d'attention | Son different + `notify-send` | Son "question en attente" |

### Hook Stop : son de fin de tache (Linux)

Plusieurs options selon l'environnement audio :

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "paplay /usr/share/sounds/freedesktop/stereo/complete.oga 2>/dev/null || aplay /usr/share/sounds/sound-icons/trumpet-12.wav 2>/dev/null || echo -e '\\a'",
            "async": true
          }
        ]
      }
    ]
  }
}
```

#### Details des commandes audio Linux

```bash
# PulseAudio/PipeWire (la plupart des distros modernes)
paplay /usr/share/sounds/freedesktop/stereo/complete.oga

# ALSA (fallback, toujours disponible)
aplay /usr/share/sounds/sound-icons/trumpet-12.wav

# SoX (universel)
play /chemin/vers/son.wav

# mpv (universel)
mpv --no-video /chemin/vers/son.wav

# Rechercher les sons disponibles
find /usr/share/sounds -name "*.oga" -o -name "*.wav" 2>/dev/null | head -20

# Installer des sons supplementaires
# Debian/Ubuntu
sudo apt install sound-theme-freedesktop
# Fedora
sudo dnf install sound-theme-freedesktop
# Arch
sudo pacman -S sound-theme-freedesktop

# Fallback ultime : beep terminal
echo -e '\a'

# Ou via speaker interne (si pcspkr charge)
beep  # apt install beep
```

### Sons freedesktop (standard Linux)
```bash
ls /usr/share/sounds/freedesktop/stereo/
# alarm-clock-elapsed.oga  bell.oga  complete.oga
# dialog-error.oga  message.oga  trash-empty.oga
# ... et plus
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

### Hook Notification : notify-send (Linux)

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' \"$NOTIFICATION_MESSAGE\" --icon=dialog-information --urgency=normal && paplay /usr/share/sounds/freedesktop/stereo/message.oga 2>/dev/null",
            "async": true
          }
        ]
      }
    ]
  }
}
```

### Hook Notification : son different (alternative)
```json
{
  "type": "command",
  "command": "paplay /usr/share/sounds/freedesktop/stereo/bell.oga 2>/dev/null || echo -e '\\a'",
  "async": true
}
```

#### Installation de notify-send

```bash
# Debian/Ubuntu (generalement pre-installe)
sudo apt install libnotify-bin

# Fedora
sudo dnf install libnotify

# Arch
sudo pacman -S libnotify
```

### Hook PreToolUse : command-validator

Voir section 5 (Securite). Configuration identique a macOS, basee sur Bun.

### Hook custom : backup automatique avant Write

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "FILE=$(echo \"$TOOL_INPUT\" | jq -r '.file_path'); [ -f \"$FILE\" ] && cp \"$FILE\" \"$FILE.bak.$(date +%Y%m%d_%H%M%S)\" || true"
          }
        ]
      }
    ]
  }
}
```

### Sur VPS sans son (notifications alternatives)

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

#### Notifications push via ntfy.sh (gratuit)
```bash
# Notification gratuite via ntfy.sh
curl -d "Claude Code : tache terminee" ntfy.sh/mon-topic-secret

# Dans un hook Stop :
"command": "curl -s -d 'Tache terminee' ntfy.sh/mon-topic"
```

---

## 8. MCP (Model Context Protocol)

### Principe

Les MCP sont des serveurs externes qui fournissent des outils supplementaires a Claude Code.
**Regle Melvynx : maximum 2-3 MCP actifs.** Chaque MCP consomme du contexte.

### MCP recommandes

| MCP | Usage | Impact contexte |
|-----|-------|-----------------|
| Context7 | Documentation a jour des libs | Faible |
| Exa.ai | Recherche web IA | Moyen |
| Memory (KG) | Knowledge graph persistant | Faible |

### Installation rapide

```bash
claude mcp add --scope user context7
claude mcp add --scope user exa
```

JAMAIS plus de 3 MCP simultanement.

### Configuration dans settings.json

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@context7/mcp-server"]
    },
    "exa": {
      "command": "npx",
      "args": ["-y", "exa-mcp-server"],
      "env": {
        "EXA_API_KEY": "votre-cle-exa"
      }
    },
    "memory": {
      "command": "npx",
      "args": ["-y", "@anthropic/memory-mcp-server"],
      "env": {
        "MEMORY_FILE_PATH": "~/.claude/memory.json"
      }
    }
  }
}
```

### Configuration MCP par projet

Fichier `.mcp.json` a la racine du projet :

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

### autoSearchThreshold

```json
{
  "autoSearchThreshold": 10
}
```

Quand les MCP representent plus de 10% du contexte, Claude active automatiquement le Tool Search pour ne charger que les outils necessaires.

### Regles MCP

- **Max 2-3 MCP actifs** simultanement
- Au-dela de 10% du contexte en MCP, la qualite degrade
- Preferer les sub-agents pour les recherches web (preservent le contexte principal)
- Verifier avec `/mcp` les serveurs connectes

---

## 9. Sub-Agents

### Principe (Methode Melvynx)

Un sub-agent s'execute dans un contexte isole (120K tokens) et retourne ~1500 mots au contexte principal.
Sans sub-agent, une recherche web consomme ~28K tokens dans le contexte principal.

### 3 sub-agents recommandes

#### 1. web-search

```
~/.claude/agents/websearch/
  AGENT.md
```

```markdown
---
name: websearch
description: Recherche web via Exa ou WebSearch
tools: ["WebSearch", "WebFetch"]
---

# Agent Web Search

Tu es un agent de recherche web. Tu recois une question et tu dois :
1. Formuler 2-3 requetes de recherche pertinentes
2. Chercher via WebSearch ou Exa
3. Synthetiser les resultats en max 1500 mots
4. Citer les sources avec URLs

Retourne UNIQUEMENT les informations pertinentes, pas de blabla.
```

#### 2. explore-codebase

```markdown
---
name: explore-codebase
description: Explorer le code source du projet
tools: ["Read", "Glob", "Grep"]
---

# Agent Explore Codebase

Tu es un agent d'exploration de code. Tu recois une question sur le code et tu dois :
1. Identifier les fichiers pertinents via Glob/Grep
2. Lire les fichiers identifies
3. Analyser l'architecture et les patterns
4. Retourner un resume structure de tes decouvertes

Max 1500 mots en sortie. Etre precis et factuel.
```

#### 3. explore-docs

```markdown
---
name: explore-docs
description: Explorer la documentation du projet
tools: ["Read", "Glob", "Grep", "WebFetch"]
---

# Agent Explore Docs

Tu es un agent de documentation. Tu recois une question et tu dois :
1. Chercher dans les fichiers .md, README, docs/ du projet
2. Si pas trouve, chercher dans la documentation officielle en ligne
3. Synthetiser en max 1500 mots
4. Donner les references exactes (fichier + ligne ou URL)
```

### Usage

```
@agent websearch "derniere version de Next.js et changements majeurs"
@agent explore-codebase "comment fonctionne l'authentification dans ce projet"
```

---

## 10. Workflows

### Tableau de choix

| Tache | Workflow | Flags |
|-------|---------|-------|
| Feature complete | `/apex` | `--auto --branch --test` |
| Quick fix simple | `/oneshot` | aucun |
| Bug a debugger | `/debug` | aucun |
| Recherche profonde | `/brainstorm` | aucun (gourmand) |
| Erreurs build | `/fix-errors` | aucun (sub-agents auto) |
| Grammaire | `/fix-grammar dossier/` | aucun |
| Review code | `/simplify` | aucun (3 agents paralleles) |
| Reflexion complexe | `/ultrathink` | aucun (2+ min) |
| Monitoring | `/loop 5m commande` | intervalle |
| Tache planifiee | `/schedule` | cron persistant |
| Commit propre | `/commit` | aucun |
| Pull Request | `/create-pr` | aucun |
| Petite modif | Langage naturel | PAS de skill |

### Workflow APEX (le plus complet)

```
/apex --auto --branch --test
```

Etapes automatiques :
1. **Explore** : lit le code, comprend l'architecture
2. **Plan** : propose un plan detaille
3. **Code** : implemente le plan
4. **Test** : lance les tests, corrige si echec
5. **Commit** : commit propre Conventional Commits

### Flags disponibles

| Flag | Effet |
|------|-------|
| `--auto` | Mode automatique, pas de confirmation |
| `--examine` | Analyse approfondie avant d'agir |
| `--test` | Lance les tests apres implementation |
| `--economy` | Economise les tokens (abonnement 20$) |
| `--branch` | Cree une branche pour le changement |
| `--pr` | Cree une PR apres le travail |
| `--teams` | Active les multi-agents (work trees) |
| `--plan` | S'arrete apres la planification |

### Regles d'usage des workflows

- Ne PAS taguer de fichiers (`@fichier`) avec Apex/OneShot — laisser l'exploration les trouver
- Ne PAS utiliser `/batch` — preferer main agent + sub-agents
- `/clear` OBLIGATOIRE entre taches majeures
- Preferer le langage naturel pour petites modifs (economise le contexte)
- `--economy` pour economiser les tokens si abonnement limite

---

## 11. Contexte

### Chiffres cles

| Parametre | Valeur |
|-----------|--------|
| Contexte max | 200K tokens |
| Tokens au demarrage | ~27K (system prompt + tools + skills + CLAUDE.md + rules + MCP) |
| Autocompact | ~96-99% (degrade la qualite) |
| Sub-agent isole | 120K tokens, retourne ~1500 mots |

### Probleme "Lost in the Middle"

Les transformers privilegient le **debut** et la **fin** du contexte. Le milieu est progressivement ignore.
Quand le contexte grossit, les instructions de CLAUDE.md finissent "au milieu" et sont moins respectees.

### Solutions

1. **`/clear` entre les taches** — reset le contexte
2. **Prompt Discovery** — decouper les skills en steps/ (chaque step lu juste avant execution)
3. **Sub-agents** — isolent les recherches dans un contexte separe
4. **Rules conditionnelles** — chargees uniquement quand pertinent (globs)
5. **Langage naturel** pour les petites modifs (economise le contexte)

### Surveiller le contexte

```bash
# Dans Claude Code
/context

# Regles
# < 50% : tout va bien
# 50-80% : attention, eviter les recherches larges
# > 80% : /clear IMMEDIATEMENT
# > 96% : autocompact (perte de qualite)
```

### Debug du contexte sur Linux : mitmproxy

`mitmproxy` permet d'intercepter les requetes API pour voir exactement ce qui est envoye au modele.

```bash
# Installation
# Debian/Ubuntu
sudo apt install mitmproxy
# Fedora
sudo dnf install mitmproxy
# Arch
sudo pacman -S mitmproxy
# ou via pip (universel)
pip install mitmproxy

# Lancement
mitmproxy --mode regular --listen-port 8080

# Dans un autre terminal, lancer Claude avec le proxy
HTTPS_PROXY=http://localhost:8080 claude

# Dans mitmproxy, filtrer sur api.anthropic.com
# On voit exactement le contenu du contexte envoye
```

Utiliser mitmproxy permet de :
- Voir la taille exacte du contexte envoye
- Identifier les tokens gaspilles
- Verifier quels MCP/rules/skills sont charges
- Debugger les problemes de memoire

---

## 12. Prompting

### Greenfield (nouveau projet)

1. Specifier la stack : "cree un projet Vite avec React, TypeScript, Tailwind et shadcn"
2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier les conventions techniques (camelCase etc.) au premier run
5. Utiliser une boilerplate existante pour imposer les patterns

### Brownfield (projet existant)

1. **Screenshots annotes** (Flameshot sur Linux) — montrer exactement le probleme
2. Donner des references internes : "style comme le composant illustration-picker"
3. Donner les ressources : chemins, images, URLs
4. Les screenshots = l'IA VOIT ce qu'elle fait

### Debug (methode la plus efficace)

1. Demander a l'IA d'ajouter des `console.log` strategiques
2. Reproduire le bug soi-meme
3. Copier la sortie console BRUTE
4. Envoyer a Claude — ne PAS dire quoi faire, exposer le PROBLEME

### Technique des 10 variations UI

```
Cree un fichier /demo/page.tsx avec 10 variations UI,
10 composants separes dans 10 fichiers.
```

Comparer, choisir la meilleure, supprimer le reste.

### Processus iteratif front-end

Screenshot -> demander modif -> re-screenshot -> ajustement -> etc.

### Commentaires comme "prompt injection"

Documenter les helpers avec des exemples d'utilisation dans les commentaires du code.
Avec la regle "lire 3 fichiers similaires", l'IA absorbe ces instructions automatiquement.

### Screenshots sur Linux

```bash
# Flameshot (RECOMMANDE — le CleanShotX de Linux)
# Debian/Ubuntu
sudo apt install flameshot
# Fedora
sudo dnf install flameshot
# Arch
sudo pacman -S flameshot
# Usage
flameshot gui  # Capture interactive avec annotation
flameshot full -p /tmp/screenshot.png  # Plein ecran

# Spectacle (KDE)
sudo apt install kde-spectacle  # Debian/Ubuntu
spectacle

# GNOME Screenshot
gnome-screenshot -a  # Zone selectionnee

# Grim + Slurp (Wayland pur)
sudo apt install grim slurp
grim -g "$(slurp)" /tmp/screenshot.png

# Depuis le terminal (pour envoyer a Claude)
flameshot full -p /tmp/screenshot.png
# Puis dans Claude Code : @/tmp/screenshot.png
```

### Anti-patterns prompting

- Ne PAS relancer un skill pour une petite modif — utiliser le langage naturel
- Ne PAS empiler les demandes pendant que l'IA travaille
- Ne PAS utiliser Chrome Headless (juge lourd et lent par Melvynx)

---

## 13. Multi-Agents et Teams

### Principe

Les Teams Claude Code permettent de lancer plusieurs agents en parallele, chacun dans un **git worktree** separe.
Maximum 4 terminaux/work trees simultanes.

### Prerequis Linux : tmux

```bash
# Debian/Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux

# Arch
sudo pacman -S tmux

# Verification
tmux -V
```

### Activation

```json
{
  "enableAgentTeams": true
}
```

### Usage avec tmux

```bash
# Creer une session tmux
tmux new-session -s claude-team

# Splitter en 4 panneaux
Ctrl+B puis %     # Split vertical
Ctrl+B puis "     # Split horizontal
Ctrl+B puis fleches  # Naviguer entre panneaux

# Dans chaque panneau, lancer un agent specialise
# Panneau 1 : agent principal
claude --session-name "TEAM: orchestrateur"

# Panneau 2 : agent backend
claude --session-name "TEAM: backend API"

# Panneau 3 : agent frontend
claude --session-name "TEAM: frontend UI"

# Panneau 4 : agent tests
claude --session-name "TEAM: tests E2E"
```

### Raccourcis tmux essentiels

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `tmux new -s nom` | Nouvelle session nommee |
| `Ctrl+B C` | Nouveau terminal (window) |
| `Ctrl+B n/p` | Tab suivant/precedent |
| `Shift+fleches` | Switcher entre windows |
| `Ctrl+B %` | Split vertical |
| `Ctrl+B "` | Split horizontal |
| `Ctrl+B D` | Detacher (quitter sans fermer) |
| `tmux attach -t nom` / `tmux a` | Rattacher la session |
| `Ctrl+B ,` | Renommer la window |
| `Ctrl+B $` | Renommer la session |

### tmux sur VPS (indispensable)

Sur un VPS, tmux est **obligatoire** :
- Permet de detacher la session sans tuer Claude Code
- Permet de reconnecter apres deconnexion SSH
- Permet de lancer plusieurs Claude Code en parallele

```bash
# Lancer Claude Code dans tmux
tmux new -s claude
claude

# Detacher : Ctrl+B D
# Reconnecter : tmux a -t claude
```

### tmux-resurrect : persister les sessions apres reboot

```bash
# tmux ne persiste pas les sessions apres reboot
# Solution : tmux-resurrect plugin
git clone https://github.com/tmux-plugins/tmux-resurrect ~/.tmux/plugins/tmux-resurrect
# Ajouter dans ~/.tmux.conf :
# run-shell ~/.tmux/plugins/tmux-resurrect/resurrect.tmux
```

### Git Worktrees (zones de modification separees)

```bash
# Creer un worktree pour chaque agent
git worktree add ../projet-backend feature/backend
git worktree add ../projet-frontend feature/frontend

# Chaque agent travaille dans son worktree
cd ../projet-backend && claude
cd ../projet-frontend && claude

# Fusionner apres
git checkout main
git merge feature/backend
git merge feature/frontend

# Nettoyer les worktrees
git worktree remove ../projet-backend
git worktree remove ../projet-frontend
```

### Regles multi-agents

- Utiliser UNIQUEMENT pour des zones de modification SEPAREES
- Maximum 4 terminaux/work trees simultanes
- Chaque agent doit avoir un perimetre clair et non chevauchant
- Le merge final doit etre fait manuellement apres verification

---

## 14. Pricing et Limites

### Abonnements Claude (2026)

| Plan | Prix | Limites approximatives |
|------|------|----------------------|
| Free | 0$ | Tres limite, pas de Claude Code |
| Pro | 20$/mois | ~45 messages Opus/jour, illimite Sonnet |
| Max 5x | 100$/mois | 5x les limites Pro |
| Max 20x | 200$/mois | 20x les limites Pro |
| API | Pay-as-you-go | Selon consommation |

### Cout par token (API, prix indicatifs)

| Modele | Input/1M tokens | Output/1M tokens |
|--------|-----------------|------------------|
| Claude Haiku | 0.25$ | 1.25$ |
| Claude Sonnet | 3$ | 15$ |
| Claude Opus | 15$ | 75$ |

### Strategies d'economie

1. **`--economy`** flag sur les workflows (utilise Haiku pour les sous-taches)
2. **`/clear`** entre les taches (evite de payer pour du contexte inutile)
3. **Sub-agents** (contexte isole, pas de pollution du principal)
4. **Langage naturel** pour les petites modifs (pas de skill = moins de tokens)
5. **Sonnet par defaut**, Opus uniquement pour les taches complexes

### Verifier sa consommation

```bash
/usage
/context
```

---

## 15. Outils Recommandes Linux

### Terminaux

| Terminal | Installation | Points forts |
|----------|-------------|-------------|
| **Ghostty** | `flatpak install flathub com.mitchellh.ghostty` | Rapide, GPU-accelere, config simple |
| **Kitty** | `sudo apt install kitty` / `sudo pacman -S kitty` | GPU-accelere, images inline, splits |
| **Alacritty** | `sudo apt install alacritty` / `cargo install alacritty` | Ultra-rapide, minimal, OpenGL |
| **WezTerm** | `flatpak install org.wezfurlong.wezterm` | Multiplexer integre, Lua config |
| **Foot** | `sudo apt install foot` | Leger, Wayland natif |
| **GNOME Terminal** | Pre-installe (GNOME) | Basique mais fiable |
| **Konsole** | Pre-installe (KDE) | Riche en fonctionnalites |

Recommandation : **Ghostty** ou **Kitty** pour la meilleure experience Claude Code.

### IDE

| IDE | Installation | Notes |
|-----|-------------|-------|
| **VS Code** | `sudo snap install code --classic` ou `.deb`/`.rpm` | Extension Claude Code disponible |
| **Cursor** | AppImage depuis cursor.com | Fork VS Code + IA integree |
| **Neovim** | `sudo apt install neovim` | Pour les adeptes vim |
| **Zed** | AppImage/Flatpak | Editeur rapide, IA integree |

### Screenshots et annotations

```bash
# Flameshot (RECOMMANDE — le CleanShotX de Linux)
sudo apt install flameshot      # Debian/Ubuntu
sudo dnf install flameshot      # Fedora
sudo pacman -S flameshot        # Arch

# Usage
flameshot gui                   # Capture + annotation interactive
flameshot full -p /tmp/         # Capture plein ecran

# Spectacle (KDE)
sudo apt install kde-spectacle

# GNOME Screenshot
gnome-screenshot -a             # Zone selectionnee

# Grim + Slurp (Wayland pur)
sudo apt install grim slurp
grim -g "$(slurp)" /tmp/screenshot.png
```

### App Launchers (equivalent Raycast/Spotlight)

```bash
# Albert (RECOMMANDE)
# Debian/Ubuntu : via PPA
sudo add-apt-repository ppa:nickoledeon/albert
sudo apt install albert

# Ulauncher
sudo add-apt-repository ppa:agornostal/ulauncher
sudo apt install ulauncher

# Rofi (leger, scriptable)
sudo apt install rofi   # Debian/Ubuntu
sudo pacman -S rofi     # Arch

# dmenu (minimaliste)
sudo apt install dmenu
```

### Package Managers

```bash
# Debian/Ubuntu
sudo apt update && sudo apt install <package>
sudo apt upgrade        # Mise a jour

# Fedora/RHEL
sudo dnf install <package>
sudo dnf upgrade        # Mise a jour

# Arch/Manjaro
sudo pacman -S <package>
sudo pacman -Syu        # Mise a jour

# Universel : Flatpak
flatpak install flathub <app>

# Universel : Snap
sudo snap install <app>

# Universel : AppImage
chmod +x App.AppImage && ./App.AppImage
```

### Multiplexer terminal

```bash
# tmux (RECOMMANDE)
sudo apt install tmux       # Debian/Ubuntu
sudo dnf install tmux       # Fedora
sudo pacman -S tmux         # Arch

# Commandes essentielles tmux
tmux new -s claude           # Nouvelle session
tmux attach -t claude        # Rattacher
Ctrl+B puis d                # Detacher
Ctrl+B puis %                # Split vertical
Ctrl+B puis "                # Split horizontal
Ctrl+B puis fleches          # Naviguer
Ctrl+B puis c                # Nouveau tab
Ctrl+B puis n/p              # Tab suivant/precedent
```

### Proxy/Debug reseau

```bash
# mitmproxy (debug API Claude)
sudo apt install mitmproxy   # Debian/Ubuntu
sudo dnf install mitmproxy   # Fedora
pip install mitmproxy        # Universel

# Usage pour debugger le contexte Claude
mitmproxy --mode regular --listen-port 8080
# Autre terminal :
HTTPS_PROXY=http://localhost:8080 claude
```

### Ouvrir un dossier/fichier

```bash
# Ouvrir dans le file manager par defaut
xdg-open .                    # Universel Linux
nautilus .                     # GNOME
dolphin .                      # KDE
thunar .                       # XFCE

# Ouvrir dans VS Code
code .
```

### Trash (poubelle)

```bash
# trash-cli via pip (RECOMMANDE — le plus complet)
pip install trash-cli
trash-put fichier.txt          # Mettre a la poubelle
trash-list                     # Lister la poubelle
trash-restore                  # Restaurer
trash-empty                    # Vider la poubelle

# gio trash (GNOME, pre-installe)
gio trash fichier.txt

# trash-cli via npm
npm install -g trash-cli
trash fichier.txt

# Emplacement de la poubelle
~/.local/share/Trash/files/
~/.local/share/Trash/info/
```

### Outils systeme utiles

```bash
# htop — moniteur systeme interactif
sudo apt install htop

# btop — moniteur systeme moderne
sudo apt install btop

# ncdu — analyseur espace disque en TUI
sudo apt install ncdu

# fd — alternative rapide a find
sudo apt install fd-find       # Debian/Ubuntu (commande: fdfind)
sudo pacman -S fd              # Arch (commande: fd)

# ripgrep — alternative rapide a grep
sudo apt install ripgrep       # Debian/Ubuntu (commande: rg)
sudo pacman -S ripgrep         # Arch

# bat — alternative a cat avec coloration
sudo apt install bat           # Debian/Ubuntu (commande: batcat)
sudo pacman -S bat             # Arch

# eza — alternative a ls
cargo install eza              # Via Rust
sudo pacman -S eza             # Arch

# jq — manipuler du JSON en CLI
sudo apt install jq

# fzf — fuzzy finder
sudo apt install fzf

# pv — progress bar pour pipes
sudo apt install pv
```

### Dossier "CC" — Fourre-tout Claude Code

```bash
mkdir ~/cc
echo 'alias cdcc="cd ~/cc"' >> ~/.bashrc
source ~/.bashrc
```

---

## 16. Status Line

### Principe

La status line affiche en temps reel dans le terminal : branche git, tokens utilises, cout, pourcentage du contexte.

### Installation (script Melvynx via Bun)

```bash
# Cloner le script
cd ~/.claude/hooks
git clone https://github.com/Melvynx/aiblueprint.git /tmp/aiblueprint 2>/dev/null
cp -r /tmp/aiblueprint/scripts/statusline ~/.claude/hooks/

# Installer les dependances
cd ~/.claude/hooks/statusline
bun install

# Lancer
bun run index.ts
```

### Installation via Blueprint CLI

```bash
# Prerequis : bun installe
bun --version

# Installe automatiquement par Blueprint CLI
npx ig-blueprint-cli
```

### Si ne fonctionne pas
- Verifier que bun est installe : `bun --version`
- `/debug` dans Claude Code pour auto-reparer

### Integration dans tmux

```bash
# Dans ~/.tmux.conf
set -g status-right '#(~/.claude/hooks/statusline/run.sh 2>/dev/null)'
set -g status-interval 5
```

### Integration dans le prompt bash/zsh

```bash
# Dans ~/.bashrc ou ~/.zshrc
claude_status() {
  local ctx=$(cat /tmp/claude-context-pct 2>/dev/null)
  [ -n "$ctx" ] && echo " [Claude: ${ctx}%]"
}
PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(claude_status)\$ '
```

---

## 17. Structure .claude

### Arborescence complete recommandee

```
~/.claude/
  CLAUDE.md                    # Memoire globale (max 500 lignes)
  settings.json                # Permissions, hooks, MCP
  settings.local.json          # Override local (non committe)

  rules/                       # Regles modulaires
    00-document-before-modify.md
    01-no-delete-without-backup.md
    02-memory-architecture.md
    09-linux-specifics.md      # Regles Linux
    11-auto-learning.md
    20-json.md                 # globs: ["*.json"]
    21-git.md                  # globs: [".git/**"]
    22-security.md
    23-test.md                 # globs: ["*.test.*", "*.spec.*"]

  memory/                      # Fichiers de reference detailles
    01_architecture.md
    02_lessons_log.md
    03_conventions.md

  skills/                      # Skills (prompts reutilisables)
    apex/
      SKILL.md
      steps/
    commit/
      SKILL.md
    oneshot/
      SKILL.md
    debug/
      SKILL.md
    disabled/                  # Deplacer ici pour desactiver

  agents/                      # Agents specialises
    websearch/
      AGENT.md
    explore-codebase/
      AGENT.md
    explore-docs/
      AGENT.md

  hooks/                       # Scripts de hooks
    command-validator/
      src/
        cli.ts
      package.json
    statusline/
      index.ts
      package.json

  scripts/                     # command-validator, statusline (alias)
  bible/                       # Reference Melvynx
    BIBLE_LINUX.md

  plugins/                     # Plugins Claude Code

  projects/                    # Memoire par projet (auto-generee)
    -home-user-projet1/
      MEMORY.md

  plans/                       # Plans generes
  tasks/                       # Taches executees
  teams/                       # Equipes (JSON)
  backups/                     # Backups automatiques
    20260326_143000/
```

### Regles de fichiers

- `CLAUDE.md` : max 500 lignes, global et stable
- `rules/` : toujours `alwaysApply: true` sauf si `globs:` specifie
- `memory/` : pour les documents trop longs pour rules/ (>50 lignes)
- `skills/` : un dossier par skill, `SKILL.md` comme point d'entree
- `agents/` : un dossier par agent, `AGENT.md` comme point d'entree

---

## 18. Plugins

### Plugins officiels Claude Code

Les plugins etendent les capacites de Claude Code. Ils sont installes dans `~/.claude/plugins/`.

```bash
# Lister les plugins disponibles
claude plugins list

# Installer un plugin
claude plugins install <nom-plugin>

# Desinstaller
claude plugins remove <nom-plugin>
```

### Plugins recommandes

| Plugin | Usage |
|--------|-------|
| `claude-test` | Integration tests automatique |
| `claude-lint` | Linting intelligent |
| `claude-git` | Operations git avancees |
| `claude-docker` | Gestion conteneurs |

### Creer un plugin custom

```bash
mkdir -p ~/.claude/plugins/mon-plugin
```

Fichier `~/.claude/plugins/mon-plugin/plugin.json` :

```json
{
  "name": "mon-plugin",
  "version": "1.0.0",
  "description": "Mon plugin custom",
  "hooks": {
    "PostToolUse": {
      "matcher": "Bash",
      "command": "echo 'Commande executee: $TOOL_INPUT'"
    }
  }
}
```

---

## 19. Deploiement

### Claude Code en CI/CD

```bash
# Mode headless pour pipelines
claude --headless --prompt "lint et fix tous les fichiers TypeScript"

# Avec variable d'environnement pour la cle API
export ANTHROPIC_API_KEY="sk-ant-..."
claude --headless --prompt "genere les tests manquants"
```

### GitHub Actions

```yaml
name: Claude Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      - name: Install Claude Code
        run: curl -fsSL https://claude.ai/install.sh | bash
      - name: Review PR
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude --headless --prompt "review le diff de cette PR et donne un feedback structure"
```

### GitLab CI

```yaml
claude-review:
  image: node:20
  script:
    - curl -fsSL https://claude.ai/install.sh | bash
    - export PATH="$HOME/.claude/bin:$PATH"
    - claude --headless --prompt "review les changements et verifie la qualite du code"
  only:
    - merge_requests
```

### Systemd Service (daemon Linux)

Pour lancer Claude Code comme service persistant :

```ini
# /etc/systemd/user/claude-agent.service
# (ou ~/.config/systemd/user/claude-agent.service)
[Unit]
Description=Claude Code Agent
After=network.target

[Service]
Type=simple
ExecStart=/home/user/.claude/bin/claude --headless --prompt "surveille les logs et alerte si erreur"
Restart=on-failure
RestartSec=30
Environment=ANTHROPIC_API_KEY=sk-ant-...
Environment=HOME=/home/user
Environment=SANDBOX=1
WorkingDirectory=/home/user/projet

[Install]
WantedBy=default.target
```

```bash
# Activer et demarrer
systemctl --user daemon-reload
systemctl --user enable claude-agent
systemctl --user start claude-agent

# Voir les logs
journalctl --user -u claude-agent -f
```

### Cron jobs

```bash
# Audit quotidien a 6h du matin
crontab -e
0 6 * * * /home/user/.claude/bin/claude --headless --prompt "lance un audit complet du projet" >> /tmp/claude-audit.log 2>&1
```

---

## 20. OpenClaw

### Principe

OpenClaw est un agent Claude Code distant (Telegram bot) qui tourne sur un VPS Linux.
Il permet de controler Claude Code depuis n'importe ou via Telegram (texte + audio).
Lance des agents en arriere-plan.

### Installation locale

```bash
# Installation
curl -fsSL https://maltbot.sh | bash

# ou manuellement
git clone https://github.com/example/openclaw.git
cd openclaw
npm install
```

### Deploiement VPS (solution principale)

Le VPS Linux est la plateforme ideale pour OpenClaw.

#### VPS recommande par Melvynx

**Hetzner CAX21/CAX31** (ARM, ~$7.59/mois). IPv4 obligatoire.

| Parametre | Valeur |
|-----------|--------|
| Fournisseur | Hetzner Cloud |
| Instance | CAX21 (ARM) |
| Prix | ~7.59$/mois |
| OS | Ubuntu 24.04 LTS |
| CPU | 4 vCPU ARM |
| RAM | 8 GB |
| Stockage | 80 GB NVMe |
| IPv4 | Incluse |

### Setup VPS

```bash
# 1. Creer l'instance Hetzner (via hcloud CLI ou interface web)
hcloud server create \
  --name openclaw \
  --type cax21 \
  --image ubuntu-24.04 \
  --ssh-key ma-cle

# 2. Se connecter
ssh root@<ip-vps>

# 3. Mise a jour systeme
apt update && apt upgrade -y

# 4. Installer les outils de base
apt install -y curl git tmux htop

# 5. Installer Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs

# 6. Installer Claude Code
curl -fsSL https://claude.ai/install.sh | bash

# 7. Installer Docker (recommande pour isolation)
curl -fsSL https://get.docker.com | bash
systemctl enable docker
systemctl start docker

# 8. Installer Bun
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
```

### Installation via Blueprint CLI (VPS)

```bash
# Sur le VPS avec Docker
npx openclaw-vps-setup
```

### Bypass sandbox sur Linux

```bash
# Methode 1 : variable d'environnement
SANDBOX=1 claude

# Methode 2 : dans .bashrc (permanent)
echo 'export SANDBOX=1' >> ~/.bashrc
source ~/.bashrc

# Methode 3 : dans le service systemd
Environment=SANDBOX=1
```

> **ATTENTION** : Le bypass sandbox desactive les protections de securite.
> Ne l'utiliser que sur un VPS dedie, jamais sur une machine de travail avec des donnees sensibles.
> Toujours combiner avec UFW + Fail2Ban + SSH hardening.

### Securisation du VPS

```bash
# --- UFW (Uncomplicated Firewall) ---
apt install ufw
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh                  # Port 22
ufw allow 443/tcp              # HTTPS
ufw enable
ufw status verbose

# --- Fail2Ban ---
apt install fail2ban
systemctl enable fail2ban
systemctl start fail2ban

# Config Fail2Ban pour SSH
cat > /etc/fail2ban/jail.local << 'EOF'
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600
EOF
systemctl restart fail2ban

# --- SSH Hardening ---
# Editer /etc/ssh/sshd_config
sed -i 's/#PermitRootLogin yes/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config

# Changer le port SSH (optionnel, recommande)
sed -i 's/#Port 22/Port 47281/' /etc/ssh/sshd_config
ufw allow 47281/tcp

# Whitelister vos IPs AVANT d'activer le firewall
ufw allow from <VOTRE_IP> to any port 22

# Verifier et redemarrer
sshd -t && systemctl restart sshd

# --- Docker isolation ---
# Creer un utilisateur non-root pour les conteneurs
adduser --disabled-password openclaw
usermod -aG docker openclaw
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

### Precautions de securite
- Ne PAS activer 1Password sur le VPS (acces a toutes les cles)
- Telegram est plus securise que l'interface web d'OpenClaw
- Le gateway OpenClaw ouvre le port localhost 18789 — verifier qu'il n'est pas expose

### Configuration Telegram Bot

```bash
# 1. Creer un bot via @BotFather sur Telegram
# Obtenir le token : 1234567890:ABCdefGHIjklMNOpqrsTUVwxyz

# 2. Configurer OpenClaw
cat > ~/.openclaw/config.json << 'EOF'
{
  "telegram_token": "VOTRE_TOKEN_BOT",
  "allowed_users": [123456789],
  "claude_model": "claude-sonnet-4-20250514",
  "max_tokens": 4096
}
EOF

# 3. Lancer
openclaw start

# 4. Ou via Docker
docker run -d \
  --name openclaw \
  -e TELEGRAM_TOKEN=VOTRE_TOKEN \
  -e ANTHROPIC_API_KEY=sk-ant-... \
  -v ~/.openclaw:/app/config \
  openclaw/openclaw:latest
```

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

### Workflow OpenClaw

1. Message/audio Telegram avec description de la feature
2. Clone worktree -> npm install -> GitHub Issue -> lance apex --pr
3. Notification Telegram avec lien PR
4. "Merge la PR et delete le worktree"

### Modeles recommandes pour OpenClaw

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2/$12 |
| Cron jobs | Gemini 3 Flash | Tres faible |
| Agents code | Claude Opus | Abonnement |

### Skills OpenClaw (remplacent les MCP)

Un skill = SKILL.md + execute.sh. Plus leger que MCP.
Skills disponibles : Cloudflare, CodeLine, Dub, FlightData, Front, Google Image, LinkedIn Post, Lumail, Mercury, Porkbun, SaveIt, TypeFully, Vercel, YouTube Comments

### Fichiers de configuration OpenClaw

- `soul.md` : identite/personnalite du bot
- `memory/` : memoire quotidienne
- `canvas/index.html` : interface web locale
- `.env` : variables d'environnement

---

## 21. Anti-Patterns

### 18 regles a NE JAMAIS violer

| # | Anti-Pattern | Pourquoi |
|---|-------------|----------|
| 1 | Lancer Claude a la racine `/` | Contexte pollue, risque systeme |
| 2 | Surcharger CLAUDE.md (>500 lignes) | Perte de qualite, "Lost in the Middle" |
| 3 | Installer >3 MCP | Consomme trop de contexte, degrade la qualite |
| 4 | Bypass SANS deny-list | Toute commande est executee sans controle |
| 5 | Features complexes sans workflow | L'IA saute des etapes, code incomplet |
| 6 | Dependre de plugins non controles | Code tiers non audite |
| 7 | Taguer des fichiers avec Apex/OneShot | Empeche l'exploration intelligente |
| 8 | Utiliser `/batch` | Preferer main agent + sub-agents |
| 9 | Oublier `/clear` entre taches | Contexte pollue, instructions ignorees |
| 10 | Empiler les demandes pendant que l'IA travaille | Confusion, resultats incoherents |
| 11 | Relancer un skill pour une petite modif | Gaspillage de tokens, langage naturel suffit |
| 12 | Utiliser Chrome Headless | Lourd, lent, consomme des ressources |
| 13 | `rm -rf` au lieu de `trash-put` | Perte de donnees irreversible |
| 14 | Ignorer les erreurs de hook | Faux sentiment de securite |
| 15 | Ne pas lire CLAUDE.md d'un projet avant d'agir | Conventions ignorees, code incoherent |
| 16 | Coder sans explorer d'abord | Reinvention, duplication, bugs |
| 17 | Ignorer `/context` | Autocompact surprise, perte de qualite |
| 18 | Modifier `/etc/` sans backup | Systeme potentiellement inutilisable |

### Anti-patterns specifiques Linux

| Anti-Pattern | Alternative |
|-------------|-------------|
| `sudo rm -rf /var/log/*` | `sudo journalctl --vacuum-time=7d` |
| `chmod -R 777 .` | `chmod -R u+rwX,go+rX .` |
| `curl ... \| sudo bash` | Telecharger, inspecter, puis executer |
| `pip install --user` sans venv | `python3 -m venv .venv && source .venv/bin/activate` |
| Editer `/etc/fstab` sans backup | `sudo cp /etc/fstab /etc/fstab.bak.$(date +%s)` |
| `kill -9` en premier | `kill` (SIGTERM) d'abord, `kill -9` si necessaire |
| Desactiver SELinux/AppArmor | Creer une politique specifique |

---

## 22. Niveaux de Maitrise

### Niveau 1 : Debutant (Semaine 1)

- [ ] Installer Claude Code
- [ ] Lancer une premiere conversation
- [ ] Utiliser `/clear` entre les taches
- [ ] Creer un premier `CLAUDE.md` avec `/init`
- [ ] Configurer `settings.json` avec une deny-list basique
- [ ] Installer `trash-cli` et ne plus utiliser `rm`
- [ ] Comprendre les 3 modes de permissions (ask/auto/bypass)

### Niveau 2 : Intermediaire (Semaine 2-3)

- [ ] Configurer les hooks (son Stop, Notification via notify-send)
- [ ] Installer le command-validator (hook PreToolUse)
- [ ] Utiliser les workflows `/apex` et `/oneshot`
- [ ] Creer des rules conditionnelles (globs)
- [ ] Installer et configurer 2 MCP (Context7, Memory)
- [ ] Utiliser `/context` regulierement
- [ ] Maitriser tmux pour les sessions Claude Code

### Niveau 3 : Avance (Semaine 4+)

- [ ] Creer des skills custom avec Prompt Discovery (steps/)
- [ ] Deployer des sub-agents (websearch, explore-codebase, explore-docs)
- [ ] Utiliser les Teams (multi-agents + git worktrees)
- [ ] Configurer la status line
- [ ] Deployer OpenClaw sur un VPS
- [ ] Integrer Claude Code en CI/CD (GitHub Actions, GitLab CI)
- [ ] Utiliser mitmproxy pour debugger le contexte
- [ ] Creer des plugins custom
- [ ] Configurer un systemd service pour des taches automatiques

### Niveau 4 : Expert

- [ ] Architecture memoire complete (3 niveaux Melvynx)
- [ ] Knowledge graph avec decisions techniques et erreurs
- [ ] Skills avec auto-apprentissage (detection corrections/preferences)
- [ ] Pipeline complete : dev -> test -> review -> deploy via Claude Code
- [ ] Monitoring continu via `/loop` et agents specialises
- [ ] Contribution upstream aux skills/agents de la communaute

---

## 23. Nouveautes et Roadmap

### Fonctionnalites recentes (2026)

| Feature | Description | Status |
|---------|-------------|--------|
| Plugins natifs | Extensions tierces installables | Disponible |
| Agent Teams | Multi-agents paralleles (git worktrees) | Disponible |
| Tool Search | Chargement conditionnel des outils MCP | Disponible |
| Sub-agents | Agents isoles avec contexte separe | Disponible |
| Prompt Discovery | Steps/ pour eviter Lost in the Middle | Disponible |
| autoSearchThreshold | Seuil auto pour Tool Search | Disponible |
| Mode Headless | CI/CD et automation | Disponible |
| Voice Mode | Interaction vocale | Disponible |
| Sessions nommees | `--session-name` pour identification | Disponible |
| WebSearch/WebFetch | Recherche et fetch web natifs | Disponible |

### Specificites Linux

| Feature | Support Linux | Notes |
|---------|--------------|-------|
| Installation CLI | Complet | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Hooks | Complet | Bash natif, bun pour command-validator |
| Notifications | `notify-send` | libnotify requis |
| Son | `paplay` / `aplay` | PulseAudio/PipeWire/ALSA |
| Proxy debug | `mitmproxy` | Disponible via apt/pip |
| Screenshots | Flameshot | `sudo apt install flameshot` |
| Systemd | Complet | Services user et system |
| Docker | Complet | Isolation recommandee pour VPS |
| Wayland | Partiel | Certains outils X11 uniquement |

### Roadmap anticipee

- **Claude Code VS Code Extension** — integration plus profonde avec l'editeur
- **Collaboration temps reel** — plusieurs utilisateurs sur le meme contexte
- **Memoire longue native** — au-dela de 200K tokens
- **Agents autonomes** — execution continue sans supervision
- **MCP standardise** — protocole unifie entre tous les outils IA

---

## Annexes

### A. Checklist post-installation Linux

```bash
# 1. Verifier Node.js
node --version  # >= 18

# 2. Verifier Claude Code
claude --version

# 3. Installer les outils essentiels
sudo apt install -y git tmux jq ripgrep fd-find bat pv htop  # Debian/Ubuntu
sudo dnf install -y git tmux jq ripgrep fd-find bat pv htop  # Fedora
sudo pacman -S git tmux jq ripgrep fd bat pv htop             # Arch

# 4. Installer trash-cli
pip install trash-cli

# 5. Installer bun (pour hooks)
curl -fsSL https://bun.sh/install | bash

# 6. Configurer l'alias anti-rm
echo 'alias rm="echo INTERDIT: utiliser trash-put; false"' >> ~/.bashrc

# 7. Creer la structure .claude
mkdir -p ~/.claude/{rules,memory,skills,agents,hooks,bible,backups,plugins}

# 8. Copier settings.json (voir section 4)
# 9. Configurer CLAUDE.md global (voir section 3)
# 10. Installer command-validator (voir section 5)

# 11. Verifier les sons
paplay /usr/share/sounds/freedesktop/stereo/complete.oga 2>/dev/null && echo "Son OK" || echo "Installer sound-theme-freedesktop"

# 12. Verifier notify-send
notify-send "Test" "Claude Code installe" 2>/dev/null && echo "Notifications OK" || echo "Installer libnotify-bin"

# 13. Premier lancement
claude
```

### B. Equivalences macOS -> Linux

| macOS | Linux | Notes |
|-------|-------|-------|
| `brew install` | `apt install` / `dnf install` / `pacman -S` | Selon la distro |
| `afplay son.aiff` | `paplay son.oga` / `aplay son.wav` | PulseAudio/ALSA |
| `osascript -e 'display notification'` | `notify-send` | libnotify |
| `open .` | `xdg-open .` / `nautilus .` | Selon le DE |
| `pbcopy` / `pbpaste` | `xclip -sel clip` / `xclip -sel clip -o` | ou `wl-copy`/`wl-paste` (Wayland) |
| `launchctl` (LaunchAgent) | `systemctl --user` (systemd) | Services utilisateur |
| `diskutil` | `lsblk` / `fdisk -l` / `df -h` | Gestion disques |
| `trash` (brew) | `trash-put` (pip) / `gio trash` | Poubelle |
| `say "texte"` | `espeak "texte"` / `festival --tts` | Synthese vocale |
| CleanShotX | Flameshot | Screenshots annotes |
| Raycast | Albert / Ulauncher / Rofi | App launcher |
| Activity Monitor | htop / btop | Moniteur systeme |
| Spotlight | Albert / Ulauncher | Recherche |
| Finder | Nautilus / Dolphin / Thunar | File manager |
| Cmd | Ctrl (dans le terminal) | Raccourcis |
| Terminal.app | Ghostty / Kitty / Alacritty | Emulateur terminal |

### C. Distribution recommandee pour Claude Code

| Distro | Niveau | Raison |
|--------|--------|--------|
| **Ubuntu 24.04 LTS** | Debutant | Tout marche out-of-the-box, enorme communaute |
| **Fedora 41** | Intermediaire | Packages recents, SELinux, Wayland natif |
| **Arch Linux** | Avance | Rolling release, AUR, tout configurable |
| **Debian 12** | Serveur/VPS | Stable, leger, ideal pour deploiement |

### D. Troubleshooting Linux

| Probleme | Solution |
|----------|----------|
| `claude: command not found` | `source ~/.bashrc` ou ajouter `~/.claude/bin` au PATH |
| Pas de son avec `paplay` | `sudo apt install pulseaudio-utils sound-theme-freedesktop` |
| `notify-send` ne marche pas | Verifier que le serveur de notifications est lance (`dunst`, `mako`, etc.) |
| Bun ne s'installe pas | Verifier `unzip` est installe : `sudo apt install unzip` |
| Hook command-validator echoue | `cd ~/.claude/hooks/command-validator && bun install` |
| Permission denied sur trash | Verifier les permissions de `~/.local/share/Trash/` |
| Claude lent | Verifier la bande passante (`speedtest-cli`) et le proxy eventuellement |
| Wayland : Flameshot ne marche pas | Utiliser `grim + slurp` ou lancer avec `QT_QPA_PLATFORM=xcb flameshot gui` |
| tmux ne copie pas dans le clipboard | `sudo apt install xclip` et configurer tmux : `bind -T copy-mode-vi y send-keys -X copy-pipe "xclip -sel clip"` |
| `fdfind` au lieu de `fd` | Sur Debian/Ubuntu : `ln -s $(which fdfind) ~/.local/bin/fd` |
| `batcat` au lieu de `bat` | Sur Debian/Ubuntu : `ln -s $(which batcat) ~/.local/bin/bat` |
| VPS : Claude Code tue par OOM Killer | Augmenter le swap (voir ci-dessous) |
| tmux : session perdue apres reboot | Installer tmux-resurrect (voir section 13) |
| Permissions refusees sur .claude/ | `chmod -R u+rw ~/.claude/` |

#### VPS : augmenter le swap en cas d'OOM
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

---

## Credits

Methode basee sur la Masterclass de [Melvynx (CodeLynx)](https://codelynx.dev).
Voir README.md pour la methode complete (workflows, prompting, context engineering, anti-patterns).

> **Document genere le 2026-03-26 — Methode Melvynx adaptee pour Linux**
> **Auteur : Soly Kim (kim13) avec Claude Code**
> **Sources : Bible Melvynx macOS, repo AIBlueprint, Masterclass 4h + Setup 20min + Formation Complete, documentation officielle Claude Code**
