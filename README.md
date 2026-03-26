# Bible Melvynx — Claude Code Masterclass Complete

Reference complete extraite de la Masterclass 4h + Setup Terrence + Formation Complete de Melvynx (CodeLynx).
Fusionnee et structuree pour servir de reference absolue a Claude Code.

> **V2.0** (2026-03-26) — Mise a jour majeure : +56% de contenu (527 → 820 lignes). Voir [CHANGELOG.md](CHANGELOG.md) et [BIBLE_MELVYNX_V2.md](BIBLE_MELVYNX_V2.md).

---

## Fichiers du repo

| Fichier | Description | Lignes |
|---------|-------------|--------|
| **[BIBLE_MELVYNX_V2.md](BIBLE_MELVYNX_V2.md)** | Bible V2 complete — 14 chapitres + anti-patterns | 820 |
| [README.md](README.md) | Bible V1 — 30 sections (ce fichier) | ~1200 |
| [BIBLE_CLAUDE_CODE_DEVOPS_SYSADMIN.md](BIBLE_CLAUDE_CODE_DEVOPS_SYSADMIN.md) | Bible etendue DevOps/SysAdmin — 40 chapitres | 99 Ko |
| [BIBLE_MACOS.md](BIBLE_MACOS.md) | Bible specifique macOS | 400+ |
| [BIBLE_WINDOWS.md](BIBLE_WINDOWS.md) | Bible specifique Windows | 400+ |
| [BIBLE_LINUX.md](BIBLE_LINUX.md) | Bible specifique Linux/VPS | 400+ |
| [CHANGELOG.md](CHANGELOG.md) | Historique des modifications | - |
| [settings-bokador-current.json](settings-bokador-current.json) | Config Claude Code BOKADOR (43 deny, 20 agents) | - |

---

## Table des matieres (V1)

