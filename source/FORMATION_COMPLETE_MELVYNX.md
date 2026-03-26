# FORMATION COMPLETE MELVYNX -- Claude Code & OpenClaw
## Synthese exhaustive de 16 fichiers (14 283 lignes)
### Compilee le 2026-03-25

**Sources** : 6 videos Melvynx (transcriptions) couvrant Claude Code de A a Z + OpenClaw

---

# TABLE DES MATIERES

- **PARTIE 1** -- Les Fondamentaux (Terminal, Installation, Interface)
- **PARTIE 2** -- Systeme de Memoire (CLAUDE.md, Rules, Settings)
- **PARTIE 3** -- Outils Avances (Skills, Hooks, MCP, Sub-agents)
- **PARTIE 4** -- Workflows Pro (Apex, OneShot, Debug, Brainstorm)
- **PARTIE 5** -- Gestion du Contexte et Anti-patterns
- **PARTIE 6** -- Prompting Concret (Greenfield, Brownfield, Debug UI)
- **PARTIE 7** -- Multi-agents (Teams, Work Trees, Tmux)
- **PARTIE 8** -- Configuration et Pricing
- **PARTIE 9** -- OpenClaw (Agent IA distant via Telegram)
- **PARTIE 10** -- Complements et Adaptation a ton Infrastructure

---

# PARTIE 1 -- LES FONDAMENTAUX

## 1.1 Le Terminal pour Vibe Codeurs

Le terminal est un des **piliers du Vibe Coder**. On n'a plus besoin de savoir faire du HTML/CSS/JS, mais maitriser le terminal est essentiel.

### Concepts de base

- Le terminal est une page qui permet d'acceder au systeme de commande de l'ordinateur
- Chaque dossier a un **chemin** (path) -- on le voit avec `pwd`
- Le caractere `~` (tilde) represente le dossier utilisateur (home)
- Le terminal peut se "deplacer" dans les dossiers comme le Finder/Explorer

### Commandes essentielles

| Commande | Action |
|----------|--------|
| `echo "texte"` | Affiche du texte |
| `ls` | Liste le contenu du dossier courant |
| `pwd` | Affiche le chemin complet du dossier courant |
| `cd dossier` | Se deplacer dans un dossier |
| `cd ..` | Revenir au dossier parent |
| `cd` (seul) | Revenir a la racine utilisateur (~) |
| `mkdir nom` | Creer un dossier |
| `open .` | Ouvrir le dossier dans Finder (macOS) |
| `code .` | Ouvrir le dossier dans VS Code |
| `clear` | Nettoyer l'affichage du terminal |

### Bash vs PowerShell

- **Bash/ZSH** : utilise sur macOS et Linux -- c'est ce qu'il faut apprendre
- **PowerShell** : utilise sur Windows -- different de Bash
- **WSL** (Windows Subsystem for Linux) : pont qui permet d'utiliser Bash sur Windows
- Recommandation : sur Windows, installer WSL pour avoir Bash

### Terminaux recommandes

