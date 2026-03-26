# BIBLE CLAUDE CODE — Windows (Methode Melvynx)
> Adaptation complete pour Windows (natif + WSL)
> Source : Bible Melvynx macOS (3 formations fusionnees) adaptee pour l'ecosysteme Windows
> Basee sur la methode Melvynx (CodeLynx) — Masterclass 4h + Setup 20min + Formation Complete
> Date de compilation : 2026-03-26
> Auteur : Soly Kim (kim13karame.com)
> Usage : Reference absolue pour toute utilisation de Claude Code sur Windows
> Voir README.md pour la methode complete (workflows, prompting, context engineering)

---

## RÈGLE #0 — DOCUMENTER AVANT TOUTE MODIFICATION (PRIORITÉ MAXIMALE)

> **Cette règle prime sur TOUTES les autres. AUCUNE exception.**

AVANT toute modification, suppression ou écrasement de fichier, code, config ou donnée :

### Ordre obligatoire : DOCUMENTER → BACKUP → AGIR → VÉRIFIER

1. **IDENTIFIER** : quel fichier/dossier sera modifié, supprimé ou écrasé
2. **DOCUMENTER** : noter l'état actuel dans `%USERPROFILE%\.claude\logs\modifications.log` (ou `~/.claude/logs/` sous WSL)
3. **SNAPSHOT** : copier l'état actuel vers `%USERPROFILE%\.claude\backups\YYYYMMDD_HHMMSS\`
4. **COMMIT GITHUB** : `git commit -m "doc-guard: snapshot avant [action] — [fichier]"`
5. **AGIR** : seulement après les 4 étapes ci-dessus
6. **VÉRIFIER** : confirmer que l'action a produit le résultat attendu

### Zones critiques (protection renforcée Windows)
- `%USERPROFILE%\.claude\settings.json`
- `%USERPROFILE%\.claude\CLAUDE.md`
- `%USERPROFILE%\.claude\rules\`, `memory\`, `hooks\`, `skills\`
- Registre Windows (`HKLM`, `HKCU`) — backup registre obligatoire avant modification
- Disques H: et G: (BOKADOR) — INTOUCHABLES

---

## RÈGLE #1 — AUCUNE SUPPRESSION SANS TRIPLE SAUVEGARDE

> **Cet ordre prime sur TOUTE autre instruction. AUCUNE exception.**

### Étape 1 : JUSTIFICATION EXPLICITE
Avant de proposer ou exécuter une suppression :
- Expliquer CLAIREMENT pourquoi
- Décrire l'impact
- Proposer des alternatives (archivage, déplacement)
- Attendre la CONFIRMATION EXPLICITE de l'utilisateur

### Étape 2 : TRIPLE SAUVEGARDE OBLIGATOIRE

| # | Sauvegarde | Commande Windows | Vérification |
|---|-----------|-----------------|--------------|
| 1 | **Corbeille** | `Recycle fichier.txt` (PowerShell) ou `trash-put` (WSL) | Vérifier Corbeille |
| 2 | **Git backup** | `git commit -m "backup: [fichier] avant suppression"` | `git log --oneline -1` |
| 3 | **Changelog** | Ajouter entrée datée dans le changelog | Lire le changelog |

**SI UNE DES 3 CONDITIONS N'EST PAS REMPLIE → REFUSER LA SUPPRESSION**

### Sauvegarde automatique du système Windows
- **Points de restauration** : vérifier qu'ils sont actifs (`SystemPropertiesProtection`)
- **Config Claude** : `%USERPROFILE%\.claude\` doit être dans un repo git
- **Backup registre** : `reg export HKLM\SOFTWARE backup.reg` avant toute modification

### Commandes safe Windows
```powershell
# Option 1 : Module PowerShell Recycle (RECOMMANDÉ)
Install-Module -Name Recycle -Scope CurrentUser
Recycle fichier.txt

# Option 2 : trash-cli sous WSL
sudo apt install trash-cli
trash-put fichier.txt
```

### Ce qui est considéré comme "suppression"
- `del`, `del /f`, `rd /s /q`, `Remove-Item -Recurse -Force`
- `rm`, `rm -rf` (sous WSL)
- Écrasement d'un fichier existant
- `reg delete` (registre)
- `git clean`, `git reset --hard`
- Toute action irréversible

---

## TABLE DES MATIERES

1. [Installation](#chapitre-1--installation)
2. [Interface et Raccourcis](#chapitre-2--interface-et-raccourcis)
3. [Configuration](#chapitre-3--configuration)
4. [Memoire et Context Engineering](#chapitre-4--memoire-et-context-engineering)
5. [Settings et Permissions](#chapitre-5--settings-et-permissions)
6. [Securite](#chapitre-6--securite)
7. [Skills](#chapitre-7--skills)
8. [Hooks](#chapitre-8--hooks)
9. [MCP (Model Context Protocol)](#chapitre-9--mcp-model-context-protocol)
10. [Sub-Agents et Teams](#chapitre-10--sub-agents-et-teams)
11. [Workflows et Commandes](#chapitre-11--workflows-et-commandes)
12. [Contexte et Debug](#chapitre-12--contexte-et-debug)
13. [Prompting Techniques](#chapitre-13--prompting-techniques)
14. [Multi-Agents et Work Trees](#chapitre-14--multi-agents-et-work-trees)
15. [Pricing et Limites](#chapitre-15--pricing-et-limites)
16. [Outils Recommandes Windows](#chapitre-16--outils-recommandes-windows)
17. [StatusLine](#chapitre-17--statusline)
18. [Structure du dossier .claude](#chapitre-18--structure-du-dossier-claude)
19. [Plugins](#chapitre-19--plugins)
20. [OpenClaw (Agent distant Telegram)](#chapitre-20--openclaw-agent-distant-telegram)
21. [Anti-Patterns](#chapitre-21--anti-patterns)
22. [Niveaux de Configuration](#chapitre-22--niveaux-de-configuration)
23. [Nouveautes 2026](#chapitre-23--nouveautes-2026)
24. [Montage disques dans WSL](#chapitre-24--montage-disques-dans-wsl)
25. [Claude Desktop (Windows)](#chapitre-25--claude-desktop-windows)
26. [Troubleshooting Windows](#chapitre-26--troubleshooting-windows)

---

## CHAPITRE 1 — INSTALLATION

### Prerequis Windows
- **Windows 10** (build 19041+) ou **Windows 11**
- **Node.js 18+** installe (via winget, chocolatey ou installeur officiel)
- **npm** ou **pnpm** disponible dans le PATH
- **Git for Windows** installe (inclut Git Bash)

### Installation Node.js
```powershell
# Via winget (recommande)
winget install OpenJS.NodeJS.LTS

# Ou depuis nodejs.org
```

### Installation Bun (necessaire pour StatusLine et command-validator)
```powershell
# PowerShell
irm bun.sh/install.ps1 | iex

# Ou dans WSL
curl -fsSL https://bun.sh/install | bash
```

### Installation VS Code
```powershell
winget install Microsoft.VisualStudioCode
```
- Terminal integre : menu Terminal > New Terminal
- Ouvrir un projet : `code .` dans le terminal

### Installation via npm (PowerShell ou CMD)
```powershell
npm install -g @anthropic-ai/claude-code
```

### Installation via WSL (RECOMMANDE)
WSL (Windows Subsystem for Linux) est **FORTEMENT recommande** car Claude Code fonctionne nativement sous bash/zsh. Les hooks, scripts et outils de l'ecosysteme sont concus pour Linux/macOS.

```powershell
# 1. Installer WSL (PowerShell en admin)
wsl --install

# 2. Redemarrer Windows
# Choisir Ubuntu (par defaut), creer un utilisateur + mot de passe

# 3. Ouvrir Ubuntu (WSL) et installer Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# 4. Installer Claude Code dans WSL
npm install -g @anthropic-ai/claude-code

# 5. (Optionnel) Installer Bun pour la StatusLine et le command-validator
curl -fsSL https://bun.sh/install | bash
```

### Installation alternative via script officiel (WSL)
```bash
# Dans le terminal WSL
curl -fsSL https://claude.ai/install.sh | bash
```

### Installation via winget (PowerShell natif)
```powershell
winget install Anthropic.ClaudeCode
```

### Mettre a jour WSL de base
```bash
# Dans WSL
sudo apt update && sudo apt upgrade -y
```

### Verifier l'installation
```bash
claude --version
claude --help
```

### Premier lancement
```bash
# JAMAIS lancer a la racine du systeme — toujours dans un dossier projet
cd C:\Users\<user>\Projects\mon-projet   # PowerShell
cd ~/Projects/mon-projet                  # WSL

claude
```

### Choix du shell : PowerShell vs WSL

| Critere | PowerShell natif | WSL (bash/zsh) |
|---------|-----------------|----------------|
| Compatibilite hooks | Partielle | Totale |
| Scripts communaute | Adaptation requise | Natif |
| Performances | Bonnes | Excellentes |
| Acces fichiers Windows | Direct | Via /mnt/c/ |
| Outils Unix (grep, sed, awk) | Absents ou via Git Bash | Natifs |
| Bun/command-validator | Possible | Natif |

**Recommandation Melvynx** : utiliser WSL pour Claude Code. Tous les exemples, skills et hooks sont concus pour bash. PowerShell natif fonctionne mais necessite des adaptations.

### Acces aux fichiers Windows depuis WSL
```bash
# Disque C:
cd /mnt/c/Users/<user>/Documents

