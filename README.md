# mediatekdocuments_rest

Ce dépôt contient les fonctionnalités ajoutées à l'API REST de base du projet MediatekDocuments.
Le dépôt d'origine, qui contient la présentation de l'application d'origine et son mode opératoire, est disponible ici :
https://github.com/CNED-SLAM/rest_mediatekdocuments

## Fonctionnalités ajoutées

Les fonctions suivantes ont été ajoutées dans `MyAccessBDD.php` pour répondre aux besoins de l'application C# MediatekDocuments :

### Gestion des livres
- `insertLivre` : insère un livre dans les tables `document`, `livres_dvd` et `livre` en utilisant une transaction.
- `updateLivre` : modifie un livre dans les tables `document` et `livre` en utilisant une transaction.
- `deleteLivre` : supprime un livre après vérification qu'il n'a pas d'exemplaires ni de commandes.

### Gestion des DVD
- `insertDvd` : insère un DVD dans les tables `document`, `livres_dvd` et `dvd` en utilisant une transaction.
- `updateDvd` : modifie un DVD dans les tables `document` et `dvd` en utilisant une transaction.
- `deleteDvd` : supprime un DVD après vérification qu'il n'a pas d'exemplaires ni de commandes.

### Gestion des revues
- `insertRevue` : insère une revue dans les tables `document` et `revue` en utilisant une transaction.
- `updateRevue` : modifie une revue dans les tables `document` et `revue` en utilisant une transaction.
- `deleteRevue` : supprime une revue après vérification qu'elle n'a pas d'exemplaires ni d'abonnements.

### Gestion des commandes de livres et DVD
- `selectCommandesDocument` : récupère les commandes d'un document avec les informations de suivi.
- `insertCommandeDocument` : insère une commande dans les tables `commande` et `commandedocument`.
- `updateSuiviCommande` : modifie l'étape de suivi d'une commande.
- `deleteCommandeDocument` : supprime une commande si elle n'est pas encore livrée.

### Gestion des abonnements aux revues
- `selectAbonnementsRevue` : récupère les abonnements d'une revue.
- `selectAbonnementsExpirantBientot` : récupère les revues dont l'abonnement expire dans moins de 30 jours.
- `insertAbonnement` : insère un abonnement dans les tables `commande` et `abonnement`.
- `deleteAbonnement` : supprime un abonnement.

### Gestion des exemplaires
- `selectExemplairesRevue` : modifiée pour retourner aussi le libellé de l'état.
- `updateEtatExemplaire` : modifie l'état d'un exemplaire.
- `deleteExemplaire` : supprime un exemplaire.

### Gestion des utilisateurs
- `selectUtilisateur` : récupère un utilisateur par son login et mot de passe pour l'authentification.

## Modifications de sécurité

- Le fichier `.htaccess` a été modifié pour retourner une erreur 400 si l'API est appelée sans paramètres.
- Les deux lignes suivantes ont été ajoutées pour que l'authentification Basic Auth fonctionne en production :
```
RewriteCond %{HTTP:Authorization} ^(.+)$
RewriteRule ^(.*)$ - [E=HTTP_AUTHORIZATION:%1]
```

## Triggers ajoutés

- `after_livraison` : quand une commande passe à l'étape "livrée", crée automatiquement les exemplaires correspondants avec un numéro séquentiel et l'état "neuf".

## Installation en local

- Installer les outils nécessaires : WampServer, NetBeans, Postman.
- Télécharger le zip du code et le dézipper dans le dossier `www` de WampServer. Renommer le dossier en `rest_mediatekdocuments`.
- Ouvrir une fenêtre de commandes en mode admin, se positionner dans le dossier de l'API et taper `composer install`.
- Récupérer le script `mediatek86.sql` en racine du projet puis, avec phpMyAdmin, créer la BDD `mediatek86` et exécuter le script.
- Ouvrir l'API dans NetBeans.
- Pour tester l'API avec Postman, il faut configurer l'authentification Basic Auth. Les identifiants sont à demander à l'administrateur.

## API en ligne

L'API est accessible en ligne à l'adresse suivante :
http://api-mediatekdocuments.nathan-boudier.com/

L'authentification est identique à celle en local.