- **Terminal** : app par defaut macOS
- **Ghosty** (ghosty.org) : terminal optimise, gratuit, raccourcis pratiques (`Cmd+1/2/3` pour tabs, `Cmd+W` fermer, `Cmd+T` nouveau tab)
- **Hyper** (hyterm) : alternative populaire
- **CEMUX** : terminal cree pour les coding agents (4.7 stars GitHub, build au-dessus de Ghosty, tabs verticaux + horizontaux, notifications quand un agent a besoin d'attention, browser integre, split pane, scriptable, build en Swift/AppKit, pas Electron, gratuit)

### Outils macOS recommandes par Melvynx

- **Raycast** : remplacant de Spotlight (gratuit sauf IA) -- app launcher, clipboard history, emoji picker, window management, screenshot. Extensions : VS Code, Figma, GitHub, Tailwind, iOS. Remplace `Cmd+Espace`
- **Homebrew** : gestionnaire de paquets macOS (`brew install <paquet>`)
- **CleanShotX** : screenshots annotes avec dessins, fleches, carres rouges (disponible dans SetApp)
- **SetApp** : bundle d'outils macOS incluant CleanShotX et d'autres
- **WhisperTurbo** : app de Melvynx, speech-to-text local gratuit et open source
  - Modele **PRK (Parakeet)** : rapide, leger, bon pour enregistrements courts (<20s)
  - Modele **Whisper Large** : meilleur pour les longs enregistrements et le francais

### Visual Studio Code

- Telecharger sur code.visualstudio.com
- Installer la commande CLI : `Cmd+Shift+P` > "Shell Command: Install code in PATH"
- Terminal integre : menu Terminal > New Terminal
- Ouvrir un projet : `code .` dans le terminal

### Creer un premier projet (exemple complet)

```bash
npm create vite@latest     # creer un projet (choisir React, TypeScript)
cd mon-premier-projet      # entrer dans le dossier
npm install                # installer les dependances
npm run dev                # lancer le serveur (localhost:5173)
```

- `npm` est installe automatiquement avec Node.js
- Le serveur tourne dans le terminal -- `Ctrl+C` pour l'arreter
- C'est typiquement ce genre de commandes que Claude Code execute pour vous

### Astuce copie de chemin (macOS)

- Dans le Finder, clic droit sur un dossier > maintenir `Option` > "Copier comme chemin"
- Puis dans le terminal : `cd <coller le chemin>`

---

## 1.2 Installation de Claude Code

### Pre-requis

- **Node.js** : installer depuis nodejs.org (commande d'installation fournie sur la page "Get Node.js")
- **Homebrew** (macOS) : `brew install` pour gerer les paquets
- **VS Code** : recommande mais pas obligatoire

### Installation

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# Windows PowerShell (commande dediee sur la doc)
# Voir code.claude.com/doc/setup
```

- Apres installation, relancer le terminal si `claude` ne fonctionne pas
- Verifier avec : `claude` puis taper "qui suis-je ?"

### Regles fondamentales

- **JAMAIS lancer Claude Code a la racine du systeme** (/)
- Toujours se placer dans un dossier projet : `cd /chemin/vers/projet`
- Sur macOS : clic droit + Option pour copier le chemin complet d'un dossier

---

## 1.3 Interface Claude Code

### Caracteres speciaux dans le prompt

| Prefixe | Usage | Exemple |
|---------|-------|---------|
| `@` | Taguer un fichier | `@header` pour src/components/header |
| `/` | Commandes slash | `/stats`, `/clear`, `/model` |
| `!` | Mode bash direct | `!echo salut`, `!pnpm install` |

### Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `Escape Escape` | Clear texte, puis acces historique conversations |
| `Shift+Tab` | Changer mode permissions (Normal, Accept Edit, Bypass) |
| `Ctrl+T` | Afficher les taches en cours |
| `Ctrl+O` | Transcript verbose |
| `Ctrl+S` | Sauvegarder le prompt en cours (detail ci-dessous) |
| `Meta+P` | Changer de modele |

**Ctrl+S en detail** : Si tu ecris un long prompt et veux poser une question intermediaire, `Ctrl+S` sauvegarde ton prompt. Tu poses ta question, Claude repond, et le prompt sauvegarde revient automatiquement dans le champ de saisie.

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
| `/voice` | Mode vocal (beta, mieux en anglais) |
| `/loop` | Tache recurrente a intervalle |
| `/schedule` | Taches planifiees (cron persistant) |
| `/simplify` | Review et simplification du code (lance 3 agents) |
| `/copy` | Copier la derniere reponse en Markdown |
| `/config` | Configuration (tips, suggestions, progress bar) |
| `/batch` | Lance des agents en parallele avec work trees (anti-pattern, voir ci-dessous) |

### Historique des conversations

- `Escape Escape` > naviguer avec les fleches > `Enter` > "Restore conversation"

---

## 1.4 Architecture de Claude Code

### Trois produits Anthropic

1. **Claude Chat** : interface web, one-shot (question/reponse)
2. **Claude API** : acces direct au modele, one-shot par defaut
3. **Claude Code** : agent agentique dans le terminal, boucle autonome

### Fonctionnement agentique

- Claude Code = logiciel qui donne des **outils** au modele (analogie : donner une cuisine a un chef)
- Outils disponibles : Bash, Edit, Multi Edit, Read, Write, List, Web Fetch, Web Search, ToDo, + MCP
- Boucle : recuperer infos > planifier > agir > verifier > repeter jusqu'a completion
- Le modele decide, Claude Code execute

### Exemple concret (suppression de commentaires)

1. Modele appelle Read File
2. Claude Code retourne le contenu
3. Modele appelle Edit File avec le contenu modifie
4. Claude Code ecrit le fichier
5. Modele confirme la fin

### Premier projet demo : Tax Calculator

Melvynx cree en live un simulateur de taxes auto-entrepreneur :
- Prompt detaille avec stack (Vite + React), librairie UI (ShadCN), recherches web prealables sur les taxes
- Claude lance des sub-agents automatiquement pour la recherche web
- Resultat : application fonctionnelle en quelques minutes sur `localhost:5173`

### Applications creees par Melvynx avec Claude Code

- **Umail EO** : outil de newsletter concurrent a MailChimp (2M+ emails envoyes)
- **SaveIt.Now** : gestionnaire de signets avec IA (screenshot + analyse)
- **Subfast** : generateur de miniatures YouTube
- **Chao.App** : chatbot integrable sur site, cree SANS ecrire une seule ligne de code
- **PaddleTally** : app iOS + Apple Watch pour compter les points au Paddle (jamais code iOS/Watch avant)
- Capable de travailler sur des codebases existantes de 4+ ans

---

# PARTIE 2 -- SYSTEME DE MEMOIRE

## 2.1 Les trois niveaux de CLAUDE.md

| Niveau | Emplacement | Portee |
|--------|-------------|--------|
| **Global** | `~/.claude/CLAUDE.md` | TOUTES les conversations, tous les projets |
| **Projet** | `./CLAUDE.md` (racine du projet) | Ce projet uniquement |
| **Sous-dossier** | `./src/components/CLAUDE.md` | Charge uniquement quand l'IA lit un fichier de ce dossier |

### Principe fondamental

Chaque CLAUDE.md est automatiquement injecte dans le contexte. Donc :
- Chaque information doit etre **fondamentalement utile**
- Ne pas raconter sa vie, ne pas surcharger
- Si le fichier est trop gros (>200 lignes) : deplacer des blocs dans `.claude/rules/`

### Initialisation

- `/init` dans une conversation existante : genere le CLAUDE.md du projet en explorant la codebase
- **Conseil** : lancer `/init` dans un thread ou des recherches ont deja ete faites (contexte enrichi)

---

## 2.2 Les Rules (`.claude/rules/`)

- Fichiers `.md` dans `.claude/rules/` charges automatiquement a chaque session
- Permettent de decharger le CLAUDE.md principal

### Rules ciblees avec globs

On peut specifier qu'une regle ne s'applique qu'a certains types de fichiers :

```markdown
# Fichier: .claude/rules/json-rules.md
# En-tete avec pattern glob: *.json

Regles specifiques pour les fichiers JSON...
```

La regle n'est chargee que lors de la lecture de fichiers correspondant au pattern.

### Bonnes pratiques

- Si Claude fait une erreur 2+ fois : creer une regle
- Garder les regles COURTES et PRECISES
- Utiliser `CRITICAL:` pour les regles les plus importantes
- Exemple : "Tu as fait cette erreur trop souvent. Cree un fichier dans .claude/rules/ qui t'empechera de la refaire."
- Pour synchroniser : "synchronise les regles avec ce qu'on a actuellement dans le projet"

---

## 2.3 Settings et Permissions

### Fichier `settings.json`

Emplacement : `~/.claude/settings.json`

### Mode Bypass Permission (recommande par Melvynx)

```json
{
  "defaultMode": "bypassPermission"
}
```

- Supprime TOUTES les demandes de validation
- L'IA travaille de maniere 100% autonome
- **MAIS** : ajouter une deny-list pour les commandes dangereuses

### Deny-list

- Meme en bypass, les commandes en deny sont bloquees
- Erreur affichee : "Permission to use bash with command rm -rf denied"

### Double securite

1. **Deny-list dans settings.json** : bloque les commandes dangereuses
2. **Regle dans CLAUDE.md** : "utilise `trash` au lieu de `rm -rf`"
- Les deux couches se completent

### settings.local.json

- Regles locales a la machine (pas partagees avec l'equipe)
- Pour hooks locaux, preferences personnelles

---

## 2.4 Auto-Memory (Nouveaute 2026)

- `autoMemoryEnabled: true` (actif par defaut)
- Claude s'ecrit des notes entre les sessions
- Stocke dans `~/.claude/projects/<hash>/memory/`
- 200 premieres lignes de `MEMORY.md` chargees a chaque session

### CLAUDE.md vs Auto-Memory

| | CLAUDE.md | Auto-Memory |
|---|---|---|
| **Qui ecrit** | L'humain | Claude |
| **Contenu** | Instructions, regles | Patterns, apprentissages |
| **Scope** | Projet ou global | Par working directory |
| **Controle** | Total | A verifier regulierement |

**Conseil Melvynx** : Preferer dire "modifie le CLAUDE.md avec cette info" plutot que laisser l'auto-memory sauvegarder n'importe quoi.

---

# PARTIE 3 -- OUTILS AVANCES

## 3.1 Skills (anciennement Commandes)

### Concept

- **Commandes** = ancien nom, **Skills** = nouveau nom (meme chose)
- Prompts reutilisables lances avec `/nom-du-skill`
- Peuvent accepter des parametres (`$ARGUMENTS`)

### Structure d'un skill

**Petit skill** : un seul fichier `SKILL.md`

**Grand skill** : dossier avec :
- `SKILL.md` : entry point (table des matieres)
- `references/` : fichiers de documentation
- `scripts/` : scripts executables

**Avantage token-efficient** : seul l'index est charge, les sous-fichiers sont lus a la demande.

### Creation

- **REGLE** : ne JAMAIS creer les skills manuellement, laisser Claude les creer
- Claude utilise l'agent interne `CloudCodeGuide` pour la structure optimale
- Exemple : "Cree une commande globale qui permet de creer un repository sur GitHub, commit les changements actuels et prepare tout"
- Utilisation : `/github-init Tax-Calculator-2025` (le nom du repo en parametre)
- Note : le **Skill Creator** necessite **Python** pour initialiser la structure

### Installation de skills externes

```bash
npx skill add <auteur>/<nom>
```
- Source : **skill.sh** (marketplace officiel)
- Choisir "global" pour reutilisation cross-projets
- Exemples : `front-end-design`, `skill-creator`

---

## 3.2 Hooks

### Definition

Actions automatiques declenchees avant/apres l'execution d'un outil ou a la fin d'une tache.

### Types de hooks

| Hook | Declencheur |
|------|------------|
| **PreToolUse** | Avant l'execution d'un outil |
| **PostToolUse** | Apres l'execution d'un outil |
| **Stop** | Quand Claude termine de travailler |
| **Notification** | Quand Claude a besoin d'attention |

### Exemples pratiques

**Son de fin de tache (Stop hook)** :
- "Cree un Hook qui fait une sonnerie quand Claude termine"
- macOS : `afplay /System/Library/Sounds/Glass.aiff`

**Prettier automatique (PostToolUse hook)** :
- "Chaque fois que tu modifies un fichier, lance Prettier sur ce fichier"
- S'applique sur l'evenement `Edit`

### Bonnes pratiques

- Ne pas creer les hooks manuellement, demander a Claude
- Ne pas surcharger de hooks (ralentit l'execution)
- Hooks async (`async: true`) pour les actions non-bloquantes

---

## 3.3 MCP (Model Context Protocol)

### Concept

Outils externes connectes a Claude Code via un protocole standardise. Chaque MCP consomme du contexte **meme sans etre utilise**.

### REGLE : Maximum 2-3 MCP

Au-dela, la qualite des outputs se degrade.

### Les 2 MCP recommandes par Melvynx

**1. Context7** (GRATUIT, sans cle API)
- Fournit de la documentation technique a jour
- Installation : compte sur Context7, connexion GitHub
- `claude mcp add ... --scope user` (global)
- Verifier avec `/mcp`

**2. Exa.ai** (GRATUIT avec 20$ de credit)
- Recherche web optimisee pour l'IA
- Scrape Internet et retourne uniquement le contenu pertinent
- Remplace avantageusement le Web Search par defaut
- Cout tres faible : Melvynx a depense ~6$ en 5 mois malgre 3000$/mois d'usage

### Gestion du contexte MCP

- `/context` pour verifier la part MCP
- Si >10% du contexte utilise par les MCP : **Tool Search** s'active automatiquement
- `toolSearchMode: auto` (par defaut)

---

## 3.4 Sub-agents

### Concept

Claude Code peut lancer des mini instances de lui-meme qui travaillent en parallele.

### Pourquoi c'est essentiel

**Probleme** : une recherche web peut consommer 28K tokens. 3 recherches = 84K tokens (proche de la limite).

**Solution** : le sub-agent fait la recherche dans son propre contexte (peut consommer 120K tokens) et retourne seulement 1000-1500 mots pertinents au main agent. Le contexte principal est preserve.

### Types de sub-agents

| Type | Usage | Vitesse |
|------|-------|---------|
| **Explore** | Recherche, exploration | Rapide |
| **Task** | Actions generiques | Normal |

### Creation d'agents custom

`/agent` > Create New Agent > Personal Folder > Generate with Cloud

**Exemple : FixGrammar Agent**
- Prend un fichier en parametre
- Corrige la grammaire
- Modele assignable (Sonnet pour economiser)
- Peut etre lance en parallele sur N fichiers

### Sub-agents specialises recommandes

1. **web-search** : utilise WebSearch, WebFetch, Exa
2. **explore-codebase** : utilise Read, Grep, Search
3. **doc-explorer** : utilise Context7

---

## 3.5 Status Line

Barre d'information en bas du terminal, configurable via Claude.

### Informations affichables

- Dossier courant
- Modele utilise
- Graphe d'utilisation session
- Weekly limit avec delta
- Heures restantes
- Depenses du jour en $
- Si on est dans un repo git
- Pourcentage du contexte consomme

---

# PARTIE 4 -- WORKFLOWS PRO

## 4.0 Le Pattern EPCT (fondamental)

Tout workflow efficace suit ce pattern :

1. **E**xplore : lancer des sub-agents (explore-codebase, explore-doc, web-search)
2. **P**lan : entrer en plan mode, planifier les modifications
3. **C**ode : executer le plan
4. **T**est : verifier lint, tests, que tout fonctionne

Apex est la version avancee de EPCT avec des parametres modulables.

---

## 4.1 OneShot vs Apex

| | `/oneshot` | `/apex` |
|---|---|---|
| **Pour** | Petites features | Features complexes |
| **Vitesse** | Rapide | Plus lent mais plus fiable |
| **Intervention** | Zero (ne demande jamais l'avis) | Parametrable |
| **Exploration** | Legere | Profonde (sub-agents) |
| **Taux de succes** | Bon | >99% |
| **Cout tokens** | Faible | Eleve |

---

## 4.2 Apex en detail

### Parametres

| Flag | Signification |
|------|---------------|
| `--auto` (`-A`) | Mode automatique, ne demande pas les permissions |
| `--examine` (`-e`) | Active la review (securite, clean code) |
| `--test` (`-t`) | Lance les tests |
| `--economy` (`-$`) | Evite les sub-agents (economise tokens) |
| `--branch` (`-b`) | Cree une nouvelle branche git |
| `--pr` (`-p`) | Cree une Pull Request |
| `--interactive` (`-i`) | Mode interactif |
| `--teams` (`-m`) | Mode equipe multi-agents |
| `--resume` (`-r`) | Reprendre une tache interrompue |

### Phases du workflow Apex

1. **Initialisation** : lit le fichier d'init, active/desactive les modes
2. **Analyse de complexite** : determine si la tache est simple ou complexe
3. **Exploration** : lance des sub-agents pour explorer la codebase
4. **Planification** : cree un plan d'execution
5. **Execution** : code la feature
6. **Validation** : verifie sa propre tache
7. (Premium) **Review** : securite, clean code, coherence

### Conseil cle

- **Ne pas taguer de fichiers** (`@fichier`) avec Apex -- laisser l'exploration les trouver
- Les phases d'exploration trouvent quasi toujours tous les fichiers necessaires

---

## 4.3 Autres workflows

### /debug

- Fix erreurs avec propositions de solutions
- Techniques integrees :
  - **Log technique** : ajouter des console.log, reproduire, copier les logs -- la plus efficace selon Melvynx
  - **Manual debug** : l'IA debugue, vous verifiez
  - **Chrome Headless** : juge "lourd et lent" par Melvynx, preferer les logs manuels
- Cout tokens : moyen

### /brainstorm

- 4 rounds de reflexion profonde :
  1. **Exploration** : recherche large
  2. **Devil's advocate** : challenge les idees
  3. **Synthese** : 5 perspectives differentes
  4. **Cristallisation** : conclusion actionnable
- Tres gourmand en tokens

### /simplify (Nouveaute 2026)

- Lance 3 agents de review en parallele
- Review le code modifie pour reuse, qualite, efficacite
- Corrige les problemes trouves automatiquement

### /loop (Nouveaute 2026)

- Tache recurrente a intervalle
- Exemple : `/loop 2m check if PR tests pass, if no, fix it`
- **Detail technique** : cree en interne un cron job (outil `cron_create`), **auto-expire quand la session se ferme**
- Cas d'usage : monitoring PR, verification tests, babysit deploys

### /schedule (Nouveaute 2026)

- Taches planifiees remote (cron **persistant** -- contrairement a /loop qui expire avec la session)
- Exemple : "tous les matins a 9h, clean le dossier Downloads"

### /batch (ANTI-PATTERN selon Melvynx)

- Lance interactivement des agents en parallele avec des work trees separees
- Chaque agent cree sa propre PR
- **Melvynx est CONTRE** : genere des dizaines de PR a review, trop de travail de verification
- **Preferer** : un main agent orchestrateur qui lance des sub-agents et controle tout dans un seul flux
- Citation : "Je n'ai pas envie d'avoir une PR par petit bout de code. J'ai envie que mon main agent gere les sub-agents."

---

## 4.4 Meta-prompting

### Concept

Creer des **prompts qui creent des prompts**. Utiliser le Skill Creator pour creer des meta-skills.

### Meta-skills utiles

- **prompt-creator** : cree des prompts optimises (best practices Anthropic/OpenAI/Google)
- **cloud-memory** : gestion optimisee du CLAUDE.md
- **meta-hook-creator** : cree des hooks
- **meta-skill-creator** : cree des skills
- **meta-skill-workflow-creator** : cree des skills workflow
- **subagent-creator** : cree des sub-agents

---

# PARTIE 5 -- GESTION DU CONTEXTE

## 5.1 Le contexte en chiffres

- **200 000 tokens max** par conversation
- **~27 000 tokens utilises au demarrage** (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- L'autocompact se declenche automatiquement a **~96-99%** du contexte (degrade la qualite, prend du temps)

### Debug du contexte avec Proxyman

- **Proxyman** : outil pour intercepter les requetes HTTP de Claude Code
- Permet de voir exactement ce qui est envoye au modele : system prompt (~3 500 tokens), descriptions des outils (tres volumineux), skills, MCP tools, CLAUDE.md, rules
- Utile pour comprendre pourquoi les 27K tokens sont consommes au demarrage

## 5.2 Le probleme "Lost in the Middle"

- Les transformers donnent beaucoup plus d'importance aux tokens du **debut** et de la **fin**
- Les tokens au **milieu** sont structurellement moins pris en compte
- C'est un biais du transformer, pas un bug
- **Impact** : Quand le contexte grandit, les instructions initiales se retrouvent "au milieu"

## 5.3 Solution : Prompt Discovery (Multi-step)

Au lieu de donner un gros prompt au debut, decouper en etapes :

```
steps/
  step1-init.md
  step2-analyse.md
  step3-plan.md
  step4-execute.md
  step5-verify.md
```

Chaque etape lit son fichier de prompt juste avant d'agir. Le prompt courant est toujours "recent" dans le contexte, donc bien suivi.

## 5.4 Regles de gestion

- `/clear` **OBLIGATOIRE** entre les taches majeures
- `/context` regulierement pendant les longues sessions
- Si contexte > 80% : `/clear` immediat
- Preferer langage naturel pour petites modifs (economise le contexte)
- Sub-agents pour les recherches (evite de polluer le contexte principal)
- Maximum 2-3 MCP actifs

---

# PARTIE 6 -- PROMPTING CONCRET

## 6.1 Greenfield (Nouveau projet)

- Toujours **preciser la stack** (ex: "VJS React ShadCN Tailwind")
- Decrire la feature en langage naturel
- **Separer** les specs desktop et mobile
- Ne PAS specifier la technique (camelCase, architecture) sauf preference forte
- L'IA choisit ses conventions -- pour controler : utiliser un boilerplate existant
- **Boilerplate recommande** : NaotS (embarque des regles strictes que l'IA suit naturellement)
- **Astuce** : si tu as des preferences techniques (camelCase, structure fichiers), les mettre dans le CLAUDE.md global **APRES** le premier run -- pas avant, pour ne pas brider l'IA au demarrage
- **Exemple** : Timezone Checker cree en un seul prompt avec Vite + React

## 6.2 Brownfield (Projet existant)

- L'IA suit automatiquement les conventions du code existant
- **Screenshots annotes** : outil recommande CleanShotX sur macOS
- References aux composants existants : "utilise le meme style que le video picker"

## 6.3 Techniques de debug

### Methode 1 : Screenshot

Envoyer une capture d'ecran du bug. L'IA le resout souvent sans plus d'explications.

### Methode 2 : Logs (la plus efficace)

1. Demander a l'IA d'ajouter des `console.log` strategiques
2. Reproduire le bug
3. Copier les logs **bruts** de la console (pas besoin de les interpreter soi-meme)
4. Envoyer a Claude -- il comprend et corrige

### Methode 3 : 10 variations UI

Prompt exact a utiliser :
```
Cree un fichier /demo/page.tsx avec 10 variations UI/UX de cette interface,
10 composants separes dans 10 fichiers separes, importes dans la page.
```

Exemples de variations generees : dialogue classique, drawer avec switch, multi-action dialogue, split view, accordion, wizard multi-etapes, style Linear, etc.

- Tester chaque variation, choisir la meilleure
- **Supprimer le dossier `/demo/`** apres avoir choisi la version finale

## 6.4 Eviter les erreurs repetitives de l'IA

### Methode 1 : Rules dans `.claude/rules/`

Quand l'IA fait la meme erreur 2+ fois, creer une regle specifique.

**Exemple concret** : L'IA cree `middleware.ts` alors que Next.js utilise maintenant `proxy.ts`. Commande : "Tu as fait cette erreur trop souvent. Cree un fichier dans .claude/rules/ qui t'empechera de la refaire." Regle optimale : `CRITICAL: Never create middleware.ts -- middleware is proxy helper in proxy utils`

### Methode 2 : Workflow dans CLAUDE.md

"Avant de modifier un fichier, tu dois lire au moins 3 fichiers similaires."

### Methode 3 : Rules ciblees avec globs

Creer une regle specifique pour un type de fichier (ex: `api-route.md` avec `path: app/**/route/**`).

### Methode 4 : Commentaires inline (prompt injection benefique)

Ajouter des commentaires detailles dans les fichiers de librairies internes avec exemples d'utilisation. L'IA les lit (grace a la regle "lire 3 fichiers similaires") et absorbe les instructions automatiquement.

**Exemples concrets** : documenter les helpers internes comme `ZodRoute` (validation API routes) ou `Upfetch` (HTTP client) avec usage, parametres, et exemples de code dans les commentaires. Quand l'IA lit ces fichiers, elle comprend exactement comment les utiliser.

### Methode 5 : Changelog automatique

Regle dans CLAUDE.md : "CRITICAL: Apres tout changement, update le changelog avec la date du jour."

## 6.5 Conseils de prompting quotidien

- Pour features complexes : utiliser un workflow (Apex, code, etc.)
- Pour petites modifications : langage naturel simple, pas de commande
- Ne pas ecrire des prompts parfaits : fautes d'orthographe OK, l'IA comprend
- Ne pas taguer les fichiers avec Apex/OneShot -- laisser l'IA explorer
- **Ne pas empiler les demandes** pendant que l'IA travaille : ca fonctionne mais peut la perturber, preferer attendre la fin
- **Attention aux mauvais chats** : avec plusieurs terminaux ouverts, erreur frequente d'envoyer un prompt dans le mauvais terminal

---

# PARTIE 7 -- MULTI-AGENTS

## 7.1 Agent Teams

### Activation

`enableAgentTeams: true` dans settings.json

### Fonctionnement

- Le main agent (team lead) cree des equipes avec task list partagee
- Les teammates travaillent de maniere independante
- Interface : switcher entre agents avec les fleches bas dans le terminal
- Les teams utilisent Tmux pour splitter les agents en tabs

### Quand utiliser

- **OUI** : zones de modification SEPAREES (ex: monorepo front/back/mobile)
- **NON** : une seule zone modifiee -- les agents se marchent dessus
- Les agents communiquent peu entre eux (surtout via le team lead)

## 7.2 Work Trees

### Concept

Travailler sur des branches separees en parallele.

### Workflow

1. Creer le work tree
2. Coder la feature
3. Creer la PR
4. Merger
5. Detruire le work tree

### Regles

- Maximum **4 work trees simultanes**
- Pour petites features : travailler directement dans le meme repo
- Pour refactors/migrations : utiliser les work trees

### Astuce CLAUDE.md

"Si il y a des erreurs TypeScript que tu n'as pas directement implementees, c'est pas ton probleme, passe a la suite."

## 7.3 Tmux (Terminal Multiplexer)

### Installation

```bash
# macOS
brew install tmux
# Linux/WSL
apt install tmux
```

### Raccourcis essentiels

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `Ctrl+A C` | Nouveau terminal (window) |
| `Shift+fleches` | Switcher entre windows |
| `Ctrl+A S` | Split en deux |
| `Ctrl+A ,` | Renommer la window |
| `Ctrl+A $` | Renommer la session |
| `Ctrl+A D` | Detacher (quitter sans fermer) |
| `tmux attach` / `tmux a` | Rattacher la session |

### Cas d'usage

Un tab par role : CC1 (feature), CC2 (review), Server, Ingest, etc.

---

# PARTIE 8 -- CONFIGURATION ET PRICING

## 8.1 Abonnements

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| **Pro** | 20$ | ~160$ | x8 |
| **Max 5x** | 100$ | ~800$ | x8 |
| **Max 20x** | 200$ | ~3 200$ | x16 (meilleur ratio) |

- **API directe** : Opus = 15$/M input + 75$/M output (peut monter tres vite)
- **Recommandation** : Max 20x (200$/mois) pour usage pro
- Systeme de limites : sessions 5h + limite weekly globale

## 8.2 Le dossier `.claude` en profondeur

| Dossier | Contenu |
|---------|---------|
| `projects/` | Historique de toutes les sessions |
| `plans/` | Tous les plans crees |
| `rules/` | Regles globales |
| `skills/` | Skills actifs et desactives (deplacer dans "disabled" pour desactiver) |
| `agents/` | Agents actifs et desactives (deplacer dans un sous-dossier "disabled" pour desactiver) |
| `scripts/` | Scripts personnels d'automatisation (astuce : lancer Claude Code dans `~/.claude/` pour gerer/modifier sa propre config) |
| `tasks/` | Toutes les taches executees |
| `teams/` | Equipes creees (JSON) |

## 8.3 Plugins (Attention)

- Installation : `/plugin` (ex: context7, feature-dev)
- Deux sources de plugins officiels : **"Anthropic Cloud Code"** et **"Anthropic Cloud Official Plugins"**
- **Probleme** : perte de controle -- le code est dans un dossier non modifiable, ecrase a chaque mise a jour automatique
- **Recommandation forte de Melvynx** : copier le code des plugins dans vos propres skills/agents pour garder le controle
- **Philosophie** : voler les concepts, creer votre propre configuration

## 8.4 Deploiement rapide

### Netlify Drop (le plus simple)

1. Demander a Claude : "prepare un dossier pour deployer sur Netlify Drop"
2. Claude genere un dossier `dist/`
3. Aller sur `app.netlify.com/drop`
4. Drag & drop le dossier `dist/`
5. URL publique instantanee

## 8.5 Nouveautes 2026

### Claude Review (beta entreprise)

- Review automatique des PR avec process multi-agents en parallele
- Resultats apres des mois de tests :
  - PR avec reviews passent de **16% a 54%**
  - Moins de **1%** des reviews marquees incorrectes
  - **94%** des bugs trouves sur les grandes PR
- Cout : **15-25$ par review** (paye sur token usage, augmente avec la complexite)
- Comparaison : un ingenieur mid a San Francisco coute 77-96$/h, un senior 100-150$/h -- Claude Review est quasi gratuit pour une entreprise
- Optimise pour les equipes, pas les individuels

### Voice Mode

- `/voice` dans Claude Code
- Fonctionne mieux en anglais
- Ne remplace PAS les outils de dictee natifs (Super Whisper, etc.)

### Ask User Question ameliore

- Peut maintenant proposer des UI avec exemples de code directement dans le terminal
- Choix multiples : selectionner une option et appuyer sur `N` pour ajouter une note avant d'envoyer
- Update parfaite selon Melvynx : rien a changer, ca "vient juste marcher"

---

## 8.6 Commandes principales de Melvynx (Resume)

| Commande | Usage | Cout tokens |
|----------|-------|-------------|
| `/apex` | Feature complete, >99% succes | Eleve |
| `/oneshot` | Quick fix rapide | Faible |
| `/debug` | Fix erreurs avec solutions | Moyen |
| `/brainstorm` | Recherche profonde 4 rounds | Tres eleve |
| `/clean-code` | Principes de clean code | Moyen |
| `/review-code` | Review multi-agents | Eleve |
| `/load-memory` | Optimise la memoire CLAUDE.md | Faible |
| `/commit` | Commits simplifies | Faible |
| `/fix-errors` | Corrige erreurs en parallele | Moyen |
| `/fix-grammar` | Corrige grammaire en bulk | Moyen |
| `/fix-pr` | Corrige problemes d'une PR | Moyen |
| `/prompt-creator` | Cree des prompts IA | Faible |
| `/skill-creator` | Cree des skills | Faible |
| `/subagent-creator` | Cree des sub-agents | Faible |
| `/ultrathink` | Max thinking tokens (2+ min) | Tres eleve |

## 8.7 Le dossier "CC" (Experimentation)

Dossier personnel (`~/cc/`, accessible via alias `cdcc`) pour tout faire avec Claude Code hors projets :
- Generateur de titres YouTube
- Createur de slides (site web)
- Analyses business, reviews d'emails
- Gestion de projets, comptabilite, todo lists
- **Philosophie** : "Est-ce que Claude Code peut le faire ?" -- si oui, pas besoin de SaaS

## 8.8 Niveaux de configuration

| Niveau | Description |
|--------|------------|
| **0** | Aucune config |
| **OK** | Claude Code de base |
| **OG (90%)** | Config gratuite Melvynx (Blueprint CLI : `npx ig-blueprint-cli`) |
| **OG++ (99%)** | Config premium Melvynx (workflows avances, review securite) |

### Ce que Blueprint CLI installe

- **Backup automatique** de la config existante dans un dossier `backups/`
- **Skills gratuits** : `/load-memory`, `/commit`, `/fix-errors`, `/fix-grammar`, `/oneshot`, `/apex`, etc.
- **Command Validator** : script qui verifie les commandes avant execution (couche de securite supplementaire au-dessus de la deny-list)
- **StatusLine** avancee (necessite `bun`)
- **Deux sons de notification distincts** :
  - Son **"fin de tache"** : Claude a termine son travail
  - Son **"need human"** : Claude pose une question et attend ta reponse
- Configuration des permissions (bypass + deny-list)
- Si ca ne marche pas du premier coup : utiliser `/debug` pour que Claude s'auto-corrige

---
---
---

# PARTIE 9 -- OPENCLAW (Agent IA distant via Telegram)

> Cette partie couvre OpenClaw (anciennement ClawdBOT / MaltBot), l'agent IA qui permet de controler son ordinateur a distance depuis Telegram.

---

## 9.1 Concept general

OpenClaw permet de **controler son ordinateur a distance depuis Telegram**, y compris par audio/vocal. Il lance des agents Claude Code en arriere-plan et notifie quand c'est termine.

### Cas d'usage reels

- A la piscine/salle de sport : envoyer un audio Telegram pour fixer un bug
- Controle de plateformes (coupons, abonnes, emails) via Telegram
- Planification de tweets depuis Telegram
- Lancer des features, creer des PR, merger du code -- tout depuis le telephone

---

## 9.2 Installation de base (sur ordinateur local)

### Etape 1 : Installer OpenClaw

```bash
curl maltbot.sh
```

- Choisir **Claude Code CLI** comme backend
- Autoriser via URL dans le navigateur

### Etape 2 : Configurer Telegram

1. Ouvrir **BotFather** dans Telegram
2. `/newbot` pour creer un nouveau bot
3. Nommer le bot
4. Copier le token API
5. Le coller dans la config OpenClaw

### Etape 3 : Skills optionnels

Skills disponibles a l'installation : 1Password, Apple Notes, Apple Reminders, Obsidian, Things, X/Twitter (Bird), GitHub, Google Places API, Edit PDF, Oracle, Summarize, etc.

### Audio bidirectionnel avec ElevenLabs

- Envoi de **messages audio** depuis Telegram : l'agent les transcrit et repond
- Configuration de **ElevenLabs** pour une voix personnalisee : reponses audio en retour
- Discussion vocale bidirectionnelle via Telegram

### Note : le bot ne gere qu'une session a la fois

- Utiliser `/new` dans le chat Telegram pour creer une nouvelle session
- Une seule conversation active par chat

### Idee Mac Mini dedie

- Un **Mac Mini** a ~600 EUR pour faire tourner OpenClaw 24/7
- Permet de ne pas bloquer l'ordinateur principal
- Reference : le YouTubeur **Alex Finn** a documente son setup ClawdBot complet

---

## 9.3 Installation securisee sur VPS (Docker)

### Pourquoi un VPS

- Agent disponible **24/7** meme si l'ordinateur est eteint
- Docker ajoute une couche de **securite** (sandboxing)

### VPS recommande

- **Hetzner** : CAX21 ou CAX31 (ARM, ~$7.59/mois)
- OS : Ubuntu
- IPv4 obligatoire

### Methode rapide : script automatise

```bash
npx openclaw-vps-setup
```

Ce script tout-en-un installe :
- Node.js, OpenClaw, Claude Code, Bun, Cloudflared, GCloud
- Options securite : **UFW** (firewall ports), **Fail2Ban** (brute force), **SSH Hardening** (desactiver password, cle uniquement)
- Onboarding OpenClaw interactif

### Methode manuelle : etapes completes

**1. Creer et configurer la cle SSH**
```bash
ssh-keygen -t ed25519 -f ~/.ssh/hetzner3
# Copier la cle publique dans Hetzner
```

**1b. Creer un profil SSH avec keep-alive** (evite les deconnexions) :
```
# Dans ~/.ssh/config :
Host hetzner3
  HostName <IP_DU_VPS>
  User root
  IdentityFile ~/.ssh/hetzner3
  ServerAliveInterval 60
  ServerAliveCountMax 3
```
Astuce : demander a Claude Code de creer la config SSH directement.

**2. Se connecter au VPS**
```bash
ssh hetzner3
```

**3. Installer Docker**
```bash
apt-get update
apt-get install -y ca-certificates curl
curl -fsSL https://get.docker.com | sh
docker --version
docker compose version
```

**4. Cloner OpenClaw**
```bash
git clone <repo-openclaw>
cd openclaw
mkdir -p /root/.openclaw /root/.openclaw-workspace
chown -R 1000:1000 /root/.openclaw /root/.openclaw-workspace
```

**5. Creer le fichier .env**
```bash
vim .env
# TOKEN secret, PASSWORD, config Google (optionnel)
# Sauvegarder : :wq
```

**6. Installer Claude Code sur le VPS**
```bash
curl -fsSL https://claude.ai/install.sh | sh
export PATH="$HOME/.local/bin:$PATH"
claude
# Se connecter avec son abonnement
```

**7. Build et lancer Docker**
```bash
docker compose build    # peut prendre du temps
docker compose up -d
```
- Si erreurs au build : demander a Claude Code "fixe puis relance le build et auto-fixe jusqu'a ce que tout soit OK"
- Problemes frequents : permissions bypass, ports, binaires manquants -- Claude les resout en boucle

**8. Onboarding dans le Docker**
```bash
docker compose exec openclaw-gateway bash
node dist/index.js onboard
```
- Accepter les conditions
- Quick start
- Choisir le provider : **OpenAI Codex** (rentable) ou **Anthropic**
- Choisir le modele
- Choisir channel : **Telegram Bot API**
- Creer le bot dans BotFather, copier le token

**9. Pairing Telegram**
```bash
node dist/index.js pairing approve telegram <CODE>
```

**10. Dashboard**
```bash
node dist/index.js dashboard
# Donne une URL localhost:18789?token=xxx
# SSH tunnel pour acces distant :
ssh -N -L 18789:localhost:18789 hetzner3
# Puis ouvrir localhost:18789?token=xxx dans le navigateur
```

**11. Approuver les devices**
```bash
# Lister les devices connectes
node dist/index.js devices list

# Approuver un device
node dist/index.js devices approve <device_id>
```

### Securite VPS

- **UFW** (firewall) sur les ports
- **Fail2Ban** pour bloquer les brute force
- Desactiver le password SSH (uniquement cle SSH)
- Docker pour le sandboxing
- Ne jamais laisser le dashboard ouvert en permanence

### Astuce bypass permission sur VPS

Claude Code refuse le bypass permission sur VPS pour raisons de securite. Hack :
```bash
SANDBOX=1 claude
# Claude croit etre dans une sandbox et accepte le bypass
# Ajouter dans .bashrc pour persistance :
export SANDBOX=1
```

---

## 9.4 Workflow complet OpenClaw

### Lancer une feature depuis Telegram

1. Envoyer un message/audio sur Telegram avec la description de la feature
2. Le skill **"code_task"** prend le relais avec cette sequence precise :
   - Clone/cree un **worktree** Git
   - Installe les dependances (`npm install`)
   - Cree une **GitHub Issue** avec la description de la tache
   - Lance la commande exacte : `claude -p --dangerously-skip-permissions apex --pr <issue_number> <description>`
   - Schedule un **cron job** (completion watcher) qui verifie **chaque minute** si le processus est termine
3. Quand l'agent termine : notification Telegram avec le lien de la PR
4. Demander : "merge la PR et delete le worktree"

### Commandes utiles

```bash
claude -p --dangerously-skip-permissions  # Lance un agent Claude en background
ps -p <PID> -o stat,etime                # Verifier le statut et temps ecoule d'un processus
```

---

## 9.5 Skills OpenClaw

### Philosophie : Skills remplacent les MCP

Melvynx considere que les MCP sont une abstraction inutile pour OpenClaw :
- Un agent Claude peut directement faire un `fetch` sur n'importe quelle API publique
- Un skill = un fichier `SKILL.md` (prompt) + un script `execute.sh` (appel API)
- Plus leger, plus simple, plus controlable -- pas besoin d'intermediaire MCP
- Chaque skill fait exactement ce que VOUS voulez, dans VOTRE style

Structure : `skill.md` (prompt) + `execute.sh` (script)

### Skills installes lors du setup VPS

Skills proposes pendant l'onboarding : ClawUps (nouveautes), Gemini (modele alternatif), GOG (Google Gmail/Calendar), Nano Banana Pro, OpenAI Whisper (speech-to-text), Google Places API

### Creer un skill API

1. Copier la doc markdown de l'API
2. La coller dans un prompt
3. Demander a Claude de creer le skill
- Pas de dependances, pas de hacks, utilise votre style

### Skills de Melvynx (liste)

| Skill | Usage |
|-------|-------|
| Cloudflare | Manager les serveurs |
| Code process | Lancer Claude Code pour coder |
| CodeLine | Plateforme de formation (coupons, users) |
| Context7 | Recuperer de la documentation |
| Dub | URL shortener (mlv.sh/xxx) |
| FlightData | API Aviation Stack pour les vols |
| Front | Support client |
| Google Image | Generation d'images |
| LinkedIn Post | Publier sur LinkedIn |
| Lumail | Email marketing |
| Mercury | Acces bancaire |
| Porkbun | Gestion de domaines |
| SaveIt | App de sauvegarde de signets |
| TypeFully | Tweeter / planifier des tweets |
| Vercel | Deployements de sites |
| YouTube Comments | Lire les commentaires YouTube |

---

## 9.6 Configuration Gmail + Calendar

1. Creer un projet **Google Cloud**
2. Activer les API Gmail et Google Calendar dans le **Marketplace**
3. Creer des **Credentials** > OAuth > Application de bureau
4. Telecharger le JSON des credentials
5. Donner le JSON a OpenClaw (envoyer via Telegram)
6. OpenClaw demande un lien OAuth a cliquer pour autoriser l'acces
7. Auto-approuver en mode dev (pas besoin de publier l'app)
8. Coller le code d'autorisation

### Cloudflare Tunnel (pour webhooks Gmail)

- Necessite un **domaine** et **Cloudflared** installe sur le VPS
- Permet d'exposer une adresse publique du VPS pour que Gmail envoie des notifications en temps reel
- Quand un email arrive, OpenClaw recoit une notification et peut agir (ex: sauvegarder les infos d'un vol)
- "Complique a faire, accrochez-vous" (citation Melvynx)

### Cron jobs utiles

- "Tous les jours a 8h, lis mes emails et fais une review"
- "Backup mon workspace sur Git matin et soir"
- Calendrier : creer/modifier/supprimer des evenements via Telegram

---

## 9.7 Modeles recommandes pour OpenClaw

| Usage | Modele | Cout par M tokens |
|-------|--------|-------------------|
| Taches OpenClaw generales | **Gemini 3 Pro** | **$2 input / $12 output** |
| Cron jobs | **Gemini 3 Flash** | Encore moins cher (ideal pour les watchers) |
| Agents de code | **Claude Opus** | Meilleure qualite (utilise l'abonnement Claude Code) |

---

## 9.8 Fichiers de configuration OpenClaw

| Fichier | Role |
|---------|------|
| `soul.md` | Identite/personnalite du bot (nom, timezone, site web, personnalite) |
| `memory/` | Memoire quotidienne (souvenirs entre sessions) |
| `canvas/index.html` | Interface web locale |
| `.env` | Variables d'environnement (token, password, Google credentials) |

### Configurer l'identite du bot

- Demander a Claude Code : "Je suis en train de setup mon agent, fais-moi un resume de ce que tu sais de moi"
- Envoyer le resume au bot OpenClaw pour qu'il connaisse son utilisateur
- Le bot sauvegarde dans `soul.md` et `memory/`

### Workspace GitHub (backup)

- Creer un repo GitHub pour sauvegarder la config OpenClaw
- Commande : "Cree un repo openclaw-workspace sur GitHub et initialise ce projet"
- Cron automatique : backup matin et soir sur Git

---

## 9.9 Securite OpenClaw (IMPORTANT)

- Le gateway ouvre un port localhost (18789) potentiellement accessible
- Risques si quelqu'un accede au bot : suppression de fichiers, vol de mots de passe, malware
- Commande d'audit securite : `claudebot security-audit` (verifie les vulnerabilites)
- **Sandboxing Docker** obligatoire pour limiter l'acces
- Ne PAS activer 1Password sans reflexion (acces a toutes les cles)
- Telegram est plus securise que l'interface web

### Machine dediee

- Recommande : **Mac Mini dedie** (~600 EUR) pour laisser OpenClaw tourner 24/7
- Alternative : **VPS** (solution recommandee par Melvynx)

---

## 9.10 Conseils finaux OpenClaw

- Plus OpenClaw a d'outils/skills, plus il est utile
- **Bouton "Partager via Telegram"** : depuis n'importe quelle app (Twitter, Safari, etc.), partager un lien/tweet/article directement au bot -- il le sauvegarde ou agit dessus
- Envoyer une **photo** via Telegram : OpenClaw la copie dans le workspace VPS
- Ne jamais modifier les settings manuellement : demander a Claude de le faire
- Si OpenClaw crash : SSH dans le VPS, lancer Claude Code pour debug
- Creer un repo GitHub pour sauvegarder la config OpenClaw (backup auto matin/soir)
- **OC Pro** : produit payant de Melvynx avec script automatise qui fait tout le setup en 20 min
- **La seule limite est la creativite**

---

# ANNEXE -- FICHIERS SOURCE

| Fichier | Contenu | Lignes |
|---------|---------|--------|
| 1000h de Claude Code resumees en 1h30 | Formation complete gratuite 2026 | 1 684 |
| TUTO COURS Claude Code COMPLET 4h | Masterclass bases + avance + pro | 5 245 |
| TUTO COURS Claude Code (version alt) | **DOUBLON** du precedent | 2 540 |
| TUTO COURS Claude Code (copie) | **DOUBLON** du precedent | ~5 245 |
| Claude Code TUE Encore 3 Startup | Nouveautes 2026 (Voice, CEMUX, /simplify, /loop, Review) | 364 |
| VIBE CODEUR Formation Terminal | Bases du terminal pour debutants | 196 |
| Je setup Claude Code chez un Vibe Codeur | Setup config Melvynx en 20 min | 451 |
| Je setup Claude Code (copie 2) | **DOUBLON** du precedent | 451 |
| Mon Setup OpenClaw Telegram | Workflow OpenClaw avance | 264 |
| Je teste ClawdBOT | Premier test ClawdBOT | 817 |
| TUTO OpenClaw Docker VPS | Setup securise VPS | 836 |
| Je SETUP OpenClaw debutant | Setup complet avec coaching | 1 995 |

**Doublons identifies** : 3 paires de doublons (6 fichiers), 10 fichiers uniques au total.

---

---
---

# PARTIE 10 -- COMPLEMENTS ET ADAPTATION A TON INFRASTRUCTURE

> Cette partie complete la formation avec les elements de l'ecosysteme Melvynx non couverts par les transcriptions, ainsi que les adaptations specifiques a ton infrastructure.

---

## 10.1 L'ecosysteme etendu de Melvynx (La Stack Complete)

### Les IDEs IA (Cursor & Windsurf)

Le document se concentre a 100% sur VS Code et le terminal. Or, la philosophie "Vibe Coder" de Melvynx repose massivement sur l'utilisation de **Cursor** (ou Windsurf) comme editeur principal pour le code "Brownfield" (projets existants). Claude Code est son chef d'orchestre dans le terminal, mais Cursor reste l'outil de base pour l'edition visuelle.

### Le Backend et la BDD (Supabase)

La partie 6.1 mentionne le front-end (`Vite + React + ShadCN + Tailwind`). Il manque sa brique favorite pour le backend : **Supabase**. C'est ce qu'il utilise systematiquement pour l'authentification et la base de donnees de ses SaaS (Umail, SaveIt, etc.).

### Le vrai deploiement (Vercel)

Netlify Drop (partie 8.4) est mentionne pour les demos rapides, mais pour la mise en production de ses vrais projets, Melvynx deploie presque exclusivement sur **Vercel** (souvent avec Next.js).

---

## 10.2 Adaptation et Securite de l'Infrastructure

### Le choix du VPS

La partie 9.3 recommande fortement Hetzner. Cependant, toute cette architecture Docker/OpenClaw est agnostique. Elle tournera de maniere tout aussi performante sur une machine Hostinger (comme le VPS 109.176.197.139). Il n'y a aucune obligation de changer de fournisseur.

### Angles morts de Fail2Ban / UFW (CRITIQUE)

Le script de setup VPS installe des securites agressives contre le brute force. **Avertissement critique** : avant d'activer UFW ou Fail2Ban, tu dois imperativement configurer tes propres adresses en "priorite absolue" et ne jamais les bannir.

**Adresses a whitelister obligatoirement :**

| IP | Description |
|----|-------------|
| 176.133.20.227 | Domicile Soly |
| 185.208.60.2 | Ibis/Rezocean (IP travail) |
| 109.176.197.139 | VPS Hostinger |
| 31.35.20.184 | BOKADOR (IP publique maison) |
| 92.92.127.0 - 92.92.128.255 | Plage Kim (92.92.127.0/23) |

Ne pas whitelister ces adresses = risque de se bannir soi-meme de ses propres machines (SSH, RDP bloques).

### Le confort sur WSL (Windows)

La partie 1.1 recommande WSL pour les utilisateurs Windows. Pour que l'experience Vibe Coder soit fluide et que l'agent ait acces a tous tes projets, il manque l'etape d'automatisation du montage de tes disques locaux (G:, H:, etc.) a chaque lancement de WSL.

---

## 10.3 Diagnostic Config BOKADOR (2026-03-25)

Verification de la config `~/.claude/` de BOKADOR par rapport a la video "Je setup Claude Code chez un Vibe Codeur en 20 minutes" :

### CONFORME (deja en place)

| Element | Statut |
|---------|--------|
| Bypass permission + deny-list | OK |
| Hook Stop (son fin de tache) | OK |
| Hook PreToolUse (command validator) | OK |
| Hook PostToolUse (log modifications) | OK |
| enableAgentTeams: true | OK |
| autoMemoryEnabled: true | OK |
| Skills : /apex, /oneshot, /load-memory, /commit, /fix-errors, /fix-grammar, /fix-pr, /ultrathink | OK |
| Agents : explore-codebase, doc-explorer, web-search, debugger, etc. (18 agents) | OK |
| Plugins : hookify, commit-commands, feature-dev, code-review, pr-review-toolkit, etc. | OK |
| StatusLine | OK |

### MANQUANT (3 elements a installer)

| Manque | Commande d'installation | Priorite |
|--------|------------------------|----------|
| **Context7 MCP** | `claude mcp add context7 --url <url> --api-key <KEY> -s user` (creer un compte sur Context7, sign in GitHub, generer API key) | HAUTE |
| **Exa.ai MCP** | `claude mcp add exa --url <url> --api-key <KEY> -s user` (creer un compte sur exa.ai, sign in Google, 20$ credit gratuit) | HAUTE |
| **Hook Notification** (son "need human") | Creer un hook Notification avec un son different du Stop pour savoir quand Claude pose une question | MOYENNE |

**Note** : Les MCP actuels (`memory` + `fetch`) ne sont PAS ceux recommandes par Melvynx. Il recommande Context7 + Exa.ai. Les MCP `memory` et `fetch` peuvent rester (3 MCP max) mais Context7 et Exa sont prioritaires.

---

## 10.4 Prudence sur le nettoyage

### Les doublons de l'annexe

La derniere section du document liste 6 fichiers identifies comme des "doublons". **Attention** : ne jamais demander a Claude Code de les nettoyer a l'aveugle. Ne supprime jamais de fichiers qui ne sont prouves etre des doublons que par leur taille -- le risque de perte de donnees est trop eleve. Toujours verifier avec un hash SHA256 avant suppression.

---

> **Formation compilee depuis les transcriptions Melvynx par Claude Code -- 2026-03-25**

---
---

# PARTIE 11 -- COMPLEMENT : "1000h de Claude Code resumees en 1h30" (2026)

> Source : Video Melvynx "1000h de Claude Code resumees en 1h30 (formation gratuite 2026)"

## 11.1 Deploiement rapide avec Netlify Drop

- Methode la plus simple : Netlify Drop (app.netlify.com/drop)
- Workflow : demander a Claude "prepare un dossier pour deployer sur Netlify Drop"
- Claude genere un dossier dist/ (index.html + assets)
- Drag and drop du dossier sur Netlify Drop = URL publique instantanee

## 11.2 Agent FixGrammar en parallele

- Creation via /agent > Create New Agent > Personal Folder > Generate with Cloud
- Assigner un modele different (Sonnet) pour economiser + une couleur
- Main agent lance FixGrammar en parallele sur chaque fichier (13 simultanement dans la demo)

## 11.3 Recommandation progressive abonnements

- Commencer Pro 20$/mois > Max 100$ > Max 200$ si necessaire
- Depense API directe possible : 171$ en une seule journee

## 11.4 Analogie cuisinier

- Claude Code = robot executant du chef (modele). Chat = chef qui explique sans agir.
- Skills = livre de cuisine (SKILL.md = table des matieres + recettes individuelles)

---

# PARTIE 12 -- COMPLEMENT : "Claude Code TUE Encore 3 Startup" (2026)

## 12.1 /voice (beta) -- Espace pour micro, mauvais en francais, ne remplace pas dictee native
## 12.2 CEMUX -- Terminal agents IA, Ghosty-based, tabs V+H, notifications, browser integre, gratuit
## 12.3 Ask User Question -- Choix UI riches, N pour ajouter note, automatique
## 12.4 /simplify -- 3 agents review paralleles (reuse, qualite, efficacite), auto-corrige
## 12.5 Auto-Memory -- autoMemoryEnabled:true, 200 lignes MEMORY.md, verifier regulierement
## 12.6 /schedule (cron persistant) vs /loop (expire avec session)
## 12.7 Claude Review beta -- 94% bugs trouves, 15-25$/review, entreprises uniquement

---

# PARTIE 13 -- COMPLEMENT : "VIBE CODEUR : Formation terminal" (2026)

## 13.1 3 piliers Vibe Codeur : Terminal + VS Code + Git/GitHub (langages plus obligatoires)
## 13.2 Bash != PowerShell (zero compatibilite). WSL recommande sur Windows
## 13.3 macOS : Clic droit + Option = "Copy as pathname". cd seul = retour ~
## 13.4 Outils : Raycast, Homebrew, CleanShotX, WhisperTurbo (Parakeet=court, Large=francais), Ghosty
## 13.5 Premiere app : npm create vite@latest > cd > npm install > npm run dev (localhost:5173)

---


---

# PARTIE 14 -- COMPLEMENT : "Je SETUP OpenClaw en 1h a un total debutant" (2026)

> Source : Video Melvynx — Setup complet OpenClaw avec un debutant total

## 14.1 VPS Setup — Commandes exactes

- Script automatise : `npx open-claw VPS setup`
- Installe automatiquement : Node.js, openclaw-github, Claude CLI, Bun, Cloudflared, GCloud
- Security Hardening : UFW (firewall), Fail2Ban, SSH Hardening (desactiver password, cle uniquement)

## 14.2 Permission Bypass sur VPS (IMPORTANT)

Par defaut Claude Code refuse le bypass permission sur VPS. Hack :
- Ajouter dans .bashrc du VPS : `export CLAUDE_SANDBOX=0`
- Ou lancer : `CLAUDE_SANDBOX=0 claude [commande]`

## 14.3 Telegram Bot Setup detaille

1. Ouvrir BotFather sur Telegram > /newbot > nommer le bot
2. Recuperer le TOKEN API
3. `openclaw onboard` > Quick start > Provider Anthropic > Token > Telegram Bot API > Configure Skills

## 14.4 Skills recommandes a l'installation

- claw-ups (web updates), openai-whisper (speech-to-text), gemini (fallback LLM)
- PAS Google Workspace (deprecated, utiliser CLI recent)

## 14.5 Google Workspace Integration (Calendar + Gmail)

1. Creer Google Cloud Project
2. Activer APIs : Google Calendar API + Gmail API
3. Create Credentials > Desktop App > telecharger JSON (client_id + client_secret)
4. Donner le fichier JSON a OpenClaw
5. OAuth2 flow : OpenClaw envoie URL > user copie code > re-colle dans Telegram

## 14.6 Workspace et Auto-sync GitHub

- `openclaw workspace init` cree un repo GitHub prive
- Structure : config.json + skills/ + sessions/ + memory/
- OpenClaw commit automatiquement chaque modification config vers GitHub

## 14.7 Gestion des cles API

- `openclaw config add OPENAI_API_KEY [key]`
- Tout stocke dans `~/.openclaw/.env` (ne JAMAIS commiter)

## 14.8 Troubleshooting

- Si OpenClaw ne repond plus : auto-restart apres 5 min, ou relancer manuellement via SSH
- Permission bypass echoue : verifier export CLAUDE_SANDBOX=0
- Cron actif seulement si OpenClaw service actif (pour persistant : systemd)

---

# PARTIE 15 -- COMPLEMENT : "Je teste ClawdBOT pour la premiere fois" (2026)

> Source : Video Melvynx — Premier test en direct de ClawdBOT

## 15.1 Historique et naming

- Ancien nom : Claude Bot > renomme Malt Bot (demande Anthropic, confusion tonalite)
- Beaucoup d'utilisateurs achetent des Mac mini (~600 EUR) pour ClawdBOT 24/7

## 15.2 Architecture fichiers ClawdBOT

```
~/.claude/cloud/
  agent_identity.json    -- nom, timezone, description
  memory/today.md        -- memoire du jour
  canvas/index.html      -- interface web locale
```

Le bot met a jour ses fichiers automatiquement pour se souvenir du contexte.

## 15.3 Securite — Risques identifies (CRITIQUE)

- Port gateway ouvert (localhost:18789) = quiconque y accede controle le bot
- Si bot Telegram compromis = acces PC complet
- 1Password skill = extraction de TOUS les secrets
- Bot peut etre ordonne de supprimer recursivement

Recommandations :
1. TOUJOURS sandboxer dans Docker
2. Pas de port ouvert vers Internet
3. Ne connecter que les integrations essentielles
4. Whitelist IPs Telegram
5. Commande audit : `cloudbot security-audit`

## 15.4 Cas d'usage reel — Feature complete via Telegram

Workflow teste en direct :
1. Envoyer commande Telegram : "Dans Subfast, supprime X, ajoute Y, affiche stats"
2. Bot cree/modifie fichiers, routes API, cache NextJS, fixe ESLint
3. PR creee automatiquement avec commit au nom de l'utilisateur
4. Tout en < 10 minutes, sans toucher un clavier d'ordinateur

## 15.5 Limitations decouvertes

- Pas de multi-session Telegram (/new coupe la session actuelle)
- VPS instabilite : SSH se coupe toutes les 20 secondes (selon testeur)
- Email integration absente au moment du test
- Latence interface web au chargement des sessions

## 15.6 Screenshots + Instructions contextuelles

Le bot peut recevoir des screenshots annotes et comprendre les instructions visuelles.
Exemple : envoyer screenshot du site + "Fous-moi le logo Product Hunt en dessous du email generator"
Le bot comprend le contexte visuel et code la modification.

---

# PARTIE 16 -- COMPLEMENT : "Mon Setup OpenClaw pour Coder via Telegram" (2026)

> Source : Video Melvynx — Workflow complet de codage a distance

## 16.1 Code Task via Screenshot + URL

Au lieu d'ecrire des commandes, envoyer une URL + screenshot avec la description.
Le skill code_task execute :
1. Clone le repo / cree worktree
2. Installe dependances
3. Cree GitHub issue
4. Lance : `claude -p --dangerously-skip-permissions apex --pr <issue> <description>`
5. Schedule completion watcher (cron chaque minute)
6. Notifie quand termine

## 16.2 Modeles alternatifs et pricing

- Gemini 3 Pro : $2/M input + $12/M output (plus economique que Claude Opus)
- Gemini 3 Flash : pour cron jobs recurrents (100+/jour)
- Strategie : Flash pour taches repetitives, Opus pour taches complexes ponctuelles

## 16.3 Skills API concretes de Melvynx

### CodeLine (plateforme formation)
- Lister coupons, consulter infos, recuperer subscribers + derniers emails
- Skill = SKILL.md (prompt) + execute.sh (curl API)

### Lumail (email marketing)
- get_campaign, replace_campaign, send_email
- Controle complet des campagnes depuis Telegram

### TypeFully (social media)
- Creer et planifier des tweets
- Consulter posts planifies, brouillons
- Exemple : "Cree un tweet et planifie-le demain a 14h"

## 16.4 Creer un Skill API (methode universelle)

1. Copier la doc markdown de l'API (ex: TypeFully docs = ~7000 lignes)
2. Coller dans un prompt Claude
3. Demander : "Create Skill Creator, similaire a code_line. Nouveau skill TypeFully avec API Key [TOKEN]"
4. Claude genere : SKILL.md + execute.sh
5. Utilisation directe via Telegram

## 16.5 MCP vs Skills — Critique structuree de Melvynx

**Pourquoi les Skills > MCP pour OpenClaw :**
- MCP = abstraction inutile (Claude > MCP > API au lieu de Claude > API)
- MCP ne savent pas quand arreter (rabbit hole)
- Skills = controle total, pas de dependances, facile a debugger
- Chaque skill fait exactement ce que VOUS voulez

## 16.6 Cas reel : Incident au spa (demo vocale)

- Bug arrive sur l'app, notification WhatsApp
- Envoie audio vocal 32 secondes via Telegram > bot cree l'issue
- Continue en mode vocal depuis le cafe sans ordinateur
- Code review, refactor, merge PR — tout par la voix
- Ne revient jamais a l'ordinateur

---

> **BIBLE COMPLETE — Formation compilee depuis 9 videos Melvynx — 16 parties**
> **Sources : TUTO 4h, Setup Terrence, OpenClaw x4, 1000h resume, TUE 3 Startup, VIBE CODEUR terminal**
> **Derniere mise a jour : 2026-03-26**