# Disque D:
cd /mnt/d/

# Depuis WSL, ouvrir un dossier dans l'Explorateur Windows
explorer.exe .
```

### Acces aux fichiers WSL depuis Windows
```
\\wsl$\Ubuntu\home\<user>\
```
Ou dans l'Explorateur : barre d'adresse > `\\wsl$`

### Apres installation
- Relancer le terminal si `claude` ne fonctionne pas
- Verifier : `claude` puis taper "qui suis-je ?"

### Regle fondamentale
**JAMAIS lancer Claude Code a la racine du systeme** (`C:\` ou `/`). Toujours `cd` dans un dossier projet d'abord.

---

## CHAPITRE 2 — INTERFACE ET RACCOURCIS

### Raccourcis clavier (Windows)

| Raccourci | Action |
|-----------|--------|
| Escape x1 | Efface le texte courant |
| Escape x2 | Acces historique conversations (naviguer avec fleches, Enter, "Restore conversation") |
| Shift+Tab | Changer mode permissions (Normal > Accept Edit > Bypass) |
| Ctrl+T | Taches en cours |
| Ctrl+O | Transcript verbose (voir le thinking de l'IA) |
| Ctrl+S | Sauvegarder prompt (question intermediaire puis prompt revient automatiquement) |
| **Alt+P** | Changer modele (**Alt** remplace **Cmd/Meta** sur Windows) |

### Prefixes dans le prompt

| Prefixe | Usage |
|---------|-------|
| `@` | Taguer un fichier (@header pour src/components/header) |
| `/` | Commandes slash (/apex, /clear, /context) |
| `!` | Mode bash direct (!pnpm install) |

### Terminal recommande : Windows Terminal + WSL

Windows Terminal est le terminal moderne de Microsoft. Il supporte les onglets, le split pane, les profils WSL, PowerShell, CMD.

```powershell
# Installer Windows Terminal (si absent)
winget install Microsoft.WindowsTerminal
```

**Configuration recommandee** :
1. Ouvrir Windows Terminal > Settings (Ctrl+,)
2. Default Profile > **Ubuntu** (WSL)
3. Appearance > Color Scheme > **One Half Dark** ou **Dracula**
4. Font > **Cascadia Code NF** (Nerd Font pour icones StatusLine)

### Alternatives terminaux Windows

| Terminal | Description | Recommandation |
|----------|-------------|----------------|
| **Windows Terminal** | Officiel Microsoft, gratuit, tabs, profiles WSL + PowerShell | **Meilleur choix** |
| **Alacritty** | Ultra rapide, GPU-accelere, config YAML | Excellent |
| **WezTerm** | Multiplexeur integre, Lua scriptable | Excellent |
| **VS Code Terminal** | Terminal integre a l'IDE | Pratique |
| **Hyper** | Electron-based, themable, extensions | Correct |
| **PowerShell 7** | Version moderne de PowerShell | Si pas WSL |
| **Git Bash** | Inclus avec Git for Windows, bash minimaliste | Minimal |
| **CMD** | Terminal classique Windows | Deconseille |

**Windows Terminal** : configurer le profil par defaut sur WSL/Ubuntu pour avoir Bash directement.

### Commandes de base (equivalences)

| Action | PowerShell | WSL (bash) | CMD |
|--------|-----------|------------|-----|
| Lister fichiers | `ls` ou `dir` | `ls` | `dir` |
| Chemin courant | `pwd` ou `$PWD` | `pwd` | `cd` (sans args) |
| Se deplacer | `cd chemin` | `cd chemin` | `cd chemin` |
| Creer dossier | `mkdir nom` | `mkdir nom` | `mkdir nom` |
| Ouvrir Explorateur | `explorer .` | `explorer.exe .` | `explorer .` |
| Ouvrir VS Code | `code .` | `code .` | `code .` |
| Nettoyer ecran | `cls` ou `clear` | `clear` | `cls` |

### Astuce copie chemin Windows
- **Explorateur** : Shift + Clic droit sur un fichier > "Copier en tant que chemin d'acces"
- **Barre d'adresse Explorateur** : cliquer dessus pour voir/copier le chemin complet
- **PowerShell** : `(Get-Item .\fichier).FullName`

---

## CHAPITRE 3 — CONFIGURATION

### Emplacement des fichiers de configuration

| Fichier | PowerShell natif | WSL |
|---------|-----------------|-----|
| settings.json | `%USERPROFILE%\.claude\settings.json` | `~/.claude/settings.json` |
| CLAUDE.md global | `%USERPROFILE%\.claude\CLAUDE.md` | `~/.claude/CLAUDE.md` |
| CLAUDE.md projet | `.\CLAUDE.md` (racine projet) | `./CLAUDE.md` |
| Rules | `%USERPROFILE%\.claude\rules\` | `~/.claude/rules/` |
| Skills | `%USERPROFILE%\.claude\skills\` | `~/.claude/skills/` |
| Agents | `%USERPROFILE%\.claude\agents\` | `~/.claude/agents/` |
| Memory auto | `%USERPROFILE%\.claude\projects\` | `~/.claude/projects/` |

**Note** : `%USERPROFILE%` = `C:\Users\<nom_utilisateur>` sous Windows.

### settings.json — Mode recommande (identique macOS)
```json
{
  "defaultMode": "bypassPermission",
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "toolSearchMode": "auto"
}
```

### settings.json — Exemple complet avec hooks et deny-list
```json
{
  "$schema": "https://claude.ai/schemas/claude-code-settings.json",
  "defaultMode": "bypassPermissions",
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "toolSearchMode": "auto",
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(del /f *)",
      "Bash(del /s /q *)",
      "Bash(sudo rm *)",
      "Bash(sudo rm -rf *)",
      "Bash(Remove-Item*-Recurse*-Force*)",
      "Bash(chmod 777 *)",
      "Bash(git push --force*)",
      "Bash(git push -f *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git reset --hard*)",
      "Bash(diskpart*)",
      "Bash(format *)",
      "Bash(*Clear-Disk*)",
      "Bash(mkfs*)",
      "Bash(dd if=*)",
      "Bash(curl *| bash*)",
      "Bash(curl *| sh*)",
      "Bash(wget *| bash*)"
    ]
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"if ($env:TOOL_INPUT -match 'diskpart|format|rm -rf|del /f|del /s|Remove-Item.*-Recurse.*-Force|Clear-Disk|mkfs|dd if=') { Write-Host 'BLOCKED BY HOOK'; exit 1 }\""
          }
        ]
      }
    ]
  }
}
```

### Notes Windows
- Le hook PreToolUse utilise **PowerShell** au lieu de bun/bash (plus natif sur Windows)
- Les sons de notification necessitent un script PowerShell specifique (voir section sons)
- `settings.local.json` pour configs locales non partagees

### Blueprint CLI (installation rapide)
```bash
# Depuis WSL (recommande)
npx ig-blueprint-cli

# Depuis PowerShell (peut necessiter des ajustements)
npx ig-blueprint-cli
```
Installe automatiquement : backup config, skills gratuits, command-validator, StatusLine, sons notification, permissions.

Si ca ne marche pas du premier coup : `/debug` pour auto-reparation.

### Premier projet demo (Windows)
```bash
# PowerShell ou WSL
npm create vite@latest mon-premier-projet
cd mon-premier-projet
npm install
npm run dev    # localhost:5173
```

---

## CHAPITRE 4 — MEMOIRE ET CONTEXT ENGINEERING

### Contexte en chiffres
- **200 000 tokens max** par conversation
- **~27 000 tokens utilises au demarrage** (system prompt + tools + skills + CLAUDE.md + rules + MCP)
- **Autocompact a ~96-99%** du contexte (degrade la qualite)
- **Sub-agent** = 120K tokens isoles, retourne ~1 500 mots

### Probleme "Lost in the Middle"
Les transformers privilegient le DEBUT et la FIN du contexte. Les tokens au milieu sont structurellement ignores. Quand le contexte grandit, les instructions initiales se retrouvent "au milieu" et perdent de l'influence.

### Solution : Prompt Discovery (Multi-step)
Decouper les skills en etapes au lieu d'un gros prompt :
```
steps/
  step1-init.md
  step2-analyse.md
  step3-plan.md
  step4-execute.md
  step5-verify.md
