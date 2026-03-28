# CHANGELOG — Bible Melvynx Claude Code

## [V3.3] — 2026-03-28

### Completions finales (1489 → 1502 lignes)
- Alias YOLO mode : `alias cc="claude --dangerously-skip-permissions"` (Ch.3)
- MCP vs Skills distinction fondamentale : MCP = outils (tool calls), Skills = connaissances (prompts). Skills plus token-efficient. (Ch.3)
- Parler : app Melvynx speech-to-text open source, DMG disponible (Ch.11)

---

## [V3.2] — 2026-03-28

### Fusion massive : 36 sources Melvynx (1289 → 1489 lignes, +15%)

Fusion de 36 videos/transcriptions Melvynx dans la Bible V3. Toutes les sources du dossier `CLAUDE_CODE_MELVYNX_BIBLE/` analysees par 6 agents paralleles, dedupliquees et integrees.

### Contenu ajoute

#### APEX enrichi (Ch.4)
- Definition : modele cognitif / framework de raisonnement, implementation de ReAct (Reasoning and Acting)
- Explication "sous le capot" : interdiction de toucher au code pendant Analyze et Plan, SCRATCHPAD.md, micro-taches numerotees, diff + linter + tests dans eXamine
- Pourquoi APEX existe : modeles autoregressifs → instinct de cracher du code → APEX brise cette habitude
- workflow-apex-free : saute les sur-verifications (pas juste "bride les sous-agents")
- State Persistence : sauvegarde d'etat si quota epuise ou PC ferme, reprend au prochain lancement

#### Nouveaux chapitres
- **Ch.15 — Ralph** : boucle autonome infinie (PRD, prd.json, progress.txt, ralph.sh)
- **Ch.16 — 6 niveaux de maitrise** (Prompteur → Agents Autonomes H24)
- **Ch.17 — Skills tiers remarquables** (Top 10 : SEO, OWASP, Superpowers, Remotion, Canva Design)
- **Ch.18 — Erreurs en entreprise** (5 erreurs : permissions, multiagent, workflow, automatisation)
- **Ch.19 — Outils complementaires** (Conductor, Crab Review, CEMUX+Hium, cceva, Mistral Vibe)
- **Ch.20 — Usages non-code & configs avancees** (duti, hack token API, YOLO resume, hooks JSON complet, agents.md Teams, contextFork, install VPS OpenClaw 9 etapes, /config toggles, MCP syntaxe exacte)

#### Enrichissements chapitres existants
- Ch.3 : autocompact desactive, tool search, migration Context7 MCP→CLI, bypass VPS, statusline technique
- Ch.4 : /rewind, /remote control, thinking levels, feedback loop debug, Claude memory review 9 criteres
- Ch.5 : /compact, /resume, Tab thinking toggle, # memoire rapide
- Ch.6 : archivage agents agent-save/, ajout memoire #, /memory
- Ch.7 : Agent Swarm (delegate mode, modes execution, outils Teams), worktree sibling directory
- Ch.9 : plugins structure marketplace.json
- Ch.12 : threads OpenClaw, agent email/sponsors, anti prompt-injection, situation.md, claudebot security-audit, OC Pro
- Ch.13 : Skills V2 frontmatter, API-to-CLI, fusion commands/skills
- Ch.14 : Chrome automation, Tasks system, Cowork, Ask User Question

### Anti-patterns
- 20 → 25 regles (+5 : plugins 25, autocompact actif, 1 seul agent, OpenClaw devant ordi, tool search si MCP < 2-3%)

### Sources (36 fichiers)
Plugins, 5 Erreurs entreprise, 6 mois lecons, 500h, 1000h formation, Arrete MCP, Ralph, Updates devs, 100x mieux app, Chrome, Pour tout, TUE Startup, Remote Control, Rewind, Drama abonnement, Formation 2.0, Formation Avancee, Trompe Cursor, Setup Vibe Codeur x2, OpenClaw setup, Nouvelle feature, Cowork, 6 Niveaux, 10 Skills, Formation 1h, Mistral Vibe, OpenClaw Telegram, OpenClaw VRAI usage, Skills passer a cote, Skills revolution, Statusline, TUTO dev, TUTO 4h, Formation Complete

---

## [V3.1] — 2026-03-28

### Fusion Checklist Integrale (820 → 1289 lignes, +57%)

Fusion de la Checklist Integrale d'Instructions (22 sections, ~200 instructions cochables) dans la Bible.
Sources ajoutees : Masterclass 1h30 + Masterclass 4h + 6 Nouveautes + Terminal Vibe Coder.

### Nouveau fichier
- `BIBLE_MELVYNX_V3.md` — Bible V3.1 fusionnee (1289 lignes, 20 chapitres + anti-patterns)

