# Règle : Prompting (Bible Melvynx)

## Greenfield (nouveau projet)
- TOUJOURS préciser la stack (ex: "Vite React ShadCN Tailwind")
- Décrire la feature en langage naturel
- Séparer specs desktop et mobile
- Ne PAS spécifier technique sauf préférence forte
- Mettre préférences techniques dans CLAUDE.md APRÈS le premier run

## Brownfield (projet existant)
- L'IA suit automatiquement les conventions existantes
- Utiliser screenshots annotés pour le debug UI
- Référencer composants existants ("même style que X")

## Debug (ordre d'efficacité)
1. **Logs** (le plus efficace) : ajouter console.log → reproduire → copier logs bruts → envoyer
2. **Screenshots** : capturer le bug, l'IA résout souvent sans explication
3. **Chrome Headless** : lourd et lent, en dernier recours

## 10 Variations UI
Prompt : "Crée /demo/page.tsx avec 10 variations UI, 10 composants séparés"
Tester chaque variation, choisir la meilleure, supprimer /demo/

## Anti-patterns
- Ne PAS taguer fichiers avec Apex/OneShot → laisser l'exploration
- Ne PAS empiler les demandes pendant que l'IA travaille
- Ne PAS écrire des prompts parfaits : fautes OK, l'IA comprend