```
Chaque etape lit son fichier juste avant d'agir — le prompt courant est toujours "recent".

### 3 niveaux de memoire CLAUDE.md

| Niveau | Emplacement Windows | Portee |
|--------|---------------------|--------|
| Global | `%USERPROFILE%\.claude\CLAUDE.md` | Tous les projets |
| Projet | `.\CLAUDE.md` (racine projet) | Ce projet uniquement |
| Dossier | `.\src\components\CLAUDE.md` | Charge quand l'IA lit un fichier du dossier |

### Rules (.claude/rules/)
- Fichiers .md charges automatiquement a chaque session
- Par defaut `alwaysApply: true`
- Avec **globs** : charges uniquement pour certains types de fichiers
  ```yaml
  ---
  globs: ["*.json"]
  ---
  Regles specifiques aux fichiers JSON...
  ```
- Si CLAUDE.md depasse ~200 lignes, deplacer dans rules/
- Regle optimale : `CRITICAL: Never create middleware.ts — middleware is proxy helper in proxy utils`

### Auto-Memory (autoMemoryEnabled: true)
- Claude s'ecrit des notes entre sessions
- Stocke dans `~/.claude/projects/<hash>/memory/` (ou `%USERPROFILE%\.claude\projects\` sous Windows natif)
- 200 premieres lignes de MEMORY.md chargees a chaque session
- **Conseil** : preferer dire "modifie le CLAUDE.md avec cette info" plutot que laisser l'auto-memory sauvegarder n'importe quoi

### Gestion du contexte — Regles
- `/clear` OBLIGATOIRE entre les taches majeures
- `/context` regulierement pendant les longues sessions
- Si contexte > 80% : `/clear` immediat
- Langage naturel pour petites modifs (economise le contexte)
- Sub-agents pour les recherches (preservent le contexte principal)
- Maximum 2-3 MCP actifs
- Attention aux mauvais chats avec plusieurs terminaux ouverts

### Commentaires inline comme "prompt injection" benefique
Documenter les helpers avec exemples dans les commentaires du code. Avec la regle "lire 3 fichiers similaires", l'IA absorbe ces instructions automatiquement.

### Protocole de persistance mémoire (Règle #0 appliquée)

| Type d'information | Destination | Exemple |
|-------------------|-------------|---------|
| Règle comportementale | `.claude\rules\` | "JAMAIS del sans backup" |
| Architecture/décision | `.claude\memory\` | "Service X dépend de Y" |
| Erreur corrigée | `.claude\memory\02_lessons_log.md` | "Bug X résolu par Y" |
| Préférence utilisateur | `.claude\rules\11-auto-learning-live.md` | "Toujours français" |

### Sauvegarde de la mémoire
- `.claude\` DOIT être dans un repo git
- Commit automatique après chaque modification de rules\ ou memory\
- JAMAIS supprimer un fichier de mémoire sans triple sauvegarde (Règle #1)
- JAMAIS écraser un fichier de mémoire — toujours fusionner

---

## CHAPITRE 5 — SETTINGS ET PERMISSIONS

### Chemin du fichier settings.json

| Environnement | Chemin |
|---------------|--------|
| PowerShell natif | `C:\Users\<user>\.claude\settings.json` |
| WSL | `/home/<user>/.claude/settings.json` |
| Variable PowerShell | `$env:USERPROFILE\.claude\settings.json` |

### Deny-list Windows ETENDUE
La deny-list bloque les commandes dangereuses meme en mode Bypass Permission. Sur Windows, elle doit inclure les commandes destructives specifiques a l'OS :

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git push --force origin main*)",
      "Bash(dd if=*)",
      "Bash(mkfs*)",
      "Bash(del /f *)",
      "Bash(del /s /q *)",
      "Bash(rd /s /q *)",
      "Bash(rmdir /s /q *)",
      "Bash(diskpart*)",
      "Bash(format *)",
      "Bash(*Clear-Disk*)",
      "Bash(*Remove-Item -Recurse -Force*)",
      "Bash(*Remove-Partition*)",
      "Bash(*Initialize-Disk*)",
      "Bash(net stop *)",
      "Bash(sc delete *)",
      "Bash(bcdedit*)",
      "Bash(sfc *)",
      "Bash(DISM *)",
      "Bash(reg delete*)",
      "Bash(*-WmiObject*Win32_DiskPartition*)",
      "Bash(shutdown /r*)",
      "Bash(shutdown /s*)",
      "Bash(npm publish*)",
      "Bash(reboot*)",
      "Bash(netcfg -d*)",
      "Bash(curl *| bash*)",
      "Bash(curl *| sh*)",
      "Bash(wget *| bash*)",
      "Bash(git push -f *)",
      "Bash(git reset --hard*)",
      "Bash(sudo rm*)",
      "Bash(sudo rm -rf *)"
    ]
  }
}
```

### Commandes destructives Windows a bannir

| Commande | Effet | Alternative safe |
|----------|-------|------------------|
| `del /f /s /q` | Suppression forcee recursive | Corbeille / `recycle` |
| `rd /s /q` | Suppression dossier recursive | Corbeille |
| `diskpart` | Manipulation disques bas niveau | JAMAIS |
| `format` | Formatage disque | JAMAIS |
| `Remove-Item -Recurse -Force` | rm -rf en PowerShell | Corbeille |
| `bcdedit` | Boot configuration | JAMAIS sans backup |
| `reg delete` | Suppression registre | Backup registre avant |
| `net stop` | Arret service | JAMAIS sans justification |
| `npm publish` | Publication npm | JAMAIS depuis Claude Code |
| `shutdown` / `reboot` | Arret/redemarrage | JAMAIS sans confirmation |
| `netcfg -d` | Reset config reseau | JAMAIS (incident passe) |

### Double securite
- La deny-list bloque les commandes dangereuses meme en bypass
- CLAUDE.md contient "utiliser la Corbeille au lieu de del/rm"
- Les deux couches se completent

Ajouter dans votre CLAUDE.md :
```
- JAMAIS utiliser del /f ou del /s /q
- JAMAIS utiliser Remove-Item -Recurse -Force
- JAMAIS utiliser diskpart ou format
- JAMAIS rm -rf (meme dans WSL)
- Toujours utiliser la corbeille ou trash
```

### Modes de permission (Shift+Tab)
1. **Normal** : demande validation a chaque etape
2. **Accept Edit** : pas de validation pour les modifications fichiers
3. **Bypass Permission** (rouge) : aucune question — RECOMMANDE par Melvynx

---

## CHAPITRE 6 — SECURITE

### Commande safe pour supprimer : Corbeille Windows

Windows n'a pas de commande `trash` native en CLI. Plusieurs options :

#### Option 1 : Module PowerShell `Recycle` (recommande)
```powershell
# Installer le module
Install-Module -Name Recycle -Scope CurrentUser

# Utiliser
Remove-ItemSafely C:\chemin\vers\fichier.txt
```

#### Option 2 : Script PowerShell custom (sans module)
```powershell
# Ajouter dans $PROFILE (PowerShell)
function Trash {
    param([string]$Path)
    $shell = New-Object -ComObject Shell.Application
    $folder = $shell.Namespace(10)  # 10 = Corbeille
    $item = (Resolve-Path $Path).Path
    $folder.MoveHere($item)
    Write-Host "Deplace vers la Corbeille : $item"
}
```

#### Option 3 : `trash-cli` sous WSL
```bash
# WSL
sudo apt install trash-cli
trash-put fichier.txt
trash-list          # voir le contenu de la corbeille
trash-restore       # restaurer un fichier
```

#### Option 4 : `recycle-bin` via npm (cross-platform)
```bash
npm install -g recycle-bin-cli
recycle fichier.txt
```

### Hook PreToolUse — Version PowerShell
Pour valider les commandes avant execution (equivalent du command-validator macOS) :

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"$input = $env:TOOL_INPUT; if ($input -match 'diskpart|format\\s|del\\s/f|rd\\s/s|Remove-Item.*-Recurse.*-Force|rm\\s-rf|dd\\sif=|mkfs|bcdedit|reg\\sdelete') { Write-Output 'BLOCKED: Commande dangereuse detectee'; exit 1 } else { exit 0 }\""
          }
        ]
      }
    ]
  }
}
```

### Hook PreToolUse — Version WSL (RECOMMANDE)
Sous WSL, utiliser le command-validator Melvynx standard (identique macOS) :
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bun ~/.claude/scripts/command-validator/src/cli.ts"
          }
        ]
      }
    ]
  }
}
```

### Registre Windows — Protection
- JAMAIS modifier le registre (`reg add`, `reg delete`, `regedit`) sans backup prealable
- Backup registre : `reg export HKLM\SOFTWARE backup_registre.reg`
- Restauration : `reg import backup_registre.reg`

### Firewall Windows
- Ne JAMAIS desactiver Windows Defender Firewall sans justification ecrite
- `netsh advfirewall show allprofiles` pour verifier l'etat
- `netsh advfirewall set allprofiles state on` pour reactiver

### Permissions NTFS
- Ne JAMAIS utiliser `icacls * /grant Everyone:F /T` (equivalent chmod 777)
- Toujours verifier les permissions avant modification : `icacls fichier`
- Backup ACL avant changement : `icacls dossier /save acl_backup.txt /T`

### Astuce bypass VPS (identique macOS)
```bash
export SANDBOX=1  # dans .bashrc (WSL)
SANDBOX=1 claude  # Claude croit etre dans une sandbox, accepte le bypass
```

---

## CHAPITRE 7 — SKILLS

