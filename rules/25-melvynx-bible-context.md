# Bible Melvynx — Gestion du Contexte

## Limites
- 200K tokens max par conversation
- ~27K tokens utilises des le depart (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- Autocompact a ~96-99% du contexte = degrade la qualite

## Regles CRITIQUES
- /clear OBLIGATOIRE entre taches majeures
- /context regulierement pour surveiller
- Si contexte > 80% : /clear immediat
- Petites modifs en langage naturel (economise le contexte)

## MCP et contexte
- Chaque MCP consomme du contexte MEME SANS ETRE UTILISE
- Maximum 2-3 MCP actifs
- Au-dela de 10% du contexte en MCP : Tool Search s'active

## MCP recommandes par Melvynx (les 2 seuls qu'il utilise)
1. **Context7** : documentation technique a jour, GRATUIT, pas de cle API
2. **Exa.ai** : recherche web optimisee IA, 20$ credit gratuit, ~6$ en 5 mois

## Sub-agents et contexte
- Sans sub-agents : 3 recherches de 28K = 84K tokens dans le contexte principal
- Avec sub-agents : consomment dans leur propre contexte, retournent 1000-1500 mots
- TOUJOURS utiliser des sub-agents pour les recherches

## Skills et contexte (token efficient)
- Petit skill : un fichier SKILL.md
- Grand skill : SKILL.md (index) + references/ + scripts/
- Un skill de 500 lignes en un fichier = tout charge = MAUVAIS
- Un skill avec index + sous-fichiers = seul l'index charge = BON

## Prompt Discovery (anti Lost-in-the-Middle)
Decouper le workflow en steps :
```
steps/
  step1-init.md
  step2-analyse.md
  step3-plan.md
  step4-execute.md
  step5-verify.md
```
Chaque step lit le suivant — le dernier prompt est toujours recent dans le contexte.
