<img src="./imgs/zelros.svg" width="250">


# 600: Be ready to deploy

## 610: Run on different environments

Le déploiement doit se faire aussi simplement :
- en local, en mode développement
- en local, avec des containers
- en distant, avec des containers, et ce sur n'importe quel environnement.

Les tests E2E doivent fonctionner sur tous les environnements

Parler des differents environnements

- Local: environnement de test
- Acceptance: test interne Zelros (PO, SE)
- Staging: test par le client
- Production: live pour tous

Comment ?
Le déploiement doit se faire avec Helm


## 620: ReadOnly mode

Pour des raisons de sécurité, les containers doivent fonctionner en readonly.
Il ne doivent pas générer de fichier temporaire.

Toute donnée doit être stockée dans une base de données.

Si des fichiers temporaires sont nécessaires, nous devons utiliser un système de fichier monté en mémoire, qui sera détruit à l'arrêt du container.