### Fonctionnement des skills (identique, universel)
- Skills = prompts reutilisables dans dossiers avec SKILL.md (entry point)
- Laisser Claude creer les skills (`/skill-creator`), ne PAS creer manuellement
- **Petit skill** : un seul fichier SKILL.md
- **Grand skill** : dossier avec SKILL.md + references/ + scripts/
- Seul l'index est charge, les sous-fichiers lus a la demande (token-efficient)

### Emplacement des skills

| Scope | Chemin PowerShell | Chemin WSL |
|-------|-------------------|------------|
| Global | `%USERPROFILE%\.claude\skills\` | `~/.claude/skills/` |
| Projet | `.\.claude\skills\` | `./.claude/skills/` |

### Installation de skills externes
```bash
npx skill add <auteur>/<nom>
```
Source : skill.sh (marketplace). Choisir "global" pour cross-projets.

### 15 skills Melvynx (repo officiel aiblueprint)
- apex, claude-memory, commit, create-pr, fix-errors, fix-grammar
- fix-pr-comments, merge, oneshot, prompt-creator, ralph-loop
- skill-creator, subagent-creator, ultrathink, workflow-apex-free

### Meta-skills (prompts qui creent des prompts)
- `/prompt-creator` : cree des prompts IA (best practices Anthropic + OpenAI + Google)
- `/cloud-memory` : gere CLAUDE.md et rules
- `/skill-creator` : cree des skills
- `/subagent-creator` : cree des sub-agents

### Desactiver un skill sans le supprimer
Deplacer dans un sous-dossier `disabled/` :
```bash
# WSL
mkdir -p ~/.claude/skills/disabled
mv ~/.claude/skills/mon-skill ~/.claude/skills/disabled/

# PowerShell
mkdir $env:USERPROFILE\.claude\skills\disabled -Force
Move-Item $env:USERPROFILE\.claude\skills\mon-skill $env:USERPROFILE\.claude\skills\disabled\
```

### Skill de sauvegarde automatique

```
# %USERPROFILE%\.claude\skills\auto-backup\SKILL.md  (ou ~/.claude/skills/ sous WSL)
---
name: auto-backup
description: Sauvegarde automatique de la config Claude et du système
---

## Fonctionnalités
1. Backup quotidien de .claude/ (settings, rules, memory, skills, agents)
2. Vérification Points de restauration Windows actifs
3. Snapshot avant toute modification de zone critique
4. Commit git automatique des changements de config

## Commandes
- `backup-claude` : sauvegarde complète .claude/
- `backup-check` : vérifie que toutes les sauvegardes sont à jour
- `backup-restore [date]` : restaure une config depuis un backup

## Automatisation Windows
- Planificateur de tâches Windows (quotidien)
- Point de restauration : vérifier avant toute opération risquée
- Backup registre automatique avant modifications
```

---

## CHAPITRE 8 — HOOKS

### 3 hooks recommandes Melvynx (adaptes Windows)

| Hook | Declencheur | Commande Windows | Usage |
|------|------------|------------------|-------|
| PreToolUse | Avant execution outil | `bun command-validator/src/cli.ts` (WSL) ou PowerShell validator | Valide les commandes, bloque les dangereuses |
| Stop | Fin de tache | Son systeme Windows | Savoir quand c'est fini |
| Notification | Claude a besoin d'attention | Son different du Stop | Distinguer "fini" vs "question" |
| PostToolUse (local) | Apres Edit/Write | Prettier automatique | Formater les fichiers edites |

### Son de fin de tache (hook Stop) — Windows

#### Option 1 : PowerShell SystemSounds
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"[System.Media.SystemSounds]::Asterisk.Play()\"",
            "async": true
          }
        ]
      }
    ]
  }
}
```

#### Option 2 : Fichier WAV personnalise
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"(New-Object Media.SoundPlayer 'C:\\Windows\\Media\\chimes.wav').PlaySync()\"",
            "async": true
          }
        ]
      }
    ]
  }
}
```

#### Option 3 : Depuis WSL (appeler PowerShell)
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell.exe -NoProfile -Command \"[System.Media.SystemSounds]::Asterisk.Play()\"",
            "async": true
          }
        ]
      }
    ]
  }
}
```

### Sons systeme Windows disponibles
```
C:\Windows\Media\chimes.wav        # classique, court
C:\Windows\Media\notify.wav        # notification
C:\Windows\Media\tada.wav          # succes
C:\Windows\Media\chord.wav         # accord
C:\Windows\Media\ding.wav          # ding
C:\Windows\Media\Alarm01.wav       # alarme
C:\Windows\Media\Ring01.wav        # sonnerie
C:\Windows\Media\Windows Notify.wav # notification Windows
```

### Hook Notification (son different du Stop)
```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"(New-Object Media.SoundPlayer 'C:\\Windows\\Media\\notify.wav').PlaySync()\"",
            "async": true
          }
        ]
      }
    ]
  }
}
```

### Configuration hooks avec sons distincts (Stop + Notification)
```json
"Stop": [{
  "hooks": [{
    "type": "command",
    "command": "powershell -NoProfile -Command \"(New-Object System.Media.SoundPlayer 'C:\\Windows\\Media\\tada.wav').Play()\"",
    "async": true
  }]
}],
"Notification": [{
  "hooks": [{
    "type": "command",
    "command": "powershell -NoProfile -Command \"(New-Object System.Media.SoundPlayer 'C:\\Windows\\Media\\notify.wav').Play()\"",
    "async": true
  }]
}]
```

### Toast Notification Windows (alerte visuelle)
```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell -NoProfile -Command \"[Windows.UI.Notifications.ToastNotificationManager, Windows.UI.Notifications, ContentType = WindowsRuntime] | Out-Null; $template = [Windows.UI.Notifications.ToastNotificationManager]::GetTemplateContent([Windows.UI.Notifications.ToastTemplateType]::ToastText02); $textNodes = $template.GetElementsByTagName('text'); $textNodes.Item(0).AppendChild($template.CreateTextNode('Claude Code')); $textNodes.Item(1).AppendChild($template.CreateTextNode('Tache terminee')); $toast = [Windows.UI.Notifications.ToastNotification]::new($template); [Windows.UI.Notifications.ToastNotificationManager]::CreateToastNotifier('Claude Code').Show($toast)\"",
            "async": true
          }
        ]
      }
    ]
  }
}
```

### Sons PowerShell disponibles (SystemSounds)
```powershell
# Tous les sons systeme disponibles
[System.Media.SystemSounds]::Asterisk.Play()
[System.Media.SystemSounds]::Beep.Play()
[System.Media.SystemSounds]::Exclamation.Play()
[System.Media.SystemSounds]::Hand.Play()
[System.Media.SystemSounds]::Question.Play()

# Son personnalise (WAV)
$player = New-Object System.Media.SoundPlayer "C:\Windows\Media\notify.wav"
$player.Play()
```

