<img src="./imgs/zelros.svg" width="250">


# 400: Always monitor

Add the Request-Id and Session-Id header to trace request (see "basics)


## 410: Test API contract

Toujours coder une route en TDD.

Les développeurs doivent écrire une série de tests E2E qui listent la plupart des cas.
Les tests doivent être écrit avant de commencer le code de l'API

Quand-doivent être jouer les tests ?

- pendant le développement
- à chaque release
- si possible, dans une partie du monitoring

Avoir une stratégie de Mockup


## 420: Monitor SLA

Dans cette partie, nous nous assurons que l'API est "vivante", et quelle n'est pas tombée.
Toute alerte doit être remontées à l'équipe et un ticket est générée.
Chaque ticket doit être pris en compte.

Ces tickets doivent être traités en priorité car sinon ils vont s'accumuler.

On doit s'intégrer dans un système de monitoring existant.

Une API doit avoir plusieurs profondeurs de tests, qui s'exécutent chacun à des intervalles différents.

Par exemple, 

- on execute un simple test HTTP 200 OK sur une route toutes les minutes (c'est ce que fait l'API gateway d'Azure).
- on execute un test E2E complet toutes les 15 minutes ou toutes les heures.

Remarque: tout dépend du cout du test. Si le test E2E est très rapide, on peut l'exécuter toutes les minutes!

Réfléchir au comment.


## 430: Log business informations

Expliquer la distincition entre les informations business et les informations technicals

Le but des logs business est de réaliser :
- un suivi des opérations sur l'API avec une vue fonctionnel
- des statistiques d'utilisation de l'API

Tous les accès aux données en lecture ou modification doivent être loggués

Indiquer aussi ce que l'on doit logguer :
- l'identifiant de l'utilisateur réalisant l'opération
- le timestamp de l'operation en UTC (<= ce qui nécessite d'avoir des serveurs réglés en UTC)
- la nature de l'operation
- les données complémentaires

Il est aussi important de distinguer "configuration" et "consommation" pour des raisons de limitations d'appels (vérifier le nombre d'appel de l'API par une personne)

Réfléchir au comment.


## 440: Log technical informations

Les logs techniques sont tous les logs des services: 
- logs de debug
- logs des applications des bases de données
- logs des accès technique aux API
- etc.

Remarque: ces logs sont très nombreux.
