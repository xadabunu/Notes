# Trajets d'une requête HTTP

## Injection Container

## Middlewares

La requête passe tout d'abord par une série de middlewares ayant 
chacun la possibilité de court-circuiter la requête, la renvoyant directement
au client.
Les middlewares ont différents rôles et sont toujours parcourus dans le même ordre :
- Authentification : qui envoie la requête ?
- Autorisation : a-t-il le droit d'envoyer la requête ?
- Journalisation : garde une trace de toutes les informations importantes pour les rendre accessibles plus tard
- Compression : réduit la taille des données envoyées au client pour améliorer les performances
- et d'autres
- Routing : détermine l'endpoint (méthode du serveur) d'une requête sur le serveur

## Binding

Le binding consiste à mapper les données de la requête HTTP aux paramètres des méthodes du contrôleur. Il permet de récupérer les valeurs envoyées dans la requête et de les lier aux paramètres de méthode dans le code de l'application.

## Filtres

Les filtres sont des composants qui permettent d'ajouter une logique supplémentaire à l'exécution des actions de contrôleur. Ils peuvent être utilisés pour effectuer des opérations telles que la validation des données, la gestion des erreurs, la mise en cache, etc. Voici quelques types de filtres couramment utilisés :

- Filtres d'action : Exécutent du code avant ou après l'exécution d'une action de contrôleur.
- Filtres de résultat : Modifient le résultat retourné par une action de contrôleur avant qu'il ne soit envoyé au client.
- Filtres d'exception : Gèrent les exceptions qui se produisent lors de l'exécution d'une action de contrôleur.
