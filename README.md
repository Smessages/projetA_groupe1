Projet de DevOps avec Linux, Bash/Python, vagrant, virtualbox

Objectif général du projet
Déploiement de l'application WORDPRESS dans un environement linux 
Le but de ce projet est de vous familiariser avec l'administration système, l'automatisation, 
la mise en œuvre de la tolérance de charge et la gestion de la sécurité dans un environnement Linux

Cette application est constituée d'une partie Front-end(IHM), et d'une partie Back-end principalement une base de données MySQL pour ce pojet.


![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/5455f4d8-8b91-4b0c-ab71-42ca930bdcdc)

Prérequis du projet:

commande de ligne linux

VirtualBox pour créer l'environement linux 

Vagrant pour l'automatisation des tâches

Bash Script

application: Wordpress

DataBase: MySQL

outils openource: HaProxy, OpenSSl

* Automatisation l'installation

*le fichier install_tools.sh permet le déploiement de toute l'infrastructure.

-Nous utilisons l'outil Vagrant installer sur notre machine window, pour automatiser le déploiement des serveurs linux , le Vagrantfile 
permettra de lancer dans un premier les trois serveurs et dans un second temps nous utilserons toujours vagrant pour lancer le script install_tools pour 
configurer chaque server en fonction du role qu'aura chaque machine comme l'indique le shéma d'architecture.

-une (1) machine qui fait la repartion de charge et reverse proxy sur laquelle nous allons installer et configurer l'aplication HAproxy

-deux serveurs wordpress  derières le serveur proxy sur lesquels seront répartis les requêtes clients.

Comment fonctionne les scripts?

Grâce à l'outil vagrant et à sa fonction provisioning, nous pouvons automatiser des process qui sont repètitif comme le déploiement des instances linux de notre

infrastructure, modifier des fichiers de configuration dans chacune des instances tout ceci sans aucune intervention humaine.

l'utilisation de vagrant est assez simple, une fois installé sur la machine de devlopement 

![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/e82dca4b-6f78-4573-8225-250f73905e9d)

la commande: Vagrant -v permet d'aficher la version installé sur votre machine.

![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/707c5dd4-3f7a-412b-b1a7-3ec15b4fa969)


on crée un repertoire et on l'initialise avec la commande: vagrant init qui crée un fichier d'initialisation Vagrantfile qu'on peut modifier en fonction de nos besoin.

La fonction principale du Vagrantfile est de décrire le type de machine requis pour un projet, et comment configurer et provisionner ces machines donc c'est ce que fait notre Vaagrantfile.

c'est une suite d'instruction que va exécuter l'outil(l'operatuer vagrant) pour mettre en place notre infrastructure.

![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/1490afe2-8440-4193-bb74-ab19d11e1470)

sur cet example nous voulons lancer une machine ubuntu , on ajoute quelaues paramètres de configuration tel que le réseau dans lequel on déploie la machine , une adresse IP,

le nom de la machine , la taille de la mémoire ram et le nombre de processeur et très important le provider dans l'instruction ![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/7df0be09-eecc-4240-8096-114bff07f43b) 

car vagrant peut être utiliser dans plusieur enviornement de virtualisation (virtualbox, hyper-v, docker, KVM et biens d'autres)


On peut ainsi dans le Vagrantfile , une fois qu'on a spécifié les caractéristiques de la  machine qu'on veut déployer 

faire la commande: vagrant up pour que l'outil vagrant déploie la machine configurée.

La commande vagrant up doit s'exécuter dans le repertoire où se trouve le fichier Vagrantfile sinon il faut indiquer le chemin complet du fichier.

les commandes de l'outil vagrant sont accéssible sur la documentation officiel de vagrant.
https://developer.hashicorp.com/vagrant/docs/vagrantfile




