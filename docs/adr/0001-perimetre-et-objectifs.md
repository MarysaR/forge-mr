# ADR-0001 — Périmètre et objectifs de ForgeMR

**Statut :** Accepté
**Date :** 2026-07-24

## Objectif

ForgeMR est un projet Ansible dont l'objectif est de construire, configurer et maintenir un environnement de travail de manière :

- reproductible ;
- documentée ;
- modulaire ;
- maintenable ;
- pédagogique.

L'automatisation n'est pas une fin en soi : chaque action doit pouvoir être comprise, expliquée et reproduite.

## Ce que ForgeMR fait

ForgeMR est responsable de :

- installer les outils nécessaires ;
- configurer ces outils ;
- appliquer les bonnes pratiques retenues ;
- documenter les choix techniques ;
- reconstruire un environnement de travail de manière fiable et reproductible ;
- préparer et, lorsque cela est pertinent, déployer les composants nécessaires à un environnement de développement, de test, de laboratoire ou de production.

## Ce que ForgeMR ne fait pas

ForgeMR n'a pas vocation à :

- remplacer un outil de gestion de parc informatique ;
- contenir le code métier des applications qu'il prépare ou déploie ;
- contenir des données personnelles ou des secrets ;
- devenir une collection de scripts sans architecture ni documentation.

ForgeMR peut déployer une application ou une infrastructure lorsque cela contribue à son objectif d'automatisation et de reproductibilité. En revanche, le code source, la logique métier et les données de ces applications doivent rester dans leurs dépôts respectifs.

## Principes

Toute nouvelle fonctionnalité doit :

- répondre à une responsabilité clairement identifiée ;
- respecter la Constitution du projet ;
- être documentée ;
- être testable ;
- être réutilisable ;
- apporter une valeur réelle au projet.

La simplicité est privilégiée. La complexité n'est introduite que lorsqu'elle est justifiée par un besoin concret.

## Critère d'intégration

Avant d'ajouter une nouvelle fonctionnalité, la question suivante doit toujours être posée :

> « Cette fonctionnalité relève-t-elle réellement du périmètre et des objectifs de ForgeMR ? »

Si la réponse est non, elle doit être développée dans un projet distinct.

## Révision

Cette décision constitue la référence pour la suite de la conception de ForgeMR. Elle pourra être remise en question à l'avenir si un argument technique solide, une évolution des besoins ou des bonnes pratiques le justifie — auquel cas elle sera modifiée ou remplacée par un nouvel ADR, jamais réinterprétée implicitement.
