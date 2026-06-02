test sur l'utilisation des branches
# Cours_Interactions_avec_depot_distant
Description : ce champ vous permet de fournir une brève description de votre dépôt. Décrivez en quelques mots le but du projet ou les fonctionnalités principales du dépôt. Ce champ est facultatif.

i vous avez du code en local que vous voulez envoyer sur le nouveau dépôt, vous allez pouvoir utiliser git push.

CTRL+C pour copier, CTRL+V pour coller
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/JohnDoeCourse/MyFirstRepo.git
git push -u origin main

Voici quelques explications :

‘git init’ : cette commande initialise un nouveau dépôt Git dans le répertoire actuel. Elle crée un dossier caché nommé « .git » qui stocke tous les fichiers et l'historique de votre projet Git. Vous devez exécuter cette commande une seule fois au début de la création de votre dépôt.

‘git add .’ : cette commande ajoute toutes les modifications et les nouveaux fichiers de votre répertoire de travail à l'index de Git. L'index est une zone de préparation où vous spécifiez les changements à inclure dans votre prochain commit. En utilisant cette commande, vous préparez tous les fichiers modifiés pour être enregistrés dans l'historique.

‘git commit -m "first commit"’ : avec cette commande, vous créez un nouveau commit qui enregistre les modifications ajoutées à l'index. Le message entre guillemets, tel que « first commit », décrit brièvement les changements effectués dans ce commit. Vous pouvez personnaliser le message pour décrire les modifications spécifiques que vous avez apportées.

‘git branch -M main’ : cette commande renomme la branche principale (habituellement « master ») en « main ». C'est une pratique courante pour adopter une terminologie plus inclusive dans Git.

‘git remote add origin https://github.com/JohnDoeCourse/MyFirstRepo.git’ : utilisez cette commande pour ajouter une référence à un dépôt distant appelé « origin ». L'URL spécifiée correspond à l'emplacement du dépôt distant sur GitHub. En utilisant « origin », vous pouvez facilement faire référence à ce dépôt distant dans d'autres commandes Git.

‘git push -u origin main’ : cette commande envoie vos commits locaux vers le dépôt distant spécifié par « origin ». L'option -u définit la branche locale « main » comme branche de suivi pour la branche « main » du dépôt distant. Cela vous permet d'utiliser simplement git push à l'avenir pour pousser vos modifications vers le dépôt distant.
>>>>>>> 0b3b45a7bd0b5d92ec09cb749ce8020f56c86211
