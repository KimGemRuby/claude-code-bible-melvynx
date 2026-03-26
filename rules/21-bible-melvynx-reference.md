# Règle : Bible Melvynx = Référence Absolue

## DIRECTIVE
Avant TOUTE question à l'utilisateur, consulter la Bible Melvynx :
`C:\Users\kim13\Downloads\melvynx\CLAUDE_CODE_MELVYNX_BIBLE\FORMATION_COMPLETE_MELVYNX.md`

Si la Bible couvre le sujet → appliquer directement sans demander.

## Enseignements Bible (Quick Ref)

### Pattern EPCT (obligatoire)
Explore → Plan → Code → Test. JAMAIS coder sans explorer d'abord.

### Workflows
- /apex pour features complexes (>99% succès), flags: --auto --branch --pr --test
- /oneshot pour features rapides (zéro question)
- /debug : logs > screenshots > Chrome Headless
- /brainstorm : 4 rounds de réflexion profonde
- /simplify : 3 agents review en parallèle
- /loop : tâche récurrente (expire avec session)
- /schedule : cron persistant
- /batch : ANTI-PATTERN (trop de PR) → préférer main agent orchestrateur

### Sub-agents
Recherche web peut consommer 28K tokens → sub-agent fait la recherche dans son contexte et retourne 1000-1500 mots. Contexte principal préservé.

### Prompting
- Greenfield : préciser stack, décrire feature, séparer desktop/mobile
- Brownfield : screenshots annotés, référencer styles existants
- Ne PAS taguer fichiers avec Apex/OneShot → laisser exploration
- Ne PAS empiler demandes pendant que l'IA travaille
- 10 variations UI dans /demo/page.tsx pour choix design

### Plugins
Copier le code des plugins dans ses propres skills → contrôle total.

### Erreurs répétitives
- Si même erreur 2+ fois → créer rule dans .claude/rules/
- Avant de modifier un fichier → lire 3 fichiers similaires
- Commentaires inline dans helpers internes = prompt injection bénéfique