1. [Fondamentaux du terminal](#fondamentaux-du-terminal)
2. [Architecture Claude Code](#architecture-claude-code)
3. [Installation rapide](#installation-rapide)
4. [Architecture de securite (4 couches)](#architecture-de-securite)
5. [Configuration settings.json](#configuration-settingsjson)
6. [CLAUDE.md — 3 niveaux](#claudemd--3-niveaux)
7. [Rules](#rules)
8. [Auto-Memory](#auto-memory)
9. [Structure du dossier .claude/](#structure-du-dossier-claude)
10. [Workflows et commandes](#workflows-et-commandes)
11. [Commandes slash natives](#commandes-slash-natives)
12. [Prompting avance](#prompting-avance)
13. [Meta-prompting](#meta-prompting)
14. [Gestion du contexte](#gestion-du-contexte)
15. [MCP recommandes](#mcp-recommandes)
16. [Hooks](#hooks)
17. [Sub-agents et Teams](#sub-agents-et-teams)
18. [Work Trees](#work-trees)
19. [Skills](#skills)
20. [Plugins](#plugins)
21. [Status Line](#status-line)
22. [Anti-patterns](#anti-patterns)
23. [Raccourcis clavier](#raccourcis-clavier)
24. [Pricing](#pricing)
25. [Nouveautes 2026](#nouveautes-2026)
26. [Outils recommandes](#outils-recommandes)
27. [tmux](#tmux)
28. [Dossier CC](#dossier-cc)
29. [OpenClaw (agent distant)](#openclaw)
30. [Niveaux de configuration](#niveaux-de-configuration)

---

## Bibles par OS

| Bible | Fichier | Contenu |
|-------|---------|---------|
| **macOS** | [BIBLE_MACOS.md](BIBLE_MACOS.md) | Installation, Homebrew, CEMUX/Ghostty, afplay hooks, Raycast, CleanShotX, Proxyman, tmux, LaunchAgents, Mac Mini OpenClaw |
| **Windows** | [BIBLE_WINDOWS.md](BIBLE_WINDOWS.md) | WSL, PowerShell hooks, Windows Terminal, sons .wav, deny-list diskpart/del, montage disques WSL, Claude Desktop config |
| **Linux** | [BIBLE_LINUX.md](BIBLE_LINUX.md) | Installation distros, paplay/aplay hooks, tmux VPS, Docker sandboxing, OpenClaw VPS, securite (UFW, Fail2Ban, SSH), systemd, ntfy.sh |
| **DevOps/SysAdmin** | [BIBLE_CLAUDE_CODE_DEVOPS_SYSADMIN.md](BIBLE_CLAUDE_CODE_DEVOPS_SYSADMIN.md) | Bible etendue 40 chapitres — IaC, CI/CD, monitoring, conteneurs, backup, runbooks, infra kim13 |

Ce README contient la **methode universelle** (workflows, prompting, context engineering, anti-patterns). Les Bibles par OS contiennent les **specificites d'installation et configuration** pour chaque plateforme.

---

## Fondamentaux du terminal

### Commandes de base
```bash
echo "hello"       # Afficher du texte
ls                 # Lister les fichiers
pwd                # Afficher le chemin courant
cd dossier         # Entrer dans un dossier
cd ..              # Remonter d'un niveau
cd                 # Retourner au home (~)
mkdir mon-dossier  # Creer un dossier
open .             # Ouvrir le Finder (macOS)
code .             # Ouvrir VS Code
clear              # Vider le terminal
```

Le caractere `~` = dossier utilisateur (home).

### Bash vs PowerShell vs WSL
- **macOS/Linux** : Bash ou Zsh natif
- **Windows** : installer WSL pour avoir Bash, ou utiliser PowerShell

### Astuce copie de chemin macOS
Dans le Finder, clic droit sur un dossier > maintenir **Option** > "Copier comme chemin d'acces"

### Installation VS Code
1. Telecharger sur code.visualstudio.com
2. `Cmd+Shift+P` > "Shell Command: Install code in PATH"
3. Terminal integre : menu Terminal > New Terminal

### Terminaux recommandes

| Terminal | Description |
|----------|-------------|
| **CEMUX** | Terminal cree pour les coding agents. 4.7 stars GitHub. Build au-dessus de Ghostty. Tabs verticaux + horizontaux, notifications agent, browser integre, split pane, scriptable, Swift/AppKit (pas Electron), gratuit |
| **Ghostty** | Terminal rapide. `Cmd+1/2/3` pour tabs, `Cmd+W` fermer, `Cmd+T` nouveau tab |
| **Hyper** (hyterm) | Alternative populaire |
| **Terminal natif** | macOS Terminal.app ou iTerm2 |

### Premier projet demo
```bash
npm create vite@latest     # Creer un projet (choisir React, TypeScript)
cd mon-premier-projet
npm install
npm run dev                # localhost:5173
```

---

## Architecture Claude Code

### Les 3 produits Anthropic

| Produit | Type | Fonctionnement |
|---------|------|---------------|
| **Claude Chat** | Interface web | One-shot (question/reponse) |
| **Claude API** | Acces direct au modele | One-shot par defaut |
| **Claude Code** | Agent terminal | Boucle agentique autonome |

**Analogie du chef cuisinier** : Claude Chat = le chef te dit la recette. Claude Code = le chef cuisine dans ta cuisine.

### Outils natifs disponibles
Bash, Edit, Multi Edit, Read, Write, List, Web Fetch, Web Search, ToDo + MCP

### Fonctionnement agentique (boucle)
Instruction → Outils → Action → Verification → Repetition

Exemple concret — "supprime les commentaires" :
1. Modele appelle Read File
2. Claude Code retourne le contenu
3. Modele appelle Edit File avec le contenu modifie
4. Claude Code ecrit le fichier
5. Modele confirme la fin

### Principe EPCT (workflow de base)
1. **E**xplore : lancer des sub-agents (explore-codebase, explore-doc, web-search)
2. **P**lan : entrer en plan mode, planifier les modifications
3. **C**ode : executer le plan
4. **T**est : verifier lint, tests, que tout fonctionne

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
- Deux sons distincts (fin de tache + need human)

---

## Architecture de securite

| Couche | Mecanisme | Comportement |
|--------|-----------|-------------|
| 1. Bypass | `defaultMode: "bypassPermissions"` | Autonomie totale par defaut |
| 2. Deny list | Patterns dans `permissions.deny` | Override le bypass → bascule en Ask Mode → validation humaine |
| 3. CLAUDE.md | "jamais rm-rf, toujours trash" | Alternative comportementale sure |
| 4. Hook Regex | PreToolUse command-validator | Filet de securite supplementaire (AST-level, pas de faux positifs) |

**Le mecanisme exact :** en mode bypass, si l'IA tente une commande dans la deny list, le systeme bloque et renvoie : `error permission to use bash with command rm-rf`. L'utilisateur garde le dernier mot.

### Double securite recommandee
1. Deny-list dans settings.json (bloque au niveau systeme)
2. "Utilise trash au lieu de rm" dans CLAUDE.md (bloque au niveau comportemental)

---

## Configuration settings.json

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
            "command": "bun command-validator/src/cli.ts"
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
    ]
  }
}
```

### settings.json vs settings.local.json
- `settings.json` : partage dans le repo (equipe). Commite dans Git.
- `settings.local.json` : local a la machine (hooks persos, configs locales). Non partage.

### Options importantes
- `skipDangerousModePermissionPrompt` : supprime le prompt de confirmation du mode bypass
- `toolSearchMode: "auto"` : active Tool Search automatiquement si >10% du contexte en MCP

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
- `/init` genere le CLAUDE.md du projet
- **Astuce** : lancer `/init` dans un thread enrichi (ou des recherches ont deja ete faites) pour un meilleur resultat
- Ne pas surcharger — charge dans CHAQUE conversation
- Utiliser `CRITICAL:` pour les regles les plus importantes
- **Timing conventions techniques** : mettre les preferences (camelCase, structure fichiers) dans CLAUDE.md APRES le premier run — pas avant, pour ne pas brider l'IA au demarrage

### Commande utile
"Synchronise les regles avec ce qu'on a actuellement dans le projet" — force Claude a relire le projet et mettre a jour CLAUDE.md.

---

## Rules

Fichiers `.md` dans `~/.claude/rules/`, charges automatiquement.

### Rules ciblees par chemin (globs)
```markdown
---
globs: ["*.json"]
---
Quand tu lis des fichiers JSON, fais ceci...
```
Chargees uniquement quand l'IA touche un fichier correspondant au pattern.

### Continuous Learning
Quand l'IA fait la meme erreur 2+ fois :
1. "Arrete la tache en cours"
2. "Cree un fichier dans .claude/rules/ qui t'empechera de refaire cette erreur"
3. Utiliser `CRITICAL:` pour renforcer

### Bonnes pratiques rules
- Avant de modifier un fichier, lire au moins 3 fichiers similaires
- Apres tout changement, update le changelog avec la date du jour
- Ne pas empiler les demandes pendant que l'IA travaille (attendre la fin)

---

## Auto-Memory

### Configuration
- `autoMemoryEnabled: true` (actif par defaut dans settings.json)
- Claude s'ecrit des notes entre les sessions
- Stocke dans `~/.claude/projects/<hash>/memory/`
- 200 premieres lignes de `MEMORY.md` chargees a chaque session

### CLAUDE.md vs Auto-Memory

| | CLAUDE.md | Auto-Memory |
|---|---|---|
| Qui ecrit | L'humain | Claude |
| Contenu | Instructions, regles | Patterns, apprentissages |
| Scope | Projet ou global | Par working directory |
| Controle | Total | A verifier regulierement |

**Conseil Melvynx** : preferer dire "modifie le CLAUDE.md avec cette info" plutot que laisser l'auto-memory sauvegarder n'importe quoi.

---

## Structure du dossier .claude/

```
~/.claude/
├── CLAUDE.md          # Memoire globale
├── settings.json      # Configuration partagee
├── settings.local.json # Configuration locale
├── rules/             # Regles persistantes (.md)
├── skills/            # Skills (SKILL.md entry point)
│   └── disabled/      # Skills desactives (deplacer ici pour desactiver)
├── agents/            # Agents custom
│   └── disabled/      # Agents desactives
├── commands/          # Ancien nom des skills (legacy)
├── scripts/           # Scripts d'automatisation (command-validator, statusline)
├── hooks/             # Scripts de hooks
├── projects/          # Historique de toutes les sessions (transcripts recuperables)
├── plans/             # Plans generes en plan mode
├── tasks/             # Taches executees
└── teams/             # Equipes creees (JSON)
```

**Astuce** : lancer Claude Code dans `~/.claude/` pour gerer/modifier sa propre config.

**Desactiver skills/agents** : deplacer dans un sous-dossier `disabled/`.

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
| `/fix-errors` | Correction erreurs paralleles (jusqu'a 23 sub-agents) | Moyen |
| `/fix-grammar <dossier>` | Correction grammaire bulk (sub-agents par groupe) | Moyen |
| `/load-memory` | Optimise tokens memoire | Faible |
| `/commit` | Commit simplifie | Faible |
| `/create-pr` | Pull Request automatique | Faible |
| `/merge` | Merge intelligent | Faible |
| `/fix-pr-comments` | Corriger les commentaires PR | Moyen |
| `/ultrathink` | Max thinking tokens (2+ min) | Tres eleve |
| `/review-code` | Review multi-perspectives (pending, best practices, securite) | Moyen |
| `/clean-code` | Applique les principes clean code | Moyen |
| `/prompt-creator` | Cree des prompts optimises | Faible |
| `/skill-creator` | Cree des skills personnalises | Faible |
| `/subagent-creator` | Cree des sub-agents personnalises | Faible |

### Apex — Workflow principal (>99% succes)

LA commande principale de Melvynx. Phases :
1. Initialisation : lit fichier init, determine complexite
2. Exploration : lance sub-agents explore-codebase (1 a 3+ selon complexite)
3. Relecture contexte : confirme
4. Planification : cree un plan d'execution
5. Execution : implemente le plan
6. Validation : valide sa propre tache
7. (Premium OG++) Review securite + clean code + coherence

| Flag | Raccourci | Effet |
|------|-----------|-------|
| `--auto` | `-a` | Mode automatique, aucune question |
| `--examine` | `-e` | Active la review |
| `--test` | `-t` | Lance les tests |
| `--economy` | `-$` | Evite les sub-agents (economise tokens) |
| `--branch` | `-b` | Cree une branche git |
| `--pr` | `-p` | Cree une Pull Request |
| `--interactive` | `-i` | Mode interactif (menu flags) |
| `--teams` | `-m` | Mode equipe multi-agents |
| `--resume` | `-r` | Reprendre une tache interrompue |
| `--plan` | | Plan mode (sinon plan inline) |
| `--save` | | Save mode |

**Conseil cle** : ne PAS taguer de fichiers (@fichier) — laisser Apex explorer seul.

### /debug — Fix erreurs structurees

Workflow en steps (prompt discovery) :
1. Init : initialiser parametres
2. Analyse : analyser l'erreur
3. Solutions : proposer plusieurs solutions
4. Fix : appliquer la solution
5. Verify : verifier

Techniques de debug (par efficacite) :
1. **Log technique** (LE PLUS EFFICACE) : ajouter console.log → reproduire → copier logs bruts → envoyer a Claude
2. **Screenshot annote** : CleanShotX pour montrer le bug visuellement
3. **Ne pas dire quoi faire** : exposer le PROBLEME, l'IA resout

### /brainstorm — Recherche profonde (4 rounds)

1. **Expansive exploration** : recherche large en profondeur (sub-agents)
2. **Devil's advocate** : challenge toutes les idees
3. **Synthese** : condense en 5 perspectives differentes
4. **Cristallisation** : recommandations actionnables

**TRES gourmand en tokens.** Reserver aux decisions architecturales importantes.

### /simplify — Review automatique

Lance 3 agents de review en parallele. Review le code modifie pour reuse, qualite, efficacite. Corrige les problemes trouves automatiquement.

### /loop — Tache recurrente

Cree en interne un cron job (outil `cron_create`). Auto-expire quand la session se ferme.
```
/loop 2m check if PR tests pass, if no, fix it
```

### /schedule — Taches planifiees persistantes

Cron persistant (contrairement a /loop qui expire avec la session). Continue meme apres fermeture.
```
/schedule tous les matins a 9h, clean le dossier Downloads
```

**Difference /loop vs /schedule** : /loop = temporaire (session), /schedule = persistant (cron).

### /batch — ANTI-PATTERN selon Melvynx

Lance agents en parallele avec work trees separees, chaque agent cree sa PR. Melvynx est CONTRE : genere des dizaines de PR. Preferer un main agent orchestrateur.

### Petites modifications

Langage naturel direct, sans workflow ni skill. Economise le contexte.

---

## Commandes slash natives

| Commande | Action |
|----------|--------|
| `/clear` | Reset contexte (OBLIGATOIRE entre taches majeures) |
| `/init` | Generer CLAUDE.md du projet |
| `/context` | Utilisation contexte en % |
| `/usage` | Limites et consommation |
| `/model` | Changer modele |
| `/agent` | Creer/gerer agents |
| `/mcp` | Voir MCP connectes |
| `/plugin` | Installer/gerer plugins |
| `/stats` | Statistiques de la session |
| `/config` | Configuration interactive (tips, suggestions, progress bar) |
| `/voice` | Mode vocal (beta, mieux en anglais) |
| `/loop` | Tache recurrente a intervalle |
| `/schedule` | Taches planifiees persistantes |
| `/simplify` | Review et simplification |
| `/copy` | Copier derniere reponse en Markdown |

---

## Prompting avance

### Greenfield (nouveau projet)
1. Preciser la stack : "create a new vite project with shadcn and tailwind"
2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier les conventions techniques (camelCase, etc.) au premier run — laisser l'IA choisir
5. Utiliser une boilerplate existante pour imposer les conventions (ex: NaotS/nowts.app)

### Brownfield (projet existant)
1. Screenshots annotes (CleanShotX) — montrer exactement le probleme
2. Donner des references internes : "style comme illustration-picker"
3. Donner les ressources : chemins, images, URLs
4. Les screenshots permettent a l'IA de VOIR — ameliore drastiquement le front-end

### 10 variations UI
"Cree un fichier /demo/page.tsx avec 10 variations UI/UX, 10 composants separes dans 10 fichiers."
Comparer, choisir la meilleure, supprimer le reste.

### Debug (methode logs = la plus efficace)
1. Ajouter console.log strategiques
2. Reproduire le bug soi-meme
3. Copier les logs bruts de la console
4. Envoyer a Claude — ne PAS dire quoi faire, exposer le PROBLEME

### Processus iteratif front-end
Screenshot → demander modif → re-screenshot → ajustement → etc.

### Commentaires comme prompt injection
Documenter les helpers internes avec exemples d'utilisation dans les commentaires du code. Avec la regle "lire 3 fichiers similaires", l'IA absorbe ces instructions automatiquement. Exemples : ZodRoute, Upfetch.

### Regles de prompting
- Ne PAS relancer un skill pour une petite modif — langage naturel
- Ne PAS empiler les demandes pendant que l'IA travaille (attendre la fin)
- Attention aux mauvais chats avec plusieurs terminaux ouverts
- Ne PAS utiliser Chrome Headless (juge lourd et lent par Melvynx)

---

## Meta-prompting

Concept : creer des prompts qui creent des prompts.

### Meta-skills disponibles

| Skill | Usage |
|-------|-------|
| `/prompt-creator` | Cree des prompts optimises (best practices Anthropic + OpenAI + Google) |
| `/cloud-memory` (ou `/load-memory`) | Gestion optimisee du CLAUDE.md et rules |
| `/skill-creator` | Cree des skills personnalises (necessite Python) |
| `/subagent-creator` | Cree des sub-agents personnalises |

### Comment creer un meta-prompt
"Utilise le skill creator pour creer un nouveau skill qui s'appelle meta prompt creator. Je veux que tu recherches sur internet les meilleures techniques de prompt proposees par Anthropic, OpenAI, Google et que tu crees un prompt super puissant pour creer des prompts dans mes applications."

---

## Gestion du contexte

### Contexte en chiffres
- **200 000 tokens max** par conversation
- **~27 000 tokens utilises au demarrage** (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- **Autocompact a ~96-99%** du contexte (degrade la qualite, prend du temps)

### Regles
- `/clear` OBLIGATOIRE entre taches majeures
- `/context` regulierement pendant les longues sessions
- Si contexte > 80% : `/clear` immediat
- Langage naturel pour petites modifs (economise le contexte)
- Sub-agents pour les recherches (preservent le contexte principal)
- Maximum 2-3 MCP actifs
- Skills multi-fichiers (prompt discovery) = token efficient

### Lost in the Middle
Les transformers donnent plus d'importance au DEBUT et a la FIN du contexte. Les tokens au milieu sont structurellement ignores. Quand le contexte grandit, les instructions initiales se retrouvent "au milieu".

**Solution : Prompt Discovery (multi-step)**
```
steps/
  step1-init.md
  step2-analyse.md
  step3-plan.md
  step4-execute.md
  step5-verify.md
```
Chaque etape lit son fichier juste avant d'agir — le prompt courant est toujours "recent".

### Debug du contexte avec Proxyman
- **Proxyman** : outil macOS pour intercepter les requetes HTTP de Claude Code
- Permet de voir exactement ce qui est envoye au modele
- System prompt (~3 500 tokens), descriptions outils (tres volumineux), skills, MCP tools, CLAUDE.md, rules
- Utile pour comprendre pourquoi les ~27K tokens sont consommes au demarrage

### Tool Search
Si >10% du contexte est utilise par les MCP : Tool Search s'active automatiquement (`toolSearchMode: "auto"` par defaut). Les descriptions MCP ne sont chargees qu'a la demande.

---

## MCP recommandes

Melvynx n'utilise que 2 MCP :

| MCP | Prix | Usage |
|-----|------|-------|
| **Context7** | Gratuit (API key GitHub) | Documentation technique a jour |
| **Exa.ai** | 20$ credit gratuit (~6$/5mois) | Recherche web optimisee IA |

JAMAIS plus de 3 MCP simultanement (degrade la qualite des outputs).

Installation globale :
```bash
claude mcp add --scope user context7
claude mcp add --scope user exa
```

---

## Hooks

### 4 hooks recommandes (Melvynx standard)

| Hook | Declencheur | Commande | Usage |
|------|------------|---------|-------|
| **PreToolUse** | Avant execution outil | `bun command-validator/src/cli.ts` | Valide les commandes, bloque les dangereuses (AST-level, pas de faux positifs) |
| **Stop** | Fin de tache | `afplay Glass.aiff` | Son pour savoir quand c'est fini |
| **Notification** | Claude a besoin d'attention | Son different du Stop | Distinguer "fini" vs "question" |
| **PostToolUse** (local) | Apres Edit/Write | Prettier automatique | Formater les fichiers edites |

### Bonnes pratiques
- **Hooks async** : `async: true` pour les actions non-bloquantes (sons, notifications)
- Ne pas surcharger de hooks (ralentit l'execution)
- **UN SEUL** hook PreToolUse recommande : command-validator (pas de faux positifs contrairement a grep)
- Ne JAMAIS creer les hooks manuellement — demander a Claude : "cree un hook qui..."

---

## Sub-agents et Teams

### Sub-agents — Protection du contexte
- Une recherche web = ~28K tokens dans le contexte principal
- Avec sub-agent : 0 tokens dans le principal, ~1500 mots retournes
- Le sub-agent travaille dans SON propre contexte (120K tokens)
- La description du sub-agent determine QUAND l'IA l'utilise automatiquement

### Types de sub-agents

| Type | Usage | Vitesse |
|------|-------|---------|
| **Explore** | Recherche, exploration | Rapide |
| **Task** | Actions generiques | Normal |

### 3 sub-agents recommandes

| Agent | Outils | Usage |
|-------|--------|-------|
| **web-search** | WebSearch, WebFetch, Exa MCP | Recherche web |
| **explore-codebase** | Read, Grep, Search | Exploration code |
| **doc-explorer** | Context7 MCP | Documentation |

### Creation d'agents custom
`/agent` > Create New Agent > Personal Folder > Generate with Cloud
- Peut assigner un **modele** specifique (ex: Sonnet pour economiser)
- Peut assigner une **couleur**
- Description = ce qui determine quand l'agent est utilise automatiquement

**Attention** : l'IA n'utilise pas toujours les agents automatiquement. C'est pourquoi il faut les integrer dans des workflows (skills).

### Teams (Agent Teams)
- Activer : `enableAgentTeams: true` dans settings.json
- Le main agent (team lead) cree des equipes, assigne des taches via task list partagee
- Les teammates travaillent en parallele
- Interface : fleche bas pour switcher entre agents
- **QUAND utiliser** : zones de modification SEPAREES (monorepo back/front/mobile)
- **NE PAS utiliser** : memes fichiers — les agents se "battent"
- Si les agents se marchent dessus, leur dire "occupe-toi que de ta feature"

---

## Work Trees

### Principe
Travailler sur des branches separees en parallele, chaque branche dans son propre dossier.

### Workflow complet
1. Creer le work tree
2. Coder la feature
3. Creer la PR
4. Merger
5. Detruire le work tree

### Regles
- **Maximum 4 work trees simultanes**
- Reserver aux gros refactors, migrations, changements structurels
- NE PAS utiliser pour petites features (travailler directement dans le meme repo)
- Attention au merge si les work trees touchent des schemas differents

### Astuce CLAUDE.md pour Work Trees
"Si il y a des erreurs TypeScript que tu n'as pas directement implementees, c'est pas ton probleme, passe a la suite" — evite que les agents perdent du temps sur des erreurs qui ne sont pas les leurs.

---

## Skills

### Structure
- **Petit skill** : un seul fichier `SKILL.md`
- **Grand skill** : dossier avec `SKILL.md` (entry point) + `references/` + `scripts/`
- Seul l'index est charge, les sous-fichiers lus a la demande (token-efficient)

### Installation de skills externes
```bash
npx skill add <auteur>/<nom>
```
Source : **skill.sh** (marketplace). Choisir "global" pour reutilisation cross-projets.

### skills vs commandes
Commandes = ancien nom (legacy). Skills = nouveau nom. Meme chose.

### Bonnes pratiques
- JAMAIS creer les skills manuellement — utiliser `/skill-creator`
- Le Skill Creator necessite **Python** pour initialiser la structure
- Deplacer dans `disabled/` pour desactiver temporairement

---

## Plugins

### Installation
`/plugin` dans Claude Code. Deux sources :
1. "Anthropic Cloud Code"
2. "Anthropic Cloud Official Plugins"

### Probleme des plugins
- Perte de controle : le code est dans un dossier non modifiable
- Ecrase a chaque mise a jour automatique (vos modifs sont perdues)
- Aucune visibilite sur ce que le plugin fait exactement

### Recommandation forte de Melvynx
> "Ces plugins vous permettent d'integrer des outils mais vous perdez le controle. Je vous invite vraiment a copier ces fichiers et les installer dans votre dossier skills."

**Philosophie** : voler les concepts des plugins, creer votre propre configuration maitrisee.

---

## Status Line

Barre d'information en bas du terminal, toujours visible.

### Informations affichees
- Dossier courant et repo git
- Modele utilise
- Cout de la session en $
- Tokens utilises et % contexte
- Graphique session et weekly limit
- Heures restantes

### Configuration
- Necessite **bun** installe
- Installe automatiquement par `npx ig-blueprint-cli`
- Si ne marche pas : `/debug` pour auto-reparer
- Commande : "affiche une status line avec le chemin et projet dans lequel je suis actuellement"

---

## Anti-patterns

1. JAMAIS lancer Claude Code a la racine du systeme
2. JAMAIS surcharger CLAUDE.md (max ~200-500 lignes) — deleguer dans rules/
3. JAMAIS installer >3 MCP
4. JAMAIS bypass SANS deny-list configuree
5. JAMAIS features complexes sans workflow (l'IA saute des etapes)
6. JAMAIS dependre des plugins sans controler le code — copier dans ses skills
7. JAMAIS utiliser /batch (preferer main agent + sub-agents)
8. JAMAIS empiler les demandes pendant que l'IA travaille
9. JAMAIS taguer les fichiers (@fichier) avec Apex/OneShot — laisser l'exploration les trouver
10. JAMAIS utiliser Teams sur une seule zone de code (les agents se battent)
11. JAMAIS laisser l'auto-memory sauvegarder n'importe quoi (preferer CLAUDE.md explicite)
12. JAMAIS specifier la technique au premier run Greenfield
13. JAMAIS lancer plus de 4 terminaux/work trees simultanes
14. JAMAIS utiliser work trees pour de petites features
15. JAMAIS laisser les plugins controler votre config
16. JAMAIS relancer des skills pour petites modifs (langage naturel)
17. JAMAIS creer skills/hooks/agents manuellement — demander a Claude
18. JAMAIS modifier settings.json manuellement — demander a Claude
19. JAMAIS repeter une commande qui echoue sans analyser l'erreur
20. Si Claude fait une erreur 2+ fois : creer une regle dans .claude/rules/

---

## Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `@fichier` | Taguer fichier (ex: @header → src/components/header) |
| `!commande` | Bash direct (!pnpm install) |
| `/` | Commandes slash |
| `Shift+Tab` | Changer mode (normal / accept edit / bypass) |
| `Ctrl+T` | Taches en cours |
| `Ctrl+O` | Transcript verbose |
| `Ctrl+S` | Sauvegarder prompt (question intermediaire) |
| `Meta+P` | Changer modele |
| `Escape x1` | Efface le texte courant |
| `Escape x2` | Acces historique conversations |
| `N` (sur Ask User Question) | Ajouter une note avant de repondre |

---

## Pricing

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| **Max 20x** | **200$** | **~3 200$** | **x16 (meilleur ratio)** |

### Systeme de sessions et limites
- Sessions de **5 heures** qui se resetent
- Jusqu'a **4.5 sessions par jour**
- **Session limit** : limite par session (plus souvent atteinte)
- **Weekly limit** : limite globale de la semaine (rarement atteinte avec Max 20x)
- La StatusLine affiche le graphique session/weekly limit en temps reel

### Couts API directs (pour comparaison)
- Opus : 15$/M input + 75$/M output
- Avec Max 20x, le ratio est x16 par rapport a l'API directe

### Recommandation
Commencer a 20$ (Pro), monter selon les besoins. Max 20x = meilleur rapport qualite/prix.

---

## Nouveautes 2026

### Claude Review (beta entreprise)
- Review automatique des PR avec process multi-agents en parallele
- PR avec reviews passent de **16% a 54%** d'acceptation
- Moins de **1%** des reviews marquees incorrectes
- **94%** des bugs trouves sur les grandes PR
- Cout : **15-25$ par review** (vs ingenieur mid SF : 77-96$/h)

### Voice Mode
- `/voice` dans Claude Code
- Fonctionne mieux en anglais
- Ne remplace PAS les outils de dictee natifs (Super Whisper, WhisperTurbo, etc.)

### Ask User Question ameliore
- Peut maintenant proposer des UI avec exemples de code directement dans le terminal
- Choix multiples : selectionner une option et appuyer sur `N` pour ajouter une note

### /simplify (nouveau)
Review automatique par 3 agents paralleles.

### /loop et /schedule (nouveaux)
Taches recurrentes temporaires (/loop) et persistantes (/schedule).

---

## Outils recommandes

### Stack technique Melvynx

| Outil | Usage |
|-------|-------|
| **Supabase** | Backend/BDD pour SaaS |
| **Vercel** | Deploiement production (Next.js) |
| **Netlify Drop** | Demos rapides (drag & drop du dossier dist/) |
| **NaotS** (nowts.app) | Boilerplate avec regles strictes que l'IA suit naturellement |

### Outils macOS

| Outil | Usage |
|-------|-------|
| **Raycast** | Remplacant Spotlight (launcher, clipboard, window management) |
| **CEMUX** | Terminal pour coding agents (notifs, browser integre, split pane) |
| **Ghostty** | Terminal rapide et configurable |
| **CleanShotX** | Screenshots annotes pour debugging visuel |
| **WhisperTurbo** | Speech-to-text local gratuit |
| **Proxyman** | Debug HTTP / inspection du contexte Claude Code |

---

## tmux

Multiplexeur terminal — plusieurs Claude Code en parallele.

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

### Cas d'usage concret
Un tab par role : CC1 (feature), CC2 (review), Server, Ingest, etc.

---

## Dossier CC

Dossier personnel (`~/cc/`, accessible via alias `cdcc`) pour tout faire avec Claude Code hors projets.

### Usages
- Generateur de titres YouTube
- Createur de slides (site web)
- Analyses business
- Reviews d'emails
- Gestion de projets
- Comptabilite
- Todo lists

**Philosophie Melvynx** : "Est-ce que Claude Code peut le faire ? Si oui, pas besoin de SaaS."

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
Hetzner CAX21/CAX31 (ARM, ~$7.59/mois). IPv4 obligatoire.

### Workflow
1. Message/audio Telegram avec description de la feature
2. Clone worktree → npm install → GitHub Issue → lance apex --pr
3. Notification Telegram avec lien PR
4. "Merge la PR et delete le worktree"

### Modeles recommandes OpenClaw

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2/$12 |
| Cron jobs | Gemini 3 Flash | Tres faible |
| Agents code | Claude Opus | Abonnement |

### Skills OpenClaw (remplacent les MCP)
Philosophie : un agent Claude peut directement `fetch` n'importe quelle API. Un skill = SKILL.md + execute.sh. Plus leger que MCP.

Skills disponibles : Cloudflare, CodeLine, Dub, FlightData, Front, Google Image, LinkedIn Post, Lumail, Mercury, Porkbun, SaveIt, TypeFully, Vercel, YouTube Comments

### Configuration Gmail + Calendar
1. Creer projet Google Cloud
2. Activer APIs Gmail et Calendar
3. Creer Credentials > OAuth > Application de bureau
4. Telecharger JSON credentials
5. Donner a OpenClaw via Telegram
6. Cliquer lien OAuth pour autoriser
7. Coller le code d'autorisation

**Cloudflare Tunnel** : necessite un domaine et Cloudflared installe sur le VPS. Expose une adresse publique pour les webhooks Gmail en temps reel.

### Fichiers de configuration OpenClaw
- `soul.md` : identite/personnalite du bot
- `memory/` : memoire quotidienne
- `canvas/index.html` : interface web locale
- `.env` : variables d'environnement

### Securite OpenClaw
- Le gateway ouvre un port localhost (18789) potentiellement accessible
- Commande d'audit : `claudebot security-audit`
- Ne PAS activer 1Password sans reflexion (acces a toutes les cles)
- Telegram est plus securise que l'interface web
- UFW + Fail2Ban (whitelister IPs AVANT)
- Desactiver password SSH
- Docker pour sandboxing

### Bypass permission sur VPS
```bash
SANDBOX=1 claude          # Claude croit etre dans une sandbox
export SANDBOX=1          # Mettre dans .bashrc pour permanent
```

### Bouton "Partager via Telegram"
Depuis n'importe quelle app (Twitter, Safari, etc.), partager un lien/tweet/article directement au bot.

---

## Niveaux de configuration

| Niveau | Description |
|--------|------------|
| 0 | Aucune config |
| OK | Claude Code de base |
| OG (90%) | Config gratuite : `npx ig-blueprint-cli` |
| OG++ (99%) | Config premium (review securite, clean code, workflows avances) |

> "90% OG avec la config free, 99% OG avec la config premium, 100% impossible car il n'y a que moi" — Melvynx

---

## Credits

Methode basee sur la Masterclass de [Melvynx (CodeLynx)](https://codelynx.dev).
Transcriptions originales dans le dossier `source/`.

### Applications de Melvynx (pour contexte)
- **Umail.io** : newsletter (2M+ emails)
- **SaveIt.Now** : gestionnaire de signets avec IA
- **Subfast** : generateur de miniatures YouTube
- **Chao.App** : chatbot integrable, cree sans une ligne de code
- **PaddleTally** : app iOS + Apple Watch
- Plateforme de formation (editeur custom, 4+ ans de code)
