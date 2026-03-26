# BIBLE MELVYNX — Claude Code Masterclass Complete
> Source : 3 fichiers fusionnes (Formation Complete + Setup 20min + Cours 4h)
> Date de compilation : 2026-03-26 — Mise a jour V2 : 2026-03-26
> Usage : Reference absolue avant toute question ou doute

---

## CHAPITRE 1 — ARCHITECTURE FONDAMENTALE

### Qu'est-ce que Claude Code ?
- Agent autonome (PAS un chat one-shot) fonctionnant en boucle : instruction → outils → action → verification → repetition
- 3 produits Anthropic distincts : Claude Chat (one-shot), Claude API (one-shot), Claude Code (agentique)
- Outils disponibles : Bash, Edit, Write, Read, WebFetch, WebSearch, Task (sub-agents), MCP
- Le modele genere token par token — a chaque token, tout le contexte est retraite
- Analogie Melvynx : Claude Code = un chef cuisinier a qui tu donnes les outils (cuisine, frigo, poele) + une consigne. Il enchaine les actions sans que tu aies a intervenir.

### Applications creees par Melvynx avec Claude Code
- **Umail.io** : outil de newsletter concurrent a MailChimp (2M+ emails envoyes)
- **SaveIt.Now** : gestionnaire de signets avec IA (screenshot + analyse)
- **Subfast** : generateur de miniatures YouTube
- **Chao.App** : chatbot integrable sur site, cree SANS ecrire une seule ligne de code
- **PaddleTally** : app iOS + Apple Watch pour compter les points au Paddle (jamais code iOS/Watch avant)
- Capable de travailler sur des codebases existantes de 4+ ans
- Dossier `~/cc/` pour gerer sa vie : titres YouTube, slides, analyses, comptabilite, todo lists

### Principe EPCT (workflow de base)
1. **Explore** : lancer des sub-agents (explore-codebase, explore-doc, web-search)
2. **Plan** : entrer en plan mode, planifier les modifications
3. **Code** : executer le plan
4. **Test** : verifier lint, tests, que tout fonctionne

