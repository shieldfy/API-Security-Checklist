[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# API Security Checklist
Checklist des points de sécurité les plus importants lors de la conception, du test et de la mise en production de votre API.


---

## Authentification
- [ ] Ne pas utiliser une authentification basique http (`Basic Auth`) mais plutôt un standard d'authentification (tel que JWT, OAuth).
- [ ] Ne pas réinventer la roue lors de `l'authentification`, `la génération de token`, `le stockage de mots de passe` mais utiliser les standards.
- [ ] Lors de l'authentification, mettre en place les fonctionnalités de bannissement (`jail`) avec des seuils de tentatives maximales (`Max Retry`).
- [ ] Chiffrer toutes les données sensibles.

### JWT (JSON Web Token)
- [ ] Utiliser des clés aléatoires complexes (`JWT Secret`) pour rendre les attaques par force brute difficiles.
- [ ] Ne pas extraire l'algorithme du payload. Imposer l'algorithme côté serveur (`HS256` ou `RS256`).
- [ ] Rendre la durée de vie des tokens (`TTL`, `RTTL`) aussi courte que possible.
- [ ] Ne pas stocker des informations sensibles du payload JWT, son décryptage est très [simple](https://jwt.io/#debugger-io).

### OAuth
- [ ] Toujours valider la redirection d'uri (`redirect_uri`) côté serveur afin d'accéder uniquement aux URLs autorisées.
- [ ] Toujours utiliser un échange de code plutôt que des tokens (ne pas autoriser `response_type=token`).
- [ ] Utiliser le paramètre d'état (`state`) avec un hash aléatoire pour prévenir les CSRF sur le processus d'authentification OAuth.
- [ ] Définir la portée par défaut et valider le paramètre de portée pour chaque application.

## Accès
- [ ] Limiter le nombre de requêtes (limitation de bande passante) pour éviter les dénis de service et les attaques par force brute.
- [ ] Utiliser le protocole HTTPS côté serveur afin d'éviter les attaques de l'homme du milieu (MITM).
- [ ] Utiliser les entêtes `HSTS` avec SSL pour éviter les attaques SSL Strip.

## Entrées
- [ ] Utiliser la bonne méthode en fonction de l'opération, `GET (lire)`, `POST (créer)`, `PUT (remplacer/mettre à jour)` et `DELETE (pour supprimer un enregistrement)`.
- [ ] Valider le `content-type` dans l'en-tête HTTP des requêtes (négociation de contenu) pour n'autoriser que les formats supportés (e.g. `application/xml`, `application/json`, etc…) et renvoyer une réponse `406 Not Acceptable` si ça ne correspond pas.
- [ ] Valider le `content-type` des données postées avec celles acceptées (e.g. `application/x-www-form-urlencoded`, `multipart/form-data, application/json`, etc…).
- [ ] Valider les entrées utilisateur pour éviter les vulnérabilités classiques (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc…).
- [ ] N'utiliser aucune donnée sensible (`identifiants`, `mots de passe`, `tokens de sécurité`, ou `clés d'API`) dans l'URL, mais utiliser les en-têtes d'autorisations standards.
- [ ] Utiliser un service de passerelle d'API afin d'obtenir la mise en cache, une limitation de la saturation des ressources, la gestion des pics d'activités et le déploiement automatique des ressources.

## Traitement
- [ ] Vérifier qu'aucun point d'entrée dans l'application n'échappe à l'authentification.
- [ ] Éviter l'utilisation des identifiants de ressource utilisateur. Préférer `/me/orders` au lieu de `/user/654321/orders`
- [ ] Ne pas utiliser d'identifiant auto-incrémenté mais plutôt des `UUID`.
- [ ] Dans le cas du traitement de fichiers XML, être sûr que l'analyse des entités n'est pas activée par défaut afin d'éviter les failles `XXE` (XML external entity attack).
- [ ] Dans le cas du traitement de fichiers XML, être sûr que l'expansion des entités n'est pas activée par défaut afin d'éviter les `Billion Laughs/XML bomb` (exponential entity expansion attack).
- [ ] Utiliser les réseaux de diffusion de contenu (CDN) pour l'envoie de fichier.
- [ ] Dans le cas du traitement d'importantes quantités de données, utiliser des Workers et des Queues pour retourner les réponses rapidement et éviter un blocage HTTP.
- [ ] Ne pas oublier de désactiver le mode DEBUG.

## Sorties
- [ ] Envoyer l'en-tête `X-Content-Type-Options: nosniff`.
- [ ] Envoyer l'en-tête `X-Frame-Options: deny`.
- [ ] Envoyer l'en-tête `Content-Security-Policy: default-src 'none'`.
- [ ] Supprimer les en-têtes d'empreinte - `X-Powered-By`, `Server`, `X-AspNet-Version`, etc…
- [ ] Imposer le `content-type` des réponses, si la réponse est du `application/json` alors l'en-tête `content-type` est `application/json`.
- [ ] Ne pas retourner de données sensibles dans les réponses `identifiants`, `mots de passe`, `tokens de sécurité`.
- [ ] Retourner un code de statuts en adéquation avec l'opération effectuée. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc…).

## CI & CD
- [ ] Vérifiez votre conception et votre implémentation avec une couverture des tests unitaires et d'intégration.
- [ ] Utilisez un processus de revue de code et ignorez l'auto-approbation.
- [ ] Assurez-vous que tous les composants de vos services sont scannés par un logiciel anti-virus avant la mise en production, ainsi que les bibliothèques tierces et autres dépendances.
- [ ] Concevez une solution de rollback pour les déploiements.


---

## Voir également :
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Une collection de ressources utiles pour créer des API RESTful HTTP+JSON.


---

# Contribution
N'hésitez pas à contribuer en forkant ce dépôt, faire quelques changements, et soumettre une pull request. Pour toute question, envoyez un courriel à `team@shieldfy.io`.
