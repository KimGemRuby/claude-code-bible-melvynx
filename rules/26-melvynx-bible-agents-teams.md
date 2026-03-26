# Bible Melvynx — Sub-Agents et Teams

## Sub-agents recommandes
| Agent | Outils | Usage |
|-------|--------|-------|
| web-search | WebSearch, WebFetch, Exa | Recherche web rapide |
| explore-codebase | Read, Grep, Search | Explorer le code |
| doc-explorer | Context7 MCP | Documentation technique |

## Regles sub-agents
- Toujours utiliser des sub-agents pour les recherches (preserve le contexte principal)
- Maximum 4 agents simultanes
- Ne PAS lancer sur le meme fichier
- La description de l'agent determine quand l'IA l'utilise

## Teams (enableAgentTeams: true)
- Uniquement pour zones de modification SEPAREES (front, back, mobile)
- Ne PAS utiliser pour une seule zone
- Main agent = team lead, cree l'equipe avec task list partagee
- Les teammates travaillent independamment
- C'est encore "un peu bugge" — les agents communiquent surtout avec le team lead

## Work Trees
- Uniquement pour refactors structurels
- Pour petites features : plusieurs Claude Code dans le meme repo
- Si ils se marchent dessus, leur dire de resoudre
- Regle : "Si erreurs TypeScript pas implementees par toi, c'est pas ton probleme, passe a la suite"
- Max 4 worktrees simultanes

## Creation d'agents
Via /agent > Create New Agent > Personal Folder > Generate with Cloud
Ou via /subagent-creator

## Skills
- Lancer avec /nom-du-skill
- Acceptent des parametres ($ARGUMENTS)
- Installer depuis skill.sh : npx skill add auteur/nom
- Installer en global pour cross-projets
- Desactiver : deplacer dans sous-dossier disabled/
- JAMAIS creer manuellement — laisser Claude creer via CloudCodeGuide
