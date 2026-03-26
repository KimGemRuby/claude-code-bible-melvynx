# Bible Melvynx — Claude Code Masterclass Complete

Reference complete extraite de la Masterclass 4h + Setup Terrence + Formation Complete de Melvynx (CodeLynx).
Fusionnee et structuree pour servir de reference absolue a Claude Code.

---

## Table des matieres

1. [Installation rapide](#installation-rapide)
2. [Architecture de securite (3 couches)](#architecture-de-securite)
3. [Configuration settings.json](#configuration-settingsjson)
4. [CLAUDE.md — 3 niveaux](#claudemd--3-niveaux)
5. [Rules](#rules)
6. [Workflows et commandes](#workflows-et-commandes)
7. [Prompting avance](#prompting-avance)
8. [Gestion du contexte](#gestion-du-contexte)
9. [MCP recommandes](#mcp-recommandes)
10. [Hooks](#hooks)
11. [Sub-agents et Teams](#sub-agents-et-teams)
12. [Skills](#skills)
13. [Anti-patterns](#anti-patterns)
14. [Raccourcis clavier](#raccourcis-clavier)
15. [Pricing](#pricing)
16. [Outils recommandes](#outils-recommandes)
17. [OpenClaw (agent distant)](#openclaw)

---

## Installation rapide

```bash
# Installer Claude Code
curl -fsSL https://claude.ai/install.sh | bash

# Installer la config Melvynx OG (gratuite)
npx ig-blueprint-cli

# Ou manuellement via mlv.sh/fc
```

Le CLI installe automatiquement :
- Backup de la config existante dans `backups/`
- settings.json (bypass + deny list)
- Hooks (son fin de tache, command validator, permission rules)
- Skills (/apex, /oneshot, /commit, /fix-errors, /fix-grammar, etc.)
- Status Line (necessite `bun`)

---

## Architecture de securite

| Couche | Mecanisme | Comportement |
|--------|-----------|-------------|
| 1. Bypass | `defaultMode: "bypassPermissions"` | Autonomie totale par defaut |
| 2. Deny list | Patterns dans `permissions.deny` | Override le bypass → bascule en Ask Mode → validation humaine |
| 3. CLAUDE.md | "jamais rm-rf, toujours trash" | Alternative comportementale sure |
| 4. Hook Regex | PreToolUse PowerShell | Filet de securite supplementaire |

**Le mecanisme exact :** en mode bypass, si l'IA tente une commande dans la deny list, le systeme bloque et renvoie : `error permission to use bash with command rm-rf`. L'utilisateur garde le dernier mot.

---

## Configuration settings.json

Fichier : `~/.claude/settings.json`

```json
{
  "$schema": "https://claude.ai/schemas/claude-code-settings.json",
  "defaultMode": "bypassPermissions",
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(del /f *)",
      "Bash(del /s /q *)",
      "Bash(sudo *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(diskpart*)",
      "Bash(format *)",
      "Bash(*Clear-Disk*)",
      "Bash(mkfs*)",
      "Bash(dd if=*)"
    ]
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"if ($env:TOOL_INPUT -match 'diskpart|format|rm -rf|del /f|del /s|Remove-Item.*-Recurse.*-Force|Clear-Disk|mkfs|dd if=') { Write-Host 'BLOCKED BY HOOK'; exit 1 }\""
          }
        ]
      }
    ]
  }
}
```

### settings.json vs settings.local.json
- `settings.json` : partage dans le repo (equipe)
- `settings.local.json` : local a la machine (hooks persos, configs locales)

---

## CLAUDE.md — 3 niveaux

| Niveau | Emplacement | Portee |
|--------|-------------|--------|
| Global | `~/.claude/CLAUDE.md` | TOUTES les conversations |
| Projet | `./CLAUDE.md` (racine) | Ce projet uniquement |
| Sous-dossier | `./src/components/CLAUDE.md` | Charge quand l'IA lit un fichier de ce dossier |

### Regles
- Chaque info doit etre "fondamentalement utile"
- Si >200 lignes : deplacer dans `.claude/rules/`
- `/init` genere le CLAUDE.md du projet (lancer dans un thread enrichi)
- Ne pas surcharger — charge dans CHAQUE conversation

---

## Rules

Fichiers `.md` dans `~/.claude/rules/`, charges automatiquement.

### Rules ciblees par chemin
```markdown
---
path: "*.json"
---
Quand tu lis des fichiers JSON, fais ceci...
```

### Continuous Learning
Quand l'IA fait la meme erreur 2+ fois :
1. "Arrete la tache en cours"
2. "Cree un fichier dans .claude/rules/ qui t'empechera de refaire cette erreur"
3. Utiliser `CRITICAL:` pour renforcer

---

## Workflows et commandes

### Pattern EPCT (fondamental)
**E**xplore > **P**lan > **C**ode > **T**est — JAMAIS coder sans explorer d'abord.

### Commandes principales

| Commande | Usage | Tokens |
|----------|-------|--------|
| `/apex -a` | Feature complexe, autonomie totale | Eleve |
| `/oneshot` | Quick fix, zero question | Faible |
| `/debug` | Fix erreurs (steps structurees) | Moyen |
| `/brainstorm` | Recherche profonde 4 rounds | Tres eleve |
| `/simplify` | Review code (3 agents paralleles) | Moyen |
| `/fix-errors` | Correction erreurs paralleles | Moyen |
| `/fix-grammar <dossier>` | Correction grammaire bulk | Moyen |
| `/load-memory` | Optimise tokens memoire | Faible |
| `/commit` | Commit simplifie | Faible |
| `/ultrathink` | Max thinking tokens (2+ min) | Tres eleve |

### Apex flags
`--auto` (`-a`), `--examine` (`-e`), `--test` (`-t`), `--economy` (`-$`), `--branch` (`-b`), `--pr` (`-p`), `--interactive` (`-i`), `--teams` (`-m`), `--resume` (`-r`), `--plan`

### Petites modifications
Langage naturel direct, sans workflow. Economise le contexte.

---

## Prompting avance

### Greenfield (nouveau projet)
- Preciser la stack
- Decrire la feature en langage naturel
- Separer desktop et mobile

### Brownfield (projet existant)
- Screenshots annotes
- Referencer les styles existants
- L'IA suit les conventions du code

### 10 variations UI
"Cree un fichier /demo/page.tsx avec 10 variations UI/UX, 10 composants separes dans 10 fichiers."

### Debug (methode logs = la plus efficace)
1. Ajouter console.log
2. Reproduire le bug
3. Copier les logs bruts
4. Envoyer a Claude

### Commentaires comme prompt injection
Documenter les helpers dans les commentaires du code — l'IA les absorbe via la regle "lire 3 fichiers similaires".

---

## Gestion du contexte

- **200K tokens max** par conversation
- **~27K tokens** au demarrage
- **Autocompact a ~96-99%** = degrade la qualite

### Regles
- `/clear` OBLIGATOIRE entre taches majeures
- `/context` pour surveiller
- Sub-agents pour les recherches (preserve le contexte)
- Skills multi-fichiers = token efficient

### Lost in the Middle
Les transformers ignorent les tokens du milieu. Solution : **prompt discovery** (multi-step).

---

## MCP recommandes

Melvynx n'utilise que 2 MCP :

| MCP | Prix | Usage |
|-----|------|-------|
| **Context7** | Gratuit | Documentation technique a jour |
| **Exa.ai** | 20$ credit gratuit | Recherche web optimisee IA |

JAMAIS plus de 3 MCP simultanement.

---

## Hooks

| Hook | Usage |
|------|-------|
| Stop | Son fin de tache (notification) |
| Notification | Son "need human" (different du Stop) |
| PreToolUse | Command Validator (Regex securite) |
| PostToolUse | Prettier auto sur Edit/Write |

Ne JAMAIS creer manuellement — demander a Claude.

---

## Sub-agents et Teams

### Sub-agents recommandes
| Agent | Outils |
|-------|--------|
| web-search | WebSearch, Exa |
| explore-codebase | Read, Grep |
| doc-explorer | Context7 |

### Teams
- `enableAgentTeams: true` dans settings.json
- Uniquement pour zones SEPAREES (front, back, mobile)
- Max 4 worktrees simultanes

---

## Skills

- Entry point : `SKILL.md`
- Petit skill : un fichier
- Grand skill : `SKILL.md` + `references/` + `scripts/`
- Installer : `npx skill add auteur/nom`
- JAMAIS creer manuellement

---

## Anti-patterns

1. JAMAIS lancer Claude a la racine du systeme
2. JAMAIS surcharger CLAUDE.md
3. JAMAIS >3 MCP
4. JAMAIS bypass SANS deny list
5. JAMAIS features complexes sans workflow
6. JAMAIS dependre des plugins — copier dans ses skills
7. JAMAIS /batch pour petites features
8. JAMAIS taguer fichiers avec Apex/OneShot
9. JAMAIS >4 worktrees simultanes

---

## Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `@fichier` | Taguer fichier |
| `!commande` | Bash direct |
| `Shift+Tab` | Changer mode (normal/accept/bypass) |
| `Ctrl+T` | Taches en cours |
| `Ctrl+O` | Transcript verbose |
| `Ctrl+S` | Sauvegarder prompt en cours |
| `Escape x2` | Clear > historique |
| `Meta+P` | Changer modele |
| `/clear` | Reset contexte |
| `/context` | Usage tokens % |

---

## Pricing

| Plan | Prix/mois | Equiv. API |
|------|-----------|------------|
| Pro | 20$ | ~160$ |
| Max 5x | 100$ | ~800$ |
| Max 20x | 200$ | ~3200$ (meilleur ratio) |

---

## Outils recommandes

- **Terminal** : Ghosty ou tmux
- **IDE** : VS Code
- **Screenshots** : CleanShotX (Mac)
- **Proxy/Debug** : Proxyman (Mac)
- **Boilerplate** : nowts.app (NaotS)
- **Deploiement rapide** : Netlify Drop (drag & drop du dossier dist/)

---

## OpenClaw

Agent distant controlable depuis Telegram. Lance des agents Claude Code en arriere-plan sur VPS.

### Installation
```bash
# Local
curl maltbot.sh

# VPS Docker
npx openclaw-vps-setup
```

### VPS recommande
Hetzner CAX21/CAX31 ARM ~$7.59/mois

### Securite VPS
- UFW + Fail2Ban (whitelister IPs AVANT)
- Desactiver password SSH
- Docker pour sandboxing
- `SANDBOX=1 claude` pour bypass en sandbox

---

## Niveaux de configuration

| Niveau | Description |
|--------|------------|
| 0 | Aucune config |
| OK | Claude Code de base |
| OG (90%) | Config gratuite : `npx ig-blueprint-cli` |
| OG++ (99%) | Config premium (review securite, clean code) |

> "90% OG avec la config free, 99% OG avec la config premium, 100% impossible car il n'y a que moi" — Melvynx

---

## Credits

Methode basee sur la Masterclass de [Melvynx (CodeLynx)](https://codelynx.dev).
Transcriptions originales dans le dossier `source/`.
