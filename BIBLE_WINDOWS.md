# Bible Claude Code — Windows

> Guide complet pour configurer et utiliser Claude Code sur Windows 10/11 (natif + WSL)
> Basee sur la methode Melvynx (CodeLynx) — Masterclass 4h + Setup 20min + Formation Complete
> Voir README.md pour la methode complete (workflows, prompting, context engineering)

---

## Table des matieres

1. [Pre-requis](#pre-requis)
2. [Installation WSL](#installation-wsl)
3. [Installation Claude Code](#installation-claude-code)
4. [Terminaux recommandes](#terminaux-recommandes)
5. [Configuration settings.json (Windows)](#configuration-settingsjson-windows)
6. [Hooks Windows (PowerShell)](#hooks-windows-powershell)
7. [Sons et notifications](#sons-et-notifications)
8. [Deny-list Windows](#deny-list-windows)
9. [MCP recommandes](#mcp-recommandes)
10. [Status Line](#status-line)
11. [tmux sur WSL](#tmux-sur-wsl)
12. [Structure du dossier .claude/](#structure-du-dossier-claude)
13. [Montage disques dans WSL](#montage-disques-dans-wsl)
14. [Astuces Windows](#astuces-windows)
15. [Claude Desktop (Windows)](#claude-desktop-windows)
16. [Troubleshooting Windows](#troubleshooting-windows)

---

## Pre-requis

### Node.js
```powershell
# Via winget (recommande)
winget install OpenJS.NodeJS.LTS

# Ou depuis nodejs.org
```

### Bun (necessaire pour StatusLine et command-validator)
```powershell
# PowerShell
irm bun.sh/install.ps1 | iex

# Ou dans WSL
curl -fsSL https://bun.sh/install | bash
```

### VS Code
1. `winget install Microsoft.VisualStudioCode`
2. Terminal integre : menu Terminal > New Terminal
3. Ouvrir un projet : `code .` dans le terminal

---

## Installation WSL

### Pourquoi WSL ?
- Claude Code fonctionne nativement avec Bash
- PowerShell est different de Bash — WSL donne un vrai environnement Linux
- **Recommandation Melvynx** : sur Windows, installer WSL pour avoir Bash
- Tous les workflows, skills et hooks sont ecrits pour Bash

### Installation
```powershell
# PowerShell en admin
wsl --install

# Choisir Ubuntu (par defaut)
# Creer un utilisateur + mot de passe
```

### Configuration de base dans WSL
```bash
# Mettre a jour
sudo apt update && sudo apt upgrade -y

# Installer Node.js
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs

# Installer bun
curl -fsSL https://bun.sh/install | bash
```

---

## Installation Claude Code

### Via WSL (recommande)
```bash
# Dans le terminal WSL
curl -fsSL https://claude.ai/install.sh | bash
```

### Via PowerShell natif
```powershell
# Voir la commande exacte sur code.claude.com/doc/setup
# La doc officielle fournit la commande Windows dediee
```

### Apres installation
- Relancer le terminal si `claude` ne fonctionne pas
- Verifier : `claude` puis taper "qui suis-je ?"

### Regle fondamentale
**JAMAIS lancer Claude Code a la racine du systeme** (`C:\` ou `/`). Toujours `cd` dans un dossier projet d'abord.

---

## Terminaux recommandes

| Terminal | Description | Recommandation |
|----------|-------------|----------------|
| **Windows Terminal** | Terminal Microsoft officiel, tabs, profiles WSL + PowerShell | **★★★ Meilleur choix** |
| **VS Code Terminal** | Terminal integre a l'IDE | ★★ Pratique |
| **PowerShell 7** | Version moderne de PowerShell | ★★ Si pas WSL |
| **CMD** | Terminal classique Windows | ★ Deconseille |

**Windows Terminal** : configurer le profil par defaut sur WSL/Ubuntu pour avoir Bash directement.

---

## Configuration settings.json (Windows)

Fichier : `~/.claude/settings.json` (dans le home WSL ou Windows selon l'installation)

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

---

## Hooks Windows (PowerShell)

### PreToolUse — Command Validator (PowerShell)
```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "powershell -NoProfile -Command \"if ($env:TOOL_INPUT -match 'diskpart|format|rm -rf|del /f|del /s|Remove-Item.*-Recurse.*-Force|Clear-Disk|mkfs|dd if=') { Write-Host 'BLOCKED BY HOOK'; exit 1 }\""
    }
  ]
}
```

### Stop — Son fin de tache (PowerShell)
```json
{
  "hooks": [
    {
      "type": "command",
      "command": "powershell -NoProfile -Command \"[System.Media.SystemSounds]::Asterisk.Play()\"",
      "async": true
    }
  ]
}
```

### Alternative : Command Validator via bun (WSL)
Si bun est installe dans WSL :
```json
{
  "type": "command",
  "command": "bun ~/.claude/scripts/command-validator/src/cli.ts"
}
```

---

## Sons et notifications

### PowerShell (natif Windows)
```powershell
# Son systeme
[System.Media.SystemSounds]::Asterisk.Play()
[System.Media.SystemSounds]::Beep.Play()
[System.Media.SystemSounds]::Exclamation.Play()
[System.Media.SystemSounds]::Hand.Play()
[System.Media.SystemSounds]::Question.Play()

# Son personnalise (WAV)
$player = New-Object System.Media.SoundPlayer "C:\Windows\Media\notify.wav"
$player.Play()
```

### Sons Windows disponibles
```
C:\Windows\Media\
├── notify.wav
├── Windows Notify.wav
├── tada.wav
├── chimes.wav
├── chord.wav
└── ... (nombreux autres)
```

### Configuration hooks avec sons distincts
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

---

## Deny-list Windows

Commandes dangereuses specifiques a Windows :

```json
"permissions": {
  "deny": [
    "Bash(rm -rf *)",
    "Bash(del /f *)",
    "Bash(del /s /q *)",
    "Bash(Remove-Item*-Recurse*-Force*)",
    "Bash(diskpart*)",
    "Bash(format *)",
    "Bash(*Clear-Disk*)",
    "Bash(dd if=*)",
    "Bash(mkfs*)",
    "Bash(sudo rm*)",
    "Bash(chmod 777 *)",
    "Bash(git push --force*)",
    "Bash(git reset --hard*)",
    "Bash(curl *| bash*)",
    "Bash(npm publish*)",
    "Bash(shutdown*)",
    "Bash(reboot*)",
    "Bash(netcfg -d*)"
  ]
}
```

### Double securite (CLAUDE.md)
Ajouter dans votre CLAUDE.md :
```
- JAMAIS utiliser del /f ou del /s /q
- JAMAIS utiliser Remove-Item -Recurse -Force
- JAMAIS utiliser diskpart ou format
- JAMAIS rm -rf (meme dans WSL)
- Toujours utiliser la corbeille ou trash
```

---

## MCP recommandes

```bash
# Dans WSL ou PowerShell
claude mcp add --scope user context7
claude mcp add --scope user exa
```

JAMAIS plus de 3 MCP simultanement.

---

## Status Line

### Installation
```bash
# Prerequis : bun installe
bun --version

# Installe automatiquement par Blueprint CLI
npx ig-blueprint-cli
```

### Si ne fonctionne pas
- Verifier que bun est installe dans le bon environnement (WSL ou Windows natif)
- `/debug` dans Claude Code pour auto-reparer

---

## tmux sur WSL

### Installation
```bash
# Dans WSL Ubuntu
sudo apt install tmux
```

### Raccourcis essentiels

| Raccourci | Action |
|-----------|--------|
| `tmux` | Nouvelle session |
| `Ctrl+A C` | Nouveau terminal (window) |
| `Shift+fleches` | Switcher entre windows |
| `Ctrl+A S` | Split en deux |
| `Ctrl+A D` | Detacher (quitter sans fermer) |
| `tmux attach` | Rattacher la session |

### Note Windows Terminal
Windows Terminal supporte nativement les tabs et splits — tmux est optionnel si vous utilisez Windows Terminal.

---

## Structure du dossier .claude/

### Emplacement selon l'installation

| Installation | Chemin |
|-------------|--------|
| WSL | `~/.claude/` (dans le filesystem WSL) |
| Windows natif | `C:\Users\<user>\.claude\` |
| Claude Desktop | `%APPDATA%\Claude\` |

### Arborescence
```
~/.claude/
├── CLAUDE.md             # Memoire globale
├── settings.json         # Config partagee
├── settings.local.json   # Config locale
├── rules/                # Regles persistantes
├── skills/               # Skills
├── agents/               # Agents custom
├── scripts/              # command-validator, statusline
├── projects/             # Historique sessions
├── plans/                # Plans generes
├── tasks/                # Taches executees
└── teams/                # Equipes (JSON)
```

---

## Montage disques dans WSL

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

---

## Astuces Windows

### Copier un chemin dans l'Explorateur
- Barre d'adresse > clic > copier le chemin
- Ou : Shift + clic droit > "Copier comme chemin d'acces"

### Ouvrir VS Code depuis le terminal
```bash
code .          # Ouvrir le projet courant (fonctionne dans WSL aussi)
```

### Dossier "CC" — Fourre-tout Claude Code
```bash
mkdir ~/cc
# Dans .bashrc ou .zshrc :
alias cdcc="cd ~/cc"
```

### Scripts PowerShell vs Bash
- **CLAUDE.md global** : preciser que les scripts doivent etre en PowerShell OU en Bash selon le contexte
- **CLAUDE.md projet** : preciser `Scripts PowerShell sur cette machine`
- Si WSL : Bash est recommande (meme workflows que macOS/Linux)

---

## Claude Desktop (Windows)

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

---

## Troubleshooting Windows

### Claude Code ne trouve pas la commande `claude`
```powershell
# Relancer le terminal
# Ou verifier le PATH :
echo $env:PATH | tr ';' '\n' | grep -i claude
```

### WSL : commande claude introuvable
```bash
# Reinstaller dans WSL
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

---

## Credits

Methode basee sur la Masterclass de [Melvynx (CodeLynx)](https://codelynx.dev).
Voir README.md pour la methode complete (workflows, prompting, context engineering, anti-patterns).