### Chapitres restructures (20 → 20, reorganises)
- **Ch.2 — Installation & Prerequis** : enrichi avec commande `curl` install, guide WSL mlv.sh/fs, organisation ~/Developer/
- **Ch.3 — Le Terminal** : ajout analogie Minecraft, commandes npm, ouvrir terminal par OS
- **Ch.5 — Abonnement & Limites** : details reset sessions (~4.5/jour), attendre ~1h si limit
- **Ch.8 — Skills** : section Plugins vs Skills detaillee, front-end-design, github-init recommandes
- **Ch.9 — Workflows** : tableau choix workflow selon tache, flags /apex complets (--resume, --tasks, --interactive)
- **Ch.11 — Work Trees** : chapitre dedie (avant fusionne dans Sub-agents), alternative sans work tree
- **Ch.12 — Prompting** : NowTS boilerplate, rules ciblees par path avec exemple globs
- **Ch.13 — Securite** : securite dans CLAUDE.md (hash, idempotent, diagnostiquer avant agir)
- **Ch.14 — Deploiement** : chapitre dedie (Netlify Drop + Vercel + github-init)
- **Ch.15 — Nouveautes 2026** : CEMUX details (Swift/AppKit), Batch, Auto-Memory, Scheduled Tasks
- **Ch.17 — Tmux** : commandes installation (brew/apt)
- **Ch.18 — Multi-projets & Dossier CC** : symlinks skills, philosophie "Claude peut le faire ?"
- **Ch.20 — Hooks** : chapitre dedie avec exemple JSON complet settings.json
- **Ch.21 — WSL** : chapitre dedie Windows

### Meta-skills & meta-prompting
- Section enrichie : creer des skills qui creent des skills, app-prompting-creator, meta-prompts pour cloud memory/hooks/skills/sub-agents

### Anti-patterns
- 20 → 25 regles
- Ajout : JAMAIS ignorer erreurs, JAMAIS rm -rf, JAMAIS supprimer sans backup, JAMAIS >4 work trees, JAMAIS relancer skills inutilement

### Resume express (nouveau)
- Tableau de reference rapide : 14 composants avec instruction cle chacun

---

## [V2.0] — 2026-03-26

### Mise a jour majeure : Bible V2 (+56% de contenu, 527 → 820 lignes)

Analyse complete des 3 sources originales (4411 lignes) vs Bible V1 (527 lignes).
Identification de 25+ sections manquantes. Fusion integrale.

### Nouveau fichier
- `BIBLE_MELVYNX_V2.md` — Bible complete mise a jour (820 lignes, 14 chapitres + anti-patterns)

### Chapitres ajoutes
- **Chapitre 2 — Le Terminal pour Vibe Codeurs** (NOUVEAU)
  - Commandes essentielles (pwd, cd, ls, mkdir, echo, clear)
  - Bash vs PowerShell vs WSL
  - Astuce copie chemin macOS (clic droit + Option)
  - Premier projet demo (npm create vite)

- **Chapitre 14 — Nouveautes 2026** (NOUVEAU)
  - Claude Review (beta entreprise) : stats 16%→54% PRs, 94% bugs trouves, cout 15-25$/review
  - Voice Mode : `/voice`, mieux en anglais
  - Ask User Question ameliore : choix multiples, touche N pour notes

### Sections enrichies dans chapitres existants

#### Chapitre 1 — Architecture
- Applications creees par Melvynx (Umail, SaveIt, Subfast, Chao.App, PaddleTally)
- Analogie chef cuisinier

#### Chapitre 3 — Configuration
- Niveaux de configuration (0 → OK → OG 90% → OG++ 99%)
- Blueprint CLI Premium (`npx aiblueprint-cli pro activate`)
- settings.local.json (hooks locaux, Prettier, pas partage equipe)
- StatusLine : heures restantes, depenses du jour

#### Chapitre 4 — Workflows
- /oneshot : exploration et planification autonomes
- /debug : Chrome Headless juge "lourd et lent", log technique detaillee 4 etapes
- /fix-errors : ideal builds Swift
- /fix-grammar : usage avec chemin dossier
- /loop : cas d'usage (monitoring PR, babysit deploys)
- /ralph-loop : coding autonome en boucle
- /batch : citation Melvynx "pas de PR par petit bout de code"

#### Chapitre 5 — Commandes slash
- /init : conseil thread enrichi de contexte
- /plugin : 2 sources (Anthropic Cloud Code + Official Plugins)
- /voice : ne remplace pas outils dictee
- /debug : auto-debug config
- Escape x2 : naviguer fleches, Enter, "Restore conversation"
- Shift+Tab : progression Normal → Accept Edit → Bypass
- Ctrl+O : voir le thinking
- Ctrl+S : prompt revient automatiquement

