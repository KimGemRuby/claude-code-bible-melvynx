# Bible Melvynx — Securite Complete

## Architecture 3 couches
1. **Bypass** : `defaultMode: "bypassPermissions"` dans settings.json
2. **Deny list** : Override le bypass, bascule en Ask Mode sur commandes critiques
3. **CLAUDE.md** : "jamais rm-rf, toujours trash" — alternative sure

## Deny list (settings.json)
Commandes a interdire :
- rm -rf, del /f, del /s /q
- sudo
- chmod 777
- git push --force sur main/master
- diskpart, format, Clear-Disk, mkfs, dd if=
- curl | bash, wget | bash

## Hook Command Validator (PreToolUse)
Script PowerShell avec Regex qui verifie $env:TOOL_INPUT avant execution.
ATTENTION: Pas toujours fiable sous Windows — la deny list est le vrai mur.

## Doublons et suppression
- CRITICAL: Ne JAMAIS supprimer un fichier identifie comme doublon uniquement par sa taille
- OBLIGATOIRE: Hash SHA-256 avant toute suppression (certutil -hashfile)
- Workflow : hash > confirmer identite > validation utilisateur > trash

## VPS / Agents distants
- UFW + Fail2Ban : whitelister TOUTES les IPs de confiance AVANT activation
- Desactiver password SSH (uniquement cle SSH)
- Docker pour sandboxing
- Ne jamais laisser le dashboard OpenClaw ouvert en permanence
- Audit : claudebot security-audit

## Astuce bypass sur VPS
SANDBOX=1 claude  # Claude croit etre dans une sandbox
export SANDBOX=1  # dans .bashrc pour persistance
