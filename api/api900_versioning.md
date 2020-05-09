<img src="./imgs/zelros.svg" width="250">


# 900: Manage multiple versions

Le versionnement est exposé dans le standard de l'API.

Nous devons au maximum gérer la backward compatibility.

A chaque nouvelle version:
1/ La base doit etre la nouvelle version
2/ La précédente version doit être un proxy sur la nouvelle version

Et pas l'inverse!

<Mettre un schéma pour être plus explicite>

Le versioning est facultatif. S'il n'est pas précisé, on renvois tjs la dernière version

Once your API is declared production-ready and stable, do not make backwards incompatible changes within that API version. If you need to make backwards-incompatible changes, create a new API with an incremented version number.