### Regles absolues
- JAMAIS lancer Claude Code a la racine du systeme — toujours dans un dossier projet
- JAMAIS surcharger CLAUDE.md (max ~200-500 lignes) — deleguer dans rules/
- JAMAIS installer >3 MCP (degrade la qualite)
- JAMAIS bypass SANS deny-list configuree
- JAMAIS features complexes sans workflow (l'IA saute des etapes)
- JAMAIS dependre des plugins sans controler le code
- JAMAIS taguer de fichiers avec Apex/OneShot — laisser l'exploration les trouver
- JAMAIS repeter une commande qui echoue sans analyser l'erreur
- JAMAIS creer les skills/hooks/agents manuellement — laisser Claude les creer
- JAMAIS modifier settings.json manuellement — demander a Claude
- Si Claude fait une erreur 2+ fois : creer une regle dans .claude/rules/
- Utiliser CRITICAL: pour les regles les plus importantes
- Avant de modifier un fichier, lire au moins 3 fichiers similaires
- Apres tout changement, update le changelog avec la date du jour

---

## CHAPITRE 2 — LE TERMINAL POUR VIBE CODEURS

### Concepts de base
- Le terminal est une page pour acceder au systeme de commande de l'ordinateur
- Chaque dossier a un **chemin** (path) — on le voit avec `pwd`
- Le `~` (tilde) represente le dossier utilisateur (home)
- Le terminal peut se "deplacer" dans les dossiers comme le Finder

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
- **Bash/ZSH** : utilise sur macOS et Linux — c'est ce qu'il faut apprendre
- **PowerShell** : utilise sur Windows — different de Bash
- **WSL** (Windows Subsystem for Linux) : pont qui permet d'utiliser Bash sur Windows
- Recommandation : sur Windows, installer WSL

### Astuce copie chemin macOS
Dans le Finder, clic droit sur un dossier > maintenir `Option` > "Copier comme chemin". Puis `cd <coller>`.

### Premier projet demo
```bash
npm create vite@latest     # creer un projet (choisir React, TypeScript)
cd mon-premier-projet
npm install
npm run dev                # localhost:5173
```

---

## CHAPITRE 3 — CONFIGURATION

### settings.json — Mode recommande
```json
{
  "defaultMode": "bypassPermission",
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "toolSearchMode": "auto"
}
```
- **Bypass Permission** (mode rouge) : supprime toutes les demandes de validation
- **Deny-list** : meme en bypass, les commandes deny sont bloquees (rm -rf, sudo rm, etc.)
- **Double securite** : deny-list dans settings.json + "utilise trash au lieu de rm" dans CLAUDE.md
- **settings.local.json** : regles locales (non partagees avec l'equipe). Exemple : Prettier en PostToolUse local

### Hooks recommandes (3 hooks standard Melvynx)

| Hook | Declencheur | Commande | Usage |
|------|------------|---------|-------|
| PreToolUse | Avant execution outil | `bun command-validator/src/cli.ts` | Valide les commandes, bloque les dangereuses (AST-level) |
| Stop | Fin de tache | `afplay Glass.aiff` | Son pour savoir quand c'est fini |
| Notification | Claude a besoin d'attention | Son different du Stop | Distinguer "fini" vs "question" |
| PostToolUse (local) | Apres Edit/Write | Prettier automatique | Formater les fichiers edites |

- Hooks async (`async: true`) pour les actions non-bloquantes
- Ne pas surcharger de hooks (ralentit l'execution)
- UN SEUL hook PreToolUse recommande : command-validator (pas de faux positifs contrairement a grep)

### MCP — Maximum 2-3 (STRICT)

| MCP | Type | Usage | Cout |
|-----|------|-------|------|
| **Context7** | Documentation | Doc de n'importe quelle librairie, a jour | Gratuit (API key GitHub) |
| **Exa.ai** | Web Search | Recherche web optimisee pour l'IA, scrape + output pertinent | 20$ credit gratuit (~6$/5mois) |

- Au-dela de 3 MCP, la qualite des outputs se degrade
- Si >10% du contexte est pris par les MCP, Tool Search s'active automatiquement
- Installation globale : `claude mcp add --scope user ...`
- Pour verifier : `/mcp` dans Claude Code

### Blueprint CLI (installation rapide)
```bash
npx ig-blueprint-cli
```
Installe automatiquement : backup config, skills gratuits, command-validator, statusline, sons notification, permissions.
- Si ca ne marche pas du premier coup : `/debug` pour auto-reparation
- Premium : `npx aiblueprint-cli@latest claude-code pro activate <TOKEN>`

### Niveaux de configuration

| Niveau | Description | Comment |
|--------|------------|---------|
| **0** | Aucune config | Claude Code brut |
| **OK** | Claude Code de base | Installation standard |
| **OG (90%)** | Config gratuite Melvynx | `npx ig-blueprint-cli` |
| **OG++ (99%)** | Config premium Melvynx | Workflows avances, review securite |

### StatusLine
Affiche en permanence en bas du terminal :
- Dossier courant et repo git
- Modele utilise
- Cout de la session en $
- Tokens utilises et % contexte
- Graphique session/weekly limit
- Heures restantes dans la session
- Depenses du jour
- Necessite `bun` installe
- Si ne marche pas : `/debug` pour auto-reparer

---

## CHAPITRE 4 — WORKFLOWS ET COMMANDES

### /apex — Workflow principal (>99% succes)
LA commande principale de Melvynx. Phases :
1. Initialisation : lit fichier init, determine complexite
2. Exploration : lance sub-agents explore-codebase (1 a 3+ selon complexite)
3. Relecture contexte : confirme
4. Planification : cree un plan d'execution
5. Execution : implemente le plan
6. Validation : valide sa propre tache
7. (Premium) Review securite + clean code + coherence

| Flag | Raccourci | Effet |
|------|-----------|-------|
| --auto | -A | Mode automatique, aucune question |
| --examine | -e | Active la review |
| --test | -t | Lance les tests |
| --economy | -$ | Evite les sub-agents (economise tokens) |
| --branch | -b | Cree une branche git |
| --pr | -p | Cree une Pull Request |
| --interactive | -i | Mode interactif (menu flags) |
| --teams | -m | Mode equipe multi-agents |
| --resume | -r | Reprendre une tache interrompue |
| --plan | | Plan mode (sinon plan inline) |
| --save | | Save mode |

**Conseil cle** : ne PAS taguer de fichiers — laisser Apex explorer seul.

### /oneshot — Quick fix rapide
- Petites features, zero intervention, ne demande jamais l'avis
- Mode "vibe" : zero explication, zero tagage
- Cout tokens faible
- Fait beaucoup d'exploration et planification de maniere autonome

### /debug — Fix erreurs structure
Workflow en steps (prompt discovery) :
1. Init : initialiser parametres
2. Analyse : analyser l'erreur
3. Solutions : proposer plusieurs solutions
4. Fix : appliquer la solution
5. Verify : verifier

Techniques de debug :
- **Log technique** (LA PLUS EFFICACE) : demander a l'IA d'ajouter des console.log strategiques → reproduire le bug soi-meme → copier les logs console BRUTS → envoyer a Claude. Ne PAS interpreter les logs, ne PAS dire quoi faire — exposer le PROBLEME.
- **Screenshot annote** : CleanShotX pour montrer le bug visuellement avec carres rouges, fleches
- **Chrome Headless** : juge "lourd et lent" par Melvynx — preferer les logs manuels
- **Ne pas dire quoi faire** : exposer le PROBLEME, l'IA resout

### /brainstorm — Recherche profonde (4 rounds)
1. Expansive Exploration : explorer en profondeur (sub-agents)
2. Devil's Advocate : challenger tout
3. Synthese : condenser en 5 perspectives
4. Cristallisation : recommandations actionnables
TRES gourmand en tokens.

### /fix-errors — Correction parallele
Lance des sub-agents en parallele (jusqu'a 23) pour corriger plusieurs erreurs simultanement.
Ideal pour les builds Swift qui fail avec plein d'erreurs.

### /fix-grammar — Grammaire en bulk
Analyse tous les fichiers d'un dossier, lance des sub-agents par groupe de fichiers.
Usage : `/fix-grammar onboarding/` — lance des sub-agents sur chaque groupe de fichiers.

### /simplify — Review automatique (Nouveaute 2026)
Lance 3 agents de review en parallele. Review code pour reuse, qualite, efficacite. Corrige auto.

### /loop — Tache recurrente (Nouveaute 2026)
Cron interne qui expire quand la session se ferme.
Exemple : `/loop 2m check if PR tests pass, if no, fix it`
Cas d'usage : monitoring PR, verification tests, babysit deploys.

### /schedule — Taches planifiees persistantes (Nouveaute 2026)
Cron persistant (contrairement a /loop).
Exemple : "tous les matins a 9h, clean le dossier Downloads"

### /ultrathink — Reflexion profonde
Force le maximum de thinking tokens. Peut durer 2+ minutes. Pour problemes complexes.

### /commit — Commits simplifies
### /create-pr — Pull Request automatique
### /merge — Merge intelligent
### /fix-pr-comments — Corriger les commentaires PR
### /load-memory (ou /cloud-memory) — Optimise CLAUDE.md et rules
### /prompt-creator — Cree des prompts optimises (best practices Anthropic + OpenAI + Google)
### /skill-creator — Cree des skills personnalises (necessite Python pour init)
### /subagent-creator — Cree des sub-agents personnalises
### /review-code — Review multi-perspectives
### /clean-code — Principes clean code
### /ralph-loop — Coding autonome en boucle

### /batch — ANTI-PATTERN selon Melvynx
Lance agents en parallele avec work trees separees, chaque agent cree sa PR.
Melvynx est CONTRE : genere des dizaines de PR a review. Preferer un main agent orchestrateur.
Citation : "Je n'ai pas envie d'avoir une PR par petit bout de code."

---

## CHAPITRE 5 — COMMANDES SLASH NATIVES

| Commande | Action |
|----------|--------|
| `/clear` | Reset contexte (OBLIGATOIRE entre taches majeures) |
| `/init` | Generer CLAUDE.md du projet (conseil : dans un thread enrichi de contexte) |
| `/context` | Utilisation contexte en % |
| `/usage` | Limites et consommation |
| `/model` | Changer modele |
| `/agent` | Creer/gerer agents (create → personal folder → generate with cloud) |
| `/mcp` | Voir MCP connectes |
| `/plugin` | Installer plugins (2 sources : Anthropic Cloud Code + Anthropic Official Plugins) |
| `/stat` | Statistiques session |
| `/config` | Configuration interactive (tips, suggestions, progress bar) |
| `/voice` | Mode vocal (beta, mieux en anglais, ne remplace pas les outils de dictee) |
| `/loop` | Tache recurrente a intervalle |
| `/schedule` | Taches planifiees persistantes |
| `/simplify` | Review et simplification |
| `/copy` | Copier derniere reponse en Markdown |
| `/debug` | Auto-debug configuration Claude Code |

### Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| Escape x1 | Efface le texte courant |
| Escape x2 | Acces historique conversations (naviguer avec fleches, Enter, "Restore conversation") |
| Shift+Tab | Changer mode permissions (Normal → Accept Edit → Bypass) |
| Ctrl+T | Taches en cours |
| Ctrl+O | Transcript verbose (voir le thinking de l'IA) |
| Ctrl+S | Sauvegarder prompt (question intermediaire puis prompt revient automatiquement) |
| Meta+P | Changer modele |

### Prefixes dans le prompt
- `@` : taguer un fichier (@header pour src/components/header)
- `/` : commandes slash
- `!` : mode bash direct (!pnpm install)

---

## CHAPITRE 6 — CONTEXT ENGINEERING & MEMOIRE

### Contexte en chiffres
- **200 000 tokens max** par conversation
- **~27 000 tokens utilises au demarrage** (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- **System prompt** : ~3 500 tokens (1,7% du contexte)
- **Outils (tools)** : tres volumineux — chaque outil avec tous ses parametres et descriptions
- **Skills** : description de chaque skill injectee dans le prompt
- **MCP tools** : definitions injectees meme si non utilises
- **Autocompact a ~96-99%** du contexte (degrade la qualite, prend du temps)

### Debug du contexte avec Proxyman
- **Proxyman** : outil macOS pour intercepter les requetes HTTP de Claude Code
- Permet de voir exactement ce qui est envoye au modele a chaque requete
- Utile pour comprendre les 27K tokens de demarrage
- Hack : demander a Claude de creer un fichier HTML qui affiche toutes les infos du contexte

### Probleme "Lost in the Middle"
Les transformers donnent plus d'importance au DEBUT et a la FIN du contexte. Les tokens au milieu sont structurellement ignores. C'est un biais du transformer, pas un bug — il reflete le biais cognitif humain. Les chunks au milieu de contexte sont oublies, les instructions au centre sont ponderes 1/3 a 1/2.

**Impact concret** : Quand tu lances un workflow comme EPCT, le prompt initial a beaucoup d'importance. Mais apres exploration et planification, le prompt se retrouve "au milieu" du contexte et perd de l'influence.

### Solution : Prompt Discovery (Multi-step)
Au lieu de donner un gros prompt au debut qui sera oublie, decouper en etapes :
```
steps/
  step1-init.md
  step2-analyse.md
  step3-plan.md
  step4-execute.md
  step5-verify.md
```
Chaque etape lit son fichier juste avant d'agir — le prompt courant est toujours "recent". Resultat : l'IA respecte beaucoup mieux les instructions.

Pour creer un workflow avec prompt discovery : "j'ai envie que pour cette commande code tu fasses du prompt discovery avec des steps — on cree un dossier step et on separe chaque etape"

### 3 niveaux de memoire CLAUDE.md

| Niveau | Emplacement | Portee |
|--------|-------------|--------|
| Global | ~/.claude/CLAUDE.md | Tous les projets |
| Projet | ./CLAUDE.md (racine) | Ce projet uniquement |
| Dossier | ./src/components/CLAUDE.md | Charge quand l'IA lit un fichier du dossier |

### Rules (.claude/rules/)
- Fichiers .md charges automatiquement a chaque session
- Par defaut `alwaysApply: true`
- Avec **globs** : charges uniquement pour certains types de fichiers
  ```yaml
  ---
  globs: ["*.json"]
  ---
  Regles specifiques aux fichiers JSON...
  ```
- Permet de decharger CLAUDE.md : si >200 lignes, deplacer dans rules/
- Exemple : "Tu as fait cette erreur trop souvent. Cree un fichier dans .claude/rules/ qui t'empechera de la refaire."
- Regle optimale : `CRITICAL: Never create middleware.ts — middleware is proxy helper in proxy utils`

### Auto-Memory (autoMemoryEnabled: true)
- Claude s'ecrit des notes entre sessions
- Stocke dans `~/.claude/projects/<hash>/memory/`
- 200 premieres lignes de MEMORY.md chargees a chaque session
- **Conseil Melvynx** : preferer dire "modifie le CLAUDE.md avec cette info" plutot que laisser l'auto-memory sauvegarder n'importe quoi

### CLAUDE.md vs Auto-Memory

| | CLAUDE.md | Auto-Memory |
|---|---|---|
| **Qui ecrit** | L'humain | Claude |
| **Contenu** | Instructions, regles | Patterns, apprentissages |
| **Scope** | Projet ou global | Par working directory |
| **Controle** | Total | A verifier regulierement |

### Commentaires inline comme "prompt injection" benefique
Documenter les helpers internes (ZodRoute, Upfetch, etc.) avec des exemples d'utilisation detailles dans les commentaires du code. Quand la regle "lire 3 fichiers similaires" est active, l'IA absorbe ces instructions automatiquement et reproduit les patterns corrects.

### Changelog automatique
Regle dans CLAUDE.md : `CRITICAL: Apres tout changement, update le changelog avec la date du jour.`

### Gestion du contexte — Regles
- `/clear` OBLIGATOIRE entre les taches majeures
- `/context` regulierement pendant les longues sessions
- Si contexte > 80% : `/clear` immediat
- Langage naturel pour petites modifs (economise le contexte)
- Sub-agents pour les recherches (preservent le contexte principal)
- Maximum 2-3 MCP actifs
- Attention aux mauvais chats : avec plusieurs terminaux ouverts, erreur frequente d'envoyer un prompt dans le mauvais terminal

### Structure du dossier .claude/

| Dossier | Contenu |
|---------|---------|
| projects/ | Historique sessions (transcripts recuperables — "analyse le dossier project pour retrouver la session qui parle de X") |
| plans/ | Tous les plans generes en plan mode |
| rules/ | Regles persistantes globales |
| skills/ | Skills actifs (deplacer dans sous-dossier "disabled" pour desactiver) |
| agents/ | Agents custom (idem, sous-dossier "disabled" pour desactiver) |
| commands/ | Ancien nom des skills |
| scripts/ | Scripts d'automatisation (hack : lancer Claude Code dans ~/.claude/ pour gerer sa propre config) |
| teams/ | Equipes creees (JSON) |
| tasks/ | Toutes les taches executees |

---

## CHAPITRE 7 — SUB-AGENTS & TEAMS

### Sub-agents — Protection du contexte
- Une recherche web = ~28K tokens. 3 recherches = 84K (proche de la limite)
- Le sub-agent fait la recherche dans SON propre contexte (120K tokens) et retourne ~1500 mots au main agent
- La description du sub-agent determine QUAND l'IA l'utilise (injectee dans le prompt)
- Apres creation d'un sub-agent, relancer Claude Code pour qu'il le detecte

### Types de sub-agents

| Type | Usage | Vitesse |
|------|-------|---------|
| Explore | Recherche, exploration | Rapide |
| Task | Actions generiques | Normal |

### 3 sub-agents recommandes
1. **Web Search** : WebSearch + WebFetch + Exa MCP — description : "use when you need to quickly find information"
2. **Explore Codebase** : Read + Grep + Search
3. **Explore Doc** : Context7 MCP — specialise documentation

### Creation d'un agent custom (exemple live)
1. `/agent` > Create New Agent > Personal Folder > Generate with Cloud
2. Decrire la tache (ex: "prend un fichier en parametre, corrige la grammaire, retourne le resultat")
3. Assigner un modele (Sonnet pour economiser)
4. Assigner une couleur
5. Le main agent peut ensuite lancer cet agent en parallele sur N fichiers

### Teams (Agent Teams)
- Activer : `enableAgentTeams: true`
- Le main agent (team lead) cree des equipes, assigne des taches via task list partagee
- Les teammates travaillent en parallele
- Interface : fleche bas pour switcher entre agents (main, dev1, dev2, etc.)
- Les teams utilisent Tmux pour splitter les agents en tabs
- **QUAND utiliser** : zones de modification SEPAREES (monorepo back/front/mobile)
- **NE PAS utiliser** : memes fichiers — les agents se "battent"
- **Etat actuel** : les agents communiquent peu entre eux, surtout via le team lead. Pas encore tres cooperatifs.

### Work Trees
- Travailler sur des branches separees en parallele
- Workflow : creer work tree → feature → PR → merge → detruire le work tree
- **Maximum 4 work trees simultanes**
- Reserver aux gros refactors structurels (ex: changer toute la database de Postgres a Convex)
- Ne PAS utiliser pour de petites features — travailler directement dans le meme repo
- Attention au merge si les work trees touchent des schemas differents
- Astuce CLAUDE.md : "Si il y a des erreurs TypeScript que tu n'as pas directement implementees, c'est pas ton probleme, passe a la suite."
- Melvynx : "Je n'utilise jamais les work trees sauf pour des features qui vont structurellement changer mon application"
- Piege : les agents demandent chacun ton attention → tu te retrouves avec des agents qui travaillent 25% du temps

---

## CHAPITRE 8 — PROMPTING TECHNIQUES

### Greenfield (nouveau projet)
1. Specifier la stack : "create a new vite project with shadcn and tailwind"
2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier la technique (camelCase etc.) au premier run — laisser l'IA choisir
5. Utiliser une boilerplate existante pour imposer les conventions (ex: **NaotS.app** de Melvynx avec regles strictes)
6. Si preferences techniques : les mettre dans CLAUDE.md global APRES le premier run, pas avant
7. Exemple Melvynx : Timezone Checker cree en un seul prompt avec Vite + React

### Brownfield (projet existant)
1. Screenshots annotes (CleanShotX) — montrer exactement le probleme avec dessins, fleches, carres rouges
2. Donner des references internes : "style comme illustration-picker" ou "video-picker"
3. Donner les ressources : chemins, images, URLs
4. Les screenshots permettent a l'IA de VOIR — ameliore drastiquement le front-end
5. L'IA suit automatiquement les conventions du code existant

### Debugging
1. **Screenshot + description** : "j'ai mis le sleep time a 23h et pourtant le 23 n'est pas selectionne" — l'IA resout automatiquement
2. **Log technique** (LE PLUS EFFICACE selon Melvynx) :
   - Demander a l'IA d'ajouter des `console.log` strategiques
   - Reproduire le bug soi-meme
   - Copier les logs **bruts** de la console (pas besoin de les interpreter)
   - Envoyer a Claude — il comprend et corrige
3. **Chrome Headless** : juge "lourd et lent" par Melvynx — preferer les logs
4. **Ne pas dire quoi faire** : exposer le PROBLEME, l'IA resout

### Technique des 10 variations UI
Prompt exact a utiliser :
```
Cree un fichier /demo/page.tsx avec 10 variations UI/UX de cette interface,
10 composants separes dans 10 fichiers separes, importes dans la page.
```
Exemples de variations : dialogue classique, drawer avec switch, multi-action dialogue, split view, accordion, wizard multi-etapes, style Linear, etc.
- Tester chaque variation, choisir la meilleure
- **Supprimer le dossier `/demo/`** apres avoir choisi la version finale

### Processus iteratif front-end
Screenshot → demander modif → re-screenshot → ajustement → etc.
Donner la vision visuellement = le plus grand hack pour le code front-end.

### Petites modifications
- Langage naturel directement, PAS de commandes/skills
- Les commandes consomment du contexte inutilement pour les petites demandes

### Eviter les erreurs repetitives de l'IA
1. **Rules dans .claude/rules/** : creer une regle specifique quand erreur 2+ fois
2. **Workflow dans CLAUDE.md** : "Avant de modifier un fichier, lire 3 fichiers similaires"
3. **Rules ciblees avec globs** : regle specifique pour un type de fichier (ex: `api-route.md` avec `path: app/**/route/**`)
4. **Commentaires inline** : documenter les helpers internes avec exemples — l'IA les lit automatiquement
5. **Changelog automatique** : `CRITICAL: Apres tout changement, update le changelog`

### Anti-patterns prompting
- Ne PAS relancer un skill pour une petite modif — langage naturel
- Ne PAS empiler les demandes pendant que l'IA travaille (ca peut la perturber)
- Ne PAS utiliser Chrome Headless (Melvynx le juge lourd et lent)
- Attention aux mauvais chats avec plusieurs terminaux ouverts

---

## CHAPITRE 9 — SECURITE

### Modes de permission (Shift+Tab)
1. Normal : demande validation a chaque etape
2. Accept Edit : pas de validation pour les modifs fichiers
3. **Bypass Permission** (rouge) : aucune question — RECOMMANDE par Melvynx

### Deny List — Meme en bypass, ces commandes sont bloquees
- `rm -rf`, `sudo rm`, commandes destructives
- Erreur affichee : "Permission to use bash with command rm -rf denied"
- Double securite : deny-list + CLAUDE.md ("utiliser trash au lieu de rm")

### Plugins — ATTENTION
- Deux sources : "Anthropic Cloud Code" et "Anthropic Cloud Official Plugins"
- Installation : `/plugin` dans Claude Code
- **Probleme** : installent du code non controle dans un dossier non modifiable
- Se mettent a jour automatiquement (ecrasent vos modifs)
- **RECOMMANDATION forte de Melvynx** : copier le code dans vos propres skills/agents
- Citation : "Volez les concepts, creez votre propre configuration"

### Command Validator (PreToolUse hook)
- Script TypeScript/Bun qui valide les commandes au niveau AST
- Pas de faux positifs (contrairement a grep)
- Bloque rm -rf, sudo, dd, privilege escalation, remote execution

### Astuce bypass VPS
```bash
export SANDBOX=1  # dans .bashrc
SANDBOX=1 claude  # Claude croit etre dans une sandbox, accepte le bypass
```

---

## CHAPITRE 10 — PRICING & LIMITES

### Abonnements

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| **Max 20x** | **200$** | **~3 200$** | **x16 (meilleur ratio)** |

- API directe : Opus = 15$/M input + 75$/M output (peut monter tres vite)
- **Recommandation Melvynx** : Max 20x (200$/mois) pour usage pro — pour chaque euro depense, deux fois plus d'utilisation que le Max 5x

### Sessions et limites
- Sessions de **5 heures**, reset toutes les 5h (~4.5 sessions/jour)
- **Session limit** : pourcentage d'utilisation dans la session (plus atteignable)
- **Weekly limit** : limite hebdomadaire globale (rarement atteinte avec Max 20x)
- Si session limit atteinte : attendre ~1h pour reset
- Si weekly limit atteinte : bloque jusqu'a la semaine suivante
- Verifier sur claude.ai > Settings > Usage

### Quand monter d'abonnement
- Pro (20$) : usage hobby/fun, longs espaces entre sessions
- Max 5x (100$) : usage professionnel regulier
- Max 20x (200$) : usage intensif, plusieurs projets simultanes, freelance/CDI

---

## CHAPITRE 11 — OUTILS RECOMMANDES

### Stack technique Melvynx
- **Supabase** : backend/BDD pour SaaS (auth + database)
- **Vercel** : deploiement production (Next.js) — vrais projets
- **Netlify Drop** : demos rapides (drag & drop dist/) — prototypage

### Deploiement rapide avec Netlify Drop
1. Demander a Claude : "prepare un dossier pour deployer sur Netlify Drop"
2. Claude genere un dossier `dist/`
3. Aller sur `app.netlify.com/drop`
4. Drag & drop le dossier `dist/`
5. URL publique instantanee

### Terminaux recommandes
- **Ghostty** (ghosty.org) : terminal optimise, gratuit, minimaliste. Raccourcis : `Cmd+1/2/3` tabs, `Cmd+W` fermer, `Cmd+T` nouveau tab
- **CEMUX** : terminal cree pour les coding agents (4.7 stars GitHub). Build au-dessus de Ghostty. Tabs verticaux + horizontaux, notifications quand agent a besoin d'attention, browser integre, split pane, scriptable. Build en Swift/AppKit (pas Electron). Gratuit.
- **Hyper** : alternative populaire

### Outils macOS
- **Raycast** : remplacant de Spotlight (gratuit sauf IA). App launcher, clipboard history, emoji picker, window management, screenshot. Extensions : VS Code, Figma, GitHub, Tailwind, iOS. Remplace `Cmd+Espace`.
- **CleanShotX** : screenshots annotes avec dessins, fleches, carres rouges (disponible dans SetApp)
- **SetApp** : bundle d'outils macOS incluant CleanShotX
- **WhisperTurbo** : app de Melvynx, speech-to-text local gratuit et open source. Modele PRK (Parakeet) : rapide pour <20s. Modele Whisper Large : meilleur pour longs enregistrements et francais.
- **Homebrew** : gestionnaire de paquets macOS (`brew install <paquet>`)

### Visual Studio Code
- Telecharger sur code.visualstudio.com
- Installer la commande CLI : `Cmd+Shift+P` > "Shell Command: Install code in PATH"
- Terminal integre : menu Terminal > New Terminal

### tmux essentiels

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

Cas d'usage : Un tab par role — CC1 (feature), CC2 (review), Server, Ingest, etc.

### Dossier "CC" — Fourre-tout Claude Code
`~/cc/` (accessible via alias `cdcc`) pour tout faire hors projets :
- Generateur de titres YouTube
- Createur de slides (site web)
- Analyses business, reviews d'emails
- Gestion de projets, comptabilite, todo lists
- **Philosophie** : "Est-ce que Claude Code peut le faire ? Si oui, pas besoin de SaaS."

---

## CHAPITRE 12 — OPENCLAW (Agent distant Telegram)

### Concept
Controler son ordinateur a distance via Telegram (texte + audio). Lance des agents Claude Code en arriere-plan, notifie quand termine. Le bot ne gere qu'une session a la fois (`/new` pour nouvelle session).

### Cas d'usage reels
- A la piscine/salle de sport : envoyer un audio Telegram pour fixer un bug
- Controle de plateformes (coupons, abonnes, emails) via Telegram
- Planification de tweets depuis Telegram
- Lancer des features, creer des PR, merger du code — tout depuis le telephone

### Installation locale
```bash
curl maltbot.sh
```
Choisir Claude Code CLI comme backend. Autoriser via URL navigateur. Configurer BotFather (Telegram > /newbot > copier token API).

### Installation VPS (Docker)
```bash
npx openclaw-vps-setup
```
Installe : Node.js, OpenClaw, Claude Code, Bun, Cloudflared, GCloud + options securite (UFW, Fail2Ban, SSH Hardening).

### Installation manuelle VPS (etapes completes)
1. Creer cle SSH : `ssh-keygen -t ed25519 -f ~/.ssh/hetzner3`
2. Configurer `~/.ssh/config` avec keep-alive (ServerAliveInterval 60)
3. `ssh hetzner3` → installer Docker (`curl -fsSL https://get.docker.com | sh`)
4. Cloner OpenClaw, creer `.env`, installer Claude Code sur le VPS
5. `docker compose build && docker compose up -d`
6. Onboarding dans Docker : `docker compose exec openclaw-gateway bash && node dist/index.js onboard`
7. Pairing Telegram : `node dist/index.js pairing approve telegram <CODE>`
8. Dashboard : `node dist/index.js dashboard` (SSH tunnel pour acces distant)
9. Approuver devices : `node dist/index.js devices list && devices approve <id>`

### VPS recommande
Hetzner CAX21/CAX31 (ARM, ~$7.59/mois). IPv4 obligatoire.

### Workflow complet depuis Telegram
1. Message/audio Telegram avec description feature
2. Skill "code_task" : clone worktree → npm install → GitHub Issue → lance `claude -p --dangerously-skip-permissions apex --pr <issue> <description>`
3. Schedule cron job (verifie chaque minute si termine)
4. Notification Telegram avec lien PR
5. "Merge la PR et delete le worktree"

### Philosophie Skills > MCP pour OpenClaw
Un agent Claude peut directement fetch n'importe quelle API publique. Skill = SKILL.md + execute.sh. Plus leger, plus simple, plus controlable que MCP.

### Skills OpenClaw (liste Melvynx)
Cloudflare, Code process, CodeLine, Context7, Dub (URL shortener), FlightData, Front (support client), Google Image, LinkedIn Post, Lumail, Mercury (bancaire), Porkbun (domaines), SaveIt, TypeFully (tweets), Vercel, YouTube Comments.

### Gmail + Calendar
1. Creer projet Google Cloud → activer APIs Gmail/Calendar dans Marketplace
2. Creer Credentials > OAuth > Application de bureau
3. Telecharger JSON credentials → donner a OpenClaw via Telegram
4. Cloudflare Tunnel pour webhooks Gmail (necessite domaine + Cloudflared)
5. Cron jobs utiles : "tous les jours a 8h, lis mes emails et fais une review"

### Audio bidirectionnel avec ElevenLabs
- Envoi audio depuis Telegram : transcription automatique
- Config ElevenLabs pour voix personnalisee en retour
- Discussion vocale bidirectionnelle via Telegram

### Modeles recommandes OpenClaw

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2 input / $12 output |
| Cron jobs | Gemini 3 Flash | Moins cher (ideal watchers) |
| Agents de code | Claude Opus | Meilleure qualite (abonnement CC) |

### Fichiers de configuration

| Fichier | Role |
|---------|------|
| `soul.md` | Identite/personnalite du bot (nom, timezone, site web) |
| `memory/` | Memoire quotidienne (souvenirs entre sessions) |
| `canvas/index.html` | Interface web locale |
| `.env` | Variables d'environnement |

### Securite OpenClaw
- Gateway ouvre port localhost (18789) — potentiellement accessible
- `claudebot security-audit` pour verifier les vulnerabilites
- Docker obligatoire pour sandboxing
- Ne PAS activer 1Password sans reflexion
- Telegram plus securise que l'interface web
- Machine dediee recommandee : Mac Mini (~600 EUR) pour 24/7

### OC Pro
Produit payant de Melvynx avec script automatise qui fait tout le setup en 20 min.

---

## CHAPITRE 13 — META-SKILLS & SKILLS AVANCES

### Meta-skills (prompts qui creent des prompts)
- /prompt-creator : cree des prompts IA (best practices Anthropic + OpenAI + Google)
- /cloud-memory : gere CLAUDE.md et rules
- /skill-creator : cree des skills (necessite Python pour init)
- /subagent-creator : cree des sub-agents
- meta-hook-creator : cree des hooks
- meta-skill-workflow-creator : cree des skills workflow

### Comment creer un meta-skill
1. Utiliser le skill-creator : `utilise /skill-creator pour creer un nouveau skill qui s'appelle meta prompt creator`
2. Demander a rechercher les best practices : "recherche sur internet les meilleures techniques de prompt proposees par Anthropic, OpenAI, Google"
3. Le skill cree un dossier avec fichiers de references (identity, objectif, patterns, templates)
4. Ensuite utiliser `/prompt-creator` dans n'importe quel projet pour creer des prompts optimises

### Structure d'un skill
- **Petit skill** : un seul fichier SKILL.md (analogie : une seule page dans un livre de recettes)
- **Grand skill** : dossier avec SKILL.md (entry point = table des matieres) + references/ + scripts/
- Seul l'index est charge, les sous-fichiers lus a la demande (token-efficient)
- Le fichier SKILL.md c'est comme donner un livre de cuisine au chef — il navigue les pages au lieu de tout memoriser

### Installation de skills externes
```bash
npx skill add <auteur>/<nom>
```
Source : skill.sh (marketplace). Choisir "global" pour cross-projets. Melvynx prefere skill.sh aux plugins pour garder le controle du code.

### Repo GitHub officiel Melvynx (aiblueprint)
15 skills : apex, claude-memory, commit, create-pr, fix-errors, fix-grammar, fix-pr-comments, merge, oneshot, prompt-creator, ralph-loop, skill-creator, subagent-creator, ultrathink, workflow-apex-free
3 agents : explore-codebase, Snipper, websearch

### Debug de la config
- `/debug` : auto-debug et auto-reparation (teste les scripts, la StatusLine, etc.)
- La StatusLine et les scripts ont des tests integres
- Lancer Claude Code dans ~/.claude/ pour gerer sa propre config

### Debug du contexte avec Proxyman
Intercepte les requetes HTTP de Claude Code. Permet de voir exactement ce qui est envoye : system prompt (~3 500 tokens), descriptions outils (tres volumineux), skills, MCP, CLAUDE.md, rules.

---

## CHAPITRE 14 — NOUVEAUTES 2026

### Claude Review (beta entreprise)
- Review automatique des PR avec process multi-agents en parallele
- Resultats :
  - PR avec reviews passent de **16% a 54%**
  - Moins de **1%** des reviews marquees incorrectes
  - **94%** des bugs trouves sur les grandes PR
- Cout : **15-25$ par review** (token usage, augmente avec complexite)
- Optimise pour les equipes, pas les individuels

### Voice Mode
- `/voice` dans Claude Code
- Fonctionne mieux en anglais
- Ne remplace PAS les outils de dictee natifs (Super Whisper, etc.)

### Ask User Question ameliore
- Peut proposer des UI avec exemples de code dans le terminal
- Choix multiples : selectionner une option et appuyer sur `N` pour ajouter une note avant d'envoyer
- "Update parfaite" selon Melvynx — ca "vient juste marcher"

### /simplify (3 agents review)
Lance 3 agents de review en parallele pour reuse, qualite, efficacite. Corrige auto.

### /loop (cron interne)
Cron qui expire quand la session se ferme. `/loop 2m check if PR tests pass`

### /schedule (cron persistant)
Contrairement a /loop, persiste entre sessions. "Tous les matins a 9h, clean Downloads"

---

## ANTI-PATTERNS COMPLETS (Liste definitive)

1. JAMAIS lancer Claude Code a la racine du systeme
2. JAMAIS surcharger CLAUDE.md (max ~200-500 lignes)
3. JAMAIS installer >3 MCP
4. JAMAIS bypass SANS deny-list
5. JAMAIS features complexes sans workflow
6. JAMAIS dependre des plugins sans controler le code
7. JAMAIS utiliser /batch (preferer main agent + sub-agents)
8. JAMAIS empiler les demandes pendant que l'IA travaille
9. JAMAIS taguer les fichiers avec Apex/OneShot
10. JAMAIS utiliser Teams sur une seule zone de code
11. JAMAIS laisser l'auto-memory sauvegarder n'importe quoi
12. JAMAIS specifier la technique au premier run Greenfield
13. JAMAIS lancer plus de 4 terminaux/work trees simultanes
14. JAMAIS utiliser work trees pour de petites features
15. JAMAIS laisser les plugins controler votre config
16. JAMAIS relancer des skills pour petites modifs (langage naturel)
17. JAMAIS creer skills/hooks/agents manuellement
18. JAMAIS modifier settings.json manuellement
19. JAMAIS utiliser Chrome Headless pour debug (lourd et lent)
20. JAMAIS attendre des agents teams qu'ils communiquent entre eux (ils sont independants)
