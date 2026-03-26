# BIBLE COMPLETE -- Claude Code & DevOps/SysAdmin
## Pour kim13 -- Night Auditor & SysAdmin
### Compilee le 2026-03-26

**Sources** : Formation Melvynx (6 videos, 14 283 lignes), Skill SysAdmin-DevOps, Configuration BOKADOR, Infrastructure kim13

---

# TABLE DES MATIERES

- **CHAPITRE 1** -- Claude Code : Fondamentaux
- **CHAPITRE 2** -- Systeme de Memoire (CLAUDE.md, Rules, Settings)
- **CHAPITRE 3** -- Outils Avances (Skills, Hooks, MCP, Sub-agents)
- **CHAPITRE 4** -- Workflows Pro (EPCT, Apex, OneShot, Debug, Brainstorm)
- **CHAPITRE 5** -- Gestion du Contexte et Anti-patterns
- **CHAPITRE 6** -- Prompting Concret (Greenfield, Brownfield, Debug UI)
- **CHAPITRE 7** -- Multi-agents (Teams, Work Trees, Tmux)
- **CHAPITRE 8** -- Configuration, Pricing et Deploiement
- **CHAPITRE 9** -- OpenClaw (Agent IA distant via Telegram)
- **CHAPITRE 10** -- Infrastructure as Code (IaC)
- **CHAPITRE 11** -- CI/CD (Integration et Deploiement Continu)
- **CHAPITRE 12** -- Monitoring & Observabilite
- **CHAPITRE 13** -- Securite Systeme et Applicative
- **CHAPITRE 14** -- Conteneurisation & Orchestration
- **CHAPITRE 15** -- Backup & Disaster Recovery
- **CHAPITRE 16** -- Logging
- **CHAPITRE 17** -- Automatisation & Scripting
- **CHAPITRE 18** -- Documentation & Runbooks
- **CHAPITRE 19** -- Adaptation a l'Infrastructure kim13
- **CHAPITRE 20** -- Checklist Rapide -- Les Essentiels
- **CHAPITRE 21** -- Skills Custom kim13 (39 skills)
- **CHAPITRE 22** -- Agents Custom kim13 (20 agents)
- **CHAPITRE 23** -- Hooks kim13 (8 hooks)
- **CHAPITRE 24** -- Plugins Actifs kim13
- **CHAPITRE 25** -- Rules kim13 (20 regles)
- **CHAPITRE 26** -- Deny-List Complete (34 regles)
- **CHAPITRE 27** -- Reseau Complet Bbox/NAT (12 regles)
- **CHAPITRE 28** -- Scripts & Fichiers Memoire kim13
- **CHAPITRE 29** -- Procedures Operationnelles (Runbooks)
- **CHAPITRE 30** -- Protocole Auto-Apprentissage kim13
- **CHAPITRE 31** -- Regle #0 Absolue : Documenter Avant Toute Modification
- **CHAPITRE 32** -- Inventaire MCP Complet (28+ MCP)
- **CHAPITRE 33** -- LaunchAgents macOS (10 agents com.kim.*)
- **CHAPITRE 34** -- Commandes Custom (43 commandes)
- **CHAPITRE 35** -- Contenu Fichiers Memoire (Bootstrap Jarvis)
- **CHAPITRE 36** -- Prompt Discovery (Anti Lost-in-the-Middle)
- **CHAPITRE 37** -- Proxyman (Debug Contexte Claude Code)
- **CHAPITRE 38** -- Opera Cloud PMS & Workflows Hotelier
- **CHAPITRE 39** -- Disaster Recovery Complet
- **CHAPITRE 40** -- Alias Shell & Raccourcis (zshrc)
- **CHAPITRE 41** -- Architecture Skill+SubAgent+Workflow (Pattern Melvynx)
- **CHAPITRE 42** -- Claude Review (Beta Entreprise)
- **CHAPITRE 43** -- Anomalies Connues & Corrections
- **CHAPITRE 44** -- CLAUDE.md Global Reference
- **CHAPITRE 45** -- Profils SSH (~/.ssh/config)
- **CHAPITRE 46** -- Methode Melvynx Complete (20 sections)
- **CHAPITRE 47** -- Skills Cowork (12 skills VM)
- **CHAPITRE 48** -- Anti-Patterns Complets (18 patterns)
- **CHAPITRE 49** -- Checklists Pre/Post Session
- **CHAPITRE 50** -- Flags CLI Avances Claude Code
- **CHAPITRE 51** -- Scripts Deployes (VPS + Mac, 7 scripts)
- **CHAPITRE 52** -- Etat Systeme & Taches Restantes

---
---

# CHAPITRE 1 -- CLAUDE CODE : FONDAMENTAUX

## 1.1 Le Terminal

Le terminal est le pilier du workflow Claude Code. Commandes essentielles :

| Commande | Action |
|----------|--------|
| `echo "texte"` | Affiche du texte |
| `ls` | Liste le contenu du dossier courant |
| `pwd` | Affiche le chemin complet |
| `cd dossier` | Se deplacer |
| `cd ..` | Revenir au parent |
| `mkdir nom` | Creer un dossier |
| `open .` | Ouvrir dans Finder (macOS) |
| `code .` | Ouvrir dans VS Code |

Bash/ZSH sur macOS/Linux, PowerShell sur Windows, WSL comme pont.

