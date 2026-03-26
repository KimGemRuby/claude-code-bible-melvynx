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

**FIN DE LA BIBLE -- Derniere mise a jour : 2026-03-26**
