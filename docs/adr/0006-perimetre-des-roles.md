# ADR-0006 — Périmètre des rôles pour la v0.0.1

**Statut :** Accepté
**Date :** 2026-08-18

## Contexte

Le rôle `shell` étant terminé, ForgeMR doit fixer, avant leur implémentation, le périmètre exact des rôles `development` et `devops` pour la v0.0.1, ainsi que la frontière entre ce que ForgeMR installe ou configure sur la machine et ce qui reste de la responsabilité des projets qu'il prépare. Sans cette clarification, ces rôles risqueraient d'empiéter progressivement sur des responsabilités qui ne sont pas les leurs (dépendances de projet, services applicatifs), à l'encontre du principe de responsabilité unique posé par [ADR-0002](0002-architecture-generale.md) et [ADR-0003](0003-principes-conception-roles.md).

## Décision

### Rôle `shell` — terminé

Responsabilité : environnement shell interactif. Le contenu, les variables et les vérifications du rôle sont documentés dans [docs/roles/shell.md](../roles/shell.md) ; cette décision confirme que son périmètre est figé pour la v0.0.1.

### Rôle `development`

Responsabilité : outils de développement, uniquement.

Contenu v0.0.1 :

- Git
- Node.js, installé via mise (cf. [ADR-0005](0005-gestionnaire-de-runtimes.md))
- pnpm, installé via mise

Décisions figées :

- npm n'est jamais installé explicitement : il est fourni par Node.js.
- Node.js et pnpm sont installés uniquement via mise, jamais par un autre mécanisme (paquet système, installateur officiel...).
- Aucune installation globale via npm.
- Aucune dépendance propre à un framework ou à un projet.

### Rôle `devops`

Responsabilité : outils permettant d'exécuter des services, uniquement.

Contenu v0.0.1 :

- Docker Engine
- Docker Compose

Les outils suivants ne seront ajoutés que lorsqu'un besoin réel apparaît, conformément au principe YAGNI de la [Constitution](../../CONSTITUTION.md) : kubectl, Helm, k9s, Azure CLI, et tout autre outil équivalent.

### ForgeMR ne gère jamais les dépendances d'un projet

Les dépendances propres à un projet applicatif — par exemple Vue, Vite, TypeScript, Vue Router, Sass, Vitest, Vue Test Utils, ou Pinia (uniquement lorsqu'un besoin réel apparaît) — n'appartiennent jamais à ForgeMR. Elles sont installées uniquement via le `package.json` du projet concerné, jamais par un rôle ForgeMR.

Cette décision précise, pour les dépendances de projet, le principe déjà posé par [ADR-0001](0001-perimetre-et-objectifs.md) selon lequel le code métier des applications reste dans leurs dépôts respectifs.

### ForgeMR ne gère jamais les services applicatifs

Un service applicatif — par exemple Directus ou PostgreSQL — n'est jamais installé directement sur le système par ForgeMR. Il est fourni uniquement via Docker Compose, exécuté grâce aux outils mis en place par le rôle `devops`.

## Conséquences

- Le rôle `development` n'installera jamais npm explicitement, ni de paquet global via npm.
- Le rôle `devops` reste limité à Docker Engine et Docker Compose tant qu'aucun besoin réel ne justifie l'ajout d'un outil supplémentaire.
- Aucun rôle ForgeMR n'installera jamais Vue, Vite, TypeScript, Vue Router, Sass, Vitest, Vue Test Utils, Pinia, Directus ou PostgreSQL directement sur le système.
- Cette frontière s'applique à tous les rôles actuels et futurs de ForgeMR, pas seulement à `development` et `devops`.

## Références

- [ADR-0001 — Périmètre et objectifs de ForgeMR](0001-perimetre-et-objectifs.md)
- [ADR-0002 — Architecture générale de ForgeMR](0002-architecture-generale.md)
- [ADR-0003 — Principes de conception des rôles Ansible](0003-principes-conception-roles.md)
- [ADR-0005 — Gestionnaire de runtimes](0005-gestionnaire-de-runtimes.md)
- [docs/roles/shell.md](../roles/shell.md)
