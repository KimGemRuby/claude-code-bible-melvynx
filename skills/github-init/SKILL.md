---
name: github-init
description: Initialise un repo Git, cree le .gitignore, fait le premier commit, cree le repo GitHub et push. Tout en une commande.
---

# GitHub Init

Tu dois initialiser un projet sur GitHub de zero.

## Steps

1. Verifier si git est deja initialise (`git rev-parse --git-dir`). Si oui, passer a l'etape 3.
2. `git init`
3. Creer un `.gitignore` adapte au projet (detecter le langage/framework)
4. `git add -A` (verifier qu'il n'y a pas de fichiers sensibles .env, .pem, etc.)
5. Premier commit avec message descriptif
6. Creer le repo GitHub avec `gh repo create <nom> --public --source=. --push`
7. Confirmer le push et retourner l'URL du repo

## Arguments
$ARGUMENTS sera le nom du repo. Si vide, utiliser le nom du dossier courant.

## Regles
- Ne JAMAIS commiter de fichiers sensibles (.env, credentials, tokens, .pem, .ssh)
- Toujours creer un .gitignore AVANT le premier commit
- Demander confirmation avant de push si le repo contient des fichiers inhabituels
