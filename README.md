# Informations sur le projet

CES'EAT est un projet de cinq semaines réalisé dans le cadre de nos études à CESI. Voici le cahier des charges auquel notre équipe de développeurs a dû se conformer :

## Les spécifications fonctionnelles

### L'utilisateur final

Il est le client consommateur (celui qui commande son repas). Il doit être en mesure de :

- Créer, modifier, supprimer, consulter son compte
- Créer, modifier, supprimer, consulter, payer une commande
- Consulter l'historique de ses commandes
- Suivre une livraison en cours
- Parrainer un ami
- Recevoir des notifications (hors e-mail, de type pop-up)

### Le restaurateur

Le restaurateur qui souhaite accroître sa visibilité et son offre commerciale peut bénéficier des services de la plateforme. Il doit être en mesure de :

- Créer, modifier, supprimer, consulter son compte
- Créer, modifier, supprimer, consulter un article (plat, boisson, sauce, accompagnement,...)
- Créer, modifier, supprimer, consulter un menu (composition d'articles)
- Visualiser, valider une commande
- Suivre une livraison
- Consulter l'historique des commandes
- Consulter des statistiques
- Parrainer un autre restaurateur
- Recevoir des notifications (hors e-mail, de type pop-up)

### Le livreur

Le livreur est comme le restaurateur un indépendant, il peut adhérer à la plate forme pour prendre à son compte des livraisons (courses). Il doit être en mesure de :

- Créer, modifier, supprimer, consulter son compte
- Accepter ou refuser une livraison
- Prendre en charge et acquitter la livraison (Bonus : une application qui permet de vérifier / Validation : Edition d'un QRcode / pour côté resto et côté client)
- Parrainer un livreur
- Recevoir des notifications (hors e-mail, de type pop-up)

### Le développeur tiers

Il est un indépendant ou collaborateur d'une entreprise. Il a la responsabilité d'intégrer des composants logiciels proposés par la plateforme, sur ses propres applications. Ainsi, d'autres applications clients développées par des entreprises ou des indépendants peuvent être intégrées à d'autres solutions clientes ou plateformes tierces (middleware).

- Créer, modifier, supprimer, consulter son compte
- Utiliser l'API en fonction de sa clé de sécurité
- Consulter les composants disponibles
- Télécharger un composant

### Le service commercial

Elle est la porteuse de l'offre de convergence, mais elle est elle-même cliente de sa propre plateforme. Elle doit être en mesure de :

- Consulter, suspendre, modifier, supprimer des comptes clients
- Disposer en temps réel de tableaux de bord de suivi de processus de commande (passation commande, acceptation commande, acceptation livraison, acquittement livraison, chiffre d'affaires transactionnel global en cours)
- Recevoir des notifications (hors e-mail, de type pop-up)

### Le service technique

Il est le garant de la qualité technique des prestations offertes par la plateforme. Il doit être en mesure de :

- Ajouter, supprimer des composants réutilisables
- Consulter les logs de connexions
- Consulter les statistiques de performances des serveurs et microservices
- Consulter les logs de téléchargement des composants réutilisables
- Orchestrer les routes pour les résolutions des demandes entrantes
- Déployer de nouveaux services ou microservices sans interruption du service client
- Recevoir des notifications (hors e-mail, de type pop-up)

## Les spécifications d'architecture

### Les applications : livreur – restaurateur – client final – commercial/technique

Ces applications doivent répondre aux caractéristiques techniques suivantes :

- Ces applications doivent proposer une couche présentation qui permettra à l'utilisateur d'utiliser les fonctionnalités.
- Ces applications doivent proposer une couche de composants locaux. Ils servent à faire les traitements locaux nécessaires au bon fonctionnement de l'application. Elle invoque les services distants.
- Ces applications doivent proposer une couche de communication. Elle assure la sécurité de la communication, adhère au protocole de communication imposée par la plateforme, elle fournit les informations de sécurité, elle fournit les informations d'applications.
- Uniquement pour les applications commercial/technique, elles doivent être architecturées autour des techniques de composants graphiques réutilisables et elles sont dynamiques.

### La plateforme – Le middleware

La plateforme à construire est une hybridation entre une architecture orientée service, un entreprise service bus et une architecture microservice. Elle propose une liaison de données sécurisée et asynchrone. Elle est orientée message. Elle est scalable pour anticiper la montée en charge et les dépressions d'activité. Puisque scalable, les modules à forte consommation de cycles processeur seront facilement déployables dans le cloud si nécessaire ; ils seront dans tous les cas conteneurisés.

- Le point de terminaison doit disposer d'une liaison sécurisée. Elle est interopérable pour les systèmes hétérogènes. Il vérifie l'authenticité des applications (token app). Il est asynchrone. Il reçoit un message et retourne un message en réponse.
- La couche services expose tous les services offerts par la plate forme en une seule façade. Elle propose la documentation technique des services aux développeurs.
- Le contrôleur de résolution vérifie les autorisations au travers du token utilisateur. Il autorise ou met un terme à la demande en fonction des droits attribués à l'utilisateur. Il examine la demande contenue dans le message, l'application invocatrice et son numéro de version et redirige alors vers le bon micro service ou méta service.
- Le proxy se chargera de récupérer les statistiques de performances des serveurs virtuels (load balancing) et routera le message sur le serveur le plus disponible.
- Le microservice ou méta-service (orchestrateur de microservices) gère les opérations transactionnelles. Il exécute une tâche et compose le message de réponse.
- Les composants proposent des actions spécialisées. Ils sont agrégés par les microservices.

### La plateforme – Les data

La couche donnée propose la persistance des informations non relationnelles. Un entrepôt est destiné aux sources documentaires et un autre est destiné au stockage des composants réutilisables.

## Le schéma d'architecture

Vous trouverez ci-après le schéma (grandes mailles) de l'architecture globale de la solution informatique à développer.


## Les spécifications techniques

L'architecture globale à mettre en place concernant la solution informatique d'entreprise doit être un équilibre entre une architecture orientée service et une architecture orientée microservices.

### Partie Applicative

Les applications (livreur, restaurateur, client final, développeur, service commercial, service technique) doivent être découpées en trois modules :

- Le front : en charge de la mise à disposition des fonctionnalités de l'application et de la validation des saisies utilisateurs.
--Chaque composant devra être interchangeable dans chacune des plateformes développées.
--La sécurité devra être gérée à l'aide de routes autorisées et de datas restrictions.

- Le middleware local : un ensemble d'API en charge du traitement local des messages issus de la plateforme (plateforme -> application), ou du prétraitement du message à destination de la plateforme (application -> plateforme) et ou de services locaux non proposés par la plateforme (car spécifiques à l'application).
--Une couche d'abstraction permettra un appel vers les différentes APIs.
--Les messages (sucess / faillure / pending...) devront être normalisés, et proposer des éléments UI spécifiques.

- Le proxy : en charge de la communication avec la plateforme

### Partie plateforme

La plateforme est composée d'une partie service, d'une partie microservices virtualisée en deux machines, d'une partie data composée d'un serveur de base de données relationnelles et d'un serveur NoSQL.

## Couche services

- Un endpoint unique (point d'entrée de toutes les communications)
- Une couche services qui présente l'ensemble des services (qui seront consommés par les applications) ainsi que la documentation développeur nécessaire à l'utilisation des services
- Le contrôleur de résolution, il doit se charger de vérifier (demande entrante) le type d'application, la version de l'application, la demande, les droits de l'utilisateur vis à vis de la demande. Il appellera alors en fonction de ces vérifications le bon contrôleur d'exécution transactionnel
- Le proxy, en charge de la communication, il veillera à appeler la bonne machine virtuelle (load-balancing).

## Couche composants (virtualisés X2)

Un endpoint unique (point d'entrée de toutes les communications)

- Contrôleur d'exécution transactionnel des micro services, en charge d'orchestrer le travail des plugins et de gérer les transactions.
- Les plugins, en charge de la réalisation d'une partie du microservice.
- Le proxy, qui dirigera la demande sur le type de bases de données nécessaires au traitement de la demande.

## Couche data

- Comporte les systèmes de gestion de base de données (R/NoSQL)
- Un système de supervision / reporting devra être en place, ayant pour but la surveillance et l'optimisation de l'acquisition des données.

## Autres spécifications

- Concernant le suivi de livraison, vous pouvez, si possible, utiliser un composant réutilisable type MAP) / Étudiez également la possibilité d'utiliser un socket en temps réel.
- La gestion des données applicatives devront être stockées sur une base NoSQL (MongoDB).
- La documentation des APIs devra permettre à n'importe quel développeur de consommer les APIs.
- Les messages de retour devront être normalisés et faire état de bonnes pratiques définies en amont du projet.
- L'application pour le développeur tiers ne tient pas compte d'une architecture précise (puisque nous n'avons pas la main sur cette dernière). Toutefois, elle doit être en lien avec un service NPM (composants réutilisables).
