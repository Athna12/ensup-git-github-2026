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


TP 5 : Le laboratoire des erreurs

Préparation : dossier labo-conflit, commit de référence 2b4eda3

git restore --staged brouillon.txt retire le fichier de l'index sans le supprimer
Puis commit c5e8733

git revert --no-edit HEAD crée le commit inverse 4eaf1a5 mais c5e8733 reste dans l'historique

$ git show --no-patch --format="%h %p %s" HEAD
bc8480e ca185e4 9f34ab3 fix: convenir de 09h45
commit de fusion : bc8480e
1er parent : ca185e4 (main, 09h30)
2e parent : 9f34ab3 (feature/horaire, 10h00)




F. Expliquer · 5 minutes
Montrez le résultat à votre binôme. Répondez :  


pourquoi Git n'a-t-il pas choisi seul ?
Les deux branches ont modifié la même ligne de façon différente Git ne sait pas quelle valeur est la bonne.

Quelle version était HEAD ?
Celle de main puisque c’est la branche sur laquelle j’étais au moment du merge.

À quoi sert git merge --abort pendant une fusion en cours ?

Il annule la fusion et remet le dépôt dans l’état d’avant le git merge. C’est utile si on veut réfléchir ou demander avant de décider.


Incident distant : le programme modifié en ligne · 20 minutes

Situation :

sur GitHub : ajout de "12h00 : visite libre des stands, Hall A." dans programme.md (commit 94846c5)
 
en local : ajout de "Prévoir une arrivée dix minutes avant le premier atelier." dans README.md (commit c27367e)

Les deux sont parties de 4bae8d0 et ont avancé chacune de leur côté.

Pourquoi fetch seul n'aurait pas ajouté la visite libre à mon fichier ?

git fetch télécharge les commits de GitHub et met à jour le repère origin/main (ici de 4bae8d0 à 94846c5)
Mais il ne touche ni à ma branche main ni à mes fichiers.
Après le fetch, le commit 94846c5 était sur mon PC mais programme.md ne contenait toujours pas la ligne de 12h00.
Il a fallu git merge origin/main pour intégrer ce commit dans mon main et donc dans mes fichiers.

Fusion sans conflit (commit e86c26b) : les deux modifications portent sur des fichiers différents (programme.md et README.md).

Mission finale : une version utilisable · 75 minutes

Étape 3 : demander une revue · 15 minutes

https://github.com/Athna12/ensup-git-github-2026/pull/3


Contrôle de rendu individuel

Que copie add ?

git add copie l'état actuel du fichier du dossier de travail vers l'index

Comment prouver qu'un commit a été poussé ?

git status affiche "Your branch is up to date with 'origin/main'"
git log --oneline montre origin/main sur le même commit que main
le commit apparaît sur GitHub dans l'onglet Commits de mon fork

Pourquoi Git n'a-t-il pas choisi l'horaire ?

Git ne peut pas savoir quelle valeur est la bonne

Que reste-t-il après revert ?

Le commit annulé reste dans l'historique

Un diff dont je suis l'auteur :

$ git show 94846c5
commit 94846c53c32ec6ec6622fc09497274b6f933a920
Author: Nathanaël <132097635+Athna12@users.noreply.github.com>
Date:   Sun Oct 11 14:17:25 2026 +0200

    ajout la visite libre de 12h00

diff --git a/rendus/Athna12/programme.md b/rendus/Athna12/programme.md
--- a/rendus/Athna12/programme.md
+++ b/rendus/Athna12/programme.md
@@ -2,3 +2,4 @@
 09h00 : accueil : Hall A
 10h00 : découverte des associations : Salle 1
 11h00 : atelier découverte : Salle 2
+12h00 : visite libre des stands, Hall A.