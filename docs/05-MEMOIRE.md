# Système de Mémoire Configuré

## Emplacement
`C:\Users\kim13\.claude\projects\C--Windows-System32\memory\`

## Fichiers de mémoire créés

### MEMORY.md (index)
4 entrées pointant vers les fichiers détaillés :
```
- Power Mode Melvynx OG++ → feedback_power_mode_config.md
- settings.json reference → reference_settings_json.md
- Bible Melvynx reference → reference_melvynx_bible.md
- Consulter la Bible d'abord → feedback_bible_first.md
```

### feedback_power_mode_config.md
**Type : feedback**

Contient :
- Architecture 3 couches détaillée avec explications
- Deny list active (11 patterns listés)
- Avertissement sur le hook Regex sous Windows
- Règle comportementale trash + SHA-256
- Why + How to apply

### reference_settings_json.md
**Type : reference**

Contient :
- Contenu JSON exact du settings.json pour restauration
- Fichiers associés listés
- Procédure de restauration si corruption

### reference_melvynx_bible.md
**Type : reference**

Contient :
- Chemins des 3 fichiers source de la Bible
- Liste des 6 rules dérivées (21-26)
- URL du repo GitHub
- Directive : consulter les rules et sources AVANT de poser une question

### feedback_bible_first.md
**Type : feedback**

Contient :
- Directive explicite de consultation prioritaire
- Why : l'utilisateur a demandé que la Bible soit la référence
- How to apply : rules 21-26 → fichiers source → question en dernier recours

## Fonctionnement
- Les 200 premières lignes de MEMORY.md sont chargées automatiquement à chaque session
- Les fichiers de mémoire sont consultés quand pertinent
- La directive "Bible first" modifie le comportement par défaut de l'agent
