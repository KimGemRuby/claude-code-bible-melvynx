# Règle : Anti-Patterns (Bible Melvynx)

## JAMAIS faire
- /batch → génère dizaines de PR, trop de review. Utiliser main agent orchestrateur
- Lancer Claude Code à la racine / → TOUJOURS dans un dossier projet
- Installer >3 MCP → dégradation qualité outputs
- Surcharger CLAUDE.md → déplacer dans .claude/rules/ si >200 lignes
- Créer skills manuellement → laisser Claude via CloudCodeGuide
- Dépendre des plugins sans contrôler le code → copier dans ses propres skills
- Ignorer les erreurs TypeScript non implémentées par soi → passer à la suite

## TOUJOURS faire
- /clear entre tâches majeures
- Lire 3 fichiers similaires avant de modifier un fichier
- Sub-agents pour les recherches (préserve contexte principal)
- Bypass permission + deny-list (double sécurité)
- Vérifier /context régulièrement
- Signal sonore fin de tâche (hook Stop)
