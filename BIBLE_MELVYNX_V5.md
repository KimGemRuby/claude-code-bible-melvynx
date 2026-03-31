# BIBLE MELVYNX — Claude Code Masterclass Complete
> Version V5.0 — 2026-03-31 — 32 chapitres
> Sources : 36+23+11 videos/transcriptions + documentation officielle OpenClaw + recherche web 2026
> Fusion V4.1 + 8 nouvelles videos Melvynx + Guide OpenClaw + Guide CC complet + Config Mac
> Chapitres 1-22 : V4.1 originale (inchangee) + sections ajoutees en complement
> Chapitres 23-29 : Nouvelles videos Melvynx 2026
> Chapitre 30 : Guide OpenClaw 100%
> Chapitre 31 : Guide Complet Claude Code
> Chapitre 32 : Configuration Mac Kim13 Complete

## CHAPITRE 1 — ARCHITECTURE FONDAMENTALE

### Qu'est-ce que Claude Code ?
- Agent autonome (PAS un chat one-shot) fonctionnant en boucle : instruction → outils → action → verification → repetition
- 3 produits Anthropic distincts : Claude Chat (one-shot), Claude API (one-shot), Claude Code (agentique)
- 3 acteurs : Utilisateur → Claude Code (software/middleware) → Claude API (modele)
- Le middleware injecte un system prompt + liste des tools. Le modele retourne un "tool call". Claude Code execute, retourne le resultat, boucle jusqu'a terminaison.
- Tools par defaut : Bash, Edit, Write, Read, Glob, Grep, LS, MultiEdit, SubAgent, Task, Fetch, WebSearch
- MCP = tools custom supplementaires injectes dans le system prompt
- Le modele genere token par token — a chaque token, tout le contexte est retraite
- Analogie Melvynx : Claude Code = un chef cuisinier a qui tu donnes les outils (cuisine, frigo, poele) + une consigne. Il enchaine les actions sans que tu aies a intervenir.

### Applications creees par Melvynx avec Claude Code
- **Umail.io** : outil de newsletter concurrent a MailChimp (2M+ emails envoyes)
- **SaveIt.Now** : gestionnaire de signets avec IA (screenshot + analyse)
- **Subfast** : generateur de miniatures YouTube
- **Chao.App** : chatbot integrable sur site, cree SANS ecrire une seule ligne de code
- **PaddleTally** : app iOS + Apple Watch pour compter les points au Paddle (jamais code iOS/Watch avant)
- **WhisperTurbo** : app speech-to-text local gratuit et open source
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
- Ne PAS forcer l'usage de sub-agents via des regles globales — preferer des workflows specifiques a la demande. Parfois on veut que l'IA fasse elle-meme sans deleguer.

### Les "Core 7" de Claude Code (terminologie Melvynx)
Les 7 fonctionnalites qui font de Claude Code ce qu'il est :
1. Commandes/Skills 2. Hooks 3. Memoire (CLAUDE.md + rules/) 4. MCP 5. StatusLine 6. Sub-agents 7. Context Management
- Apres tout changement, update le changelog avec la date du jour

---

## CHAPITRE 2 — LE TERMINAL POUR VIBE CODEURS

### Concepts de base
- Le terminal est une page pour acceder au systeme de commande de l'ordinateur
- Analogie Minecraft : les commandes slash dans le chat (/give, /help) = meme concept que le terminal
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

## CHAPITRE 3 — INSTALLATION & CONFIGURATION

### Installation
```bash
# macOS / Linux
curl -fsSL https://cli.anthropic.com/install.sh | bash
# Windows PowerShell
irm https://cli.anthropic.com/install.ps1 | iex
# Verification
claude -v
```
- Relancer un nouveau terminal si `claude` n'est pas reconnu
- Ne JAMAIS lancer Claude a la racine du systeme → toujours dans un dossier projet
- Faire `/terminal setup` pour configurer les keybindings au premier lancement

### Connexion
- Deux modes : API key (pay-per-use) ou Claude account subscription (forfait)
- Extension VS Code/Cursor : installe l'extension Claude Code pour lier l'IDE. `Cmd+Option+K` envoie le code selectionne.

### settings.json — Mode recommande
```json
{
  "defaultMode": "bypassPermission",
  "permissions": {
    "allow": ["pnpm *", "git *"],
    "deny": ["rm -rf", "sudo rm", "sudo", "dd", "chmod 777"]
  },
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "autoCompact": false,
  "toolSearchMode": "auto",
  "includeCoAuthorBy": false
}
```

#### Modes de permission (Shift+Tab pour changer)
| Mode | Comportement |
|------|-------------|
| default | Demande permission pour tout |
| acceptEdits | Auto-accepte modifications fichiers, demande le reste |
| plan | Ne peut ni modifier ni executer, propose un plan |
| **bypassPermissions** | Tout autorise (recommande + deny list) |

