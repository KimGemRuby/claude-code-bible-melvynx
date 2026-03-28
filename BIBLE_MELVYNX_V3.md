# BIBLE MELVYNX — Claude Code Masterclass Complete
> Source : 36 videos/transcriptions fusionnees (Formation Complete + Setup 20min + Cours 4h + 33 videos supplementaires)
> Date de compilation : 2026-03-28 — Version V3.1 fusionnee avec Checklist Integrale (20 chapitres, 25 anti-patterns)
> Usage : Reference absolue avant toute question ou doute

---

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
- Si ne marche pas : `/debug` pour auto-reparer

#### Technique interne StatusLine
- Lecture du fichier transcript JSON de Claude Code (genere automatiquement)
- Calcul tokens : input_token + cache_read_input_token + cache_creation_input_token
- Soustraire 45 000 tokens autocompact du max pour le contexte reellement disponible
- Recuperation usage API : `security find-generic-password -s "claude-code-credential"` puis fetch `api.anthropic.com/api/auth/usage`

### Hack bypass permission VPS
Claude refuse le bypass permission sur VPS. Hack :
```bash
SANDBOX=1 claude
# Persister dans .bashrc :
export SANDBOX=1
```

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
- **Feedback loop** : creer une API route de test, definir le resultat attendu, demander a Claude de faire curl en boucle jusqu'au bon resultat
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

## CHAPITRE 8 — PROMPTING TECHNIQUES

### Greenfield (nouveau projet)
1. Specifier la stack : "create a new vite project with shadcn and tailwind"
2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier la technique (camelCase etc.) au premier run — laisser l'IA choisir
5. Utiliser une boilerplate existante pour imposer les conventions (ex: **NowTS** de Melvynx avec regles strictes pour API routes, fetching, middleware)
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

---

## CHAPITRE 9 — SECURITE

### Modes de permission (Shift+Tab)
1. Normal : demande validation a chaque etape
2. Accept Edit : pas de validation pour les modifs fichiers
3. Plan : propose un plan, pas de modification
4. **Bypass Permission** (rouge) : aucune question — RECOMMANDE par Melvynx

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

---

## CHAPITRE 10 — PRICING & LIMITES

### Abonnements

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| **Max 20x** | **200$** | **~3 200$** | **x16 (meilleur ratio)** |

- API directe : Opus = 15$/M input + 75$/M output (peut monter tres vite)
- Melvynx depense ~3000$/mois en API equivalente pour 200$/mois
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
- **CEMUX** : terminal cree pour coding agents (4.7 stars). Build au-dessus de Ghostty. Tabs verticaux + horizontaux, notifications agent, browser integre (Safari), split pane, ports ouverts, PR en cours, branches git. Build en Swift/AppKit (pas Electron). Gratuit.
- **Hyper** : alternative populaire

### Outils macOS
- **Raycast** : remplacant de Spotlight (gratuit sauf IA 8$/mois). App launcher, clipboard history (texte + screenshots), emoji picker langage naturel, window management, screenshot, extensions (VS Code, Figma, GitHub, Tailwind, iOS)
- **CleanShotX** : screenshots annotes avec dessins, fleches, carres rouges (disponible dans SetApp)
- **SetApp** : bundle d'outils macOS incluant CleanShotX

### Speech-to-text
- **Voice Ink** : speech-to-text pour Mac (39$ one-time, alternative a Super Whisper 9$/mois)
- **WhisperTurbo** : app Melvynx, speech-to-text local gratuit et open source. Modele PRK (Parakeet) : rapide pour <20s. Modele Whisper Large : meilleur pour longs enregistrements et francais
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
npx skill add <auteur>/<nom>
# Sources : skill.sh, context7.com/skills
# Scope global : --scope user pour tous les projets
```
- Skills recommandes a installer : **front-end-design** (design professionnel), **skill-creator** (creer skills avec l'IA), **github-init** (init Git, commit, creation repo, push)

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
- Clean Code review : si fichier > 400 lignes → sub-agent clean code
- Boucle retroactive : tests → run → verifier → boucle si echec

### Erreur 4 : Un dev = un agent
- Un dev devrait monitorer **minimum 3 agents** en parallele
- Bon workflow = multiagent facile a monitorer

### Erreur 5 : Ne pas automatiser
- IA pour reviewer les PR (Replit, BugBot)
- 1 review IA + 1 review humaine (vision produit) suffit

---

## CHAPITRE 19 — OUTILS COMPLEMENTAIRES

### Conductor (orchestrateur worktrees)
- Automatise worktrees + agents paralleles + merge
- Separe automatiquement les worktrees dans `conductor/workspace/<projet>/<branche>/`
- Chaque agent travaille dans son propre worktree (pas de conflits fichiers)
- Merge automatique a la fin
- Alternative au pattern main agent + sub-agents pour les gros refactors
- Avis Melvynx : "Trop d'interface" — prefere le pattern main agent orchestrateur

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
| `canvas/index.html` | Interface web locale |
| `.env` | Variables d'environnement (tokens, API keys) |

### Claude Review — ROI entreprise
- Cout : **15-25$** par review (augmente avec complexite)
- Un ingenieur mid/senior coute **77-150$/h**
- ROI evident : une review IA de 25$ vs 1-2h de review humaine a 150$/h
- Combine avec review humaine : 1 review IA (clean code, bugs) + 1 review humaine (vision produit, taste)

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