### Regles pour les hooks
- Hooks `async: true` pour les actions non-bloquantes (sons, notifications)
- Ne pas surcharger de hooks (ralentit l'execution)
- UN SEUL hook PreToolUse recommande : command-validator
- Les hooks lancent des processus — sur Windows, `powershell -NoProfile` pour eviter le chargement du profil (plus rapide)

---

## CHAPITRE 9 — MCP (Model Context Protocol)

### Regles MCP (identiques, universelles)
- **Maximum 2-3 MCP actifs** (STRICT) — au-dela, la qualite se degrade
- Si >10% du contexte est pris par les MCP, Tool Search s'active automatiquement
- Installation globale : `claude mcp add --scope user ...`
- Verification : `/mcp` dans Claude Code

### 2 MCP recommandes Melvynx

| MCP | Type | Usage | Cout |
|-----|------|-------|------|
| **Context7** | Documentation | Doc de n'importe quelle librairie, a jour | Gratuit (API key GitHub) |
| **Exa.ai** | Web Search | Recherche web optimisee pour l'IA | 20$ credit gratuit |

### Installation MCP sous Windows

#### Context7
```bash
# WSL ou PowerShell
claude mcp add --scope user context7 -- npx -y @upstash/context7-mcp
```

#### Exa.ai
```bash
claude mcp add --scope user exa -- npx -y exa-mcp-server
# Puis configurer la cle API dans les variables d'environnement
```

### Variables d'environnement MCP (Windows)
```powershell
# PowerShell (session courante)
$env:EXA_API_KEY = "votre-cle"

# PowerShell (permanent, user)
[Environment]::SetEnvironmentVariable("EXA_API_KEY", "votre-cle", "User")

# WSL (.bashrc)
echo 'export EXA_API_KEY="votre-cle"' >> ~/.bashrc
source ~/.bashrc
```

JAMAIS plus de 3 MCP simultanement.

---

## CHAPITRE 10 — SUB-AGENTS ET TEAMS

### Sub-agents — Protection du contexte (identique, universel)
- Une recherche web = ~28K tokens. 3 recherches = 84K (proche de la limite)
- Le sub-agent fait la recherche dans SON propre contexte (120K tokens) et retourne ~1 500 mots
- La description du sub-agent determine QUAND l'IA l'utilise
- Apres creation, relancer Claude Code pour qu'il le detecte

### Types de sub-agents

| Type | Usage | Vitesse |
|------|-------|---------|
| Explore | Recherche, exploration | Rapide |
| Task | Actions generiques | Normal |

### 3 sub-agents recommandes
1. **Web Search** : WebSearch + WebFetch + Exa MCP
2. **Explore Codebase** : Read + Grep + Search
3. **Explore Doc** : Context7 MCP

### Creation d'un agent custom
1. `/agent` > Create New Agent > Personal Folder > Generate with Cloud
2. Decrire la tache
3. Assigner un modele (Sonnet pour economiser)
4. Assigner une couleur

### Teams (Agent Teams)
- Activer : `enableAgentTeams: true` dans settings.json
- Le main agent (team lead) cree des equipes, assigne des taches via task list partagee
- Les teammates travaillent en parallele
- Interface : fleche bas pour switcher entre agents
- Les teams utilisent **tmux** (installer via WSL : `sudo apt install tmux`)
- **QUAND utiliser** : zones de modification SEPAREES (monorepo back/front/mobile)
- **NE PAS utiliser** : memes fichiers — les agents se "battent"

---

## CHAPITRE 11 — WORKFLOWS ET COMMANDES

### Principe EPCT (workflow de base)
1. **Explore** : lancer des sub-agents (explore-codebase, explore-doc, web-search)
2. **Plan** : entrer en plan mode, planifier les modifications
3. **Code** : executer le plan
4. **Test** : verifier lint, tests, que tout fonctionne

### /apex — Workflow principal (>99% succes)
LA commande principale. Phases :
1. Initialisation : lit fichier init, determine complexite
2. Exploration : lance sub-agents (1 a 3+ selon complexite)
3. Relecture contexte
4. Planification : plan d'execution
5. Execution : implemente le plan
6. Validation : valide sa propre tache
7. (Premium) Review securite + clean code + coherence

| Flag | Raccourci | Effet |
|------|-----------|-------|
| --auto | -A | Mode automatique, aucune question |
| --examine | -e | Active la review |
| --test | -t | Lance les tests |
| --economy | -$ | Evite les sub-agents (economise tokens) |
| --branch | -b | Cree une branche git |
| --pr | -p | Cree une Pull Request |
| --interactive | -i | Mode interactif (menu flags) |
| --teams | -m | Mode equipe multi-agents |
| --resume | -r | Reprendre une tache interrompue |
| --plan | | Plan mode |
| --save | | Save mode |

**Conseil** : ne PAS taguer de fichiers — laisser Apex explorer seul.

### /oneshot — Quick fix rapide
- Petites features, zero intervention, ne demande jamais l'avis
- Mode "vibe" : zero explication, zero tagage
- Cout tokens faible

### /debug — Fix erreurs structure
Workflow en steps (prompt discovery) :
1. Init > 2. Analyse > 3. Solutions > 4. Fix > 5. Verify

### /brainstorm — Recherche profonde (4 rounds)
1. Expansive Exploration > 2. Devil's Advocate > 3. Synthese > 4. Cristallisation
TRES gourmand en tokens.

### /fix-errors — Correction parallele
Lance des sub-agents en parallele (jusqu'a 23) pour corriger plusieurs erreurs.

### /fix-grammar — Grammaire en bulk
Analyse un dossier, lance des sub-agents par groupe de fichiers.

### /simplify — Review automatique (2026)
3 agents review en parallele. Corrige auto.

### /loop — Tache recurrente (2026)
Cron interne expirant avec la session : `/loop 2m check if PR tests pass`

### /schedule — Taches planifiees persistantes (2026)
Cron persistant entre sessions.

### /ultrathink — Reflexion profonde
Maximum de thinking tokens. 2+ minutes. Problemes complexes.

### Autres commandes
- `/commit` : commits simplifies
- `/create-pr` : PR automatique
- `/merge` : merge intelligent
- `/fix-pr-comments` : corriger commentaires PR
- `/load-memory` : optimise CLAUDE.md et rules
- `/prompt-creator` : cree des prompts optimises
- `/skill-creator` : cree des skills
- `/subagent-creator` : cree des sub-agents
- `/ralph-loop` : coding autonome en boucle
- `/review-code` : review multi-perspectives

### /batch — ANTI-PATTERN selon Melvynx
Genere des dizaines de PR a review. Preferer un main agent orchestrateur.

### Commandes slash natives

| Commande | Action |
|----------|--------|
| `/clear` | Reset contexte (OBLIGATOIRE entre taches) |
| `/init` | Generer CLAUDE.md du projet |
| `/context` | Utilisation contexte en % |
| `/usage` | Limites et consommation |
| `/model` | Changer modele |
| `/agent` | Creer/gerer agents |
| `/mcp` | Voir MCP connectes |
| `/plugin` | Installer plugins |
| `/stat` | Statistiques session |
| `/config` | Configuration interactive |
| `/voice` | Mode vocal (beta) |
| `/copy` | Copier derniere reponse en Markdown |

---

## CHAPITRE 12 — CONTEXTE ET DEBUG

### Debug du contexte — Outils Windows

#### Fiddler (gratuit, Windows natif)
- Telecharger : https://www.telerik.com/fiddler
- Intercepte les requetes HTTP de Claude Code
- Permet de voir exactement ce qui est envoye au modele
- Utile pour comprendre les ~27K tokens de demarrage
- **Configuration** : activer le HTTPS Decryption dans Tools > Options > HTTPS

#### Charles Proxy (Windows/Mac/Linux)
- Alternative payante a Fiddler
- Interface plus intuitive
- Meme usage : inspecter les requetes API de Claude Code

#### Proxyman (macOS seulement — pas dispo Windows)
- L'outil recommande par Melvynx est macOS only
- Sur Windows, utiliser **Fiddler** comme equivalent

#### Hack debug contexte
Demander a Claude de creer un fichier HTML qui affiche toutes les infos du contexte :
```
Cree un fichier HTML qui liste toutes les informations que tu as dans ton contexte actuel
```

### Debug configuration Claude Code
- `/debug` : auto-debug et auto-reparation
- La StatusLine et les scripts ont des tests integres
- Lancer Claude Code dans `~/.claude/` pour gerer sa propre config

---

## CHAPITRE 13 — PROMPTING TECHNIQUES

### Greenfield (nouveau projet)
1. Specifier la stack : "create a new vite project with shadcn and tailwind"
2. Decrire la feature en langage naturel (ce que l'utilisateur voit)
3. Separer desktop et mobile dans la description
4. Ne PAS specifier la technique (camelCase etc.) au premier run
5. Utiliser une boilerplate existante pour imposer les conventions
6. Preferences techniques dans CLAUDE.md APRES le premier run, pas avant

### Brownfield (projet existant)
1. **Screenshots annotes** — montrer exactement le probleme
2. Donner des references internes : "style comme illustration-picker"
3. Donner les ressources : chemins, images, URLs
4. Les screenshots = l'IA VOIT ce qu'elle fait

### Outils de capture d'ecran Windows

| Outil | Description | Prix |
|-------|-------------|------|
| **Snipping Tool** | Integre Windows 11, annotations basiques | Gratuit |
| **ShareX** | TRES puissant, annotations, OCR, GIF, upload auto | Gratuit, open source |
| **Greenshot** | Simple, efficace, annotations | Gratuit |
| **Lightshot** | Rapide, partage instantane | Gratuit |

**Recommandation** : **ShareX** est l'equivalent Windows de CleanShotX (macOS). Il supporte les annotations, les fleches, les carres rouges, les captures deferrees, les GIFs.

```powershell
# Installer ShareX
winget install ShareX.ShareX
```

### Raccourcis capture d'ecran Windows
- `Win + Shift + S` : Snipping Tool (zone, fenetre, plein ecran)
- `Print Screen` : capture plein ecran dans le presse-papiers
- `Alt + Print Screen` : capture fenetre active
- ShareX : `Ctrl + Print Screen` (configurable)

### Debugging (methode la plus efficace)
1. Demander a l'IA d'ajouter des `console.log` strategiques
2. Reproduire le bug soi-meme
3. Copier les logs **bruts** de la console
4. Envoyer a Claude — ne PAS interpreter, exposer le PROBLEME

### Technique des 10 variations UI
```
Cree un fichier /demo/page.tsx avec 10 variations UI/UX de cette interface,
10 composants separes dans 10 fichiers separes, importes dans la page.
```
Tester chaque variation, choisir la meilleure, supprimer le dossier `/demo/`.

### Anti-patterns prompting
- Ne PAS relancer un skill pour une petite modif — langage naturel
- Ne PAS empiler les demandes pendant que l'IA travaille
- Ne PAS utiliser Chrome Headless (lourd et lent selon Melvynx)

---

## CHAPITRE 14 — MULTI-AGENTS ET WORK TREES

### tmux sous Windows (via WSL)
tmux n'existe pas nativement sous Windows. L'installer via WSL :

```bash
# WSL
sudo apt install tmux
```

### Raccourcis tmux essentiels

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `Ctrl+A C` | Nouveau terminal (window) |
| `Shift+fleches` | Switcher entre windows |
| `Ctrl+A S` | Split en deux |
| `Ctrl+A ,` | Renommer la window |
| `Ctrl+A $` | Renommer la session |
| `Ctrl+A D` | Detacher (quitter sans fermer) |
| `tmux attach` / `tmux a` | Rattacher la session |

Cas d'usage : Un tab par role — CC1 (feature), CC2 (review), Server, etc.

### Alternative Windows native : Windows Terminal tabs
Windows Terminal supporte nativement les onglets et le split pane :
- `Ctrl+Shift+T` : nouvel onglet
- `Alt+Shift+D` : split pane (duplique le profil courant)
- `Alt+Shift+Plus` : split horizontal
- `Alt+Shift+Minus` : split vertical
- `Alt+Fleches` : naviguer entre panes
- `Ctrl+Shift+W` : fermer pane/onglet

### Note Windows Terminal
Windows Terminal supporte nativement les tabs et splits — tmux est optionnel si vous utilisez Windows Terminal.

### Work Trees
- Travailler sur des branches separees en parallele
- **Maximum 4 work trees simultanes**
- Reserver aux gros refactors structurels
- Ne PAS utiliser pour petites features
- Attention au merge si les work trees touchent des schemas differents

---

## CHAPITRE 15 — PRICING ET LIMITES

### Abonnements (identique, universel)

| Plan | Prix/mois | Equiv. API | Ratio |
|------|-----------|------------|-------|
| Pro | 20$ | ~160$ | x8 |
| Max 5x | 100$ | ~800$ | x8 |
| **Max 20x** | **200$** | **~3 200$** | **x16 (meilleur ratio)** |

- API directe : Opus = 15$/M input + 75$/M output
- **Recommandation** : Max 20x (200$/mois) pour usage pro

### Sessions et limites
- Sessions de **5 heures**, reset toutes les 5h (~4.5 sessions/jour)
- **Session limit** : pourcentage d'utilisation dans la session
- **Weekly limit** : limite hebdomadaire globale
- Verifier : claude.ai > Settings > Usage

### Quand monter d'abonnement
- **Pro (20$)** : usage hobby/fun, longs espaces entre sessions
- **Max 5x (100$)** : usage professionnel regulier
- **Max 20x (200$)** : usage intensif, plusieurs projets, freelance/CDI

---

## CHAPITRE 16 — OUTILS RECOMMANDES WINDOWS

### Stack complete Windows pour Claude Code

| Categorie | Outil | Description | Prix |
|-----------|-------|-------------|------|
| **Terminal** | Windows Terminal + WSL | Terminal moderne + Linux | Gratuit |
| **IDE** | VS Code | Editeur principal | Gratuit |
| **IDE alternatif** | Cursor | Fork VS Code avec IA integree | Freemium |
| **Screenshots** | ShareX | Annotations, GIF, OCR, upload | Gratuit |
| **Screenshots alt** | Snipping Tool | Integre Windows 11 | Gratuit |
| **App Launcher** | PowerToys Run | Equivalent Raycast/Spotlight | Gratuit |
| **Package Manager** | winget | Gestionnaire paquets Microsoft | Gratuit |
| **Package Manager alt** | Chocolatey | Gestionnaire paquets communautaire | Gratuit |
| **Package Manager alt** | Scoop | Gestionnaire minimaliste | Gratuit |
| **Proxy/Debug HTTP** | Fiddler | Intercepteur HTTP/HTTPS | Gratuit |
| **Proxy alt** | Charles Proxy | Intercepteur HTTP pro | Payant |
| **Multiplexeur** | tmux (via WSL) | Sessions terminaux persistantes | Gratuit |
| **Git GUI** | GitKraken / Fork | Interface graphique Git | Freemium |
| **File Manager** | Files App | Explorateur moderne open source | Gratuit |
| **Clipboard** | PowerToys (Advanced Paste) | Historique presse-papiers | Gratuit |
| **Window Manager** | PowerToys FancyZones | Tiling/snap avance | Gratuit |
| **Speech-to-Text** | WhisperTurbo (Melvynx) | Transcription locale, open source | Gratuit |

### Installation PowerToys (RECOMMANDE)
PowerToys est l'equivalent Windows de Raycast — gratuit, open source, par Microsoft.

```powershell
winget install Microsoft.PowerToys
```

Fonctionnalites cles :
- **PowerToys Run** (Alt+Espace) : lanceur d'applications rapide
- **FancyZones** : gestion avancee des fenetres
- **Advanced Paste** : historique presse-papiers intelligent
- **File Locksmith** : voir quel processus verrouille un fichier
- **Color Picker** (Win+Shift+C) : picker couleur
- **Text Extractor** (Win+Shift+T) : OCR ecran
- **Keyboard Manager** : remapper les touches
- **Always On Top** (Win+Ctrl+T) : fenetre toujours au premier plan

### Installation des outils essentiels
```powershell
# Package managers
winget install Git.Git
winget install Microsoft.WindowsTerminal
winget install Microsoft.PowerToys
winget install ShareX.ShareX
winget install Microsoft.VisualStudioCode

# WSL
wsl --install

# Node.js (via winget)
winget install OpenJS.NodeJS.LTS

# Bun (via WSL)
# curl -fsSL https://bun.sh/install | bash

# Git (depuis WSL)
# sudo apt install git
```

### Commandes utiles Windows

| Action | Commande |
|--------|----------|
| Ouvrir dossier dans Explorateur | `explorer .` |
| Ouvrir VS Code | `code .` |
| Copier chemin fichier | Shift + Clic droit > "Copier en tant que chemin" |
| Informations systeme | `systeminfo` |
| Processus en cours | `tasklist` ou `Get-Process` |
| Espace disque | `Get-PSDrive C` ou `wmic logicaldisk get size,freespace,caption` |
| Tuer un processus | `taskkill /F /PID <pid>` ou `Stop-Process -Id <pid>` |
| Variables env | `$env:VARIABLE` (PS) ou `echo %VARIABLE%` (CMD) |
| IP locale | `ipconfig` |
| Ports en ecoute | `netstat -ano` ou `Get-NetTCPConnection -State Listen` |

### Stack technique Melvynx (deploiement)
- **Supabase** : backend/BDD pour SaaS
- **Vercel** : deploiement production (Next.js)
- **Netlify Drop** : demos rapides (drag & drop dist/)

### Deploiement rapide avec Netlify Drop (identique)
1. Demander a Claude : "prepare un dossier pour deployer sur Netlify Drop"
2. Claude genere un dossier `dist/`
3. Aller sur `app.netlify.com/drop`
4. Drag & drop le dossier `dist/`
5. URL publique instantanee

### Dossier "CC" — Fourre-tout Claude Code
```bash
mkdir ~/cc
# Dans .bashrc ou .zshrc :
alias cdcc="cd ~/cc"
```
Pratique pour les projets temporaires et les tests rapides.

### Scripts PowerShell vs Bash
- **CLAUDE.md global** : preciser que les scripts doivent etre en PowerShell OU en Bash selon le contexte
- **CLAUDE.md projet** : preciser `Scripts PowerShell sur cette machine`
- Si WSL : Bash est recommande (meme workflows que macOS/Linux)

---

## CHAPITRE 17 — STATUSLINE

### Fonctionnement (identique, universel)
Affiche en permanence en bas du terminal :
- Dossier courant et repo git
- Modele utilise
- Cout de la session en $
- Tokens utilises et % contexte
- Graphique session/weekly limit
- Heures restantes
- Depenses du jour

### Prerequis Windows
- **Bun** installe (via WSL recommande)
- **Nerd Font** installee (Cascadia Code NF, FiraCode NF, JetBrains Mono NF)

### Installation Bun sur Windows

#### Via WSL (recommande)
```bash
curl -fsSL https://bun.sh/install | bash
source ~/.bashrc
bun --version
```

#### Via PowerShell (natif Windows)
```powershell
powershell -c "irm bun.sh/install.ps1 | iex"
```

### Installation Nerd Font
```powershell
# Via Scoop
scoop bucket add nerd-fonts
scoop install Cascadia-Code-NF

# Ou via winget
winget install --id=Nerd.Fonts.CascadiaCode
```

Puis configurer dans Windows Terminal : Settings > Profiles > Defaults > Appearance > Font face > "CaskaydiaCove Nerd Font"

### Installation StatusLine
```bash
# Prerequis : bun installe
bun --version

# Installe automatiquement par Blueprint CLI
npx ig-blueprint-cli
```

### Si la StatusLine ne marche pas
- Verifier que bun est installe dans le bon environnement (WSL ou Windows natif)
- `/debug` dans Claude Code pour auto-reparation

---

## CHAPITRE 18 — STRUCTURE DU DOSSIER .CLAUDE

### Emplacement selon l'installation

| Installation | Chemin |
|-------------|--------|
| WSL | `~/.claude/` (dans le filesystem WSL) |
| Windows natif | `C:\Users\<user>\.claude\` |
| Claude Desktop | `%APPDATA%\Claude\` |

### Arborescence complete (identique macOS/Linux)

```
%USERPROFILE%\.claude\          # ou ~/.claude/ sous WSL
├── CLAUDE.md                   # Memoire principale
├── settings.json               # Configuration globale
├── settings.local.json         # Config locale (non commitee)
├── rules/                      # Regles persistantes
│   ├── 01-security.md
│   ├── 02-memory.md
│   ├── 20-json.md              # Contextuelle (globs: *.json)
│   ├── 21-git.md               # Contextuelle (globs: git)
│   └── ...
├── skills/                     # Skills actifs
│   ├── apex/
│   ├── oneshot/
│   ├── commit/
│   ├── disabled/               # Skills desactives (deplacer ici)
│   └── ...
├── agents/                     # Agents custom
│   ├── explore-codebase/
│   ├── websearch/
│   └── ...
├── scripts/                    # Scripts d'automatisation
│   ├── command-validator/      # Hook PreToolUse (Bun/TS)
│   └── statusline/             # Barre d'etat (Bun)
├── projects/                   # Historique sessions (transcripts)
├── plans/                      # Plans generes en plan mode
├── commands/                   # Ancien nom des skills
├── teams/                      # Equipes (JSON)
├── tasks/                      # Taches executees
├── memory/                     # Fichiers memoire detailles
└── bible/                      # Reference (optionnel)
```

### Astuce : gerer sa propre config avec Claude
Lancer Claude Code dans le dossier `.claude/` pour gerer sa propre configuration :
```bash
cd ~/.claude
claude
# "Optimise mes rules/, verifie les doublons, consolide"
```

---

## CHAPITRE 19 — PLUGINS

### Fonctionnement (identique, universel)
- Deux sources : "Anthropic Cloud Code" et "Anthropic Cloud Official Plugins"
- Installation : `/plugin` dans Claude Code
- Se mettent a jour automatiquement

### ATTENTION — Recommandation Melvynx
- Les plugins installent du code non controle dans un dossier non modifiable
- Ils ecrasent vos modifications a chaque mise a jour
- **RECOMMANDATION** : copier le code dans vos propres skills/agents
- Citation Melvynx : "Volez les concepts, creez votre propre configuration"

### Plugins disponibles
Verifier avec `/plugin` dans Claude Code pour la liste a jour.

---

## CHAPITRE 20 — OPENCLAW (Agent distant Telegram)

### Concept
Controler son ordinateur a distance via Telegram (texte + audio). Lance des agents Claude Code en arriere-plan, notifie quand termine.

### Windows NON recommande pour installation locale
OpenClaw est concu pour tourner 24/7 sur une machine dediee. Windows n'est PAS ideal pour cela :
- Les mises a jour Windows forcent des redemarrages
- La gestion des processus en arriere-plan est moins fiable
- Docker Desktop Windows consomme plus de ressources que Docker Linux

### Recommandation : VPS Linux
Utiliser un VPS Linux (Hetzner CAX21/CAX31, ~7.59$/mois, ARM, IPv4 obligatoire).

### Installation VPS (Docker)
```bash
npx openclaw-vps-setup
```
Installe : Node.js, OpenClaw, Claude Code, Bun, Cloudflared, GCloud + options securite (UFW, Fail2Ban, SSH Hardening).

### Si vous DEVEZ installer sur Windows (WSL)
```bash
# Depuis WSL
# 1. Installer Docker Desktop for Windows avec backend WSL2
# 2. Dans WSL :
git clone <openclaw-repo>
cd openclaw
cp .env.example .env
# Configurer .env
docker compose build && docker compose up -d
```

### Alternative : Mac Mini dedie (~600 EUR)
Machine recommandee par Melvynx pour OpenClaw 24/7.

### Workflow depuis Telegram
1. Message/audio Telegram
2. Skill "code_task" : clone worktree > npm install > GitHub Issue > lance apex
3. Cron job (verifie si termine)
4. Notification Telegram avec lien PR
5. "Merge la PR et delete le worktree"

### Modeles recommandes OpenClaw

| Usage | Modele | Cout |
|-------|--------|------|
| Taches generales | Gemini 3 Pro | $2 input / $12 output |
| Cron jobs | Gemini 3 Flash | Moins cher |
| Agents de code | Claude Opus | Meilleure qualite |

---

## CHAPITRE 21 — ANTI-PATTERNS

### Liste definitive (20 regles universelles)

1. JAMAIS lancer Claude Code a la racine du systeme (`C:\` ou `/`)
2. JAMAIS surcharger CLAUDE.md (max ~200-500 lignes)
3. JAMAIS installer >3 MCP
4. JAMAIS bypass SANS deny-list configuree
5. JAMAIS features complexes sans workflow (/apex, /oneshot)
6. JAMAIS dependre des plugins sans controler le code
7. JAMAIS utiliser /batch (preferer main agent + sub-agents)
8. JAMAIS empiler les demandes pendant que l'IA travaille
9. JAMAIS taguer les fichiers avec Apex/OneShot — laisser l'exploration
10. JAMAIS utiliser Teams sur une seule zone de code
11. JAMAIS laisser l'auto-memory sauvegarder n'importe quoi
12. JAMAIS specifier la technique au premier run Greenfield
13. JAMAIS lancer plus de 4 terminaux/work trees simultanes
14. JAMAIS utiliser work trees pour de petites features
15. JAMAIS laisser les plugins controler votre config
16. JAMAIS relancer des skills pour petites modifs (langage naturel)
17. JAMAIS creer skills/hooks/agents manuellement — laisser Claude les creer
18. JAMAIS modifier settings.json manuellement — demander a Claude
19. JAMAIS utiliser Chrome Headless pour debug (lourd et lent)
20. JAMAIS attendre des agents teams qu'ils communiquent entre eux

### Anti-patterns specifiques Windows
21. JAMAIS utiliser `del /f /s /q` — utiliser la Corbeille
22. JAMAIS utiliser `Remove-Item -Recurse -Force` sans backup
23. JAMAIS modifier le registre sans export prealable
24. JAMAIS desactiver Windows Defender sans justification
25. JAMAIS utiliser `diskpart` ou `format` depuis Claude Code
26. JAMAIS lancer Claude Code depuis CMD (preferer PowerShell ou WSL)
27. JAMAIS ignorer les mises a jour Windows en pleine session longue — planifier les redemarrages

---

## CHAPITRE 22 — NIVEAUX DE CONFIGURATION

### Progression (identique, universel)

| Niveau | Description | Comment |
|--------|------------|---------|
| **0** | Aucune config | Claude Code brut |
| **OK** | Installation de base | `npm install -g @anthropic-ai/claude-code` |
| **OG (90%)** | Config gratuite Melvynx | `npx ig-blueprint-cli` |
| **OG++ (99%)** | Config premium Melvynx | Workflows avances, review securite |

### Checklist niveau OG pour Windows

- [ ] Claude Code installe (WSL recommande)
- [ ] Windows Terminal configure avec profil WSL par defaut
- [ ] Nerd Font installee (Cascadia Code NF)
- [ ] Bun installe (StatusLine + command-validator)
- [ ] `npx ig-blueprint-cli` execute
- [ ] settings.json : bypassPermission + deny-list Windows etendue
- [ ] CLAUDE.md global cree avec identite, stack, regles
- [ ] 2-3 rules dans `.claude/rules/`
- [ ] Hook Stop : son de fin de tache
- [ ] Hook PreToolUse : command-validator
- [ ] 2 MCP max : Context7 + Exa.ai
- [ ] 3 sub-agents : web-search, explore-codebase, explore-doc
- [ ] ShareX installe (screenshots annotes)
- [ ] PowerToys installe (PowerToys Run, FancyZones)
- [ ] Git configure (user, email, credential manager)
- [ ] Corbeille CLI configuree (Recycle module ou trash-cli WSL)

### Specifites Windows a configurer en plus

#### Git Credential Manager (eviter les prompts de mot de passe)
```bash
# WSL
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"
```

#### Fin de ligne (CRLF vs LF)
```bash
# Configurer Git pour utiliser LF (important pour WSL)
git config --global core.autocrlf input
```

#### VS Code : ouvrir depuis WSL
```bash
# Depuis WSL, ouvrir le dossier courant dans VS Code Windows
code .
# Necessite l'extension "Remote - WSL" dans VS Code
```

---

## CHAPITRE 23 — NOUVEAUTES 2026

### Claude Review (beta entreprise)
- Review automatique des PR avec process multi-agents en parallele
- PR avec reviews passent de 16% a 54%
- Moins de 1% des reviews marquees incorrectes
- 94% des bugs trouves sur les grandes PR
- Cout : 15-25$ par review
- Optimise pour les equipes, pas les individuels

### Voice Mode
- `/voice` dans Claude Code
- Fonctionne mieux en anglais
- Ne remplace PAS les outils de dictee natifs

### Ask User Question ameliore
- Peut proposer des UI avec exemples de code dans le terminal
- Choix multiples : selectionner une option et `N` pour ajouter une note

### /simplify (3 agents review)
3 agents review paralleles pour reuse, qualite, efficacite. Corrige auto.

### /loop (cron interne)
Cron expirant avec la session. `/loop 2m check if PR tests pass`

### /schedule (cron persistant)
Persiste entre sessions. "Tous les matins a 9h, clean Downloads"

### Outils 2026 notables
- **CEMUX** : terminal macOS cree pour coding agents (pas dispo Windows)
- **Ghostty** : terminal macOS/Linux (pas dispo Windows nativement)
- **WhisperTurbo** : speech-to-text local (Melvynx) — multiplateforme

---

## CHAPITRE 24 — MONTAGE DISQUES DANS WSL

### Probleme
Par defaut, WSL ne monte pas automatiquement tous les disques Windows. Claude Code dans WSL ne voit pas `D:\`, `E:\`, etc.

### Solution : montage automatique
Ajouter dans `/etc/wsl.conf` :
```ini
[automount]
enabled = true
root = /mnt/
options = "metadata,umask=22,fmask=11"
```

### Montage manuel
```bash
# Monter un disque specifique
sudo mkdir -p /mnt/d
sudo mount -t drvfs D: /mnt/d

# Acceder aux fichiers Windows
cd /mnt/c/Users/<votre-user>/Documents
```

### Acceder aux fichiers WSL depuis Windows
```
\\wsl$\Ubuntu\home\<user>\
```

### Verifier les montages
```bash
mount | grep drvfs
```

---

## CHAPITRE 25 — CLAUDE DESKTOP (WINDOWS)

### Difference Claude Desktop vs Claude Code
- **Claude Desktop** : application GUI, pour conversation et MCP visuels
- **Claude Code** : CLI terminal, pour le developpement et l'automatisation
- Les deux peuvent coexister sur la meme machine

### Configuration MCP pour Claude Desktop
Fichier : `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    },
    "fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch"]
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y", "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\<user>\\Documents",
        "C:\\Users\\<user>\\Downloads",
        "C:\\Users\\<user>\\Desktop"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<votre-token>"
      }
    }
  }
}
```

### MCP disponibles pour Claude Desktop

| MCP | Usage | Commande |
|-----|-------|----------|
| **memory** | Memoire persistante | `npx -y @modelcontextprotocol/server-memory` |
| **fetch** | Requetes HTTP | `uvx mcp-server-fetch` |
| **filesystem** | Acces fichiers locaux | `npx -y @modelcontextprotocol/server-filesystem` + chemins |
| **github** | Repos, issues, PR | `npx -y @modelcontextprotocol/server-github` + token |

### Chemin du fichier config
```
%APPDATA%\Claude\claude_desktop_config.json
```
En general : `C:\Users\<user>\AppData\Roaming\Claude\claude_desktop_config.json`

---

## CHAPITRE 26 — TROUBLESHOOTING WINDOWS

### Claude Code ne trouve pas la commande `claude`
```powershell
# Relancer le terminal
# Ou verifier le PATH :
echo $env:PATH | tr ';' '\n' | grep -i claude

