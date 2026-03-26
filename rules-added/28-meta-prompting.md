# Bible Melvynx — Meta-Prompting

## Concept
Le meta-prompting consiste a creer des prompts qui creent des prompts.
Melvynx utilise des skills dedies pour generer automatiquement des configs optimales.

## Meta-skills disponibles
- `/prompt-creator` : cree des prompts IA optimises (best practices Anthropic/OpenAI/Google)
- `/skill-creator` : cree des skills Claude Code
- `/subagent-creator` : cree des sub-agents specialises
- `/meta-hook-creator` : cree des hooks
- `/meta-skill-workflow-creator` : cree des workflows multi-etapes

## Quand utiliser le meta-prompting
- Quand une tache se repete 3+ fois : creer un skill
- Quand un workflow est valide : transformer en skill workflow
- Quand un agent manque : utiliser /subagent-creator
- Quand un prompt est complexe : utiliser /prompt-creator

## Philosophie
"Installez, inspirez-vous, puis creez vos propres versions."
Ne pas dependre de plugins ou skills externes — copier le code et l'adapter.
