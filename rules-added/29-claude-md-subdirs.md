# Bible Melvynx — CLAUDE.md dans les sous-dossiers

## 3 niveaux de CLAUDE.md

| Niveau | Emplacement | Charge quand |
|--------|-------------|-------------|
| Global | `~/.claude/CLAUDE.md` | TOUJOURS |
| Projet | `./CLAUDE.md` (racine) | Quand on travaille dans ce projet |
| Sous-dossier | `./src/components/CLAUDE.md` | Quand l'IA lit un fichier de ce dossier |

## Usage des CLAUDE.md de sous-dossiers
- Mettre des regles specifiques a un dossier (ex: conventions de composants React)
- Ne charge que quand necessaire = economise le contexte
- Ideal pour les monorepos avec zones distinctes (front/back/mobile)

## Rules ciblees avec globs (alternative)
Creer des rules dans `~/.claude/rules/` avec un pattern en en-tete :
```markdown
---
path: "*.json"
---
Quand tu lis des fichiers JSON, fais ceci...
```

```markdown
---
path: "app/**/route.*"
---
CRITICAL: All API routes must use the helper function...
```
La rule ne se charge que quand l'IA lit un fichier correspondant au pattern.

## Conseil Melvynx
- `/init` dans un thread ou des recherches ont deja ete faites = CLAUDE.md plus riche
- Preferences techniques (camelCase, structure) dans CLAUDE.md APRES le premier run, pas avant
- "Synchronise les regles avec ce qu'on a actuellement" = commande a utiliser regulierement
