# Power Mode — Configuration Détaillée

## Principe (Méthode Melvynx)

L'autonomie totale est donnée à Claude Code via 3 couches superposées :

```
┌─────────────────────────────────────────┐
│  Couche 3 : CLAUDE.md / Rules           │  Comportementale
│  "jamais rm-rf, toujours trash"         │  (alternative sûre)
├─────────────────────────────────────────┤
│  Couche 2 : Deny List (settings.json)   │  Technique
│  Override le bypass → Ask Mode          │  (mur de béton)
├─────────────────────────────────────────┤
│  Couche 1 : bypassPermissions           │  Moteur
│  Autonomie totale par défaut            │  (vitesse)
└─────────────────────────────────────────┘
```

## Mécanisme exact

1. **Bypass actif** → Claude agit sans demander de permission
2. **Commande dans la deny list** → le système bascule en **Ask Mode** et demande validation
3. **L'utilisateur dit "non"** → action annulée, agent arrêté
4. **L'utilisateur dit "oui"** → action autorisée exceptionnellement

Le message d'erreur exact : `error permission to use bash with command rm-rf`

## Configuration finale appliquée

```json
{
  "$schema": "https://claude.ai/schemas/claude-code-settings.json",
  "defaultMode": "bypassPermissions",
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(del /f *)",
      "Bash(del /s /q *)",
      "Bash(rm -rf D:*)",
      "Bash(rm -rf E:*)",
      "Bash(git push --force-with-lease origin main*)",
      "Bash(diskpart*)",
      "Bash(format *)",
      "Bash(*Clear-Disk*)",
      "Bash(mkfs*)",
      "Bash(dd if=*)"
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

## Évolution de la config durant la session

| Version | Changement |
|---------|-----------|
| v1 | Allow list + 16 patterns deny + skipDangerousModePermissionPrompt |
| v2 | Suppression allow list, réduction à 6 patterns (méthode Melvynx pure) |
| v3 | Ajout schéma JSON pour autocomplétion VS Code |
| v4 | Ajout de 5 patterns critiques après pentest (diskpart, format, etc.) |
| v5 (finale) | 11 patterns deny + hook Regex + skipDangerousModePermissionPrompt restauré |

## Fichiers impactés

| Fichier | Action |
|---------|--------|
| `~/.claude/settings.json` | Réécrit 5 fois (convergence vers config finale) |
| `~/.claude/rules/file-operation.md` | Créé (trash + SHA-256) |
| `~/.claude/CLAUDE.md` | Déjà contenait "Ne JAMAIS utiliser rm-rf" |
