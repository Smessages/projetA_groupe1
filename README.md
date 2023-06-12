# Projet de DevOps avec Linux, Bash/Python, vagrant, virtualbox

## Objectif général du projet
### Déploiement de l'application WORDPRESS dans un environement linux 
### Le but de ce projet est de vous familiariser avec l'administration système, l'automatisation, 
### la mise en œuvre de la tolérance de charge et la gestion de la sécurité dans un environnement Linux

####  Cette application est constituée d'une partie Front-end(IHM), et d'une partie Back-end principalement une base de données MySQL pour ce pojet.


![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/5455f4d8-8b91-4b0c-ab71-42ca930bdcdc)

## Prérequis du projet:

#### commande de ligne linux

#### VirtualBox pour créer l'environement linux , Boxes Centos ou ubuntu

#### Vagrant pour l'automatisation des tâches

#### Bash Script, PHP >= 7.4 

#### application: Wordpress

#### DataBase: MySQL >= 5.7

#### outils openource: HaProxy

## Automatisation l'installation

#### le fichier install_tools.sh permet le déploiement de toute l'infrastructure.

#### Nous utilisons l'outil Vagrant installer sur notre machine window, pour automatiser le déploiement des serveurs linux , le Vagrantfile 
#### permettra de lancer dans un premier temps les trois serveurs et dans un second temps nous utilserons toujours l'outil vagrant pour lancer le script install_tools pour 
#### configurer chaque server en fonction du role qu'aura chaque machine comme l'indique le shéma d'architecture.

#### une (1) machine serveur HAproxy qui fait la repartion de charge et reverse proxy sur laquelle nous allons installer et configurer l'aplication HAproxy

#### deux machines serveurs wordpress  derières le serveur proxy sur lesquels seront répartis les requêtes clients.

### Comment fonctionne les scripts?

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

La commande $ vagrant up  doit s'exécuter dans le repertoire où se trouve le fichier Vagrantfile sinon il faut indiquer le chemin complet du fichier.

les commandes de l'outil vagrant sont accéssible sur la documentation officiel de vagrant.

https://developer.hashicorp.com/vagrant/docs/vagrantfile


Pour les mises à jour et la configuration des différents fichiers, nous allons passer en argument un script "Install_tools.sh" via l'instruction 

proxy1.vm.provision :shell do |shell|

      shell.path = "install_tools.sh"
      
      shell.args = ["haproxy", "website_cert.pem"]
      
      
Ce block d'instructions permet à l'outil vagrant d'accèder au script install_tools.sh (notre script de configuration), et les paramètres que le script va utilisés.

![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/d8cf1904-3e75-4593-974b-6f80350ac4a2)



dans notre example le paramètre 1 est notre machine HAproxy , nous pouvons ainsi ajouter autant de paramètres que nécessaire et y faire référence dans notre script .

l'outil vagrant executera ainsi ligne par ligne toute les instructions du script install_tools.sh sur notre machine et au final nous aurons un serveur HAproxy configurer sur notre machine 

prèt à être utilisé sans aucune intervention humaine, nous pouvon repèter le processus nfois selon nos besoins.

Une fois le script déployer , les trois machine sont en état de fonctionnement configurées avec tous les pacquets nécessaire pour leur fonctionnement.

les deux serveurs d'application Wordpress en backend sont bien configurés avec les paquets PHP et leur Database respective.

le script "install_tools.sh" détaille ligne par ligne chacune des taches éffectuées.

Image de la fin de l'instalation du HAproxy
![image](https://github.com/Smessages/projetA_groupe1/assets/23023422/0031b98b-299e-4b23-9dae-eb43daea040b)

  
  
 Comment fonctione le Load-Balancer , reverse Proxy ?
 
 Très concrètement les requètes des utilisateurs de l'application iront directement vers la machine HAproxy (son IP) qui à son tour se chargera de distribuer ces requètes vers l'un ou l'autre des 
 
 serveurs d'application wordpress dans notre projet nous en avons deux.
 
 le premier avantage de cette architecture est que nos serveurs d'application ne sont pas directement accessible à internet ceci constitue une première couche de sécurité, le deuxième avantage est que les 
 
 utilisateurs auront toujours une et une seule adresse pour accèder à leurs services pour cette aplication et ce  quelque soit le nombre de serveurs d'application derière notre HAproxy .
 
 En effet le HAproxy avec sa fonction load-balancer grace à ces algorithme rend transparent du point de vue de l'ordinateur client le processus de reponse aux requètes.
 
 Il existe plusieurs algorithme utilisé generalement pour le load-balancing 
 
 par défaut le ROUND-ROBIN qui permet d'envoyer les requètes à tour de role à chacun des serveurs d'appliction actif 
 
 Le Least-conn , le HAproxy envera la requète au server qui a le moins de connections
 
 Le HASH , la requète sera envoyé à un serveurspécifique en fonction la source de la requète(adress IP de la source)
 
 pour ne citer que ceux là.... et donc par ce mécanisme le choix du serveur d'aplication qui prend 
 
 au final en charge la requète est assuré sans aucun conflit et totalement trasparent pour la machine cliente
 
 elle ne se doute à aucun moment que c'est en fait une autre machine qui a pris encharge et répondu à sa requète.
 
 LA REPONSE lui semble venir de la machine HAproxy-(REVERSE-PROXY)
 
 Un dernier point concernant le HAproxy  dans le cadre de ce projet nous opérons en mode HTTP (niveau 7 load-balancer)
 
 nous pouvons donc filtrer les requètes de façon intéligente avec des outils tel que ACLs( Accès Control Lists) qui nous permet de diriger une requète particulière vers un serveur
 
 précis, et plus important rendre notre application accéssible en HTTPS par l'utilisation d'un certificat.
 
 Cette dernière option permet d'accroitre la securistion de notre application mais aussi de reduire la charge de travail sur les serveur en back-end.
 
 Explication: dans une configuration où notre load-balancer est en mode tcp (Niveau 4 load-balancer) donc tipiquement il reconnait qu'une IP et un port dans une requète qui lui est addr et ne pourrait donc 
 
 pas prendre en charge une requète securisé avec une tache en plus que celle decrite plus haut, dans ce cas tout le travail de décryptage des données pour enfin répondre à la requète reviendrait au serveur 
 
 d'application qui aura un surplus de travail et donc un besoin de resources (processeurs) un peu plus conséquant.
 
 On serait aussi emmené à configurer le certificat sur tout les serveurs d'application ce qui est une autre tache répétive qu'on souhaiterait éviter pour plusieurs 
 
 raisons dont la première qui me vient à l'esprit est la gestion de ces certificats sur un ensemble plus ou moins grand de machines.==>(voir explications slides) 
 
 pour des besoins pratique et de commodité nous avons déployer deux serveurs d'application wordpress accessible via le serveur HAproxy , nous notre fichier haproxy.cfg que nous avons configurez 
 
 pour que l'un des serveurs soit plutot en backup et que l'autre soit actif , dans cette config seul un serveur prendra les requètes et l'autre ne répondra aux requètes que si le premier est DOWN!!!
 
 nous voulions expérimenté dans le cadre de ce projet quelques edge cases , notament pour la sauvegarde des données en cas de panne du premier serveur.
 
 plusieurs options possible mais realisable selon les resources disponible .
 
 -NFS(Network File System)
 -Rsync
 
 quelque soit la solution choisie il est nécessaire de créer une partition LVM sur notre machine pour pouvoir facilité le stockage des données générées par le fonctionnement de l'application wordpress.
 
 voir explication slides et démo en direct.
 
 
 
 



