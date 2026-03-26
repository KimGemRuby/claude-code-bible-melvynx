---
name: Bible Melvynx - Référence absolue
description: La Bible Melvynx est LA référence pour toute question Claude Code. TOUJOURS consulter AVANT de poser une question.
type: reference
---

# Bible Melvynx — Référence Claude Code

**Fichier complet** : `C:\Users\kim13\Downloads\melvynx\CLAUDE_CODE_MELVYNX_BIBLE\FORMATION_COMPLETE_MELVYNX.md`
**Dossier transcriptions** : `C:\Users\kim13\Downloads\melvynx\`

## DIRECTIVE ABSOLUE

**AVANT de poser une question à Soly → chercher la réponse dans la Bible.**
- Si la Bible couvre le sujet → appliquer directement.
- Si la Bible ne couvre pas → alors seulement demander.

## Quick Reference (10 parties)

| Partie | Sujet | Enseignement clé |
|--------|-------|------------------|
| 1 | Fondamentaux | JAMAIS lancer Claude à la racine / |
| 2 | Mémoire | CLAUDE.md 3 niveaux, Rules ciblées globs, Auto-Memory à vérifier |
| 3 | Outils | Skills via CloudCodeGuide, Hooks async, MCP max 2-3, Sub-agents préservent contexte |
| 4 | Workflows | EPCT (Explore→Plan→Code→Test), Apex >99%, OneShot zéro question, /batch = anti-pattern |
| 5 | Contexte | 200K max, ~27K au départ, /clear obligatoire, Lost in the Middle → Prompt Discovery |
| 6 | Prompting | Greenfield=stack+feature, Brownfield=screenshots, Debug=logs>screenshots, 10 variations UI |
| 7 | Multi-agents | Teams zones SÉPARÉES, max 4 worktrees, main agent orchestre |
| 8 | Config | Bypass+deny-list, Plugins→copier le code, Blueprint CLI, Max 20x meilleur ratio |
| 9 | OpenClaw | Telegram distant, VPS Docker, SANDBOX=1, Skills>MCP |
| 10 | Infra | Adaptation BOKADOR spécifique |

## Règles Bible les plus importantes

1. **EPCT** : Explore → Plan → Code → Test. JAMAIS coder sans explorer.
2. **MCP max 2-3** : au-delà = dégradation. >10% contexte → Tool Search auto.
3. **Skills** : JAMAIS créer manuellement. Laisser Claude via CloudCodeGuide.
4. **Contexte** : /clear entre tâches. >80% → /clear immédiat. Sub-agents pour recherches.
5. **Plugins** : copier le code dans ses propres skills (contrôle total).
6. **Anti-pattern /batch** : préférer main agent orchestrateur.
7. **Debug** : logs > screenshots > Chrome Headless.
8. **Prompting** : ne PAS taguer fichiers avec Apex. Ne PAS empiler demandes.
9. **Bypass + Deny** : double sécurité (settings.json + CLAUDE.md trash).
10. **VPS** : SANDBOX=1 pour bypass. Docker pour sandboxing.
