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

4. Expliquer

Sur quelle branche faut-il être pour intégrer une évolution dans main ?
Il faut être SUR main (git switch main), puis lancer git merge feature/programme.

Quelle commande prouve que le contenu est présent ?
$ ls
README.md  acces.md  journal.md  notes-privees.txt  programme.md

9b9e8fd ── f65b06e
     ↑          ↑
    main   feature/programme

Graphe après la fusion (fast-forward) :

  9b9e8fd ── f65b06e
                ↑
     main, feature/programme


Enquête 2 : divergence sans conflit · 15 minutes

point de départ commun : 66660e7
feature/accessibilite : ajout de accessibilite.md (11745f8)
main : ajout de l'horaire d'accueil dans README.md (2c1b5e8)

$ git show --no-patch --format="%h %p %s" HEAD
be0cefb 2c1b5e8 11745f8 merge: ajouter les indications d accessibilite

Pourquoi pas de conflit ?
Les deux branche modifie un fichier différent donc pas de problème pour les combinés.

TP 3 : envoyer au formateur · 35 minutes

C. Constater la mise à jour automatique · 8 minutes

https://github.com/AbidHamza/ensup-git-github-2026/pull/3

Fork permet de faire une copie d'un dépot

- Clone une copie du repot sur mon ordinateur.

- Branche : une ligne de travail à l'intérieur d'un même dépôt

Enquête 3 : pourquoi mon push est refusé ? · 15 minutes

1. Permission denied : votre origin vise-t-il votre fork ou le dépôt du formateur ? Quel compte est connecté ?

GitHub me reconnaît mais je n'ai pas le droit d'écrire dans ce dépôt.

utiliser git remote -v pour savoir si on vise le fork ou le depot,

Problème rencontrer : "Authentication failed" à cause d'un ancien identifiant mémorisé.
Corrigé en le supprimant (git credential reject) puis en me reconnectant via le navigateur.

2. non-fast-forward : des commits ont-ils été créés sur GitHub depuis votre dernier envoi ? 
Quelle commande récupère les repères sans fusionner ?

GitHub contient des commits que je n'ai pas en local
Mon main et celui de GitHub ont divergé, donc Git refuse le push pour ne pas effacer ces commits.

git fetch origin permet de récupérer sans fusionner

Utiliser git log --oneline --graph --all pour vérifier

Il faut ensuiste faire git pull puis git push.

3. nothing to commit alors que le fichier paraît modifié : 
est-il enregistré, dans le bon dossier, ignoré ou déjà commité ?

1 Vérifier si le fichier est enregistrer dans l'éditeur

2 Vérifier que je suis dans le bon dossier , utiliser pwd et après git status

3 Le fichier est ignoré: faire git check-ignore -v lenomdufichier , qui affiche la ligne qui bloque le fichier dans .gitignore

4 déjà commitéé? : faire git log --oneline -1 --lenomdufichier

TP 4 : faire relire une modification · 40 minutes
Lien PR binome:
https://github.com/Athna12/ensup-git-github-2026/pull/1