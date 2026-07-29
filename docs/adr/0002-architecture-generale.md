# ADR-0002 — Architecture générale de ForgeMR

**Statut :** Accepté
**Date :** 2026-07-24

## Contexte

Avant de développer le premier rôle, ForgeMR a besoin d'une architecture stable : une répartition claire des responsabilités entre ses composants. Sans cela, chaque nouvelle fonctionnalité risque d'être placée arbitrairement, au détriment de la maintenabilité et de la lisibilité recherchées par le projet (cf. ADR-0001). Cette architecture doit être fixée une fois, tôt, pour servir de cadre à toutes les décisions d'implémentation à venir.

## Décision

**Dépôt Git**
Responsabilité : contenir l'ensemble du code, de la configuration et de la documentation de ForgeMR, avec leur historique.
Ne doit pas : contenir de secrets ni de données personnelles en clair.

**Playbooks**
Responsabilité : orchestrer — décrire quelles machines sont concernées et quels rôles y appliquer.
Ne doit pas : contenir de logique d'implémentation (installation, configuration détaillée) ; cette logique appartient aux rôles.

**Rôles**
Responsabilité : implémenter une fonctionnalité précise et délimitée.
Ne doit pas porter la responsabilité de l'orchestration du projet. Les dépendances de rôles via `meta/main.yml` peuvent être utilisées lorsqu'elles sont justifiées, mais elles doivent rester exceptionnelles. Un rôle ne doit pas devenir un mini-playbook.

**Inventaires**
Responsabilité : décrire les cibles d'exécution (machines, groupes ou environnements) sur lesquelles Ansible agit.
Ne doit pas : contenir de logique d'exécution ou de configuration applicative.

**Documentation**
Responsabilité : décrire, expliquer et justifier l'architecture et les décisions du projet.
Ne doit pas : piloter ou conditionner l'exécution du projet — elle documente ce qui existe, elle ne l'exécute pas.

**Fichiers de configuration globaux**
Responsabilité : définir le comportement global d'Ansible pour le projet (ex. emplacement de l'inventaire, des rôles).
Ne doit pas : contenir de logique métier ni de configuration spécifique à un rôle.

## Principes d'architecture

- Une responsabilité par composant.
- Les playbooks orchestrent.
- Les rôles exécutent.
- Les rôles sont les plus indépendants possible.
- La documentation décrit l'architecture mais ne pilote jamais l'exécution.
- L'architecture privilégiée est simple, lisible et modulaire.

## Conséquences

- Meilleure maintenabilité : chaque composant peut évoluer sans affecter les autres.
- Meilleure évolutivité : un nouveau besoin trouve naturellement sa place dans l'architecture.
- Meilleure lisibilité : la responsabilité de chaque fichier est prévisible.
- Facilité de test : des composants indépendants sont plus simples à valider isolément.
- Réduction du couplage : limiter les dépendances entre composants limite les effets de bord lors des évolutions.
