# ADR-0004 — Gestion de la configuration des rôles

**Statut :** Accepté
**Date :** 2026-07-24

## Contexte

Les rôles de ForgeMR devront souvent s'adapter à des besoins ou des environnements différents : c'est le rôle des variables, qui permettent de personnaliser un comportement sans modifier le code. Mais une variable ajoutée sans besoin réel devient une source de confusion plutôt qu'un atout.

Sans règles communes, chaque rôle risque de gérer sa configuration à sa manière : certains trop rigides (tout codé en dur), d'autres trop permissifs (variables inutiles à chaque endroit). Fixer dès maintenant des principes simples permet de garantir que la configuration des rôles reste lisible, cohérente et réellement utile — au service de la réutilisabilité définie par l'ADR-0003.

## Décision

**Configuration par variables**

Une variable est une valeur nommée qui peut être définie par celui qui utilise un rôle, pour en adapter le comportement sans toucher à son code. Le comportement d'un rôle doit être configurable par des variables lorsque cela apporte une réelle valeur.

Une variable ne doit pas être créée si elle ne répond à aucun besoin concret.

**Valeurs par défaut**

Une valeur par défaut est la valeur qu'une variable prend automatiquement si personne ne la définit explicitement. Elle permet à un rôle de fonctionner correctement sans que son utilisateur ait à tout configurer.

Un rôle doit fournir des valeurs par défaut raisonnables afin de fonctionner sans configuration inutile.

**Valeurs en dur**

Une valeur en dur (ou « codée en dur ») est une valeur écrite directement dans le code du rôle, sans passer par une variable. Il faut éviter les valeurs en dur lorsqu'elles sont susceptibles d'évoluer ou de différer selon les environnements.

À l'inverse, une valeur qui fait partie de la responsabilité même du rôle, et qui n'a pas vocation à être personnalisée, peut rester en dur.

**Simplicité**

Chaque variable ajoute de la complexité : elle doit être comprise, documentée et maintenue. Une option de configuration ne doit être ajoutée que si elle apporte une réelle valeur d'usage.

Il vaut mieux quelques variables utiles que des dizaines d'options rarement utilisées.

**Documentation**

Chaque variable destinée à être modifiée par l'utilisateur doit être documentée avec :

- son objectif ;
- sa valeur par défaut ;
- son impact lorsqu'elle est modifiée ;
- dans quels cas il est pertinent de la modifier (lorsque ce n'est pas évident).

## Principes

- Privilégier des valeurs par défaut pertinentes.
- Éviter les variables inutiles.
- Ne pas rendre configurable ce qui n'a pas besoin de l'être.
- Privilégier la simplicité et la lisibilité.

## Conséquences

L'application de ces règles permettra :

- des rôles plus simples à comprendre ;
- moins d'erreurs de configuration ;
- une meilleure réutilisabilité ;
- une maintenance facilitée ;
- une évolution plus sereine des rôles.
