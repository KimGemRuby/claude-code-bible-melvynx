# Bible V5.0 — Documentation de la fusion

> Date : 2026-03-31
> Auteur : Kim (Soly Kim)
> De V4.1 (1717 lignes, 21 chapitres) a V5.0 (7057 lignes, 32 chapitres)

---

## Processus de fusion

### Etape 1 : Transcription des nouvelles videos
- 11 nouvelles videos Melvynx identifiees (non presentes dans V4.1)
- Transcription via faster-whisper (modele large-v3)
- Nettoyage automatique des transcriptions
- Fichiers stockes dans `/tmp/bible-transcripts/`

### Etape 2 : Analyse et extraction
- Lecture complete de chaque transcription
- Extraction des informations nouvelles (non presentes dans V4.1)
- Classification par chapitre thematique
- Identification de 3 guides complets a integrer

### Etape 3 : Fusion dans la Bible
- Chapitres 1-22 : V4.1 conservee integralement, enrichissements ajoutes
- Chapitres 23-29 : nouveaux chapitres issus des videos
- Chapitre 30 : Guide OpenClaw compile (documentation + transcriptions)
- Chapitre 31 : Guide Claude Code compile (cours 4h + toutes videos)
- Chapitre 32 : Config Mac reelle extraite de la machine Kim13

### Etape 4 : Verification
- Comptage lignes : 7057 confirmees
- Verification structure : 32 chapitres presents
- Verification sources : 70 sources referencees

---

## Sources nouvelles (Batch 3 — 11 videos)

| # | Video ID | Sujet principal | Chapitre |
|---|----------|-----------------|----------|
| 1 | K3VqFsUD2g8 | SaaS, copie, Subfast | Ch.24 |
| 2 | h4xp2BthaFo | Dispatch vs OpenClaw | Ch.27 |
| 3 | vamlWbDTWG4 | OpenClaw utilite reelle | Ch.27, Ch.30 |
| 4 | QG7aDk5om9I | CLI vs MCP, mort des MCP | Ch.23 |
| 5 | bGKljRcN_lE | MCP morts, CLI + Skill | Ch.23 |
| 6 | EI0JC0g3FwE | Setup OpenClaw complet | Ch.30 |
| 7 | zZqFVjiiGN8 | 4 niveaux developpeur IA | Ch.25 |
| 8 | sEifu9L0v1g | Dev iOS avec Claude Code | Ch.26 |
| 9 | 1eA8XD8OsAA | Vibe coder roadmap | Ch.26 |
| 10 | HZ15T5YQn5w | (source supplementaire) | Divers |
| 11 | lgg8MgEFVss | Masterclass 4h (mise a jour) | Ch.29, Ch.31 |

---

## Statistiques comparatives

| Metrique | V4.1 | V5.0 | Delta |
|----------|------|------|-------|
| Chapitres | 21 | 32 | +11 (+52%) |
| Lignes | 1717 | 7057 | +5340 (+311%) |
| Sources | 59 | 70 | +11 (+19%) |
| Sections estimees | 220+ | 400+ | +180 (+82%) |
| Taille fichier | ~85 Ko | ~350 Ko | +265 Ko |

### Repartition par partie
| Partie | Chapitres | Lignes approx |
|--------|-----------|---------------|
| Fondamentaux (1-11) | 11 | ~1050 |
| Avance (12-22) | 11 | ~930 |
| Nouvelles videos (23-29) | 7 | ~760 |
| Guide OpenClaw (30) | 1 | ~1233 |
| Guide Claude Code (31) | 1 | ~1963 |
| Config Mac Kim13 (32) | 1 | ~1121 |

---

## Nouveaux chapitres — Resume

### Ch.23 — CLI vs MCP : La nouvelle philosophie
Position Melvynx : les MCP sont "morts". Preferer CLI + Skill
(ex: Context7 en CLI au lieu de MCP). Raisons : tokens, fiabilite,
simplicite. Guide migration inclus.

### Ch.24 — SaaS et business avec Claude Code
Comment lancer un SaaS avec Claude Code. Cas Subfast (copies),
strategies de monetisation, stack recommandee, cout reel.

### Ch.25 — Les 4 niveaux de developpeur IA
Classification Melvynx des profils dev : debutant, intermediaire,
avance, expert. Competences attendues, roadmap progression.

### Ch.26 — Roadmap vibe coder 2026
Plan 6 mois pour devenir vibe coder. Stack, methode, objectifs
concrets. Interview Terence (dev iOS).

### Ch.27 — Dispatch vs OpenClaw
Comparatif des deux solutions d'agents distants. Dispatch = officiel
Anthropic (Cowork). OpenClaw = communautaire (Telegram). Cas d'usage.

### Ch.28 — CEMUX et outils recommandes
Setup complet CEMUX (terminal Claude Code). Raccourcis, split pane,
notifications, integrations avec tmux.

### Ch.29 — Masterclass : les 7 piliers (Core Seven)
Recapitulatif des 7 fonctionnalites cles de Claude Code selon
Melvynx : Commandes, Hooks, Memoire, MCP, StatusLine, Sub-agents,
Context Management.

### Ch.30 — Guide OpenClaw 100%
Guide complet de A a Z : prerequis, installation VPS, configuration,
daemon, 16 skills, Gmail/Calendar OAuth, audio ElevenLabs, securite,
modeles IA, troubleshooting. 1233 lignes.

### Ch.31 — Guide complet Claude Code
Reference technique exhaustive : architecture interne, boucle agent,
configuration, hooks, MCP, plugins, sub-agents, Teams, Cowork,
Dispatch, Channels, securite. 1963 lignes.

### Ch.32 — Configuration Mac Kim13 complete
Inventaire reel de la config deployee sur MacBook Pro M4 :
76 deny rules, 50 skills, 31 agents, 15 hooks, 5 MCP, 9 plugins,
61 commands, 24 rules, architecture memoire, statusline. 1121 lignes.

---

## Fichiers generes

| Fichier | Emplacement |
|---------|-------------|
| Bible V5.0 | `BIBLE_MELVYNX_V5.md` |
| README mis a jour | `README.md` |
| CHANGELOG mis a jour | `CHANGELOG.md` |
| Ce document | `docs/12-BIBLE-V5-FUSION.md` |
| Transcriptions | `source/transcriptions/` (existantes) |

---

## Compatibilite

- V5.0 est **retrocompatible** : les chapitres 1-22 sont identiques
  a V4.1, avec des enrichissements ajoutes (jamais de suppression)
- Les versions precedentes (V2 a V4.1) restent disponibles dans le repo
- La config Kim13 (Ch.32) est specifique au MacBook Pro M4 de Kim
  mais sert de reference pour toute config avancee
