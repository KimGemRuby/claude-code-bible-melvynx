# Bible Melvynx — Workflows Obligatoires

## Pattern EPCT (fondamental)
Tout workflow efficace suit : Explore > Plan > Code > Test
- JAMAIS coder sans explorer d'abord
- JAMAIS explorer sans plan ensuite

## Commandes par situation

| Situation | Commande | Flags |
|-----------|----------|-------|
| Feature complexe | `/apex` | `-a` (auto), `-b` (branch), `-p` (PR), `-t` (test) |
| Quick fix | `/oneshot` | Zero question, zero validation |
| Debug | `/debug` | Steps structurees (prompt discovery) |
| Recherche profonde | `/brainstorm` | 4 rounds (gourmand en tokens) |
| Review code | `/simplify` | 3 agents paralleles |
| Correction grammaire | `/fix-grammar <dossier>` | Sub-agents paralleles |
| Correction erreurs | `/fix-errors` | Sub-agents paralleles |
| Memoire | `/load-memory` | Optimise les tokens memoire |
| Commit | `/commit` | Commit simplifie |

## Regles d'usage
- Petites modifications : langage naturel direct (pas de workflow = economie tokens)
- JAMAIS relancer une commande pour une petite modif
- `/clear` OBLIGATOIRE entre taches majeures
- Ne PAS taguer de fichiers (@fichier) avec /apex ou /oneshot — laisser l'exploration les trouver
- Ne PAS ecrire des prompts parfaits — fautes d'orthographe OK, l'IA comprend

## Apex flags complets
- `--auto` (`-a`) : autonomie totale
- `--examine` (`-e`) : auto-review
- `--test` (`-t`) : lance les tests
- `--economy` (`-$`) : economise tokens
- `--branch` (`-b`) : cree branche git
- `--pr` (`-p`) : cree Pull Request
- `--interactive` (`-i`) : menu interactif
- `--teams` (`-m`) : multi-agents
- `--resume` (`-r`) : reprendre tache interrompue
- `--plan` : active plan mode