#### Chapitre 6 — Context Engineering
- System prompt : ~3 500 tokens (1,7%)
- Outils, skills, MCP tools : definitions injectees meme si non utilises
- Proxyman detaille : intercepter requetes HTTP, hack fichier HTML
- Lost in the Middle : explication approfondie (biais transformer, 1/3 a 1/2)
- Impact concret sur EPCT
- Prompt Discovery : comment creer un workflow avec steps
- Rules : exemple concret (middleware.ts → proxy.ts)
- CLAUDE.md vs Auto-Memory : tableau comparatif
- Commentaires inline comme "prompt injection" benefique (ZodRoute, Upfetch)
- Changelog automatique (regle CRITICAL)
- Attention mauvais chats multi-terminaux
- Structure .claude/ : transcripts recuperables, hack lancer CC dans ~/.claude/
- Desactiver skills/agents : sous-dossier "disabled"

#### Chapitre 7 — Sub-agents & Teams
- Description sub-agent injectee dans le prompt
- Relancer CC apres creation sub-agent
- Creation agent custom live (/agent → create → personal → generate)
- Assigner modele (Sonnet pour economiser) et couleur
- Teams : fleche bas switcher agents, Tmux tabs, etat actuel (peu cooperatifs)
- Work Trees : ne PAS utiliser pour petites features, piege 25% temps travail
- Astuce CLAUDE.md erreurs TypeScript non implementees
- Citation Melvynx sur work trees

#### Chapitre 8 — Prompting
- Greenfield : NaotS.app boilerplate, preferences APRES premier run
- Brownfield : dessins fleches carres rouges, conventions code existant auto
- Debugging : screenshot + description exemple concret, Chrome Headless deconseille
- Technique 10 variations UI : prompt exact verbatim + exemples variations
- Processus iteratif front-end : vision visuelle = plus grand hack
- 5 methodes eviter erreurs repetitives (rules, workflow, globs, comments, changelog)
- Anti-patterns prompting complets

#### Chapitre 9 — Securite
- Erreur deny-list affichee
- Plugins : 2 sources, dossier non modifiable, citation "volez les concepts"
- Command Validator : privilege escalation, remote execution

#### Chapitre 10 — Pricing
- API directe Opus : 15$/M input + 75$/M output
- Recommandation detaillee Max 20x
- Sessions et limites : session limit vs weekly limit, reset, verifier sur claude.ai
- Quand monter d'abonnement (hobby → pro → intensif)

#### Chapitre 11 — Outils
- Netlify Drop : procedure complete 5 etapes
- Ghostty : raccourcis detailles
- CEMUX : specs completes (Swift/AppKit, notifications, browser, split pane)
- Raycast : extensions VS Code/Figma/GitHub/Tailwind/iOS
- WhisperTurbo : modeles PRK vs Whisper Large
- VS Code : installation CLI
- tmux : renommer session ($), cas d'usage par role
- Dossier CC : alias cdcc, liste utilisations

#### Chapitre 12 — OpenClaw
- Cas d'usage reels (piscine, tweets, plateformes)
- Installation locale detaillee (BotFather)
- Installation VPS manuelle (9 etapes completes avec commandes)
- Workflow complet 5 etapes avec cron job watcher
- Skills OpenClaw liste complete (16 skills)
- Gmail + Calendar (5 etapes Google Cloud OAuth)
- Audio bidirectionnel ElevenLabs
- Modeles : couts detailles ($2/$12 Gemini Pro)
- Fichiers config (soul.md, memory/, canvas/, .env)
- Securite : security-audit, 1Password warning, Mac Mini dedie
- OC Pro produit payant

#### Chapitre 13 — Meta-skills
- meta-hook-creator et meta-skill-workflow-creator
- Comment creer un meta-skill (4 etapes)
- Analogie livre de cuisine
- Repo GitHub officiel : 15 skills, 3 agents listes
- Melvynx prefere skill.sh aux plugins

### Anti-patterns
- 18 → 20 regles
- Ajout #19 : JAMAIS Chrome Headless pour debug
- Ajout #20 : JAMAIS attendre agents teams communiquent entre eux

### Configuration MacBook mise a jour
- Permissions Claude Code : allow simplifie (`Bash(*)`, `Edit(*)`, etc.)
- Deny-list Read maintenue (fichiers sensibles : .pem, password, secret, token, .env, .ssh)

---

## [V1.0] — 2026-03-26

### Creation initiale
- Bible Melvynx V1 : 527 lignes, 12 chapitres
- Fusion de 3 sources : Formation Complete (1420 lignes) + Setup 20min (451 lignes) + Cours 4h (2540 lignes)
- README.md : 17 sections structurees
- Bibles OS : macOS, Windows, Linux
- Bible DevOps/SysAdmin : 40 chapitres, 99 Ko
- Config BOKADOR : settings.json, rules, agents, docs