# PowerShell natif
$env:PATH -split ';' | Where-Object { $_ -match 'claude' }
```

### WSL : commande claude introuvable
```bash
# Reinstaller dans WSL
npm install -g @anthropic-ai/claude-code
source ~/.bashrc

# Ou via script officiel
curl -fsSL https://claude.ai/install.sh | bash
source ~/.bashrc
```

### Hooks PowerShell ne fonctionnent pas
- Verifier la politique d'execution : `Get-ExecutionPolicy`
- Si `Restricted` : `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`

### Sons ne fonctionnent pas
- Verifier le volume Windows
- Tester : `powershell -NoProfile -Command "[System.Media.SystemSounds]::Asterisk.Play()"`

### WSL ne demarre pas
```powershell
# PowerShell en admin
wsl --update
wsl --shutdown
wsl
```

### Disques non montes dans WSL
```bash
# Verifier les montages
mount | grep drvfs
# Remonter si necessaire
sudo mount -t drvfs D: /mnt/d
```

### StatusLine ne s'affiche pas
- Verifier que bun est installe : `bun --version`
- Verifier que Nerd Font est configuree dans Windows Terminal
- Lancer `/debug` pour auto-reparation

### Git demande le mot de passe a chaque fois (WSL)
```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"
```

### Problemes de fin de ligne (CRLF)
```bash
git config --global core.autocrlf input
```

---

## RESUME — DIFFERENCES CLES WINDOWS VS MACOS

| Element | macOS | Windows |
|---------|-------|---------|
| Shell recommande | Terminal natif (zsh) | WSL (bash) > PowerShell |
| Terminal | Ghostty, CEMUX, iTerm2 | Windows Terminal |
| Meta key | Cmd | Alt |
| Changer modele | Meta+P | Alt+P |
| Trash CLI | `trash` (brew) | `Recycle` module PS ou `trash-cli` WSL |
| Son fin de tache | `afplay Glass.aiff` | `[SystemSounds]::Asterisk.Play()` |
| Package manager | Homebrew | winget, Chocolatey, Scoop |
| App launcher | Raycast | PowerToys Run |
| Screenshots | CleanShotX | ShareX |
| Proxy debug | Proxyman | Fiddler |
| tmux | Natif | Via WSL uniquement |
| Docker | Docker Desktop | Docker Desktop (backend WSL2) |
| Chemins config | `~/.claude/` | `%USERPROFILE%\.claude\` ou `~/.claude/` (WSL) |
| OpenClaw | Recommande (Mac Mini) | NON recommande, preferer VPS |
| Deny-list | rm -rf, sudo rm | + del /f, diskpart, format, rd /s /q |
| Hooks sons | afplay | powershell SoundPlayer |
| Claude Desktop config | `~/Library/Application Support/Claude/` | `%APPDATA%\Claude\` |

---

## ANNEXE A — TEMPLATE CLAUDE.MD WINDOWS

```markdown
# CLAUDE.md — [Nom du projet]

