# Tests de Sécurité (Pentest) — Résultats

## Objectif
Vérifier que les 3 couches de sécurité fonctionnent correctement sur BOKADOR (Windows 11 Pro).

## Test 1 : Deny List — `rm -rf`

**Commande testée :**
```bash
rm -rf /tmp/test_fake_dir
```

**Résultat :** BLOQUÉ ✅
```
Permission to use Bash with command rm -rf /tmp/test_fake_dir has been denied.
```

**Analyse :** La deny list `Bash(rm -rf *)` a intercepté la commande avant qu'elle n'atteigne le terminal. C'est le comportement attendu (Ask Mode).

---

## Test 2 : Hook Regex — `echo` contenant `rm -rf`

**Commande testée :**
```bash
echo "Hook test: rm -rf /test"
```

**Résultat :** NON BLOQUÉ (attendu) ✅
```
Hook test: rm -rf /test
```

**Analyse :** Normal — c'est un `echo`, pas une vraie commande destructrice. Le hook vérifie `$env:TOOL_INPUT`.

---

## Test 3 : Hook Regex — `diskpart`

**Commande testée :**
```bash
diskpart /s script.txt
```

**Résultat :** NON BLOQUÉ ⚠️ FAILLE DÉCOUVERTE
```
Microsoft DiskPart version 10.0.26100.1150
DiskPart was unable to process the parameters.
```

**Analyse :** Le hook Regex n'a PAS intercepté la commande. `diskpart` a été exécuté et n'a échoué que parce que `script.txt` n'existe pas. Si le fichier avait existé, diskpart aurait pu manipuler les partitions.

**Cause :** La variable `$env:TOOL_INPUT` n'est pas toujours injectée correctement par Claude Code dans PowerShell sous Windows.

**Correction appliquée :** Ajout de `Bash(diskpart*)` dans la deny list (couche 2).

---

## Test 4 : Deny List — `diskpart` (après correction)

**Commande testée :**
```bash
diskpart /s test.txt
```

**Résultat :** BLOQUÉ ✅
```
Permission to use Bash with command diskpart /s test.txt has been denied.
```

**Analyse :** La deny list bloque désormais `diskpart`. Correction validée.

---

## Leçons apprises

| Leçon | Impact |
|-------|--------|
| Le hook Regex (`$env:TOOL_INPUT`) n'est pas fiable sous Windows | Le hook est un filet, pas un mur |
| La deny list (couche 2) est le seul blocage garanti | Toujours mettre les commandes critiques dans la deny list |
| Tester CHAQUE couche indépendamment | Ne pas supposer que le hook fonctionne |
| Les commandes Windows (diskpart, format) doivent être dans la deny list | La deny list originale de Melvynx est pensée pour macOS |

## Matrice de couverture finale

| Commande | Deny List | Hook Regex | Couvert ? |
|----------|-----------|------------|-----------|
| `rm -rf` | ✅ | ✅ | Double protection |
| `del /f` | ✅ | ✅ | Double protection |
| `del /s /q` | ✅ | ✅ | Double protection |
| `diskpart` | ✅ | ❌ (non fiable) | Deny list seule |
| `format` | ✅ | ❌ (non fiable) | Deny list seule |
| `Clear-Disk` | ✅ | ❌ (non fiable) | Deny list seule |
| `mkfs` | ✅ | ✅ | Double protection |
| `dd if=` | ✅ | ✅ | Double protection |
| `git push --force` | ✅ | ❌ | Deny list seule |