Terminaux recommandes : Ghosty, CEMUX (build pour coding agents, notifications quand un agent a besoin d'attention, browser integre, split pane, scriptable en Swift/AppKit).

Outils macOS recommandes : Raycast (remplacant Spotlight), Homebrew, CleanShotX (screenshots annotes), WhisperTurbo (speech-to-text local).

## 1.2 Installation

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

REGLE FONDAMENTALE : JAMAIS lancer Claude Code a la racine du systeme (/). Toujours se placer dans un dossier projet.

## 1.3 Interface Claude Code

### Caracteres speciaux

| Prefixe | Usage |
|---------|-------|
| `@` | Taguer un fichier |
| `/` | Commandes slash |
| `!` | Mode bash direct |

### Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `Escape Escape` | Clear texte, puis acces historique |
| `Shift+Tab` | Changer mode permissions (Normal, Accept Edit, Bypass) |
| `Ctrl+T` | Taches en cours |
| `Ctrl+O` | Transcript verbose |
| `Ctrl+S` | Sauvegarder le prompt en cours |
| `Meta+P` | Changer de modele |

### Commandes slash

| Commande | Action |
|----------|--------|
| `/clear` | Reset du contexte (ESSENTIEL entre taches majeures) |
| `/init` | Generer le CLAUDE.md du projet |
| `/stats` | Statistiques de la session |
| `/usage` | Limites et consommation |
| `/context` | Utilisation du contexte en % |
| `/model` | Changer le modele |
| `/mcp` | Voir les MCP connectes |
| `/agent` | Gerer les agents |
| `/voice` | Mode vocal (beta) |
| `/loop` | Tache recurrente a intervalle |
| `/schedule` | Taches planifiees (cron persistant) |
| `/simplify` | Review et simplification (3 agents) |
| `/copy` | Copier la derniere reponse en Markdown |
| `/config` | Configuration |
| `/batch` | Agents paralleles avec work trees (ANTI-PATTERN) |

## 1.4 Architecture

Trois produits Anthropic : Claude Chat (one-shot web), Claude API (acces direct modele), Claude Code (agent agentique terminal).

Fonctionnement agentique : Claude Code donne des outils au modele (Bash, Edit, Multi Edit, Read, Write, List, Web Fetch, Web Search, ToDo, MCP). Boucle : recuperer infos > planifier > agir > verifier > repeter.

---

# CHAPITRE 2 -- SYSTEME DE MEMOIRE

## 2.1 Les trois niveaux de CLAUDE.md

| Niveau | Emplacement | Portee |
|--------|-------------|--------|
| Global | `~/.claude/CLAUDE.md` | Tous les projets |
| Projet | `./CLAUDE.md` (racine) | Ce projet |
| Sous-dossier | `./src/components/CLAUDE.md` | Charge quand l'IA lit un fichier de ce dossier |

Chaque CLAUDE.md est injecte dans le contexte. Chaque info doit etre fondamentalement utile. Si >200 lignes : deplacer dans `.claude/rules/`.

## 2.2 Les Rules (`.claude/rules/`)

Fichiers `.md` charges automatiquement. Rules ciblees avec globs possibles (ex: regle qui ne s'applique qu'aux fichiers `*.json`).

Bonnes pratiques : si Claude fait une erreur 2+ fois, creer une regle. Garder les regles courtes. Utiliser `CRITICAL:` pour les plus importantes.

## 2.3 Settings et Permissions

### Mode Bypass Permission (recommande)

```json
{
  "defaultMode": "bypassPermission"
}
```

Ajouter une deny-list pour les commandes dangereuses. Double securite : deny-list dans settings.json + regle dans CLAUDE.md ("utilise `trash` au lieu de `rm -rf`").

### settings.local.json

Regles locales a la machine (pas partagees avec l'equipe).

## 2.4 Auto-Memory

`autoMemoryEnabled: true` (actif par defaut). Claude s'ecrit des notes entre les sessions dans `~/.claude/projects/<hash>/memory/`. 200 premieres lignes de `MEMORY.md` chargees a chaque session.

Conseil : preferer "modifie le CLAUDE.md avec cette info" plutot que laisser l'auto-memory sauvegarder n'importe quoi.

---

# CHAPITRE 3 -- OUTILS AVANCES

## 3.1 Skills

Prompts reutilisables lances avec `/nom-du-skill`. Peuvent accepter des parametres (`$ARGUMENTS`).

Petit skill : un seul fichier `SKILL.md`. Grand skill : dossier avec `SKILL.md` (entry point), `references/`, `scripts/`.

REGLE : ne JAMAIS creer les skills manuellement, laisser Claude les creer (utilise l'agent interne CloudCodeGuide).

Installation externes : `npx skill add <auteur>/<nom>` (source : skill.sh)

## 3.2 Hooks

| Hook | Declencheur |
|------|------------|
| PreToolUse | Avant execution d'un outil |
| PostToolUse | Apres execution d'un outil |
| Stop | Quand Claude termine |
| Notification | Quand Claude a besoin d'attention |

Exemples : son de fin (afplay sur macOS), Prettier automatique (PostToolUse sur Edit).

Ne pas surcharger de hooks. Hooks async (`async: true`) pour actions non-bloquantes.

## 3.3 MCP (Model Context Protocol)

Outils externes connectes via protocole standardise. Chaque MCP consomme du contexte MEME sans etre utilise.

REGLE : Maximum 2-3 MCP. Au-dela, qualite degradee.

Recommandes : Context7 (doc technique a jour, gratuit), Exa.ai (recherche web optimisee IA, gratuit avec 20$ credit).

Si >10% du contexte utilise par MCP : Tool Search s'active (`toolSearchMode: auto`).

## 3.4 Sub-agents

Claude Code lance des mini instances de lui-meme en parallele. Un sub-agent fait sa recherche dans son propre contexte (peut consommer 120K tokens) et retourne seulement 1000-1500 mots au main agent.

| Type | Usage | Vitesse |
|------|-------|---------|
| Explore | Recherche, exploration | Rapide |
| Task | Actions generiques | Normal |

Creation : `/agent` > Create New Agent > Personal Folder > Generate with Cloud

Sub-agents specialises recommandes : web-search, explore-codebase, doc-explorer.

## 3.5 Status Line

Barre d'info en bas du terminal : dossier courant, modele, graphe utilisation, weekly limit, depenses, repo git, % contexte.

---

# CHAPITRE 4 -- WORKFLOWS PRO

## 4.0 Le Pattern EPCT (fondamental)

1. **E**xplore : sub-agents (explore-codebase, explore-doc, web-search)
2. **P**lan : plan mode, planifier
3. **C**ode : executer le plan
4. **T**est : lint, tests, verification

## 4.1 OneShot vs Apex

| | `/oneshot` | `/apex` |
|---|---|---|
| Pour | Petites features | Features complexes |
| Vitesse | Rapide | Plus lent mais plus fiable |
| Intervention | Zero | Parametrable |
| Exploration | Legere | Profonde (sub-agents) |
| Taux de succes | Bon | >99% |

## 4.2 Apex en detail

| Flag | Signification |
|------|---------------|
| `--auto` (`-A`) | Mode automatique |
| `--examine` (`-e`) | Review (securite, clean code) |
| `--test` (`-t`) | Lance les tests |
| `--economy` (`-$`) | Evite sub-agents |
| `--branch` (`-b`) | Nouvelle branche git |
| `--pr` (`-p`) | Cree une Pull Request |
| `--interactive` (`-i`) | Mode interactif |
| `--teams` (`-m`) | Multi-agents |
| `--resume` (`-r`) | Reprendre tache interrompue |

Phases : Initialisation > Analyse complexite > Exploration > Planification > Execution > Validation > (Premium) Review

CONSEIL CLE : Ne pas taguer de fichiers (`@fichier`) avec Apex -- laisser l'exploration les trouver.

## 4.3 Autres workflows

### /debug
Fix erreurs. Methode la plus efficace : logs (console.log strategiques > reproduire > copier logs bruts > envoyer a Claude).

### /brainstorm
4 rounds : Exploration > Devil's advocate > Synthese (5 perspectives) > Cristallisation. Tres gourmand en tokens.

### /simplify
3 agents de review en parallele. Review code modifie pour reuse, qualite, efficacite.

### /loop
Tache recurrente a intervalle. Ex : `/loop 2m check if PR tests pass, if no, fix it`. Auto-expire quand la session se ferme.

### /schedule
Taches planifiees remote (cron persistant).

### /batch (ANTI-PATTERN)
Lance des agents avec work trees separees, chaque agent cree sa PR. Melvynx est CONTRE : trop de PR a review. Preferer un main agent orchestrateur avec sub-agents.

## 4.4 Meta-prompting

Prompts qui creent des prompts : prompt-creator, cloud-memory, meta-hook-creator, meta-skill-creator, subagent-creator.

---

# CHAPITRE 5 -- GESTION DU CONTEXTE

## 5.1 Chiffres

- 200 000 tokens max par conversation
- ~27 000 tokens au demarrage (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- Autocompact a ~96-99% (degrade qualite)

## 5.2 Lost in the Middle

Les transformers donnent plus d'importance aux tokens du debut et de la fin. Les tokens au milieu sont structurellement moins pris en compte. Quand le contexte grandit, les instructions initiales se retrouvent "au milieu".

## 5.3 Solution : Prompt Discovery (Multi-step)

Decouper en etapes (`steps/step1-init.md`, `step2-analyse.md`, etc.). Chaque etape lit son fichier juste avant d'agir. Le prompt courant est toujours "recent".

## 5.4 Regles de gestion

- `/clear` OBLIGATOIRE entre taches majeures
- `/context` regulierement
- Si contexte > 80% : `/clear` immediat
- Langage naturel pour petites modifs (economise contexte)
- Sub-agents pour recherches
- Maximum 2-3 MCP actifs

---

# CHAPITRE 6 -- PROMPTING CONCRET

## 6.1 Greenfield (Nouveau projet)

- Preciser la stack (ex: "VJS React ShadCN Tailwind")
- Decrire la feature en langage naturel
- Separer specs desktop et mobile
- Ne PAS specifier la technique sauf preference forte
- Mettre preferences techniques dans CLAUDE.md APRES le premier run

## 6.2 Brownfield (Projet existant)

L'IA suit automatiquement les conventions existantes. Utiliser screenshots annotes et references aux composants existants.

## 6.3 Techniques de debug

1. **Screenshot** : envoyer capture du bug
2. **Logs** (la plus efficace) : console.log strategiques > reproduire > copier logs bruts > envoyer
3. **10 variations UI** : creer 10 versions differentes, choisir la meilleure

## 6.4 Eviter les erreurs repetitives

1. Rules dans `.claude/rules/` (erreur 2+ fois = creer regle)
2. Workflow dans CLAUDE.md ("avant de modifier, lire 3 fichiers similaires")
3. Rules ciblees avec globs
4. Commentaires inline (prompt injection benefique) dans les fichiers de librairies internes
5. Changelog automatique dans CLAUDE.md

## 6.5 Conseils quotidiens

- Features complexes : workflow (Apex)
- Petites modifs : langage naturel simple
- Fautes d'orthographe OK, l'IA comprend
- Ne pas empiler les demandes pendant que l'IA travaille
- Attention aux mauvais chats (plusieurs terminaux)

---

# CHAPITRE 7 -- MULTI-AGENTS

## 7.1 Agent Teams

`enableAgentTeams: true` dans settings.json. Le main agent (team lead) cree des equipes avec task list partagee. Switcher entre agents avec fleches bas. Teams utilisent Tmux.

OUI : zones de modification SEPAREES (monorepo front/back/mobile). NON : une seule zone (agents se marchent dessus).

## 7.2 Work Trees

Travailler sur des branches separees en parallele. Maximum 4 work trees simultanes. Pour refactors/migrations.

## 7.3 Tmux

```bash
brew install tmux   # macOS
apt install tmux    # Linux/WSL
```

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `Ctrl+A C` | Nouveau terminal |
| `Shift+fleches` | Switcher |
| `Ctrl+A S` | Split |
| `Ctrl+A D` | Detacher |
| `tmux a` | Rattacher |

---

# CHAPITRE 8 -- CONFIGURATION ET PRICING

## 8.1 Abonnements

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| Max 20x | 200$ | ~3 200$ | x16 (meilleur) |

API directe : Opus = 15$/M input + 75$/M output.

## 8.2 Le dossier `.claude`

| Dossier | Contenu |
|---------|---------|
| `projects/` | Historique sessions |
| `plans/` | Plans crees |
| `rules/` | Regles globales |
| `skills/` | Skills (deplacer dans "disabled" pour desactiver) |
| `agents/` | Agents |
| `scripts/` | Scripts personnels |
| `tasks/` | Taches executees |
| `teams/` | Equipes (JSON) |

## 8.3 Plugins

Installation : `/plugin`. Deux sources officielles : "Anthropic Cloud Code" et "Anthropic Cloud Official Plugins".

PROBLEME : perte de controle (code non modifiable, ecrase a chaque MAJ). RECOMMANDATION FORTE : copier le code des plugins dans vos propres skills/agents.

## 8.4 Commandes Melvynx (Resume)

| Commande | Usage | Cout |
|----------|-------|------|
| `/apex` | Feature complete, >99% | Eleve |
| `/oneshot` | Quick fix rapide | Faible |
| `/debug` | Fix erreurs | Moyen |
| `/brainstorm` | Recherche profonde 4 rounds | Tres eleve |
| `/clean-code` | Principes clean code | Moyen |
| `/review-code` | Review multi-agents | Eleve |
| `/load-memory` | Optimise CLAUDE.md | Faible |
| `/commit` | Commits simplifies | Faible |
| `/fix-errors` | Corrige erreurs parallele | Moyen |
| `/fix-grammar` | Corrige grammaire bulk | Moyen |
| `/ultrathink` | Max thinking tokens | Tres eleve |

## 8.5 Niveaux de configuration

| Niveau | Description |
|--------|------------|
| 0 | Aucune config |
| OK | Claude Code de base |
| OG (90%) | Config gratuite Melvynx (`npx ig-blueprint-cli`) |
| OG++ (99%) | Config premium Melvynx |

Blueprint CLI installe : backup auto, skills gratuits, command validator, statusline (necessite bun), 2 sons de notification, permissions (bypass + deny-list).

---

# CHAPITRE 9 -- OPENCLAW (Agent IA distant via Telegram)

## 9.1 Concept

Controler son ordinateur a distance depuis Telegram, y compris par audio/vocal. Lance des agents Claude Code en arriere-plan, notifie quand termine.

## 9.2 Installation

```bash
curl maltbot.sh
```

Configurer Telegram via BotFather > `/newbot` > copier token API.

## 9.3 Installation VPS (Docker)

VPS recommande : Hetzner CAX21/CAX31 (ARM, ~$7.59/mois). Script automatise : `npx openclaw-vps-setup`.

Installe : Node.js, OpenClaw, Claude Code, Bun, Cloudflared, GCloud, UFW, Fail2Ban, SSH Hardening.

Bypass permission VPS : `SANDBOX=1 claude` (ajouter `export SANDBOX=1` dans .bashrc).

## 9.4 Workflow OpenClaw

1. Envoyer message/audio Telegram
2. Skill "code_task" : clone worktree > npm install > GitHub Issue > `claude -p --dangerously-skip-permissions apex --pr <issue> <description>` > cron watcher
3. Notification Telegram avec lien PR
4. "merge la PR et delete le worktree"

## 9.5 Skills OpenClaw

Skills remplacent les MCP. Un skill = `SKILL.md` (prompt) + `execute.sh` (script API). Plus leger, plus controlable.

## 9.6 Configuration Gmail + Calendar

Google Cloud > activer API Gmail/Calendar > OAuth > donner JSON a OpenClaw > Cloudflare Tunnel pour webhooks.

## 9.7 Modeles recommandes

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2/$12 per M |
| Cron jobs | Gemini 3 Flash | Tres bas |
| Agents de code | Claude Opus | Meilleure qualite |

## 9.8 Securite OpenClaw

Gateway ouvre port localhost (18789). Sandboxing Docker obligatoire. Ne PAS activer 1Password sans reflexion. Telegram plus securise que l'interface web.

---
---

# CHAPITRE 10 -- INFRASTRUCTURE AS CODE (IaC)

## Principes

L'infrastructure doit etre traitee comme du code : versionnee, testee, revue, reproductible. Eliminer toute action manuelle sur les serveurs.

## Rules

- Tout en version control (Git, sans exception)
- Jamais de modification manuelle en production
- Immutabilite des serveurs : remplacement > modification
- Etat desire (desired state) : Ansible, Terraform, Puppet

## Instructions

1. Identifier l'outil IaC (Terraform, Ansible, Pulumi, CloudFormation)
2. Structurer en modules reutilisables
3. Separer variables par environnement (dev, staging, prod)
4. Inclure README avec instructions d'application
5. Ajouter des outputs utiles (IP, DNS, endpoints)

```
modules/
  networking/
    main.tf / variables.tf / outputs.tf
  compute/
    main.tf / variables.tf / outputs.tf
environments/
  dev/
    main.tf / terraform.tfvars
  prod/
    main.tf / terraform.tfvars
```

---

# CHAPITRE 11 -- CI/CD

## Principes

Detecter les problemes le plus tot possible (shift left). Rendre les deploiements routiniers et ennuyeux.

## Rules

- Pipeline as Code (Jenkinsfile, .github/workflows/*.yml)
- Build une fois, deployer partout (meme artefact en staging et prod)
- Tests automatises obligatoires (lint, unit, integration, SAST/DAST)
- Rollback instantane (<5 min) : blue/green ou canary
- Secrets hors du code (Vault, AWS Secrets Manager, GitHub Secrets)

## Structure pipeline

1. Build : compilation, creation artefact
2. Test : unit, integration, security scan
3. Deploy staging : auto
4. Approval : gate manuelle avant prod
5. Deploy prod : avec strategie rollback

---

# CHAPITRE 12 -- MONITORING & OBSERVABILITE

## Principes

On ne peut pas gerer ce qu'on ne mesure pas. Comprendre le comportement normal pour detecter les anomalies.

## Rules

- Les 3 piliers : Metriques (Prometheus), Logs (ELK/Loki), Traces (Jaeger/Tempo)
- Alertes actionables uniquement (chaque alerte = runbook)
- Les 4 Golden Signals (Google SRE) : Latence, Traffic, Erreurs, Saturation
- SLO/SLI/SLA : definir clairement (ex: "99.9% des requetes < 200ms")
- Dashboards par audience : exec, ops, dev
- Retention adaptee : haute resolution 2 semaines, puis agregation

## Instructions

1. Commencer par les 4 Golden Signals
2. Stack recommandee : Prometheus + Grafana (standard open source)
3. Dashboards : vue d'ensemble, par service, infrastructure
4. Alertes avec seuils realistes et runbooks
5. Prevoir retention et stockage

---

# CHAPITRE 13 -- SECURITE

## Principes

Securite = pratique continue integree a chaque phase (DevSecOps). Moindre privilege + defense en profondeur.

## Rules

- Principe du moindre privilege : chaque user/service/conteneur n'a acces qu'au strict necessaire
- Pas de secrets en clair : vault + rotation 90 jours max
- Mises a jour automatisees : unattended-upgrades
- SSH : desactiver root, cles uniquement, port non standard, fail2ban
- Firewall : deny-all par defaut, ouvrir uniquement le necessaire
- Auditing : logs centralises, immuables, retention 1 an min
- Scan regulier : images Docker (Trivy), dependances (Dependabot/Snyk), infra (ScoutSuite)
- 2FA/MFA obligatoire pour tout acces admin

## Configuration SSH durcie

```
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
AllowUsers deploy admin
Protocol 2
```

---

# CHAPITRE 14 -- CONTENEURISATION & ORCHESTRATION

## Rules

- Images minimales : Alpine, distroless
- Multi-stage builds : separer build du runtime
- Un processus par conteneur
- Pas de donnees dans le conteneur : volumes ou services manages
- Tags explicites : jamais `latest` en prod (SHA commit ou semver)
- Health checks : toujours liveness et readiness probes
- Limites de ressources : toujours requests et limits CPU/memoire

## Dockerfile best practices

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:20-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
USER appuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD wget -q --spider http://localhost:3000/health || exit 1
CMD ["node", "dist/server.js"]
```

## Kubernetes best practices

- replicas: 3, RollingUpdate (maxUnavailable: 1, maxSurge: 1)
- securityContext: runAsNonRoot, runAsUser: 1000
- resources: requests + limits
- liveness/readiness probes sur /health et /ready

---

# CHAPITRE 15 -- BACKUP & DISASTER RECOVERY

## Rules

- Regle 3-2-1 : 3 copies, 2 supports differents, 1 hors site
- Tester les restaurations trimestriellement minimum
- Automatiser les backups (cron, notifications echec)
- Chiffrement au repos et en transit
- Documentation restauration accessible meme si infra down
- JAMAIS supprimer sans confirmer backup complet verifie

## RPO/RTO

Definir Recovery Point Objective et Recovery Time Objective pour chaque service critique.

---

# CHAPITRE 16 -- LOGGING

## Rules

- Logs structures (JSON)
- Centralisation (ELK, Loki, Datadog)
- Niveaux coherents : DEBUG, INFO, WARN, ERROR, FATAL
- Rotation (logrotate)
- Jamais de donnees sensibles dans les logs
- Correlation : request ID / trace ID

## Format recommande

```json
{
  "timestamp": "2026-03-26T10:15:30.123Z",
  "level": "ERROR",
  "service": "payment-api",
  "trace_id": "abc123def456",
  "message": "Payment processing failed",
  "error": {"type": "TimeoutError", "message": "Gateway timeout after 30s"},
  "context": {"user_id": "usr_789", "endpoint": "/api/v1/payments", "method": "POST", "duration_ms": 30042}
}
```

---

# CHAPITRE 17 -- AUTOMATISATION & SCRIPTING

## Rules

- Scripts idempotents (executer 2 fois = meme resultat)
- Gestion des erreurs : `set -euo pipefail`
- Logging avec timestamp dans chaque script
- Header documentant ce que fait le script, prerequis, parametres
- Tests pour scripts critiques (bats pour Bash, pytest pour Python)
- Pas de hardcoding : variables d'environnement ou fichiers de config

## Template Bash

```bash
set -euo pipefail

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
LOG_FILE="/var/log/myscript_$(date +%Y%m%d).log"

log() { echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"; }
die() { log "ERREUR: $*"; exit 1; }
cleanup() { log "Nettoyage..."; }
trap cleanup EXIT

command -v docker >/dev/null 2>&1 || die "Docker non installe"

main() {
  log "Demarrage"
  log "Termine avec succes"
}
main "$@"
```

---

# CHAPITRE 18 -- DOCUMENTATION & RUNBOOKS

## Rules

- Runbooks pour chaque alerte (etapes exactes)
- Architecture Decision Records (ADR) pour decisions techniques
- Diagrammes d'architecture a jour
- Post-mortems apres chaque incident (sans blame)
- Documentation as Code (meme repo, meme workflow)

## Template Runbook

```
## Runbook: [Nom de l'alerte]
Severite : Critical / Warning / Info
Service affecte : [nom]

### Symptomes
- Ce que l'alerte indique
- Ce que l'utilisateur observe

### Diagnostic
1. Verifier [commande]
2. Consulter dashboard [lien]
3. Logs : kubectl logs -l app=service --tail=100

### Resolution
1. Action concrete 1
2. Action concrete 2
3. Verifier resolution alerte

### Escalation
- Non resolu en 15 min : contacter [equipe]
- Impact utilisateur : communiquer sur [canal]
```

---

# CHAPITRE 19 -- ADAPTATION INFRASTRUCTURE KIM13

## 19.1 Machines

| Machine | Specs | Role |
|---------|-------|------|
| MacBook Pro M4 | 24GB RAM, 926GB SSD | Poste principal, Claude Code |
| BOKADOR | Windows, RTX 3060, 40TB (H:) | Serveur stockage, SSH 31.35.20.184:443 |
| VPS Kali | 109.176.197.139 | Operations securite |

## 19.2 BOKADOR -- Regles critiques

- JAMAIS toucher aux disques H: et G: sans autorisation explicite
- SSH via port 443, user kim13
- WSL Ubuntu-24.04 disponible avec rsync 3.2.7
- rsync via SSH+WSL : `rsync --checksum -e "ssh -p 443" --rsync-path="wsl -d Ubuntu-24.04 -- rsync"`

## 19.3 IPs a whitelister (firewall/fail2ban)

| IP | Description |
|----|-------------|
| 176.133.20.227 | Domicile Soly |
| 185.208.60.2 | Ibis/Rezocean (travail) |
| 109.176.197.139 | VPS Hostinger |
| 31.35.20.184 | BOKADOR |
| 92.92.127.0/23 | Plage Kim |

## 19.4 Config BOKADOR verifiee

Conforme a la config Melvynx : bypass permission + deny-list, hooks (Stop, PreToolUse, PostToolUse), enableAgentTeams, autoMemoryEnabled, 18 agents, skills complets (/apex, /oneshot, /load-memory, /commit, /fix-errors, /fix-grammar, /fix-pr, /ultrathink), plugins, statusline.

## 19.5 Notifications Claude Code

- macOS : changer iTerm de "Alerts" a "Banners" dans System Settings > Notifications
- terminal-notifier installe pour notifications custom avec auto-dismiss
- Hooks crees : notify-stop.sh et notify-permission.sh

## 19.6 Transfer Media MacBook > BOKADOR

Script rsync v4 pret : 11 632 fichiers, 201.59 Go (514 videos 128GB, 11 118 photos 74GB). Commande : `python3 ~/transfer_media_to_bokador.py`. Exclut cloud-only/dataless, caches, cloud storage dirs.

---

# CHAPITRE 20 -- CHECKLIST RAPIDE

## Securite
SSH durci, firewall actif, secrets dans vault, MFA active, MAJ auto, scans vulnerabilites

## Monitoring
4 Golden Signals surveilles, alertes avec runbooks, dashboards crees, logs centralises

## Backup
Regle 3-2-1, restauration testee, chiffrement actif, RPO/RTO definis, JAMAIS supprimer sans backup verifie

## CI/CD
Pipeline automatise, tests obligatoires, rollback pret, secrets securises

## Documentation
Architecture documentee, runbooks a jour, procedure DR testee, ADRs a jour

## Conteneurs
Images minimales, health checks, limites ressources, pas de root, tags explicites

## Claude Code
/clear entre taches, max 2-3 MCP, sub-agents pour recherches, EPCT pour workflows, bypass+deny-list, rules pour erreurs repetitives

---
---

# CHAPITRE 21 -- SKILLS CUSTOM KIM13 (39 skills)

## Skills Workflow (les plus utilises)

| Skill | Usage |
|-------|-------|
| `/apex` | Feature complete, >99% succes (version Melvynx) |
| `/oneshot` | Quick fix rapide sans intervention |
| `/workflow-apex-free` | Apex version gratuite |
| `/workflow-code` | Workflow code standard |
| `/workflow-steps` | Workflow multi-etapes avec fichiers step |
| `/plan-before-code` | Force la phase planification avant code |
| `/strategic-compact` | Compactage strategique du contexte |

## Skills Maintenance & DevOps

| Skill | Usage |
|-------|-------|
| `/infra-check` | Verification infrastructure (BOKADOR, VPS, Mac) |
| `/backup-strategy` | Strategie backup 3-2-1 |
| `/docker-management` | Gestion conteneurs Docker |
| `/monitoring-setup` | Mise en place monitoring |
| `/network-hardening` | Durcissement reseau |
| `/security-config-audit` | Audit config securite |
| `/incident-response` | Protocole reponse incident |
| `/error-recovery` | Recovery apres erreur |

## Skills Git & Code

| Skill | Usage |
|-------|-------|
| `/commit` | Commits simplifies (Conventional Commits) |
| `/create-pr` | Creation Pull Request |
| `/merge` | Merge branches |
| `/fix-errors` | Corrige erreurs en parallele (sub-agents) |
| `/fix-grammar` | Corrige grammaire en bulk |
| `/fix-pr-comments` | Corrige commentaires PR |
| `/git-workflow` | Workflow Git complet |
| `/tdd-workflow` | Test-Driven Development |
| `/parallel-agents` | Lancer agents en parallele |

## Skills Memoire & Apprentissage

| Skill | Usage |
|-------|-------|
| `/claude-memory` | Gestion optimisee CLAUDE.md |
| `/auto-learn` | Auto-apprentissage des preferences |
| `/learn-from-user` | Apprentissage explicite correction |
| `/my-profile` | Profil kim13 complet |
| `/my-daily-standup` | Standup quotidien (charge contexte) |
| `/my-analysis` | Analyse personnalisee |
| `/thoughts-directory` | Reflexions et decisions |
| `/context-management` | Gestion contexte avancee |

## Skills Meta & Creation

| Skill | Usage |
|-------|-------|
| `/prompt-creator` | Cree des prompts optimises |
| `/meta-prompt-creator` | Cree des prompts qui creent des prompts |
| `/skill-creator` | Cree des skills |
| `/subagent-creator` | Cree des sub-agents |
| `/automation-patterns` | Patterns d'automatisation |
| `/ultrathink` | Max thinking tokens (2+ min reflexion) |
| `/ralph-loop` | Boucle iterative Ralph |

---

# CHAPITRE 22 -- AGENTS CUSTOM KIM13 (20 agents)

| Agent | Role | Quand l'utiliser |
|-------|------|-----------------|
| `action.md` | Actions generiques | Taches courtes autonomes |
| `backup-verifier.md` | Verification backups | Avant/apres operations backup |
| `code-explorer.md` | Exploration codebase | Comprendre structure projet |
| `debugger.md` | Debug avance | Erreurs complexes multi-fichiers |
| `doc-explorer.md` | Exploration documentation | Chercher dans docs techniques |
| `doc-writer.md` | Redaction documentation | Runbooks, README, ADR |
| `explore-codebase.md` | Exploration code (rapide) | Recherche fichiers/patterns |
| `explore-docs.md` | Exploration docs | Context7, web search |
| `firewall-auditor.md` | Audit firewall | Verifier regles UFW/iptables |
| `infra-checker.md` | Check infrastructure | Etat BOKADOR/VPS/Mac |
| `jarvis.md` | Assistant general | Taches multi-domaines |
| `network_monitor.md` | Monitoring reseau | Connectivite, latence, ports |
| `performance-tuner.md` | Optimisation perf | CPU, RAM, disque, reseau |
| `planner.md` | Planification | Plans d'execution complexes |
| `researcher.md` | Recherche web | Exa, Context7, web search |
| `reviewer.md` | Code review | Qualite, securite, clean code |
| `security-auditor.md` | Audit securite | Scan vulnerabilites, permissions |
| `test-writer.md` | Ecriture tests | Unit, integration, E2E |
| `web-search.md` | Recherche web | WebSearch, WebFetch, Exa |
| `websearch.md` | Recherche web (alt) | Variante web-search |

---

# CHAPITRE 23 -- HOOKS KIM13 (8 hooks)

## Hooks actifs dans settings.json

| Hook | Type | Script | Declencheur |
|------|------|--------|------------|
| command-validator | PreToolUse (Bash) | `bun ~/.claude/scripts/command-validator/src/cli.ts` | Valide chaque commande Bash avant execution |
| edit-logger | PostToolUse (Edit\|Write) | `echo '[HOOK] File modified' >> /tmp/claude-code-edits.log` | Log toute modification fichier |
| notify-stop | Stop | `bash ~/.claude/hooks/notify-stop.sh` | Notification sonore + visuelle fin de tache |
| set-tab-title | UserPromptSubmit | `bash ~/.claude/hooks/set-tab-title.sh` | Met a jour le titre du tab terminal |
| notification-sound | Notification | `afplay /System/Library/Sounds/Funk.aiff` | Son quand Claude a besoin d'attention |

## Hooks disponibles (dans ~/.claude/hooks/)

| Script | Role |
|--------|------|
| `block-delete.sh` | Bloque les suppressions non autorisees |
| `claude-firewall.js` | Firewall commandes Claude (JS) |
| `jarvis-firewall.js` | Firewall commandes Jarvis (JS) |
| `notify-permission.sh` | Notification demande de permission |
| `notify-with-timeout.sh` | Notification avec auto-dismiss |
| `pre-backup.sh` | Backup automatique avant operations risquees |

---

# CHAPITRE 24 -- PLUGINS ACTIFS KIM13

| Plugin | Source | Role |
|--------|--------|------|
| hookify | claude-plugins-official | Gestion avancee des hooks |
| commit-commands | claude-plugins-official | Commandes de commit ameliorees |
| feature-dev | claude-plugins-official | Workflow developpement features |
| code-review | claude-plugins-official | Review de code automatisee |
| code-simplifier | claude-plugins-official | Simplification et refactoring |
| pr-review-toolkit | claude-plugins-official | Toolkit review Pull Requests |
| claude-md-management | claude-plugins-official | Gestion fichiers CLAUDE.md |
| claude-code-setup | claude-plugins-official | Setup et configuration |
| security-guidance | claude-plugins-official | Conseils securite |

RAPPEL MELVYNX : copier le code des plugins dans ses propres skills pour garder le controle.

---

# CHAPITRE 25 -- RULES KIM13 (20 regles)

## Regles alwaysApply (chargees a chaque session)

| Fichier | Role |
|---------|------|
| `01-no-delete-without-triple-backup.md` | REGLE #1 ABSOLUE : triple backup avant toute suppression |
| `02-memory-architecture-melvynx.md` | Architecture memoire 3 niveaux obligatoire |
| `07-melvynx-checklist.md` | Bonnes pratiques Melvynx |
| `08-claude-code-commands.md` | Commandes et raccourcis |
| `09-mac-specifics.md` | Regles specifiques MacBook M4 |
| `11-auto-learning-live.md` | Auto-apprentissage temps reel |
| `24-process-kill-protocol.md` | Protocole obligatoire avant kill processus |
| `25-notification-format.md` | Format alertes macOS obligatoire |
| `25-progress-bars.md` | Barre de progression obligatoire taches longues |

## Regles ciblees (globs)

| Fichier | Globs | Role |
|---------|-------|------|
| `20-json-files.md` | `*.json` | Regles manipulation JSON |
| `21-git-operations.md` | `.git/**` | Regles operations Git |
| `22-security-context.md` | `**/.env*` | Regles securite secrets |
| `23-test-context.md` | `**/*.test.*` | Regles ecriture tests |
| `24-bbox-nat-infrastructure.md` | `**/ssh*, **/network*, **/nat*, **/firewall*, **/port*, **/*.conf` | Infra reseau Bbox |

## Regles reference Melvynx

| Fichier | Role |
|---------|------|
| `10-melvynx-sync.md` | Sync avec repo Melvynx/aiblueprint |
| `12-progress-tracking.md` | Progression visible taches longues |
| `30-bible-melvynx-reference.md` | Bible = source de verite |
| `31-workflows-melvynx.md` | Workflows reference rapide |
| `32-prompting-melvynx.md` | Techniques prompting |
| `33-context-engineering-melvynx.md` | Context engineering |

---

# CHAPITRE 26 -- DENY-LIST COMPLETE (34 regles)

## Commandes Bash interdites

```
rm -rf *              # Suppression recursive
rm -rf /              # Suppression racine
rm -rf ~              # Suppression home
rm -rf G:*            # Protection disque G: BOKADOR
rm -rf H:*            # Protection disque H: BOKADOR
rm -rf C:\Windows*    # Protection Windows
sudo rm *             # Suppression root
sudo rm -rf *         # Suppression root recursive
chmod 777 *           # Permissions trop permissives
git push --force *    # Force push
git push -f *         # Force push
git reset --hard *    # Reset destructif
curl * | bash         # Execution code distant
curl * | sh           # Execution code distant
wget * | bash         # Execution code distant
wget * | sh           # Execution code distant
npm publish *         # Publication npm
reboot *              # Redemarrage
shutdown *            # Arret
init 0                # Arret
init 6                # Redemarrage
dd if=/dev/zero*      # Ecrasement disque
mkfs *                # Formatage
format *              # Formatage
diskutil eraseDisk *  # Formatage macOS
diskutil eraseVolume *# Formatage volume macOS
netcfg *              # Config reseau (incident passe)
> /dev/sd*            # Ecriture directe disque
```

## Fichiers interdits en lecture

```
.env, .env.*          # Variables d'environnement
**/*password*         # Fichiers mots de passe
**/*secret*           # Fichiers secrets
**/*token*            # Fichiers tokens
**/*.pem              # Certificats prives
.ssh/id_*             # Cles SSH privees
```

---

# CHAPITRE 27 -- RESEAU COMPLET BBOX/NAT (12 regles)

## Topologie

```
Internet
  |
  v
Bbox (IP publique: 31.35.20.184, dynamique)
  |
  +-- DESKTOP-6SNQTTK (BOKADOR Windows, SSH interne 2222/22222)
  +-- kims-MBP (MacBook Pro M4, SSH interne 22)
  +-- 192.168.1.97 (device SSH port 44)
  +-- 192.168.1.4 (Ancien Mac, SSH port 22)
```

## Table NAT/PAT active

| Port ext | Port int | Equipement | Proto | Restriction IP | Usage |
|----------|----------|------------|-------|----------------|-------|
| 22 | 22 | Ancien Mac | TCP | * | SSH standard ancien Mac |
| 443 | 443 | BOKADOR | TCP | * | HTTPS/SSL passthrough |
| 1983 | 22 | kims-MBP | TCP | * | SSH MacBook depuis exterieur |
| 2223 | 2222 | BOKADOR | TCP | VPS only | SSH VPS→Windows |
| 2224 | 2222 | BOKADOR | TCP | VPS only | SSH VPS→Windows (2eme) |
| 4422 | 44 | 192.168.1.97 | TCP | * | SSH externe custom |
| 9999 | 9 | BOKADOR | UDP | * | Wake-on-LAN via VPS |
| 38271 | 2222 | kims-MBP | TCP | VPS only | SSH VPS→Mac |
| 44 | 22 | BOKADOR | TCP | * | SSH test |
| 47281 | 22222 | BOKADOR | TCP | * | SSH anti-vol port obscur |
| 50000 | 3389 | BOKADOR | TCP | * | RDP port non-standard |

## Connexions rapides

```bash
ssh -p 1983 kim13@31.35.20.184           # Mac depuis exterieur
ssh -p 47281 kim13@31.35.20.184          # BOKADOR SSH (port obscur)
ssh -p 443 kim13@31.35.20.184            # BOKADOR via HTTPS port
ssh -p 38271 kim13@31.35.20.184          # Mac depuis VPS
ssh -p 2223 user@31.35.20.184            # Windows depuis VPS
ssh root@109.176.197.139                 # VPS Kali direct
```

## Wake-on-LAN BOKADOR depuis VPS

```bash
ssh root@109.176.197.139 "wakeonlan -i 31.35.20.184 -p 9999 <MAC_BOKADOR>"
```

---

# CHAPITRE 28 -- SCRIPTS & FICHIERS MEMOIRE KIM13

## Scripts (~/.claude/scripts/)

| Script | Role |
|--------|------|
| `command-validator/` | Valide commandes Bash avant execution (bun/TypeScript) |
| `statusline/` | StatusLine avancee (bun/TypeScript) : modele, cout, tokens, %, git |
| `auto-backup.sh` | Backup automatique config Claude |
| `claude-session.sh` | Gestion sessions Claude Code |
| `watch-claude-config.sh` | Watchdog modifications config |

## Fichiers memoire (~/.claude/memory/)

| Fichier | Role |
|---------|------|
| `00_status.md` | Etat courant du systeme |
| `01_architecture.md` | Architecture infra (OBLIGATOIRE lecture avant action) |
| `02_lessons_log.md` | Lecons apprises (OBLIGATOIRE lecture avant action) |
| `03_conventions.md` | Conventions strictes (OBLIGATOIRE lecture avant action) |
| `04_autonomy_matrix.md` | Matrice d'autonomie (quand agir vs demander) |
| `05_reflection_loop.md` | Boucle de reflexion |
| `07_security_bypass_guards.md` | Gardes securite bypass |
| `08_claude_code_advanced_patterns.md` | Patterns avances Claude Code |
| `09_methode_melvynx_complete.md` | Methode Melvynx complete |
| `CLAUDE_WEB_MEMORY_EXHAUSTIVE.txt` | Memoire web exhaustive |

## Bootstrap Jarvis (OBLIGATOIRE avant chaque action)

Lire silencieusement dans l'ordre :
1. `memory/01_architecture.md` — etat du projet
2. `memory/02_lessons_log.md` — erreurs passees
3. `memory/03_conventions.md` — regles strictes

---

# CHAPITRE 29 -- PROCEDURES OPERATIONNELLES

## 29.1 Night Audit (Opera Cloud PMS - H0373)

Contexte : Mercure Paris Montmartre, poste de Night Auditor.
Opera Cloud PMS est le systeme de gestion hoteliere.
Claude Code peut assister pour : scripts d'automatisation, reporting, analyse de donnees, mais n'a PAS acces direct a Opera Cloud.

## 29.2 Runbook : BOKADOR inaccessible

1. Ping `31.35.20.184` depuis VPS : `ssh root@109.176.197.139 "ping -c 3 31.35.20.184"`
2. Si timeout : Wake-on-LAN via VPS (UDP port 9999)
3. Attendre 2 min, re-ping
4. Si toujours down : verifier IP Bbox (dynamique), verifier alimentation physique
5. Escalation : acces physique requis

## 29.3 Runbook : VPS inaccessible

1. Ping `109.176.197.139` depuis Mac : `ping -c 3 109.176.197.139`
2. Si timeout : verifier console Hostinger (reboot VPS)
3. Si SSH refuse : verifier fail2ban (`fail2ban-client status sshd`)
4. Si IP bannie : unban depuis console Hostinger ou attendre expiration

## 29.4 Runbook : Espace disque MacBook plein

1. `df -h /` pour confirmer
2. Verifier caches : `du -sh ~/Library/Caches/*` (triable)
3. Verifier logs : `du -sh ~/Library/Logs/*`
4. Verifier Docker : `docker system df` puis `docker system prune`
5. Verifier node_modules : `find ~ -name node_modules -type d -maxdepth 4`
6. Transferer media vers BOKADOR : `python3 ~/transfer_media_to_bokador.py`
7. JAMAIS supprimer sans backup confirme

## 29.5 Runbook : Contexte Claude Code sature (>80%)

1. `/context` pour verifier le pourcentage
2. `/clear` immediat
3. Si tache en cours : noter l'etat dans un fichier step avant /clear
4. Relancer avec prompt discovery (step files)
5. Reduire MCP si >3 actifs

---

# CHAPITRE 30 -- PROTOCOLE AUTO-APPRENTISSAGE KIM13

## Declencheurs automatiques

1. **Correction utilisateur** : quand kim13 corrige une action → `/memorize` auto
2. **Echec commande 2+ fois** : creer regle dans `.claude/rules/`
3. **Preference exprimee** : ajouter dans `rules/11-auto-learning-live.md` ET knowledge graph
4. **Decision technique** : persister dans `memory/` + knowledge graph

## Triple persistance obligatoire

Chaque apprentissage doit etre sauvegarde dans :
1. `rules/` (fichier .md avec alwaysApply ou globs)
2. Knowledge graph (mcp memory)
3. Rules specialisees si securite

## Commandes memoire

| Commande | Action |
|----------|--------|
| `/memorize` | Workflow auto-apprentissage complet |
| `/auto-learn` | Detection et persistance patterns |
| `/learn-from-user` | Apprentissage explicite |
| `/cloud-memory` | Optimisation quand fichiers >100 lignes |
| `/standup` | Charge contexte en debut de session |
| `/my-profile` | Affiche profil complet kim13 |

---

---
---

# CHAPITRE 31 -- REGLE #0 ABSOLUE : DOCUMENTER AVANT TOUTE MODIFICATION

**Creee le 2026-03-26 sur demande explicite kim13.**

AVANT toute modification, suppression ou ecrasement :

1. **DOCUMENTER** dans un commit GitHub AVANT l'action (quoi, pourquoi, etat avant)
2. **CHANGELOG** : entree datee
3. **KNOWLEDGE GRAPH** : persister dans memory MCP
4. **JAMAIS** modifier/supprimer/ecraser sans avoir documente

Ordre obligatoire : **DOCUMENTER > BACKUP > AGIR > VERIFIER**

Cette regle prime sur TOUTES les autres sauf la regle #1 (triple backup avant suppression). Les deux sont complementaires.

Fichier : `~/.claude/rules/00-document-before-modify.md` (alwaysApply: true)

---

# CHAPITRE 32 -- INVENTAIRE MCP COMPLET (28+ MCP)

## MCP Infrastructure & Systeme

| MCP | Outils | Usage |
|-----|--------|-------|
| Desktop Commander | start_process, write_file, read_file, edit_block, search, etc. | Controle complet MacBook (processus, fichiers, PDF) |
| filesystem | read_file, write_file, search_files, directory_tree, etc. | Operations fichiers sandboxees |
| git | git_add, git_commit, git_diff, git_log, git_status, etc. | Operations Git sans bash |
| ssh | ssh_exec, ssh_connect, sftp_read, sftp_write, etc. | SSH vers BOKADOR, VPS |
| docker | run_command | Commandes Docker |
| kubernetes | kubectl_get, kubectl_apply, kubectl_logs, helm, port_forward, etc. | Orchestration K8s |
| iterm | read_terminal_output, write_to_terminal, send_control_character | Controle iTerm2 |

## MCP Donnees & Recherche

| MCP | Outils | Usage |
|-----|--------|-------|
| memory | create_entities, add_observations, create_relations, search_nodes, read_graph | Knowledge graph persistant |
| context7 | resolve-library-id, query-docs | Documentation technique a jour |
| exa | web_search_exa, get_code_context_exa | Recherche web optimisee IA |
| fetch | fetch | Recuperer contenu web |
| postgres | query | Requetes PostgreSQL |
| sqlite | read_query, write_query, create_table, list_tables | Base SQLite locale |
| sequential-thinking | sequentialthinking | Raisonnement etape par etape |

## MCP Navigateur & Web

| MCP | Outils | Usage |
|-----|--------|-------|
| Claude in Chrome | navigate, read_page, get_page_text, form_input, computer, javascript_tool, tabs, etc. | Controle Chrome complet |
| Control Chrome | open_url, get_page_content, execute_javascript, list_tabs, etc. | Chrome via AppleScript |
| puppeteer | puppeteer_navigate, puppeteer_click, puppeteer_fill, puppeteer_screenshot, etc. | Automatisation headless |

## MCP Applications Apple

| MCP | Outils | Usage |
|-----|--------|-------|
| Control your Mac | osascript | Execution AppleScript |
| Apple Notes | add_note, list_notes, get_note_content, update_note_content | Notes Apple |
| iMessages | read_imessages, send_imessage, search_contacts, get_unread | Messages Apple |
| Google Calendar | gcal_list_events, gcal_create_event, gcal_update_event, gcal_find_free_time | Agenda Google |
| Google Drive | google_drive_search, google_drive_fetch | Fichiers Google Drive |

## MCP Systeme Cowork & Plugins

| MCP | Outils | Usage |
|-----|--------|-------|
| PDF (Anthropic) | display_pdf, list_pdfs, read_pdf_bytes | Manipulation PDF |
| scheduled-tasks | create_scheduled_task, list_scheduled_tasks, update_scheduled_task | Taches planifiees |
| session_info | list_sessions, read_transcript | Historique sessions |
| mcp-registry | search_mcp_registry, suggest_connectors | Recherche MCP disponibles |
| plugins | search_plugins, suggest_plugin_install | Gestion plugins |
| everything | echo, get-env, simulate-research-query, etc. | Outils utilitaires |
| applescript | applescript_execute | AppleScript direct |
| cowork | allow_cowork_file_delete, present_files, request_cowork_directory | Gestion fichiers Cowork |

## ATTENTION : Regle Melvynx vs Realite

Melvynx recommande max 2-3 MCP pour preserver le contexte. kim13 a 28+ MCP actifs. Ceci est compense par `toolSearchMode: auto` qui ne charge les definitions que quand necessaire. Surveiller `/context` regulierement.

---

# CHAPITRE 33 -- LAUNCHAGENTS MACOS (10 agents com.kim.*)

CRITICAL: JAMAIS supprimer ces LaunchAgents sans autorisation explicite.

| LaunchAgent | PID | Role |
|-------------|-----|------|
| `com.kim.sysadmin-monitor` | 5186 | Monitoring systeme MacBook (CPU, RAM, disque) |
| `com.kim.claude-watchdog` | 5188 | Watchdog Claude Code (relance si crash) |
| `com.kim.bokador-tunnel` | 5189 | Tunnel SSH persistant vers BOKADOR |
| `com.kim.quotes-jim-rohn` | 5196 | Citations motivantes Jim Rohn (notifications) |
| `com.kim.mount-stockage-bokador` | 5197 | Montage automatique stockage BOKADOR |
| `com.kim.terminal-journal` | 5200 | Journal terminal automatique |
| `com.kim.claude-config-backup` | inactif | Backup periodique config Claude |
| `com.kim.rappel-france-travail` | inactif | Rappels France Travail |
| `com.kim.timemachine-backup` | inactif | Declencheur Time Machine |
| `com.kim.claude-memory` | inactif | Sauvegarde periodique memoire Claude |

Emplacement : `~/Library/LaunchAgents/com.kim.*.plist`

---

# CHAPITRE 34 -- COMMANDES CUSTOM (43 commandes dans ~/.claude/commands/)

## Commandes Maintenance

| Commande | Usage |
|----------|-------|
| `/clean-sys` | Nettoyage systeme (caches, logs, tmp) |
| `/disk-report` | Rapport espace disque detaille |
| `/update-sys` | Mise a jour systeme (brew, npm, pip) |
| `/process-mgr` | Gestion processus (list, kill, priority) |
| `/optimize` | Optimisation performances |

## Commandes Infrastructure

| Commande | Usage |
|----------|-------|
| `/infra` | Vue d'ensemble infrastructure (BOKADOR, VPS, Mac) |
| `/tunnel-check` | Verifier tunnels SSH actifs |
| `/wsl-init` | Initialiser WSL sur BOKADOR |
| `/deploy-check` | Pre-verification deploiement |
| `/deploy-staging` | Deployer en staging |
| `/vps-sec` | Securite VPS (fail2ban, ufw, logs) |
| `/ssl-check` | Verification certificats SSL |
| `/nginx-mgr` | Gestion Nginx |
| `/fail2ban-mgr` | Gestion Fail2Ban |

## Commandes Code

| Commande | Usage |
|----------|-------|
| `/refactor` | Refactoring code |
| `/code-review` | Review de code |
| `/smart-commit` | Commit intelligent (analyse diff) |
| `/test` | Lancer tests |
| `/fix` | Fix erreurs |
| `/commit` | Commit simple (Conventional Commits) |
| `/debug-error` | Debug erreur specifique |
| `/review` | Review complete |
| `/doc` | Generer documentation |
| `/plan` | Planifier une tache |

## Commandes Securite

| Commande | Usage |
|----------|-------|
| `/audit` | Audit securite rapide |
| `/alert` | Alerte securite |
| `/security-log` | Consulter logs securite |

## Commandes Jarvis (assistant IA)

| Commande | Usage |
|----------|-------|
| `/jarvis-status` | Etat Jarvis |
| `/jarvis-diag` | Diagnostic Jarvis |
| `/jarvis-fix` | Fix Jarvis |
| `/jarvis-deploy` | Deployer Jarvis |
| `/jarvis-watch` | Surveiller Jarvis |

## Commandes Memoire

| Commande | Usage |
|----------|-------|
| `/memorize` | Workflow auto-apprentissage complet |
| `/auto-learn` | Detection patterns et persistance |
| `/cloud-memory` | Compaction memoire (>100 lignes) |
| `/standup` | Charge contexte debut session |
| `/compact-report` | Rapport compaction contexte |
| `/thoughts` | Reflexions et decisions |
| `/spend` | Suivi depenses tokens/API |

## Commandes Hotel (Opera Cloud PMS)

| Commande | Usage |
|----------|-------|
| `/hotel` | Assistant hotel Mercure Montmartre H0373 |
| `/night-audit` | Rapport de releve reception + checklist audit |
| `/hotel breakfast` | Rapport petit-dejeuner (avant 6h) |
| `/hotel audit` | Checklist night audit, balances, no-shows |
| `/hotel hk` | Feuille de travail housekeeping |
| `/hotel consigne` | Rediger consigne Opera Cloud |

## Commandes Recherche

| Commande | Usage |
|----------|-------|
| `/research` | Recherche approfondie (web + docs) |
| `/backup-now` | Backup immediat |

---

# CHAPITRE 35 -- CONTENU FICHIERS MEMOIRE (BOOTSTRAP JARVIS)

## 01_architecture.md — Architecture 5 niveaux memoire

| Niveau | Scope | Persistence |
|--------|-------|-------------|
| 1. Ephemere | Conversation courante | Perdu au /clear |
| 2. Session | auto-memory projects/hash/memory.md | Persiste entre sessions |
| 3. Projet | CLAUDE.md racine projet | Stack, conventions |
| 4. Utilisateur | ~/.claude/rules/ + memory/ | Persiste entre projets |
| 5. Meta | rules/09-learned-*.md | Auto-genere par apprentissage |

Structure ~/.claude/ : CLAUDE.md (99L), settings.json (39 allow + 36 deny + 9 plugins + hooks + teams), 42 commands, 24 skills, 17+ agents, 10 memory files, 4 step files prompt discovery.

Machines synchronisees : VPS Kali (198/198 audit), MacBook M4 (synce 2026-03-18), BOKADOR (a synchroniser).

## 02_lessons_log.md — Lecons critiques

| Severite | Lecon |
|----------|-------|
| CRITICAL | Ne JAMAIS ecraser CLAUDE.md et settings.json — toujours lire et FUSIONNER |
| CRITICAL | Valider format exact settings.json avec python3 apres modification |
| WARNING | declare -A incompatible zsh/bash3 macOS — ne jamais utiliser |
| WARNING | defaultMode = bypassPermissions (avec S) — pas bypassPermission |
| INFO | MCP max 2-3 sinon contexte sature (~5% chacun) |
| INFO | Prompt discovery anti lost-in-the-middle (lire step juste avant execution) |
| INFO | Fichiers Cowork dans VM, pas ~/Downloads — utiliser present_files |

## 03_conventions.md — 6 conventions strictes

1. Fusion avant ecrasement (backup > lire > fusionner > valider JSON > verifier)
2. Scripts Mac compatibles (#!/bin/bash, pas declare -A, pas echo -e, lancer avec bash)
3. Trash obligatoire (alias t=trash, JAMAIS rm -rf)
4. Auto-apprentissage continu (/memorize, /auto-learn, /cloud-memory, /standup)
5. MCP minimal (max 2-3 actifs, verifier /context)
6. Workflow Melvynx (EPCT, /clear entre taches, workflow pour features complexes)

## 04_autonomy_matrix.md — 5 niveaux autonomie

| Niveau | Actions | Permission |
|--------|---------|------------|
| 1 | Lecture, diagnostic | Toujours autorise |
| 2 | Creer fichiers, modifier code, git add/commit | Autorise sans demander |
| 3 | Modifier configs serveur | Verifier + backup avant |
| 4 | Suppression, permissions, push/merge | Double confirmation |
| 5 | rm -rf, mots de passe, H:/G:, iCloud, LaunchAgents | INTERDIT sans autorisation |

## 05_reflection_loop.md — Boucle reflexion

ACTION > RESULTAT > REFLEXION > LECON > PERSISTANCE > AMELIORATION > retour ACTION

Declencheurs : erreur commande, correction user, pattern repetitif, fin tache complexe, debut session.
Anti-patterns : pas de boucle infinie (max 1 reflexion/action), pas de regle pour cas unique (attendre 2+).

## 07_security_bypass_guards.md — 12 principes securite bypass

1. User non-root, 2. Deny-list (87 patterns), 3. Hook PreToolUse guard, 4. Logs complets, 5. Workspace limite, 6. Sandbox/VM, 7. Pas de prod directe, 8. Chemins proteges, 9. Limites ressources, 10. Secrets interdits, 11. Regles CLAUDE.md, 12. Safe ops (test config avant reload)

ANOMALIE CONNUE : edit-logger.js reference dans settings.json PostToolUse mais ABSENT de ~/.claude/hooks/

---

# CHAPITRE 36 -- PROMPT DISCOVERY (Anti Lost-in-the-Middle)

## Concept

Les transformers donnent plus d'importance aux tokens du debut et de la fin. Solution : decouper en etapes, chaque etape lit son fichier juste avant d'agir.

## Fichiers step (~/.claude/step/)

| Fichier | Phase |
|---------|-------|
| `1_init.md` | Initialisation : charger contexte, definir objectifs |
| `2_explore_ultra.md` | Exploration ultra : sub-agents, codebase, docs, web |
| `3_plan.md` | Planification : plan detaille avec checkpoints |
| `4_implement.md` | Implementation : executer le plan, tester |

## Usage

Chaque workflow avance (apex, workflow-steps) lit automatiquement le step courant. Le prompt est toujours "recent" dans le contexte, donc bien suivi par le modele.

---

# CHAPITRE 37 -- PROXYMAN (Debug Contexte Claude Code)

## Concept

Proxyman intercepte les requetes HTTP de Claude Code vers l'API Anthropic. Permet de voir exactement ce qui est envoye au modele.

## Ce qu'on peut observer

| Element | Tokens approx |
|---------|---------------|
| System prompt | ~3 500 |
| Descriptions outils (tools) | Tres volumineux |
| Skills charges | Variable |
| MCP tools descriptions | ~5% par MCP |
| CLAUDE.md + rules | Variable |
| Conversation | Croissant |

## Installation

```bash
brew install --cask proxyman
```

Configurer le proxy SSL pour intercepter les requetes HTTPS de Claude Code.

## Quand l'utiliser

- Debug contexte sature (comprendre ce qui consomme les 27K tokens de demarrage)
- Verifier qu'un MCP ne surcharge pas le contexte
- Optimiser les skills (reduire leur taille token)

---

# CHAPITRE 38 -- OPERA CLOUD PMS & WORKFLOWS HOTELIER

## Contexte

Mercure Paris Montmartre (H0373). Night Auditor. Opera Cloud PMS = systeme de gestion hoteliere cloud Oracle.

## Commandes Claude Code pour l'hotellerie

| Commande | Declencheur | Actions |
|----------|------------|---------|
| `/night-audit` | Fin de shift nuit | Rapport releve, checklist audit, balances, no-shows |
| `/hotel breakfast` | Avant 6h | Rapport petit-dejeuner (couverts, observations) |
| `/hotel audit` | Night audit | Checklist complete, rapports gestion |
| `/hotel hk` | Matin | Feuille travail housekeeping (format Excel si possible) |
| `/hotel consigne` | A tout moment | Rediger consigne Opera Cloud |

## Workflow Night Audit typique

1. Verifier balances Opera Cloud (manuellement dans le PMS)
2. `/night-audit` : fournir en vrac les evenements → Claude formate le rapport
3. Identifier no-shows, litiges, VIP
4. Mettre en evidence actions pour le shift matin
5. `/hotel consigne` pour consignes specifiques

## Limites

Claude Code n'a PAS acces direct a Opera Cloud PMS. L'integration est manuelle : copier-coller des donnees du PMS vers Claude pour formatage et analyse.

---

# CHAPITRE 39 -- DISASTER RECOVERY COMPLET

## 39.1 DR MacBook Pro M4

### Sauvegardes existantes
- Time Machine (com.kim.timemachine-backup LaunchAgent)
- Config Claude : com.kim.claude-config-backup + GitHub repos (claude-config-mac, claude-code-bokador)
- Photos/Videos : script transfer vers BOKADOR H: (201 Go)
- iCloud : ~/Library/Mobile Documents (JAMAIS toucher)

### Procedure restauration complete
1. Restaurer depuis Time Machine (priorite 1)
2. Si Time Machine indisponible : reinstaller macOS, puis `curl -fsSL https://claude.ai/install.sh | bash`
3. Cloner config : `gh repo clone KimGemRuby/claude-config-mac ~/.claude`
4. Reinstaller Homebrew : `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
5. `brew install node bun tmux terminal-notifier`
6. Restaurer LaunchAgents depuis backup
7. Tester SSH vers BOKADOR et VPS
8. `/standup` pour charger le contexte

### RPO/RTO
- RPO : 1 jour (Time Machine hourly + iCloud continu)
- RTO : 2-4 heures (reinstallation complete)

## 39.2 DR BOKADOR

### Sauvegardes
- Disques H: et G: = stockage primaire (40TB)
- Config Claude : GitHub repo claude-code-bokador
- WSL Ubuntu-24.04 : config dans /home/kim13/

### Procedure restauration
1. Si Windows OK mais config Claude perdue : `gh repo clone KimGemRuby/claude-code-bokador`
2. Si Windows reinstalle : reinstaller SSH (OpenSSH Server), WSL Ubuntu-24.04, rsync
3. Reconfigurer ports SSH (interne 2222 et 22222)
4. Verifier NAT Bbox (regles port 443, 47281, etc.)
5. Tester : `ssh -p 47281 kim13@31.35.20.184`

### RPO/RTO
- RPO : variable (derniere sync GitHub)
- RTO : 4-8 heures (reinstallation Windows + config)

## 39.3 DR VPS Kali (109.176.197.139)

### Sauvegardes
- GitHub repo vps-backup (config complete)
- Snapshots Hostinger (si actives)

### Procedure restauration
1. Console Hostinger : rebuild VPS Ubuntu/Kali
2. `ssh root@<nouvelle-IP>` (si IP change, mettre a jour NAT Bbox regles 9, 10, 12)
3. Cloner config : `gh repo clone KimGemRuby/vps-backup`
4. Reinstaller : Claude Code, fail2ban, ufw, Docker
5. Reconfigurer SSH hardening (cles, port, fail2ban)
6. Whitelist IPs (176.133.20.227, 185.208.60.2, 31.35.20.184)
7. Tester depuis Mac : `ssh root@109.176.197.139`

### RPO/RTO
- RPO : derniere sync GitHub
- RTO : 1-2 heures (VPS rapide a reconstruire)

## 39.4 Test DR trimestriel

Checklist a executer tous les 3 mois :
- [ ] Verifier Time Machine fonctionne (derniere sauvegarde <24h)
- [ ] Verifier repos GitHub a jour (claude-config-mac, claude-code-bokador, vps-backup)
- [ ] Tester restauration config Claude depuis GitHub sur machine de test
- [ ] Verifier snapshots VPS Hostinger
- [ ] Tester Wake-on-LAN BOKADOR depuis VPS
- [ ] Verifier que tous les LaunchAgents sont actifs
- [ ] Verifier SSH vers toutes les machines
- [ ] Documenter tout changement dans la Bible

---

---
---

# CHAPITRE 40 -- ALIAS SHELL & RACCOURCIS (zshrc)

## Alias Claude Code

| Alias | Commande | Usage |
|-------|----------|-------|
| `cc` | `cd ~/Developer/CC && claude` | Lancer Claude Code dans le dossier CC |
| `ccs` | `~/.claude/scripts/claude-session.sh` ou tmux fallback | Session Claude avec tmux |
| `ccdir` | `cd ~/.claude && ls -la` | Explorer dossier config Claude |
| `ccrules` | `cat ~/.claude/rules/*.md` | Afficher toutes les rules |
| `ccbackup` | `bash ~/.claude/scripts/auto-backup.sh manual` | Backup manuel config |
| `god` | `claude --dangerously-skip-permissions` | Mode bypass total |
| `claude-yolo` | `claude --dangerously-skip-permissions` | Alias alternatif bypass |

## Alias SSH Machines

| Alias | Commande | Usage |
|-------|----------|-------|
| `cc-vps` | `ssh root@109.176.197.139` | SSH direct VPS |
| `cc-win` | `ssh -p 47281 kim13@31.35.20.184` | SSH direct BOKADOR |
| `cc-audit-vps` | SSH + `bash ~/.claude/scripts/status.sh` | Audit VPS distant |
| `cc-audit-win` | SSH + PowerShell audit script | Audit BOKADOR distant |
| `cc-backup-full` | tar+scp VPS→BOKADOR | Backup complet cross-machine |
| `vps` | `ssh vps-kim` | SSH VPS via profil SSH |

## Alias Systeme

| Alias | Commande | Usage |
|-------|----------|-------|
| `t` | `trash` | Suppression securisee (JAMAIS rm) |
| `ll` | `ls -lah --color=auto` | Liste detaillee |
| `..` | `cd ..` | Remontee rapide |
| `...` | `cd ../..` | Double remontee |

---

# CHAPITRE 41 -- ARCHITECTURE SKILL+SUBAGENT+WORKFLOW (Pattern Melvynx)

## Schema hierarchique

```
Skill (englobe tout)
  |
  |-- SubAgents (supportent le skill)
  |     |-- web-search (WebSearch + WebFetch + Exa)
  |     |-- explore-codebase (Read, Grep, Glob, Search)
  |     |-- explore-doc (Context7 MCP)
  |
  |-- Steps/ (prompt discovery anti lost-in-the-middle)
  |     |-- 01-init.md (initialiser parametres)
  |     |-- 02-analyse.md (analyser erreur/feature)
  |     |-- 03-plan.md (planifier solutions)
  |     |-- 04-execute.md (executer plan)
  |     |-- 05-verify.md (verifier resultat)
  |
  |-- Parametres (--plan, --teams, --auto, --branch, --pr, --economy, --resume)
```

## Principes

- On OBLIGE l'IA a suivre un workflow specifique
- Les sub-agents supportent le skill (sous le skill)
- Le skill englobe tout
- Chaque step est lue JUSTE AVANT execution (anti lost-in-the-middle)
- La description du sub-agent est critique (decide quand l'IA l'utilise)
- Forcer l'utilisation des sub-agents via le workflow (sinon l'IA peut ne pas les appeler)

## Meta-Prompting

Ne JAMAIS ecrire un prompt a la main. Utiliser les meta-skills :
- `/prompt-creator` : genere prompts optimises (utilise anthropic-best-practices.md)
- `/skill-creator` : genere skills
- `/subagent-creator` : genere sub-agents
- `/meta-prompt-creator` : genere prompts qui generent des prompts

Le fichier cle est `anthropic-best-practices.md` dans le skill, contenant anti-patterns, clarity principle, context management, few-shot pattern.

---

# CHAPITRE 42 -- CLAUDE REVIEW (Beta Entreprise)

## Concept

Review automatique des PR avec processus multi-agents en parallele.

## Resultats apres mois de tests

- PR avec reviews passent de 16% a 54%
- Moins de 1% des reviews marquees incorrectes
- 94% des bugs trouves sur les grandes PR

## Cout

15-25$ par review (token usage, augmente avec complexite). Comparaison : ingenieur mid San Francisco = 77-96$/h, senior = 100-150$/h.

## Optimise pour

Equipes, pas individuel. Activer quand on travaille en equipe sur des PR complexes.

---

# CHAPITRE 43 -- ANOMALIES CONNUES & CORRECTIONS

| Anomalie | Severite | Status | Action |
|----------|----------|--------|--------|
| edit-logger.js reference dans settings.json PostToolUse mais ABSENT de ~/.claude/hooks/ | WARNING | Non corrige | Creer le fichier ou retirer la reference |
| settings.json protege par macOS App Management (EPERM sur write) | INFO | Contournement | Creer .new puis `mv` manuellement |
| defaultMode = bypassPermissions (avec S final) et non bypassPermission | WARNING | Corrige | Verifier orthographe apres chaque modif |
| declare -A incompatible zsh/bash3 macOS | WARNING | Documente | Ne jamais utiliser dans scripts Mac |
| echo -e non portable sur macOS | INFO | Documente | Utiliser printf a la place |
| settings.local.json contient des allow tres specifiques (debug Telegram) | INFO | Normal | Residus de session debug, nettoyer si besoin |
| Bbox IP publique dynamique (31.35.20.184) | WARNING | Surveiller | Peut changer apres reboot Bbox, impacte toutes les regles NAT |
| Whisper large-v3 OOM sur VM Cowork | INFO | Non resolvable | VM a 2.2GB free, utiliser yt-dlp auto-subs |

---

# CHAPITRE 44 -- CLAUDE.md GLOBAL REFERENCE

Reproduction de reference du fichier `~/.claude/CLAUDE.md` (MacBook Pro M4) :

## Identite
- Night Auditor & SysAdmin — Mercure Paris Montmartre H0373 (Opera Cloud PMS)
- Infrastructure : MacBook Pro M4 24GB, BOKADOR (RTX 3060, 40TB H:), VPS Kali 109.176.197.139
- Claude Code deploye sur 4 machines

## Langue
- Francais sauf si j'ecris en anglais. Direct et concis, zero blabla.

## Machine courante
- MacBook Pro M4 24GB RAM, 1TB SSD, user kim13karame.com
- SSH BOKADOR : `ssh -p 47281 kim13@31.35.20.184`
- SSH VPS : `ssh root@109.176.197.139`
- Sons : afplay /System/Library/Sounds/Glass.aiff (fin de tache)
- Watchdog : com.kim.claude-watchdog

## Regles Absolues (CRITICAL)
1. (#0) Documenter dans GitHub AVANT toute modification
2. (#1) Triple backup avant suppression (trash + knowledge graph + GitHub)
3. JAMAIS rm -rf — utiliser trash
4. JAMAIS supprimer sans backup confirme (SHA256 + chemin + confirmation)
5. Double confirmation avant suppression ou ecrasement
6. JAMAIS toucher H: et G: BOKADOR
7. JAMAIS toucher ~/Library/Mobile Documents (iCloud)
8. JAMAIS supprimer LaunchAgents actifs (com.kim.*)
9. Scripts idempotents obligatoires
10. Diagnostiquer avant agir (logs, processus, espace, permissions)
11. Backup/snapshot avant operation risquee
12. JAMAIS commencer un script par # seul

## IPs de confiance
185.208.60.2 (travail), 176.133.20.227 (domicile), 109.176.197.139 (VPS), 31.35.20.184 (BOKADOR), 92.92.127.0/23 (plage Kim), 127.0.0.0/8 (localhost)

## Ports SSH
- VPS : 22, BOKADOR : 47281, Mac : 1983

## Commandes interdites
rm -rf, sudo rm, chmod 777, git push --force, netcfg -d, curl|bash, wget|bash

## Workflow
- Lire 3 fichiers similaires avant de modifier
- Erreurs TypeScript non implementees : passer a la suite
- Changelog apres tout changement
- Tester via SSH avant de poser une question — agir directement
- Indiquer cout en $ quand tokens mentionnes

## Preferences Code
- 2 spaces JS/TS/JSON, 4 spaces Python
- Single quotes, trailing commas, semicolons
- pnpm preferred, npm fallback
- Conventional Commits (feat:, fix:, refactor:, test:, ci:)
- TDD when possible

## Auto-Apprentissage
- Correction → /memorize auto
- Echec 2+ fois → creer rule
- Preference → rules/11 + knowledge graph
- Detection : corrections, preferences, patterns, decisions, workflows implicites
- Persistance triple : rules/ + knowledge graph + rules specialisees

## Bootstrap Jarvis (OBLIGATOIRE)
Avant CHAQUE action, lire silencieusement : memory/01, 02, 03.

## Protocole Melvynx Diamant
- Son fin de tache : afplay Glass.aiff
- /master-audit avant intervention majeure
- Workflow : Explore > Plan > Code > Test

---

---
---

# CHAPITRE 45 -- PROFILS SSH (~/.ssh/config)

```
Host windows-rdp
    HostName 176.133.20.227
    User kim13
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
    LocalForward 3390 localhost:3389

Host vps-kim
    HostName 31.35.20.184
    User kim13
    Port 443
    LocalForward 13389 localhost:3389

Host vps
    HostName 109.176.197.139
    User root
    ServerAliveInterval 60
    ServerAliveCountMax 3

Host hostinger
    HostName 109.176.197.139
    Port 443
    User root
    SetEnv TERM=xterm-256color

Host bokador
    AddressFamily inet
    HostName 31.35.20.184
    Port 443
    User kim13
    LocalForward 13389 localhost:3389
```

## Usage rapide

| Commande | Destination |
|----------|-------------|
| `ssh vps` | VPS Kali (root, keepalive 60s) |
| `ssh hostinger` | VPS via port 443 |
| `ssh bokador` | BOKADOR via port 443 + tunnel RDP |
| `ssh vps-kim` | BOKADOR via port 443 (nom legacy) |
| `ssh windows-rdp` | PC via domicile + tunnel RDP |

---

# CHAPITRE 46 -- METHODE MELVYNX COMPLETE (20 sections, memory/09)

Reference fusionnee de toutes les transcriptions Melvynx. Contenu cle non couvert ailleurs :

## Philosophie Vibe Coding
- Developpeur = chef d'orchestre de l'IA, pas ecrivain de code
- Prompting > coding : la competence cle est de bien communiquer avec l'IA
- Ne JAMAIS ecrire de prompts a la main → meta-prompting obligatoire

## Template CLAUDE.md Melvynx (structure recommandee)
Role > Workflow > Contexte Projet > Memoire (4 niveaux) > Securite > macOS/Systeme > MCP Servers > Sous-Agents > Prompting > Qualite > Git > Plugins & Hooks > Checklists > Anti-patterns

REGLE : CLAUDE.md ne doit JAMAIS depasser 500 lignes (anti-pattern mega-CLAUDE.md)

## Principe de protection du contexte (sub-agents)
- Ne JAMAIS laisser l'agent principal faire de la recherche web ou lire de longues docs
- Le sub-agent consomme des dizaines de milliers de tokens de son cote
- Il ne renvoie qu'un resume ultra-condense (1500 mots au lieu de 120 000)
- Protege le contexte principal de la pollution

## Clear Context entre planification et execution
- Separer la phase de planification de la phase d'execution
- Plan valide → /clear ou /compact → Execution
- Repartir sur base de tokens allegee pour l'implementation

## Memoire modulaire chirurgicale
- Refuser de polluer le CLAUDE.md global avec des instructions temporaires
- Utiliser `.claude/rules/` avec regles contextuelles chargees conditionnellement
- Chaque regle ne se charge que quand elle est pertinente (globs)

## MCP limitation stricte
- Limiter a 2 MCP essentiels : Context7 + Exa
- Parametre `mcp.autoSearchThreshold: 10%` — si MCP prennent >10% memoire, outil de recherche optimise active
- Plus de MCP = plus de latence + contexte gaspille

## Workflows par taille
| Taille | Lignes | Approche |
|--------|--------|----------|
| XS | <50 | Direct, pas de plan |
| S | 50-200 | Plan simple, un agent |
| M | 200-1000 | EPCT complet |
| L | 1000+ | Equipes multi-agents, worktrees |

## Correspondance config kim13
Deja en place : CLAUDE.md 13 regles CRITICAL, settings.json 39 allow + 36 deny + 4 hooks + 9 plugins, 27+ skills, auto-backup 3 couches, delete guard, firewall JARVIS, 16+ MCP, aliases zsh, memory files, step files, Conventional Commits.

A corriger : edit-logger.js fantome, bypass-guards a fusionner.

---

# CHAPITRE 47 -- SKILLS COWORK (12 skills VM)

Skills disponibles dans l'environnement Cowork (VM Linux legere, differents des skills MacBook) :

| Skill | Declencheur |
|-------|------------|
| `schedule` | Taches planifiees (cron on-demand ou intervalle) |
| `xlsx` | Tout fichier spreadsheet (.xlsx, .csv, .tsv) |
| `pdf` | Tout fichier PDF (extraction, merge, split, formulaires, OCR) |
| `pptx` | Tout fichier PowerPoint (creation, edition, extraction) |
| `docx` | Tout document Word (creation, edition, TOC, images) |
| `kim13-terminal` | Memoire multi-niveaux kim13 (profil, preferences, knowledge graph) |
| `skill-creator` | Creer, modifier, optimiser des skills + evals |
| `security-hardening` | Hardening serveur, audit, OWASP, firewall, SSL, fail2ban |
| `monitoring-alerting` | Prometheus, Grafana, alerting, dashboards, SLO/SLI |
| `sysadmin-devops` | IaC, CI/CD, Docker, K8s, Ansible, Terraform, scripts Bash |
| `cowork-plugin-customizer` | Personnaliser un plugin Claude Code |
| `create-cowork-plugin` | Creer un plugin from scratch |

---

# CHAPITRE 48 -- ANTI-PATTERNS COMPLETS (18 patterns)

| # | Anti-Pattern | Pourquoi c'est mauvais | Alternative |
|---|-------------|----------------------|-------------|
| 1 | Mega-CLAUDE.md > 500 lignes | Pollue le contexte a chaque session | Deplacer dans rules/ avec globs |
| 2 | Trop de MCP (>3) | Latence + contexte gaspille | Max 2-3, toolSearchMode: auto |
| 3 | Hooks bloquants | Ralentissent toute execution | async: true |
| 4 | Context stuffing | Infos inutiles dans CLAUDE.md | Memoire modulaire chirurgicale |
| 5 | Pas de tests apres modifs | Regressions silencieuses | /test ou --test flag |
| 6 | Sessions >50% sans /compact | Qualite degradee, lost-in-the-middle | /compact ou /clear |
| 7 | Ecrire prompts a la main | Sous-optimal, pas structure | Meta-prompting (/prompt-creator) |
| 8 | Agents teams zones chevauchees | Agents se marchent dessus | Un agent par zone separee |
| 9 | Force push sur main | Perte historique | PR + merge |
| 10 | rm au lieu de trash | Perte irreversible | alias t=trash |
| 11 | Ecraser configs existantes | Perte config | TOUJOURS fusionner (merge) |
| 12 | /batch multi-PR | Trop de PR a review | Main agent + sub-agents |
| 13 | Taguer fichiers avec @ (Apex) | L'exploration les trouve mieux | Laisser exploration auto |
| 14 | Empiler demandes pendant travail IA | Perturbe l'agent | Attendre fin de tache |
| 15 | Agent principal fait recherche web | Pollue contexte (28K+ tokens) | Sub-agent web-search |
| 16 | declare -A dans scripts Mac | Incompatible zsh/bash3 macOS | Fonctions simples |
| 17 | echo -e dans scripts Mac | Non portable | printf |
| 18 | Lancer Claude Code a la racine (/) | Acces systeme complet non desire | Toujours cd dans un projet |

---

# CHAPITRE 49 -- CHECKLISTS PRE/POST SESSION

## Pre-session

- [ ] `git status` propre
- [ ] CLAUDE.md a jour
- [ ] Branche correcte
- [ ] Contexte libre (pas de session precedente polluante)
- [ ] `/standup` pour charger le contexte
- [ ] Bootstrap Jarvis : lire memory/01, 02, 03
- [ ] Verifier `/context` < 10%

## Post-session

- [ ] Tests passent
- [ ] `git status` propre
- [ ] Docs a jour si API modifiee
- [ ] `/compact` ou `/clear` si session longue
- [ ] `/auto-learn` pour persister les patterns
- [ ] Changelog a jour
- [ ] Knowledge graph mis a jour si nouvelles decisions

## Pre-intervention majeure

- [ ] `/master-audit` avant intervention
- [ ] Backup config : `ccbackup`
- [ ] Verifier SSH vers toutes les machines
- [ ] Documenter dans GitHub AVANT (regle #0)

---

# CHAPITRE 50 -- FLAGS CLI AVANCES CLAUDE CODE

| Flag | Usage |
|------|-------|
| `--dangerously-skip-permissions` | Bypass toutes les confirmations (alias: god) |
| `--model claude-opus-4-6` | Forcer un modele specifique |
| `--allowedTools Bash,Read,Write` | Limiter les outils disponibles |
| `--output-format json` | Sortie JSON pour CI/CD |
| `-p "question"` / `--print` | One-shot sans boucle agentique |
| `--resume` | Reprendre une session precedente |
| `--continue` | Continuer la derniere conversation |
| `SANDBOX=1 claude` | Forcer mode sandbox (bypass permission sur VPS) |

---

---
---

# CHAPITRE 51 -- SCRIPTS DEPLOYES (VPS + Mac)

## Scripts VPS Kali (109.176.197.139)

| Script | Lignes | Role |
|--------|--------|------|
| `setup-claude-code-vps-kali.sh` | 756 | Setup initial complet (Node, CC, CLAUDE.md, settings, rules, agents, SSH, UFW, fail2ban, sysctl, tmux) |
| `restore-merge-claude-code.sh` | 577 | Restauration configs ecrasees + fusion sans perte |
| `audit-claude-code-vps.sh` | 666 | Audit 17 sections, 199 verifications |
| `fix-melvynx-gaps.sh` | 358 | MCP 4→2, workflow steps, logging actif, nettoyage auto |
| `setup-autolearn-system.sh` | 501 | Boucle auto-apprentissage complete |
| `sync-memory-vps.sh` | — | Memorisation lecons + conventions dans memory/ |

## Script Mac

| Script | Lignes | Role |
|--------|--------|------|
| `sync-mac-from-vps.sh` | 730 | Sync Mac ← VPS (CLAUDE.md, settings, rules, agents, skills, commands, memory, step, tmux, aliases) |

## Hooks VPS (differents des hooks Mac)

| Hook | Role |
|------|------|
| `reflection-loop.sh` | Boucle reflexion post-action |
| `auto-learn.sh` | Auto-apprentissage automatique |
| `jarvis-firewall.js` | Firewall commandes Jarvis |
| `notify.js` | Notifications |
| `validate_bash.js` | Validation commandes Bash |

## Scores d'audit VPS

| Audit | Score | Details |
|-------|-------|---------|
| Audit 1 (initial) | 197/197 PASS, 0 FAIL | Avant fix Melvynx |
| Audit 2 (post-fix) | 198/198 PASS, 0 FAIL, 0 WARN | Apres fix + autolearn |
| Score Melvynx | Structure 100%, Securite 150%, Workflows 95%, Auto-learning 100% | Evaluation qualite config |

## Backups (~/.claude/backups/)

| Dossier | Date/Contenu |
|---------|-------------|
| 20260318 | Premiere session Cowork |
| 20260322 | Session 22 mars |
| 20260323 | Session 23 mars |
| 20260323-032420-before-melvynx-merge | Avant merge Melvynx |
| 20260323-audit | Apres audit |
| 20260323-session2 | Session 2 du 23 |
| 20260326 | Session courante |
| claude-config | Config complete |
| claude-melvynx-macos | Config Melvynx Mac |
| melvynx-memory-system | Systeme memoire Melvynx |

---

# CHAPITRE 52 -- ETAT SYSTEME & TACHES RESTANTES

## Etat courant (2026-03-26)

| Element | Valeur |
|---------|--------|
| Claude Code version | v2.1.76 |
| MacBook | Operationnel |
| VPS Kali | Operationnel, audit 198/198 |
| BOKADOR | Operationnel, SSH OK |
| Dernier sync Mac←VPS | 2026-03-18 |
| Bible version | 5.0+ (52 chapitres, ~2100 lignes) |

## Taches restantes (non completees)

| Priorite | Tache | Machine |
|----------|-------|---------|
| HAUTE | Migration iCloudDrive 525Go restants vers H: | Mac→BOKADOR |
| HAUTE | Lancer transfer_media_to_bokador.py (11632 fichiers, 201Go) | Mac→BOKADOR |
| MOYENNE | Augmenter MaxStartups SSH sur BOKADOR | BOKADOR |
| MOYENNE | Configurer mcp.json sur Mac (Context7 + Exa) | Mac |
| MOYENNE | Corriger edit-logger.js fantome dans settings.json | Mac |
| BASSE | mv ~/.claude/settings.json.new ~/.claude/settings.json (hooks notification) | Mac |
| BASSE | Changer iTerm de "Alerts" a "Banners" (System Settings > Notifications) | Mac |
| BASSE | Synchroniser config BOKADOR avec architecture Mac/VPS | BOKADOR |

## Documents crees lors des sessions precedentes

| Document | Taille | Contenu |
|----------|--------|---------|
| Guide-Claude-Code-DevOps.docx | 26.8KB, 561 paragraphes | Checklist 22 sections (4 masterclass Melvynx, ~12700 lignes → 200 instructions) |
| transcription_youtube.txt | 446 lignes | Transcription video "Claude Code fonctionne 100x mieux" |
| transfer_media_to_bokador.py | v4 | Script rsync Mac→BOKADOR (11632 fichiers, 201Go) |
| BIBLE_CLAUDE_CODE_DEVOPS_SYSADMIN.md | 2100+ lignes | Ce document |

---

**FIN DE LA BIBLE COMPLETE -- Version 6.0 -- 52 chapitres -- Derniere mise a jour : 2026-03-26**
