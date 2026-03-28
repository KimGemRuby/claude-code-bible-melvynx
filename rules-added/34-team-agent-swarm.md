---
alwaysApply: true
---

# Règle : Team Agent Swarm OBLIGATOIRE

## Ordre de kim13 (2026-03-28) — PRIORITÉ HAUTE

### Principe
Pour TOUTE tâche non-triviale (>1 étape), utiliser systématiquement une approche **team agent swarm** :
- Lancer plusieurs sub-agents en parallèle
- Chaque agent a une responsabilité précise
- Le main agent orchestre et vérifie

### Quand utiliser le swarm
- Feature dev : 1 agent explore + 1 agent code + 1 agent test
- Debugging : 1 agent logs + 1 agent fix + 1 agent verify
- Recherche : 2-3 agents web-search/explore en parallèle
- Review : 1 agent code-review + 1 agent security + 1 agent simplify
- Refactoring : 1 agent par zone de fichiers (SÉPARÉES)
- Sysadmin : 1 agent diagnostic + 1 agent fix + 1 agent verify

### Pattern obligatoire
```
Main Agent (team lead / orchestrateur)
  ├── Sub-agent 1 (tâche spécialisée A)
  ├── Sub-agent 2 (tâche spécialisée B)
  └── Sub-agent 3 (tâche spécialisée C)
Main Agent vérifie que tout fonctionne
```

### Règles du swarm
- Maximum 4 agents simultanés (Bible Melvynx)
- Agents sur des zones de fichiers SÉPARÉES (jamais les mêmes fichiers)
- `enableAgentTeams: true` dans settings.json (déjà activé)
- Utiliser `subagent_type` approprié : Explore, general-purpose, websearch, etc.
- Privilégier les agents background pour les tâches indépendantes
- Main agent = coordinateur, NE code PAS lui-même si possible

### Exceptions (pas de swarm)
- Tâche triviale (1 seul fichier, <3 lignes)
- Question simple / réponse factuelle
- Commande unique (git status, ls, etc.)

### Référence
Bible Melvynx Chapitre 7 — Sub-Agents & Teams
"Un dev devrait monitorer minimum 3 agents en parallèle"
