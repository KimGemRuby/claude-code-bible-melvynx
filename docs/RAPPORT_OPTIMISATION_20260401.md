---
date: 2026-04-01
tags: [rapport, audit, optimisation, bible-melvynx, bible-jarvis, tokens, dictee, mcp, skills, context-engineering]
type: rapport
status: terminé
---

# Rapport Complet — Optimisation Claude Code
## MacBook Pro M4 Pro — Soly Kim — 2026-04-01

---

## 1. Dictée Vocale macOS (Neural Engine M4)

### Problème
La dictée Apple ne reconnaît pas les termes techniques Claude Code.

### Actions
- **207 remplacements de texte** dans GlobalPreferences.plist
  - team d'agent swarm : 16 variantes
  - GitHub : 10 variantes
  - Neural Engine : 14 variantes
  - Obsidian : 11 variantes
  - Termes techniques : 82 (Docker, TS, React, Nginx, etc.)
  - DevOps/Git/Web/BDD/Sécu : 46 (Kubernetes, JWT, firewall, etc.)
  - Outils Bible : 8 (Parler, WhisperTurbo, Voice Ink)
- **5 contacts fictifs** dans Contacts.app (vocabulaire)
- **30 contacts VCF** générés (à importer)
- **6 langues** optimisées : fr-FR, en-US, en-GB, fr-CA, fr-CH, fr-BE
- **Voice Commands** activées pour toutes les langues
- **2 scripts CLI** installés :
  - `/usr/local/bin/dictee-add "erreur" "correction"`
  - `/usr/local/bin/dictee-fix` (post-traitement presse-papier)

### Recommandation Bible (Ch.28)
- Installer **Parler** (Melvynx, gratuit, Whisper Large) — pas encore fait

---

## 2. Audit Tokens & Context Engineering

### Méthode
3 rounds de team agent swarm (9 agents au total) :
- Round 1 : Infra Checker + Bible Researcher + Reviewer
- Round 2 : Reviewer doublons + Infra MCP + Researcher JARVIS
- Round 3 : Vérification + Création skills

### Consommation au démarrage
- Rules always-on (9) : ~6 500 tokens
- CLAUDE.md global : ~1 900 tokens
- MEMORY.md : ~1 300 tokens
- Settings.json : ~1 600 tokens
- Skills/Commands/Agents : chargés à la demande (OK)

### Corrections appliquées

#### settings.local.json
- `autoCompact: true → false` (Bible Ch.3 : "JAMAIS actif")
- `autoSearchThreshold: 10` ajouté (manquait)
- `toolSearchMode: "auto"` confirmé

#### MCP Desktop (16 → 10 actifs)
6 désactivés avec `disabled: true` (sans suppression) :
- puppeteer, docker, kubernetes, postgres, ssh, everything

10 actifs restants :
- Tier 1 : context7, exa, memory, fetch
- Tier 2 : filesystem, applescript, git, sqlite, iterm, sequential-thinking

#### Rules archived (13 fichiers)
Toutes passées à `alwaysApply: false` — économie ~3000 tokens/session

#### Nouvelle rule
- `43-context-hygiene-bible.md` — 10 règles contexte propre

### 10 règles Context Hygiene (Bible JARVIS)
1. /compact à 50% — pas à 70%
2. 1 session = 1 tâche → /clear entre chaque
3. CLAUDE.md max 80 lignes global, 50 lignes projet
4. Max 2 MCP actifs (Context7 + Exa), reste disabled
5. Sub-agents pour recherches volumineuses (économise 28K tokens)
6. autoCompact: false — contrôle manuel uniquement
7. @fichier au lieu de "lis ce fichier"
8. Sonnet par défaut, Opus pour problèmes durs
9. thoughts/ pour mémoire inter-sessions
10. Logs : toujours filtrer avant de coller dans le prompt

---

## 3. Nouveaux Skills Créés

### /checkpoint
- Save session + recommande /clear + prompt reload
- Remplace le workflow manuel save→clear→reload

### /pre-delete
- Automatise la Règle #1 (triple backup)
- Étapes : justification → trash → knowledge graph → git commit
- Zones interdites : iCloud, settings.json, CLAUDE.md, BOKADOR

### /swarm-templates
5 templates prédéfinis de team agent swarm :
- swarm-debug : Explorer + Researcher + Fixer
- swarm-feature : Explorer + Implementer + Reviewer
- swarm-review : Code Reviewer + Security + Simplifier
- swarm-research : Web + Codebase + Docs
- swarm-infra : Diagnostic + Fix + Verify

---

## 4. Doublons Détectés (non corrigés, notés)

- commit skill vs smart-commit command
- code-review vs review commands
- fix vs debug-error commands
- thoughts vs session-save commands
- infra command vs infra-check skill

Rule 35 (skills > commands) s'applique mais pas encore nettoyé.

---

## 5. Score Bible Final

| Principe Melvynx | Score |
|---|---|
| Ultra Think | ✓ |
| CLAUDE.md centralisé (67 lignes) | ✓ |
| Commandes personnalisées | ✓ |
| Filtrer MCP (10 actifs, lazy-loaded) | ⚠ |
| StatusLine | ✓ |

**Score : 4.5/5** — seul écart : 10 MCP au lieu de 2-3 (mais lazy-loaded via autoSearchThreshold)

---

## 6. Fichiers Modifiés (inventaire complet)

| Fichier | Action |
|---|---|
| `~/.claude/settings.local.json` | autoCompact false, autoSearchThreshold 10 |
| `~/Library/Application Support/Claude/claude_desktop_config.json` | 6 MCP disabled |
| `~/Library/Preferences/.GlobalPreferences.plist` | 207 remplacements texte |
| `~/Library/Preferences/com.apple.assistant.support.plist` | 6 langues optimisées |
| `~/.claude/rules/43-context-hygiene-bible.md` | Créé |
| `~/.claude/rules/archived/*.md` (13 fichiers) | alwaysApply: false |
| `~/.claude/skills/checkpoint/SKILL.md` | Créé |
| `~/.claude/skills/pre-delete/SKILL.md` | Créé |
| `~/.claude/skills/swarm-templates/SKILL.md` | Créé |
| `/usr/local/bin/dictee-add` | Créé |
| `/usr/local/bin/dictee-fix` | Créé |
| Contacts.app | 5 contacts + 30 VCF |

---

## 7. À Faire (prochaines sessions)

- [ ] Installer Parler (Bible Ch.28, dictée Whisper Large)
- [ ] Installer WhisperTurbo
- [ ] Importer 30 contacts VCF
- [ ] Réduire MCP Desktop à 4 essentiels
- [ ] Résoudre autoCompact qui revient à true (identifier le processus)
- [ ] Nettoyer les 5 doublons skills/commands

---

*Rapport généré par 9 agents (3 rounds de team swarm)*
*MacBook Pro M4 Pro — Claude Code Opus 4.6 — Plan Max*