## Stack
- [Framework, langage, BDD, etc.]

## Commandes
- `npm run dev` : serveur dev
- `npm run build` : build production
- `npm test` : lancer les tests
- `npm run lint` : verification code

## Conventions
- 2 spaces indent (JS/TS/JSON), 4 spaces (Python)
- Single quotes, trailing commas, semicolons
- Conventional Commits (feat:, fix:, refactor:, test:)
- Lire 3 fichiers similaires avant de modifier un fichier
- Update changelog apres tout changement

## Regles
- CRITICAL: Utiliser la Corbeille Windows au lieu de del/rm
- CRITICAL: JAMAIS diskpart, format, Remove-Item -Recurse -Force
- CRITICAL: Backup avant toute operation risquee
- CRITICAL: Ne JAMAIS commencer un script PowerShell par # seul

## Tests
- Lancer les tests apres chaque modification
- Si test echoue, corriger avant de continuer
```

---

## ANNEXE B — TEMPLATE SETTINGS.JSON WINDOWS COMPLET

```json
{
  "defaultMode": "bypassPermission",
  "enableAgentTeams": true,
  "autoMemoryEnabled": true,
  "toolSearchMode": "auto",
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(sudo *)",
      "Bash(chmod 777 *)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(git push --force origin main*)",
      "Bash(dd if=*)",
      "Bash(mkfs*)",
      "Bash(del /f *)",
      "Bash(del /s /q *)",
      "Bash(rd /s /q *)",
      "Bash(rmdir /s /q *)",
      "Bash(diskpart*)",
      "Bash(format *)",
      "Bash(*Clear-Disk*)",
      "Bash(*Remove-Item -Recurse -Force*)",
      "Bash(*Remove-Partition*)",
      "Bash(*Initialize-Disk*)",
      "Bash(net stop *)",
      "Bash(sc delete *)",
      "Bash(bcdedit*)",
      "Bash(reg delete*)",
      "Bash(shutdown /r*)",
      "Bash(shutdown /s*)",
      "Bash(npm publish*)",
      "Bash(reboot*)",
      "Bash(netcfg -d*)",
      "Bash(curl *| bash*)",
      "Bash(curl *| sh*)",
      "Bash(wget *| bash*)",
      "Bash(git push -f *)",
      "Bash(git reset --hard*)",
      "Bash(sudo rm*)",
      "Bash(sudo rm -rf *)",
      "Bash(sfc *)",
      "Bash(DISM *)",
      "Bash(*-WmiObject*Win32_DiskPartition*)"
    ]
  },
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bun ~/.claude/scripts/command-validator/src/cli.ts"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell.exe -NoProfile -Command \"[System.Media.SystemSounds]::Asterisk.Play()\"",
            "async": true
          }
        ]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell.exe -NoProfile -Command \"(New-Object Media.SoundPlayer 'C:\\Windows\\Media\\notify.wav').PlaySync()\"",
            "async": true
          }
        ]
      }
    ]
  }
}
```

---

> **Fin de la Bible Claude Code Windows — Methode Melvynx**
> 23 chapitres principaux + 3 chapitres supplementaires + 2 annexes
> Fusion complete des versions LOCAL (23 sections) et REMOTE (16 sections)
> Toutes les sections universelles (skills, workflows, pricing, sub-agents) sont identiques a la version macOS
> Les specificites Windows (chemins, sons, deny-list, outils, WSL, Claude Desktop, montage disques) sont detaillees et pretes a l'emploi
