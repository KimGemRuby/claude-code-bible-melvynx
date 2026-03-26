# Rules Créées — Détail Complet

## Vue d'ensemble

7 fichiers de rules créés dans `~/.claude/rules/`, chargés automatiquement à chaque session Claude Code.

## Règle de sécurité fichiers

### `file-operation.md`
**Créée lors de la Phase 2 (Power Mode)**

Contenu :
- Interdiction de rm, rm -rf, rm -r, del, Remove-Item
- Obligation d'utiliser trash comme alternative
- S'applique sur TOUTES les machines
- Aucune exception même en mode autonome

Section doublons ajoutée après discussion sur les failles Melvynx :
- Ne JAMAIS supprimer un fichier identifié comme doublon par sa taille seule
- Vérification SHA-256 obligatoire (`certutil -hashfile` ou `sha256sum`)
- Workflow : hash → confirmer identité → validation utilisateur → trash

---

## Rules Bible Melvynx (21-26)

### `21-melvynx-bible-workflows.md`
**Thème : Workflows et commandes**

Contenu clé :
- Pattern EPCT : Explore > Plan > Code > Test
- Tableau de correspondance situation → commande (/apex, /oneshot, /debug, etc.)
- Tous les flags Apex détaillés (--auto, --examine, --test, --economy, etc.)
- Règle : petites modifs en langage naturel, pas de workflow

### `22-melvynx-bible-prompting.md`
**Thème : Techniques de prompting**

Contenu clé :
- Greenfield : préciser stack, décrire feature, séparer desktop/mobile
- Brownfield : screenshots annotés, référencer styles existants
- 10 variations UI (prompt exact fourni)
- Debug par logs (méthode la plus efficace selon Melvynx)
- Prompts vagues intentionnels pour liberté créative
- Commentaires comme prompt injection bénéfique
- Continuous Learning : créer une rule quand erreur 2+ fois

### `23-melvynx-bible-antipatterns.md`
**Thème : 14 anti-patterns interdits**

Les interdictions :
1. Claude à la racine du système
2. Surcharger CLAUDE.md
3. Plus de 3 MCP
4. Bypass sans deny list
5. Features complexes sans workflow
6. Dépendre des plugins
7. /batch pour petites features
8. Taguer fichiers avec Apex/OneShot
9. Empiler les demandes
10. Sub-agents sur le même fichier
11. Plus de 4 worktrees
12. Worktrees pour petites features
13. Ignorer l'autocompact
14. Relancer commande pour petite modif

Plus : explication du Lost in the Middle et solution prompt discovery

### `24-melvynx-bible-securite.md`
**Thème : Sécurité complète**

Contenu clé :
- Architecture 3 couches détaillée
- Liste complète des commandes à interdire
- Avertissement sur le hook Regex sous Windows
- Règles doublons/suppression avec SHA-256
- Sécurité VPS (UFW, Fail2Ban, SSH keys only)
- Astuce `SANDBOX=1 claude` pour bypass sur VPS

### `25-melvynx-bible-context.md`
**Thème : Gestion du contexte**

Contenu clé :
- Limites : 200K tokens max, ~27K au démarrage
- Autocompact à ~96-99% = dégradation qualité
- Règles : /clear entre tâches, /context pour surveiller
- MCP recommandés : Context7 (gratuit) + Exa.ai (20$ credit)
- Optimisation via sub-agents (préserve le contexte principal)
- Skills multi-fichiers = token efficient
- Prompt discovery anti Lost-in-the-Middle

### `26-melvynx-bible-agents-teams.md`
**Thème : Sub-agents, teams, skills**

Contenu clé :
- 3 sub-agents recommandés (web-search, explore-codebase, doc-explorer)
- Teams : uniquement pour zones séparées, max 4 worktrees
- Work trees : uniquement pour refactors structurels
- Skills : JAMAIS créer manuellement, laisser Claude créer
- Installation skills : `npx skill add auteur/nom`
