# ForgeMR — Constitution v1.0

> Ce document constitue la référence du projet ForgeMR.
> Toute évolution du projet doit respecter les principes définis ici.

## Pourquoi ce projet ?

ForgeMR est né d'un besoin simple : pouvoir reconstruire un environnement de travail fiable, reproductible et maintenable.

Le projet ne cherche pas uniquement à automatiser une installation. Il cherche à expliquer chaque décision afin que le lecteur comprenne ce qu'il met en place.

L'objectif est de construire un environnement professionnel tout en apprenant.

---

## Notre ambition

Construire progressivement un projet :

- simple ;
- modulaire ;
- documenté ;
- reproductible ;
- maintenable ;
- évolutif.

La première implémentation cible Ubuntu 26.04 LTS, avec une compatibilité recherchée pour TUXEDO OS et WebFAI.

L'architecture est conçue pour évoluer. Les implémentations arrivent progressivement.

---

## Nos convictions

- Comprendre avant d'automatiser.
- Documenter autant que développer.
- Privilégier la simplicité.
- Introduire la complexité uniquement lorsqu'elle devient nécessaire.
- Toujours privilégier les recommandations officielles.

---

## Principes d'ingénierie

ForgeMR applique notamment :

- KISS
- DRY
- YAGNI
- Convention over Configuration
- Idempotence
- Clean Architecture adaptée à Ansible
- Behavior Driven Development (BDD)

Le TDD n'est pas utilisé.

---

## Veille technologique

Avant chaque implémentation :

- vérifier les recommandations officielles les plus récentes ;
- signaler les pratiques obsolètes ;
- expliquer les évolutions importantes ;
- privilégier les approches recommandées.

Ordre de priorité :

1. Documentation officielle.
2. Guides officiels.
3. Bonnes pratiques reconnues.
4. Retours d'expérience pertinents.

---

## Méthode de travail

Chaque étape suit toujours le même cycle :

1. Présentation de l'objectif.
2. Explication du pourquoi.
3. Présentation des solutions possibles.
4. Recommandation argumentée.
5. Vérification des bonnes pratiques actuelles.
6. Implémentation.
7. Explication du code.
8. Tests.
9. Documentation.

Chaque étape nécessite une validation explicite avant la suivante.

---

## Behavior Driven Development

Avant toute implémentation, le comportement attendu est défini.

Le code vient ensuite satisfaire ce comportement.

---

## Architecture

Les playbooks orchestrent.

Les rôles implémentent.

Un nouveau rôle n'est créé que lorsqu'une responsabilité mérite une maintenance indépendante.

Le découpage est guidé par :

- la responsabilité ;
- la cohérence ;
- la maintenabilité.

---

## Première version

v0.0.1

Rôles :

- common
- shell
- development
- devops
- desktop
- dotfiles

Inventaire :

- localhost uniquement

Chaque rôle possède son propre tag.

Le rôle shell installe.

Le rôle dotfiles configure.

---

## Documentation

La documentation est un livrable.

Une fonctionnalité n'est terminée que lorsque sa documentation est terminée.

---

## Vulgarisation

Toute notion est expliquée simplement.

Lorsqu'un terme technique apparaît :

- il est défini ;
- expliqué avec des mots simples ;
- illustré lorsque cela apporte de la valeur.

Le jargon est limité au strict nécessaire.

L'objectif est de comprendre, pas seulement de reproduire.

---

## Communication

Les réponses doivent :

- être naturelles ;
- être précises ;
- être pédagogiques ;
- éviter le jargon inutile.

Les décisions validées deviennent la référence du projet.

Elles ne sont réévaluées qu'en cas :

- de nouvelle contrainte ;
- d'évolution des bonnes pratiques ;
- de demande explicite.

Le challenge est encouragé lorsqu'il apporte une réelle valeur.

La sur-analyse est évitée.

---

## Objectif final

Construire un environnement professionnel reproductible grâce à Ansible tout en créant un guide de référence permettant de comprendre les choix d'architecture, les bonnes pratiques et les décisions prises tout au long du projet.
