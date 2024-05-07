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



## Filtres