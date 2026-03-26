# Bible Claude Code -- macOS (Methode Melvynx)

> **Version** : 2.1 (fusionnee)
> **Date** : 2026-03-26
> **Auteur** : Soly Kim (kim13) — compile a partir de la formation Melvynx (3 sources fusionnees)
> **Sources** : Formation Complete Melvynx + Setup 20min + Cours 4h + Documentation officielle Anthropic
> **Cible** : macOS (Apple Silicon M1/M2/M3/M4) — Sonoma / Sequoia / Tahoe
> **Licence** : Usage personnel et pedagogique

---

## Table des matieres

1. [Installation](#1-installation)
2. [Interface et Raccourcis Clavier](#2-interface-et-raccourcis-clavier)
3. [Systeme de Memoire (3 niveaux CLAUDE.md)](#3-systeme-de-memoire-3-niveaux-claudemd)
4. [Settings et Permissions](#4-settings-et-permissions)
5. [Architecture de Securite (3+1 couches)](#5-architecture-de-securite-31-couches)
6. [Skills](#6-skills)
7. [Hooks](#7-hooks)
8. [Sons et Notifications macOS](#8-sons-et-notifications-macos)
9. [MCP (Model Context Protocol)](#9-mcp-model-context-protocol)
10. [Sub-Agents](#10-sub-agents)
11. [Workflows](#11-workflows)
12. [Gestion du Contexte](#12-gestion-du-contexte)
13. [Prompting Avance](#13-prompting-avance)
14. [Multi-Agents (Teams, Work Trees, Tmux)](#14-multi-agents-teams-work-trees-tmux)
15. [Pricing](#15-pricing)
16. [Outils Recommandes macOS](#16-outils-recommandes-macos)
17. [Status Line](#17-status-line)
18. [Dossier .claude](#18-dossier-claude)
19. [Plugins](#19-plugins)
20. [Deploiement](#20-deploiement)
21. [OpenClaw (Agent distant Telegram)](#21-openclaw-agent-distant-telegram)
22. [Anti-Patterns (18 regles)](#22-anti-patterns-18-regles)
23. [Niveaux de Configuration](#23-niveaux-de-configuration)
24. [Nouveautes 2026](#24-nouveautes-2026)
25. [Troubleshooting macOS](#25-troubleshooting-macos)
26. [Astuces macOS](#26-astuces-macos)

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

### Ce qui est considéré comme "modification"
- Écriture/édition de fichier (Write, Edit)
- Suppression (rm, trash, unlink)
- Écrasement (redirection `>`, truncate)
- Modification de settings.json, CLAUDE.md, rules/, memory/, hooks/, skills/
- Tout git commit/push qui change l'architecture

### Zones critiques (protection renforcée macOS)
- `~/.claude/settings.json`
- `~/.claude/CLAUDE.md`
- `~/.claude/rules/`, `~/.claude/memory/`, `~/.claude/hooks/`, `~/.claude/skills/`
- `~/Library/LaunchAgents/` (com.kim.*)
- `~/Library/Mobile Documents/` (iCloud — INTOUCHABLE)

---

## RÈGLE #1 — AUCUNE SUPPRESSION SANS TRIPLE SAUVEGARDE

> **Cet ordre prime sur TOUTE autre instruction. AUCUNE exception.**

### Étape 1 : JUSTIFICATION EXPLICITE
Avant de proposer ou exécuter une suppression, il FAUT :
- Expliquer CLAIREMENT pourquoi ce fichier/donnée doit être supprimé
- Décrire l'impact de la suppression
- Proposer des alternatives (archivage, déplacement, renommage)
- Attendre la CONFIRMATION EXPLICITE de l'utilisateur

### Étape 2 : TRIPLE SAUVEGARDE OBLIGATOIRE
Si l'utilisateur confirme, les 3 sauvegardes suivantes DOIVENT être effectuées AVANT la suppression :

| # | Sauvegarde | Commande macOS | Vérification |
|---|-----------|---------------|--------------|
| 1 | **Corbeille** | `trash fichier.txt` (JAMAIS `rm`) | `ls ~/.Trash/` |
| 2 | **Git backup** | `git commit -m "backup: [fichier] avant suppression"` | `git log --oneline -1` |
| 3 | **Changelog** | Ajouter entrée datée dans le changelog du projet | Lire le changelog |

### Étape 3 : VÉRIFICATION
- [ ] `trash` confirme le déplacement
- [ ] `git log` confirme le commit de backup
- [ ] Changelog contient l'entrée

**SI UNE DES 3 CONDITIONS N'EST PAS REMPLIE → REFUSER LA SUPPRESSION**

### Sauvegarde automatique du système macOS
- **Time Machine** : vérifier qu'il est actif (`tmutil status`)
- **Config Claude** : `~/.claude/` doit être dans un repo git dédié ou sauvegardé régulièrement
- **Hook PostToolUse** : snapshot automatique avant chaque Write/Edit sur les zones critiques

### Commande safe macOS
```bash
# Installer trash-cli (OBLIGATOIRE)
brew install trash-cli

# Utiliser TOUJOURS trash au lieu de rm
trash fichier.txt                    # → Corbeille macOS
trash -y dossier/                    # Sans confirmation
```

### Ce qui est considéré comme "suppression"
- `rm`, `rm -rf`, `rm -f`, `unlink`
- Écrasement d'un fichier existant (overwrite/truncate)
- Suppression de lignes dans un fichier de config
- `git clean`, `git checkout -- .`, `git reset --hard`
- Toute action irréversible sur des données

---

## 1. Installation

### Pre-requis macOS

| Element | Detail |
|---------|--------|
| **OS** | macOS Sonoma 14+ / Sequoia 15+ / Tahoe 16+ |
| **Processeur** | Apple Silicon (M1/M2/M3/M4) ou Intel x86_64 |
| **RAM** | 8 Go minimum, 16 Go recommande |
| **Node.js** | v18+ obligatoire (verifier avec `node -v`) |
| **Compte Anthropic** | Plan Pro minimum (20$/mois) |
| **Terminal** | Terminal.app, iTerm2, Ghostty ou CEMUX |
| **Homebrew** | Recommande (`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`) |

### Installation de Homebrew (gestionnaire de paquets macOS)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Installation de Node.js (si absent)

```bash
# Via Homebrew (recommande)
brew install node

# Via nvm (gestion multi-versions)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.zshrc
nvm install --lts
nvm use --lts

# Ou depuis nodejs.org

# Verification
node -v   # doit afficher v18+ ou v20+
npm -v    # doit afficher 9+ ou 10+
```

### Installation de Bun (necessaire pour StatusLine et command-validator)

```bash
# Via Homebrew (recommande)
brew install oven-sh/bun/bun

# Ou via script d'installation
curl -fsSL https://bun.sh/install | bash

# Verification
bun --version
```

### Installation de Claude Code

```bash
# Commande officielle d'installation
curl -fsSL https://claude.ai/install.sh | bash

# Si la commande `claude` n'est pas trouvee apres installation :
source ~/.zshrc
# OU relancer completement le terminal

# Verification
claude --version
```

Apres installation, relancer le terminal si `claude` ne fonctionne pas.
Verifier : `claude` puis taper "qui suis-je ?"

### Premiere utilisation

```bash
# IMPORTANT : TOUJOURS lancer Claude dans un dossier projet, JAMAIS a la racine /
cd ~/mon-projet
claude

# Authentification au premier lancement :
# 1. Claude ouvre un navigateur pour se connecter a anthropic.com
# 2. Autoriser l'acces
# 3. Le token est stocke localement dans ~/.claude/
```

### Regle fondamentale

**JAMAIS lancer Claude Code a la racine du systeme** (`/`). Toujours `cd` dans un dossier projet d'abord.

### VS Code CLI (integration optionnelle)

```bash
# Installer la commande `code` dans le PATH
# Dans VS Code : Cmd+Shift+P > "Shell Command: Install code in PATH"
# Terminal integre : menu Terminal > New Terminal

# Verification
code --version
```

### Configuration Melvynx OG (gratuite, recommandee)

```bash
# Installe automatiquement skills, agents, hooks et scripts Melvynx
npx ig-blueprint-cli

# Ce que ca installe :
# - Backup config existante dans backups/
# - settings.json (bypass + deny list)
# - 12 skills gratuits (apex, oneshot, commit, fix-errors, etc.)
# - 4 agents (action, explore-codebase, explore-docs, websearch)
# - 2 scripts (command-validator, statusline)
# - Configuration hooks (PreToolUse + Stop + Notification)
# - Status Line (necessite bun)
```

### Dependances recommandees macOS

```bash
# Package manager
brew install trash-cli      # Remplacement securise de rm
brew install tmux           # Multiplexeur terminal
brew install bun            # Runtime JS rapide (pour hooks/scripts)
brew install pv             # Barres de progression pipes
brew install jq             # Parsing JSON en CLI

# Optionnel mais utile
brew install gh             # GitHub CLI
brew install ripgrep        # Recherche rapide (rg)
brew install fd             # Find ameliore
brew install bat            # Cat avec coloration syntaxique
brew install fzf            # Recherche floue
```

### Terminaux recommandes

| Terminal | Description | Recommandation |
|----------|-------------|----------------|
| **CEMUX** | Terminal cree pour les coding agents. Build au-dessus de Ghostty. Tabs verticaux + horizontaux, notifications agent, browser integre, split pane, scriptable, Swift/AppKit (pas Electron), gratuit | **Meilleur choix** |
| **Ghostty** | Terminal rapide, GPU-accelerated. `Cmd+1/2/3` pour tabs, `Cmd+W` fermer, `Cmd+T` nouveau tab | Excellent |
| **iTerm2** | Terminal classique macOS, tres configurable | Classique |
| **Terminal.app** | App par defaut macOS | Basique |
| **Hyper** (hyterm) | Alternative populaire basee sur Electron | Alternative |

---

## 2. Interface et Raccourcis Clavier

### Syntaxe d'entree

| Prefixe | Usage | Exemple |
|---------|-------|---------|
| `@` | Taguer un fichier dans le prompt | `@src/app/page.tsx corrige le bug` |
| `/` | Commande slash (skill, commande native) | `/apex --auto` |
| `!` | Executer une commande bash directe | `!ls -la` |
| Texte libre | Langage naturel (le plus courant) | `Ajoute un bouton de connexion` |

### Raccourcis clavier essentiels

| Raccourci | Action |
|-----------|--------|
| `Escape` x2 | Premier Escape = clear input, Deuxieme = historique prompts |
| `Shift+Tab` | Changer le mode permissions (Plan/Auto/Bypass) |
| `Ctrl+T` | Voir les taches en cours |
| `Ctrl+O` | Ouvrir le transcript verbose |
| `Ctrl+S` | Sauvegarder le prompt courant |
| `Cmd+P` | Changer de modele (Sonnet/Opus) |
| `Ctrl+C` | Interrompre l'action en cours |
| `Ctrl+D` | Quitter Claude Code |
| Fleche haut | Naviguer dans l'historique des prompts |

### Commandes slash natives

| Commande | Description |
|----------|-------------|
| `/clear` | Reset du contexte (OBLIGATOIRE entre taches majeures) |
| `/init` | Generer un CLAUDE.md pour le projet courant |
| `/stats` | Statistiques de la session |
| `/usage` | Limites et consommation du plan |
| `/context` | Pourcentage de tokens utilises |
| `/model` | Changer de modele IA |
| `/mcp` | Voir les MCP connectes et leur etat |
| `/agent` | Gerer les agents (creer, lister, supprimer) |
| `/voice` | Activer le mode vocal |
| `/loop` | Prompt a intervalle regulier (expire avec la session) |
| `/schedule` | Tache cron persistante (survit a la session) |
| `/simplify` | Review par 3 agents paralleles |
| `/copy` | Copier la derniere reponse dans le presse-papier |
| `/config` | Ouvrir la configuration |
| `/plugin` | Installer un plugin |
| `/debug` | Auto-reparer les problemes Claude Code |

### Modes de permissions

| Mode | Comportement | Quand l'utiliser |
|------|-------------|-----------------|
| **Plan** | Claude explique, ne fait rien | Comprendre avant d'agir |
| **Auto** | Claude agit avec confirmation | Usage quotidien normal |
| **Bypass** | Claude agit sans confirmation | Confiance totale (avec deny-list) |

> **Basculer** : `Shift+Tab` dans l'interface, ou configurer `defaultMode` dans settings.json

---

## 3. Systeme de Memoire (3 niveaux CLAUDE.md)

### Architecture a 3 niveaux

La memoire de Claude Code fonctionne comme un systeme de fichiers hierarchique. Chaque niveau est charge automatiquement quand Claude travaille dans le contexte correspondant.

```
Niveau 1 — GLOBAL (toujours charge)
~/.claude/CLAUDE.md
~/.claude/rules/*.md

Niveau 2 — PROJET (charge quand on est dans le projet)
./CLAUDE.md                    # Racine du projet
./src/CLAUDE.md                # Sous-dossier specifique

Niveau 3 — CONTEXTUEL (charge a la demande)
~/.claude/rules/*.md           # Avec globs specifiques
```

### Niveau 1 : Memoire Globale (~/.claude/CLAUDE.md)

Ce fichier est **toujours charge**, quelle que soit la conversation. Il contient :

```markdown
# CLAUDE.md — Configuration Globale

## Identite
- Nom, role, machines, stack technique

## Regles absolues
- Commandes interdites
- Protocoles de securite
- Preferences durables

## Conventions
- Style de code (indent, quotes, semicolons)
- Format des commits (Conventional Commits)
- Workflow obligatoire (Explore > Plan > Code > Test)
```

**Regles de taille** :
- Maximum **500 lignes** (anti-pattern mega-CLAUDE.md)
- Si un bloc depasse 50 lignes de detail, le deplacer dans `~/.claude/rules/`
- Si le fichier depasse 200 lignes, auditer et factoriser

### Niveau 2 : Memoire Projet (./CLAUDE.md)

Ce fichier est charge quand Claude travaille dans le dossier du projet :

```bash
# Generer automatiquement un CLAUDE.md pour le projet
cd ~/mon-projet
claude
# puis taper : /init
```

Contenu type d'un CLAUDE.md projet :

```markdown
# Mon Projet

## Stack
- Next.js 15 + TypeScript + Prisma + Tailwind

## Architecture
- apps/ — applications
- packages/ — librairies partagees

## Commandes
- `pnpm dev` — demarrer le dev server
- `pnpm test` — lancer les tests
- `pnpm lint` — verifier le code

## Conventions
- Named exports uniquement
- Composants dans src/components/
- API routes dans src/app/api/
```

### Niveau 3 : Memoire Sous-dossier

Charge uniquement quand Claude lit un fichier dans le dossier concerne :

```
./src/components/CLAUDE.md
# Charge quand Claude accede a un fichier dans src/components/
```

Exemple :

```markdown
# Composants UI

## Convention
- Chaque composant dans son propre fichier
- Export nomme, pas de default export
- Props typees avec interface (pas type)
- Tests dans __tests__/ComponentName.test.tsx
```

### Rules : Memoire Modulaire (~/.claude/rules/)

Les rules sont des fichiers Markdown avec metadata YAML optionnel :

#### Rule toujours chargee (alwaysApply: true, ou sans metadata)

```bash
# Fichier : ~/.claude/rules/01-security.md
```

```markdown
# Regles de Securite

- JAMAIS utiliser rm -rf
- JAMAIS committer un .env
- Toujours utiliser trash au lieu de rm
```

#### Rule conditionnelle (chargee selon le type de fichier)

```bash
# Fichier : ~/.claude/rules/20-json.md
```

```markdown
---
globs: ["*.json"]
---
# Regles JSON

- JAMAIS ecraser un fichier JSON sans backup
- Toujours valider la syntaxe apres modification
- Indentation : 2 spaces
```

#### Rule conditionnelle par dossier

```bash
# Fichier : ~/.claude/rules/30-api.md
```

```markdown
---
globs: ["src/api/**"]
---
# Regles API

- Toujours valider les entrees avec zod
- Rate limiting obligatoire
- Logs structures JSON
```

### Auto-Memory

La memoire automatique persiste les informations entre sessions :

```json
// Dans ~/.claude/settings.json
{
  "autoMemoryEnabled": true
}
```

Quand active, Claude sauvegarde automatiquement :
- Les preferences detectees
- Les corrections recues
- Les decisions techniques
- Stockage : `~/.claude/projects/<chemin-projet>/memory/MEMORY.md`

### Commande /init

```bash
# Dans un projet existant, generer un CLAUDE.md adapte
cd ~/mon-projet
claude
# > /init

# Claude va :
# 1. Scanner l'arborescence du projet
# 2. Lire package.json, tsconfig, etc.
# 3. Generer un CLAUDE.md avec stack, commandes, conventions
```

### Marqueur CRITICAL

Pour les regles les plus importantes, utiliser le prefixe `CRITICAL:` :

```markdown
- CRITICAL: Ne JAMAIS supprimer de fichiers sans backup
- CRITICAL: Ne JAMAIS toucher ~/Library/Mobile Documents
- CRITICAL: Toujours lancer les tests avant de commiter
```

Claude traite les lignes `CRITICAL:` avec une priorite absolue.

### Protocole de persistance mémoire (Règle #0 appliquée)

Toute information importante DOIT être persistée selon ce protocole :

| Type d'information | Destination | Exemple |
|-------------------|-------------|---------|
| Règle comportementale | `~/.claude/rules/` | "JAMAIS rm sans backup" |
| Architecture/décision | `~/.claude/memory/` | "Service X dépend de Y" |
| Erreur corrigée | `~/.claude/memory/02_lessons_log.md` | "Bug X résolu par Y" |
| Préférence utilisateur | `~/.claude/rules/11-auto-learning-live.md` | "Toujours français" |
| Relation/historique | Knowledge Graph (MCP Memory) | "MacBook → SSH → BOKADOR" |

### Sauvegarde de la mémoire
- `~/.claude/` DOIT être dans un repo git (ou synchronisé)
- Commit automatique après chaque modification de rules/ ou memory/
- JAMAIS supprimer un fichier de mémoire sans triple sauvegarde (Règle #1)
- JAMAIS écraser un fichier de mémoire — toujours fusionner (append)

### Vérification périodique
```bash
# Vérifier que la mémoire est sauvegardée
ls -la ~/.claude/memory/
git -C ~/.claude log --oneline -5  # Si repo git
tmutil status                       # Time Machine actif ?
```

---

## 4. Settings et Permissions

### Fichier de configuration principal

```bash
# Emplacement
~/.claude/settings.json        # Global (toutes les machines)
~/.claude/settings.local.json  # Local (cette machine uniquement, gitignore)
./.claude/settings.json        # Projet (partage avec l'equipe via git)
./.claude/settings.local.json  # Projet local (gitignore)
```

### Hierarchie de priorite

```
Projet local > Projet > Global local > Global
```

### Configuration Bypass Mode (autonomie totale)

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "deny": [
      "Bash(rm -rf *)",
      "Bash(rm -r *)",
      "Bash(sudo rm *)",
      "Bash(sudo *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force*)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git reset --hard*)",
      "Bash(git clean -f*)",
      "Bash(dd if=*)",
      "Bash(mkfs*)",
      "Bash(curl * | bash*)",
      "Bash(wget * | bash*)",
      "Bash(netcfg -d*)"
    ]
  }
}
```

### Configuration complete recommandee (settings.json macOS)

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

### Explication des modes

| Mode | Config | Comportement |
|------|--------|-------------|
| **Plan** | `"plan"` | Claude planifie, ne fait rien sans confirmation |
| **Auto** | `"autoEdit"` | Claude ecrit du code, demande confirmation pour bash |
| **Bypass** | `"bypassPermissions"` | Claude fait tout sauf ce qui est dans `deny` |

> **Principe Melvynx** : Bypass + deny-list = meilleur ratio autonomie/securite.
> Les commandes dans `deny` passent automatiquement en Ask Mode (confirmation requise).

### Deny-list standard macOS

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(rm -r *)",
      "Bash(sudo rm *)",
      "Bash(sudo *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git push --force origin main*)",
      "Bash(git reset --hard*)",
      "Bash(git clean -f*)",
      "Bash(dd if=*)",
      "Bash(mkfs*)",
      "Bash(diskutil eraseDisk*)",
      "Bash(diskutil partitionDisk*)",
      "Bash(launchctl unload*)",
      "Bash(curl * | bash*)",
      "Bash(curl * | sh*)",
      "Bash(wget * | bash*)",
      "Bash(wget * | sh*)"
    ]
  }
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

### Deny-list renforcee (config Kim OG++)

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(rm -r *)",
      "Bash(rm -f *)",
      "Bash(sudo rm *)",
      "Bash(sudo *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force*)",
      "Bash(git reset --hard*)",
      "Bash(git clean *)",
      "Bash(dd if=*)",
      "Bash(mkfs*)",
      "Bash(curl * | bash*)",
      "Bash(wget * | bash*)",
      "Bash(netcfg *)",
      "Bash(launchctl unload *)",
      "Bash(launchctl bootout *)",
      "Bash(diskutil erase*)",
      "Bash(diskutil partitionDisk*)",
      "Bash(tmutil delete*)",
      "Bash(kill -9 *)",
      "Bash(killall *)",
      "Bash(pkill *)",
      "Bash(shutdown *)",
      "Bash(reboot*)",
      "Bash(halt*)",
      "Bash(shred *)"
    ],
    "additionalDirectories": ["/etc", "/usr", "/var"]
  }
}
```

### Installer trash-cli (remplacement de rm)

```bash
# Installation
brew install trash-cli

# Usage
trash fichier.txt                    # Envoie dans la Corbeille macOS
trash -y dossier/                    # Sans confirmation
trash fichier1.txt fichier2.txt      # Plusieurs fichiers

# Utiliser : trash fichier.txt (au lieu de rm)
# Recuperation : ouvrir la Corbeille macOS et restaurer
```

> **REGLE ABSOLUE** : Sur macOS, TOUJOURS utiliser `trash` au lieu de `rm`. C'est la premiere couche de securite anti-perte de donnees.

### settings.json vs settings.local.json

| Fichier | Usage | Git |
|---------|-------|-----|
| `settings.json` | Partage equipe, config commune | Commite |
| `settings.local.json` | Machine locale, tokens, chemins locaux | Gitignore |

---

## 5. Architecture de Securite (3+1 couches)

### Vue d'ensemble

```
Couche 4 (optionnelle) : Hook PreToolUse — command-validator
        |
Couche 3 : CLAUDE.md — instructions comportementales
        |
Couche 2 : Deny-list — override le bypass → Ask Mode
        |
Couche 1 : Bypass permissions — autonomie totale
```

### Couche 1 : Bypass Permissions

```json
// ~/.claude/settings.json
{
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

- Claude agit **sans jamais demander confirmation**
- Gain de temps enorme sur les taches repetitives
- DANGEREUX sans les couches superieures

### Couche 2 : Deny-list (Guard Rails)

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)"
    ]
  }
}
```

- Les commandes matchant un pattern `deny` passent en **Ask Mode**
- Claude DOIT demander confirmation avant d'executer
- C'est le filet de securite principal
- Fonctionne meme en Bypass Mode

### Couche 3 : CLAUDE.md (Instructions comportementales)

```markdown
# Dans CLAUDE.md
- CRITICAL: Ne JAMAIS utiliser rm — utiliser trash
- CRITICAL: Backup obligatoire avant modification
- CRITICAL: Ne JAMAIS toucher ~/Library/Mobile Documents
```

- Regles "douces" : Claude les suit mais peut techniquement les ignorer
- Efficaces a ~95% (les modeles respectent bien les instructions CRITICAL)
- Completent la deny-list avec des regles nuancees

### Couche 4 : Hook PreToolUse (command-validator)

```json
// Dans settings.json > hooks > PreToolUse
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "bun ~/.claude/scripts/command-validator/src/cli.ts \"$TOOL_INPUT\"",
        "timeout": 5000
      }
    ]
  }
}
```

Le command-validator Melvynx :
- Analyse les commandes au niveau AST (pas de faux positifs par regex)
- Bloque les commandes dangereuses AVANT execution
- Ecrit en TypeScript, execute via Bun
- Retourne `exit 2` pour bloquer, `exit 0` pour autoriser

```bash
# Installation du command-validator
cd ~/.claude/scripts
git clone https://github.com/Melvynx/aiblueprint
cp -r aiblueprint/command-validator ./
cd command-validator
bun install
```

### Commande trash : le remplacement de rm

```bash
# Installation macOS
brew install trash-cli

# Wrapper optionnel dans /usr/local/bin/rm (protection ultime)
#!/bin/bash
echo "[SECURITE] rm intercepte — redirection vers trash"
echo "Fichier(s) : $@" >> /tmp/rm-intercepted.log
trash "$@"
```

> **Principe** : Les 4 couches se completent. La couche 1 donne l'autonomie, les couches 2-4 protegent contre les erreurs.

---

## 6. Skills

### Concept

Les skills sont des **prompts reutilisables** stockes dans des dossiers avec un fichier d'entree `SKILL.md`. Ils structurent les workflows complexes et evitent de reexpliquer les memes instructions.

### Emplacement

```
~/.claude/skills/             # Skills globaux (tous les projets)
./.claude/skills/             # Skills projet (specifiques au repo)
```

### Structure d'un skill

```
~/.claude/skills/mon-skill/
  SKILL.md                    # Point d'entree (OBLIGATOIRE)
  steps/                      # Etapes optionnelles (Prompt Discovery)
    step1-init.md
    step2-analyse.md
    step3-execute.md
    step4-verify.md
```

### SKILL.md type

```markdown
# Mon Skill

## Description
Ce skill fait X pour Y.

## Etapes
1. Lire le fichier @steps/step1-init.md et suivre les instructions
2. Lire le fichier @steps/step2-analyse.md et suivre les instructions
3. Lire le fichier @steps/step3-execute.md et suivre les instructions
4. Lire le fichier @steps/step4-verify.md et suivre les instructions
```

> **Pourquoi des etapes separees ?** C'est le pattern **Prompt Discovery** de Melvynx.
> Les transformers privilegient le debut et la fin du contexte. En decoupant en etapes,
> chaque instruction est lue "juste avant" d'etre executee = toujours recente dans le contexte.

### JAMAIS creer de skills manuellement

```bash
# BONNE methode : demander a Claude de le creer
claude
# > /skill-creator
# > "Cree un skill qui fait X, Y, Z"

# Claude genere la structure complete avec SKILL.md + steps/
```

### Installation de skills externes

```bash
# Depuis le registre Melvynx
npx skill add melvynx/apex
npx skill add melvynx/oneshot

# Ou via ig-blueprint-cli (installe tous les skills gratuits)
npx ig-blueprint-cli
```

### Skills gratuits Melvynx (inclus dans ig-blueprint-cli)

| Skill | Commande | Description |
|-------|----------|-------------|
| **Apex** | `/apex` | Feature complete (explore > plan > code > test) |
| **OneShot** | `/oneshot` | Fix rapide en une passe |
| **Commit** | `/commit` | Commit propre Conventional Commits |
| **Fix-Errors** | `/fix-errors` | Correction d'erreurs avec sub-agents |
| **Fix-Grammar** | `/fix-grammar` | Correction orthographe/grammaire |
| **Fix-PR-Comments** | `/fix-pr-comments` | Corriger les commentaires d'une PR |
| **Prompt-Creator** | `/prompt-creator` | Generer un prompt optimise |
| **Skill-Creator** | `/skill-creator` | Creer un nouveau skill |
| **Subagent-Creator** | `/subagent-creator` | Creer un sub-agent |
| **Ultrathink** | `/ultrathink` | Reflexion profonde (2+ minutes) |
| **Workflow-Apex-Free** | `/workflow-apex-free` | Version gratuite d'Apex |
| **Claude-Memory** | `/load-memory` | Gerer la memoire (CLAUDE.md + rules/) |

### Desactiver un skill sans le supprimer

```bash
# Creer un sous-dossier "disabled" et y deplacer le skill
mkdir -p ~/.claude/skills/disabled
mv ~/.claude/skills/mon-skill ~/.claude/skills/disabled/mon-skill

# Pour reactiver : deplacer dans le sens inverse
```

### Choix du skill selon la tache

| Tache | Skill recommande |
|-------|-----------------|
| Feature complete | `/apex --auto` |
| Fix rapide | `/oneshot` |
| Bug a debugger | `/debug` |
| Commit propre | `/commit` |
| Pull Request | `/create-pr` |
| Review code | `/simplify` |
| Reflexion complexe | `/ultrathink` |
| Correction erreurs build | `/fix-errors` |
| Petite modification | Langage naturel (pas de skill) |

### Skill de sauvegarde automatique

Un skill dédié à la sauvegarde automatique doit être configuré :

```
# ~/.claude/skills/auto-backup/SKILL.md
---
name: auto-backup
description: Sauvegarde automatique de la config Claude et du système
---

## Fonctionnalités
1. Backup quotidien de ~/.claude/ (settings, rules, memory, skills, agents)
2. Vérification Time Machine active
3. Snapshot avant toute modification de zone critique
4. Commit git automatique des changements de config

## Commandes
- `backup-claude` : sauvegarde complète ~/.claude/
- `backup-check` : vérifie que toutes les sauvegardes sont à jour
- `backup-restore [date]` : restaure une config depuis un backup

## Automatisation macOS
- LaunchAgent : `com.claude.auto-backup` (quotidien)
- Time Machine : vérifier `tmutil status` avant toute opération risquée
- Hook PostToolUse sur Edit|Write : snapshot automatique des zones critiques
```

---

## 7. Hooks

### Concept

Les hooks sont des scripts executes automatiquement a des moments precis du cycle de vie de Claude Code. Ils permettent d'ajouter de la validation, du formatage, des notifications.

### Types de hooks

| Hook | Declenchement | Usage typique |
|------|---------------|---------------|
| **PreToolUse** | AVANT chaque utilisation d'outil | Validation de commandes, securite |
| **PostToolUse** | APRES chaque utilisation d'outil | Formatage, linting |
| **Stop** | Quand Claude termine une tache | Notification sonore |
| **Notification** | Quand Claude envoie une notification | Alerte visuelle/sonore |

### 4 hooks recommandes

| Hook | Declencheur | Commande macOS | Usage |
|------|------------|----------------|-------|
| **PreToolUse** | Avant execution outil | `bun command-validator/src/cli.ts` | Valide les commandes (AST-level, zero faux positifs) |
| **Stop** | Fin de tache | `afplay /System/Library/Sounds/Glass.aiff` | Son "tache terminee" |
| **Notification** | Claude a besoin d'attention | `afplay /System/Library/Sounds/Funk.aiff` | Son "question en attente" (different du Stop) |
| **PostToolUse** (local) | Apres Edit/Write | `npx prettier --write $FILEPATH` | Formatter automatiquement |

### Configuration dans settings.json

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "bun ~/.claude/scripts/command-validator/src/cli.ts \"$TOOL_INPUT\"",
        "timeout": 5000
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "command": "npx prettier --write \"$TOOL_INPUT_PATH\" 2>/dev/null || true",
        "timeout": 10000
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "command": "afplay /System/Library/Sounds/Glass.aiff",
        "timeout": 5000
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "command": "afplay /System/Library/Sounds/Funk.aiff",
        "timeout": 5000
      }
    ]
  }
}
```

### Hook Stop : Son de fin de tache (macOS)

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "command": "afplay /System/Library/Sounds/Glass.aiff",
        "timeout": 5000
      }
    ]
  }
}
```

### Hook PreToolUse : Command Validator (securite)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "bun ~/.claude/scripts/command-validator/src/cli.ts \"$TOOL_INPUT\"",
        "timeout": 5000
      }
    ]
  }
}
```

Le command-validator analyse les commandes au niveau AST :
- `exit 0` = commande autorisee
- `exit 2` = commande bloquee (Claude recoit un message d'erreur)

> **Important** : un seul hook PreToolUse est recommande. Ne pas empiler les hooks de validation (risque de faux positifs et ralentissement).

### Hook PostToolUse : Prettier (formatage automatique)

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "command": "npx prettier --write \"$TOOL_INPUT_PATH\" 2>/dev/null || true",
        "timeout": 10000
      }
    ]
  }
}
```

Installation de Prettier :

```bash
npm install -g prettier
# OU dans le projet
pnpm add -D prettier
```

### Hooks locaux (settings.local.json)

Pour des hooks specifiques a une machine (chemins, sons, outils locaux) :

```bash
# Fichier : ~/.claude/settings.local.json (gitignore)
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "command": "osascript -e 'display notification \"Claude a termine\" with title \"Claude Code\"'",
        "timeout": 5000
      }
    ]
  }
}
```

### Bonnes pratiques hooks

- **Ne pas surcharger** : chaque hook ajoute de la latence
- **Timeout court** : 5000ms max pour PreToolUse, 10000ms pour PostToolUse
- **Un seul PreToolUse** : le command-validator suffit
- **Toujours `async: true`** pour les sons (non-bloquant)
- **settings.local.json** pour les hooks locaux (sons, notifications macOS)
- **JAMAIS creer de hooks manuellement** dans le code — configurer via settings.json ou demander a Claude

### Variables d'environnement disponibles dans les hooks

| Variable | Contenu |
|----------|---------|
| `$TOOL_NAME` | Nom de l'outil (Bash, Write, Edit, etc.) |
| `$TOOL_INPUT` | Input brut passe a l'outil |
| `$TOOL_INPUT_PATH` | Chemin du fichier (pour Write/Edit) |
| `$SESSION_ID` | ID de la session Claude |

---

## 8. Sons et Notifications macOS

### Sons systeme macOS disponibles

```bash
ls /System/Library/Sounds/
# Basso.aiff  Blow.aiff  Bottle.aiff  Frog.aiff  Funk.aiff
# Glass.aiff  Hero.aiff  Morse.aiff  Ping.aiff  Pop.aiff
# Purr.aiff  Sosumi.aiff  Submarine.aiff  Tink.aiff
```

### Tableau des sons et usages recommandes

| Fichier | Description | Usage recommande |
|---------|-------------|-----------------|
| `Glass.aiff` | Son cristallin | Fin de tache (Stop) |
| `Funk.aiff` | Son funk, plus insistant | Notifications (question en attente) |
| `Ping.aiff` | Son ping | Alerte legere |
| `Pop.aiff` | Son pop | — |
| `Purr.aiff` | Ronronnement | — |
| `Submarine.aiff` | Sous-marin | — |
| `Hero.aiff` | Son heroique | Succes important |
| `Basso.aiff` | Son grave | Erreur |

### Configuration recommandee

- **Stop** (tache terminee) : `Glass.aiff` — son doux
- **Notification** (question) : `Funk.aiff` — son different, plus insistant

### Tester les sons

```bash
# Tester un son individuel
afplay /System/Library/Sounds/Glass.aiff
afplay /System/Library/Sounds/Funk.aiff

# Tester tous les sons disponibles
for f in /System/Library/Sounds/*.aiff; do
  echo "Son : $(basename $f)"
  afplay "$f"
  sleep 1
done

# Sons recommandes
afplay /System/Library/Sounds/Glass.aiff       # Fin de tache (Stop)
afplay /System/Library/Sounds/Funk.aiff        # Notification
afplay /System/Library/Sounds/Basso.aiff       # Erreur
afplay /System/Library/Sounds/Hero.aiff        # Succes important
afplay /System/Library/Sounds/Ping.aiff        # Alerte legere
```

### Note importante : Sandbox Audio

Les sons ne fonctionnent PAS depuis un processus sandboxe (ex: terminal Claude Code). Ils fonctionnent via hooks et LaunchAgents.

Pour tester directement : `afplay /System/Library/Sounds/Glass.aiff` dans Terminal.app (pas dans Claude Code).

---

## 9. MCP (Model Context Protocol)

### Concept

Les MCP (Model Context Protocol) sont des serveurs externes qui fournissent des outils supplementaires a Claude Code. Chaque MCP consomme du contexte meme sans etre utilise.

### Regle critique : MAXIMUM 2-3 MCP

```
Chaque MCP ajoute ses definitions d'outils au contexte (~2-5K tokens chacun).
Au-dela de 3 MCP, la qualite des reponses commence a se degrader.
Si les MCP representent >10% du contexte : Tool Search s'active automatiquement.
```

JAMAIS plus de 3 MCP simultanement sur macOS (degrade la qualite).

### Configuration Tool Search

```json
// Dans settings.json
{
  "toolSearchMode": "auto"
}
```

Quand active, Claude ne charge les outils MCP qu'a la demande au lieu de tous les charger au demarrage. Recommande des 3+ MCP.

### MCP recommandes (formation Melvynx)

#### 1. Context7 (gratuit)

Documentation technique a jour pour n'importe quelle librairie.

```bash
# Installation
claude mcp add context7 --url "https://mcp.context7.com/mcp" -s user

# Ou via la commande simplifiee
claude mcp add --scope user context7

# Usage dans Claude
# Claude utilise automatiquement Context7 quand il a besoin de docs
```

Prerequis : compte GitHub (sign in gratuit sur context7.com)

#### 2. Exa.ai (recherche web IA)

```bash
# Installation (necessite une cle API — 20$ de credit gratuit a l'inscription)
claude mcp add exa --url "https://mcp.exa.ai" --api-key "YOUR_KEY" -s user

# Ou via la commande simplifiee
claude mcp add --scope user exa

# Usage dans Claude
# Claude utilise Exa pour chercher sur le web quand necessaire
```

### Commandes MCP

```bash
# Voir les MCP installes
claude mcp list

# Ajouter un MCP
claude mcp add <nom> --url <url> -s user          # Scope user (global)
claude mcp add <nom> --url <url> -s project        # Scope projet

# Supprimer un MCP
claude mcp remove <nom>

# Verifier dans l'interface
/mcp
```

### Configuration MCP dans settings.json

```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp"
    },
    "exa": {
      "url": "https://mcp.exa.ai",
      "env": {
        "EXA_API_KEY": "votre-cle-api"
      }
    }
  }
}
```

### MCP supplementaires utiles (optionnels)

| MCP | Usage | Cout |
|-----|-------|------|
| Memory | Memoire persistante (knowledge graph) | Gratuit (local) |
| Fetch | Telecharger des URLs | Gratuit (local) |
| Gmail | Lire/envoyer des emails | Gratuit (OAuth) |
| Google Calendar | Gerer le calendrier | Gratuit (OAuth) |

> **Rappel** : ne pas depasser 3 MCP actifs simultanement. Desactiver ceux qui ne sont pas utilises.

---

## 10. Sub-Agents

### Pourquoi des sub-agents ?

| Action | Sans sub-agent | Avec sub-agent |
|--------|---------------|----------------|
| Recherche web | ~28K tokens dans le contexte principal | 0 tokens (isole) |
| Exploration codebase | ~15K tokens | 0 tokens |
| Lecture documentation | ~20K tokens | 0 tokens |
| Retour au principal | — | ~1500 mots (resume) |

Les sub-agents sont des **conversations isolees** avec leur propre contexte de 120K tokens. Ils retournent un resume (~1500 mots) au contexte principal, preservant ainsi la capacite du thread principal.

### Types de sub-agents

| Type | Description | Usage |
|------|-------------|-------|
| **Explore** | Rapide, lecture seule | Recherche, exploration |
| **Task** | Generique, peut modifier | Sous-taches complexes |

### Creation d'un sub-agent

```bash
# Dans Claude Code
/agent

# Choisir :
# 1. Create New Agent
# 2. Personal Folder (global) ou Project Folder (projet)
# 3. Generate (Claude cree le AGENT.md)
```

### 3 sub-agents recommandes (Melvynx)

#### 1. Web Search

```markdown
# AGENT.md — ~/.claude/agents/web-search/AGENT.md

## Role
Tu es un agent specialise en recherche web. Tu utilises les MCP disponibles
(Exa, Context7) pour trouver des informations a jour.

## Instructions
1. Recevoir la question de l'agent principal
2. Chercher les sources les plus pertinentes
3. Retourner un resume structure avec les URLs sources
4. Maximum 1500 mots dans la reponse
```

#### 2. Explore Codebase

```markdown
# AGENT.md — ~/.claude/agents/explore-codebase/AGENT.md

## Role
Tu es un agent specialise en exploration de code. Tu analyses l'architecture,
les patterns, et les conventions d'un projet.

## Instructions
1. Recevoir la question de l'agent principal
2. Explorer les fichiers pertinents (glob, grep, read)
3. Retourner une synthese structuree
4. Inclure les chemins des fichiers importants
```

#### 3. Doc Explorer

```markdown
# AGENT.md — ~/.claude/agents/explore-docs/AGENT.md

## Role
Tu es un agent specialise en exploration de documentation.
Tu utilises Context7 et les fichiers locaux pour trouver des informations.

## Instructions
1. Recevoir la question de l'agent principal
2. Chercher dans la doc locale et Context7
3. Retourner les extraits pertinents avec references
```

### Utilisation des sub-agents

Claude utilise automatiquement les sub-agents quand ils sont disponibles et pertinents. On peut aussi les invoquer explicitement :

```
Utilise le sub-agent web-search pour trouver la doc de Prisma 6
```

---

## 11. Workflows

### Pattern fondamental : EPCT

Tout workflow Claude Code suit le pattern **Explore > Plan > Code > Test** :

```
1. EXPLORE — Lire le code existant, comprendre l'architecture
2. PLAN   — Decrire ce qui va etre fait, dans quel ordre
3. CODE   — Implementer les changements
4. TEST   — Verifier que tout fonctionne
```

> **Regle Melvynx** : JAMAIS coder sans avoir explore. JAMAIS merger sans avoir teste.

### /apex — Feature complete (workflow principal)

```bash
# Usage standard
/apex

# Avec flags
/apex --auto            # Mode bypass (pas de confirmation)
/apex --auto --test     # Bypass + lancer les tests
/apex --auto --branch   # Bypass + creer une branche git
/apex --auto --pr       # Bypass + creer une PR
/apex --economy         # Economiser les tokens (plan 20$)
/apex --examine         # Mode examen (plus de detail)
/apex --interactive     # Mode interactif (questions a chaque etape)
/apex --teams           # Utiliser les teams (multi-agents)
/apex --resume          # Reprendre un workflow interrompu
/apex --plan            # S'arreter apres la phase de planification
```

Etapes internes d'Apex :
1. **Discovery** — Explorer le codebase (sub-agent)
2. **Plan** — Generer un plan detaille
3. **Execute** — Implementer les changements
4. **Verify** — Tests + linting
5. **Commit** — Commit propre (si --auto)

> **Taux de succes** : >99% sur les features bien decrites (source Melvynx)

### /oneshot — Fix rapide

```bash
/oneshot
# Decris le fix en une phrase
# Claude corrige en une seule passe
```

Ideal pour :
- Corriger un bug simple
- Renommer une variable
- Ajouter un import manquant
- Corriger une typo

### /debug — Debugger un probleme

```bash
/debug
# Coller le message d'erreur ou decrire le symptome
# Claude ajoute des logs > demande de reproduire > corrige
```

Etapes :
1. Claude ajoute des `console.log` strategiques
2. Demande de reproduire le bug
3. Analyse les logs
4. Propose et applique le fix

### /brainstorm — Reflexion strategique

```bash
/brainstorm
# Poser une question d'architecture ou de strategie
# 4 rounds de reflexion iterative
```

> **Attention** : tres gourmand en tokens (~50K+). Utiliser avec parcimonie.

### /simplify — Review de code (3 agents)

```bash
/simplify
# 3 agents paralleles analysent le code
# Retournent des suggestions de simplification
```

### /fix-errors — Corrections paralleles

```bash
/fix-errors
# Claude lance des sub-agents pour corriger les erreurs de build/lint
# Chaque erreur est traitee par un agent dedie
```

### /ultrathink — Reflexion profonde

```bash
/ultrathink
# Dure 2+ minutes
# Pour les problemes complexes qui necessitent une reflexion approfondie
```

### /loop — Monitoring recurrent

```bash
/loop 5m "verifie que le serveur repond sur localhost:3000"
# Execute la commande toutes les 5 minutes
# Expire quand la session se ferme
```

### /schedule — Tache cron persistante

```bash
/schedule
# Cree une tache cron qui persiste meme apres fermeture de la session
# Utilise launchctl sur macOS
```

### /commit — Commit propre

```bash
/commit
# Analyse les changements
# Genere un message Conventional Commits
# Commit
```

### /create-pr — Pull Request

```bash
/create-pr
# Analyse les commits de la branche
# Genere titre + description
# Cree la PR via gh
```

### Tableau de decision des workflows

| Situation | Workflow | Flags |
|-----------|---------|-------|
| Nouvelle feature complete | `/apex` | `--auto --branch --test` |
| Fix rapide et simple | `/oneshot` | aucun |
| Bug a debugger | `/debug` | aucun |
| Question d'architecture | `/brainstorm` | aucun |
| Erreurs de build | `/fix-errors` | aucun |
| Review de code | `/simplify` | aucun |
| Reflexion longue et complexe | `/ultrathink` | aucun |
| Commit | `/commit` | aucun |
| Pull Request | `/create-pr` | aucun |
| Monitoring | `/loop 5m commande` | intervalle |
| Tache planifiee | `/schedule` | aucun |
| Petite modification | Langage naturel | PAS de skill |

### Regles d'usage des workflows

- **Ne PAS taguer de fichiers** (`@fichier`) avec `/apex` ou `/oneshot` — laisser l'exploration les trouver automatiquement
- **Ne PAS utiliser /batch** — preferer le main agent + sub-agents
- **Preferer le langage naturel** pour les petites modifications (economise le contexte)
- **`/clear` OBLIGATOIRE** entre taches majeures pour eviter l'autocompact

---

## 12. Gestion du Contexte

### Chiffres cles

| Metrique | Valeur |
|----------|--------|
| Contexte maximum | 200K tokens par conversation |
| Tokens au demarrage | ~27K (system prompt + tools + skills + CLAUDE.md + rules + MCP) |
| Seuil autocompact | ~96-99% du contexte |
| Contexte sub-agent | 120K tokens (isole) |
| Retour sub-agent | ~1500 mots |

### Le probleme "Lost in the Middle"

Les modeles de langage (transformers) traitent le contexte de maniere inegale :

```
[DEBUT — bien retenu] ... [MILIEU — mal retenu] ... [FIN — bien retenu]
```

Quand le contexte grandit, les instructions initiales de CLAUDE.md finissent "au milieu" et sont moins bien suivies. C'est pourquoi :
- Les skills decoupent en etapes (chaque instruction lue juste avant execution)
- `/clear` est obligatoire entre taches majeures
- Les sub-agents isolent les recherches volumineuses

### Commandes de gestion

```bash
# Verifier l'utilisation du contexte
/context

# Voir la consommation detaillee
/stats

# Reset complet du contexte
/clear

# Voir les limites du plan
/usage
```

### Regles de gestion du contexte

1. **`/clear` entre chaque tache majeure** — ne pas enchainer feature + debug + refactor dans la meme conversation
2. **`/context` regulierement** — verifier qu'on est sous 80%
3. **Si >80% : `/clear` immediat** — la qualite se degrade apres
4. **Langage naturel pour les petites modifs** — pas de skill pour changer une couleur
5. **Sub-agents pour les recherches** — isole les tokens du contexte principal
6. **Maximum 2-3 MCP** — chaque MCP ajoute des tokens au demarrage
7. **`toolSearchMode: auto`** — charge les outils MCP a la demande

### Debug du contexte sur macOS

**Proxyman** permet d'intercepter les requetes HTTP de Claude Code pour voir exactement ce qui est envoye a l'API :

```bash
# Installation
brew install --cask proxyman

# Lancer Proxyman
# Configurer le proxy HTTP
# Observer les requetes vers api.anthropic.com
# Analyser la taille du contexte envoye
```

### Prompt Discovery (solution Lost in the Middle)

Au lieu d'un gros prompt :

```markdown
# MAUVAIS — tout dans un seul SKILL.md
## Etape 1 : ...
## Etape 2 : ...
## Etape 3 : ...
## Etape 4 : ...
## Etape 5 : ...
```

Decouper en fichiers separes :

```markdown
# BON — SKILL.md pointe vers les etapes
1. Lire @steps/step1-init.md
2. Lire @steps/step2-analyse.md
3. Lire @steps/step3-plan.md
4. Lire @steps/step4-execute.md
5. Lire @steps/step5-verify.md
```

Chaque etape est lue **juste avant** d'etre executee, donc toujours en position "fin de contexte" (bien retenue).

---

## 13. Prompting Avance

### Greenfield (nouveau projet)

Pour demarrer un projet de zero :

```
1. Specifier la stack technique
   "Cree un nouveau projet Vite avec React, Tailwind et shadcn/ui"

2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
   "L'utilisateur arrive sur une page d'accueil avec un hero, un formulaire d'inscription,
    et une section temoignages"

3. Separer desktop et mobile
   "Sur desktop : 3 colonnes. Sur mobile : empile verticalement"

4. NE PAS specifier les conventions techniques au premier run
   Claude va suivre les conventions du boilerplate automatiquement

5. Utiliser une boilerplate existante pour imposer les patterns
   npx create-next-app@latest --typescript --tailwind --app
```

### Brownfield (projet existant)

Pour modifier un projet existant :

```
1. Screenshots annotes
   - Utiliser CleanShotX (macOS via SetApp)
   - Annoter : entourer le probleme, flecher ce qui doit changer
   - Claude VOIT les images — c'est la methode la plus efficace

2. References internes
   "Style comme le composant illustration-picker dans src/components/"

3. Ressources explicites
   "Image dans public/images/hero.png, couleur #FF6B35, font Inter"

4. Chemins precis
   "Le fichier a modifier est src/app/(auth)/login/page.tsx"
```

### Technique des 10 variations UI

```
Cree un fichier /demo/page.tsx avec 10 variations UI du composant Button.
Chaque variation dans un fichier separe : Button1.tsx a Button10.tsx.
Affiche-les toutes sur la meme page pour comparaison.
```

Ensuite :
1. Ouvrir la page `/demo` dans le navigateur
2. Comparer visuellement les 10 versions
3. Choisir la meilleure
4. Supprimer les autres

### Debug (methode la plus efficace)

```
Etape 1 : Demander a Claude d'ajouter des console.log strategiques
   "Ajoute des console.log dans le composant Login pour tracer le flow d'authentification"

Etape 2 : Reproduire le bug soi-meme dans le navigateur

Etape 3 : Copier la sortie console BRUTE (Cmd+A dans la console Chrome)

Etape 4 : Coller dans Claude — NE PAS dire quoi faire, exposer le PROBLEME
   "Voici les logs console quand je clique sur Login :
   [coller les logs bruts]
   Le comportement attendu est X, mais j'obtiens Y"
```

> **Regle Melvynx** : ne JAMAIS dire a Claude comment corriger un bug. Lui montrer le probleme (logs, screenshot, comportement) et le laisser diagnostiquer.

### Commentaires comme prompt injection

Documenter les helpers avec des exemples d'utilisation dans les commentaires :

```typescript
/**
 * Formatte un prix en euros
 * @example formatPrice(1234.5) → "1 234,50 EUR"
 * @example formatPrice(0) → "0,00 EUR"
 * @example formatPrice(-50) → "-50,00 EUR"
 */
export function formatPrice(amount: number): string {
  // ...
}
```

Avec la regle "lire 3 fichiers similaires avant de modifier", Claude absorbe ces commentaires-instructions automatiquement.

### Processus iteratif front-end

```
1. Decrire ce qu'on veut
2. Claude code
3. Screenshot du resultat
4. "Le bouton est trop gros, la couleur trop claire"
5. Claude ajuste
6. Re-screenshot
7. "Parfait, mais ajoute un hover effect"
8. Claude ajuste
9. Valide
```

### Continuous Learning (apprentissage continu)

```
Si une erreur se repete 2+ fois dans la meme session :
→ Claude cree automatiquement une regle dans rules/
→ L'erreur ne se reproduira plus dans les sessions futures
```

### Anti-patterns prompting

- Ne PAS relancer un skill pour une petite modif — langage naturel
- Ne PAS empiler les demandes pendant que l'IA travaille
- Ne PAS utiliser Chrome Headless (juge lourd et lent par Melvynx)

---

## 14. Multi-Agents (Teams, Work Trees, Tmux)

### Teams (agents paralleles)

Les teams permettent a plusieurs agents de travailler simultanement sur des **zones separees** du code :

```json
// Dans settings.json
{
  "enableAgentTeams": true
}
```

```bash
# Activer dans Apex
/apex --teams

# Claude repartit automatiquement les sous-taches entre agents
```

**Regles** :
- UNIQUEMENT pour des zones de modification **separees** (pas le meme fichier)
- Maximum **4 terminaux/agents** simultanes
- Chaque agent a son propre contexte
- Coordination automatique par Claude

### Work Trees (branches git paralleles)

Les work trees permettent de travailler sur plusieurs branches simultanement sans changer de branche :

```bash
# Creer un work tree
git worktree add ../mon-projet-feature feature/ma-feature

# Lancer Claude dans le work tree
cd ../mon-projet-feature
claude

# Maximum 4 work trees simultanement
```

**Usage typique** :
- Refactoring sur une branche + feature sur une autre
- Migration de base de donnees + code applicatif
- Tests de performance + optimisations

### Tmux (multiplexeur terminal macOS)

Tmux permet de gerer plusieurs sessions Claude Code dans un seul terminal :

```bash
# Installation
brew install tmux
```

#### Raccourcis tmux essentiels

Avec prefixe par defaut `Ctrl+B` (ou `Ctrl+A` si configure) :

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
| `Ctrl+B %` | Split vertical |
| `Ctrl+B "` | Split horizontal |
| `Ctrl+B fleches` | Naviguer entre les panes |
| `Ctrl+B [` | Mode scroll (q pour quitter) |

#### Commandes tmux

```bash
tmux ls                    # Lister les sessions
tmux attach -t <nom>       # Reattacher une session
tmux new -s <nom>          # Nouvelle session nommee
tmux kill-session -t <nom> # Tuer une session
```

#### Cas d'usage

Un tab par role : CC1 (feature), CC2 (review), Server, Ingest, etc.

#### Configuration recommandee (`~/.tmux.conf`)

```bash
# Prefixe Ctrl+A au lieu de Ctrl+B (plus accessible)
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# Navigation entre panes avec Shift+fleches
bind -n S-Left select-pane -L
bind -n S-Right select-pane -R
bind -n S-Up select-pane -U
bind -n S-Down select-pane -D

# Souris activee
set -g mouse on

# 256 couleurs
set -g default-terminal "screen-256color"

# Historique 10000 lignes
set -g history-limit 10000
```

#### Workflow multi-agents avec tmux

```bash
# Pane 1 : Claude principal (feature)
tmux new -s claude
claude --session-name "FEATURE: nouveau dashboard"

# Pane 2 : Claude secondaire (tests)
Ctrl+B %
claude --session-name "TESTS: dashboard tests"

# Pane 3 : Monitoring
Ctrl+B "
watch -n 5 "curl -s localhost:3000/health"
```

---

## 15. Pricing

### Plans et tarifs

| Plan | Prix/mois | Equivalent API | Ratio valeur |
|------|-----------|---------------|-------------|
| **Pro** | 20$ | ~160$ d'usage API | x8 |
| **Max 5x** | 100$ | ~800$ d'usage API | x8 |
| **Max 20x** | 200$ | ~3200$ d'usage API | x16 |

### Strategie de progression

```
1. Commencer a 20$/mois (Pro)
   → Suffisant pour 90% des usages
   → Flag --economy dans les workflows pour economiser

2. Passer a 100$/mois (Max 5x) quand :
   → Les limites sont atteintes regulierement
   → Besoin de sessions longues (>200K tokens)
   → Usage quotidien intensif

3. Passer a 200$/mois (Max 20x) quand :
   → Usage professionnel intensif
   → Multiple projets simultanes
   → Teams et multi-agents frequents
   → Le meilleur ratio qualite/prix
```

### Verifier sa consommation

```bash
# Dans Claude Code
/usage

# Affiche :
# - Tokens utilises / limite
# - Nombre de requetes restantes
# - Date de reset
```

### Economies de tokens

| Technique | Economie |
|-----------|----------|
| `/clear` entre taches | -50% gaspillage |
| Sub-agents pour recherche | -80% tokens contexte |
| Langage naturel (pas de skill) pour petites modifs | -30% overhead |
| `--economy` flag dans workflows | -40% tokens |
| Maximum 2-3 MCP | -10-20% tokens demarrage |
| `toolSearchMode: auto` | -5-15% tokens MCP |

---

## 16. Outils Recommandes macOS

### Terminal

| Outil | Description | Installation |
|-------|-------------|-------------|
| **CEMUX** | Terminal specialise Claude Code, build sur Ghostty, tabs verticaux + horizontaux, notifications agent, browser integre, split pane, scriptable, Swift/AppKit (pas Electron), gratuit | Via le site officiel |
| **Ghostty** | Terminal rapide, GPU-accelerated | `brew install --cask ghostty` |
| **iTerm2** | Terminal macOS classique avance | `brew install --cask iterm2` |
| **Terminal.app** | Terminal natif macOS | Pre-installe |

### IDE

| Outil | Description | Installation |
|-------|-------------|-------------|
| **VS Code** | IDE principal, extensions riches | `brew install --cask visual-studio-code` |
| **Cursor** | IDE avec IA integree | `brew install --cask cursor` |

### Screenshots et Annotation

| Outil | Description | Installation |
|-------|-------------|-------------|
| **CleanShotX** | Screenshots annotes (ESSENTIEL pour brownfield) | Via SetApp ou achat direct |

Usage CleanShotX pour le prompting :
1. `Cmd+Shift+4` (capture zone)
2. Annoter : entourer, flecher, ecrire
3. Glisser l'image dans Claude Code
4. Claude VOIT l'annotation et comprend ce qu'il faut faire

### Lanceur d'applications

| Outil | Description | Installation | Prix |
|-------|-------------|-------------|------|
| **Raycast** | Remplace Spotlight : app launcher, clipboard history, emoji picker, window management, screenshot. Extensions : VS Code, Figma, GitHub, Tailwind, iOS. Remplace `Cmd+Espace` | `brew install --cask raycast` | Gratuit (sauf IA) |

### Speech-to-Text

| Outil | Description | Installation | Prix |
|-------|-------------|-------------|------|
| **WhisperTurbo** | Transcription locale rapide et open source (app Melvynx). Modele PRK (Parakeet) = rapide/court, Whisper Large = long/francais | `brew install --cask whisper-turbo` | Gratuit |

### Proxy / Debug HTTP

| Outil | Description | Installation | Prix |
|-------|-------------|-------------|------|
| **Proxyman** | Intercepte les requetes HTTP de Claude Code. Voir exactement ce qui est envoye au modele (system prompt, tools, MCP, rules) | `brew install --cask proxyman` | Freemium |

Utile pour debugger le contexte envoye a l'API Claude :
1. Lancer Proxyman
2. Lancer Claude Code
3. Observer les requetes vers `api.anthropic.com`
4. Analyser la taille et le contenu du contexte

### Bundle macOS

| Outil | Description | Prix |
|-------|-------------|------|
| **SetApp** | Bundle d'outils macOS incluant CleanShotX | Abonnement |

### Package Manager

```bash
# Homebrew — indispensable sur macOS
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Commandes essentielles
brew install <package>        # Installer un CLI
brew install --cask <app>     # Installer une app GUI
brew update && brew upgrade   # Mettre a jour
brew cleanup                  # Nettoyer les anciennes versions
brew list                     # Lister les packages installes
```

### Multiplexeur Terminal

```bash
brew install tmux
```

### Poubelle securisee

```bash
brew install trash-cli

# Usage
trash fichier.txt              # Au lieu de rm fichier.txt
```

### Finder depuis le terminal

```bash
open .                         # Ouvre le Finder dans le dossier courant
open ~/Documents               # Ouvre un dossier specifique
open fichier.pdf               # Ouvre un fichier avec l'app par defaut
open -a "Visual Studio Code" . # Ouvre le dossier dans VS Code
```

### Homebrew — Paquets utiles (resume)

```bash
brew install trash-cli  # Remplacant de rm
brew install tmux       # Multiplexeur terminal
brew install bun        # Runtime JS rapide (StatusLine)
brew install pv         # Progress bar pour pipes
brew install jq         # Parsing JSON
brew install gh         # GitHub CLI
brew install ripgrep    # Recherche rapide (rg)
brew install fd         # Find ameliore
brew install bat        # Cat avec coloration
brew install fzf        # Recherche floue
```

---

## 17. Status Line

### Concept

La status line Melvynx affiche une barre d'information en bas du terminal avec :
- Dossier courant et repo git
- Modele IA utilise (Opus, Sonnet, etc.)
- Pourcentage de contexte utilise
- Cout estime de la session en $
- Branche git et status
- Limite hebdomadaire restante
- Graphique session et weekly limit

### Installation

```bash
# Pre-requis : bun
brew install bun

# La statusline est incluse dans ig-blueprint-cli
npx ig-blueprint-cli

# OU installation manuelle
cd ~/.claude/scripts
git clone https://github.com/Melvynx/aiblueprint
cd aiblueprint/statusline
bun install
```

### Configuration

La statusline se configure dans `~/.claude/settings.json` :

```json
{
  "statusLine": {
    "enabled": true,
    "script": "bun ~/.claude/scripts/statusline/src/index.ts"
  }
}
```

### Informations affichees

```
[~/mon-projet] [opus-4] [ctx: 42%] [$1.23] [main ✓] [weekly: 78%]
     |              |        |          |       |           |
     Dossier      Modele  Contexte    Cout    Git      Limite hebdo
```

### Si ne fonctionne pas

- Verifier que bun est installe : `bun --version`
- Si absent : `brew install oven-sh/bun/bun`
- `/debug` dans Claude Code pour auto-reparer

---

## 18. Dossier .claude

### Structure complete

```
~/.claude/
  CLAUDE.md                    # Memoire globale (niveau 1)
  settings.json                # Configuration globale
  settings.local.json          # Configuration locale (gitignore)
  memory.json                  # Memoire persistante JSON

  projects/                    # Historique des sessions par projet
    <hash-chemin>/
      memory/
        MEMORY.md              # Auto-memory du projet
      sessions/                # Historique des conversations

  plans/                       # Plans generes par les workflows

  rules/                       # Regles globales et conditionnelles
    01-security.md
    02-memory.md
    20-json.md                 # Conditionnel (globs: ["*.json"])
    21-git.md                  # Conditionnel (globs: git operations)
    ...

  skills/                      # Skills actifs
    apex/
      SKILL.md
      steps/
    oneshot/
    commit/
    ...
    disabled/                  # Skills desactives (pas supprimes)

  agents/                      # Agents actifs
    web-search/
      AGENT.md
    explore-codebase/
    explore-docs/
    ...

  scripts/                     # Scripts d'automatisation
    command-validator/         # Validateur de commandes (hook)
    statusline/                # Barre de statut
    ...

  hooks/                       # Scripts de hooks (reference)
    block-delete.sh
    ...

  tasks/                       # Taches executees

  teams/                       # Configuration teams (JSON)

  plugins/                     # Plugins installes

  bible/                       # Bible Melvynx (reference)
    BIBLE_MELVYNX_COMPLETE.md

  memory/                      # Fichiers memoire detailles
    00_status.md
    01_architecture.md
    02_lessons_log.md
    03_conventions.md
    ...

  logs/                        # Logs de modifications
    modifications.log

  backups/                     # Snapshots avant modifications
    YYYYMMDD_HHMMSS/
```

### Astuce : gerer sa config avec Claude

```bash
# Lancer Claude DANS le dossier .claude pour gerer sa propre configuration
cd ~/.claude
claude

# Exemples :
# "Ajoute une regle pour les fichiers Python"
# "Liste tous mes skills actifs"
# "Optimise mon settings.json"
```

---

## 19. Plugins

### Installation

```bash
# Dans Claude Code
/plugin

# Choisir dans la liste des plugins disponibles
```

### Recommandation Melvynx

> **Copier le code des plugins dans ses propres skills/agents** plutot que de dependre du systeme de plugins.

Raisons :
- Controle total sur le code
- Pas de mise a jour surprise
- Adaptation aux besoins specifiques
- Pas de dependance externe

### Plugins utiles

| Plugin | Description |
|--------|-------------|
| **Claude Memory** | Gestion memoire avancee |
| **Code Review** | Review automatique |
| **Git Integration** | Integration git amelioree |

---

## 20. Deploiement

### Netlify (front-end statique)

```bash
# Build du projet
npm run build
# OU
pnpm build

# Deploiement via drag & drop
# 1. Aller sur https://app.netlify.com/drop
# 2. Glisser le dossier dist/ ou out/
# 3. Deploye en 30 secondes
```

### Vercel (Next.js)

```bash
# Installation CLI
npm i -g vercel

# Deploiement
cd mon-projet-nextjs
vercel

# Deploiement production
vercel --prod
```

### Supabase (backend)

```bash
# CLI
npx supabase init
npx supabase start

# Dashboard : https://supabase.com/dashboard
```

### Workflow Melvynx de deploiement

```
1. /apex --auto --test --branch --pr
2. Review la PR
3. Merge
4. Deploiement automatique (Vercel/Netlify via GitHub)
```

---

## 21. OpenClaw (Agent distant Telegram)

### Concept

OpenClaw permet de controler Claude Code a distance via Telegram. L'agent tourne sur une machine dediee et repond aux messages Telegram.

### Installation locale macOS

```bash
# Installation sur le Mac
curl maltbot.sh
# Suivre les instructions d'installation
```

### Machine dediee recommandee

| Option | Specs | Prix |
|--------|-------|------|
| **Mac Mini** (recommande) | Apple Silicon, 8GB+ | ~600 EUR |
| **VPS Hetzner CAX21** | ARM, 4 vCPU, 8GB RAM | ~7.59$/mois |
| **VPS Hetzner CAX31** | ARM, specs superieures | ~$10+/mois |

### Avantages du Mac Mini dedie

- Toujours allume, jamais en veille
- Acces natif aux outils macOS (afplay, osascript, etc.)
- Pas besoin de VPS (economie sur le long terme)

### Alternative VPS

Si pas de Mac Mini : Hetzner CAX21/CAX31 (ARM, ~$7.59/mois) avec Docker.

### Configuration VPS pour OpenClaw

```bash
# 1. Ubuntu Server (recommande)
# 2. Securite (OBLIGATOIRE AVANT OpenClaw)

# UFW (firewall)
ufw allow ssh
ufw allow from VOTRE_IP
ufw enable

# Fail2Ban
apt install fail2ban
systemctl enable fail2ban

# SSH Hardening
# Editer /etc/ssh/sshd_config
PasswordAuthentication no
PermitRootLogin no
Port 22222    # Port non-standard

# Docker (pour isoler OpenClaw)
curl -fsSL https://get.docker.com | sh
```

### Securite OpenClaw

- **TOUJOURS whitelister ses IPs AVANT d'activer UFW/Fail2Ban**
- Ne JAMAIS ouvrir le dashboard OpenClaw en permanence
- Ne JAMAIS connecter 1Password sans reflexion prealable
- Utiliser Docker pour isoler l'agent

---

## 22. Anti-Patterns (18 regles)

Ces regles sont tirees directement de la formation Melvynx et des erreurs les plus courantes :

### 1. JAMAIS lancer Claude a la racine /

```bash
# MAUVAIS
cd /
claude

# BON
cd ~/mon-projet
claude
```

Claude indexe le dossier courant. A la racine = scan de tout le systeme.

### 2. JAMAIS surcharger CLAUDE.md

```markdown
# MAUVAIS — 2000 lignes de CLAUDE.md
(tout le detail du projet, l'historique, les conventions, les exemples...)

# BON — 200 lignes max, factoriser dans rules/
- Stack, conventions cles, commandes essentielles
- Le reste dans rules/ et memory/
```

### 3. JAMAIS installer plus de 3 MCP

```
Chaque MCP consomme du contexte meme sans etre utilise.
2-3 MCP = optimal. >3 = degradation de qualite.
```

### 4. JAMAIS bypass SANS deny-list

```json
// MAUVAIS — bypass nu
{ "defaultMode": "bypassPermissions" }

// BON — bypass + deny-list
{
  "defaultMode": "bypassPermissions",
  "deny": ["Bash(rm -rf *)", "Bash(sudo *)"]
}
```

### 5. JAMAIS features complexes sans workflow

```bash
# MAUVAIS — prompt direct pour une feature complexe
"Ajoute un systeme d'authentification complet avec OAuth, roles, permissions..."

# BON — utiliser un workflow structure
/apex --auto --test
"Ajoute un systeme d'authentification OAuth avec Google et GitHub"
```

### 6. JAMAIS dependre des plugins sans controler le code

```
Copier le code des plugins dans ses propres skills/agents.
Un plugin peut etre mis a jour et casser le workflow.
```

### 7. JAMAIS /batch pour petites features

```bash
# /batch est deprecie — utiliser main agent + sub-agents
```

### 8. JAMAIS taguer des fichiers avec Apex/OneShot

```bash
# MAUVAIS
/apex @src/app/page.tsx @src/components/Button.tsx

# BON — laisser l'exploration trouver les fichiers
/apex
"Modifie le bouton principal de la page d'accueil"
```

### 9. JAMAIS plus de 4 work trees simultanes

```
Au-dela de 4, la gestion devient trop complexe.
Les conflits de merge se multiplient.
```

### 10. JAMAIS empiler des demandes pendant que l'IA travaille

```
Attendre que Claude finisse une tache avant d'en demander une autre.
Empiler = contexte pollue = resultats degrades.
```

### 11. JAMAIS Chrome Headless pour debug

```
Melvynx considere Chrome Headless comme lourd et lent.
Preferer : screenshots manuels (CleanShotX) + logs console.
```

### 12. JAMAIS creer des skills manuellement

```bash
# MAUVAIS — creer SKILL.md a la main
mkdir ~/.claude/skills/mon-skill
vim ~/.claude/skills/mon-skill/SKILL.md

# BON — laisser Claude creer le skill
/skill-creator
"Cree un skill qui..."
```

### 13. JAMAIS creer des hooks manuellement dans le code

```
Configurer les hooks UNIQUEMENT via settings.json ou settings.local.json.
Ne pas creer de scripts de hooks ad hoc dans le code du projet.
```

### 14. JAMAIS relancer un skill pour une petite modification

```bash
# MAUVAIS — relancer /apex pour changer une couleur
/apex
"Change la couleur du bouton de bleu a vert"

# BON — langage naturel
"Change la couleur du bouton dans src/components/Button.tsx de bleu a vert"
```

### 15. JAMAIS dashboard OpenClaw ouvert en permanence

```
Le dashboard OpenClaw consomme des ressources.
L'ouvrir uniquement pour verifier/configurer.
```

### 16. JAMAIS 1Password dans OpenClaw sans reflexion

```
Connecter un gestionnaire de mots de passe a un agent IA
= risque de securite majeur. Reflechir aux implications.
```

### 17. JAMAIS modifier settings.json manuellement sans backup

```bash
# TOUJOURS
cp ~/.claude/settings.json ~/.claude/settings.json.backup
# PUIS modifier
```

### 18. JAMAIS oublier de whitelister ses IPs avant UFW/Fail2Ban

```bash
# TOUJOURS en premier
ufw allow from MON_IP
# PUIS activer
ufw enable
```

---

## 23. Niveaux de Configuration

### Echelle de configuration Claude Code

| Niveau | Nom | Description | Comment atteindre |
|--------|-----|-------------|-------------------|
| **0** | Aucune config | Claude Code brut, pas de CLAUDE.md | Installation de base |
| **OK** | Base | CLAUDE.md basique, pas de skills | `/init` + quelques regles |
| **OG** (90%) | Config gratuite Melvynx | Skills, agents, hooks, deny-list | `npx ig-blueprint-cli` |
| **OG++** (99%) | Config premium | Workflows avances, memory, MCP | Formation complete Melvynx |

### De 0 a OG en 20 minutes

```bash
# 1. Installer Claude Code
curl -fsSL https://claude.ai/install.sh | bash

# 2. Installer les dependances macOS
brew install trash-cli tmux bun

# 3. Installer la config Melvynx
npx ig-blueprint-cli

# 4. Generer le CLAUDE.md projet
cd ~/mon-projet
claude
# > /init

# 5. Activer le bypass avec deny-list
# Editer ~/.claude/settings.json (voir section 4)

# 6. Installer 2 MCP
claude mcp add context7 --url "https://mcp.context7.com/mcp" -s user
claude mcp add exa --url "https://mcp.exa.ai" --api-key "KEY" -s user

# DONE — Niveau OG atteint
```

### De OG a OG++

```bash
# 1. Rules modulaires dans ~/.claude/rules/
#    - Securite, JSON, Git, Tests, Python, TypeScript

# 2. Memory structuree dans ~/.claude/memory/
#    - Architecture, lecons, conventions, status

# 3. Sub-agents personnalises
#    - web-search, explore-codebase, doc-explorer, security-auditor

# 4. Skills avances
#    - ultrathink, brainstorm, debug, fix-errors

# 5. Hooks complets
#    - PreToolUse (command-validator)
#    - PostToolUse (prettier)
#    - Stop (Glass.aiff)
#    - Notification (Funk.aiff)

# 6. Knowledge graph (MCP Memory)
#    - Decisions, erreurs, preferences, architecture

# 7. Auto-learning
#    - autoMemoryEnabled: true
#    - Rules auto-apprentissage

# 8. Workflows maitrises
#    - /apex, /oneshot, /debug, /simplify, /ultrathink
#    - Flags : --auto, --test, --branch, --pr, --economy
```

---

## 24. Nouveautes 2026

### Claude Review (beta entreprise)

- Review automatique de Pull Requests par Claude
- Prix : 15-25$ par review
- Disponible pour les equipes entreprise
- Analyse : securite, performance, lisibilite, tests

### Voice Mode

```bash
# Activer le mode vocal
/voice

# Claude ecoute le micro et repond vocalement
# Utile pour le pair programming mains libres
```

### Ask User Question ameliore

Claude peut maintenant poser des questions plus precises et structurees quand il a besoin de clarification, avec des options de reponse suggerees.

### /simplify (3 agents paralleles)

```bash
/simplify

# 3 agents independants analysent le code :
# Agent 1 : lisibilite et nommage
# Agent 2 : complexite et performance
# Agent 3 : patterns et architecture
# Resultat fusionne en un seul rapport
```

### /schedule (cron persistant)

```bash
/schedule

# Contrairement a /loop (expire avec la session),
# /schedule cree une tache cron qui persiste :
# - macOS : via launchctl (LaunchAgent)
# - Survit au reboot
# - Logs dans /tmp/
```

### Auto-Memory

```json
{
  "autoMemoryEnabled": true
}
```

Claude memorise automatiquement les preferences, corrections et decisions entre les sessions. Stockage dans `~/.claude/projects/<hash>/memory/MEMORY.md`.

### CEMUX Terminal

Nouveau terminal specialise pour Claude Code avec :
- Interface optimisee pour les conversations IA
- Barre de contexte integree
- Gestion multi-sessions native
- Tabs verticaux + horizontaux
- Browser integre
- Notifications agent
- Scriptable (Swift/AppKit)

---

## 25. Troubleshooting macOS

### Claude Code ne trouve pas la commande `claude`

```bash
# Relancer le terminal ou
source ~/.zshrc
```

### Sons ne fonctionnent pas

- Les sons ne marchent PAS depuis un processus sandboxe (terminal Claude Code)
- Verifier que les hooks sont en `async: true`
- Tester directement dans Terminal.app : `afplay /System/Library/Sounds/Glass.aiff`
- Les sons fonctionnent via hooks et LaunchAgents (pas depuis le terminal Claude Code)

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
- Activer `toolSearchMode: auto` dans settings.json

### Commande `code` non trouvee

```bash
# Dans VS Code : Cmd+Shift+P > "Shell Command: Install code in PATH"
code --version  # Verification
```

### Hook PreToolUse bloque des commandes legitimes (faux positifs)

- Verifier que vous utilisez le command-validator Melvynx (analyse AST, pas regex)
- Ne pas empiler plusieurs hooks PreToolUse
- Si un script custom (ex: block-delete.sh) cause des faux positifs, le debrancher de PreToolUse et garder uniquement le command-validator

---

## 26. Astuces macOS

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

---

## Annexe A : Checklist de configuration macOS

```
[ ] Node.js v18+ installe
[ ] Claude Code installe (curl -fsSL https://claude.ai/install.sh | bash)
[ ] Homebrew installe
[ ] trash-cli installe (brew install trash-cli)
[ ] tmux installe (brew install tmux)
[ ] bun installe (brew install oven-sh/bun/bun)
[ ] Config Melvynx (npx ig-blueprint-cli)
[ ] settings.json configure (bypass + deny-list)
[ ] CLAUDE.md global cree (~/.claude/CLAUDE.md)
[ ] Rules de base creees (~/.claude/rules/)
[ ] MCP Context7 installe
[ ] MCP Exa installe (optionnel)
[ ] Hook Stop configure (Glass.aiff)
[ ] Hook PreToolUse configure (command-validator)
[ ] Hook PostToolUse configure (prettier)
[ ] Hook Notification configure (Funk.aiff)
[ ] Sub-agents crees (web-search, explore-codebase, doc-explorer)
[ ] autoMemoryEnabled active
[ ] toolSearchMode: auto active
[ ] CleanShotX installe (pour screenshots)
[ ] Raycast installe (pour productivite)
```

## Annexe B : Commandes de reference rapide

```bash
# Installation
curl -fsSL https://claude.ai/install.sh | bash
npx ig-blueprint-cli
brew install trash-cli tmux bun

# Quotidien
claude                          # Demarrer
/clear                          # Reset contexte
/context                        # Verifier utilisation
/usage                          # Limites du plan
/apex --auto                    # Feature complete
/oneshot                        # Fix rapide
/commit                         # Commit propre
/debug                          # Auto-reparer

# MCP
claude mcp add <nom> --url <url> -s user
claude mcp list
/mcp

# Gestion
/init                           # CLAUDE.md projet
/agent                          # Gerer agents
/plugin                         # Installer plugin
/model                          # Changer modele
/voice                          # Mode vocal

# macOS specifique
trash fichier.txt               # Au lieu de rm
open .                          # Ouvrir Finder
afplay /System/Library/Sounds/Glass.aiff  # Son
code .                          # Ouvrir VS Code
```

## Annexe C : Sons macOS pour les hooks

```bash
# Tester tous les sons disponibles
for f in /System/Library/Sounds/*.aiff; do
  echo "Son : $(basename $f)"
  afplay "$f"
  sleep 1
done

# Sons recommandes
afplay /System/Library/Sounds/Glass.aiff       # Fin de tache (Stop)
afplay /System/Library/Sounds/Funk.aiff        # Notification
afplay /System/Library/Sounds/Basso.aiff       # Erreur
afplay /System/Library/Sounds/Hero.aiff        # Succes important
afplay /System/Library/Sounds/Ping.aiff        # Alerte legere
```

---

> **Document compile par Soly Kim** — Base sur la methode Melvynx (Formation Complete + Setup 20min + Cours 4h)
> **Version** : 2.1 (fusionnee LOCAL + REMOTE)
> **Derniere mise a jour** : 2026-03-26
> **Repo de reference** : [KimGemRuby/claude-code-bible-melvynx](https://github.com/KimGemRuby/claude-code-bible-melvynx)
> Voir README.md pour la methode complete (workflows, prompting, context engineering, anti-patterns)
