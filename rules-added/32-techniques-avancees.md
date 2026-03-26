# Bible Melvynx — Techniques Avancees

## Lancer Claude Code dans ~/.claude/
Melvynx lance Claude Code directement dans `~/.claude/` pour modifier/creer sa propre config.
L'agent a alors acces direct a settings.json, rules, skills, agents.

## Plans sauvegardes
Tous les plans crees en plan mode sont sauvegardes dans `~/.claude/plans/`.
Recuperables et reutilisables.

## Desactiver un skill ou agent
Deplacer le fichier/dossier dans un sous-dossier `disabled/` :
- `~/.claude/skills/disabled/mon-skill/`
- `~/.claude/agents/disabled/mon-agent.md`

## Raccourci N (note) dans Ask User Question
Quand Claude propose des choix multiples, selectionner une option et appuyer sur `N` pour ajouter une note avant d'envoyer.

## Debug du contexte (Proxyman / Fiddler)
Intercepter les requetes HTTP de Claude Code pour voir exactement ce qui est envoye au modele :
- macOS : Proxyman
- Windows : Fiddler, mitmproxy
Permet de voir : system prompt, tools definitions, skills descriptions, memory files, MCP tools.

## Tmux (multi-agents visuels)
```bash
# Installation
brew install tmux        # macOS
apt install tmux         # Linux/WSL

# Raccourcis
tmux                     # nouvelle session
Ctrl+A C                 # nouveau terminal
Shift+fleches            # switcher entre windows
Ctrl+A S                 # split en deux
Ctrl+A D                 # detacher (quitter sans fermer)
tmux attach              # rattacher la session
```
Windows : utiliser Windows Terminal avec split panes, ou tmux via WSL.

## CEMUX (terminal pour agents IA)
Terminal construit au-dessus de Ghosty :
- Tabs verticaux + horizontaux = multitasking
- Notifications quand un agent a besoin d'attention
- Gratuit
- A evaluer pour Windows.

## WhisperTurbo (speech-to-text local)
App de Melvynx pour transcription locale :
- Modeles : Parakeet (rapide) et Whisper Large (precis)
- Gratuit, tourne sur GPU local
- BOKADOR a une RTX 3060 compatible

## Dossier CC (usage polyvalent)
Creer un dossier dedie pour tout faire avec Claude Code hors projets :
- Generateur de titres YouTube
- Slides de presentation
- Analyses business
- Reviews d'emails
- Scripts divers
Philosophie : "Est-ce que Claude Code peut le faire ? Si oui, pas besoin de SaaS."
Emplacement recommande : `C:\Users\kim13\cc\`