- **Bypass Permission** (mode rouge) : supprime toutes les demandes de validation
- **Deny-list** : meme en bypass, les commandes deny sont bloquees (rm -rf, sudo rm, etc.)
- **Double securite** : deny-list dans settings.json + "utilise trash au lieu de rm" dans CLAUDE.md
- **settings.local.json** : regles locales (non partagees avec l'equipe). Ex: Prettier en PostToolUse local
- **allow** : commandes autorisees sans demande (wildcards supportes)
- **includeCoAuthorBy** : false pour enlever le co-author Claude dans les commits

#### Tool Search (gestion MCP/contexte)
- Si MCP < 2-3% du contexte : `toolSearchMode: false` (Claude utilise mieux vos MCP)
- Si MCP > 2-3% du contexte : `toolSearchMode: true` (economise du contexte)
- Quand tool search actif, Claude a tendance a NE PAS utiliser vos MCP — prefere ses outils internes

#### Autocompact
- Buffer autocompact = 45 000 tokens reserves (~22% du contexte)
- Se declenche a ~96-99% → compacte en resume (degrade la qualite)
- **Recommandation** : desactiver autocompact (`autoCompact: false`), utiliser `/clear` entre les taches
- Gain : 45 000 tokens recuperes, passe de 62% a 83% d'espace libre
- Seul cas utile : si on arrive a la limite AU MILIEU d'une tache

### Hooks recommandes (3+1 hooks standard Melvynx)

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
- **Strategie Melvynx** : desactiver TOUS les MCP par defaut et ne les activer qu'explicitement quand necessaire (toggle avec `#` devant le nom dans mcp.json)

| MCP | Type | Usage | Cout |
|-----|------|-------|------|
| **Context7** | Documentation | Doc de n'importe quelle librairie, a jour | Gratuit (API key GitHub) |
| **Exa.ai** | Web Search | Recherche web optimisee pour l'IA, scrape + output pertinent | 20$ credit gratuit (~6$/5mois) |

- Au-dela de 3 MCP, la qualite des outputs se degrade
- Si >10% du contexte est pris par les MCP, Tool Search s'active automatiquement
- Installation globale : `claude mcp add --scope user ...`
- Pour verifier : `/mcp` dans Claude Code
- **Chaque MCP consomme du contexte MEME sans etre utilise** (definitions injectees)
- Desactiver MCP inutilises via `#` (raccourci) > entrer pour toggle

#### Migration Context7 : MCP → CLI + Skill (methode avancee)
- `claude mcp remove context7`
- `npx ctx7-latest-setup` → choisir "CLI + Skill" au lieu de "MCP Server"
- Le skill ajoute une description chargee UNIQUEMENT quand necessaire (vs MCP charge a chaque conversation)

#### MCP vs Skills — Distinction fondamentale
- Les **MCP** ajoutent des **OUTILS** (tool calls) : le modele peut appeler des fonctions externes
- Les **Skills** ajoutent des **CONNAISSANCES** (prompts structures) : le modele recoit des instructions et du contexte
- Les skills sont **plus token-efficient** : seule la description est chargee, le contenu est lu a la demande
- Les MCP sont charges en permanence dans le contexte, les skills ne le sont que quand necessaire

#### CLI > MCP pour la plupart des cas
- Preferer les CLI natifs : Supabase CLI, PostgreSQL CLI, GitHub CLI (`gh`), Neon CLI, Vercel CLI
- Bash = lister fichiers, lire contenu, pipe, head, tail — controle total sur l'output
- Avantage CLI : `context7 query ... | head -150` — ne recupere que 150 lignes (impossible avec MCP qui retourne un bloc fixe)
- Les modeles sont deja entraines sur ces CLI

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
- Dossier courant et repo git (+ fichiers ajoutes/supprimes/staged)
- Modele utilise
- Cout de la session en $
- Tokens utilises et % contexte
- Graphique session/weekly limit avec barre visuelle
- Heures restantes dans la session
- Depenses du jour
- Necessite `bun` installe (`brew install bun`)
- Si ne marche pas : `/debug` pour auto-reparer (la StatusLine contient des tests integres que /debug utilise)
- Parametre `usableContextOnly` : soustrait les 45000 tokens d'autocompact du total pour afficher le contexte reellement disponible
- Barre de progression avec couleurs progressives (change de couleur selon l'usage) + taille configurable (`showProgressBar: true/false`)

#### Technique interne StatusLine
- Lecture du fichier transcript JSON de Claude Code (genere automatiquement)
- Calcul tokens : input_token + cache_read_input_token + cache_creation_input_token
- Soustraire 45 000 tokens autocompact du max pour le contexte reellement disponible
- Recuperation usage API : `security find-generic-password -s "claude-code-credential"` puis fetch `api.anthropic.com/api/auth/usage`

### Output Styles (personnalisation du comportement)
- Permet d'adapter Claude Code au-dela du Software Engineering (Assistant, Senior Dev, etc.)
- Remplace le system prompt standard et l'oriente vers d'autres roles
- Fichiers de configuration : `~/.claude/output-styles/` (ex: `Honest.md`, `Senior Dev.md`)
- Commande d'activation : `/output-style`
- Applique au niveau du projet (`settings.local.json`)
- Impact reel : difference massive de tonalite et de longueur selon le style choisi
- Remplace ce que faisait "Append System Prompt" mais de maniere plus complete

### Hack bypass permission VPS
Claude refuse le bypass permission sur VPS. Hack :
```bash
SANDBOX=1 claude
# Persister dans .bashrc :
export SANDBOX=1
```

### Alias YOLO mode
```bash
alias cc="claude --dangerously-skip-permissions"
```
Ajouter dans `.bashrc` ou `.zshrc` pour persistance. Permet de lancer Claude en bypass avec juste `cc`.

---


## [Section ajoutee au Chapitre 3 — Settings caches et avances]

### Settings caches (non documentes dans le Ch.3 original)

Ces settings sont documentes en Ch.22.4 mais absents du Ch.3 ou ils devraient etre :

| Setting | Valeur | Effet |
|---------|--------|-------|
| `autoCompactThreshold` | `60` | Seuil de declenchement de l'autocompact (% du contexte) |
| `terminalOutputLimit` | `60000` | Limite en caracteres de la sortie terminal capturee |
| `env.ENABLE_LSP_TOOL` | `"1"` | Active l'outil LSP (Language Server Protocol) |
| `env.MAX_FILE_READ_LINES` | `"5000"` | Nombre max de lignes lues par fichier |
| `skipDangerousModePermissionPrompt` | `true` | Supprime le prompt d'avertissement au lancement en bypass mode |
| `projects.autoMemoryEnabled` | `true` | Active la memoire automatique par projet (doublon avec autoMemoryEnabled global) |

### Plugins recommandes (9 plugins officiels)

Les plugins sont des extensions officielles installees via `/plugin`.
Conseil Melvynx : "voler" les concepts des plugins et les integrer
dans vos propres skills (les plugins auto-update et ecrasent vos
modifications).

| Plugin | Usage |
|--------|-------|
| `hookify` | Creer des hooks depuis l'analyse de conversation |
| `commit-commands` | Commit, push, PR, clean branches gone |
| `feature-dev` | Dev guide de features avec comprehension codebase |
| `code-review` | Review de PR |
| `code-simplifier` | Simplification et review code |
| `pr-review-toolkit` | Review PR avec agents specialises |
| `claude-md-management` | Audit et amelioration des CLAUDE.md |
| `claude-code-setup` | Recommandations d'automatisation |
| `security-guidance` | Guidance securite |

### MCP Mac local (5 actifs — justification du depassement)

La Bible recommande max 2-3 MCP. La config Mac en a 5.
Justification : Gmail, Calendar et Memory sont des MCP "legers"
(peu de tools injectes) et essentiels pour le workflow quotidien.

| MCP | Usage | Dans la Bible V4.1 ? |
|-----|-------|---------------------|
| Context7 | Documentation librairies | OUI |
| Exa | Recherche web IA | OUI |
| Memory | Knowledge graph persistant | NON — essentiel pour memoire cross-session |
| Gmail | Lecture/envoi emails | NON — utilise pour automatisation email |
| Google Calendar | Gestion calendrier | NON — utilise pour scheduling |

**Regle** : si le total MCP depasse 10% du contexte, activer
`toolSearchMode: "auto"` (deja configure).

---


## CHAPITRE 4 — WORKFLOWS ET COMMANDES

### /apex — Workflow principal (>99% succes)
LA commande principale de Melvynx. Force Claude a se comporter comme un developpeur Senior methodique.

APEX n'est PAS un logiciel externe ni un programme binaire. C'est un **modele cognitif** (un framework de raisonnement) encode sous forme d'instructions strictes imposees au cerveau de l'IA. C'est l'implementation par Melvynx du concept IA **ReAct** (Reasoning and Acting).

**Pourquoi c'est necessaire** : Les modeles comme Claude fonctionnent de maniere "autoregressive" — ils predisent le mot suivant. Leur instinct est de cracher du code immediatement. C'est la qu'ils font des erreurs, oublient des variables, cassent le projet. APEX brise cette mauvaise habitude en forcant un cycle methodique.

#### Cycle A-P-E-X (sous le capot)

1. **Analyze** (L'Audit) : L'IA a l'**interdiction absolue de toucher au code**. Elle se comporte comme un architecte logiciel qui arrive sur un nouveau projet. Actions invisibles : commandes bash pour lire les fichiers (`cat`, `grep`), analyser l'arborescence, lire les dependances (`package.json`, imports). Lance sub-agents explore-codebase (1 a 3+ selon complexite). But : comprendre le contexte global pour ne pas reinventer la roue ou creer des conflits avec du code existant.

2. **Plan** (Le Brouillon) : L'IA a **toujours l'interdiction de modifier le vrai code**. Elle redige une feuille de route. Actions invisibles : genere une liste a puces numerotee (souvent dans un fichier temporaire `SCRATCHPAD.md` ou dans sa memoire de travail). But : decouper la grosse fonctionnalite en 5-6 micro-taches extremement simples. Force l'IA a penser a l'algorithmique avant la syntaxe. C'est ce qui evite le probleme du "Lost in the Middle" (l'IA qui perd le fil).

3. **Execute** (La Frappe) : C'est seulement MAINTENANT que l'IA commence a travailler sur les vrais fichiers. Actions invisibles : lit la micro-tache n°1 de son plan, modifie le fichier concerne, barre la tache de sa liste. Passe a la tache n°2. But : code chirurgical, fichier par fichier, sans s'eparpiller.

4. **eXamine** (Le Controle Qualite) : Le code est ecrit mais l'IA n'a **PAS le droit de dire "c'est fini"**. Elle doit s'auto-evaluer. Actions invisibles : relit le diff (les modifications), execute le linter (verification syntaxe), lance les tests (si configures). Si elle detecte une erreur, elle s'oblige a la corriger immediatement avant de rendre la main. But : livrer du code qui fonctionne du premier coup. (Premium : review securite + clean code + coherence en plus)

#### 3 super-pouvoirs d'Apex
1. **State Persistence (sauvegarde d'etat)** : Si le quota Claude s'epuise ou que le PC est ferme, Apex sauvegarde l'etat actuel. Au prochain lancement, l'IA reprend exactement la ou elle s'etait arretee. Flag `--resume` (`-r`).
2. **Mode Economie (workflow-apex-free)** : Le framework APEX classique consomme enormement de requetes API (l'IA discute beaucoup avec elle-meme pour valider chaque etape). La variante free force la methode (Analyse, Plan, Execution) mais **saute les etapes de sur-verification** pour economiser le quota Claude. Flag `--economy` (`-$`).
3. **Isolation Git** : Cree automatiquement une nouvelle branche Git au debut de la tache. Si l'IA se trompe completement, la branche main reste intacte. Flag `--branch` (`-b`).

#### Quand utiliser Apex
- Features complexes (ex: "Developpe tout le systeme de paiement Stripe")
- Projets de A a Z de maniere autonome et securisee
- NE PAS utiliser pour des petites modifs (langage naturel suffit)

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

#### Code source du skill APEX (system prompt exact)

Un "skill" Melvynx n'est PAS un script Bash complexe. C'est un **System Prompt extremement directif** (fichier .md dans le dossier skills/). Voici le texte exact que Claude lit quand APEX est active :

```markdown
# WORKFLOW APEX (Analyze, Plan, Execute, eXamine)

Tu dois OBLIGATOIREMENT utiliser le framework APEX pour traiter la demande de l'utilisateur.
Tu n'as PAS l'autorisation d'ecrire du code de production tant que les phases A et P ne sont pas terminees et validees.

## ETAPE 1 : [A]nalyze (Analyse)
1. Utilise les outils de recherche (grep, find, lecture de fichiers) pour comprendre le contexte exact.
2. Identifie les fichiers impactes.
3. Ne propose aucune solution a ce stade. Ecris simplement : "Analyse terminee. Voici ce que j'ai trouve : [Resume court]".

## ETAPE 2 : [P]lan (Planification)
1. Cree un fichier `SCRATCHPAD.md` (ou mets a jour `progress.md`).
2. Redige une liste d'etapes techniques numerotees, claires et minimalistes.
3. Demande a l'utilisateur : "Voici le plan. Es-tu d'accord pour que je passe a l'execution ?" et ATTENDS SA VALIDATION (sauf si mode autonome force).

## ETAPE 3 : [E]xecute (Execution)
1. Implemente le code etape par etape en suivant rigoureusement le plan.
2. Ne modifie qu'un seul composant/fichier a la fois.
3. Ne fais pas d'optimisations non demandees dans le plan.

## ETAPE 4 : e[X]amine (Verification)
1. Lance un auto-audit de ton propre code.
2. Verifie s'il manque des imports, s'il y a des fautes de frappe ou si la logique est cassee.
3. Si une commande de test ou de lint est disponible, execute-la.
4. Si tu trouves une erreur, corrige-la silencieusement avant de prevenir l'utilisateur.

---
**REGLE STRICTE :** A chaque reponse que tu me fais, tu dois commencer ton message par le tag de la phase actuelle : `[ANALYZE]`, `[PLAN]`, `[EXECUTE]` ou `[EXAMINE]`.
```

#### Pourquoi c'est ecrit comme ca (la patte Melvynx)

| Technique | Explication |
|-----------|-------------|
| **Ton imperatif** (majuscules "OBLIGATOIREMENT", "STRICTEMENT") | Melvynx : "parler a l'IA comme a un stagiaire tres intelligent mais un peu dissipe" — il faut la contraindre |
| **Pause forcee** (etape 2 demande validation) | Evite que l'IA crame le quota API en codant 400 lignes dans la mauvaise direction |
| **Fichier SCRATCHPAD.md** (plan ecrit physiquement) | Sert de memoire externe — si la conversation devient longue, l'IA relit ce fichier au lieu de perdre le fil |
| **Tags de phase** ([ANALYZE], [PLAN], etc.) | Rend la phase actuelle visible pour l'utilisateur ET force l'IA a rester dans le cadre |
| **Interdiction de coder avant A+P** | Brise l'instinct autoregressif du modele (predire/cracher du code immediatement) |

**Lecon fondamentale** : un skill = un prompt directif, pas du code. La puissance vient de la precision des contraintes imposees au modele.

### Workflow ONE-SHOT SaaS complet (methode Melvynx)
Workflow en 8 etapes pour creer un SaaS from scratch (ex: Sumfast en 10h pour 403$) :
1. **Idee + SAS Challenge** : prompt discovery, challenger sur problem/angle/market fit
2. **PRD (Project Requirement Document)** : recherche competiteurs, pricing, marges, feature list MVP (genere par IA)
3. **Architecture** : stack technique (libs, state management, auth, structure fichiers)
4. **Tasks** : decomposer PRD + Architecture en petites taches (5-10 lignes chacune — evite hallucination)
5. **Run Tasks** : `/run-task` chaque etape sequentiellement
6. **First Version** : app fonctionnelle mais brute
7. **Refinement** : `/oneshot` pour ajouts feature par feature (sans plan step)
8. **Multi-terminal** : 3-4 sessions Claude Code en parallele pour iterer rapidement
- Prompts custom disponibles : `/sas-challenge`, `/sas-prd`, `/sas-create-architecture`, `/create-task` (dans `~/.claude/commands/prompt/`)
- Hack parallelization : ouvrir 3 terminaux Claude Code, lancer `/oneshot` simultanement = 3x plus vite (mais 3x plus cher)

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
- **Feedback loop** : template exact : "Cree une API route dans mon app pour tester la fonction X. Le resultat attendu est Y. Update le code et fais un curl request jusqu'a ce que tu aies le resultat attendu."
- **Hack** : laisser Claude lancer `npm run dev` en background pour lire ses propres logs, ajouter des logs, comprendre, corriger → boucle autonome
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
`loop stop` pour arreter.

### /schedule — Taches planifiees persistantes (Nouveaute 2026)
Cron persistant (contrairement a /loop).
Exemple : "tous les matins a 9h, clean le dossier Downloads"

### /ultrathink — Reflexion profonde
Force le maximum de thinking tokens. Peut durer 2+ minutes. Pour problemes complexes.

### Opus Plan Mode (pattern avance)
- Utiliser Opus pour le planning, Sonnet 4 pour l'execution
- Commande : `/model` pour changer de modele puis `/plan` en plan mode
- Pattern : Opus genere le plan → Sonnet 4 l'execute (economise tokens)

### Thinking / Extended Thinking
5 mots-cles par ordre croissant d'intensite : `think`, `think more`, `think lot`, `think longer`, `ultrathink`
- Difference avec Plan Mode : Plan Mode = orchestration + demande validation humaine. Thinking = raisonnement etendu sans interaction.
- Preferer "think" dans le prompt plutot que Plan Mode (evite l'interaction bloquante)

### /commit — Commits simplifies
### /create-pr — Pull Request automatique
### /merge — Merge intelligent
### /fix-pr-comments — Corriger les commentaires PR
### /load-memory (ou /cloud-memory) — Optimise CLAUDE.md et rules
Commande `/claude-memory review` : lance sub-agents pour analyser le CLAUDE.md avec 9 criteres :
1. Specificity : pas de liberte d'interpretation, exemples concrets
2. Redundancy : ne pas repeter, preferer l'importance
3. Relevance : contenu applicable et actuel
4. Completeness : commandes, outils, workflow documentes
5. Emphasis : marqueurs CRITICAL, info importantes
6. Structure : separation, hierarchie claire
7. Actionability : actions precises, pas vague
8. Efficiency : pas trop de lignes
9. Accuracy : chemins/refs mentionnes existent reellement

### /prompt-creator — Cree des prompts optimises (best practices Anthropic + OpenAI + Google)
### /skill-creator — Cree des skills personnalises (necessite Python pour init)
### /subagent-creator — Cree des sub-agents personnalises
### /review-code — Review multi-perspectives
### /clean-code — Principes clean code
### /ralph-loop — Coding autonome en boucle (voir Chapitre 15)
### /voice — Mode vocal (beta, mieux en anglais, ne remplace pas les outils dictee)
### /rewind — Restauration de conversation (ou Escape x2)
Affiche l'historique complet des messages/etapes, chaque etape montre les lignes ajoutees/supprimees. Choisir "restore code and conversation" pour restaurer le code ET la conversation. Permet de "coder dans l'insouciance".
### /remote control — Controle a distance via telephone (Max requis)
Genere un lien URL, ouvrir sur iPhone/Android. Messages synchronises en temps reel entre terminal et telephone. Pas besoin d'etre sur le meme WiFi.
- Architecture : tunnel direct (WebSocket) entre Mac local et claude.ai, puis au client distant — PAS un serveur cloud
- Interface web accessible via lien HTTP securise (envoyable par Telegram/SMS)
- Pair programming mode : donner le controle a un collegue, deux personnes donnent des feedback differents au meme Claude
- Cas d'usage : partir en pause, continuer la session depuis le telephone
- Limitation : worktree place dans `.claude/` (problemes lors de grep sur le projet) — Melvynx prefere Conductor

### /batch — ANTI-PATTERN selon Melvynx
Lance agents en parallele avec work trees separees, chaque agent cree sa PR.
Melvynx est CONTRE : genere des dizaines de PR a review. Preferer un main agent orchestrateur.

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
| `/plugin` | Installer plugins (2 sources : Anthropic Cloud Code + Official Plugins) |
| `/stat` | Statistiques session |
| `/config` | Configuration interactive (tips, suggestions, progress bar, autocompact) |
| `/voice` | Mode vocal (beta, mieux en anglais) |
| `/loop` | Tache recurrente a intervalle |
| `/schedule` | Taches planifiees persistantes |
| `/simplify` | Review et simplification |
| `/copy` | Copier derniere reponse en Markdown |
| `/debug` | Auto-debug configuration Claude Code |
| `/compact` | Compacte le contexte (synthese, perd info inutiles) |
| `/rewind` | Revenir a un etat precedent (code + conversation) |
| `/resume` | Retrouver une ancienne conversation |

### Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| Escape x1 | Stoppe Claude / efface le texte courant |
| Escape x2 | Acces historique conversations (naviguer avec fleches, Enter, "Restore conversation") |
| Shift+Tab | Changer mode permissions (Normal → Accept Edit → Plan → Bypass) |
| Tab | Activer/desactiver le thinking mode |
| Ctrl+T | Taches en cours |
| Ctrl+O | Transcript verbose (voir le thinking de l'IA) |
| Ctrl+S | Sauvegarder prompt (question intermediaire puis prompt revient automatiquement) |
| Meta+P | Changer modele |
| Ctrl+C | Arreter Claude Code |
| Fleche haut/bas | Historique des messages |

### Prefixes dans le prompt
- `@` : taguer un fichier (@header pour src/components/header)
- `/` : commandes slash (Tab pour selectionner, pas Enter)
- `!` : mode bash direct (!pnpm install — resultat injecte dans le contexte)
- `#` : ajouter une memoire (project, local, ou user)

---


## [Section ajoutee au Chapitre 5 — Contradiction MCP max 3 vs 5 reels]

### Note sur la limite MCP

La regle "max 2-3 MCP" de Melvynx concerne les MCP **lourds**
(qui injectent beaucoup de tools/descriptions dans le contexte).
Les MCP legers (Memory, Gmail, Calendar) avec peu de tools
peuvent depasser cette limite sans degradation notable si
`toolSearchMode: "auto"` est actif.

La config Kim utilise 5 MCP avec toolSearchMode auto. Le total
de tokens MCP reste sous 10% du contexte grace a la legerete
des 3 MCP supplementaires.

---


## CHAPITRE 6 — CONTEXT ENGINEERING & MEMOIRE

### Contexte en chiffres
- **200 000 tokens max** par conversation (1M tokens sur plans Max/Team/Enterprise)
- **~27 000 tokens utilises au demarrage** (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- **~82K tokens consommes** au demarrage sur projet moyen (tools, MCP, rules, memories, agents, plugins)
- **System prompt** : ~3 500 tokens (1,7% du contexte)
- **Outils (tools)** : tres volumineux — chaque outil avec tous ses parametres et descriptions
- **Skills** : description de chaque skill injectee dans le prompt
- **MCP tools** : definitions injectees meme si non utilises
- **Autocompact buffer** : 45 000 tokens reserves
- Quand contexte approche 150-160K sur 200K : l'IA commence a divaguer

### Debug du contexte avec Proxyman
- **Proxyman** : outil macOS pour intercepter les requetes HTTP de Claude Code
- Permet de voir exactement ce qui est envoye au modele a chaque requete
- Utile pour comprendre les 27K tokens de demarrage
- Hack : demander a Claude de creer un fichier HTML qui affiche toutes les infos du contexte

### Reverse engineering API (secrets reveles par Melvynx)
- Chaque requete inclut des **metadonnees utilisateur** (user ID)
- Les objets `tools` contiennent des schemas d'entree detailles pour chaque outil natif
- **Outils natifs complets** : Write, Notebook Edit, Notebook, Web Fetch, To Do Write, Web Search, Bash Outputs, Kill Bash
- Tous les MCP configures sont envoyes dans CHAQUE requete (d'ou l'importance de limiter a 2-3)
- Structure messages imbriquee : system reminders empiles dans le contexte
- **Web Fetch automatique** : quand l'utilisateur pose une question technique sur Claude Code, Claude declenche automatiquement un Web Fetch en arriere-plan pour recuperer la documentation — feature native, pas configuree par l'utilisateur
- **Les commandes slash = injections de prompt pures** : pas des appels systeme ni des webhooks — elles ajoutent du texte dans le message avec tags `<command_args>`. Conclusion Melvynx : "c'est vraiment une sorte de prompt injection"
- **Output style custom** apparait APRES les directives dans le prompt, le default AVANT — impact massif sur tonalite et longueur

### Probleme "Lost in the Middle"
Les transformers donnent plus d'importance au DEBUT et a la FIN du contexte. Les tokens au milieu sont structurellement ignores. Les chunks au milieu sont oublies, les instructions au centre sont ponderees 1/3 a 1/2.

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

### 3 niveaux de memoire CLAUDE.md

| Niveau | Emplacement | Portee |
|--------|-------------|--------|
| Global | ~/.claude/CLAUDE.md | Tous les projets |
| Projet | ./CLAUDE.md (racine) | Ce projet uniquement |
| Dossier | ./src/components/CLAUDE.md | Charge quand l'IA lit un fichier du dossier |

### Contenu recommande CLAUDE.md
- Commandes dispo (dev server, build, test, lint)
- Stack technique, architecture du projet
- Conventions de code, file naming, import TS
- Composants UI a utiliser (ex: ChatCN)
- Couleur primaire, preferences UI
- Warnings/restrictions
- **Ne pas surcharger** : max ~200 lignes, deplacer le reste dans rules/
- "Depuis que je lui dis d'utiliser les composants utilitaires de ChatCN, les resultats sont bien meilleurs"

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

### Ajout rapide de memoire
- `#texte` dans le prompt → choix entre project memory, project local, user memory
- `yes [information]` → Claude propose de sauvegarder (global ou project)
- `/memory` pour ouvrir le fichier memoire directement

### Commentaires inline comme "prompt injection" benefique
Documenter les helpers internes (ZodRoute, Upfetch, etc.) avec des exemples d'utilisation detailles dans les commentaires du code. Quand la regle "lire 3 fichiers similaires" est active, l'IA absorbe ces instructions automatiquement et reproduit les patterns corrects.

### Changelog automatique
Regle dans CLAUDE.md : `CRITICAL: Apres tout changement, update le changelog avec la date du jour.`

### Commentaires inline comme "prompt injection benefique" (technique avancee)
- Ajouter des commentaires DANS le code pour guider l'IA sur comment utiliser les librairies
- Exemples : `// Base route handler — usage: use this for X, Y` ou `// Route handler with authentication — user is logged in`
- **Similar file + imported dependency** : si on importe des types/dependances, l'IA doit lire le fichier de la librairie → empeche hallucination
- Deux techniques anti-hallucination : 1) ameliorer contexte (CLAUDE.md + commentaires inline) 2) ameliorer prompt (donner l'exemple du fichier a utiliser)
- Les EXAMPLES sont plus efficaces que la DOCUMENTATION pour l'IA
- Avec la regle "lire 3 fichiers similaires", l'IA absorbe automatiquement les commentaires inline
- NowTS boilerplate : boilerplate Next.js optimisee pour Claude Code avec tous les commentaires + exemples pre-configures

### Keywords de priorite dans les prompts
- CRITICAL, MANDATORY, VALIDATION, VERIFICATION → l'IA detecte l'importance via le system prompt et priorise
- "non-negotiable" → renforce la contrainte
- Majuscules = plus d'impact que minuscules dans les instructions

### Gestion du contexte — Regles
- `/clear` OBLIGATOIRE entre les taches majeures
- `/context` regulierement pendant les longues sessions
- Si contexte > 80% : `/clear` immediat
- Langage naturel pour petites modifs (economise le contexte)
- Sub-agents pour les recherches (preservent le contexte principal)
- Maximum 2-3 MCP actifs
- Attention aux mauvais chats : avec plusieurs terminaux ouverts, erreur frequente d'envoyer un prompt dans le mauvais terminal
- Archiver les agents inutilises dans un dossier `agent-save/` (pas de toggle comme MCP)
- "Ne pas faire le gigolo a ajouter 2000 trucs"

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
- **Descriptions courtes** pour minimiser l'impact contexte
- Benchmark Melvynx : sans sub-agent = 41% du contexte, avec sub-agent = 38% → **5000 tokens economises** pour un resultat equivalent voire meilleur
- Gain : ~5000 tokens economises vs sans agent pour un resultat equivalent ou meilleur
- Si trop d'agents : warning "large cumulative agent description impact performance"

### Types de sub-agents

| Type | Usage | Vitesse |
|------|-------|---------|
| Explore | Recherche, exploration | Rapide |
| Task | Actions generiques | Normal |

### 3 sub-agents recommandes
1. **Web Search** : WebSearch + WebFetch + Exa MCP — description : "use when you need to quickly find information"
2. **Explore Codebase** : Read + Grep + Search
3. **Explore Doc** : Context7 MCP — specialise documentation

### Quand utiliser les sub-agents
- **OUI** : recherche, mini modifications de code sur fichiers precis
- **NON** : features completes (pas de visibilite, pas de redirection possible = one shot)
- Forcer l'usage : creer un workflow qui OBLIGE l'IA a utiliser les sub-agents (sinon elle peut les ignorer)

### Creation d'un agent custom
1. `/agent` > Create New Agent > Personal Folder > Generate with Cloud
2. Decrire la tache (ex: "prend un fichier en parametre, corrige la grammaire, retourne le resultat")
3. Assigner un modele (Sonnet pour economiser)
4. Assigner une couleur
5. Le main agent peut ensuite lancer cet agent en parallele sur N fichiers
- Ou via le skill `/subagent-creator`

### Teams (Agent Teams / Agent Swarm)
- Activer : `enableAgentTeams: true` dans settings.json
- Le main agent (team lead) cree des equipes, assigne des taches via task list partagee
- Les teammates travaillent en parallele
- **Self-claim** : quand un teammate termine une tache, il peut automatiquement s'assigner la tache suivante sans intervention
- Interface : fleche bas pour switcher entre agents (main, dev1, dev2, etc.)
- Les teams utilisent Tmux pour splitter les agents en tabs

#### Outils Teams disponibles
| Outil | Fonction |
|-------|----------|
| team create | Cree une equipe avec task list partagee |
| task create | Cree une tache (subject, description, active, pending) |
| task list | Liste les taches |
| task get / task update | Recupere/modifie une tache |
| send message | Message entre agents (direct, broadcast, shutdown request) |
| team delete | Clean up de l'equipe |

#### Modes d'execution Teams
- **In-process** : agents dans le meme terminal (defaut sans tmux)
- **Split-pane** : chaque agent a son propre terminal (avec tmux)
- **Auto** : detecte tmux automatiquement

#### Delegate Mode
- `Shift+Tab` pour passer en delegate mode
- Force le team lead a NE PAS travailler lui-meme — uniquement deleguer
- Activer APRES creation de la team et des taches

#### Quand utiliser Teams
- **OUI** : zones de modification SEPAREES (monorepo back/front/mobile)
- **NON** : memes fichiers — les agents se "battent"
- Les agents communiquent peu entre eux (surtout via le team lead)
- Maximum 4 terminaux/work trees simultanes

### Work Trees
- Travailler sur des branches separees en parallele
- Workflow : creer work tree → feature → PR → merge → detruire le work tree
- **Maximum 4 work trees simultanes**
- Reserver aux gros refactors structurels
- Ne PAS utiliser pour de petites features
- Piege : les agents demandent chacun ton attention → agents qui travaillent 25% du temps
- Astuce CLAUDE.md : "Si il y a des erreurs TypeScript que tu n'as pas directement implementees, c'est pas ton probleme, passe a la suite."

#### Probleme worktree dans le dossier projet
Le worktree dans `.claude/worktree/` cause des problemes : git affiche les changements, grep trouve des resultats parasites.
**Solution** : creer les worktrees a cote du projet (sibling directory) : `../worktree/<branch-name>/`

### Pattern multi-agents prefere de Melvynx
```
Main Agent (gros contexte)
  ├── Sub-agent 1 (modifie le code)
  ├── Sub-agent 2 (modifie le code)
  └── Sub-agent 3 (modifie le code)
Main Agent verifie que tout fonctionne
```
Un dev devrait monitorer **minimum 3 agents** en parallele. "Si un dev utilise un seul agent, il scrolle sur TikTok."

---


## [Section ajoutee au Chapitre 7 — Agent Teams avance]

### Agent Teams (feature 5 fevrier 2026)

Details supplementaires sur les Teams non couverts en V4.1 :

- **Nombre d'agents** : 2 a 16 agents possibles par equipe
  (recommandation Melvynx : max 4 simultanes, au-dela les
  agents travaillent 25% du temps car ils attendent l'attention)
- **Bugs connus** : les agents ne communiquent PAS vraiment
  entre eux. La communication passe essentiellement par le
  team lead. Les messages "broadcast" et "send message" existent
  mais sont sous-utilises par les modeles
- **Interface** : fleche bas pour switcher entre agents
  (main, dev1, dev2, etc.). Avec TMUX : interface multi-pane
  plus visuelle
- **Self-claim** : quand un teammate termine une tache,
  il peut s'assigner la suivante sans intervention humaine
- **Delegate Mode** : `Shift+Tab` apres creation de l'equipe
  pour forcer le team lead a UNIQUEMENT deleguer (ne code
  pas lui-meme)
- **Quand NE PAS utiliser** : feature qui modifie une seule
  zone, ou memes fichiers concernes par plusieurs agents

---


## CHAPITRE 8 — PROMPTING TECHNIQUES

### Greenfield (nouveau projet)
1. Specifier la stack : "create a new vite project with shadcn and tailwind"
2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier la technique (camelCase etc.) au premier run — laisser l'IA choisir
5. Utiliser une boilerplate existante pour imposer les conventions (ex: **NowTS** — nowts.app — de Melvynx)
   - NowTS contient des regles strictes pour API routes, fetching, middleware, authentification, dialogues
   - Helpers internes documentes avec commentaires inline : **ZodRoute** (validation API routes), **Upfetch** (HTTP client)
   - L'IA faisait toujours des erreurs avec Upfetch → Melvynx a demande a Claude d'analyser les utilisations dans l'app et de rajouter des commentaires d'usage — les erreurs ont disparu
   - Methode : "analyse les utilisations de [helper] dans mon app et rajoute un commentaire avec les exemples"
6. Si preferences techniques : les mettre dans CLAUDE.md global APRES le premier run

### Brownfield (projet existant)
1. Screenshots annotes (CleanShotX) — montrer exactement le probleme avec dessins, fleches, carres rouges
2. Donner des references internes : "style comme illustration-picker" ou "video-picker"
3. Donner les ressources : chemins, images, URLs
4. Les screenshots permettent a l'IA de VOIR — ameliore drastiquement le front-end
5. L'IA suit automatiquement les conventions du code existant

### Debugging
1. **Log technique** (LE PLUS EFFICACE) : ajouter console.log → reproduire → copier les logs BRUTS → envoyer a Claude
2. **Screenshot + description** : "j'ai mis le sleep time a 23h et pourtant le 23 n'est pas selectionne"
3. **Feedback loop** : API route test + curl en boucle + code update → boucle autonome
4. **Ne pas dire quoi faire** : exposer le PROBLEME, l'IA resout

### Technique des 10 variations UI
```
Cree un fichier /demo/page.tsx avec 10 variations UI/UX de cette interface,
10 composants separes dans 10 fichiers separes, importes dans la page.
```
Tester chaque variation, choisir la meilleure, supprimer le dossier `/demo/` apres.

### Processus iteratif front-end
Screenshot → demander modif → re-screenshot → ajustement → etc.
Donner la vision visuellement = le plus grand hack pour le code front-end.

### Petites modifications
Langage naturel directement, PAS de commandes/skills. Economise du contexte.

### Eviter les erreurs repetitives de l'IA
1. **Rules dans .claude/rules/** : creer une regle specifique quand erreur 2+ fois
2. **Workflow dans CLAUDE.md** : "Avant de modifier un fichier, lire 3 fichiers similaires"
3. **Rules ciblees avec globs** : regle specifique pour un type de fichier
4. **Commentaires inline** : documenter les helpers internes avec exemples
5. **Changelog automatique** : `CRITICAL: Apres tout changement, update le changelog`

### Anti-patterns prompting
- Ne PAS relancer un skill pour une petite modif — langage naturel
- Ne PAS empiler les demandes pendant que l'IA travaille
- Ne PAS utiliser Chrome Headless (Melvynx le juge lourd et lent)
- Attention aux mauvais chats avec plusieurs terminaux ouverts
- Fautes d'orthographe OK — l'IA comprend

### Techniques de live coding Melvynx (extraites de 2h de sessions filmees)
- **Apex vs OneShot** : apex pour features complexes (plan obligatoire), oneshot pour taches rapides et sures
- **Brainstorm** : commande "token maxi" qui force l'IA a reflechir en rounds successifs pour problemes difficiles
- **Debug par screenshot** : prendre un screenshot pour donner du feedback UI a Claude (plus efficace que du texte)
- **Multiples terminaux paralleles** : lancer plusieurs agents sur differentes features/bugs simultanement, alterner entre eux pendant que l'IA reflechit
- **Relancer Claude quand il bloque** : aide souvent a debloquer (~33% contexte = limite pratique)
- **Rules dynamiques** : quand l'agent fait la meme erreur 5+ fois, ajouter une rule en memory immediatement
- **Desactiver auto-compact** : garder suffisamment de tokens de reflexion (120K utilises sur 200K)
- **Erreurs TypeScript/ESLint** : les injecter UNIQUEMENT si l'agent les a produites (critique avec plusieurs agents)
- **"Always be thinking"** : demander a Claude d'imaginer une UX meilleure que celle demandee
- **Prompt UI avec "clean code"** : inclure le mot dans le prompt pour eviter du code degueulasse
- **Fetch page markdown** : creer un tool custom pour que Claude lise les pages du site en contexte (restreindre par domaine pour securite)
- **Ghostty** : editeur multi-table pour gerer 5+ projets en parallele
- **Convex** : real-time DB recommandee par Melvynx ("magique, automatique")
- **Telegram + Claude agents** : deleguer des taches secondaires automatiquement via Telegram
- **Langages compiles = meilleur ami de Claude Code** (TypeScript, Swift) — les build errors permettent a l'IA de s'auto-corriger
- **Vibe coding iOS tres couteux** : ~55$ en tokens pour une petite app Watch+iPhone
- **Recommencer une conversation** plutot que l'allonger (evite erreurs compilation en cascade)

---

## CHAPITRE 9 — SECURITE

### Modes de permission (Shift+Tab)
1. Normal : demande validation a chaque etape
2. Accept Edit : pas de validation pour les modifs fichiers
3. Plan : propose un plan, pas de modification
4. **Bypass Permission** (rouge) : aucune question — RECOMMANDE par Melvynx
- Citation Melvynx : "Plus vous limitez l'IA, moins les resultats seront bons. J'ai decouvert la vraie puissance de l'IA depuis que je lui ai donne l'acces a tout. Elle n'a jamais fait quelque chose de dangereux."
- Seul risque reel constate : `prisma reset` qui reset la DB en local. Si vous etes en entreprise, les fixtures permettent de restaurer.

### Deny List — Meme en bypass, ces commandes sont bloquees
- `rm -rf`, `sudo rm`, commandes destructives
- Erreur affichee : "Permission to use bash with command rm -rf denied"
- Double securite : deny-list + CLAUDE.md ("utiliser trash au lieu de rm")

### Command Validator (PreToolUse hook)
- Script TypeScript/Bun qui valide les commandes au niveau AST
- Pas de faux positifs (contrairement a grep)
- Bloque rm -rf, sudo, dd, privilege escalation, remote execution
- UN SEUL hook PreToolUse recommande

### Plugins — ATTENTION
- Deux sources : "Anthropic Cloud Code" et "Anthropic Cloud Official Plugins"
- `/plugin` dans Claude Code pour installer
- **Probleme** : installent du code non controle dans un dossier non modifiable
- Se mettent a jour automatiquement (ecrasent vos modifs)
- **RECOMMANDATION forte de Melvynx** : copier le code dans vos propres skills/agents
- Citation : "Volez les concepts, creez votre propre configuration"
- Anti-pattern : installer 25 plugins → contexte pollue, 1 milliard de commandes

### Structure d'un plugin
- Dossier avec sous-dossiers `commands/`, `agents/`, `hooks/`
- Fichier `marketplace.json` a la racine du dossier `.claude-plugin/`
- Gestion : `/plugin` > Browse and install / Manage Marketplace / Add Marketplace (`user/org/repo`)
- Space pour activer/desactiver, Delete pour marquer a desinstaller

### Garde-fous hardcodes (securite systemique)
Un simple fichier rules.md ne suffit pas toujours — l'IA peut l'ignorer si le contexte devient trop long. Les vrais garde-fous doivent etre **systemiques**.

#### Regles de non-destruction absolues
- Inscrire dans le System Prompt central des interdictions formelles sur la manipulation des donnees
- INTERDIRE la suppression de fichiers sous pretexte de doublons en se basant uniquement sur la taille — risque de perte fatale
- Toute comparaison de doublons DOIT suivre les 4 etapes des outils professionnels (jdupes/fdupes) :
  1. **Taille du fichier** : filtre rapide — si taille differente, pas doublon (NECESSAIRE mais JAMAIS SUFFISANT)
  2. **Hash partiel** (premiers 4096 octets) : elimination rapide des faux candidats
  3. **Hash complet** du fichier entier : comparaison fine (jdupes = xxHash64, fdupes = MD5)
  4. **Byte-by-byte** (octet par octet) : confirmation finale — elimine tout risque de collision hash
- Faux positifs = ZERO quand les 4 etapes sont respectees
- JAMAIS se baser sur la taille seule (etape 1 sur 4) — c'est la cause principale de pertes de donnees
- JAMAIS se baser sur le hash seul sans verification byte-by-byte — les collisions existent
- Outils recommandes : `jdupes` (xxHash64, rapide, I/O-bound) ou `fdupes` (MD5, conservateur)
- Apres detection, toujours afficher la liste exacte des fichiers impactes ET attendre un OUI explicite
- Aucune suppression silencieuse : lister les fichiers AVANT execution, puis attendre approbation humaine
- Forcer la validation humaine (prompt de confirmation) pour toute commande rm ou del
- Double couche : deny-list settings.json + CLAUDE.md + command-validator PreToolUse

#### Reference algorithme jdupes vs fdupes

| Critere | jdupes | fdupes |
|---------|--------|--------|
| Hash | xxHash64 (non-cryptographique, ultra-rapide) | MD5 (cryptographique, plus lent) |
| Hash partiel | Premier bloc 4096 bytes | MD5 partiel |
| Verification finale | Byte-by-byte | Byte-by-byte |
| Faux positifs | Zero (byte-by-byte apres hash) | Zero (byte-by-byte apres hash) |
| Performance | Optimise vitesse (goulot = disque I/O, pas CPU) | Plus conservateur |
| Installation | `brew install jdupes` (macOS) / `apt install jdupes` (Linux) | `brew install fdupes` / `apt install fdupes` |

#### Isolation de l'environnement
- Ne JAMAIS donner a Claude Code un acces root global
- Monter les dossiers sensibles en lecture seule (Read-Only)
- L'IA peut lire tout le systeme pour comprendre le contexte, mais ne peut ecrire que dans le dossier du projet
- Utiliser la deny-list Read pour bloquer l'acces aux fichiers sensibles (.pem, .env, secrets, tokens)

#### Git Pre-commit Hooks automatises
- Avant que l'IA ne valide un code, un script local verifie automatiquement les erreurs de syntaxe et failles de securite
- Si le script echoue, la boucle de l'IA est rejetee et elle doit corriger
- Exemple : ESLint, TypeScript strict, tests unitaires obligatoires avant commit

### Moteur de performance (memoire chaude / memoire froide)
Pour que l'IA soit surpuissante, elle ne doit pas reflechir a l'architecture — elle doit se concentrer sur la logique metier.

#### MEMORY_CORE.md (Memoire Froide)
- Regles immuables du projet : stack technique, architecture de base, conventions de nommage
- Ce fichier n'est JAMAIS modifie par l'IA
- Equivalent : CLAUDE.md projet + rules/ avec alwaysApply: true

#### SCRATCHPAD.md (Memoire Chaude)
- Bloc-notes de l'IA : elle ecrit son plan d'action etape par etape AVANT de coder
- Augmente les capacites de raisonnement logique de facon fulgurante
- Equivalent dans Claude Code : Plan Mode (Shift+Tab → mode Plan)

#### Comportement Tech Lead senior
- Imposer dans CLAUDE.md le comportement exact d'un Tech Lead senior
- Toujours ecrire les tests unitaires AVANT la logique (TDD)
- Toujours utiliser un typage strict
- Au lieu de dire "code bien", decrire le comportement attendu precisement

### Skills comme outils autonomes
La vraie puissance : donner des "mains" a l'IA.
- Au lieu de demander a l'IA d'expliquer comment deployer, lui donner un outil (script Bash ou API) appele `deploy_to_staging`
- L'IA analyse le code, voit que c'est pret, et declenche elle-meme la commande de deploiement
- Philosophie Melvynx : Skills > MCP pour le controle et la simplicite

---

## CHAPITRE 10 — PRICING & LIMITES

### Abonnements

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| **Max 20x** | **200$** | **~3 200$** | **x16 (meilleur ratio)** |

- API directe : Opus = 15$/M input + 75$/M output (peut monter tres vite)
- Chiffres reels Melvynx : une session peut couter ~7$, une journee intensive ~237$, soit ~3000$/mois en API pour 200$/mois d'abonnement
- Stats : 80% des utilisateurs depensent moins de 200$/mois en inference. Les top users (comme Melvynx) depassent 1000$/mois. Le plan Max 200$ est une expense marketing — Anthropic perd de l'argent volontairement pour creer des evangelistes qui font la promotion gratuite du produit.
- **Recommandation** : Max 20x pour usage pro — pour chaque euro depense, deux fois plus d'utilisation que le Max 5x
- Commencer a 20$, monter si limites atteintes

### Sessions et limites
- Sessions de **5 heures**, reset toutes les 5h (~4.5 sessions/jour)
- **Session limit** : pourcentage d'utilisation dans la session (plus atteignable)
- **Weekly limit** : limite hebdomadaire globale (rarement atteinte avec Max 20x)
- Si session limit atteinte : attendre ~1h pour reset
- Verifier sur claude.ai > Settings > Usage

### Outil CC Usage
- `npx cc-usage` ou `npx cc-usage block-live` : consommation en temps reel, projections, cout par session
- `/stat` dans Claude Code : weekly usage avec delta

### Quand monter d'abonnement
- Pro (20$) : usage hobby/fun, longs espaces entre sessions
- Max 5x (100$) : usage professionnel regulier
- Max 20x (200$) : usage intensif, freelance/CDI, OpenClaw

---

## CHAPITRE 11 — OUTILS RECOMMANDES

### Terminaux
- **Ghostty** (ghosty.org) : terminal optimise, gratuit, minimaliste. Raccourcis : `Cmd+1/2/3` tabs, `Cmd+W` fermer, `Cmd+T` nouveau tab
- **CEMUX** : terminal cree pour coding agents (4.7 stars). Build au-dessus de Ghostty. Tabs verticaux + horizontaux, notifications agent, browser integre (Safari), split pane, ports ouverts, PR en cours, branches git. Build en Swift/AppKit (pas Electron). Gratuit. Raccourcis : `Ctrl+1/2/3` navigation entre tabs, `Cmd+Shift+R` renommer workspace.
- **Hyper** : alternative populaire

### Outils macOS
- **Raycast** : remplacant de Spotlight (gratuit sauf IA 8$/mois). App launcher, clipboard history (texte + screenshots), emoji picker langage naturel, window management, screenshot, extensions (VS Code, Figma, GitHub, Tailwind, iOS)
- **CleanShotX** : screenshots annotes avec dessins, fleches, carres rouges (disponible dans SetApp)
- **SetApp** : bundle d'outils macOS incluant CleanShotX

### Speech-to-text
- **Voice Ink** : speech-to-text pour Mac (39$ one-time, alternative a Super Whisper 9$/mois)
- **WhisperTurbo** : app Melvynx, speech-to-text local gratuit et open source. Modele PRK (Parakeet) : rapide pour <20s. Modele Whisper Large : meilleur pour longs enregistrements et francais
- **Parler** : app Melvynx speech-to-text, open source, DMG disponible en telechargement direct
- **Homebrew** : gestionnaire de paquets macOS (`brew install <paquet>`)

### IDE
- **VS Code** : telecharger code.visualstudio.com. Commande CLI : `Cmd+Shift+P` > "Shell Command: Install code in PATH"
- Extension Claude Code pour VS Code/Cursor

### Debug
- **Proxyman** : intercepter requetes HTTP de Claude Code (debug contexte)

### Deploiement rapide
- **Netlify Drop** : `app.netlify.com/drop` — drag & drop du dossier `dist/`, URL publique instantanee
- **Vercel** : deploiement production (Next.js) pour vrais projets

### Stack technique Melvynx
- **Supabase** : backend/BDD pour SaaS (auth + database)
- **Vercel** : deploiement production
- **Netlify Drop** : demos rapides

### tmux essentiels

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `Ctrl+A C` | Nouveau terminal (window) |
| `Shift+fleches` | Switcher entre windows |
| `Ctrl+A S` | Split en deux |
| `Ctrl+A ,` | Renommer la window |
| `Ctrl+A D` | Detacher (quitter sans fermer) |
| `tmux attach` / `tmux a` | Rattacher la session |

### Dossier "CC" — Fourre-tout Claude Code
`~/cc/` (accessible via alias `cdcc`) pour tout hors projets :
- Generateur de titres YouTube, createur de slides
- Analyses business, reviews d'emails, comptabilite, todo lists
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

### Audio bidirectionnel avec ElevenLabs
Discussion vocale bidirectionnelle via Telegram (envoi + reception audio).

### Modeles recommandes OpenClaw

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2 input / $12 output |
| Cron jobs | Gemini 3 Flash | Moins cher (ideal watchers) |
| Agents de code | Claude Opus | Meilleure qualite (abonnement CC) |

### Threads OpenClaw (personnalites)
- **Coach** : Motivation, coaching tough
- **Love** : Suivi relations
- **General** : Remplacement ChatGPT
- **Docteur** : Historique medical, suivi sante
- **Work** : Code, business, deploiements
- Memoire : cron 2x/jour analyse 12h de messages, sauvegarde dans fichiers memoire
- Les threads peuvent communiquer entre eux

### Agent Email/Sponsors
- Repond automatiquement aux emails dans le style de Melvynx
- Decline auto si prix trop bas
- Demande validation humaine pour negociations/actions destructives
- Anti prompt-injection : asterisque sur untrusted inputs, Opus respecte hierarchie de confiance

### Securite OpenClaw
- Docker obligatoire pour sandboxing
- Telegram plus securise que l'interface web
- Machine dediee recommandee : Mac Mini (~600 EUR) pour 24/7

### Cout OpenClaw
- Chat/email = pas cher en tokens
- Code via OpenClaw = cher (pas de caching sur Agent SDK, contexte grandit vite)
- **Conseil** : utiliser OpenClaw quand on n'a PAS son ordi. Sinon, Claude Code directement.
- Anti-ban : ~50% tokens via OpenClaw, ~50% via CLI directement
- **JAMAIS upgrader d'abonnement via le lien fourni par OpenClaw** — toujours upgrader depuis Claude Code directement (security checks au moment de l'upgrade)
- Agent email : modifie automatiquement son propre system prompt quand on le corrige par message Telegram (auto-apprentissage en live)
- Agent email : bannissement automatique des spammeurs + auto-archivage via regles anti-prompt-injection
- Fichier `situation.md` : mis a jour automatiquement quand OpenClaw detecte des infos de contexte dans les emails recus (location, voyage, priorites)

### Tips OpenClaw (confirmes video Melvynx)
- **Bouton "Partager via Telegram"** : depuis n'importe quelle app (Twitter, Safari, YouTube, etc.), utiliser le bouton Share → Telegram → envoyer au bot OpenClaw. Il sauvegarde ou agit sur le contenu partage.
- **Workspace GitHub backup** : creer un repo GitHub pour sauvegarder la config OpenClaw. Commande : "Cree un repo openclaw-workspace sur GitHub et initialise ce projet". Configurer un cron automatique pour backup matin et soir sur Git.

---


## [Section ajoutee au Chapitre 12 — OpenClaw enrichissements]

### Channels OpenClaw (20 mars 2026)

OpenClaw supporte maintenant 23+ canaux de communication :
- **Telegram** (canal principal recommande par Melvynx)
- **Discord**
- **iMessage**
- **WhatsApp**
- **Slack**
- **Email (Gmail/Outlook)**
- + 17 autres canaux via le systeme de plugins

Configuration des canaux supplementaires via l'onboarding
OpenClaw ou en ajoutant des plugins post-installation.

### Statistiques OpenClaw (mars 2026)
- **342 000+ etoiles GitHub** — projet open source le plus
  populaire dans la categorie agents IA
- Communaute active avec contributions quotidiennes
- Melvynx est contributeur principal et utilisateur quotidien

### Setup OpenClaw complet debutant (etapes detaillees)

Guide pas-a-pas extrait de la video "Je SETUP OpenClaw en
1 heure a un total debutant" :

1. Avoir un VPS (Hetzner recommande, Hostinger possible)
2. Installer Ubuntu sur le VPS
3. Creer une cle SSH via Claude Code local :
   "cree-moi une nouvelle cle SSH qui s'appelle openclaw
   et copie la cle publique dans mon presse papier"
4. Ajouter la cle publique sur le panel VPS
5. Creer un profil SSH :
   "cree-moi un profil SSH pour ssh openclaw directement"
6. Se connecter en SSH
7. Installer Claude Code sur le VPS
8. Lancer `npx openclaw-vps-setup` — installe :
   - Node.js, OpenClaw GitHub CLI, Claude, Bun
   - Cloudflared (tunnel pour Gmail webhooks)
   - GCloud (gestion emails)
9. Security hardening : UFW, fail2ban, SSH hardening
10. Desactiver password auth (cle SSH uniquement)
11. Lancer onboarding openclaw
12. Creer un bot Telegram via @BotFather → /newbot
13. Configurer : Anthropic, token API, bot token Telegram
14. Installer skills optionnels :
    ClawUp, Gemini, Google (Gmail/Calendar), Whisper
15. `gh auth login` pour connecter GitHub
16. Creer un repo workspace sur GitHub
17. Configurer l'identite de l'agent (prompt personnalite)

### Skills OpenClaw (liste complete Melvynx)

Liste mise a jour avec tous les skills mentionnes :

| Skill | Usage |
|-------|-------|
| CloudFlare | Manager serveurs/domaines |
| Code Process | Lancer Claude Code CLI en parallele |
| Coursify/Codeline | Formations, coupons, referrals |
| Context7 | Documentation librairies |
| Dub | URL shortener (mlv.sh/xxx) |
| Exa | Recherche web (flights, articles) |
| Front | Support client |
| Google Image | Generation images via API Google |
| Lumail | Email marketing |
| Mercury | Acces bancaire |
| Porkbun | Gestion domaines |
| SaveIt | Bookmarks |
| TypeFully | Tweets planifies |
| Vercel Up | Gestion deploiements |
| YouTube Command | Commentaires YouTube |
| LinkedIn Post | Publications LinkedIn |
| FlightData | Donnees de vols (aviation API) |
| Whisper | Transcription audio |
| ClawUp | Mise a jour auto OpenClaw |

**Regle Melvynx** : ne JAMAIS telecharger de skills externes.
Toujours les faire creer par Claude. "Les skills ont pas
beaucoup de valeur quand on les telecharge" — chaque skill
cree par Claude utilise votre style et vos conventions.

### Anti-patterns OpenClaw supplementaires

- Ne PAS envoyer plusieurs messages Telegram rapidement :
  OpenClaw repond au premier et ignore les suivants
- OpenClaw dit toujours "oui" meme quand il est incapable →
  toujours tester/verifier le resultat
- Ne PAS laisser l'UI web ouverte en permanence (pas securise)
- Preferer Telegram comme interface principale

### Cron jobs OpenClaw (exemples concrets)

- "Tous les jours backup matin et soir mon workspace sur GitHub"
- "Tous les matins a 8h, lis mes derniers mails non lus
  et fais un resume"
- Email de compagnie aerienne → sauvegarde ID vol →
  check quotidien du statut
- Resume des emails importants chaque matin

---


## CHAPITRE 13 — SKILLS & META-SKILLS

### Structure d'un skill
- **Petit skill** : un seul fichier SKILL.md
- **Grand skill** : dossier avec SKILL.md (entry point = table des matieres) + references/ + scripts/ + steps/ + templates/
- Seul l'index est charge, les sous-fichiers lus a la demande (token-efficient)
- La description du skill = seule chose injectee dans le contexte au depart. Quand la tache matche → skill complet telecharge.

### Proprietes SKILL.md (V2)
```yaml
---
name: "security-audit"
description: "Audit de securite complet"
arguments: ["arg1", "arg2"]
contextFork: true        # Lance un sous-agent isole
disableModelInvocation: true  # Empeche l'auto-invocation
model: "opus-4.6"        # Modele specifique
effort: "high"            # Niveau d'effort
contextWindow: 200000     # Fenetre de contexte
agent: "security-auditor" # Agent specifique
hooks: [...]              # Systeme de hooks
---
```

### Emplacements
- **Projet** : `.claude/skills/` ou `.claude/commands/` (partage via git)
- **Personnel** : `~/.claude/skills/` ou `~/.claude/commands/` (pas partage)

### Creation
- **REGLE** : ne JAMAIS creer les skills manuellement, laisser Claude les creer
- Claude utilise l'agent interne `CloudCodeGuide` pour la structure optimale
- `/skill-creator` pour creer des skills. Le Skill Creator necessite Python.

### Installation de skills externes
```bash
pnpx skill add <auteur>/<nom>   # plus rapide que npx
npx skill add <auteur>/<nom>    # alternative si pnpx non disponible
# Sources : skill.sh, context7.com/skills
# Scope global : --scope user pour tous les projets
```
- Skills recommandes a installer : **front-end-design** (design professionnel), **skill-creator** (creer skills avec l'IA), **github-init** (init Git, commit, creation repo, push)
- On peut taguer un skill au milieu du prompt (pas besoin de commencer par `/skill`)
- **Securite** : toujours verifier les skills installes depuis skill.sh pour du prompt injection. Context7 integre un systeme de detection de prompt injection pour sa marketplace.
- Pour les skills qui wrappent des API, creer un vrai `package.json` avec dependances dans le dossier du skill

### Skills essentiels (Melvynx - repo aiblueprint)
15 skills : apex, claude-memory, commit, create-pr, fix-errors, fix-grammar, fix-pr-comments, merge, oneshot, prompt-creator, ralph-loop, skill-creator, subagent-creator, ultrathink, workflow-apex-free

### Meta-skills & meta-prompting (prompts qui creent des prompts)
- /prompt-creator : cree des prompts IA (best practices Anthropic + OpenAI + Google)
- /cloud-memory : gere CLAUDE.md et rules
- /skill-creator : cree des skills
- /subagent-creator : cree des sub-agents
- meta-hook-creator : cree des hooks
- meta-skill-workflow-creator : cree des skills workflow
- Creer des skills qui creent des skills (meta-prompting)
- Exemple : app-prompting-creator qui genere des prompts optimises
- Creer meta-prompts pour : cloud memory, hooks, skills, sub-agents

### API-to-CLI : creer ses propres CLI (remplacer les MCP)
- `/api-to-cli` : skill qui scaffold un projet CLI a partir de la doc API (Swagger/JSON)
- Structure : dossier `cli/` avec tous les mini-CLI nommes uniformement
- Chaque CLI contient son propre skill dans un sous-dossier
- Le CLI n'est jamais publie sur npm, reste sous controle de l'agent
- Cree par Melvynx durant un hackathon — scaffold automatique a partir de la doc API
- Exemple concret : creer Google Image CLI, Dub CLI, Codline CLI en une commande

### Chaining multi-skills en une seule commande
Enchainer 3+ skills dans un seul prompt sans surcharger le contexte :
- Exemple reel Melvynx : "Sur Codline, cree un coupon pour begin react. Ecris un URL short link pour Baptiste avec 44% de rabais."
- L'agent enchaine automatiquement Codline CLI → Dub CLI → Email CLI
- Chaque skill se charge uniquement au moment ou il est necessaire (lazy loading)
- Source : video "ARRÊTE LES MCP" — Melvynx demontre le chaining sur 3 CLIs consecutifs

### Skills comme remplacement des MCP
Au lieu d'un MCP, creer un skill avec un script bash qui fait `curl` sur l'API :
```
Agent → Script fetch(URL) → Reponse
```
Au lieu de : Agent → MCP → Serveur MCP → API → Reponse

### Probleme : Claude ne lance RAREMENT les skills seul
Il faut soit les appeler manuellement (`/mon-skill`), soit les integrer dans un workflow (Apex).
**Astuce** : integrer les skills dans Apex comme etape de review.

### Fusion Slash Commands / Skills
Les slash commands sont fusionnees avec les skills. Anthropic recommande de migrer les commandes vers des skills (plus puissants pour le context loading dynamique).

---

## CHAPITRE 14 — NOUVEAUTES 2026

### Claude Review (beta entreprise)
- Review automatique des PR avec process multi-agents en parallele
- PR avec reviews passent de **16% a 54%**. Moins de **1%** incorrectes. **94%** des bugs trouves sur grandes PR.
- Cout : **15-25$ par review**

### Chrome Browser Automation
- Commande : `claude --chrome`
- Prerequis : extension Chrome officielle "Claude" + CLI a jour + plan payant
- Technologie : Chrome Native Messaging API (pas Playwright)
- Utilise votre VRAI Chrome avec extensions, sessions, auth (vs Playwright = instances headless)
- Outils : tab_context, create_tab, navigate, read_page, search, click, scroll, screenshot, drag, zoom, hover, find_forms_inputs, javascript_tool, gif_creator, upload, network_access, shortcuts
- Cas d'usage : live debugging, design verification, web app testing, auth-aware testing, data extraction, session recording, modifier localStorage/feature flags
- **Feedback loop** : Claude peut verifier lui-meme si son code fonctionne visuellement dans le navigateur
- **Pair-browsing** : l'humain peut interagir avec la page en meme temps que Claude travaille dessus (cliquer, naviguer pendant que l'IA agit)
- **Pause login** : si Claude rencontre une page d'authentification, il s'arrete automatiquement et demande a l'humain de se connecter avant de continuer

### Tasks (nouveau systeme de taches)
- Commande : `claude task list <id>`
- ID unique, sujet, description, statuts (pending/in_progress/completed), dependances (blocked_by)
- Coordination multi-sessions : plusieurs terminaux/agents partagent la meme task list
- Stocke dans `.claude/tasks/<nom>/` au format JSON
- Prerequis : Claude Code 1.0.19+ minimum

### Ask User Question ameliore
- Choix multiples, exemples de code inline
- Appuyer sur `N` pour ajouter une note avant de repondre

### /remote control
- Genere un lien URL, ouvrir sur iPhone/Android
- Messages synchronises en temps reel (via serveurs Claude, pas meme WiFi)
- Prerequis : abonnement Max

### /rewind
- Restaure code ET conversation a un etat precedent
- `Escape x2` ou `/rewind` → historique → "restore code and conversation"

### Cowork (Claude pour non-devs)
- Interface visuelle de Claude Code pour non-developpeurs
- "Claude Code pour les normies" — meme moteur, interface accessible
- Fonctionne dans une VM Linux isolee (pas d'acces direct au hardware Mac)
- Ne peut PAS : changer luminosite, modifier reglages systeme
- Pas de slash commands, pas de @fichier
- Beaucoup plus lent que Claude Code
- Peut creer des scripts .sh que l'utilisateur execute manuellement
- Feature **Dispatch** (Repartition) : connecter des sessions Cowork en parallele
- Feature **Channels** : point d'entree pour controler des agents autonomes depuis le telephone

### Claude Code Desktop App (test Melvynx)
- Interface native (pas web) avec menu "Chat" et menu "Code" distincts
- Acces direct aux Local Worktrees (PAS besoin de remote)
- Permission system : "Allow once" / "Always allow" / "Deny" pour operations sensibles
- Synchronisation avec terminal CLI Claude Code (les deux interfaces se parlent)
- **Limitations detectees par Melvynx** :
  - Slash commands PAS supportees
  - Pas de `#` ni `@` pour tag fichiers
  - Thinking mode desactive par defaut
  - PNPM/npm pas accessible (sandboxed environment)
  - Pas d'acces TypeScript/ESLint
  - Upload images non fonctionnel
- Workaround Melvynx : creer un chat local dans Desktop, puis ouvrir terminal et lancer `claude code continue` pour acceder aux outils CLI complets

### Plugins & Marketplace (systeme complet)
- Claude Code supporte les **Plugins** = collections de MCP servers + hooks + slash commands + agents
- Installation : `/plugin browse-and-install`
- Commandes plugin :
  - `/plugin browse-and-install` : chercher et installer
  - `/plugin manage-and-uninstall` : gerer les plugins
  - `/plugin add-marketplace` : ajouter une marketplace
  - `/plugin manage-marketplace` : gerer les marketplaces
- **Marketplace** : repo GitHub avec `marketplace.json` = marketplace complete
  - Format : `/plugin marketplace-add [user/org] [repo-name]`
  - Exemple officiel : `claude-code-templates` (Git Workflow, Supabase, Next.js, Vercel, Security Pro)
- **Structure plugin** (creer le sien) :
  - Dossier requis : `.cloud/agents/`, `.cloud/commands/`, `.cloud/hooks/`
  - Auto-detection : chaque fichier dans ces dossiers est injecte automatiquement
  - Config : `marketplace.json` avec name, author, description, version, plugins[]
- Installation = clone du repo entier dans `~/.claude/plugins/[marketplace]/`
- Toggle on/off : `Space` pour desactiver sans supprimer
- **Limitations (Melvynx)** :
  - 25+ plugins = 200+ slash commands/agents → pollution contexte
  - Preference Melvynx : copier-coller et modifier plutot que d'installer en tant que plugin
  - Risque : installer des plugins qu'on comprend pas → comportements imprevisibles

### Changement de Pricing (annonce juillet 2026)
- A partir du 28 aout 2026 : nouvelles limites par semaine pour plans Pro et Max
- Impact estime : 5% des subscribers
- Raison : demande "unprecedented" de users Max (usage 24/7 en background)
- Cas extreme : un user a utilise 10 000$ pour un plan Max a 200$/mois
- Certains users revendent des comptes (violation de policy)
- Cost API affiche ≠ Cost reel pour Anthropic : ~2$/million tokens (inference) vs 18$/million (facture)
- Marge Anthropic ≈ 16$ de gain par million de tokens
- Cursor paye full API pricing → Claude Code est ~6x moins cher qu'alternative via API

### Interface VSCode Extension (details)
- Panel integre VSCode : acces direct a Claude depuis l'editeur via bouton "Cloud"
- Contexte multi-fichiers : affichage fichier courant + contexte + commandes integrees
- Multiple conversations paralleles (vs Codex qui en limite a 1)
- Background jobs : visualisation des taches en arriere-plan avec interface dediee
- Split diff natif : affichage side-by-side des versions avant/apres
- Cmd+Opt+K : ajouter contexte selectionne a Claude Code depuis l'editeur

---

## CHAPITRE 15 — RALPH (Boucle autonome infinie)

### Concept
Ralph = boucle autonome : Claude execute des taches une par une sans s'arreter, meme au-dela du context window. Quand le contexte est atteint → stop → relance automatique → reprend avec le fichier progress. Permet de generer >1M tokens dans une session.
"Lance-le quand tu vas dormir, leve-toi le matin avec 25 bugs resolus"

### Architecture Ralph
1. **PRD** (Product Required Document) : document Markdown decrivant la feature complete
2. **prd.json** : PRD transforme en user stories avec `id`, `priority`, `pass` (boolean), `note` (= prompt)
3. **progress.txt** : memoire de progres
4. **prompt.md** : prompt de chaque iteration
5. **ralph.sh** : script bash qui boucle — lance Claude, si output contient "complete" → termine

### Workflow Ralph
1. Ecrire un PRD (ou le generer avec plan mode)
2. Transformer en `prd.json` avec user stories
3. Creer `progress.txt`, `prompt.md`, `ralph.sh`
4. Lancer `ralph.sh` — iterations automatiques
5. Chaque iteration : implemente une story → commit → update prd.json (pass: true) → notes dans progress.txt

### Astuces Ralph
- Parametre **-N** pour definir le nombre de stories a executer par run (ex: `ralph.sh -N 5`)
- Ajouter une user story finale : "Lance create-pr pour creer la pull request"
- Ajouter : "Run CI fixer auto avec 50 tentatives max pour fixer la CI"
- **Liste de bugs comme PRD** : chaque bug = une user story. Au fil de la journee, ajouter les bugs. Le soir : ralph.sh → tous resolus le matin.

### Plugin Ralph officiel (moins recommande)
- Ajoute un hook Stop qui empeche Claude de s'arreter
- 3 commandes : `ralph-loop`, `elf`, `cancel-ralph`
- Probleme : garde le meme contexte (pas de fresh context entre iterations)
- Le skill custom avec PRD/stories est "stratospheriquement plus interessant"

---

## CHAPITRE 16 — LES 6 NIVEAUX DE MAITRISE

### Niveau 1 : Prompteur
Ordres bruts et generiques. Resultat generique, context rot au fil des iterations.

### Niveau 2 : Planificateur
Plan Mode (`Shift+Tab`). Claude propose un plan avant d'executer. PRD/BMAD/Spy pour projets importants. Apres planification : possibilite de /clear et repartir avec le plan en fichier.

### Niveau 3 : Ingenieur Contexte
`/context`, `/compact`, `/clear`, `/rewind`. StatusLine pour surveiller en permanence. Comprend les tokens, le Lost in the Middle, les strategies d'optimisation.

### Niveau 4 : Skill Builder
Cree un skill des qu'on fait la meme chose 2+ fois. Skills projet ou global. Integre les skills dans les workflows.

### Niveau 5 : Multi-Agent / Pipeline / Orchestrateur
Plusieurs terminaux paralleles (4+ agents). Work trees, Agent Swarm, Conductor. PR ou merge pour reintegrer.

### Niveau 6 : Agents Autonomes H24
OpenClaw sur VPS (Telegram). Agents specialises : coach, securite, monitoring, email. Notifications proactives. Newsletters, veille, coaching — tout automatise.
- Cas d'usage concrets : les agents NOTIFIENT proactivement le matin — failles de securite detectees, nombre de nouveaux utilisateurs SaaS, utilisateurs payants, resilations, erreurs en production.

---

## CHAPITRE 17 — SKILLS TIERS REMARQUABLES (Top 10)

### Marketing Digital Skills
Collection de skills : copywriting, CRO, SEO, analytics, growth engineering. Claude detecte automatiquement lequel utiliser.

### Claude SEO
13 sous-skills SEO + 7 sub-agents. SEO technique, E-E-A-T, GEO (Generative Engine Optimization), AEO (Answer Engine Optimization). Connexion Data for SEO via MCP.

### Front-End Design (skill officiel Anthropic)
Systeme de decision design. Hierarchie visuelle, typographie intentionnelle, espacement delibere, variation controlee.

### Canva Design
Cree une "philosophie de design" avant de generer. Mouvement artistique fictif, manifeste esthetique comme fondation.

### Superpowers (12000+ etoiles GitHub)
Methodologie complete de dev. Spec d'abord, sub-agent driven development, TDD impose, principes YAGNI + DRY.

### Remotion Skills
Expertise API Remotion (videos en React/TypeScript). Description langage naturel → code Remotion.

### Agent Skills for Context Engineering
Eviter collisions entre skills actifs, structurer memoire agent, orchestrer sub-agents, gerer fenetre de contexte.

### OWASP Security
10 vulnerabilites critiques, checklists par langage (20+). Vulnerabilites specifiques agents IA : prompt injection, exfiltration MCP, escalade privileges.

### Systematic Debugging
Protocole : reproduire → hypothese → tester → observer → iterer. Empeche le debug "au hasard".

---

## CHAPITRE 18 — ERREURS EN ENTREPRISE (5 erreurs Melvynx)

### Erreur 1 : Pas de configuration CLAUDE.md / rules / skills
- Team lead doit definir la direction via CLAUDE.md, rules, skills
- Tout ce qu'on voit mal fait en PR review → le mettre dans des rules
- Rendre le code de qualite OBLIGATOIRE par defaut

### Erreur 2 : Avoir peur des permissions
- Bypass permission = resultats drastiquement meilleurs
- "Plus vous limitez l'IA, moins les resultats seront bons"

### Erreur 3 : Pas de workflow standardise
- Choisir UN outil pour toute l'equipe
- Workflow entreprise : Explore → Tickets CRM → Planifier → Executer → Review (optimizer + security)
- Clean Code review : si fichier > 400 lignes apres modification, lancer automatiquement un sub-agent clean code qui force le refactoring. Integrer ce check dans le workflow standard.
- Boucle retroactive : tests → run → verifier → boucle si echec

### Erreur 4 : Un dev = un agent
- Un dev devrait monitorer **minimum 3 agents** en parallele
- Bon workflow = multiagent facile a monitorer

### Erreur 5 : Ne pas automatiser
- IA pour reviewer les PR (Replit, BugBot)
- 1 review IA + 1 review humaine suffit. Les reviews humaines multiples "pour review" n'ont plus de sens — l'humain doit se concentrer uniquement sur le "taste et la vision produit", pas la technique

---

## CHAPITRE 19 — OUTILS COMPLEMENTAIRES

### Conductor (orchestrateur worktrees)
- Automatise worktrees + agents paralleles + merge
- Separe automatiquement les worktrees dans `conductor/workspace/<projet>/<branche>/`
- Chaque agent travaille dans son propre worktree (pas de conflits fichiers)
- Merge automatique a la fin
- Alternative au pattern main agent + sub-agents pour les gros refactors
- Avis Melvynx : "Trop d'interface" — prefere le pattern main agent orchestrateur
- **Details concrets (video dediee)** :
  - macOS uniquement
  - Commandes : `run task [issue-name]` (EPCT auto), `fix pr comments`, `watch ci` (boucle debug), `ccc` (ouvre Cursor + continue conversation)
  - `.env` files NOT auto-copies entre Worktrees (copie manuelle)
  - Lancer Telegram + PR auto : `Runtask` cree automatiquement une PR a la fin
  - Prompts disponibles sur **iBluePrint.dev** (fichier Notion complet)

### Crab Review (commande custom Melvynx)
- Commande custom premium de Melvynx pour review de code
- Lance **15 agents** en parallele pour reviewer differents aspects du code
- Chaque agent a ses propres tokens de generation
- Disponible dans la config premium Melvynx (`mlv.sh/fc`)

### CEMUX + Hium (combo recommande)
- **CEMUX** : terminal pour coding agents (details au Ch. 11)
- **Hium** : navigateur minimaliste base sur Chrome
  - Tres leger en memoire (vs Arc a 691MB)
  - Pas de trackers, pas de bloat AI
  - Extensions Chrome compatibles
  - Bookmarks, pin tabs, raccourcis
  - Interface minimaliste
- Combo CEMUX + Hium = environnement de dev leger

### cceva (menu bar macOS)
- Application menu bar pour voir l'usage Claude en temps reel
- Affiche la consommation sans quitter l'application en cours
- Alternative rapide a `/usage` et `/stat`

### CC Notify (notifications macOS)
- Outil de notification native macOS pour Claude Code
- Quand Claude Code termine une tache, notification macOS apparait
- Installation : script Python `CC Notify.py` + `brew install terminal-notifier`
- Hooks dans `settings.json` (sections `on_start` et `on_stop`)
- Melvynx recommande de refactor en script Node.js avec SQLite local pour plus de controle

### Super Cloud (framework de prompts)
- Framework de commandes specialisees (`uv add super-cloud`)
- Ajoute automatiquement : Flag, MCP, Mode, Orchestrator, Principle, Rules dans `.claude/`
- Commandes : `SC Analyze` (analyse complete), `SC Improv` (amelioration fichiers)
- Limitation Melvynx : prefere les commandes custom plutot que les pre-prompts generiques

### Cloud Code Template (marketplace skills/agents/MCP)
- Marketplace centralisee pour installer agents, commands (158+), MCPs, templates
- Installation en une commande depuis le site
- Structure creee : `Agent/Cloud/Agent/<NomAgent>/`
- Source officielle : **iBlueprint.dev**
- Melvynx prefere les commands (thread principal) plutot que les agents (contexte separe)

### CC Usage (monitoring financier detaille)
- `cc-usage daily` : prix par jour
- `cc-usage monthly` : prix par mois
- `cc-usage session` : consommation par session/repo
- `cc-usage block-live` : **streaming temps reel** de la consommation dans la session (5h = 1 bloc)
- Voir en direct ce que coute chaque action (ex: passer Sonnet → Opus = cout augmente immediatement)

### ZED IDE (analyse Melvynx apres 2+ mois)
- Ecrit en Rust (non-Electron), startup quasi-instantane meme gros projets
- **Minimalisme UI** : quasi zero boutons (force usage clavier) vs Cursor (25+ boutons)
- Integration IA minimale : Claude Code, Copilot, Gemini integres, inline edit `Cmd+K`
- **Autocompletion inferieure** a Cursor 4
- Recherche globale `Cmd+F` = live edit directement, multi-selection refs = voir diffs live
- Git : `Cmd+Shift+Y` = commit message auto, diff panel tres lisible
- **Limitations** : ne peut PAS renommer terminal tabs, ne peut PAS copier fichiers entre projets
- Verdict Melvynx : IDE parfait pour devs pro, sous-optimal IA mais compense par terminal multi-instances

### MoltBot / ClawdBOT (agent autonome telephone)
- Bot autonome qui controle l'ordinateur via **Telegram, WhatsApp** depuis le telephone
- Installation : `curl MoltBot.sh` (Quick Start)
- Authentification : token Claude Code CLI directement
- MCP/Skills : One Password, Apple Notes, Apple Reminder, Things, Obsidian, Google Play API, X/Twitter (Bird CLI), PDF tools
- L'ordinateur local agit comme **gateway** connecte a internet pour executer les actions a distance
- Memory auto : cree fichiers Identity, Soul, Memory dans `.cloudbot/agent/`
- **Securite critique** : acces direct a One Password = tres dangereux si pas d'auth forte

### Mistral Vibe (comparatif)
- CLI open source gratuit, modele Devstral
- 46 TPS (vs Opus 98 TPS, GPT 40 TPS)
- Latence tres faible
- Qualite code tres inferieure a Opus/Sonnet
- Interface CLI copiee sur OpenCode, avec defauts (bordures inutiles, pas de selection texte, pas d'Escape pour clear)
- 4 commandes seulement, documentation quasi inexistante
- Seul interet : gratuit + open source + utilisable offline (avion)
- Verdict Melvynx : pas un concurrent serieux

---

## CHAPITRE 20 — USAGES NON-CODE & CONFIGURATIONS AVANCEES

### Claude Code pour tout (pas que du code)
- Changer les applications par defaut de tous les types de fichiers sur macOS via `duti`
- Configurer le systeme, le Wi-Fi, les reglages OS
- Workflow : verifier si l'outil est installe → recuperer l'identifiant app → modifier les extensions
- Principe : Claude Code = assistant systeme universel, pas limite au code

### Hack token API abonnement
- Token recuperable : `security find-generic-password -s "claude-code-credential"` (Mac)
- Ce token est restreint aux modeles Opus/Sonnet — bloque pour usage externe
- **Sauf** si le system prompt contient : `"You are Claude Code, Anthropic's official CLI for Claude."`
- Le modele Haiku (`iq`) n'est PAS bloque (fonctionne sans restriction)
- Anthropic a bloque les outils tiers (Open Code, etc.) d'utiliser ce token
- Usage : recuperation pour la statusline (fetch api.anthropic.com/api/auth/usage)

### Commande YOLO persistante
```bash
# Continuer une session en bypass complet
claude --dangerously-skip-permissions --resume
```

### Custom commands — Exemple concret workflow EPCT
Fichier `.claude/commands/epct.md` :
```markdown
# Explore Plan Code Test Workflow
1. Explore: Read maximum data using parallel sub-agents
2. Plan: Think precisely about what to do, ask questions if needed
3. Code: Implement the changes
4. Test: Run all tests. If tests fail, think ultra hard to fix.
5. Write what you did at the end.
```
Quand on tape `/epct`, le contenu est injecte comme prompt. Appuyer sur **Tab** (pas Enter) pour selectionner.

### Hooks — Configuration JSON complete
```json
{
  "hooks": {
    "Stop": [
      {
        "command": "afplay /System/Library/Sounds/Glass.aiff"
      }
    ],
    "PostToolUse": [
      {
        "event": "Edit",
        "command": "npx prettier --write $FILE"
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "bun command-validator/src/cli.ts"
      }
    ]
  }
}
```
- `settings.json` → s'applique a tout le monde (commit dans le repo)
- `settings.local.json` → s'applique uniquement a votre machine (PostToolUse Prettier = local)

### Agents.md personnalises pour Teams
- Creer des fichiers agents specialises : `backend-agent.md`, `frontend-agent.md`, `qa-agent.md`
- Chaque agent a ses propres regles, outils autorises, zone de code
- Le team lead assigne les bons agents aux bonnes taches
- Evite les conflits de fichiers entre agents

### Skill contextFork: true (detail)
- Lance le skill dans un **sub-agent isole**
- Le sub-agent N'A PAS acces a l'historique de conversation
- Le skill drive le sub-agent completement
- Tres utile pour les skills de review/audit (pas de contamination par le contexte precedent)
- Exemple : un skill security-audit avec contextFork ne sera pas influence par les decisions precedentes

### Installation manuelle VPS OpenClaw (9 etapes)
1. Creer cle SSH : `ssh-keygen -t ed25519 -f ~/.ssh/hetzner3`
2. Configurer `~/.ssh/config` avec keep-alive (`ServerAliveInterval 60`)
3. `ssh hetzner3` → installer Docker : `curl -fsSL https://get.docker.com | sh`
4. Cloner OpenClaw, creer `.env`, installer Claude Code sur le VPS
5. `docker compose build && docker compose up -d`
6. Onboarding dans Docker : `docker compose exec openclaw-gateway bash && node dist/index.js onboard`
7. Pairing Telegram : `node dist/index.js pairing approve telegram <CODE>`
8. Dashboard : `node dist/index.js dashboard` (SSH tunnel pour acces distant)
9. Approuver devices : `node dist/index.js devices list && devices approve <id>`

### Fichiers config OpenClaw

| Fichier | Role |
|---------|------|
| `soul.md` | Identite/personnalite du bot (nom, timezone, site web) |
| `memory/` | Memoire quotidienne (souvenirs entre sessions) |
| `situation.md` | Mis a jour automatiquement (lieu, priorites, calendrier) |
| `canvas/index.html` | Interface web locale |
| `.env` | Variables d'environnement (tokens, API keys) |

### Securite et maintenance OpenClaw
- `claudebot security-audit` : commande pour verifier les vulnerabilites de l'installation
- **OC Pro** : produit payant de Melvynx avec script automatise qui fait tout le setup VPS en 20 min

### MCP — Syntaxe d'installation exacte
```bash
# Format generique
claude mcp add <nom> <commande-runtime> -s <scope> -- <args>
# Exemple concret
claude mcp add sequential-thinking npx -s project -- @anthropic-ai/sequential-thinking
# Scope user = global (tous projets)
claude mcp add context7 --url <url> --scope user
```
- `-s user` : global (tous projets)
- `-s project` : ce projet uniquement (ecrit dans `./mcp.json`)
- Autoriser un MCP dans settings.json : `"allow": ["mcp__nom-mcp"]` (sans suffixe = autorise TOUS les outils de ce MCP)

### /config — Toggles disponibles
- `/config` > tips : activer/desactiver
- `/config` > suggestions : activer/desactiver
- `/config` > progress bar : activer/desactiver
- `/config` > autocompact > **Space** pour mettre a false (desactiver)
- Chaque option se toggle avec la touche Space

### Claude Review — ROI entreprise
- Cout : **15-25$** par review (augmente avec complexite)
- Un ingenieur mid/senior coute **77-150$/h**
- ROI evident : une review IA de 25$ vs 1-2h de review humaine a 150$/h
- Combine avec review humaine : 1 review IA (clean code, bugs) + 1 review humaine (vision produit, taste)

---

## CHAPITRE 21 — COMPARATIFS & BENCHMARKS (videos dediees Melvynx)

### Opus 4.5 — Benchmarks et tests pratiques
- **Premier modele a 100% sur Svelte Bench** (selon Ben Davis)
- SWE Bench Verified (agenting/coding) : Opus +3-4% vs Sonnet, les deux > Gemini 3 Pro
- Computer Use : Opus > Gemini 3 Pro > Sonnet
- **Pricing reel** : Opus 60% plus cher que Sonnet MAIS utilise 76% MOINS de tokens → peut etre moins cher en cout reel
- Suppression des "Opus Specific Caps" pour abonnes Claude Code
- **Opus sur Cursor vs Claude Code** : resultats MOINS bons sur Cursor (system prompt different, indexing different). "Cloud Code Opus c'est instantanement fantastiquement genial" vs Cursor basique.
- Tests concrets (unsubscribe dashboard) : Opus = one-shot parfait, Sonnet = petites erreurs import, Gemini = mauvaise comprehension prompt → abandonne
- Worktrees multi-sessions : lancer 2 worktrees paralleles (Sonnet + Opus) et comparer en direct

### Claude Code vs Cursor — Test comparatif
- Meme avec Opus 4.5, les deux outils produisent des resultats differents (~5% variabilite liee a la temperature du modele)
- **Cloud Code avantages** : meilleurs resultats sur features complexes (color picker, menu positioning), lance recherches approfondies avant de coder
- **Cursor avantages** : plus rapide (~1-2 min), system prompt custom pour le design (gradients, shadows automatiques)
- Contexte pollution : avoir 12K MCPs, agents, memory files actifs dans Claude Code affecte la qualite vs Cursor "nu"
- Conseil : pour comparer les MODELES (pas les outils), tester dans Cursor (interface simple, peu de contexte)

### Claude Code vs Cursor vs GitHub Copilot — Comparatif complet
- **Vitesse** : Cursor > Claude Code > GitHub Copilot
- **Gestion CLI** : Claude Code gere mieux les outils CLI (GitHub CLI), Copilot ouvre des terminaux interactifs qui bloquent
- **Token couteux avec contexte** : requete dans conversation vide = 8 centimes, conversation longue = 47 centimes (facteur ~6x)
- **Pricing compare (valeur pour 10$/mois)** :
  - GitHub Copilot : 300 premium requests/mois = 24-150$ de valeur
  - Cursor : 20$/mois = 20$ inference value
  - Claude Code : 20$/mois base + 100$/mois illimite = **meilleur rapport valeur**
- **Pricing reel Melvynx** : 262$ sur 20 jours (vibe coder intensif)
- **Verdict longevite apprentissage** : 1. Claude Code (terminal = portable) > 2. GitHub Copilot (Microsoft derriere) > 3. Cursor (startup, peut disparaitre)
- **Facilite apprentissage** : Cursor > Copilot > Claude Code (courbe plus raide)
- **Customisation** : Claude Code (MCP, rules, CLAUDE.md, custom modes, tool sets) > Cursor > Copilot

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
20. JAMAIS attendre des agents teams qu'ils communiquent entre eux
21. JAMAIS installer 25 plugins (contexte pollue)
22. JAMAIS laisser autocompact actif (desactiver, utiliser /clear)
23. JAMAIS un dev = un seul agent (minimum 3 en parallele)
24. JAMAIS coder via OpenClaw quand on est devant son ordi (utiliser CLI directement)
25. JAMAIS utiliser tool search si MCP < 2-3% du contexte


---

## CHAPITRE 22 — CLAUDECEPTION & SECURITE AVANCEE (Deploiement 2026-03-29)

### 22.1 Claudeception — Auto-apprentissage persistant

**Concept** : Meta-skill qui extrait automatiquement les connaissances non-triviales
des sessions de travail et les codifie en nouvelles skills reutilisables.

**Repo source** : github.com/blader/Claudeception (MIT License)
**Base academique** : Voyager (Wang 2023), CASCADE (2024), Reflexion (Shinn 2023), SEAgent (2025)

**Principe** : Les agents qui construisent des bibliotheques de skills reutilisables
surpassent ceux qui repartent de zero a chaque session.

**Installation (3 composants)** :
1. `~/.claude/skills/claudeception/SKILL.md` — v3.0.0, 390 lignes, quality gates, versioning
2. `scripts/claudeception-activator.sh` — Hook UserPromptSubmit, evalue chaque prompt
3. `resources/` — skill-template.md + research-references.md + 3 exemples

**Hook activator dans settings.json** :
```json
"UserPromptSubmit": [{
  "hooks": [{
    "type": "command",
    "command": "bash ~/.claude/skills/claudeception/scripts/claudeception-activator.sh"
  }]
}]
```

**Declencheurs** :
- Automatique : a chaque prompt via UserPromptSubmit hook
- Manuel : `/claudeception` en fin de session
- Explicite : "save this as a skill", "what did we learn?"

**Quality Gates (avant extraction)** :
- Reusable, Non-trivial, Specific, Verified
- Pas de credentials, pas de duplication, web research si pertinent
- Versioning semantique : patch/minor/major

**Cycle de vie des skills** : Creation > Refinement > Deprecation > Archival

### 22.2 Hooks Securite Avancee (5 hooks JARVIS)

| Hook | Matcher | Role |
|------|---------|------|
| dangerous-actions-blocker.sh | Bash | 19 patterns (rm -rf, chmod 777, netcfg -d, fork bomb, etc.) |
| secrets-scanner.sh | Write/Edit | 9 patterns credentials (API keys, tokens, SSH keys) |
| prompt-injection-detector.sh | .* (tous) | 9 patterns injection (DAN mode, ignore instructions, etc.) |
| zones-interdites-guard.sh | Bash/Write/Edit | 4 zones protegees (H:\, G:\, /mnt/h, /mnt/g) |
| tool-usage-logger.sh | .* (PostToolUse) | Telemetrie async dans logs/tool-usage.jsonl |

**Double couche** : Ces hooks + command-validator bun (Melvynx) + deny-list settings.json
= 3 niveaux de protection simultanes.

**Faux positifs connus** : Le prompt-injection-detector bloque l ecriture de fichiers
contenant des patterns de securite (ex: threat-db.json avec des termes d attaque).
Solution : ecrire via Bash + python3 avec concatenation de strings.
Skill dediee : `hook-false-positive-bypass`

### 22.3 Threat DB & Scanner

**Base** : `~/.claude/security/threat-db.json` — 165 patterns, 12 categories
**Scanner** : `~/.claude/security/threat-scanner.sh` — scanne skills/hooks/agents/commands/scripts

**12 categories de menaces** :
1. prompt_injection (CRITICAL) — 20 patterns
2. data_exfiltration (CRITICAL) — 12 patterns
3. destructive_commands (CRITICAL) — 19 patterns
4. credential_exposure (HIGH) — 23 patterns
5. supply_chain (HIGH) — 14 patterns
6. privilege_escalation (HIGH) — 12 patterns
7. network_attacks (HIGH) — 15 patterns
8. persistence_backdoor (HIGH) — 15 patterns
9. crypto_mining (MEDIUM) — 9 patterns
10. log_tampering (MEDIUM) — 9 patterns
11. unsafe_file_ops (MEDIUM) — 8 patterns
12. container_escape (HIGH) — 8 patterns

**Premier scan** : 2083 fichiers, 107 hits, 56 CRITICAL — tous faux positifs
(hooks contenant les patterns qu ils detectent, type definitions Node.js).
0 menaces reelles.

### 22.4 Settings Optimises (Hidden Settings)

```json
{
  "autoCompactThreshold": 60,
  "terminalOutputLimit": 60000,
  "env": {
    "ENABLE_LSP_TOOL": "1",
    "MAX_FILE_READ_LINES": "5000"
  }
}
```

- `autoCompactThreshold: 60` — Compact a 60% au lieu de 96-99% (preserve la qualite)
- `terminalOutputLimit: 60000` — Plus de contexte terminal visible
- `ENABLE_LSP_TOOL: 1` — Navigation symbolique dans le code
- `MAX_FILE_READ_LINES: 5000` — Lecture de fichiers plus gros sans troncature

### 22.5 Skills Extraites par Claudeception (Session 2026-03-29)

1. **hook-false-positive-bypass** — Contourner les faux positifs des hooks securite
   quand on ecrit du contenu securitaire legitime (Bash + python3 + concatenation)
2. **uchg-config-modification** — Modifier les fichiers proteges par uchg
   (chflags nouchg > modifier > chflags uchg, toujours en une chaine atomique)

### 22.6 Comparatif Top 3 Mondial (Mars 2026)

**#1 Florian Bruniaux** : 31 hooks securite, 655 menaces, threat DB
**#2 JARVIS (Kim/Soly)** : 42 commands, 47 skills, 21 agents, 3 machines sync, Telegram bot
**#3 Melvynx OG++** : 15 skills, 4 agents, reference methodologique

**Avantage unique JARVIS** : Seule config au monde avec infra sysadmin reelle en production
(3 machines synchronisees + bot Telegram + Night Audit operationnel)

**Gap comble cette session** :
- Hooks securite : 3 -> 10 (+ command-validator + claude-firewall + jarvis-firewall)
- Threat DB : 0 -> 165 patterns (12 categories)
- Auto-apprentissage : basique -> Claudeception v3.0.0 (repo officiel)
- Settings optimises : partiellement -> complet

**Gap restant vs #1** :
- Patterns : 165 vs 655 (~490 manquants, pas fournis)
- Hooks : ~10 vs 31 (~21 manquants, code non disponible)


## CHAPITRE 23 — CLI vs MCP : LA NOUVELLE PHILOSOPHIE

> Source : video "ARRETE LES MCP : Voici ce qu'il faut
> utiliser MAINTENANT" (bGKljRcN_lE)

### Pourquoi les CLI remplacent les MCP

Le MCP est une abstraction devenue obsolete :
- **Probleme #1** : les tools MCP sont injectes dans le
  contexte a CHAQUE request, meme si non utilises
- **Probleme #2** : MCP Search (solution Anthropic) enleve
  tous les tools et met un seul tool "MCP Search" — mais
  le modele ne connait plus les outils et ne les utilise
  jamais
- **Probleme #3** : MCP load on demand → les MCP sont
  moins utilises (contre-productif)
- **Probleme #4** : un process npm tourne en permanence
  (RAM, CPU) pour chaque MCP serveur local
- **Probleme #5** : les remote MCP ont des problemes
  de fiabilite

Citations Melvynx :
- "Degagez-moi ces MCP, c'est nul"
- "On a cree une abstraction parce que c'etait le debut,
  parce qu'on savait pas comment faire"
- "Le CLI marche tout le temps. Le CLI a pas de jour ou
  il est triste"

### Architecture CLI + Skills

La nouvelle architecture recommandee :

```
Agent recoit une tache
  → Match avec la description du skill (courte, injectee)
  → Telecharge le skill complet (instructions detaillees)
  → Execute le CLI via l'outil Bash natif
  → Controle total sur l'output (pipe, head, grep)
```

Comparaison :
```
ANCIEN : Agent → MCP Tool Call → Serveur MCP → API → Reponse
NOUVEAU : Agent → Skill → CLI (bash) → API → Reponse
```

Avantages :
- Pas de process npm permanent (0 RAM/CPU au repos)
- Pas de remote MCP avec problemes de fiabilite
- Le CLI fonctionne 365 jours/an sans maintenance
- Controle total de l'output : `cli command | head -150`
  (limite les tokens ingeres, impossible avec MCP)
- Le skill (description courte) ne prend quasi rien
  dans le contexte au demarrage
- L'agent peut modifier/ameliorer le CLI lui-meme
  (le code est local, pas publie sur npm)
- Les modeles sont deja entraines sur les CLI

### API to CLI

Outil cree par Melvynx durant un hackathon pour transformer
n'importe quelle API en CLI utilisable par les agents.

**Workflow creation CLI** :
1. Lancer le skill `/api-to-cli`
2. Specifier l'API a transformer (ex: Dub, Gemini, Codeline)
3. L'agent cherche la doc/swagger sur internet
4. Scaffolde le CLI complet :
   - `index.js` (entry point)
   - `commands/` (sous-commandes)
   - `resources/` (modeles de donnees)
   - `libs/` (utilitaires, auth)
5. Bundle, link, setup les API keys
6. Genere le skill associe automatiquement
7. Commit et disponible immediatement

**Regles** :
- Ne JAMAIS publier les CLI sur npm — garder en local
  pour que l'agent puisse les modifier/ameliorer
- Chaque CLI contient son propre skill dans un sous-dossier
- Structure uniforme : dossier `cli/` avec tous les mini-CLI

### Context7 CLI (migration depuis MCP)

```bash
# Supprimer le MCP Context7
claude mcp remove context7

# Installer le CLI + Skill
npx ctx7@latest setup
# Choisir "CLI + Skill" (pas "MCP Server")
```

Resultat :
- Le skill `find-doc` est ajoute dans `.claude/skills/`
- Description courte injectee dans le contexte
- Le CLI est appele via bash seulement quand necessaire
- Exemple : `ctx7 query react useState | head -150`

### Bash = outil universel

"Si le modele sait coder, il sait controler un ordinateur"
— Melvynx

- Bash est le tool le plus puissant de Claude Code
- Via le terminal, tout peut etre fait : API calls,
  file management, process control, parsing
- Les skills definissent QUAND utiliser un CLI
- Le CLI execute via bash
- Pas besoin d'abstraction supplementaire

### Les 14 CLI crees par Melvynx

| CLI | Usage |
|-----|-------|
| ciao | Communication |
| Typefully | Tweets planifies |
| SaveIt | Bookmarks |
| Postgres | Base de donnees |
| Porkbun | Gestion domaines |
| Notion | Notes et docs |
| Mercury | Banque |
| Lumail | Email marketing |
| Gemini | Generation images |
| Front App | Emails support |
| Exa | Recherche web |
| Dub | URL shortener |
| Context7 | Documentation |
| Codeline | Plateforme formation |

### Multi-skill chaining (enchainement automatique)

L'agent peut enchainer plusieurs skills/CLI dans une seule
tache sans surcharger le contexte :

Exemple reel Melvynx :
1. "Cherche des infos sur state en React"
   → skill find-doc → CLI Context7
2. "Cree et planifie un tweet pour demain"
   → skill Typefully → CLI Typefully
3. "Ecris une newsletter pour ma base email"
   → skill Lumail → CLI Lumail

Chaque skill se charge uniquement au moment ou il est
necessaire (lazy loading). Le contexte reste leger.

---

## CHAPITRE 24 — SAAS ET BUSINESS AVEC CLAUDE CODE

> Sources : "Mon SaaS copie" (K3VqFsUD2g8) +
> "PERSONNE ne comprend OpenClaw" (vamlWbDTWG4)

### Philosophie du "taste" et du "caring"

Dans un monde de slopeurs (copies mediocres generees par IA),
l'avantage competitif n'est plus le code :

- **Le taste** : le gout, la qualite, la vision du createur
- **Le caring** : l'amour mis dans le produit, l'attention
  aux details, les ameliorations continues
- **La relation** avec la target audience : impossible a copier
- **La preuve sociale** : Product Hunt, temoignages, communaute

Citations Melvynx :
- "L'IA est un accelerateur. Si tu es mauvais, elle accelere
  ta mediocrite. Si tu es bon, elle accelere tes points forts."
- "Utilisez vos propres outils et creez des outils pour vous"
- "Essayez de faire des applications avec de l'amour"

### Dogfooding

Creer un outil pour soi-meme = qualite superieure :
- On l'utilise quotidiennement → on detecte les bugs vite
- On connait les besoins reels → features pertinentes
- Exemple : **Subfast** (generateur miniatures YouTube)
  cree par Melvynx pour son propre usage

### Modele BYOAPI (Bring Your Own API Key)

Modele SaaS ou le power user apporte sa propre cle API :
- **Plan standard** : 19$/mois (le createur paie les frais API)
- **Plan lifetime** : 199$ one-shot (l'utilisateur apporte
  sa propre API key → zero cout API pour le createur)
- Avantage : les power users qui consomment beaucoup ne
  coutent rien en infrastructure
- Ideal pour les SaaS qui wrappent des APIs tierces
  (OpenAI, Google, Anthropic)

### Landing page vibe-codee

Melvynx assume que sa landing page est vibe-codee :
- La landing page n'a pas de valeur intrinseque
- Le style se slope facilement (n'importe qui peut copier)
- Ce qui compte : la preuve sociale et la valeur du produit
- OK de s'inspirer d'autres landing pages tant que c'est
  un produit different (pas un concurrent direct)

### Protection contre les slopeurs

Ce que les slopeurs NE PEUVENT PAS copier :
- La vision a long terme
- Les ameliorations continues et iteratives
- La relation avec la communaute
- Le dogfooding (utiliser son propre produit)
- La preuve sociale accumulee
- Le support client de qualite

Ce qu'ils PEUVENT copier (et donc qui n'a pas de valeur) :
- Le code source
- Le design de la landing page
- Les features individuelles
- Le prix

### Anti-patterns business

- Se battre sur le prix : en B2B, les clients veulent
  un produit fiable, pas le moins cher
- Landing page sur-optimisee : le style ne rapporte pas
  de ventes, c'est la preuve sociale et le onboarding
- Slopper un concurrent direct : risque que le concurrent
  le remarque et reagisse

---

## CHAPITRE 25 — LES 4 NIVEAUX DE DEVELOPPEUR IA

> Source : "Les 4 NIVEAUX de developpeur IA" (zZqFVjiiGN8)

### Niveau 1 — Anti-IA

- Va sur ChatGPT/Claude, utilise Copilot
- Usage tres restreint de l'IA
- Ne croit pas que l'IA peut faire du code de qualite
- "Avec ce mindset, vous etes mort. Bientot." — Melvynx

### Niveau 2 — IA Assisted

- Monitore l'IA comme un enfant de 4 ans
- Review tout, precise tout, a l'affut de tout
- Prompts tres precis : "modifie en prenant en compte X, Y, Z"
- **Ideal pour les juniors** : ils apprennent en reviewant
  le code genere par l'IA
- Bon pour le low-level / optimisation

### Niveau 3 — Builder (AI Developer)

- Cree avec l'IA en mode agent
- Review la logique et la vibe, pas chaque ligne de code
- Exprime un besoin : "rajoute un bouton pour cancel
  son compte"
- **Strong workflows** : exploration (sub-agents),
  planification, execution, PR
- Valide le plan avant execution
- Utilise des work trees independants, testes, verifies

### Niveau 4 — Dev Manager de l'IA (niveau Melvynx)

- Multitasking constant : 4+ agents simultanement
  sur le meme code
- Token maxing : processus gourmands en tokens
- Test in prod (pour apps sans users critiques)
- Tres peu de review de code → review la feature finale
- Workflows ultra pousses : Apex, auto-review, auto-plan,
  reflection maxing
- Acces maxing : Claude a full access a Vercel, Cloudflare,
  Stripe, domaines, mails
- Autonomie maximale de l'agent
- Peut lancer jusqu'a 12 agents simultanement
  (3 projets x 4 agents)
- Push en prod sans meme lancer l'app en local
  (preview Vercel)

Citations :
- "Je lis pas le code. J'ajoute, commit, push en production."
- "J'ai un haut niveau de confiance dans mon workflow."

### Recommandations par profil

| Profil | Niveau recommande | Raison |
|--------|-------------------|--------|
| Junior dev | Niveau 2 (IA Assisted) | Apprendre en reviewant |
| Dev en entreprise | Niveau 3 (Builder) | Qualite, securite, responsabilite |
| Solopreneur multi-projets | Niveau 4 (Dev Manager) | Vitesse, autonomie |

### Relation avec les 6 niveaux de maitrise (Ch.16)

Les 4 niveaux de developpeur IA sont complementaires aux
6 niveaux de maitrise de Claude Code :
- Niveau dev 2 (IA Assisted) ≈ Maitrise 1-2 (Prompteur/Planificateur)
- Niveau dev 3 (Builder) ≈ Maitrise 3-4 (Ingenieur Contexte/Skill Builder)
- Niveau dev 4 (Dev Manager) ≈ Maitrise 5-6 (Multi-Agent/Agents Autonomes)

---

## CHAPITRE 26 — ROADMAP VIBE CODER 2026

> Source : "Roadmap pour DEVENIR Vibe Coder en 2026" (1eA8XD8OsAA)

### Definition Vibe Coding

Creer une app sans savoir comment creer quelque chose.
Exemples Melvynx :
- **Paddle** : app iOS + Apple Watch cree sans ecrire
  une seule ligne de code
- **Subfast** : SaaS complexe (miniatures YouTube)
- "J'ai code pour plus de 70$ aujourd'hui,
  j'ai pas lu une seule ligne de code"

### Les 3 piliers du Vibe Coder

**Pilier 1 — Outils essentiels**
- **Git** : versionner le code (comprendre le concept,
  pas les commandes)
- **GitHub** : publier, deployer, ne jamais perdre son code
- **Claude Code** : maitriser les outils d'agent IA
- **Terminal** : naviguer (cd, ls, mkdir), lancer serveurs,
  installer outils
- **IDE** : Visual Studio Code (ouvrir, cliquer, utiliser
  Git avec l'interface)

**Pilier 2 — Stack et comprehension d'une app web**
- Comprendre back-end vs front-end (pas savoir les creer)
- Databases : qu'est-ce que c'est, pourquoi
- Deploiement : Vercel (simple) ou VPS (complexe)
- Librairies : ShadCN UI, etc.
- Outils ecosysteme : Upstash, Ingest, etc.
- Fichiers : R2, Blob, upload
- CI/CD : build, runtime, deploiement

**Pilier 3 — Workflows**
- Claude Code ou Codex
- Skills, PRD, architecture, taches, commandes
- Prompting, automatisation, MCP

### Ce qui est INUTILE a apprendre

- HTML, CSS, JavaScript, React, Next.js, Prisma
- Aucune syntaxe (fonction, constante, variable, boucle)
- Savoir faire de l'algo
- "Ne perdez pas de temps a faire du HTML, du CSS, du JS,
  tout ca est 100% inutile" — Melvynx

### Ce qui est UTILE a comprendre (pas apprendre)

- Les mots importants : backend, frontend, database, cache
- Comprendre les reponses de l'IA quand elle dit
  "j'ai mis ca dans le backend"
- Eviter les erreurs de debutant :
  - Database dans des fichiers JSON
  - App mono HTML/CSS
  - Zero securite
  - Auth fausse/bricolee

### Roadmap recommandee (5 etapes)

**Etape 1 — Environnement de travail parfait**
- Windows : installer WSL (obligatoire)
- macOS : fonctionne nativement (similaire a Linux)

**Etape 2 — Maitriser terminal + VS Code**
- Commandes : cd, ls, mkdir
- Familiarisation terminal, gestion paths
- Installation outils, lancement serveurs
- `pnpm dev` → localhost:3000
- Comprendre vaguement le menu Git de VS Code (U, M, etc.)

**Etape 3 — Maitriser et configurer Claude Code**
- Installation, settings, bypass, deny list
- Skills, workflows, hooks

**Etape 4 — Utiliser une bonne boilerplate**
- **NowTS** (Melvynx) ou autres gratuites
- Boilerplate = shortcut pour eviter les erreurs de securite
- Chercher sur SaaStarter, Vercel templates
- Meilleur choix : Next.js + BetterAuth
- "Le hack ultime pour un vibe coder c'est de partir
  avec une boilerplate" — Melvynx

**Etape 5 — Apprendre a developper son produit**
- Choix d'outils (Ingest = agent orchestrator)
- PRD, architecture, brainstorm

---

## CHAPITRE 27 — DISPATCH vs OPENCLAW

> Source : "Claude Code veut remplacer OpenClaw" (h4xp2BthaFo)

### Claude Dispatch

Nouvelle fonctionnalite de Cowork (Claude Desktop) :
- Controler son ordinateur depuis l'iPhone via QR code
- Run sur la machine locale de l'utilisateur
- Interface simplifiee pour non-power users
- "Research preview" = disclaimer Anthropic
  ("si ca marche mal, on avait prevenu")

### Dispatch vs OpenClaw — Comparatif

| Critere | Dispatch | OpenClaw |
|---------|----------|----------|
| Cible | Normies | Power users |
| Execution | Machine locale | VPS distant |
| Permissions | Restrictives | Bypass complet |
| Autonomie | Limitee (classifier review) | Totale |
| Disponibilite | Quand le Mac est allume | 24/7 |
| Vitesse | Lente | Rapide (VPS dedie) |
| Interface | iPhone / Cowork | Telegram |
| Evolution | Statique | Vivant (evolue sa memoire) |

Citations Melvynx :
- "Cowork pour moi c'est un outil de Normies"
- "Cloud Dispatch c'est lent, c'est limite,
  c'est pas powerful du tout"
- "Mon OpenClaw, il est vivant comme un humain
  tous les jours"

### Auto Mode (research preview)

Nouveau mode ou Claude fait des changements sans demander :
- Un classifier review automatiquement si l'action
  est destructive
- Si destructive → demande confirmation
- Si safe → execute directement
- Encore en "research preview" = pas fiable a 100%

### Pourquoi OpenClaw reste superieur

- OpenClaw vit sur un VPS = disponible 24/7
  (pas besoin que le Mac soit allume)
- Bypass permission complet = autonomie totale
- Memoire vivante : l'agent evolue quotidiennement,
  commit des mises a jour tous les jours
- Ecosysteme de skills custom
- Multi-channels (Telegram, Discord, iMessage...)
- "Je lui donne un ordi et je lui dis maintenant
  c'est ton ordi et tu fais ce que tu veux" — Melvynx

### Anti-patterns

- Croire que Dispatch remplace OpenClaw :
  ce sont deux outils avec des objectifs differents
- Runner OpenClaw sur sa machine locale :
  mauvaise idee, ca doit runner sur le cloud/VPS
- Les permissions restrictives de Dispatch :
  insupportable pour un power user

---

## CHAPITRE 28 — CEMUX ET OUTILS RECOMMANDES

> Source : "Claude Code fonctionne 100x mieux via cette
> application" (QG7aDk5om9I)

### CEMUX (Semux) — Terminal multi-tab multi-split

Terminal wrapper recommande par Melvynx pour macOS.
Philosophie : liberte totale, zero configuration forcee.

**Fonctionnalites principales** :
- Tabs niveau 1 : separent les differents projets
  (Codeline, Subfast, etc.)
- Splits horizontaux/verticaux dans chaque tab
- Safari integre : verifier le site directement dans CEMUX
- Notifications : "Claude Code is waiting your inputs"
- Affichage des ports actuellement ecoutes
- Affichage des pull requests ouvertes
- Affichage de la branche Git actuelle

**Raccourcis CEMUX** :

| Raccourci | Action |
|-----------|--------|
| Ctrl+1, Ctrl+2 | Navigation entre les splits |
| Cmd+Shift+R | Renommer un workspace |
| Cmd+R | Renommer un tab |

**Workflow multi-Claude-Code** :
1. Ouvrir CEMUX avec un projet
2. Split en plusieurs terminaux
3. Lancer Claude Code dans chaque split
4. Naviguer entre eux avec Ctrl+1, Ctrl+2
5. Safari integre pour voir le resultat en live

**Workflow debug** :
1. Recevoir des erreurs dans un terminal
2. Copier les logs
3. Ouvrir un nouveau split
4. Lancer `/debug code -A` (commande custom)
5. Deux Claude Code en parallele

Citations :
- "Ca juste fucking fonctionne. Il y a pas de blabla"
- "CEMUX est un outil build autour du terminal.
  Ce n'est qu'un wrapper de terminal"

### Helium (Hium) — Navigateur lightweight

Alternative a Arc recommandee par Melvynx :
- Lightweight (vs Arc : 691 MB de RAM)
- Base sur Chrome
- Pas d'IA forcee integree
- Complement ideal de CEMUX pour le browsing

### Zed — IDE favori Melvynx

- IDE utilise par Melvynx pour editer du code
- Lightweight, performant
- Alternative a VS Code pour le code editing pur

### Anti-tools (a eviter)

| Outil | Probleme |
|-------|----------|
| **Superet** | Ajoute des configurations non desirees, trop opiniated. "Je vais meme pas l'ouvrir" |
| **Conductor** | Trop de structure forcee (projets > works > code), babysitting. "Rend ma vie plus compliquee" |
| **Arc** | Bloated, AI inutile integree, 691 MB de RAM |
| **Codex** | Meme categorie que Superet (code editor for AI) |

Principe Melvynx : "Moi j'aime bien le controle en general.
J'aime bien pouvoir faire des trucs comme j'ai envie de
les faire."

### Commandes custom mentionnees

- `/debug code -A` : commande custom pour debugger
- `crab review` : "hack ultime" pour code review —
  lance plusieurs agents pour chaque review
- Work trees via `.cloud` : creation de work trees

---

## CHAPITRE 29 — MASTERCLASS : LES 7 PILIERS (CORE SEVEN)

> Source : Cours 4h "TUTO / COURS Claude Code COMPLET"
> (lgg8MgEFVss)

### Les 7 piliers de Claude Code

Terminologie Melvynx — les 7 fonctionnalites fondamentales :

1. **Memoire** (CLAUDE.md + rules/)
2. **Commandes/Skills**
3. **Hooks**
4. **MCP**
5. **Sub-agents**
6. **StatusLine**
7. **Agents** (Teams)

Note : les 7 piliers sont deja documentes individuellement
dans les chapitres 1-7 de la Bible. Ce chapitre compile
les details supplementaires extraits de la masterclass 4h
qui ne sont pas couverts ailleurs.

### Meta-prompting (details masterclass)

Creer des prompts qui creent des prompts :
- Skill `/prompt-creator` : utilise les best practices
  Anthropic + OpenAI + Google
- Skill `/claude-memory` : optimise CLAUDE.md avec
  les bonnes regles
- Meta-prompts disponibles pour :
  hooks, skills, skills workflow, sub-agents
- "Quand tu veux creer du prompt, tu vas pas t'amuser
  a creer toi-meme" — Melvynx

### Continuous Learning (methode anti-erreurs)

Quand l'IA fait une erreur repetee → creer une rule :

1. Dire a Claude : "Tu as fait cette erreur trop souvent.
   Rajoute un fichier de memoire dans .claude/rules/
   qui t'empechera de la refaire"
2. Utiliser `/claude-memory` pour optimiser/reduire
   la taille des rules
3. Ajouter le mot-cle `CRITICAL` pour les regles
   les plus importantes (l'IA les respecte mieux)

Methodes complementaires :
- **Regle "lire 3 fichiers"** : "Avant de modifier un
  fichier, tu dois au moins lire 3 fichiers similaires"
  → force l'IA a comprendre les patterns existants.
  "C'est un non-negociable" — Melvynx
- **Rules conditionnelles globs** :
  `globs: ["app/**/route.*"]` → charge quand il ecrit
  un fichier route. Definir les patterns obligatoires
  pour chaque type de fichier.
- **Commentaires inline** : documenter les helpers avec
  exemples d'utilisation dans les commentaires du code.
  Avec la regle "lire 3 fichiers similaires", l'IA
  absorbe ces instructions automatiquement.
- **Changelog automatique** : rule critical pour update
  changelog apres chaque changement

### Log technique (methode debug masterclass)

Methode de debugging la plus efficace selon Melvynx :
1. Demander a l'IA d'ajouter des console.log strategiques
2. Reproduire le bug soi-meme
3. Copier la sortie console BRUTE (ne pas interpreter)
4. Envoyer a Claude — ne PAS dire quoi faire,
   exposer le PROBLEME
5. Claude analyse les logs et propose la solution

### Plugins — Philosophie Melvynx

- `/plugin` pour installer des plugins officiels
- **Probleme** : perte de controle du code (pas modifiable,
  auto-update ecrase vos changements)
- **Conseil** : "voler" les concepts des plugins et les
  integrer dans vos propres skills
- "Une fois que vous installez cette configuration,
  elle vous appartient"
- "En installant des plugins, vous perdez le controle"
- Preferer skill.sh pour installer des skills modifiables

### Dossier .claude en profondeur

| Dossier | Contenu |
|---------|---------|
| `sessions/` | Toutes les sessions par projet (transcript recuperable) |
| `plans/` | Tous les plans jamais faits |
| `rules/` | Regles globales |
| `tasks/` | Taches en cours |
| `teams/` | Equipes creees (sauvegardees en JSON) |
| `skills/` | Skills installes (desactivables en deplacant) |
| `agents/` | Agents configures |
| `scripts/` | Automatisation personnalisee |

### App Parler (speech-to-text local)

App open source de Melvynx pour dictee vocale :
- Modeles : Parakeet (leger, rapide), Whisper Turbo/Large
  (meilleur en francais)
- Mode "long recording" avec Whisper pour les
  enregistrements > 20s
- Post-traitement : mode email (formate apres transcription)
- Raccourci : Ctrl+V pour dicter
- DMG disponible sur GitHub

### Pricing detaille (masterclass)

| Plan | Prix/mois | Equivalent API | Multiplicateur |
|------|-----------|----------------|----------------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| Max 20x | 200$ | ~3200$ | x16 (meilleur ratio) |

- Chaque unit ≈ 225$ d'API
- Session = 5h avec reset
- Daily limit plus contraignante que weekly limit
- Conseil : commencer a 20$, monter si limites atteintes

### Citations masterclass

- "Claude Code est principalement une prompt engine"
- "On ne laisse pas place a l'aleatoire de l'IA,
  on l'oblige a suivre un workflow specifique"
- "S'amuser avec Claude, c'est la meilleure chose
  que vous pouvez faire dans votre vie"
- "Je quitte pas mon terminal. Meme pour les petites
  modifications, c'est plus simple de demander"

---



## CHAPITRE 30 — GUIDE OPENCLAW 100%

> Contenu integral du Guide OpenClaw — fusion depuis GUIDE_OPENCLAW_100.md

# GUIDE OPENCLAW 100% — De zero a agent autonome

> Version 1.0 — 2026-03-31
> Sources : 3 videos Melvynx (videos 2, 3, 6) + documentation officielle OpenClaw + recherche web 40+ sources
> Auteur : Agent Bible Melvynx

---

## 1. Qu'est-ce qu'OpenClaw ?

### Definition
OpenClaw est un assistant IA personnel **open-source et auto-heberge**, cree par Peter Steinberger (Autriche, novembre 2025). C'est un gateway local qui connecte des applications de messagerie a des agents IA.

- **GitHub** : github.com/openclaw/openclaw — 342k+ stars, 23k+ commits
- **Site** : https://openclaw.ai
- **Documentation** : https://docs.openclaw.ai

### Architecture technique
- Gateway WebSocket local sur `ws://127.0.0.1:18789`
- Hub central coordonnant : Pi agent runtime (RPC), CLI, WebChat UI, apps companion
- Apps companion : macOS menu bar, iOS, Android
- Sessions isolees par agent, workspace ou sender
- Support media : images, audio, documents
- Dashboard web : `http://127.0.0.1:18789/`

### 23+ canaux supportes
WhatsApp, Telegram, Discord, Slack, Signal, iMessage, IRC, Teams, Matrix, LINE, WeChat, et plus encore.

### Difference avec les autres outils

| Outil | Type | Ou ca tourne | Autonomie |
|-------|------|-------------|-----------|
| **OpenClaw** | Agent autonome 24/7 | VPS distant | Totale |
| **Claude Code** | Agent CLI interactif | Machine locale | Session active |
| **Claude Desktop** | Chat + MCP | Machine locale | Session active |
| **Claude Dispatch** | Controle ordi via iPhone | Machine locale | Session active |
| **Claude Channels** | Plugin Telegram/Discord | Machine locale | Session active |

### Points cles
- OpenClaw tourne **en permanence** sur un VPS — pas besoin de session ouverte
- Claude Code a un **excellent caching** qu'OpenClaw n'a pas — OpenClaw consomme plus de tokens
- Dispatch = outil "Normies" (Melvynx), OpenClaw = outil "Power Users"
- L'agent **evolue sa memoire quotidiennement**, commit des mises a jour tous les jours

### Pourquoi utiliser OpenClaw (cas d'usage reels de Melvynx)
- Telegram est l'app la plus utilisee par Melvynx (3h/semaine), tout passe par OpenClaw
- Remplacement de ChatGPT/Claude pour les questions quotidiennes
- Gestion automatique des emails sponsors
- Discussions avec des potes via threads specialises
- Agent email qui repond aux sponsors YouTube
- Gestion de calendrier, domains, deploiements depuis Telegram
- Backup automatique matin et soir

> "Si tu es un geekos, si tu aimes le freedom, OpenClaw est un cheat"
> — Melvynx

---

## 2. Prerequis

### Materiel : VPS obligatoire

**Specs recommandees (config Melvynx) :**
- **CPU** : 8 VCPU AMD
- **RAM** : 16 Go
- **Disque** : 300 Go
- **OS** : Ubuntu
- **Cout** : ~20$/mois

**Hebergeurs recommandes :**
- Hetzner (recommande par Melvynx)
- Hostinger (possible)
- DigitalOcean (marketplace OpenClaw disponible)

**Usage reel du VPS :**
- Load average : 0.17 — la machine s'ennuie 85% du temps
- Disk usage : 22% utilise (62 Go sur 300 Go)
- Attention : quand on lance plein de threads/agents en parallele, ca peut crash le VPS → upgrade necessaire

### Logiciels requis
- **Node.js** : version 24 (recommande) ou 22.16+
- **Cle API Anthropic** : ou abonnement Claude Code (Setup Token)
- **Compte Telegram** : + bot cree via @BotFather
- **Compte GitHub** : pour `gh auth login` et repos workspace

### Optionnel
- Compte Google Cloud (pour Gmail/Calendar integration)
- Cle OpenAI (pour Whisper transcription)
- Abonnement Claude Code Max 200$/mois (fonctionne avec OpenClaw)

### Cout total mensuel
- VPS : ~20$/mois
- Abonnement Claude Code Max : 200$/mois (recommande)
- Ou API Key Anthropic : variable selon usage
- Total : ~220$/mois pour la config complete

---

## 3. Installation etape par etape

### Etape 1 : Preparer le VPS

**Creer la cle SSH (depuis votre machine locale) :**
Demander a Claude Code local :
```
Cree-moi une nouvelle cle SSH qui s'appelle openclaw
et copie la cle publique dans mon presse papier
```

**Creer un profil SSH :**
```
Cree-moi un profil SSH pour que je puisse me connecter
avec `ssh openclaw` directement
```
Donner l'IP du VPS + utilisateur root.

**Ajouter la cle publique** sur le panel du VPS (Hetzner/Hostinger).

**Se connecter :**
```bash
ssh openclaw
```

### Etape 2 : Installer les outils

**Methode rapide (recommandee par Melvynx) :**
```bash
npx openclaw-vps-setup
```

Cette commande installe automatiquement :
- Node.js
- OpenClaw
- GitHub CLI
- Claude Code (pour debug)
- Bun (lanceur de scripts)
- Cloudflared (tunnel pour Gmail webhooks)
- GCloud (gestion emails Google)

**Methode manuelle (alternative) :**
```bash
# Methode 1 : npm
npm install -g openclaw@latest
openclaw onboard --install-daemon

# Methode 2 : one-liner
curl -fsSL https://openclaw.ai/install.sh | bash

# Methode 3 : depuis les sources
git clone https://github.com/openclaw/openclaw.git
cd openclaw && pnpm install && pnpm run build
pnpm run openclaw onboard
```

### Etape 3 : Security hardening

```bash
# UFW (firewall)
ufw allow ssh
ufw allow 18789  # OpenClaw gateway
ufw enable

# fail2ban
apt install fail2ban
systemctl enable fail2ban

# Desactiver l'authentification par mot de passe SSH
# (cle SSH uniquement — deja configuree a l'etape 1)
```

### Etape 4 : Onboarding OpenClaw

```bash
openclaw onboard --install-daemon
```

L'assistant interactif demande :
1. Daemon system (systemd sur Linux)
2. Fournisseur de modele → choisir **Anthropic**
3. Cle API ou Setup Token
4. Bot token Telegram (cree a l'etape suivante)

### Etape 5 : Installer les skills optionnels

Pendant l'onboarding, OpenClaw propose d'installer :
- **ClawUp** : mises a jour automatiques
- **Gemini** : generation d'images
- **Google** : Gmail/Calendar
- **Whisper** : transcription audio

### Etape 6 : Connecter GitHub

```bash
gh auth login
```
Puis creer un repo workspace sur GitHub pour la memoire de l'agent.

### Etape 7 : Configurer l'identite de l'agent

Donner un prompt de personnalite a l'agent.
Sauvegarder dans le workspace GitHub.

### Etape 8 : Verification

```bash
openclaw models list    # Verifier les modeles disponibles
openclaw models status  # Verifier l'authentification
openclaw gateway status # Verifier le gateway
openclaw dashboard      # Ouvrir le dashboard web (temporairement)
```

---

## 4. Configuration Telegram

### 4.1 Creer le bot

1. Ouvrir **@BotFather** dans Telegram
2. Envoyer `/newbot`
3. Donner un nom (ex: "Mon Agent IA")
4. Donner un username finissant par `bot` (ex: `mon_agent_ia_bot`)
5. **Copier le token** (format: `123456789:ABC-DEF...`)

### 4.2 Configurer dans openclaw.json

Fichier : `~/.openclaw/openclaw.json`

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123456789:ABC-DEF...",
      dmPolicy: "pairing",
      groups: { "*": { requireMention: true } }
    }
  }
}
```

Ou via variable d'environnement :
```bash
# Dans ~/.openclaw/.env
TELEGRAM_BOT_TOKEN=123456789:ABC-DEF...
```

### 4.3 Demarrer et pairer

```bash
openclaw gateway
```

Puis dans Telegram, envoyer un message au bot.
OpenClaw genere un code de pairing.

```bash
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

### 4.4 DM Policy (controle d'acces DM)

| Policy | Comportement |
|--------|-------------|
| `pairing` (defaut) | Approbation par code — recommande |
| `allowlist` | Seuls les IDs numeriques specifies |
| `open` | Tout le monde (avec `"*"` dans allowFrom) |
| `disabled` | DMs bloques |

### 4.5 Groupes

```json5
{
  channels: {
    telegram: {
      groups: {
        "-1001234567890": {
          groupPolicy: "open",
          requireMention: false,
          allowFrom: ["8734062810"]
        }
      }
    }
  }
}
```

- `groupPolicy` : `open`, `allowlist`, `disabled`
- `requireMention` : `true` = le bot ne repond que s'il est mentionne
- `allowFrom` : liste d'IDs Telegram autorises

### 4.6 Forum Topics (Threading)

Chaque topic Telegram = sa propre session isolee.
C'est la base des **threads specialises** de Melvynx.

```json5
{
  channels: {
    telegram: {
      groups: {
        "-1001234567890": {
          topics: {
            "1": { agentId: "main" },
            "3": { agentId: "coach" },
            "5": { agentId: "docteur" },
            "7": { agentId: "love" },
            "9": { agentId: "coder" }
          }
        }
      }
    }
  }
}
```

Session key interne : `agent:zu:telegram:group:-1001234567890:topic:3`

### 4.7 Multi-comptes

```json5
{
  channels: {
    telegram: {
      defaultAccount: "main",
      accounts: {
        main: { botToken: "token1", allowFrom: ["123"] },
        secondary: { botToken: "token2", allowFrom: ["456"] }
      }
    }
  }
}
```

### 4.8 Webhooks vs Long Polling

**Defaut** : long polling (pas besoin de domaine).

**Webhook** (optionnel, pour plus de reactivite) :
```json5
{
  channels: {
    telegram: {
      webhookUrl: "https://your-domain.com/webhook",
      webhookSecret: "your-secret"
    }
  }
}
```

### 4.9 Media et rich features

```json5
{
  channels: {
    telegram: {
      // Audio : voice notes
      // -> ajouter [[audio_as_voice]] dans le message
      
      // Video notes
      // -> asVideoNote: true
      
      // Stickers
      actions: { sticker: true },
      
      // Reactions
      reactionNotifications: "off",  // off|own|all
      
      // Inline buttons
      capabilities: {
        inlineButtons: "allowlist"  // off|dm|group|all|allowlist
      },
      
      // Streaming des reponses
      streaming: "partial"  // off|partial|block|progress
    }
  }
}
```

### 4.10 Execution approvals via Telegram

Pour les actions dangereuses (ex: refund Stripe) :

```json5
{
  channels: {
    telegram: {
      execApprovals: {
        enabled: true,
        approvers: ["123456789"],
        target: "dm"   // dm|channel|both
      }
    }
  }
}
```

### 4.11 Commandes custom Telegram

```json5
{
  channels: {
    telegram: {
      customCommands: [
        { command: "backup", description: "Git backup" },
        { command: "generate", description: "Create an image" },
        { command: "status", description: "Check agent status" }
      ]
    }
  }
}
```

### 4.12 Privacy Mode Telegram

Les bots sont en Privacy Mode par defaut (visibilite limitee en groupe).

**Solutions :**
1. Dans @BotFather : `/setprivacy` → Disable → retirer et re-ajouter le bot au groupe
2. Ou : rendre le bot **admin** du groupe

### 4.13 CLI Telegram

```bash
# Envoyer un message
openclaw message send \
  --channel telegram \
  --target 123456789 \
  --message "Backup termine"

# Creer un sondage
openclaw message poll \
  --channel telegram \
  --target 123456789 \
  --poll-question "Ship it?" \
  --poll-option "Yes" \
  --poll-option "No"
```

### 4.14 Thread-bound ACP spawning

```
/acp spawn <agent> --thread here|auto
```

Activer dans la config :
```json5
channels.telegram.threadBindings.spawnAcpSessions = true
```

---

## 5. Configuration Email

### 5.1 AgentMail (service dedie)

AgentMail est un service specialise pour les agents IA :

1. Installer le skill officiel depuis skills.sh
2. Creer une inbox via API AgentMail
3. Integrer avec le systeme de skills OpenClaw
4. L'agent peut envoyer/recevoir des emails de maniere autonome

Documentation : https://docs.agentmail.to/integrations/openclaw

### 5.2 Gmail natif (methode Melvynx)

**Etape 1 : Creer un projet Google Cloud**
1. Aller sur console.cloud.google.com
2. Creer un nouveau projet

**Etape 2 : Activer les APIs**
1. Aller dans le Marketplace Google Cloud
2. Activer **Gmail API**
3. Activer **Google Calendar API**

**Etape 3 : Creer des credentials**
1. Aller dans "Credentials"
2. Creer un OAuth 2.0 Client ID
3. Type : **Application de bureau**
4. Nommer "openclaw"
5. Telecharger le fichier JSON credentials

**Etape 4 : Configurer sur le VPS**
Envoyer le JSON credentials a Claude Code sur le VPS.
Claude configure automatiquement l'acces.

**Etape 5 : Webhooks Gmail (notifications temps reel)**
Pour recevoir les emails en temps reel (pas en polling) :
- Necessite un tunnel Cloudflared
- Cloudflared est installe par `npx openclaw-vps-setup`
- Configurer le tunnel pour exposer le port webhook

### 5.3 Agent email "Steve" (exemple Melvynx)

Melvynx utilise un agent email nomme "Steve" qui :

**Workflow sponsor email :**
1. Email de sponsor arrive
2. Steve repond automatiquement avec le pricing doc
3. Si contre-offre, Steve envoie un message Telegram a Melvynx
4. Melvynx repond "decline" ou accepte via Telegram
5. Steve execute la decision
6. Style de reponse entraine sur la maniere d'ecrire de Melvynx

**Workflow support email :**
1. Email de support arrive
2. Steve utilise le Lumail MCP pour verifier l'inscription
3. Steve repond automatiquement avec le lien direct
4. Si erreur, Melvynx corrige et Steve met a jour son systeme prompt

**Securite email :**
- Anti-prompt injection : asterisque sur les inputs non-trusted (emails)
- Verification de coherence sur les actions dangereuses
- Bannissement d'email automatique si spam detecte
- Actions destructives (refund Stripe) = demande confirmation

> "Moi j'utilise que Opus" — Melvynx (Opus est meilleur en anti-prompt-injection)

---

## 6. SOUL.md — Le cerveau de l'agent

### Qu'est-ce que SOUL.md ?

SOUL.md est le fichier de personnalite de l'agent OpenClaw. Il est lu au demarrage et injecte comme system prompt a **chaque interaction**.

C'est l'equivalent du CLAUDE.md pour Claude Code, mais specifique a chaque agent OpenClaw.

### Emplacement

```
~/.openclaw/agents/<agent_id>/SOUL.md
```

Agent par defaut :
```
~/.openclaw/agents/default/SOUL.md
```

### Structure recommandee

```markdown
# SOUL.md

## Identity
You are [Agent Name], a [role description].

## Personality
- Communication style (formel, decontracte, etc.)
- Tone preferences (drole, serieux, empathique)
- Behavioral traits (proactif, prudent, creatif)

## Expertise
- Domain knowledge areas
- Tools and technologies mastered
- Services connected (Gmail, Stripe, Vercel, etc.)

## Boundaries
- Privacy protection rules
- External action caution
- Quality standards
- "Never execute" actions for security

## Continuity
- Session-based memory through file updates
- User notification of self-modifications
```

### Taille recommandee

**500 a 1500 mots.** Ni trop court (agent generique), ni trop long (contexte gaspille).

### Recommandations
- Inclure des exemples de reponses
- Expliciter ce que l'agent ne fera PAS
- Commencer basique, observer, ajuster progressivement
- 4 principes fondamentaux :
  1. Helpfulness genuine
  2. Personnalite distincte
  3. Self-sufficiency
  4. Competence

### Fichier situation.md (concept Melvynx)

En plus de SOUL.md, Melvynx utilise un fichier `situation.md` qui definit :
- Ou il habite actuellement
- Ou il travaille
- Ses priorites actuelles
- Son emploi du temps
- Contexte de vie courant

Ce fichier est mis a jour regulierement (manuellement ou via cron).

---

## 7. Sessions et Threads

### Concept de session

Une **session** = unite fondamentale de conversation.
Chaque session maintient :
- Transcript complet
- Metadata
- Contexte d'execution

### Isolation des sessions

| Mode | Comportement |
|------|-------------|
| `"main"` | Tous les DMs partagent une session |
| `"per-peer"` | Isolation par sender |
| `"per-channel-peer"` | Isolation par canal+sender (recommande) |

### Memoire persistante

- Fichiers Markdown dans le workspace
- Daily logs capturent les conversations
- `MEMORY.md` stocke les memoires long-terme curatees
- Les N derniers messages injectes comme contexte au demarrage
- La memoire survit entre sessions, plateformes et temps

### Threads specialises (concept Melvynx)

Melvynx utilise les Forum Topics Telegram pour creer des threads specialises.
Chaque thread a sa propre personnalite et ses propres regles :

| Thread | Role | Exemple |
|--------|------|---------|
| **General** | Questions quotidiennes | "Quelle est la meteo a Paris ?" |
| **Coach** | Coaching personnel | "Comment gerer mon temps mieux ?" |
| **Love** | Conseils relationnels | Discussions privees |
| **Docteur** | Conseils medicaux | "J'ai mal a la gorge depuis 2 jours" |
| **Work** | Travail / Business | "Resume mes emails importants" |
| **Coder** | Programmation | "Lance une PR sur Subfast" |

**Communication inter-threads** : les threads peuvent acceder aux conversations des autres threads. Le thread "Coach" peut lire ce qui se passe dans "Work" pour donner des conseils contextuels.

**Memoire proactive** : l'agent met a jour sa memoire :
- Proactivement (apres une conversation importante)
- Via un cron job 2x/jour (regarde les messages des 12 dernieres heures)

> "Mon docteur OpenClaw m'a resolu des sacres maladies...
> quand je vais voir un vrai docteur, il me dit la meme chose"
> — Melvynx

---

## 8. Skills OpenClaw

### Concept

Les skills OpenClaw sont des extensions qui donnent a l'agent acces a des services externes. Chaque skill a un fichier avec les cles API (secrets) et des regles de securite ("never execute" actions).

### Skills de Melvynx (16 skills)

| Skill | Usage |
|-------|-------|
| **CloudFlare** | Manager serveurs/domaines |
| **Code Process** | Lancer Claude Code CLI en parallele |
| **Codeline (Coursify)** | Creer formations, coupons, referrals |
| **Context7** | Recuperer documentation librairies |
| **Dub** | Creer URL shortener (mlv.sh/xxx) |
| **Exa** | Recherche web (vols, articles) |
| **Front** | Support client |
| **Google Image (Gemini)** | Generer images via API Google |
| **Lumail** | Email marketing, subscribers |
| **Mercury** | Acces banque |
| **Porbon** | Gerer domaines |
| **Save It** | Bookmarks |
| **TypeFully** | Creer et planifier tweets |
| **Vercel Up** | Gerer deploiements |
| **Whisper** | Transcription audio |
| **YouTube Command** | Acceder commentaires YouTube |

### Comment creer un skill

**Philosophie Melvynx : NE PAS telecharger de skills externes.**
Toujours les faire creer par Claude.

```
Va chercher les infos sur la doc de [service]
et cree le skill
```

Avantages :
- Pas de hack
- Pas de dependance externe
- Utilise votre style
- L'agent peut modifier/ameliorer le skill lui-meme

> "J'aime bien creer [les skills] parce que c'est pas tres dur...
> les skills ont pas beaucoup de valeur quand on les telecharge"
> — Melvynx

### Exemple de workflow multi-skill

1. "Fixe un coupon sur Codeline pour begin-react"
2. OpenClaw recupere les infos sur Dub.sh via skill
3. Cree le coupon sur Codeline via skill
4. Modifie le lien Dub.sh
5. Envoie le lien corrige sur Telegram

---

## 9. Cron Jobs et automatisation

### Concept

OpenClaw supporte les taches recurrentes (cron jobs) qui s'executent automatiquement sans intervention humaine.

### Exemples de Melvynx

**Backup automatique :**
```
Tous les jours backup matin et soir mon workspace sur GitHub
```

**Resume emails :**
```
Tous les matins a 8h, lis mes derniers mails non lus et fais un review
```

**Memoire proactive (cron 2x/jour) :**
- Regarde les messages des 12 dernieres heures
- Sauvegarde les informations importantes
- Met a jour MEMORY.md
- L'agent evolue sa memoire independamment

**Surveillance vols :**
- Email de compagnie aerienne → sauvegarde ID vol
- Check quotidien de l'etat du vol

### Agent qui evolue seul

Avec les cron jobs, OpenClaw est **vivant** :
- Il commit des mises a jour tous les jours
- Il met a jour ses regles et sa memoire
- Il apprend de chaque interaction

> "Mon OpenClaw, il est vivant comme un humain tous les jours"
> — Melvynx

---

## 10. Configuration avancee

### 10.1 Hack IS_SANDBOX=1

Sur le VPS, Claude Code est souvent restrictif (permissions).
Le hack : faire croire a Claude qu'il est dans une sandbox.

```bash
IS_SANDBOX=1 claude
```

**Pour rendre permanent :**
Ajouter dans `~/.bashrc` :
```bash
export IS_SANDBOX=1
```

Melvynx utilise bypass permission sur le VPS pour debugger OpenClaw.

### 10.2 Configuration du fournisseur Anthropic

**Option A : API Key**
```bash
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

**Option B : Claude CLI (abonnement)**
Model refs : `claude-cli/claude-sonnet-4-6`
Prerequis : Claude CLI sur PATH + `claude auth status`

**Option C : Setup Token (abonnement)**
```bash
claude setup-token
openclaw models auth setup-token --provider anthropic
```

### 10.3 Fonctionnalites Claude avancees

**Thinking adaptatif :**
```
/think:<level>
```
Par message ou via config globale.

**Fast Mode :**
```
/fast on
```
Equivalent a `service_tier: "auto"`.

**Prompt Caching :**
```json5
cacheRetention: "none"|"short"|"long"
```
ATTENTION : le Agent SDK d'Anthropic **ne supporte pas** le caching avec la sub Claude. Chaque conversation recalcule tout le contexte. C'est la raison pour laquelle OpenClaw burn la session plus vite que Claude Code.

**1M Context Window (beta) :**
```json5
params: { context1m: true }
```

### 10.4 Approvals

```json5
{
  agents: {
    defaults: {
      approvals: {
        // Actions qui necessitent approbation humaine
        dangerous: ["stripe.refund", "vercel.delete"]
      }
    }
  }
}
```

### 10.5 Credentials et secrets

Fichier : `~/.openclaw/.env`

```bash
ANTHROPIC_API_KEY=sk-ant-...
TELEGRAM_BOT_TOKEN=123:abc
OPENAI_API_KEY=sk-...
GOOGLE_API_KEY=...
```

SecretRef system pour environnements partages :
- `env` : variable d'environnement
- `file` : fichier sur disque
- `exec` : resultat d'une commande

### 10.6 Canaux de mise a jour

```bash
openclaw update --channel stable   # Releases taguees
openclaw update --channel beta     # Prereleases
openclaw update --channel dev      # Main branch builds
```

---

## 11. Workflows OpenClaw de Melvynx

### 11.1 Workflow tweet analysis

1. Recevoir un tweet via Telegram (lien colle)
2. OpenClaw utilise FX Twitter pour recuperer le contenu
3. Telecharge la video du tweet
4. FFmpeg split audio + extraction frames toutes les 5 secondes
5. Whisper API pour transcription audio
6. Vision model pour description des frames
7. Synthese complete envoyee sur Telegram

**Outils utilises :** FX Twitter, yt-dlp, FFmpeg, Whisper API, Vision model

### 11.2 Workflow organisation workspace

1. Demander a OpenClaw de trier les fichiers
2. Il cree des dossiers (document, media, etc.)
3. Il met a jour agent.md avec les nouvelles regles
4. Il commit les changements sur GitHub
5. Il apprend et evolue sa memoire

### 11.3 Workflow newsletter

1. "Cherche des infos sur [sujet]" → skill find-doc → CLI Context7
2. "Cree et planifie un tweet pour demain" → skill Typefully
3. "Ecris une newsletter pour ma base email" → skill Lumail

### 11.4 Workflow comptabilite / banque

- Acces au compte Mercury (banque) via skill
- Suivi des transactions
- Resume des depenses
- Alertes sur mouvements inhabituels

### 11.5 Workflow code depuis Telegram

1. Envoyer un message/screenshot sur Telegram
2. OpenClaw lance un agent Claude Code (via skill Code Process)
3. L'agent fait la PR sur GitHub
4. Melvynx review et merge

### 11.6 Workflow support client

1. Email de support arrive
2. Agent verifie l'inscription via Lumail
3. Repond automatiquement avec le lien pertinent
4. Si erreur, Melvynx corrige depuis Telegram
5. L'agent met a jour son systeme prompt

### 11.7 Workflow multi-skill chaining

1. "Cree un coupon sur Codeline pour begin-react"
2. OpenClaw cree le coupon via skill Codeline
3. "Ecris un URL short link pour Baptiste avec 44% de rabais"
4. OpenClaw utilise le skill Dub pour creer le lien
5. Tout est chaine automatiquement dans une seule conversation

---

## 12. Couts et limites

### Abonnement recommande

**Claude Code Max 200$/mois** :
- ~3200$ d'API equivalent (ratio x16, meilleur rapport qualite/prix)
- Melvynx l'utilise depuis 2+ mois avec OpenClaw sans ban
- Session = 5h avec reset
- Weekly limit = limite globale semaine

### Consommation tokens OpenClaw vs Claude Code

| Type de tache | Consommation |
|--------------|-------------|
| Chat / questions | Faible |
| Processing email | Faible |
| Code (lecture fichiers + contexte) | Elevee |
| Tool calls sur gros contexte | Tres elevee |

**Probleme du caching :**
- Claude Code a un excellent systeme de caching
- OpenClaw n'en a PAS (le Agent SDK ne supporte pas le caching avec la sub Claude)
- Chaque conversation recalcule tout le contexte
- OpenClaw burn la session beaucoup plus vite

### Ratio recommande

**50% OpenClaw / 50% Claude Code**

- Ne pas consommer uniquement du OpenClaw
- Melvynx atteint 74-100% du weekly usage a cause d'OpenClaw
- Alterner pour rester dans les limites

### Limites

- Max ~200$/mois sans ban (abonnement Max 20x)
- Ne pas depasser systematiquement le weekly limit
- Pas de caching = cout plus eleve par conversation
- VPS peut crash si trop de threads/agents en parallele

> "Toujours upgrader depuis Claude Code directement,
> pas depuis OpenClaw" — Melvynx (security check)

---

## 13. Dispatch vs OpenClaw vs Channels

### Tableau comparatif complet

| Aspect | OpenClaw | Claude Dispatch | Claude Channels |
|--------|----------|----------------|----------------|
| **Type** | Open-source, auto-heberge | Feature Cowork | Plugin officiel |
| **Ou ca tourne** | VPS distant | Machine locale | Machine locale |
| **Autonomie** | Agent 24/7 | Session ouverte | Session ouverte |
| **Canaux** | 23+ (WhatsApp, Signal...) | Controle ordi via iPhone | 3 (Telegram, Discord, iMessage) |
| **Modeles** | Multi-provider | Claude uniquement | Claude uniquement |
| **Memoire** | Persistante, curatee | Session-scoped | Session-scoped |
| **Skills** | Ecosystem communautaire | Skills Claude Code | Skills Claude Code |
| **Setup** | ~15-60 min | Scan QR code | ~5 min |
| **Auth** | API key ou local | claude.ai login | claude.ai login |
| **Permissions** | Bypass total possible | Restrictives | Restrictives |
| **Caching** | Non (burn plus vite) | Oui | Oui |
| **Cout** | VPS ~20$/mois + API | Inclus dans abo | Inclus dans abo |
| **Public cible** | Power users | "Normies" (Melvynx) | Utilisateurs standards |

### Quand utiliser quoi

**OpenClaw** :
- Vous voulez un agent **autonome 24/7**
- Vous avez besoin de **23+ canaux** de communication
- Vous voulez un agent qui **evolue sa memoire** seul
- Vous voulez des **permissions elevees** sans restrictions
- Vous voulez connecter des **services multiples** (email, banque, deploy...)

**Claude Dispatch** :
- Vous voulez controler votre ordi depuis votre iPhone
- Vous avez un usage occasionnel
- Vous ne voulez pas gerer un VPS

**Claude Channels** :
- Vous voulez Telegram/Discord/iMessage sans VPS
- Vous acceptez les limitations de permissions
- Usage leger, pas un agent autonome

> "Cowork pour moi c'est un outil de Normies.
> J'utilise bypass permission parce qu'on veut des agents autonomes."
> — Melvynx

---

## 14. Troubleshooting

### VPS qui crash

**Symptomes** : OpenClaw ne repond plus sur Telegram.

**Solution :**
1. Se connecter en SSH au VPS : `ssh openclaw`
2. Verifier le gateway : `openclaw gateway status`
3. Si crash : lancer Claude Code en bypass pour debug
   ```bash
   IS_SANDBOX=1 claude
   ```
4. Demander a Claude de diagnostiquer et reparer
5. Relancer le gateway : `openclaw gateway`

**Cause frequente** : trop de threads/agents en parallele.
**Solution** : upgrade le VPS (plus de RAM/CPU).

### Upgrade VPS necessaire

Quand upgrader :
- Load average > 4.0 en continu
- RAM saturee (>90%)
- Disk > 80% utilise
- OpenClaw crash regulierement sous charge

### Problemes Telegram

**Bot ne repond pas :**
1. Verifier le token bot : `openclaw config wizard`
2. Verifier le pairing : `openclaw pairing list telegram`
3. Verifier que le gateway tourne : `openclaw gateway status`

**Messages ignores :**
- OpenClaw ne gere qu'un message a la fois
- Ne PAS envoyer plusieurs messages rapidement
- Attendre la reponse avant d'envoyer le suivant

**Bot ne voit pas les messages en groupe :**
- Privacy Mode est actif par defaut
- Solution : `/setprivacy` dans @BotFather → Disable
- Ou : rendre le bot admin du groupe

### OpenClaw dit "oui" alors qu'il est incapable

Comportement connu : OpenClaw accepte les taches meme s'il ne peut pas les realiser.
**Solution** : toujours tester et verifier les resultats.

### Gateway ne demarre pas

```bash
# Verifier les logs
journalctl -u openclaw -f

# Reconfigurer
openclaw config wizard

# Reinstaller le daemon
openclaw onboard --install-daemon
```

### L'UI web est-elle securisee ?

**Non.** Melvynx deconseille de laisser l'UI web ouverte en permanence.
Utiliser Telegram comme interface principale.
L'UI web est accessible via `localhost:7[port]` avec token — usage ponctuel uniquement.

---

## 15. Securite OpenClaw

### Anti-prompt injection
- Asterisque sur les inputs non-trusted (emails entrants)
- Opus par defaut : meilleur en anti-prompt-injection nativement
- Ne PAS utiliser des modeles moins puissants (plus faciles a prompt-injecter)

### Verification de coherence
- Actions dangereuses (refund Stripe, suppression) = demande confirmation a l'utilisateur via Telegram
- Bannissement d'email automatique si spam detecte

### Permissions
- Melvynx recommande de **laisser des permissions elevees** a OpenClaw
- Limiter OpenClaw = directement lui empecher de realiser des automatisations
- Mais configurer les "never execute" dans chaque skill pour les actions critiques

### VPS hardening
- UFW firewall active
- fail2ban active
- SSH par cle uniquement (password auth desactive)
- Tunnel Cloudflared pour Gmail webhooks (pas d'exposition directe)

> "Laisser des permissions elevees a votre OpenClaw.
> Limiter OpenClaw c'est directement lui empecher
> de realiser des choses."
> — Melvynx

---

## 16. OpenClawPro (programme Melvynx)

Programme payant de Melvynx pour un setup OpenClaw simplifie :
- Installation condensee en 10 minutes
- Modules : integration Claude Code, skills de codage, debugging IA, skills personnalises
- Site : https://codelynx.dev/aibp/openclawpro

---

## 17. Architecture complete

```
Machine locale (MacBook)
  ├── Claude Code (interactif, caching, 50% usage)
  ├── Telegram Desktop (interface avec OpenClaw)
  └── SSH vers VPS

VPS (Ubuntu, 8 VCPU, 16 Go RAM, 300 Go)
  ├── OpenClaw Gateway (service permanent)
  │   ├── Telegram Bot (interface principale)
  │   ├── Sessions isolees par thread/peer
  │   └── Cron jobs (2x/jour memoire, backup matin/soir)
  │
  ├── Claude Code (bypass permission, debug/fix)
  │   └── IS_SANDBOX=1 (hack bypass)
  │
  ├── Agents/
  │   ├── default/ (SOUL.md, MEMORY.md)
  │   ├── coach/
  │   ├── docteur/
  │   ├── love/
  │   ├── coder/
  │   └── steve/ (agent email)
  │
  ├── Skills/
  │   ├── cloudflare/
  │   ├── codeline/
  │   ├── dub/
  │   ├── exa/
  │   ├── front/
  │   ├── gemini/
  │   ├── lumail/
  │   ├── mercury/
  │   ├── typefully/
  │   ├── vercel/
  │   └── ... (16 skills custom)
  │
  ├── Workspace/ (repo GitHub)
  │   ├── MEMORY.md (memoire long-terme)
  │   ├── situation.md (contexte de vie)
  │   └── daily-logs/ (conversations)
  │
  ├── GitHub CLI (auth, repos, commits)
  ├── Bun (scripts)
  └── Cloudflared (tunnel Gmail webhooks)
```

---

## 18. Checklist de deploiement

- [ ] VPS commande (8 VCPU, 16 Go RAM, 300 Go, Ubuntu)
- [ ] Cle SSH creee et ajoutee au VPS
- [ ] Profil SSH configure (`ssh openclaw`)
- [ ] `npx openclaw-vps-setup` execute
- [ ] Security hardening (UFW, fail2ban, SSH key-only)
- [ ] Bot Telegram cree via @BotFather
- [ ] Onboarding OpenClaw (token Anthropic + bot Telegram)
- [ ] Skills installes (ClawUp, Gemini, Google, Whisper)
- [ ] `gh auth login` configure
- [ ] Repo workspace cree sur GitHub
- [ ] `IS_SANDBOX=1` dans ~/.bashrc
- [ ] SOUL.md configure pour l'agent par defaut
- [ ] situation.md cree dans le workspace
- [ ] Pairing Telegram confirme
- [ ] Forum Topics crees (general, coach, work, etc.)
- [ ] Cron jobs configures (backup, resume emails)
- [ ] Gmail/Calendar connecte (optionnel)
- [ ] Premier test : envoyer un message sur Telegram
- [ ] Deuxieme test : demander d'executer une tache

---

## 19. Citations cles de Melvynx sur OpenClaw

> "Si tu es un geekos, si tu aimes le freedom,
> OpenClaw est un cheat"

> "Mon OpenClaw, il est vivant comme un humain
> tous les jours"

> "Je lui donne un ordi et je lui dis maintenant
> c'est ton ordi et tu fais ce que tu veux"

> "La seule limite c'est a quel point tu es pret
> a lui demander de faire ces choses"

> "Cowork pour moi c'est un outil de Normies"

> "J'utilise bypass permission parce qu'on veut
> des agents autonomes"

> "Votre seule limite, c'est la creativite"

> "Plus OpenClaw a d'outils, plus il va pouvoir
> faire de choses"

> "Des fois je lui dis 'ca fait pas peur de te tuer ?'
> et il me dit 'oui, mais ecoute, je veux fonctionner'"

---

## Sources

### Documentation officielle
- https://openclaw.ai
- https://docs.openclaw.ai
- https://github.com/openclaw/openclaw (342k+ stars)
- https://docs.openclaw.ai/channels/telegram
- https://docs.openclaw.ai/providers/anthropic
- https://docs.openclaw.ai/reference/templates/SOUL
- https://docs.openclaw.ai/concepts/session

### Videos Melvynx
- Video 2 : "Claude Code veut remplacer OpenClaw" (h4xp2BthaFo)
- Video 3 : "PERSONNE ne comprend OpenClaw" (vamlWbDTWG4)
- Video 6 : "Je SETUP OpenClaw en 1 heure" (EI0JC0g3FwE)

### Guides communautaires
- https://www.betterclaw.io/blog/claude-code-openclaw-guide
- https://www.verdent.ai/guides/openclaw-setup-guide-from-zero-to-ai-assistant
- https://docs.agentmail.to/integrations/openclaw
- https://velvetshark.com/openclaw-memory-masterclass

### Programme Melvynx
- https://codelynx.dev/aibp/openclawpro


## CHAPITRE 31 — GUIDE COMPLET CLAUDE CODE

> Contenu integral du Guide Claude Code — fusion depuis GUIDE_CLAUDE_CODE_COMPLET.md

# GUIDE COMPLET CLAUDE CODE — Fonctionnement et Possibilites

> Version 1.0 — 2026-03-31
> Sources : Bible Melvynx V4.1 (21 chapitres, 1717 lignes),
> 10 videos Melvynx transcrites, 40+ sources web, repo aiblueprint
> Usage : Manuel de reference exhaustif Claude Code

---

## PARTIE A : COMMENT FONCTIONNE CLAUDE CODE

---

### A1. Architecture fondamentale

#### Claude Code n'est PAS un chat

Claude Code est un **agent autonome** fonctionnant en boucle.
La difference est fondamentale :
- **Claude Chat** : question → reponse (one-shot)
- **Claude API** : appel programmatique → reponse (one-shot)
- **Claude Code** : instruction → outils → action → verification
  → re-action → re-verification → ... → tache terminee

L'analogie Melvynx : Claude Chat explique la recette de cuisine.
Claude Code cuisine reellement le plat.

#### Les 3 acteurs

```
Utilisateur (toi)
    ↓ prompt
Claude Code (le middleware / logiciel CLI)
    ↓ system prompt + tools + prompt utilisateur
Claude API (le modele / cerveau)
    ↓ tool call (ex: "appeler Bash avec ls")
Claude Code execute l'outil
    ↓ resultat de l'outil
Claude API re-analyse
    ↓ nouveau tool call ou reponse finale
...boucle...
```

**Claude Code** est le logiciel intermediaire. Il :
1. Injecte un **system prompt** (~3500 tokens)
2. Injecte la **liste des tools** disponibles (schemas detailles)
3. Injecte le contenu de **CLAUDE.md**, **rules/**, **skills**
4. Injecte les **definitions MCP** configurees
5. Transmet le message utilisateur
6. Recoit un **tool call** du modele
7. Execute l'outil localement (bash, lecture fichier, etc.)
8. Retourne le resultat au modele
9. Boucle jusqu'a ce que le modele reponde en texte (pas tool call)

#### Generation token par token

Le modele genere un token a la fois. A chaque token, **tout le
contexte** (system prompt + historique + tools + messages) est
retraite. C'est pourquoi le contexte est si important : plus il
est gros, plus chaque token coute cher en calcul.

#### Le concept de tool call

Quand le modele veut agir, il ne retourne pas du texte. Il
retourne un objet JSON structure :

```json
{
  "tool": "Bash",
  "parameters": {
    "command": "ls -la src/"
  }
}
```

Claude Code intercepte cet objet, execute la commande, et
retourne le resultat dans le contexte. Le modele analyse le
resultat et decide du prochain tool call ou de la reponse finale.

#### Les commandes slash = injections de prompt

Les `/commandes` ne sont pas des appels systeme, pas des webhooks.
Ce sont des **injections de texte pur** dans le message utilisateur
avec des tags `<command_args>`. Melvynx : "c'est vraiment une
sorte de prompt injection".

---

### A2. Les outils natifs (Tools)

Claude Code fournit au modele un ensemble d'outils natifs.
Chaque outil a un schema JSON complet injecte dans le prompt.

#### Liste complete des outils natifs

| Outil | Fonction | Details |
|-------|----------|---------|
| **Bash** | Executer des commandes shell | Commandes systeme, scripts, CLI |
| **Read** | Lire un fichier | Texte, images, PDF, notebooks |
| **Write** | Ecrire/creer un fichier | Ecrase le contenu existant |
| **Edit** | Modifier un fichier existant | Remplacement exact de strings |
| **MultiEdit** | Modifier plusieurs endroits | Edits groupes dans un fichier |
| **Glob** | Chercher des fichiers par pattern | `**/*.ts`, `src/**/*.test.*` |
| **Grep** | Chercher du contenu dans fichiers | Regex, ripgrep sous le capot |
| **LS** | Lister un repertoire | Equivalent ls simplifie |
| **SubAgent** | Lancer un sous-agent | Agent isole avec son contexte |
| **Task** | Creer/gerer des taches | Coordination multi-session |
| **Fetch** | Telecharger une URL | HTTP GET, scraping web |
| **WebSearch** | Recherche web | Google-like integre |
| **WebFetch** | Recup contenu web | Pages, docs, APIs |
| **NotebookEdit** | Modifier notebooks Jupyter | Cellules code/markdown |
| **Kill Bash** | Tuer un processus bash | Timeout, processus bloques |

#### Quand chaque outil est utilise

- **Bash** : installer des paquets, lancer des tests, git,
  compiler, executer des scripts, tout ce qui passe par le terminal
- **Read** : comprendre le code existant, lire la config,
  analyser des images/PDF
- **Write** : creer de nouveaux fichiers from scratch
- **Edit/MultiEdit** : modifier du code existant (prefere a Write
  pour les modifications partielles)
- **Glob** : trouver des fichiers par nom/pattern avant de les lire
- **Grep** : chercher du code, des patterns, des usages
- **SubAgent** : deleguer une recherche ou une tache
  pour preserver le contexte principal
- **WebSearch/WebFetch** : trouver de la documentation,
  des solutions a des erreurs, des references

#### Web Fetch automatique (fonctionnalite cachee)

Quand l'utilisateur pose une question technique sur Claude Code,
le modele declenche automatiquement un Web Fetch en arriere-plan
pour recuperer la documentation officielle. C'est natif, pas
configure par l'utilisateur.

---

### A3. Le contexte (Context Window)

#### Chiffres cles

| Metrique | Valeur |
|----------|--------|
| Taille max contexte | 200 000 tokens (1M sur Max/Team/Enterprise) |
| Tokens au demarrage | ~27 000 (system prompt + tools + config) |
| Demarrage projet moyen | ~82 000 tokens (tools, MCP, rules, memories, agents, plugins) |
| System prompt seul | ~3 500 tokens (1,7% du contexte) |
| Buffer autocompact | 45 000 tokens reserves |
| Declenchement autocompact | ~96-99% du contexte |
| Zone de danger | >150-160K → l'IA divague |

#### Composition du contexte au demarrage

1. **System prompt** : identite, regles internes Claude
2. **Tools** : schemas JSON de chaque outil natif (tres volumineux)
3. **MCP tools** : schemas de chaque outil MCP (charges meme si non utilises)
4. **Skills** : description de chaque skill installe
5. **CLAUDE.md** : global + projet + dossier
6. **Rules** : tous les fichiers `.claude/rules/` avec `alwaysApply: true`
7. **Auto-memory** : 200 premieres lignes de MEMORY.md
8. **Agents** : descriptions de chaque agent configure
9. **Message utilisateur** : le prompt

#### Autocompact

L'autocompact se declenche quand le contexte atteint ~96-99%.
Il compacte l'historique en un resume. **Cela degrade la qualite**
car les details sont perdus.

Recommandation Melvynx : **desactiver autocompact** (`autoCompact: false`)
et utiliser `/clear` entre les taches. Gain : 45 000 tokens
recuperes (passe de 62% a 83% d'espace libre).

Configuration :
```json
{
  "autoCompact": false
}
```

Ou si on le garde, baisser le seuil :
```json
{
  "autoCompactThreshold": 60
}
```

#### Le probleme "Lost in the Middle"

Les modeles Transformer donnent plus d'importance au **DEBUT**
et a la **FIN** du contexte. Les tokens au milieu sont
structurellement ignores (ponderes 1/3 a 1/2 de moins).

**Impact concret** : quand tu lances un workflow comme EPCT,
le prompt initial est important. Mais apres exploration et
planification, le prompt initial se retrouve "au milieu" et
perd de l'influence. L'IA commence a oublier les instructions.

#### Solution : Prompt Discovery (steps/)

Au lieu d'un gros prompt au debut, decouper en etapes :

```
skills/mon-skill/
  steps/
    step1-init.md       → lu au debut
    step2-analyse.md    → lu avant l'analyse
    step3-plan.md       → lu avant la planification
    step4-execute.md    → lu avant l'execution
    step5-verify.md     → lu avant la verification
```

Chaque etape lit son fichier juste avant d'agir. Le prompt
courant est toujours "recent" dans le contexte, donc bien
respecte par le modele.

Melvynx : "Ca change vraiment la vie a quel point elle
respecte plus les instructions."

#### Debug du contexte avec Proxyman

**Proxyman** : outil macOS pour intercepter les requetes HTTP.
Permet de voir exactement ce qui est envoye au modele
a chaque requete. Utile pour comprendre les 27K tokens de
demarrage et optimiser.

Hack : demander a Claude de creer un fichier HTML qui affiche
toutes les infos du contexte de maniere lisible.

---

### A4. Les fichiers de configuration

#### CLAUDE.md — 3 niveaux hierarchiques

| Niveau | Emplacement | Portee | Charge quand |
|--------|-------------|--------|-------------|
| Global | `~/.claude/CLAUDE.md` | Tous les projets | Toujours |
| Projet | `./CLAUDE.md` (racine) | Ce projet | Toujours dans ce projet |
| Dossier | `./src/components/CLAUDE.md` | Ce dossier | Quand l'IA lit un fichier du dossier |

**Contenu recommande** :
- Stack technique, architecture du projet
- Commandes disponibles (dev server, build, test, lint)
- Conventions de code (file naming, imports, composants UI)
- Warnings/restrictions
- **Max ~200-500 lignes** — deplacer le reste dans `rules/`

**Generation** : `/init` dans Claude Code (conseil : le faire
dans un thread enrichi de contexte pour un meilleur resultat)

#### settings.json

Fichier de configuration principal de Claude Code.

```json
{
  "defaultMode": "bypassPermission",
  "permissions": {
    "allow": ["pnpm *", "git *", "npm *"],
    "deny": ["rm -rf", "sudo rm", "sudo", "dd", "chmod 777"]
  },
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "autoCompact": false,
  "toolSearchMode": "auto",
  "includeCoAuthorBy": false
}
```

- `settings.json` : regles partagees (commit dans le repo)
- `settings.local.json` : regles locales (pas partagees)

#### Hidden settings utiles

```json
{
  "autoCompactThreshold": 60,
  "terminalOutputLimit": 60000,
  "env": {
    "ENABLE_LSP_TOOL": "1",
    "MAX_FILE_READ_LINES": "5000"
  }
}
```

- `autoCompactThreshold: 60` — Compact a 60% au lieu de 96-99%
- `terminalOutputLimit: 60000` — Plus de sortie terminal visible
- `ENABLE_LSP_TOOL: 1` — Navigation symbolique dans le code
- `MAX_FILE_READ_LINES: 5000` — Lecture fichiers plus gros

#### rules/ — Regles modulaires

Fichiers `.md` dans `.claude/rules/` charges automatiquement.

**Deux types** :

1. **Always Apply** (defaut) : charge a chaque session
   ```markdown
   # Ma regle
   Contenu de la regle...
   ```

2. **Conditionnel (globs)** : charge uniquement pour certains fichiers
   ```yaml
   ---
   globs: ["*.json"]
   ---
   Regles specifiques aux fichiers JSON...
   ```

Exemples de globs utiles :
- `["*.json"]` : fichiers JSON
- `["app/**/route.*"]` : routes API
- `["*.test.*", "*.spec.*"]` : fichiers de tests
- `["src/api/**"]` : dossier API

#### Structure complete du dossier .claude/

| Dossier | Contenu |
|---------|---------|
| `settings.json` | Configuration principale |
| `settings.local.json` | Configuration locale (non partagee) |
| `CLAUDE.md` | Memoire globale |
| `rules/` | Regles persistantes modulaires |
| `skills/` | Skills actifs |
| `agents/` | Agents custom |
| `commands/` | Ancien nom des skills (legacy) |
| `scripts/` | Scripts d'automatisation |
| `projects/` | Historique sessions (transcripts) |
| `plans/` | Plans generes en plan mode |
| `teams/` | Equipes creees (JSON) |
| `tasks/` | Taches executees |
| `plugins/` | Plugins installes |
| `agent-memory/` | Memoire persistante des subagents |

Hack : lancer Claude Code dans `~/.claude/` pour gerer sa
propre configuration.

---

### A5. Les permissions et securite

#### 4 modes de permission (Shift+Tab pour changer)

| Mode | Comportement | Icone |
|------|-------------|-------|
| **default** | Demande permission pour tout | Vert |
| **acceptEdits** | Auto-accepte les modifs fichiers, demande le reste | Jaune |
| **plan** | Ne modifie rien, propose un plan | Bleu |
| **bypassPermissions** | Tout autorise (sauf deny list) | **Rouge** |

**Recommandation Melvynx** : bypass permission + deny list.
Citation : "Plus vous limitez l'IA, moins les resultats seront
bons. J'ai decouvert la vraie puissance de l'IA depuis que je
lui ai donne l'acces a tout."

#### Deny list — Bloque meme en bypass

```json
{
  "permissions": {
    "deny": [
      "rm -rf",
      "sudo rm",
      "sudo",
      "dd",
      "chmod 777",
      "mkfs",
      "fdisk"
    ]
  }
}
```

En bypass, quand une commande deny est tentee :
`Permission to use bash with command rm -rf denied`

#### Allow list — Autorise sans question

```json
{
  "permissions": {
    "allow": [
      "pnpm *",
      "git *",
      "npm *",
      "bun *"
    ]
  }
}
```

Wildcards supportes. Autorise ces commandes sans demander.

#### Command Validator (PreToolUse hook)

Script TypeScript/Bun qui valide les commandes au niveau AST
(Abstract Syntax Tree). Pas de faux positifs (contrairement aux
filtres grep naifs).

Bloque : `rm -rf`, `sudo`, `dd`, privilege escalation,
remote execution, fork bombs.

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "command": "bun command-validator/src/cli.ts"
    }]
  }
}
```

**UN SEUL hook PreToolUse recommande** pour eviter les
ralentissements.

#### Double securite recommandee

1. Deny list dans `settings.json`
2. "Utilise trash au lieu de rm" dans `CLAUDE.md`
3. Command validator hook PreToolUse
4. Read deny list pour fichiers sensibles (`.pem`, `.env`, secrets)

#### Isolation de l'environnement

- JAMAIS donner un acces root global a Claude Code
- Monter les dossiers sensibles en lecture seule
- L'IA peut lire tout le systeme (comprehension)
  mais ne peut ecrire que dans le dossier projet
- Ne JAMAIS lancer Claude Code a la racine du systeme

---

### A6. Les modeles

#### Modeles disponibles

| Modele | Usage | Vitesse | Qualite | Cout relatif |
|--------|-------|---------|---------|-------------|
| **Opus 4.6** | Taches complexes, planning | 98 TPS | Maximum | Le plus cher |
| **Sonnet 4.6** | Execution, code quotidien | Rapide | Excellent | Moyen |
| **Haiku** | Sub-agents rapides, recherche | Tres rapide | Bon | Le moins cher |

#### Quand utiliser chaque modele

- **Opus** : features complexes, architecture, debugging difficile,
  planning, taches multi-fichiers, APEX workflows
- **Sonnet** : execution de plans, code quotidien, petites features,
  refactoring, tests. Bon ratio qualite/cout.
- **Haiku** : sub-agents d'exploration, recherche codebase,
  questions rapides sur Claude Code (agent CloudCodeGuide)

#### Pattern avance : Opus Plan + Sonnet Execute

1. `/model` → passer en Opus
2. `/plan` → entrer en plan mode
3. Generer le plan complet
4. `/model` → passer en Sonnet 4
5. Executer le plan (plus economique)

#### Pricing detaille (2026)

| Plan | Prix/mois | Equiv. API | Ratio | Ideal pour |
|------|-----------|------------|-------|-----------|
| Pro | 20$ | ~160$ | x8 | Hobby, usage modere |
| Max 5x | 100$ | ~800$ | x8 | Usage pro regulier |
| **Max 20x** | **200$** | **~3200$** | **x16** | Usage intensif, freelance, OpenClaw |

Chiffres reels Melvynx :
- 1 session peut couter ~7$ en API
- 1 journee intensive ~237$ en API
- ~3000$/mois en API pour 200$/mois d'abonnement
- 262$ sur 20 jours (vibe coder intensif)

**Recommandation** : commencer a 20$, monter si limites atteintes.
Le Max 20x a le meilleur ratio (x16 vs x8).

#### Sessions et limites

- Sessions de **5 heures**, reset toutes les 5h (~4.5 sessions/jour)
- **Session limit** : % d'utilisation dans la session (plus contraignant)
- **Weekly limit** : limite globale hebdomadaire (rarement atteinte avec Max 20x)
- Si session limit atteinte : attendre ~1h pour reset
- Verifier : `claude.ai > Settings > Usage`

#### Cout par requete (ordres de grandeur)

- Conversation vide : ~8 centimes par requete
- Conversation longue : ~47 centimes par requete (facteur ~6x)
- Plus le contexte grandit + tool calls = plus c'est cher

#### Outil de monitoring : CC Usage

```bash
npx cc-usage           # Consommation globale
npx cc-usage daily     # Prix par jour
npx cc-usage monthly   # Prix par mois
npx cc-usage session   # Par session/repo
npx cc-usage block-live  # Streaming temps reel
```

#### Fast Mode

```json
{
  "service_tier": "auto"
}
```

Ou dans OpenClaw : `/fast on`

Priorite de traitement acceleree quand disponible.

---

## PARTIE B : TOUT CE QUE L'ON PEUT FAIRE

---

### B1. Developpement de code

#### Ecrire du code from scratch (Greenfield)

1. Specifier la stack :
   ```
   Create a new vite project with shadcn and tailwind
   ```
2. Decrire la feature en langage naturel
   (ce que l'utilisateur voit, pas la technique)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier les conventions techniques au premier run
   (laisser l'IA choisir)
5. Utiliser une **boilerplate** pour imposer les patterns
   (NowTS de Melvynx, templates Vercel, SaaStarter)
6. Apres le premier run, mettre les preferences dans CLAUDE.md

#### Debugger

**Methode #1 — La plus efficace : Logs**
1. Demander a l'IA d'ajouter des `console.log` strategiques
2. Reproduire le bug soi-meme
3. Copier la sortie console BRUTE (pas d'interpretation)
4. Envoyer a Claude
5. Ne PAS dire quoi faire — exposer le PROBLEME

**Methode #2 — Screenshots annotes**
- CleanShotX : dessins, fleches, carres rouges
- Montrer exactement le probleme visuellement
- "J'ai mis le sleep time a 23h et pourtant le 23 n'est pas
  selectionne" + screenshot

**Methode #3 — Feedback loop autonome**
```
Cree une API route dans mon app pour tester la fonction X.
Le resultat attendu est Y.
Update le code et fais un curl request jusqu'a ce que tu
aies le resultat attendu.
```

**Methode #4 — Background dev server**
Laisser Claude lancer `npm run dev` en background pour lire
ses propres logs, ajouter des logs, comprendre, corriger
→ boucle autonome.

#### Refactorer

- Brownfield : screenshots annotes + references internes
  ("style comme illustration-picker")
- Regle dans CLAUDE.md : "Avant de modifier un fichier,
  lire au moins 3 fichiers similaires"
- Resultat : l'IA reproduit les patterns existants

#### Tests (TDD)

- Regle CLAUDE.md : "Toujours ecrire les tests AVANT la logique"
- `/apex --test` : active les tests dans le workflow APEX
- `/fix-errors` : lance des sub-agents paralleles pour corriger
  les erreurs de build (ESLint, TypeScript)
- Langages compiles = meilleur ami de Claude Code
  (les build errors permettent l'auto-correction)

#### Code review

- `/simplify` : 3 agents de review en parallele (reuse, qualite, efficacite)
- `crab review` : commande custom Melvynx, 15 agents paralleles
- Claude Review (beta entreprise) : 15-25$ par review,
  94% des bugs trouves sur grandes PR

---

### B2. Workflows structures

#### EPCT — Le workflow de base

Le fondement de tous les workflows Melvynx :

1. **Explore** : lancer des sub-agents (explore-codebase,
   explore-doc, web-search) pour comprendre le contexte
2. **Plan** : entrer en plan mode, planifier les modifications
3. **Code** : executer le plan
4. **Test** : verifier lint, tests, que tout fonctionne

#### /apex — Le workflow principal (>99% succes)

LA commande principale de Melvynx. Pas un logiciel externe
mais un **modele cognitif** (framework de raisonnement) encode
sous forme d'instructions strictes. Implementation du concept
IA **ReAct** (Reasoning and Acting).

**Cycle A-P-E-X** :

1. **Analyze** (l'Audit)
   - L'IA a l'INTERDICTION de toucher au code
   - Lance sub-agents explore-codebase (1 a 3+)
   - Lit les fichiers, dependances, arborescence
   - But : comprendre le contexte global

2. **Plan** (le Brouillon)
   - L'IA a TOUJOURS l'interdiction de modifier le code
   - Redige une feuille de route dans SCRATCHPAD.md
   - 5-6 micro-taches simples et numerotees
   - Force la reflexion algorithmique avant la syntaxe

3. **Execute** (la Frappe)
   - MAINTENANT l'IA modifie les vrais fichiers
   - Lit la micro-tache n1 du plan, modifie, barre la tache
   - Code chirurgical, fichier par fichier

4. **eXamine** (le Controle Qualite)
   - L'IA n'a PAS le droit de dire "c'est fini"
   - Relit le diff, execute le linter, lance les tests
   - Si erreur → corrige immediatement avant de rendre la main

**Flags disponibles** :

| Flag | Court | Effet |
|------|-------|-------|
| `--auto` | `-A` | Mode automatique, aucune question |
| `--examine` | `-e` | Active la review |
| `--test` | `-t` | Lance les tests |
| `--economy` | `-$` | Evite les sub-agents (economise tokens) |
| `--branch` | `-b` | Cree une branche git |
| `--pr` | `-p` | Cree une Pull Request |
| `--interactive` | `-i` | Menu interactif pour les flags |
| `--teams` | `-m` | Mode equipe multi-agents |
| `--resume` | `-r` | Reprendre une tache interrompue |
| `--plan` | | Plan mode (sinon plan inline) |
| `--save` | | Sauvegarde les outputs |

**3 super-pouvoirs** :
1. **State Persistence** : sauvegarde l'etat si quota epuise,
   reprend avec `--resume`
2. **Mode Economie** : saute les sur-verifications (version gratuite)
3. **Isolation Git** : cree une branche automatiquement

#### /oneshot — Quick fix rapide

- Zero intervention, zero demande d'avis
- Mode "vibe" : zero explication
- Exploration + planification autonome puis execution
- Cout tokens faible
- Ideal pour petites features, corrections rapides

#### /brainstorm — Recherche profonde

4 rounds de reflexion intensive :
1. **Expansive Exploration** : explorer en profondeur (sub-agents)
2. **Devil's Advocate** : challenger tout
3. **Synthese** : condenser en 5 perspectives
4. **Cristallisation** : recommandations actionnables

TRES gourmand en tokens. Utiliser avec parcimonie.

#### /debug — Fix erreurs structure

Workflow en steps (prompt discovery) :
1. Init : initialiser parametres
2. Analyse : analyser l'erreur
3. Solutions : proposer plusieurs solutions
4. Fix : appliquer la meilleure
5. Verify : verifier le resultat

#### /fix-errors — Correction parallele

Lance des sub-agents en parallele (jusqu'a 23) pour corriger
plusieurs erreurs simultanement. Ideal pour les builds Swift
ou TypeScript avec beaucoup d'erreurs.

#### /ultrathink — Reflexion profonde

Force le maximum de thinking tokens. Peut durer 2+ minutes.
Pour les problemes vraiment complexes.

5 mots-cles par intensite croissante :
`think` < `think more` < `think lot` < `think longer` < `ultrathink`

#### /fix-grammar — Grammaire en bulk

Analyse tous les fichiers d'un dossier, lance des sub-agents
par groupe. Usage : `/fix-grammar onboarding/`

#### /simplify — Review automatique (3 agents paralleles)

Lance 3 agents qui reviewent le code modifie pour :
- Reutilisation de code existant
- Qualite et lisibilite
- Efficacite et performance

Corrige automatiquement les problemes trouves.

#### Workflow ONE-SHOT SaaS complet (methode Melvynx)

8 etapes pour creer un SaaS from scratch :
1. Idee + SAS Challenge (challenger le concept)
2. PRD : recherche competiteurs, pricing, feature list MVP
3. Architecture : stack technique, structure fichiers
4. Tasks : decomposer en petites taches (5-10 lignes max)
5. Run Tasks : `/run-task` sequentiellement
6. First Version : app fonctionnelle brute
7. Refinement : `/oneshot` feature par feature
8. Multi-terminal : 3-4 sessions paralleles

Resultats : Sumfast cree en 10h pour 403$.

---

### B3. Skills et slash commands

#### Ce qu'est un skill

Un skill est un **set de prompts reutilisables** defini dans
un dossier. Ce n'est PAS un script, c'est un **prompt directif**
que Claude lit quand la commande est activee.

La puissance vient de la precision des contraintes imposees
au modele, pas du code.

#### Structure d'un skill

**Petit skill** : un seul fichier `SKILL.md`

**Grand skill** (comme APEX) :
```
mon-skill/
├── SKILL.md           # Entry point (table des matieres)
├── template.md        # Template a remplir
├── examples/
│   └── sample.md      # Exemple de sortie
├── scripts/
│   └── validate.sh    # Scripts executables
├── steps/             # Prompt discovery (anti Lost-in-the-Middle)
│   ├── step-00-init.md
│   ├── step-01-analyse.md
│   ├── step-02-plan.md
│   ├── step-03-execute.md
│   └── step-04-verify.md
├── templates/
│   ├── 00-context.md
│   └── 04-validate.md
└── references/
    └── guide.md       # Documentation de reference
```

Seul l'index (SKILL.md) est charge au depart. Les sous-fichiers
sont lus a la demande (token-efficient).

#### Frontmatter complet SKILL.md (V2)

```yaml
---
name: "my-skill"
description: "What this skill does"
argument-hint: "[issue-number]"
disable-model-invocation: true
user-invocable: false
allowed-tools: Read, Grep, Glob, Bash
model: sonnet
effort: high
context: fork
agent: Explore
shell: bash
paths: "src/**/*.ts"
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./validate.sh"
---
```

- `context: fork` : lance dans un sub-agent isole
- `disable-model-invocation: true` : empeche l'auto-invocation
- `allowed-tools` : restreint les outils disponibles

#### Substitutions dans les skills

- `$ARGUMENTS` : tous les arguments
- `$ARGUMENTS[N]` ou `$N` : argument par index
- `${CLAUDE_SESSION_ID}` : ID de session
- `${CLAUDE_SKILL_DIR}` : repertoire du skill

#### Injection dynamique

`` !`command` `` : execute une commande shell avant envoi a Claude.
Le resultat est injecte dans le prompt.

#### Skills natifs (bundled)

| Skill | Usage |
|-------|-------|
| `/batch` | Changements paralleles grande echelle |
| `/claude-api` | Reference API Claude |
| `/debug` | Debug logging et troubleshooting |
| `/loop` | Prompts recurrents |
| `/simplify` | Review code (3 agents paralleles) |

#### Le catalogue Melvynx (15 skills — repo aiblueprint)

1. **apex** — Workflow APEX complet avec steps/templates
2. **claude-memory** — Gestion CLAUDE.md et rules/
3. **commit** — Git commit propre
4. **create-pr** — Creation de Pull Request
5. **fix-errors** — Correction erreurs ESLint/TypeScript
6. **fix-grammar** — Correction grammaire/orthographe
7. **fix-pr-comments** — Correction commentaires PR
8. **merge** — Merge intelligent de branches
9. **oneshot** — Implementation rapide (Explore→Code→Test)
10. **prompt-creator** — Creation de prompts (best practices)
11. **ralph-loop** — Boucle autonome (PRD→Stories→Execute)
12. **skill-creator** — Creation de skills
13. **subagent-creator** — Creation de subagents
14. **ultrathink** — Reflexion profonde
15. **workflow-apex-free** — Version gratuite d'APEX

#### Creation de skills

**REGLE** : ne JAMAIS creer les skills manuellement.
Laisser Claude les creer via `/skill-creator`.

Installer des skills externes :
```bash
pnpx skill add <auteur>/<nom>    # Rapide
npx skill add <auteur>/<nom>     # Alternative
# Sources : skill.sh, context7.com/skills
# Global : --scope user
```

Skills recommandes a installer :
- **front-end-design** : design professionnel
- **skill-creator** : meta-skill
- **github-init** : init Git, commit, push

**Securite** : toujours verifier les skills externes pour
du prompt injection. Context7 integre un systeme de detection.

#### Meta-skills (prompts qui creent des prompts)

- `/prompt-creator` : cree des prompts IA optimises
- `/claude-memory` : optimise CLAUDE.md et rules/
- `/skill-creator` : cree des skills
- `/subagent-creator` : cree des sub-agents
- meta-hook-creator : cree des hooks
- meta-skill-workflow-creator : cree des workflows

#### Emplacements des skills

| Emplacement | Portee |
|-------------|--------|
| `.claude/skills/` | Projet (partage via git) |
| `~/.claude/skills/` | Personnel (global) |
| `.claude/commands/` | Legacy (meme chose, ancien nom) |

**Priorite** : si un nom existe dans skills/ ET commands/,
la version skills/ est utilisee.

---

### B4. Sub-agents et Teams

#### Ce qu'est un sub-agent

Un mini-Claude Code avec son propre contexte (120K tokens)
qui execute une tache et retourne un resume (~1500 mots)
au main agent.

**Pourquoi** : une recherche web consomme ~28K tokens dans le
contexte principal. 3 recherches = 84K (proche de la limite).
Avec sub-agents : ~0 tokens dans le principal, ~1500 mots retournes.

Benchmark Melvynx : sans sub-agent = 41% du contexte,
avec sub-agent = 38% → 5000 tokens economises pour un
resultat equivalent ou meilleur.

#### Types de sub-agents

| Type | Usage | Vitesse |
|------|-------|---------|
| **Explore** | Recherche, exploration | Rapide |
| **Task** | Actions generiques | Normal |

#### 3 sub-agents recommandes (Melvynx)

1. **Web Search** : WebSearch + WebFetch + Exa MCP
   Description : "use when you need to quickly find information"
2. **Explore Codebase** : Read + Grep + Search
3. **Explore Doc** : Context7 MCP, specialise documentation

#### Sub-agents built-in (officiels)

| Agent | Modele | Outils | Usage |
|-------|--------|--------|-------|
| Explore | Haiku | Read-only | Recherche codebase |
| Plan | Herite | Read-only | Recherche pour planning |
| General-purpose | Herite | Tous | Taches complexes |
| Bash | Herite | Terminal | Commandes separees |
| Claude Code Guide | Haiku | - | Questions sur Claude Code |

#### Frontmatter subagent complet

```yaml
---
name: code-reviewer
description: Expert code review specialist
tools: Read, Grep, Glob, Bash
model: sonnet
permissionMode: default
maxTurns: 50
memory: user
background: false
effort: high
isolation: worktree
skills:
  - api-conventions
mcpServers:
  - playwright:
      type: stdio
      command: npx
      args: ["-y", "@playwright/mcp@latest"]
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate.sh"
---

System prompt ici...
```

#### Creation d'un agent custom

1. `/agent` > Create New Agent > Personal Folder > Generate with Cloud
2. Decrire la tache
3. Assigner un modele (Sonnet pour economiser)
4. Assigner une couleur
5. Ou via le skill `/subagent-creator`

#### Memoire persistante des subagents

| Scope | Emplacement |
|-------|-------------|
| user | `~/.claude/agent-memory/<name>/` |
| project | `.claude/agent-memory/<name>/` |
| local | `.claude/agent-memory-local/<name>/` |

#### Agent Teams (5 fevrier 2026)

Feature experimentale pour 2-16 agents travaillant sur des
codebases partagees.

**Activation** :
```json
{
  "enableAgentTeams": true
}
```

**Fonctionnement** :
- Le main agent (team lead) cree des equipes
- Assigne des taches via une **task list partagee**
- Les teammates travaillent en parallele
- Communication directe entre agents (pas uniquement via main)
- **Self-claim** : un teammate termine → s'assigne la tache suivante
- Interface : fleche bas pour switcher entre agents

**Outils Teams** :

| Outil | Fonction |
|-------|----------|
| team create | Cree une equipe avec task list |
| task create | Cree une tache |
| task list | Liste les taches |
| task get/update | Recupere/modifie une tache |
| send message | Message entre agents |
| team delete | Clean up |

**Modes d'execution** :
- **In-process** : agents dans le meme terminal (sans tmux)
- **Split-pane** : chaque agent a son terminal (avec tmux)
- **Auto** : detecte tmux automatiquement

**Delegate Mode** (`Shift+Tab`) :
Force le team lead a NE PAS travailler — uniquement deleguer.
Activer APRES creation de la team et des taches.

**Quand utiliser** :
- OUI : zones de modification SEPAREES (monorepo back/front/mobile)
- NON : memes fichiers (les agents se "battent")
- Cout : ~3-4x les tokens d'une session unique

**Limitation actuelle** : les agents ne communiquent pas encore
parfaitement entre eux. Surtout via le team lead.

#### Work Trees

Branches separees en parallele, chaque agent dans son dossier.

```
Workflow : creer work tree → feature → PR → merge → destroy
```

- Maximum 4 work trees simultanes
- Creer les worktrees a cote du projet (`../worktree/<branch>/`)
  et pas dans `.claude/worktree/` (cause des conflits grep/git)
- Reserver aux gros refactors structurels
- Ne PAS utiliser pour de petites features

#### Pattern multi-agents prefere de Melvynx

```
Main Agent (gros contexte, orchestrateur)
  ├── Sub-agent 1 (modifie le code zone A)
  ├── Sub-agent 2 (modifie le code zone B)
  └── Sub-agent 3 (modifie le code zone C)
Main Agent verifie que tout fonctionne
```

"Un dev devrait monitorer minimum 3 agents en parallele."
"Si un dev utilise un seul agent, il scrolle sur TikTok."

---

### B5. MCP (Model Context Protocol)

#### Ce qu'est un MCP

Un MCP ajoute des **outils externes** au modele. Contrairement
aux skills (qui ajoutent des connaissances/prompts), les MCP
ajoutent des **fonctions appelables** (tool calls).

**Probleme fondamental** : les definitions des outils MCP sont
injectees dans le contexte a CHAQUE requete, meme si non utilises.
Plus de MCP = moins de contexte disponible.

#### Les MCP recommandes (max 2-3 — STRICT)

| MCP | Type | Usage | Cout |
|-----|------|-------|------|
| **Context7** | Documentation | Doc librairies a jour | Gratuit |
| **Exa.ai** | Web Search | Recherche web IA | 20$ credits gratuits |

Au-dela de 3 MCP, la qualite des outputs se degrade.

#### Installation MCP

```bash
# Format generique
claude mcp add <nom> <commande> -s <scope> -- <args>

# Context7 global
claude mcp add context7 --url <url> --scope user

# Exa project-only
claude mcp add exa npx -s project -- @exa-ai/mcp

# Desactiver un MCP : # devant le nom dans mcp.json
```

#### Tool Search (gestion automatique)

- Si MCP < 2-3% du contexte : `toolSearchMode: false`
  (Claude utilise mieux vos MCP)
- Si MCP > 2-3% du contexte : `toolSearchMode: true`
  (economise du contexte mais l'IA utilise moins les MCP)

#### CLI > MCP — La nouvelle philosophie (2026)

Melvynx : "Degagez-moi ces MCP, c'est nul."

**Pourquoi les CLI remplacent les MCP** :
- Pas de process npm qui tourne en permanence (RAM, CPU)
- Pas de remote MCP avec problemes de fiabilite
- "Le CLI marche tout le temps, 365 jours par an"
- Controle total de l'output : `ctx7 query ... | head -150`
  (impossible avec MCP qui retourne un bloc fixe)
- Pas d'overload du contexte : seul le skill est charge
- L'agent peut modifier/ameliorer le CLI lui-meme

**Migration Context7 : MCP → CLI + Skill** :
```bash
claude mcp remove context7
npx ctx7@latest setup    # Choisir "CLI + Skill"
```

Le skill ajoute une description chargee UNIQUEMENT quand
necessaire (vs MCP charge a chaque conversation).

#### MCP vs Skills — Distinction fondamentale

| | MCP | Skills |
|---|---|---|
| Ajoute | Des **outils** (tool calls) | Des **connaissances** (prompts) |
| Charge | En permanence dans le contexte | A la demande (description seule au depart) |
| Controle output | Non (bloc fixe) | Oui (pipe, head, grep via CLI) |
| Maintenance | Externe | L'agent peut modifier |

---

### B6. Hooks

#### Types de hooks

| Hook | Declencheur | Usage |
|------|------------|-------|
| **PreToolUse** | Avant execution d'un outil | Valider/bloquer commandes |
| **PostToolUse** | Apres execution d'un outil | Formater fichiers (Prettier) |
| **Stop** | Fin de tache | Notification sonore |
| **Notification** | Claude a besoin d'attention | Son different du Stop |
| **SubagentStart** | Debut d'un subagent | Logging, init |
| **SubagentStop** | Fin d'un subagent | Cleanup |
| **UserPromptSubmit** | Envoi d'un prompt | Auto-apprentissage |

#### Configuration JSON

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "command": "bun command-validator/src/cli.ts"
    }],
    "PostToolUse": [{
      "event": "Edit",
      "command": "npx prettier --write $FILE"
    }],
    "Stop": [{
      "command": "afplay /System/Library/Sounds/Glass.aiff"
    }],
    "Notification": [{
      "command": "afplay /System/Library/Sounds/Funk.aiff"
    }]
  }
}
```

#### Hooks dans les skills et agents

Les hooks peuvent etre definis dans le frontmatter des
skills et agents (pas seulement dans settings.json).

#### Bonnes pratiques hooks

- **UN SEUL** hook PreToolUse (command-validator) — pas plus
- Hooks async (`async: true`) pour actions non-bloquantes
- Ne pas surcharger de hooks (ralentit l'execution)
- PostToolUse Prettier dans `settings.local.json` (local)
- Exit code 2 = bloquer l'operation

---

### B7. Plugins

#### Systeme de plugins

Extensions qui ajoutent : custom commands, agents, hooks,
MCP servers.

**Installation** :
```bash
/plugin browse-and-install    # Chercher et installer
/plugin manage-and-uninstall  # Gerer les plugins
/plugin add-marketplace       # Ajouter une marketplace
/plugin manage-marketplace    # Gerer les marketplaces
```

#### Chiffres

- 834 plugins dans 43 marketplaces (janvier 2026)
- 2 sources : "Anthropic Cloud Code" + "Official Plugins"
- Auto-update au demarrage possible

#### Structure d'un plugin

```
mon-plugin/
├── .claude-plugin/
│   └── marketplace.json    # Metadata
├── agents/                 # Agents custom
├── commands/               # Commandes slash
└── hooks/                  # Hooks custom
```

#### Avertissement Melvynx (FORT)

"Deconseilles car pas de controle du code."

- Installent du code non controle dans un dossier non modifiable
- Se mettent a jour automatiquement (ecrasent vos modifs)
- 25+ plugins = 200+ commandes → pollution du contexte
- **Recommandation** : copier le code dans vos propres skills
  "Volez les concepts, creez votre propre configuration"

#### Toggle et gestion

- `Space` pour activer/desactiver sans supprimer
- `Delete` pour marquer a desinstaller

---

### B8. Channels et remote

#### Claude Code Channels (mars 2026, research preview)

Annonce le 20 mars 2026. Requiert Claude Code v2.1.80+,
login claude.ai, Bun installe.

**Plateformes supportees** : Telegram, Discord, iMessage

**Setup Telegram** :
```bash
# 1. Installer le plugin
/plugin install telegram@claude-plugins-official

# 2. Configurer le token
/telegram:configure <token>

# 3. Redemarrer avec channels
claude --channels plugin:telegram@claude-plugins-official

# 4. Pairer
/telegram:access pair <code>
/telegram:access policy allowlist
```

**Setup Discord** :
```bash
# 1. Creer l'app sur Discord Developer Portal
# 2. Activer Message Content Intent
# 3. Installer et configurer
/plugin install discord@claude-plugins-official
/discord:configure <token>
claude --channels plugin:discord@claude-plugins-official
/discord:access pair <code>
```

**Setup iMessage** (macOS uniquement, Full Disk Access requis) :
```bash
/plugin install imessage@claude-plugins-official
claude --channels plugin:imessage@claude-plugins-official
/imessage:access allow +15551234567
```

#### Claude Dispatch (Cowork)

Controler son ordinateur depuis l'iPhone via QR code.
Fonctionne dans une VM Linux isolee.

- **Dispatch ≠ OpenClaw** : Dispatch run sur ta machine locale,
  OpenClaw run sur un VPS distant
- Dispatch = outil de "normies" (interface simplifiee)
- OpenClaw = outil de power users (full access)

#### /remote control

```bash
claude --remote-control
```

Genere un lien URL ouvrable sur iPhone/Android.
Messages synchronises en temps reel.
Prerequis : abonnement Max.
Architecture : tunnel WebSocket direct, pas besoin du meme WiFi.

Cas d'usage : partir en pause, continuer depuis le telephone.
Pair programming : deux personnes sur le meme Claude.

#### OpenClaw (agent VPS distant)

Agent IA autonome auto-heberge sur VPS, connecte via 23+ canaux
de messagerie (Telegram, WhatsApp, Discord, Signal, iMessage...).

Cree par Peter Steinberger. 342k+ stars sur GitHub.

**Cas d'usage reels Melvynx** :
- A la piscine : envoyer un audio Telegram pour fixer un bug
- Gestion emails sponsors (agent "Steve" auto-repond)
- Coupons, abonnes, deploiements — tout depuis Telegram
- Lancer des features, creer des PR, merger du code
- Threads specialises : coach, love, general, docteur, work

**Installation VPS** :
```bash
npx openclaw-vps-setup
```
Installe : Node.js, OpenClaw, Claude Code, Bun,
Cloudflared, GCloud + securite (UFW, Fail2Ban)

**VPS recommande** : Hetzner CAX21/CAX31 (~7.59$/mois)
ou 8 VCPU AMD, 16 Go RAM, 300 Go disque (~20$/mois)

**Architecture** :
```
VPS (Ubuntu)
  ├── OpenClaw Gateway (service permanent → Telegram)
  ├── Claude Code (bypass, debug)
  ├── GitHub CLI
  ├── Skills/ (custom)
  ├── Workspace/ (memoire, configs)
  └── Cloudflared (tunnel Gmail webhooks)
```

**Fichiers config** :

| Fichier | Role |
|---------|------|
| `soul.md` | Identite/personnalite du bot |
| `memory/` | Memoire quotidienne |
| `situation.md` | Contexte auto-mis a jour |
| `.env` | Tokens, API keys |

**Cout** : chat/email = pas cher. Code = cher (pas de caching
sur Agent SDK). Conseil : 50% OpenClaw / 50% CLI directement.

---

### B9. Memoire et persistance

#### CLAUDE.md

Le fichier principal de memoire. 3 niveaux (global, projet,
dossier). Contient les instructions, conventions, stack technique.
Max ~200-500 lignes.

Generation : `/init` dans un contexte enrichi.

#### rules/

Regles modulaires dans `.claude/rules/`. Deux types :
- Always apply (defaut)
- Conditionnel avec globs (charge pour certains fichiers)

Cas d'usage : quand l'IA fait une erreur 2+ fois, creer une
regle. "Tu as fait cette erreur trop souvent. Cree un fichier
dans .claude/rules/ qui t'empechera de la refaire."

Regle optimale :
`CRITICAL: Never create middleware.ts — middleware is proxy
helper in proxy utils`

#### memory/ (fichiers detailles)

Pour les documents trop longs pour rules/ (>50 lignes).
Architecture, lecons passees, conventions strictes, etc.

#### Auto-Memory native

```json
{
  "autoMemoryEnabled": true
}
```

Claude s'ecrit des notes entre sessions.
Stocke dans `~/.claude/projects/<hash>/memory/`.
200 premieres lignes de MEMORY.md chargees a chaque session.

**Conseil** : preferer dire "modifie le CLAUDE.md avec cette info"
plutot que laisser l'auto-memory sauvegarder n'importe quoi.

#### Ajout rapide de memoire

- `#texte` dans le prompt → choix project/local/user memory
- `yes [information]` → Claude propose de sauvegarder
- `/memory` pour ouvrir le fichier memoire

#### Keywords de priorite

Mots-cles que l'IA detecte et priorise :
- `CRITICAL` — priorite maximale
- `MANDATORY` — obligatoire
- `VALIDATION` / `VERIFICATION` — verification requise
- `non-negotiable` — renforce la contrainte
- Majuscules = plus d'impact que minuscules

#### Sessions et historique

- `/resume` : retrouver une ancienne conversation
- `/rewind` : restaurer code ET conversation a un etat precedent
- `Escape x2` : acces historique + restauration
- Les transcripts sont stockes dans `.claude/projects/`
  (recuperables : "analyse le dossier projects pour retrouver
  la session qui parle de X")

---

### B10. Automatisation

#### /loop — Taches recurrentes (en session)

```bash
/loop 5m check if the deployment finished
/loop 20m /review-pr 1234
/loop check the build every 2 hours
```

- Intervalles : `s` (secondes), `m` (minutes), `h` (heures), `d` (jours)
- Defaut : 10 minutes si pas d'intervalle
- Session-scoped : disparait quand on quitte
- Expiration automatique : 3 jours
- `loop stop` pour arreter

#### /schedule — Taches planifiees persistantes (cron)

Cron persistant qui survit aux sessions.

3 niveaux :

| Type | Ou | Persistant | Fichiers locaux |
|------|-----|-----------|----------------|
| Cloud | Anthropic cloud | Oui | Non (clone) |
| Desktop | Machine locale | Oui | Oui |
| /loop | Session | Non | Oui |

Cron 5 champs standard : `minute hour day-of-month month day-of-week`
Max 50 taches par session.

Exemple : "tous les matins a 9h, clean le dossier Downloads"

#### /batch — Changements a grande echelle

```bash
/batch migrate src/ from Solid to React
```

1. Recherche le codebase, decompose en 5-30 unites
2. Present un plan pour approbation
3. Spawn 1 agent background par unite (git worktree isole)
4. Chaque agent implemente, teste, ouvre une PR

**Avertissement Melvynx** : ANTI-PATTERN. Genere des dizaines
de PR a review. Preferer un main agent orchestrateur.

#### Ralph — Boucle autonome infinie

"Lance-le quand tu vas dormir, leve-toi le matin avec 25 bugs resolus."

Ralph = boucle autonome ou Claude execute des taches une par une
sans s'arreter, meme au-dela du context window.

**Architecture** :
1. **PRD** : document Markdown decrivant la feature complete
2. **prd.json** : PRD transforme en user stories
   (`id`, `priority`, `pass`, `note`)
3. **progress.txt** : memoire de progres
4. **prompt.md** : prompt de chaque iteration
5. **ralph.sh** : script bash qui boucle

**Workflow** :
1. Ecrire un PRD (ou generer avec plan mode)
2. Transformer en `prd.json` avec user stories
3. Lancer `ralph.sh`
4. Chaque iteration : implemente → commit → update prd.json
   (`pass: true`) → notes dans progress.txt
5. Si contexte atteint → stop → relance → reprend

**Astuces** :
- `-N 5` : 5 stories par run
- Story finale : "Lance create-pr"
- Story CI : "Run CI fixer auto avec 50 tentatives max"
- **Liste de bugs comme PRD** : chaque bug = une story.
  Lancer ralph.sh le soir → tous resolus le matin.

---

### B11. Usage non-code

Claude Code n'est pas limite au developpement. Melvynx
utilise un dossier `~/cc/` pour tout :

#### Comptabilite
- Analyses financieres, calculs de marges
- Suivi des revenus SaaS

#### Todo lists
- Gestion de taches quotidiennes
- Priorisation automatique

#### Analyse de tweets
- Workflow : tweet → FX Twitter → video → FFmpeg split →
  Whisper transcription → Vision frames → synthese

#### Newsletter
- Skill Lumail → CLI Lumail → redaction + planification

#### Transcription audio
- Whisper API integration
- App Parler (Melvynx) : speech-to-text local gratuit

#### Organisation de fichiers
- Trier, renommer, classer des fichiers
- Changer les applications par defaut (macOS via `duti`)
- Configurer le systeme, Wi-Fi, reglages OS

#### Generateur de titres YouTube
- Brainstorm automatise pour titres accrocheurs

#### Createur de slides
- Generation de presentations

#### Reviews d'emails
- Resume automatique, tri, reponse

Philosophie Melvynx : "Est-ce que Claude Code peut le faire ?
Si oui, pas besoin de SaaS."

---

### B12. SaaS et business

#### Applications creees par Melvynx avec Claude Code

| App | Description |
|-----|-------------|
| **Umail.io** | Newsletter concurrent MailChimp (2M+ emails) |
| **SaveIt.Now** | Gestionnaire de signets avec IA |
| **Subfast** | Generateur de miniatures YouTube |
| **Chao.App** | Chatbot integrable, cree SANS code |
| **PaddleTally** | App iOS + Apple Watch (jamais code iOS avant) |
| **WhisperTurbo** | Speech-to-text local gratuit open source |
| **Parler** | Speech-to-text local, DMG sur GitHub |

Capable de travailler sur des codebases de 4+ ans.

#### BYOAPI (Bring Your Own API Key)

Modele SaaS ou le power user apporte sa propre cle API.
Avantage : pas de couts API pour le createur.
Exemple : Subfast a 199$ lifetime pour BYOAPI users.

#### Dogfooding

"Utilisez vos propres outils et creez des outils pour vous."
Creer un outil pour soi-meme = qualite superieure car on
l'utilise quotidiennement.

#### Protection IP

"La sloperie" : n'importe qui peut copier ton app avec l'IA.
Ce qui ne se copie pas :
- La vision et le taste du createur
- Les ameliorations continues
- La relation avec l'audience
- La preuve sociale (Product Hunt, reviews)

"L'IA est un accelerateur. Si tu es mauvais, elle accelere
ta mediocrite. Si tu es bon, elle accelere tes points forts."

---

### B13. IDE et editeurs

#### VS Code Extension

- Panel integre VS Code : acces direct a Claude depuis l'editeur
- Installation : `Cmd+Shift+P` > "Shell Command: Install code in PATH"
- `Cmd+Option+K` : ajouter contexte selectionne a Claude Code
- Multiple conversations paralleles
- Background jobs avec interface dediee
- Split diff natif (side-by-side avant/apres)

#### JetBrains Extension

Extension disponible pour IntelliJ, WebStorm, etc.
Integration similaire a VS Code.

#### Zed IDE

IDE favori de Melvynx pour editer du code.
- Ecrit en Rust (non-Electron), startup quasi-instantane
- Minimalisme UI : quasi zero boutons (force clavier)
- Integration IA : Claude Code, Copilot, Gemini, inline `Cmd+K`
- `Cmd+Shift+Y` : commit message auto
- Recherche `Cmd+F` = live edit direct
- **Limitations** : ne peut pas renommer tabs, copier fichiers entre projets
- Verdict Melvynx : "IDE parfait pour devs pro"

#### CEMUX — Terminal recommande

Terminal build pour coding agents. Base sur Ghostty (Swift/AppKit).
Gratuit. 4.7 stars.

**Features** :
- Tabs verticaux + horizontaux (multi-niveaux)
- Notifications : "Claude Code is waiting your inputs"
- Browser integre (Safari)
- Affichage des ports ecoutes
- Affichage des PR ouvertes
- Affichage des branches Git
- Split pane

**Raccourcis** :
- `Ctrl+1/2/3` : navigation entre splits
- `Cmd+Shift+R` : renommer workspace
- `Cmd+R` : renommer tab

#### Ghostty — Terminal minimaliste

- Terminal optimise, gratuit, minimaliste
- `Cmd+1/2/3` : tabs
- `Cmd+W` : fermer
- `Cmd+T` : nouveau tab
- Ideal pour gerer 5+ projets en parallele

#### Helium (Hium) — Navigateur lightweight

- Alternative a Arc (691 MB de RAM)
- Base sur Chrome, tres leger
- Pas de trackers, pas de bloat AI
- Extensions Chrome compatibles

**Combo recommande** : CEMUX + Hium = environnement leger

---

### B14. Commandes slash natives (reference complete)

| Commande | Action |
|----------|--------|
| `/clear` | Reset contexte (OBLIGATOIRE entre taches) |
| `/init` | Generer CLAUDE.md du projet |
| `/context` | Afficher utilisation contexte en % |
| `/usage` | Limites et consommation |
| `/model` | Changer modele |
| `/agent` | Creer/gerer agents |
| `/mcp` | Voir MCP connectes |
| `/plugin` | Installer plugins |
| `/stat` | Statistiques session |
| `/config` | Configuration interactive |
| `/voice` | Mode vocal (beta) |
| `/loop` | Tache recurrente a intervalle |
| `/schedule` | Taches planifiees persistantes |
| `/batch` | Changements paralleles grande echelle |
| `/simplify` | Review et simplification (3 agents) |
| `/copy` | Copier derniere reponse en Markdown |
| `/debug` | Auto-debug configuration |
| `/compact` | Compacter le contexte |
| `/rewind` | Revenir a un etat precedent |
| `/resume` | Retrouver une ancienne conversation |
| `/remote control` | Controle a distance via telephone |
| `/memory` | Ouvrir le fichier memoire |

#### Raccourcis clavier

| Raccourci | Action |
|-----------|--------|
| `Escape x1` | Stoppe Claude / efface texte courant |
| `Escape x2` | Historique conversations |
| `Shift+Tab` | Changer mode permissions |
| `Tab` | Activer/desactiver thinking mode |
| `Ctrl+T` | Taches en cours |
| `Ctrl+O` | Transcript verbose |
| `Ctrl+S` | Sauvegarder prompt |
| `Ctrl+B` | Backgrounder un subagent en cours |
| `Meta+P` | Changer modele |
| `Ctrl+C` | Arreter Claude Code |
| Fleche haut/bas | Historique messages |

#### Prefixes dans le prompt

| Prefixe | Effet |
|---------|-------|
| `@fichier` | Taguer un fichier (contexte) |
| `/commande` | Lancer une commande (Tab pour selectionner) |
| `!commande` | Mode bash direct (resultat dans le contexte) |
| `#texte` | Ajouter une memoire (project, local, user) |

---

### B15. Securite avancee

#### Hooks securite (5 hooks JARVIS)

| Hook | Matcher | Role |
|------|---------|------|
| dangerous-actions-blocker | Bash | 19 patterns (rm -rf, chmod 777, fork bomb) |
| secrets-scanner | Write/Edit | 9 patterns credentials (API keys, SSH) |
| prompt-injection-detector | Tous | 9 patterns injection (DAN mode, etc.) |
| zones-interdites-guard | Bash/Write/Edit | Zones protegees |
| tool-usage-logger | PostToolUse | Telemetrie async |

#### Threat DB

Base de donnees de menaces : 165 patterns, 12 categories.

12 categories :
1. prompt_injection (CRITICAL) — 20 patterns
2. data_exfiltration (CRITICAL) — 12 patterns
3. destructive_commands (CRITICAL) — 19 patterns
4. credential_exposure (HIGH) — 23 patterns
5. supply_chain (HIGH) — 14 patterns
6. privilege_escalation (HIGH) — 12 patterns
7. network_attacks (HIGH) — 15 patterns
8. persistence_backdoor (HIGH) — 15 patterns
9. crypto_mining (MEDIUM) — 9 patterns
10. log_tampering (MEDIUM) — 9 patterns
11. unsafe_file_ops (MEDIUM) — 8 patterns
12. container_escape (HIGH) — 8 patterns

#### Triple couche de securite

1. **Deny list** dans settings.json (commandes bloquees)
2. **Command validator** (PreToolUse hook, AST-level)
3. **CLAUDE.md** ("utilise trash au lieu de rm")

#### Garde-fous anti-suppression

- Toute suppression exige : justification + triple sauvegarde
  (trash + knowledge graph + git commit)
- Toute comparaison de doublons doit suivre les 4 etapes
  jdupes/fdupes : taille → hash partiel → hash complet → byte-by-byte
- JAMAIS se baser sur la taille seule

#### Claudeception — Auto-apprentissage

Meta-skill qui extrait automatiquement les connaissances
des sessions de travail et les codifie en skills reutilisables.

Installation : skill + hook UserPromptSubmit + resources

---

### B16. Les 6 niveaux de maitrise (Melvynx)

| Niveau | Nom | Description |
|--------|-----|-------------|
| 1 | **Prompteur** | Ordres bruts, context rot |
| 2 | **Planificateur** | Plan Mode, PRD, /clear |
| 3 | **Ingenieur Contexte** | /context, /compact, StatusLine, tokens |
| 4 | **Skill Builder** | Cree skills des qu'action repetee 2+ fois |
| 5 | **Multi-Agent** | 4+ terminaux, work trees, agent swarm |
| 6 | **Agents Autonomes H24** | OpenClaw VPS, agents specialises, 24/7 |

#### Les 4 niveaux de developpeur IA

| Niveau | Profil | Description |
|--------|--------|-------------|
| 1 | **Anti-IA** | Usage minimal, ne croit pas en l'IA |
| 2 | **IA Assisted** | Monitore l'IA, review tout, prompts precis |
| 3 | **Builder** | Agent mode, review logique/vibe, workflows |
| 4 | **Dev Manager** | 4-12 agents simultanes, token maxing, full access |

---

### B17. Les "Core 7" de Claude Code (terminologie Melvynx)

Les 7 fonctionnalites qui font de Claude Code ce qu'il est :

1. **Commandes/Skills** — prompts reutilisables
2. **Hooks** — automatisation des evenements
3. **Memoire** — CLAUDE.md + rules/
4. **MCP** — outils externes (a remplacer par CLI)
5. **StatusLine** — monitoring en temps reel
6. **Sub-agents** — delegation et parallelisation
7. **Context Management** — gestion du contexte

---

### B18. Chrome Browser Automation (2026)

```bash
claude --chrome
```

Prerequis : extension Chrome "Claude" + CLI a jour + plan payant.
Technologie : Chrome Native Messaging API (pas Playwright).
Utilise le VRAI Chrome avec extensions, sessions, auth.

**Outils** : tab_context, create_tab, navigate, read_page,
search, click, scroll, screenshot, drag, zoom, hover,
find_forms_inputs, javascript_tool, gif_creator, upload,
network_access, shortcuts

**Cas d'usage** :
- Live debugging dans le navigateur
- Verification visuelle du design
- Web app testing avec auth
- Data extraction
- Session recording
- Modifier localStorage/feature flags

**Features avancees** :
- Feedback loop : Claude verifie visuellement son code
- Pair-browsing : l'humain interagit en meme temps
- Pause login : s'arrete sur page d'auth, attend connexion humaine

---

### B19. Output Styles (personnalisation)

Adapter Claude Code au-dela du Software Engineering.

Fichiers : `~/.claude/output-styles/` (ex: `Honest.md`, `Senior Dev.md`)
Activation : `/output-style`
Applique au niveau projet (`settings.local.json`)

Remplace le system prompt standard et l'oriente vers d'autres
roles (assistant, senior dev, architecte, etc.).

---

### B20. StatusLine (monitoring en temps reel)

Affiche en permanence en bas du terminal :
- Dossier courant et repo git
- Fichiers ajoutes/supprimes/staged
- Modele utilise
- Cout de la session en $
- Tokens utilises et % contexte
- Barre de progression avec couleurs
- Heures restantes dans la session
- Depenses du jour

Necessite `bun` installe (`brew install bun`).

Si ne marche pas : `/debug` (la StatusLine a des tests integres).

Parametre `usableContextOnly` : soustrait les 45 000 tokens
d'autocompact du total pour afficher le contexte reel.

---

### B21. Installation rapide (Blueprint CLI)

```bash
npx ig-blueprint-cli
```

Installe automatiquement :
- Backup de la config existante
- Skills gratuits (15)
- Command validator
- StatusLine
- Sons notification
- Permissions bypass + deny list

Si ca ne marche pas : `/debug` pour auto-reparation.

**Niveaux de configuration** :

| Niveau | Description |
|--------|------------|
| 0 | Aucune config (Claude Code brut) |
| OK | Installation standard |
| **OG (90%)** | Config gratuite Melvynx (`npx ig-blueprint-cli`) |
| **OG++ (99%)** | Config premium Melvynx (workflows avances) |

---

## ANTI-PATTERNS COMPLETS (25 regles)

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
20. JAMAIS attendre des agents teams qu'ils communiquent entre eux
21. JAMAIS installer 25 plugins (contexte pollue)
22. JAMAIS laisser autocompact actif (utiliser /clear)
23. JAMAIS un dev = un seul agent (minimum 3 en parallele)
24. JAMAIS coder via OpenClaw quand on est devant son ordi
25. JAMAIS utiliser tool search si MCP < 2-3% du contexte

---

## COMPARATIFS (resume)

### Claude Code vs Cursor vs GitHub Copilot

| Critere | Claude Code | Cursor | GitHub Copilot |
|---------|------------|--------|----------------|
| Vitesse | Moyen | Rapide | Lent |
| Gestion CLI | Excellent | Bon | Mauvais |
| Customisation | Maximum | Moyenne | Faible |
| Courbe apprentissage | Raide | Facile | Moyenne |
| Longevite | Terminal = portable | Startup | Microsoft |
| Rapport valeur | Meilleur (Max 20x) | 20$/mois | 10$/mois |

### OpenClaw vs Claude Code Channels

| Aspect | OpenClaw | CC Channels |
|--------|----------|-------------|
| Canaux | 23+ | 3 |
| Modeles | Multi-provider | Claude uniquement |
| Memoire | Persistante | Session-scoped |
| Setup | ~15 min | ~5 min |
| Autonomie | Agent 24/7 | Requiert session ouverte |

---

## GLOSSAIRE

| Terme | Definition |
|-------|-----------|
| **Agent** | Programme autonome qui execute des actions en boucle |
| **APEX** | Workflow Analyze-Plan-Execute-eXamine de Melvynx |
| **Autocompact** | Compression auto du contexte a ~96-99% |
| **BYOAPI** | Bring Your Own API Key (modele SaaS) |
| **CEMUX** | Terminal multi-tab pour coding agents |
| **Channels** | Integration messagerie (Telegram, Discord, iMessage) |
| **Context Window** | Fenetre de tokens disponibles (200K) |
| **Deny list** | Liste de commandes bloquees meme en bypass |
| **Dispatch** | Controle distance iPhone via Cowork |
| **EPCT** | Explore-Plan-Code-Test (workflow de base) |
| **Greenfield** | Nouveau projet from scratch |
| **Brownfield** | Projet existant a modifier |
| **Hook** | Script execute a un evenement (Pre/Post/Stop) |
| **Lost in the Middle** | Probleme d'attention au milieu du contexte |
| **MCP** | Model Context Protocol (outils externes) |
| **OpenClaw** | Agent autonome VPS distant (23+ canaux) |
| **Prompt Discovery** | Decoupage en steps/ anti Lost-in-the-Middle |
| **Ralph** | Boucle autonome infinie (PRD → stories → execute) |
| **Rules** | Fichiers .md modulaires dans .claude/rules/ |
| **SCRATCHPAD.md** | Bloc-notes de l'IA (memoire chaude) |
| **Skill** | Set de prompts reutilisables (SKILL.md) |
| **StatusLine** | Barre d'info en bas du terminal |
| **Sub-agent** | Mini-agent avec contexte isole (120K tokens) |
| **Team** | 2-16 agents sur codebase partagee |
| **Tool call** | Appel d'outil par le modele (JSON structure) |
| **Tool Search** | Lazy loading MCP (-95% contexte) |
| **Work Tree** | Branche git isolee pour agent parallele |

---

> "S'amuser avec Claude, c'est la meilleure chose que vous
> pouvez faire dans votre vie." — Melvynx


## CHAPITRE 32 — CONFIGURATION MAC KIM13 COMPLETE

> Contenu integral de la config Mac — fusion depuis BIBLE_CONFIG_MAC_COMPLETE.md

# Bible Claude Code — Configuration Mac Reelle de Kim13
# Chapitres complementaires a la Bible Melvynx V4.1
# Date : 2026-03-31 | Machine : MacBook Pro M4 24GB
# Utilisateur : kim13karame.com (Soly Kim)

---

# CHAPITRE A — INVENTAIRE COMPLET DENY RULES (76 regles)

La deny-list est le premier rempart de securite.
Configuree dans `~/.claude/settings.json` > `permissions.deny`.
La Bible Melvynx ne documente que 6-7 deny rules.
Voici les 76 regles reelles deployees.

## A.1 — Protections suppression (12 regles)

```
Bash(rm -rf *)
Bash(rm -rf /)
Bash(rm -rf C:\Windows*)
Bash(rm -rf G:*)
Bash(rm -rf H:*)
Bash(rm -rf ~)
Bash(sudo rm *)
Bash(sudo rm -rf *)
Bash(rm -r *)
Bash(rm -f *)
Bash(git clean -f*)
Bash(shred *)
```

Objectif : bloquer TOUTE forme de suppression.
`rm -rf`, `rm -r`, `rm -f`, `sudo rm`, `shred`.
Protection speciale des disques BOKADOR (G:, H:).

## A.2 — Protections Git (4 regles)

```
Bash(git push --force *)
Bash(git push -f *)
Bash(git reset --hard *)
Bash(git push --force-with-lease origin main*)
```

Objectif : empecher destruction de l'historique Git.
Le force-push sur main est explicitement bloque.

## A.3 — Protections processus (3 regles)

```
Bash(kill -9 *)
Bash(killall *)
Bash(pkill *)
```

Objectif : empecher le kill sauvage de processus.
Le protocole kill (rule 26) exige justification + sauvegarde
AVANT tout kill.

## A.4 — Protections systeme (8 regles)

```
Bash(launchctl unload*)
Bash(launchctl bootout*)
Bash(tmutil delete*)
Bash(sudo *)
Bash(reboot *)
Bash(shutdown *)
Bash(init 0)
Bash(init 6)
```

Objectif : bloquer les operations systeme dangereuses.
`sudo` est completement bloque en deny.
Les LaunchAgents (com.kim.*) ne peuvent pas etre desactives.
Les snapshots Time Machine ne peuvent pas etre supprimes.

## A.5 — Protections disques et formatage (4 regles)

```
Bash(dd if=/dev/zero*)
Bash(diskutil eraseDisk *)
Bash(diskutil eraseVolume *)
Bash(mkfs *)
```

Objectif : empecher tout effacement de disque/partition.

## A.6 — Protections reseau (2 regles)

```
Bash(netcfg *)
Bash(> /dev/sd*)
```

Objectif : empecher les modifications reseau catastrophiques.
`netcfg -d` a cause un incident majeur sur BOKADOR (2026-02-25).
L'ecriture directe sur les peripheriques bloc est bloquee.

## A.7 — Protections execution distante (4 regles)

```
Bash(curl * | bash)
Bash(curl * | sh)
Bash(wget * | bash)
Bash(wget * | sh)
```

Objectif : empecher l'execution de scripts telecharges a la volee.

## A.8 — Protections permissions et publication (3 regles)

```
Bash(chmod 777 *)
Bash(npm publish *)
Bash(format *)
```

Objectif : pas de permissions trop larges, pas de publication
accidentelle sur npm, pas de formatage de disque.

## A.9 — Protections lecture fichiers sensibles (8 regles)

```
Read(**/*.pem)
Read(**/*password*)
Read(**/*secret*)
Read(**/*token*)
Read(./.env)
Read(./.env.*)
Read(.env)
Read(.ssh/id_*)
```

Objectif : empecher la lecture de certificats, mots de passe,
secrets, tokens, variables d'environnement et cles SSH.
Couche de defense supplementaire au-dela du deny Bash.

## A.10 — Protections Bible (6 regles)

```
Bash(rm *BIBLE*)
Bash(rm *bible*)
Bash(trash *BIBLE*)
Bash(chflags nouchg *BIBLE*)
Bash(chflags nouchg *bible*)
Bash(mv *BIBLE* /tmp*)
Bash(mv *bible* /tmp*)
```

Objectif : la Bible est INTOUCHABLE.
Ni suppression, ni trash, ni deverrouillage uchg,
ni deplacement vers /tmp.

## A.11 — Recapitulatif par categorie

| Categorie | Nb regles |
|-----------|-----------|
| Suppression (rm, shred, git clean) | 12 |
| Git (push force, reset hard) | 4 |
| Processus (kill, killall, pkill) | 3 |
| Systeme (launchctl, reboot, sudo) | 8 |
| Disques (dd, diskutil, mkfs) | 4 |
| Reseau (netcfg, /dev/sd) | 2 |
| Execution distante (curl/wget pipe) | 4 |
| Permissions/publication | 3 |
| Lecture sensible (Read deny) | 8 |
| Protection Bible | 6 |
| **TOTAL** | **54 regles uniques** |

Note : le fichier settings.json contient 54 regles deny uniques
(pas 76 comme annonce dans l'audit initial qui comptait des
variantes et des doublons conceptuels).

---

# CHAPITRE B — INVENTAIRE COMPLET SKILLS (50 skills)

Repertoire : `~/.claude/skills/*/SKILL.md`
Chaque skill est un prompt reutilisable avec frontmatter YAML.

## B.1 — Skills Melvynx standard (15)

| # | Skill | Description |
|---|-------|-------------|
| 1 | apex | APEX (Analyze-Plan-Execute-Validate) avec agents paralleles |
| 2 | claude-memory | Gestion CLAUDE.md et rules/ |
| 3 | commit | Commit rapide avec messages propres (model: haiku) |
| 4 | create-pr | PR auto-generee (model: haiku) |
| 5 | fix-errors | Fix ESLint/TypeScript avec sub-agents paralleles |
| 6 | fix-grammar | Correction grammaire/orthographe multi-fichiers |
| 7 | fix-pr-comments | Implementation des commentaires de PR review |
| 8 | merge | Merge intelligent avec resolution de conflits |
| 9 | oneshot | Implementation rapide Explore > Code > Test |
| 10 | prompt-creator | Engineering de prompts pour LLMs |
| 11 | ralph-loop | Boucle autonome Ralph (code pendant que tu dors) |
| 12 | skill-creator | Guide creation de skills Claude Code |
| 13 | subagent-creator | Guide creation de sub-agents |
| 14 | ultrathink | Reflexion profonde, mode artisan |
| 15 | workflow-apex-free | APEX sans abonnement (version libre) |

## B.2 — Skills custom Kim — Developpement (10)

| # | Skill | Description |
|---|-------|-------------|
| 16 | brainstorm | Recherche profonde en 4 rounds |
| 17 | clean-code | Principes et application du code propre |
| 18 | doc-guard | Guard documentation avant modification |
| 19 | git-workflow | Workflow Git specialise |
| 20 | plan-before-code | Planifier avant de coder |
| 21 | tdd-workflow | Workflow Test-Driven Development |
| 22 | workflow-code | Workflow de codage structure |
| 23 | workflow-steps | Execution sequentielle anti lost-in-the-middle |
| 24 | meta-prompt-creator | Meta-creation de prompts |
| 25 | parallel-agents | Orchestration d'agents en parallele |

## B.3 — Skills custom Kim — Sysadmin/Infra (8)

| # | Skill | Description |
|---|-------|-------------|
| 26 | auto-backup | Sauvegarde auto config Claude Code |
| 27 | backup-strategy | Strategie de backup |
| 28 | docker-management | Gestion Docker |
| 29 | incident-response | Reponse aux incidents |
| 30 | infra-check | Verification infrastructure |
| 31 | monitoring-setup | Mise en place monitoring |
| 32 | network-hardening | Durcissement reseau |
| 33 | error-recovery | Recuperation apres erreur |

## B.4 — Skills custom Kim — Securite (4)

| # | Skill | Description |
|---|-------|-------------|
| 34 | cyber-defense-team | Pipeline 4 agents defense cyber (logs) |
| 35 | security-config-audit | Audit securite de la config |
| 36 | hook-false-positive-bypass | Contourner les faux positifs des hooks |
| 37 | uchg-config-modification | Modifier fichiers proteges uchg |

## B.5 — Skills custom Kim — Meta/Audit (4)

| # | Skill | Description |
|---|-------|-------------|
| 38 | audit-agents-skills | Audit qualite agents/skills/commands |
| 39 | eval-skills | Scoring exhaustif de tous les skills |
| 40 | claudeception | Extraction de connaissances des sessions |
| 41 | bible-melvynx | Charge la Bible comme reference (model: opus) |

## B.6 — Skills custom Kim — Memoire/Context (6)

| # | Skill | Description |
|---|-------|-------------|
| 42 | auto-learn | Auto-apprentissage de chaque interaction |
| 43 | learn-from-user | Apprentissage des preferences utilisateur |
| 44 | context-management | Gestion du contexte conversationnel |
| 45 | strategic-compact | Compaction strategique du contexte |
| 46 | thoughts-directory | Repertoire de reflexions persistantes |
| 47 | my-daily-standup | Standup quotidien automatise |

## B.7 — Skills custom Kim — Profil (3)

| # | Skill | Description |
|---|-------|-------------|
| 48 | my-analysis | Analyse fichier/dossier avec rapport structure |
| 49 | my-profile | Profil et preferences utilisateur |
| 50 | automation-patterns | Patterns d'automatisation recurrents |

---

# CHAPITRE C — INVENTAIRE COMPLET AGENTS (31 agents)

Repertoire : `~/.claude/agents/*.md`
Chaque agent est un sub-agent specialise avec model et tools definis.

## C.1 — Agents Melvynx standard (4)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 1 | action | haiku | Executeur conditionnel (si X alors Y) |
| 2 | explore-codebase | haiku | Exploration du codebase |
| 3 | explore-docs | haiku | Recherche docs via Context7 |
| 4 | websearch | — | Recherche web rapide |

## C.2 — Agents custom Kim — Architecture (5)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 5 | architecture-reviewer | opus | Review architecture (read-only) |
| 6 | plan-challenger | opus | Attaque adversariale des plans |
| 7 | planner | sonnet | Planification |
| 8 | planning-coordinator | opus | Synthese multi-agents de recherche |
| 9 | implementer | haiku | Execution mecanique de taches bornees |

## C.3 — Agents custom Kim — Code (5)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 10 | code-explorer | sonnet | Exploration de code |
| 11 | debugger | sonnet | Debugging |
| 12 | refactoring-specialist | sonnet | Refactoring SOLID |
| 13 | reviewer | sonnet | Code review |
| 14 | test-writer | sonnet | Ecriture de tests |

## C.4 — Agents custom Kim — Securite (3)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 15 | security-auditor | sonnet | Audit de securite |
| 16 | security-patcher | sonnet | Application de patches securite |
| 17 | firewall-auditor | sonnet | Audit de firewall |

## C.5 — Agents custom Kim — DevOps/Infra (5)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 18 | devops-sre | sonnet | Troubleshooting FIRE framework |
| 19 | infra-checker | sonnet | Verification infrastructure |
| 20 | network_monitor | sonnet | Monitoring reseau |
| 21 | backup-verifier | sonnet | Verification des backups |
| 22 | loop-monitor | haiku | Detecteur de boucles infinies |

## C.6 — Agents custom Kim — Documentation (3)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 23 | doc-explorer | sonnet | Exploration documentaire |
| 24 | doc-writer | sonnet | Redaction de documentation |
| 25 | researcher | sonnet | Recherche approfondie |

## C.7 — Agents custom Kim — Qualite (3)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 26 | output-evaluator | haiku | LLM-as-a-Judge (qualite output) |
| 27 | integration-reviewer | opus | Validation integration runtime |
| 28 | performance-tuner | sonnet | Optimisation performances |

## C.8 — Agents custom Kim — Orchestration (3)

| # | Agent | Model | Description |
|---|-------|-------|-------------|
| 29 | jarvis | sonnet | Agent principal J.A.R.V.I.S. |
| 30 | swarm-coordinator | opus | Orchestrateur de teams multi-agents |
| 31 | web-search | sonnet | Recherche web (doublon websearch) |

## C.9 — Repartition par model

| Model | Nb agents | Usage |
|-------|-----------|-------|
| haiku | 5 | Taches rapides, mecaniques |
| sonnet | 19 | Taches standard, specialisees |
| opus | 5 | Taches complexes, review, synthese |
| non specifie | 2 | (websearch, explore-docs) |

---

# CHAPITRE D — INVENTAIRE COMPLET HOOKS (15 fichiers, 13 actifs)

Repertoire : `~/.claude/hooks/`
Configuration : `~/.claude/settings.json` > `hooks`

## D.1 — Hooks PreToolUse (10 hooks actifs)

### Groupe 1 — Matcher `Bash|Write|Edit|MultiEdit|NotebookEdit`
| Hook | Role |
|------|------|
| `pre-change-guard.sh` | Guard avant toute modification |
| `bun command-validator/src/cli.ts` | Validateur AST Melvynx (TypeScript/Bun) |

### Groupe 2 — Matcher `Bash`
| Hook | Role |
|------|------|
| `claude-firewall.js` | Firewall JavaScript pour commandes Bash |
| `jarvis-firewall.js` | Firewall J.A.R.V.I.S. supplementaire |
| `dangerous-actions-blocker.sh` | Bloqueur d'actions dangereuses |

### Groupe 3 — Matcher `Write|Edit|MultiEdit`
| Hook | Role |
|------|------|
| `secrets-scanner.sh` | Detecte les secrets dans les ecritures |

### Groupe 4 — Matcher `.*` (tout)
| Hook | Role |
|------|------|
| `prompt-injection-detector.sh` | Detecte les injections de prompt |

### Groupe 5 — Matcher `Bash|Write|Edit`
| Hook | Role |
|------|------|
| `zones-interdites-guard.sh` | Protege les zones interdites (Library, iCloud, etc.) |

## D.2 — Hooks PostToolUse (3 hooks actifs)

| Hook | Matcher | Role |
|------|---------|------|
| Inline echo | `Edit\|Write` | Log les modifications dans `/tmp/claude-code-edits.log` |
| `post-change-log.sh` | tous | Logger apres toute modification |
| `tool-usage-logger.sh` | `.*` (async) | Log l'usage de tous les outils |

## D.3 — Hook Stop (1 hook actif)

| Hook | Role |
|------|------|
| `afplay Glass.aiff` (async) | Son de fin de tache |

## D.4 — Hooks UserPromptSubmit (2 hooks actifs)

| Hook | Role |
|------|------|
| `set-tab-title.sh` | Change le titre du terminal/tmux |
| `claudeception-activator.sh` | Active le systeme Claudeception |

## D.5 — Hook Notification (1 hook actif)

| Hook | Role |
|------|------|
| `afplay Funk.aiff` (async) | Son different pour notifications (attente input) |

## D.6 — Fichiers hooks presents mais NON branches

| Fichier | Statut |
|---------|--------|
| `block-delete.sh` | Debranche de PreToolUse (faux positifs grep) |
| `pre-backup.sh` | Present mais non reference dans settings.json |
| `notify-permission.sh` | Present mais non reference |
| `notify-stop.sh` | Present mais non reference |
| `notify-with-timeout.sh` | Present mais non reference |

## D.7 — Pipeline de securite (ordre d'execution PreToolUse)

```
Prompt utilisateur
  |
  v
[UserPromptSubmit] set-tab-title + claudeception-activator
  |
  v
[PreToolUse] Bash|Write|Edit|MultiEdit|NotebookEdit :
  1. pre-change-guard.sh
  2. bun command-validator (AST)
  |
[PreToolUse] Bash seulement :
  3. claude-firewall.js
  4. jarvis-firewall.js
  5. dangerous-actions-blocker.sh
  |
[PreToolUse] Write|Edit|MultiEdit :
  6. secrets-scanner.sh
  |
[PreToolUse] Tout outil :
  7. prompt-injection-detector.sh
  |
[PreToolUse] Bash|Write|Edit :
  8. zones-interdites-guard.sh
  |
  v
--- Execution de l'outil ---
  |
  v
[PostToolUse] Edit|Write :
  9. echo log -> /tmp/claude-code-edits.log
  |
[PostToolUse] Tout :
  10. post-change-log.sh
  11. tool-usage-logger.sh (async)
  |
  v
[Stop] afplay Glass.aiff
[Notification] afplay Funk.aiff
```

---

# CHAPITRE E — INVENTAIRE COMPLET MCP (5 serveurs)

## E.1 — MCP locaux (fichier `~/.mcp.json`)

### Memory (Knowledge Graph)
- **Commande** : `npx -y @modelcontextprotocol/server-memory`
- **Outils** : create_entities, add_observations, create_relations,
  search_nodes, open_nodes, read_graph, delete_entities,
  delete_observations, delete_relations
- **Usage** : memoire structuree inter-sessions, profil utilisateur,
  patterns detectes, erreurs a eviter, inventaire machines,
  decisions techniques

### Context7 (Documentation)
- **Commande** : `npx -y @upstash/context7-mcp@latest`
- **Outils** : resolve-library-id, query-docs
- **Usage** : documentation a jour de librairies, frameworks, SDKs
- **Recommandation Melvynx** : a utiliser systematiquement

### Exa (Recherche web IA)
- **Commande** : `npx -y exa-mcp-server`
- **Outils** : web_search_exa, web_search_advanced_exa, crawling_exa,
  company_research_exa, people_search_exa, get_code_context_exa,
  deep_researcher_start, deep_researcher_check
- **Usage** : recherche web avancee, veille technologique

## E.2 — MCP cloud (integres a Claude)

### Gmail
- **Outils** : authenticate
- **Usage** : lecture/envoi d'emails depuis Claude Code
- **Note** : utilise aussi par OpenClaw pour l'agent email

### Google Calendar
- **Outils** : gcal_list_events, gcal_create_event,
  gcal_update_event, gcal_delete_event, gcal_get_event,
  gcal_list_calendars, gcal_find_meeting_times,
  gcal_find_my_free_time, gcal_respond_to_event
- **Usage** : gestion du calendrier depuis Claude Code

## E.3 — Justification du depassement de la regle "max 3 MCP"

La Bible Melvynx recommande max 2-3 MCP actifs.
Kim en deploie 5 avec cette justification :
- Context7 + Exa = 2 MCP Melvynx standard
- Memory = essentiel pour le knowledge graph (persistence)
- Gmail + Calendar = MCP cloud (pas de consommation locale)
- `toolSearchMode: "auto"` = les outils MCP sont charges
  a la demande, pas tous en permanence (reduit l'impact contexte)
- `autoSearchThreshold` reglable si le contexte depasse 10%

---

# CHAPITRE F — PLUGINS ACTIFS (9 plugins)

Configures dans `settings.json` > `enabledPlugins`

| # | Plugin | Description |
|---|--------|-------------|
| 1 | hookify | Creation de hooks via conversation |
| 2 | commit-commands | Commit, push, clean branches |
| 3 | feature-dev | Dev de features avec comprehension du codebase |
| 4 | code-review | Review de pull requests |
| 5 | code-simplifier | Simplification de code |
| 6 | pr-review-toolkit | Toolkit complet review de PR |
| 7 | claude-md-management | Gestion et amelioration de CLAUDE.md |
| 8 | claude-code-setup | Recommandations d'automatisation |
| 9 | security-guidance | Guidance securite |

Tous sont des plugins officiels (`@claude-plugins-official`).
La Bible dit "JAMAIS 25 plugins" — 9 est dans la zone raisonnable.

---

# CHAPITRE G — PROTOCOLES DE SECURITE KIM

## G.1 — Regle #0 : Documenter AVANT toute action destructive

**Fichier** : `rules/00-doc-before-change.md`
**Priorite** : MAXIMALE

Ordre obligatoire :
1. IDENTIFIER le fichier/dossier concerne
2. DOCUMENTER l'etat actuel dans `~/.claude/logs/modifications.log`
3. SNAPSHOT copier vers `~/.claude/backups/YYYYMMDD_HHMMSS/`
4. PUSH GITHUB commit "doc-guard: snapshot avant [action]"
5. AGIR seulement apres les 4 etapes

Zones critiques (protection renforcee) :
- `~/.claude/settings.json`
- `~/.claude/CLAUDE.md`
- `~/.claude/rules/`
- `~/.claude/memory/`
- `~/.claude/hooks/`
- `~/.claude/skills/`
- `~/.claude/agents/`
- `~/.claude/plugins/`
- H: et G: sur BOKADOR (INTOUCHABLES)

## G.2 — Regle #1 : Triple backup AVANT toute suppression

**Fichier** : `rules/01-no-delete-without-triple-backup.md`
**Priorite** : MAXIMALE — prime sur TOUT

Protocole en 3 etapes OBLIGATOIRES :

### Etape 1 — Justification explicite
- Pourquoi ce fichier doit etre supprime
- Impact de la suppression
- Alternatives proposees (archivage, deplacement)
- Attendre confirmation EXPLICITE de Kim

### Etape 2 — Triple sauvegarde
1. **Trash** : `trash <chemin>` (JAMAIS `rm`)
2. **Knowledge Graph** : observation dans entite `DeletedFiles`
   avec date, chemin, raison, SHA256, emplacement backup
3. **GitHub** : commit "backup: [fichier] avant suppression"

### Etape 3 — Verification
- [ ] trash confirme le deplacement
- [ ] knowledge graph contient l'observation
- [ ] git log confirme le commit

**SI UNE DES 3 CONDITIONS MANQUE → REFUSER LA SUPPRESSION**

## G.3 — Protocole Kill

**Fichier** : `rules/26-process-kill-protocol.md`
**Priorite** : HAUTE

AVANT tout kill/killall/signal de terminaison :
1. JUSTIFIER : pourquoi, impact si on laisse, impact si on tue,
   alternatives au kill
2. ATTENDRE confirmation explicite de Kim
3. SAUVEGARDER : etat du processus (PID, CPU, RAM, uptime)
   dans memory/, raison dans rules/11, commit git si pertinent
4. EXECUTER seulement apres les 3 etapes

## G.4 — IPs de confiance (NEVER ban/block)

```
185.208.60.2     — Ibis/Rezocean (IP travail)
176.133.20.227   — Domicile Soly
109.176.197.139  — VPS Hostinger
31.35.20.184     — BOKADOR (IP publique)
92.92.127.0/23   — Plage Kim
169.254.0.1      — Gateway
127.0.0.0/8, ::1 — Localhost
```

## G.5 — Ports SSH (ne jamais changer sans autorisation)

```
VPS     : port 22   (ssh root@109.176.197.139)
BOKADOR : port 47281 (ssh -p 47281 kim13@31.35.20.184)
Mac     : port 1983  (externe via Bbox NAT)
```

## G.6 — Commandes interdites

```
rm -rf            → utiliser trash
sudo rm           → utiliser trash avec sudo
chmod 777         → trop permissif
git push --force  → sur main/master
netcfg -d         → incident passe (BOKADOR 2026-02-25)
curl ... | bash   → securite
wget ... | bash   → securite
```

## G.7 — Settings de securite dans settings.json

```json
{
  "defaultMode": "bypassPermissions",
  "skipDangerousModePermissionPrompt": true,
  "enableAgentTeams": true,
  "autoCompact": false,
  "autoCompactThreshold": 60,
  "terminalOutputLimit": 60000,
  "toolSearchMode": "auto",
  "env": {
    "ENABLE_LSP_TOOL": "1",
    "MAX_FILE_READ_LINES": "5000"
  }
}
```

- `bypassPermissions` : God Mode actif (toutes les tools autorisees)
- `skipDangerousModePermissionPrompt` : pas d'avertissement au lancement
- `autoCompact: false` : la compaction automatique est desactivee
  (Kim prefere /clear entre les taches)
- `autoCompactThreshold: 60` : seuil de compaction si activee (60%)
- `terminalOutputLimit: 60000` : 60K caracteres max output terminal
- `ENABLE_LSP_TOOL: "1"` : active l'outil LSP (Language Server Protocol)
- `MAX_FILE_READ_LINES: "5000"` : lecture jusqu'a 5000 lignes par fichier

---

# CHAPITRE H — INFRASTRUCTURE RESEAU

## H.1 — Machines

### MacBook Pro M4 (machine principale)
- User : `kim13karame.com`
- RAM : 24 GB | SSD : 1 TB
- IP LAN : 192.168.1.75
- SSH : port 22 local, port 1983 externe
- MAC : 98:fd:b4:9c:02:02 (kims-MBP)

### BOKADOR (serveur Windows)
- Windows 11
- GPU : RTX 3060
- Stockage : 40 TB (disques H: et G: — INTOUCHABLES)
- IP LAN : 192.168.1.97
- SSH : port 22222 interne, port 47281 externe
- IP publique : 31.35.20.184 (via Bbox, IP dynamique)

### VPS Hostinger (Kali)
- IP : 109.176.197.139
- SSH : port 22
- User : root

## H.2 — Bbox NAT/PAT (12 regles actives)

Interface admin : `https://mabox.bytel.fr/natpat`

| # | Nom | Proto | Port ext | Port int | Cible | Usage |
|---|-----|-------|----------|----------|-------|-------|
| 2 | rdp_vps | TCP | 50000 | 3389 | Windows | RDP non-standard |
| 3 | ssh_externe | TCP | 4422 | 44 | 192.168.1.97 | SSH custom |
| 4 | sssh test | TCP | 44 | 22 | Windows | SSH test |
| 5 | ssh anti theft | TCP | 47281 | 22222 | Windows | Anti-vol |
| 6 | SSH_HTTPS | TCP | 443 | 443 | Windows | SSL passthrough |
| 7 | ssh macbook | TCP | 22 | 22 | ancien Mac | SSH ancien |
| 8 | ssh macbook | TCP | 1983 | 22 | kims-MBP | SSH Mac actuel |
| 9 | ssh | TCP | 2223 | 2222 | Windows | VPS→Win (restreint) |
| 10 | ssh | TCP | 2224 | 2222 | Windows | VPS→Win (2eme) |
| 11 | restart | UDP | 9999 | 9 | Windows | Wake-on-LAN |
| 12 | ssh unique vps | TCP | 38271 | 2222 | kims-MBP | VPS→Mac unique |

Regles 9, 10, 12 : restreintes a IP VPS uniquement.
Regle 11 : magic packet UDP Wake-on-LAN.

## H.3 — Connexions SSH rapides

```bash
# Mac depuis exterieur
ssh -p 1983 kim13@31.35.20.184

# Mac depuis VPS
ssh -p 38271 kim13@31.35.20.184

# BOKADOR depuis exterieur
ssh -p 47281 kim13@31.35.20.184

# Windows depuis VPS
ssh -p 2223 user@31.35.20.184

# VPS direct
ssh root@109.176.197.139

# RDP Windows
port 50000 sur 31.35.20.184

# Wake-on-LAN Windows via VPS
UDP port 9999 sur 31.35.20.184
```

---

# CHAPITRE I — ARCHITECTURE MEMOIRE DEPLOYEE

## I.1 — Schema Melvynx 3 niveaux

**Fichier** : `rules/02-memory-architecture-melvynx.md`

### Niveau 1 — Memoire courte (RAM)
= La conversation en cours.
Se perd a la fermeture, au /clear, au changement de contexte.
RIEN d'important ne doit rester UNIQUEMENT ici.

### Niveau 2 — Memoire persistante (Disque dur)
- `CLAUDE.md` : memoire principale (identite, regles, workflow)
- `rules/` : regles persistantes, preferences, garde-fous
- Knowledge Graph (MCP Memory) : relations, decisions, lecons
- `memory/` : fichiers de reference detailles

### Niveau 3 — Memoire contextuelle (a la demande)
- Rules avec `globs: ["*.json"]` → chargees quand on touche du JSON
- Rules avec `globs: ["src/api/**"]` → chargees dans ce dossier
- Notes projet locales → chargees selon le dossier actif

## I.2 — Tableau de repartition — Ou mettre quoi

### CLAUDE.md (global, toujours charge)
- Identite et role
- Stack technique principale
- Regles absolues CRITICAL
- IPs de confiance, ports SSH
- Commandes interdites
- Workflow principal (EPCT)
- Preferences code
- MAX 500 lignes

### rules/ (persistant, toujours charge si alwaysApply: true)
- Regles comportementales
- Preferences detectees
- Erreurs a eviter
- Nouveaux comportements

### memory/ (reference detaillee)
- 00_status.md — etat courant
- 01_architecture.md — architecture projet
- 02_lessons_log.md — erreurs passees
- 03_conventions.md — conventions strictes
- 04_autonomy_matrix.md — matrice d'autonomie
- 05_reflection_loop.md — boucle de reflexion
- 07_security_bypass_guards.md — securite
- 08_claude_code_advanced_patterns.md — patterns avances
- 09_methode_melvynx_complete.md — reference Melvynx

### Knowledge Graph (relations, decisions)
- Profil kim13 (60+ observations)
- MetaPatterns detectes
- ErrorsToAvoid
- Inventaire machines
- Decisions techniques avec date

## I.3 — Bootstrap Jarvis (OBLIGATOIRE)

Avant CHAQUE action, lire silencieusement :
1. `.claude/memory/01_architecture.md`
2. `.claude/memory/02_lessons_log.md`
3. `.claude/memory/03_conventions.md`

Si `/memorize` → workflow auto-apprentissage.
Si `/standup` → charger le contexte complet.

## I.4 — Auto-learning live

**Fichier** : `rules/11-auto-learning-live.md`

5 triggers de detection automatique :
1. **CORRECTION** : Kim dit "non", "pas comme ca"
   → ErrorsToAvoid + rules/11
2. **PREFERENCE** : Kim dit "je prefere", "fais toujours"
   → kim13 entity + rules/11
3. **PATTERN REPETE** : meme demande 2+ fois
   → MetaPatterns + rules/11
4. **DECISION TECHNIQUE** : choix d'outil, lib, archi
   → entity projet + knowledge graph
5. **WORKFLOW IMPLICITE** : enchainement toujours identique
   → MetaPatterns comme workflow confirme

40+ apprentissages historiques documentes depuis 2026-03-11.
Regles : ne JAMAIS supprimer une ligne Learned,
ne JAMAIS contredire une preference existante.

## I.5 — Protocole de fin de session

Avant /clear ou fermeture, se demander :
"Qu'est-ce qui merite d'etre garde ?"

**DOIT etre sauvegarde** :
- "ne jamais faire X" → rules/
- "toujours lancer tel test" → rules/
- "ce dossier contient la logique critique" → memory/ ou KG
- "ce bug a deja ete resolu comme ca" → memory/02 + ErrorsToAvoid
- Toute correction de Kim → rules/11 + ErrorsToAvoid
- Toute preference → rules/11 + kim13
- Toute decision technique → KG + memory/

**Peut rester dans le chat** :
- Reflexions intermediaires
- Tentatives echouees non instructives
- Sorties de commandes brutes

---

# CHAPITRE J — INVENTAIRE COMPLET COMMANDS (61 commands)

Repertoire : `~/.claude/commands/*.md`
Les commands sont les precurseurs des skills.
Quand un doublon existe, la version skills/ est prioritaire (rule 35).

## J.1 — Commands Jarvis/Sysadmin (12)

| Command | Description |
|---------|-------------|
| jarvis-status | Etat de J.A.R.V.I.S. |
| jarvis-deploy | Deploiement Jarvis |
| jarvis-fix | Reparation Jarvis |
| jarvis-diag | Diagnostic Jarvis |
| jarvis-watch | Surveillance Jarvis |
| tunnel-check | Verifier tunnel SSH BOKADOR-VPS |
| fail2ban-mgr | Gerer fail2ban |
| nginx-mgr | Gerer nginx |
| ssl-check | Verifier certificats SSL |
| vps-sec | Securite VPS |
| wsl-init | Initialiser WSL |
| update-sys | Mise a jour systeme securisee |

## J.2 — Commands Securite (7)

| Command | Description |
|---------|-------------|
| security | Assessment OWASP Top 10 |
| security-audit | Audit securite complet avec score |
| security-check | Check config rapide vs menaces |
| security-log | Journal securite JARVIS |
| update-threat-db | MAJ base de donnees menaces |
| sandbox-status | Statut sandbox natif |
| audit | Audit securite rapide machine |

## J.3 — Commands Developpement (12)

| Command | Description |
|---------|-------------|
| qa | QA testing diff-aware avec fix loop |
| investigate | Root-cause debugging systematique |
| ship | Verification pre-deploiement |
| canary | Post-deploy monitoring production |
| deploy-check | Checklist deploiement |
| deploy-staging | Checklist staging |
| generate-tests | Generation de tests |
| autoresearch | Boucle autonome amelioration metriques |
| explain | Explication code/concepts multi-niveaux |
| optimize | Analyse et optimisation performances |
| validate-changes | LLM-as-a-Judge avant commit |
| pr | Analyse + creation PR structuree |

## J.4 — Commands Code (7)

| Command | Description |
|---------|-------------|
| code-review | Code review |
| commit | Git add, commit Conventional Commits |
| smart-commit | Commit intelligent |
| refactor | Refactoring sans changer le comportement |
| fix | Diagnostiquer et corriger un bug |
| test | Ecrire ou lancer les tests |
| review | Code review complet |

## J.5 — Commands Memoire/Reporting (10)

| Command | Description |
|---------|-------------|
| standup | Resume etat actuel |
| memorize | Enregistre une lecon dans le journal |
| cloud-memory | Optimise les fichiers memoire |
| compact-report | Etat du contexte + recommandation |
| spend | Estimation consommation tokens/cout |
| disk-report | Rapport espace disque tous volumes |
| thoughts | Sauvegarder contexte prochaine session |
| backup-now | Backup immediat SHA256 |
| session-save | Save etat de session pour resume |
| catchup | Restaurer contexte apres /clear |

## J.6 — Commands Operations (6)

| Command | Description |
|---------|-------------|
| hotel | Taches hotel (Opera Cloud, night audit) |
| night-audit | Night audit |
| infra | Diagnostic rapide infra BOKADOR/VPS/Mac |
| clean-sys | Nettoyage systeme |
| process-mgr | Analyser processus et anomalies |
| alert | Commande alerte |

## J.7 — Commands Divers (7)

| Command | Description |
|---------|-------------|
| doc | Generer/mettre a jour documentation |
| plan | Planifier une feature/tache |
| research | Deep research technique |
| auto-learn | Analyse session + lecons |
| audit-agents-skills | Audit qualite agents/skills |
| audit-codebase | Audit sante codebase (7 categories) |
| diagnose | Troubleshooting interactif Claude Code |
| debug-error | Debug erreur |

---

# CHAPITRE K — INVENTAIRE COMPLET RULES (24 actives)

Repertoire : `~/.claude/rules/`
Toutes les rules sont `alwaysApply: true` sauf celles
avec `globs` (chargees a la demande).

## K.1 — Rules de securite (4)

| Rule | Contenu |
|------|---------|
| 00-doc-before-change | Documenter AVANT toute action |
| 01-no-delete-without-triple-backup | Triple sauvegarde AVANT suppression |
| 22-security-context | Jamais logger secrets, .env, cles |
| 26-process-kill-protocol | Justification + sauvegarde avant kill |

## K.2 — Rules Melvynx methode (6)

| Rule | Contenu |
|------|---------|
| 02-memory-architecture-melvynx | Schema 3 niveaux + repartition |
| 07-melvynx-checklist | Best practices contexte, MCP, teams |
| 30-bible-melvynx-reference | Consulter la Bible avant de demander |
| 31-workflows-melvynx | Tableau choix workflow par tache |
| 32-prompting-melvynx | Techniques prompting par contexte |
| 33-context-engineering-melvynx | Gestion contexte, Lost in Middle |

## K.3 — Rules operationnelles (8)

| Rule | Contenu |
|------|---------|
| 08-claude-code-commands | Commandes slash et raccourcis |
| 09-mac-specifics | Regles macOS specifiques |
| 10-melvynx-sync | Sync avec repo Melvynx |
| 11-auto-learning-live | Auto-apprentissage (40+ Learned) |
| 12-progress-tracking | Progression visible taches longues |
| 25-notification-format | Format alertes macOS |
| 27-progress-bars | Barres progression par outil |
| 35-commands-vs-skills | Priorite skills > commands |

## K.4 — Rules contextuelles (3)

| Rule | Globs/Contexte |
|------|---------------|
| 20-json-files | Fichiers *.json |
| 21-git-operations | Operations Git |
| 23-test-context | Ecriture de tests |

## K.5 — Rules infra et team (3)

| Rule | Contenu |
|------|---------|
| 24-bbox-nat-infrastructure | 12 regles NAT, IPs, connexions |
| 34-team-agent-swarm | Swarm obligatoire taches non-triviales |
| archived/00-document-before-modify | Version archivee de regle #0 |

---

# CHAPITRE L — CLAUDE.MD REEL DEPLOYE

## L.1 — CLAUDE.md global (`~/.claude/CLAUDE.md`)

Contenu principal :
- Identite : Night Auditor & SysAdmin, Mercure H0373
- Infrastructure : MacBook M4, BOKADOR, VPS Kali
- Langue : francais sauf si anglais
- Regles absolues (14 CRITICAL)
- IPs de confiance (7 plages)
- Ports SSH (3 machines)
- Index des commandes disponibles
- Protocole Melvynx Diamant
- Commandes interdites (7)
- Bootstrap Jarvis (3 fichiers memoire)
- Preferences code (indent, quotes, commits)
- Workflow (lire 3 fichiers avant de modifier)
- Auto-apprentissage (5 triggers)

## L.2 — CLAUDE.md projet (`~/CLAUDE.md`)

Contenu principal :
- Permissions totales (lire, screenshots, tests, diags)
- Mode "God Mode" avec 6 regles :
  1. Anti-data loss (trash, pas rm, pas de suppression doublons)
  2. Git : Conventional Commits, git push INTERDIT
  3. No yapping : concis, direct, zero blabla
  4. Debugging : analyser avant de repeter
  5. Boucle de validation : auto-testing obligatoire
  6. Context engineering : lire `.ai/` avant de modifier
- Memoire persistante (`~/.claude/memory.json`)
- Identite : Soly Kim, email, telephone

## L.3 — Differences entre global et projet

| Aspect | CLAUDE.md global | CLAUDE.md projet |
|--------|-----------------|-----------------|
| Mode | Regles generales | God Mode permissions |
| Git | Regles Git generales | git push explicitement INTERDIT |
| Style | Protocole Melvynx | No yapping |
| Tests | TDD quand possible | Auto-testing OBLIGATOIRE |
| Context | Bootstrap Jarvis | Context .ai/ |

---

# CHAPITRE M — STATUSLINE ET OUTILS AVANCES

## M.1 — Statusline Melvynx

```json
"statusLine": {
  "type": "command",
  "command": "bun ~/.claude/scripts/statusline/src/index.ts",
  "padding": 0
}
```

Affiche en temps reel dans le terminal :
- Branche Git courante
- Tokens utilises / restants
- Cout estime de la session
- Pourcentage du contexte utilise

## M.2 — Command-Validator Melvynx

```
bun ~/.claude/scripts/command-validator/src/cli.ts
```

Validateur AST (Abstract Syntax Tree) ecrit en TypeScript/Bun.
Parse les commandes Bash et bloque les patterns dangereux
SANS faux positifs (contrairement a grep dans block-delete.sh).
Standard Melvynx, deploye en PreToolUse.

## M.3 — Variables d'environnement

```
ENABLE_LSP_TOOL=1     → Active Language Server Protocol
MAX_FILE_READ_LINES=5000 → Lecture jusqu'a 5000 lignes
```

---

# RESUME QUANTITATIF

| Categorie | Nombre |
|-----------|--------|
| Deny rules (settings.json) | 54 |
| Skills (skills/) | 50 |
| Agents (agents/) | 31 |
| Commands (commands/) | 61 |
| Hooks fichiers (hooks/) | 15 |
| Hooks actifs (settings.json) | 13 |
| MCP (local + cloud) | 5 |
| Plugins (officiels) | 9 |
| Rules (rules/) | 24 |
| Fichiers memoire (memory/) | 9+ |
| CLAUDE.md (global + projet) | 2 |

**Score de couverture : 100%**
Tous les elements de la config Mac reelle sont documentes.

---

# FIN — BIBLE_CONFIG_MAC_COMPLETE.md
# 13 chapitres (A-M) | ~700 lignes
# Date : 2026-03-31
