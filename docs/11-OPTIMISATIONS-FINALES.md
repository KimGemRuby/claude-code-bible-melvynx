# Optimisations Finales — 2026-03-26

## 1. Rules en double fusionnees

Les rules 21a, 22a, 23a, 24a etaient des versions courtes des rules 21-26.

**Action :**
- Contenu unique des doublons fusionne dans les rules principales (21, 22, 23)
- Doublons deplaces dans `~/.claude/rules/disabled/`
- Aucune information perdue

| Doublon | Fusionne dans | Statut |
|---------|--------------|--------|
| 21a-bible-melvynx-reference.md | 21-melvynx-bible-workflows.md | Disabled |
| 22a-epct-workflow.md | 21-melvynx-bible-workflows.md | Disabled |
| 23a-prompting-bible.md | 22-melvynx-bible-prompting.md | Disabled |
| 24a-anti-patterns-bible.md | 23-melvynx-bible-antipatterns.md | Disabled |

## 2. 9 plugins desactives

Melvynx recommande 0 plugins ("copier le code dans ses skills, ne pas dependre de plugins").

**Plugins desactives :**
- hookify
- commit-commands
- feature-dev
- code-review
- code-simplifier
- pr-review-toolkit
- claude-md-management
- claude-code-setup
- security-guidance

**Couverture par skills existants :**
| Plugin | Remplace par |
|--------|-------------|
| commit-commands | /commit, /smart-commit |
| feature-dev | /apex, /oneshot |
| code-review | /code-review, /clean-code |
| code-simplifier | /simplify (built-in) |
| pr-review-toolkit | /fix-pr |
| claude-md-management | /load-memory, /cloud-memory |
| claude-code-setup | /status, config manuelle |
| security-guidance | /audit, /master-audit |
| hookify | /meta-hook-creator |

## 3. MCP memory retire

**Raison :** Redondant avec `autoMemoryEnabled: true` (auto-memory natif de Claude Code).
Melvynx ne recommande que 2 MCP : Context7 + Exa.

**MCP actifs apres optimisation :** 2 (Context7 + Exa) = conforme Bible.

## 4. Cles API securisees

**Avant :** Cle API Exa en clair dans l'URL du MCP.
**Apres :** Cle deplacee dans `env.EXA_API_KEY`, referencee par `${EXA_API_KEY}` dans l'URL.

Note : le token GitHub reste dans `env.GITHUB_TOKEN` (deja dans la section env, pas expose dans une URL).

---

## Resultat

| Optimisation | Avant | Apres |
|-------------|-------|-------|
| Rules en double | 4 doublons actifs | 0 (fusionnes + disabled) |
| Plugins | 9 actifs | 0 actifs |
| MCP | 3 (memory + context7 + exa) | 2 (context7 + exa) |
| Cle API Exa | En clair dans URL | Variable d'environnement |
