# juc-jenkins-2018
Code source de la démo JUC Jenkins 2018

# Prérequis

Afin de faire fonctionner cette démo, il est nécessaire de disposer d'un JDK fonctionnel, de la commande git ainsi que curl.

# Clonage du repository

Cloner ce repository quelque part chez vous :

    git clone https://github.com/Yannig/juc-jenkins-2018.git

Noter bien le chemin complet où se trouve le repository (ex : /home/yannig/dev/juc-jenkins-2018).

# Mise en place du hook Jenkins

Copier le fichier **post-commit** de ce repository et le mettre dans le répertoire .git/hooks relatif au chemin de clonage du repository.

Ce hook prendra en charge les événements correspondants à un commit afin de prévenir Jenkins qu'il doit lancer un build.

Ce hook est adaptable pour un serveur distant en remplaçant le chemin par l'URL du repository et en changeant le nom du hook pour **post-receive**.

# Lancement de Jenkins

Récupérer une version récente de Jenkins sous forme de war sur https://jenkins.io/ puis le lancer. Ci-dessous un exemple de lancement avec 2Go de mémoire :

    java -jar jenkins.war -Xmx2G

Jenkins devrait mettre quelques dizaines de secondes à démarrer.

# Connexion à Jenkins

Par défaut, Jenkins sera joignable sur le port 8080 et demandera un compte admin. Le mot de passe de ce dernier est généré automatiquement. La valeur initiale se trouve dans le fichier **~/.jenkins/secrets/initialAdminPassword**.

Au premier démarrage, Jenkins vous demandera quels plugins installer. Bien s'assurer que les plugins de gestion des pipelines soient bien présents.

# Création d'un job pipeline

Se rendre dans l'interface Jenkins et créer un job de type multi-branche pipeline (éventuellement pipeline simple) faisant référence à l'emplacement du clone du repository.

# Déclenchement d'un build

Tout est maintenant en place. Lorsque vous ferez un commit sur le repository local, Jenkins sera prévenu automatiquement. Ce dernier se chargera de lancer un build à chaque nouveau commit (y compris les amendements).
