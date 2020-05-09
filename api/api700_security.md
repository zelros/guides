<img src="./imgs/zelros.svg" width="250">


## 700: Secure the API

### 710: Accept only secure connection

- Require secure connections with TLS to access the API, without exception.
- Reject any non-TLS requests by not responding to requests
- Do not redirect non-TLS requests (it discards sloppy clients)


### 720: Authenticate all requests

L'authentication doit être native pour chacune des routes.

On doit parler de l'authentification des rôles/droit d'accès.

On utilisera keycloak pour ce sujet.

Il faut éviter au maximum l'utilisation des comptes de service.
> En fait, la règle est la granularité maximum.
> Obligatoire par client à minima. Au mieux, à l'utilisateur.

Donner des exemples pour la gestion des tokens.

Les tokens doivent expirer dans le temps.
Les tokens doivent avoir un mécanisme de renewal.

OAuth2 only

Comment ?

A voir comment faire avec Keycloak

TLS for Transport


### 730: Minimize informations in production error
