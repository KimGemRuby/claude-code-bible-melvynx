# Bible Melvynx — Prompting Avance

## Greenfield (nouveau projet)
- Preciser la stack : "create a new vgs project with chatcn and tailwind"
- Decrire la feature en langage naturel
- Separer desktop et mobile dans la description
- Ne PAS preciser la technique de code sauf preference forte
- Laisser l'IA choisir la structure

## Brownfield (projet existant)
- Screenshots annotes (CleanShotX ou equivalent)
- Referencer les styles existants : "mets un style qui correspond au illustration picker"
- L'IA suit automatiquement les conventions du code existant

## 10 variations UI (quand on sait pas ce qu'on veut)
Prompt exact :
"Cree un fichier /demo/page.tsx avec 10 variations UI/UX de cette interface, 10 composants separes dans 10 fichiers separes, importes dans la page."

## Debug (methode la plus efficace = logs)
1. Ajouter des console.log strategiques
2. Reproduire le bug
3. Copier les logs bruts de la console
4. Envoyer a Claude
- Preferer les logs au Chrome headless (juge "lourd et lent" par Melvynx)

## Prompts vagues (intentionnels)
Parfois utiliser des prompts vagues : "rend le selecteur plus smart", "repense cette interface"
L'IA a alors la liberte creative et propose des solutions parfois meilleures.

## Commentaires comme prompt injection benefique
Documenter les helpers internes avec exemples d'utilisation dans les commentaires du code.
L'IA les lit (grace a la regle "lire 3 fichiers similaires") et absorbe les instructions.

## Continuous Learning (anti-erreurs recurrentes)
Quand l'IA fait la meme erreur 2+ fois :
1. Lui dire d'arreter la tache en cours
2. "Cree un fichier dans .claude/rules/ qui t'empechera de refaire cette erreur"
3. Utiliser CRITICAL: et emphasis pour renforcer
4. "Reduis la taille du fichier stp, garde la regle au minimum"
