# Regle de securite : Operations sur les fichiers

Regle de securite : n'utilise jamais rm -rf, toujours utiliser trash pour supprimer les dossiers.

- Ne JAMAIS utiliser rm, rm -rf, rm -r, del, Remove-Item pour supprimer des fichiers ou dossiers
- TOUJOURS utiliser trash comme alternative sure
- Cette regle s'applique sur TOUTES les machines (BOKADOR, VPS, Mac)
- Aucune exception, meme en mode autonome (/apex, /oneshot)

## Detection de doublons
- Ne JAMAIS proposer de supprimer un fichier identifie comme "doublon" uniquement par sa taille
- La taille identique ne prouve PAS que deux fichiers sont identiques
- OBLIGATOIRE : verification par hash SHA-256 (certutil -hashfile ou sha256sum) avant toute proposition de suppression
- Workflow : comparer hash SHA-256 des deux fichiers > confirmer identite > demander validation utilisateur > utiliser trash
