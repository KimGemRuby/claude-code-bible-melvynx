# Fusion de la Bible Melvynx — Processus

## Sources fusionnées

| Fichier | Taille | Tokens | Contenu |
|---------|--------|--------|---------|
| `FORMATION_COMPLETE_MELVYNX.md` | 51 Ko | ~17K | Compilation de 16 fichiers / 14 283 lignes de transcriptions de 6 vidéos |
| `TUTO COURS Claude Code COMPLET.txt` | 248 Ko | ~71K | Transcription intégrale de la masterclass 4h |
| `Je setup Claude Code chez un Vibe Codeur.txt` | 29 Ko | ~12K | Transcription du setup chez Terrence |
| **TOTAL** | **328 Ko** | **~100K** | |

## Méthode de fusion

### Problème
Les 3 fichiers dépassent la limite de lecture directe (10K tokens max par lecture). Impossible de les lire séquentiellement sans exploser le contexte.

### Solution
3 sub-agents lancés en parallèle, chacun dédié à un fichier :

```
Main Agent (orchestrateur)
├── Agent 1 → FORMATION_COMPLETE_MELVYNX.md (17K tokens)
│   └── Lecture par blocs de 500 lignes (4 blocs)
├── Agent 2 → TUTO COURS 4h (71K tokens)
│   └── Lecture par blocs de 500 lignes (11 blocs)
└── Agent 3 → Setup Terrence (12K tokens)
    └── Lecture par blocs de 500 lignes (4 blocs)
```

### Résultats des agents

| Agent | Durée | Tokens consommés | Lectures |
|-------|-------|-----------------|----------|
| Agent 1 (Formation) | 3min 42s | 35 753 | 4 blocs |
| Agent 2 (Masterclass 4h) | 4min 44s | 95 399 | 11 blocs |
| Agent 3 (Setup Terrence) | 1min 48s | 27 523 | 4 blocs |

### Extraction structurée

Chaque agent a retourné une extraction en 8-10 catégories :
1. Configurations techniques (settings.json, CLAUDE.md, hooks)
2. Workflows et méthodes (/apex, /oneshot, /brainstorm)
3. Règles de sécurité et bonnes pratiques
4. Prompts exacts et commandes
5. Anti-patterns et erreurs à éviter
6. Skills, sub-agents et teams
7. MCP et gestion du contexte
8. Hooks
9. Prompting avancé
10. Enseignements divers

### Déduplication

Les 3 sources contiennent des informations qui se recoupent. La fusion a :
- Conservé les détails uniques de chaque source
- Priorisé les informations les plus précises (ex: flags exacts d'Apex)
- Éliminé les doublons tout en gardant les nuances

## Outputs de la fusion

| Output | Emplacement | Lignes |
|--------|-------------|--------|
| README.md (Bible fusionnée) | Repo GitHub | ~400 |
| 6 rules (21-26) | `~/.claude/rules/` | ~200 |
| 2 fichiers mémoire | `~/.claude/projects/.../memory/` | ~50 |
| MEMORY.md (index) | `~/.claude/projects/.../memory/` | 4 entrées |
