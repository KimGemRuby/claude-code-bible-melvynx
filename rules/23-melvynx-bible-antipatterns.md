# Bible Melvynx — Anti-Patterns (INTERDITS)

## CRITICAL: Ne JAMAIS faire
1. Lancer Claude Code a la racine du systeme (/)
2. Surcharger CLAUDE.md avec infos inutiles — chaque info doit etre "fondamentalement utile"
3. Installer >3 MCP simultanement (degrade la qualite)
4. Bypass SANS deny-list configuree
5. Features complexes sans workflow (/apex, /oneshot) — l'IA saute des etapes
6. Dependre des plugins sans controler le code — copier dans ses propres skills/agents
7. Utiliser /batch pour les petites features (genere trop de PR)
8. Taguer des fichiers (@fichier) avec Apex/OneShot — laisser l'IA explorer
9. Empiler les demandes pendant que l'IA travaille — ca la perturbe
10. Lancer des sub-agents sur le meme fichier — ils se marchent dessus
11. Depasser 4 worktrees simultanes
12. Lancer des worktrees pour des petites features — uniquement pour les refactors structurels
13. Ignorer l'autocompact — /clear entre les taches
14. Relancer une commande/skill pour une petite modif — langage naturel suffit

## Lost in the Middle
- Les transformers donnent plus d'importance au debut et a la fin du contexte
- Les tokens du milieu sont largement ignores
- Solution : prompt discovery (multi-step) — decouper en steps, chaque step lit son fichier

## Plugins
- Ne PAS dependre des plugins officiels — ils s'auto-mettent a jour et ecrasent les modifs
- Copier le code interessant dans ses propres skills/agents
- "Installez, inspirez-vous, puis creez vos propres versions"
