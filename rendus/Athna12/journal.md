Carnet de bord Athna12

Mes preuves :

TP 1 : Construire un historique utile

Étape A : Le premier commit · 7 minutes

Que fait add que ne fait pas l'enregistrement dans l'éditeur ?

Il prépare avant l'enregistrement

Étape B : Le piège de l'index · 12 minutes

Prédiction avant de continuer : si vous commitez maintenant, quelle phrase manque dans le commit ?
Pourquoi ?

 la phrase " Inscription sur place "
  parce que le git add n'est pas fais.

  Étape C : Choisir ce qui reste local · 9 minutes

  Enquête 1 : le fichier disparu du diff · 20 minutes

  git diff vas rien afficher par à rapport à acces.md car il faut git add d'abord

  Pour fichier non suivi  il  est marqué ??
  
  retrouver le commit d'ajout:

$ git log --oneline -- acces.md
9cb7ac1 (HEAD -> main) modif

$ git show 9cb7ac1
commit 9cb7ac1dd46420f637cd4197e446d13b5038162b (HEAD -> main)
Author: Athna12 <nathanaelnagou@gmail.com>
Date:   Thu Oct 8 15:10:24 2026 +0200
    modif
new file mode 100644
(+3 lignes ajoutées dans acces.md)

Explication:

Avant mon commit, git log -- acces.md n'affichait rien, car aucun commit
ne contenait encore le fichier.

git log -- acces.md filtre l'historique et n'affiche que les
commits qui ont modifié ce fichier.


TP 2 : Une branche pour une évolution

2. Enregistrer l'évolution

Avant de revenir sur main, prédisez quels fichiers seront visibles.

ceux qui ne sont ni dans le gitignore et dans la branche main

3. Observer puis intégrer

$ git switch main
$ ls
README.md  acces.md  journal.md  notes-privees.txt

$ git merge feature/programme
Updating 9b9e8fd..f65b06e
Fast-forward
 rendus/Athna12/programme.md | 4 ++++

 