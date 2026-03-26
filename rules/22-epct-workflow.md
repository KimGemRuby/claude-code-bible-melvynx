# Règle : Workflow EPCT Obligatoire

## Pattern
1. **E**xplore : lancer sub-agents (explore-codebase, explore-doc, web-search)
2. **P**lan : entrer en plan mode, planifier les modifications
3. **C**ode : exécuter le plan
4. **T**est : vérifier lint, tests, que tout fonctionne

## Application
- TOUTE feature non-triviale DOIT suivre EPCT
- /apex est la version avancée de EPCT avec flags modulables
- /oneshot pour les quick fixes (EPCT simplifié)
- JAMAIS coder sans explorer d'abord
