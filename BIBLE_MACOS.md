# Bible Claude Code — macOS

> Guide complet pour configurer et utiliser Claude Code sur macOS (Intel & Apple Silicon)
> Basee sur la methode Melvynx (CodeLynx) — Masterclass 4h + Setup 20min + Formation Complete
> Voir README.md pour la methode complete (workflows, prompting, context engineering)

---

## Table des matieres

1. [Pre-requis](#pre-requis)
2. [Installation](#installation)
3. [Terminaux recommandes](#terminaux-recommandes)
4. [Outils macOS recommandes](#outils-macos-recommandes)
5. [Configuration settings.json (macOS)](#configuration-settingsjson-macos)
6. [Hooks macOS](#hooks-macos)
7. [Sons et notifications](#sons-et-notifications)
8. [Deny-list macOS](#deny-list-macos)
9. [MCP recommandes](#mcp-recommandes)
10. [Status Line](#status-line)
11. [tmux sur macOS](#tmux-sur-macos)
12. [Structure du dossier .claude/](#structure-du-dossier-claude)
13. [Astuces macOS](#astuces-macos)
14. [OpenClaw sur Mac Mini](#openclaw-sur-mac-mini)
15. [Troubleshooting macOS](#troubleshooting-macos)

---

## Pre-requis

### Node.js
```bash
# Via Homebrew (recommande)
brew install node

# Ou depuis nodejs.org
```

### Homebrew (gestionnaire de paquets macOS)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Bun (necessaire pour StatusLine et command-validator)
```bash
brew install oven-sh/bun/bun
# ou
curl -fsSL https://bun.sh/install | bash
```

### VS Code
1. Telecharger sur code.visualstudio.com
2. `Cmd+Shift+P` > "Shell Command: Install code in PATH"
3. Terminal integre : menu Terminal > New Terminal

---

## Installation

### Claude Code
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Apres installation, relancer le terminal si `claude` ne fonctionne pas.
Verifier : `claude` puis taper "qui suis-je ?"

### Config Melvynx OG (gratuite)
```bash
npx ig-blueprint-cli
```

Installe automatiquement :
- Backup config existante dans `backups/`
- settings.json (bypass + deny list)
- Hooks (son fin de tache Glass.aiff, command validator, Notification)
- Skills (/apex, /oneshot, /commit, /fix-errors, /fix-grammar, etc.)
- Status Line (necessite bun)

### Regle fondamentale
**JAMAIS lancer Claude Code a la racine du systeme** (`/`). Toujours `cd` dans un dossier projet d'abord.

---

## Terminaux recommandes

| Terminal | Description | Recommandation |
|----------|-------------|----------------|
| **CEMUX** | Terminal cree pour les coding agents. Build au-dessus de Ghostty. Tabs verticaux + horizontaux, notifications agent, browser integre, split pane, scriptable, Swift/AppKit (pas Electron), gratuit | **★★★ Meilleur choix** |
| **Ghostty** | Terminal rapide. `Cmd+1/2/3` pour tabs, `Cmd+W` fermer, `Cmd+T` nouveau tab | ★★ Excellent |
| **iTerm2** | Terminal classique macOS, tres configurable | ★★ Classique |
| **Terminal.app** | App par defaut macOS | ★ Basique |
| **Hyper** (hyterm) | Alternative populaire basee sur Electron | ★ Alternative |

---

## Outils macOS recommandes

| Outil | Description | Prix |
|-------|-------------|------|
| **Raycast** | Remplacant de Spotlight : app launcher, clipboard history, emoji picker, window management, screenshot. Extensions : VS Code, Figma, GitHub, Tailwind, iOS. Remplace `Cmd+Espace` | Gratuit (sauf IA) |
| **CleanShotX** | Screenshots annotes avec dessins, fleches, carres rouges. Essentiel pour le debugging visuel et le prompting Brownfield | Payant (inclus SetApp) |
| **WhisperTurbo** | Speech-to-text local gratuit et open source (app Melvynx). Modele PRK (Parakeet) = rapide/court, Whisper Large = long/francais | Gratuit |
| **Proxyman** | Intercepte les requetes HTTP de Claude Code. Voir exactement ce qui est envoye au modele (system prompt, tools, MCP, rules) | Freemium |
| **SetApp** | Bundle d'outils macOS incluant CleanShotX | Abonnement |

---

## Configuration settings.json (macOS)

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
      "Bash(diskutil eraseDisk*)",
      "Bash(curl *| bash*)",
      "Bash(curl *| sh*)",
      "Bash(wget *| bash*)",
      "Bash(wget *| sh*)"
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
            "command": "afplay /System/Library/Sounds/Glass.aiff",
            "async": true
          }
        ]
      }
    ],
    "Notification": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Funk.aiff",
            "async": true
          }
        ]
      }
    ]
  }
}
```

### settings.local.json (local, non partage)
Fichier : `~/.claude/settings.local.json`
Pour hooks personnels, configs locales, preferences machine.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $FILEPATH",
            "async": true
          }
        ]
      }
    ]
  }
}
```

---

## Hooks macOS

### 4 hooks recommandes

| Hook | Declencheur | Commande macOS | Usage |
|------|------------|----------------|-------|
| **PreToolUse** | Avant execution outil | `bun command-validator/src/cli.ts` | Valide les commandes (AST-level, zero faux positifs) |
| **Stop** | Fin de tache | `afplay /System/Library/Sounds/Glass.aiff` | Son "tache terminee" |
| **Notification** | Claude a besoin d'attention | `afplay /System/Library/Sounds/Funk.aiff` | Son "question en attente" (different du Stop) |
| **PostToolUse** (local) | Apres Edit/Write | `npx prettier --write $FILEPATH` | Formatter automatiquement |

### Bonnes pratiques hooks
- Toujours `async: true` pour les sons (non-bloquant)
- UN SEUL hook PreToolUse (command-validator)
- Ne pas surcharger de hooks (ralentit l'execution)
- Ne JAMAIS creer les hooks manuellement — demander a Claude

---

## Sons et notifications

### Sons systeme macOS disponibles

```bash
ls /System/Library/Sounds/
# Basso.aiff  Blow.aiff  Bottle.aiff  Frog.aiff  Funk.aiff
# Glass.aiff  Hero.aiff  Morse.aiff  Ping.aiff  Pop.aiff
# Purr.aiff  Sosumi.aiff  Submarine.aiff  Tink.aiff
```

### Configuration recommandee
- **Stop** (tache terminee) : `Glass.aiff` — son doux
- **Notification** (question) : `Funk.aiff` — son different, plus insistant

### Test
```bash
afplay /System/Library/Sounds/Glass.aiff
afplay /System/Library/Sounds/Funk.aiff
```

**Note** : les sons ne fonctionnent PAS depuis un processus sandboxe (ex: terminal Claude Code). Ils fonctionnent via hooks et LaunchAgents.

---

## Deny-list macOS

Commandes dangereuses a bloquer specifiquement sur macOS :

```json
"permissions": {
  "deny": [
    "Bash(rm -rf *)",
    "Bash(sudo rm *)",
    "Bash(sudo rm -rf *)",
    "Bash(chmod 777 *)",
    "Bash(diskutil eraseDisk*)",
    "Bash(diskutil partitionDisk*)",
    "Bash(mkfs*)",
    "Bash(dd if=*)",
    "Bash(launchctl unload*)",
    "Bash(git push --force*)",
    "Bash(git reset --hard*)",
    "Bash(curl *| bash*)",
    "Bash(curl *| sh*)",
    "Bash(wget *| bash*)"
  ]
}
```

### Double securite (CLAUDE.md)
Ajouter dans votre CLAUDE.md :
```
- JAMAIS utiliser rm — toujours trash
- JAMAIS chmod 777
- JAMAIS sudo rm
- JAMAIS curl | bash
```

### Installer trash-cli
```bash
brew install trash-cli
# Utiliser : trash fichier.txt (au lieu de rm)
```

---

## MCP recommandes

```bash
# Context7 — Documentation technique a jour (gratuit)
claude mcp add --scope user context7

# Exa.ai — Recherche web optimisee IA (20$ credit gratuit)
claude mcp add --scope user exa
```

JAMAIS plus de 3 MCP simultanement sur macOS (degrade la qualite).

---

## Status Line

Barre d'information en bas du terminal.

### Informations affichees
- Dossier courant et repo git
- Modele utilise (Opus, Sonnet, etc.)
- Cout de la session en $
- Tokens utilises et % contexte
- Graphique session et weekly limit

### Installation
```bash
# Prerequis
brew install oven-sh/bun/bun

# Installe automatiquement par Blueprint CLI
npx ig-blueprint-cli
```

### Si ne fonctionne pas
- Verifier que bun est installe : `bun --version`
- `/debug` dans Claude Code pour auto-reparer

---

## tmux sur macOS

### Installation
```bash
brew install tmux
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

### Cas d'usage
Un tab par role : CC1 (feature), CC2 (review), Server, Ingest, etc.

---

## Structure du dossier .claude/

```
~/.claude/
├── CLAUDE.md             # Memoire globale
├── settings.json         # Config partagee (bypass + deny)
├── settings.local.json   # Config locale (hooks perso, Prettier)
├── rules/                # Regles persistantes (.md)
│   └── *.md              # Chargees auto a chaque session
├── skills/               # Skills (SKILL.md entry point)
│   └── disabled/         # Deplacer ici pour desactiver
├── agents/               # Agents custom
├── scripts/              # command-validator, statusline
├── hooks/                # Scripts de hooks personnels
├── projects/             # Historique sessions (transcripts)
├── plans/                # Plans generes
├── tasks/                # Taches executees
└── teams/                # Equipes (JSON)
```

**Astuce** : lancer `claude` dans `~/.claude/` pour gerer/modifier sa propre config.

---

## Astuces macOS

### Copier un chemin dans le Finder
Clic droit sur un dossier > maintenir **Option** > "Copier comme chemin d'acces"

### Ouvrir le Finder depuis le terminal
```bash
open .          # Ouvrir le dossier courant
open ~/Desktop  # Ouvrir un dossier specifique
```

### Ouvrir VS Code depuis le terminal
```bash
code .          # Ouvrir le projet courant
code fichier.ts # Ouvrir un fichier specifique
```

### Dossier "CC" — Fourre-tout Claude Code
```bash
mkdir ~/cc
alias cdcc="cd ~/cc"  # Ajouter dans ~/.zshrc
```
Usage : titres YouTube, slides, analyses, emails, comptabilite, todo lists.
Philosophie : "Si Claude Code peut le faire, pas besoin de SaaS."

### LaunchAgents (demarrage automatique)
Les scripts qui doivent tourner au demarrage du Mac se placent dans :
`~/Library/LaunchAgents/` (format plist XML)

### Homebrew — Paquets utiles
```bash
brew install trash-cli  # Remplacant de rm
brew install tmux       # Multiplexeur terminal
brew install bun        # Runtime JS rapide (StatusLine)
brew install pv         # Progress bar pour pipes
```

---

## OpenClaw sur Mac Mini

### Concept
Un **Mac Mini dedie** (~600 EUR) pour faire tourner OpenClaw 24/7, sans mobiliser le MacBook principal.

### Avantages
- Toujours allume, jamais en veille
- Acces natif aux outils macOS (afplay, osascript, etc.)
- Pas besoin de VPS (economie sur le long terme)

### Alternative VPS
Si pas de Mac Mini : Hetzner CAX21/CAX31 (ARM, ~$7.59/mois) avec Docker.

---

## Troubleshooting macOS

### Claude Code ne trouve pas la commande `claude`
```bash
# Relancer le terminal ou
source ~/.zshrc
```

### Sons ne fonctionnent pas
- Les sons ne marchent PAS depuis un processus sandboxe
- Verifier que les hooks sont en `async: true`
- Tester directement : `afplay /System/Library/Sounds/Glass.aiff`

### StatusLine ne s'affiche pas
```bash
bun --version  # Verifier que bun est installe
# Si absent :
brew install oven-sh/bun/bun
# Puis /debug dans Claude Code
```

### Permissions refusees
```bash
# Verifier les permissions du dossier .claude
ls -la ~/.claude/
# Si probleme :
chmod -R u+rw ~/.claude/
```

### Context trop charge au demarrage
- Verifier le nombre de MCP : `/mcp` (max 3)
- Verifier la taille de CLAUDE.md : `wc -l ~/.claude/CLAUDE.md` (max ~500 lignes)
- Deplacer le surplus dans `~/.claude/rules/`

---

## Credits

Methode basee sur la Masterclass de [Melvynx (CodeLynx)](https://codelynx.dev).
Voir README.md pour la methode complete (workflows, prompting, context engineering, anti-patterns).
