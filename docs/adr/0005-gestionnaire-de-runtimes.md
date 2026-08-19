# ADR-0005 — Gestionnaire de runtimes

**Statut :** Accepté
**Date :** 2026-07-30

## Contexte

ForgeMR vise à construire un environnement de travail complet, couvrant à terme le shell, le développement et le DevOps (cf. [ADR-0001](0001-perimetre-et-objectifs.md)). Ces usages impliquent l'exécution de code écrit dans plusieurs langages (Node.js, Python, et potentiellement d'autres selon les besoins des rôles `development` et `devops`), chacun disposant de sa propre notion de version.

Un **gestionnaire de runtimes** est un outil qui installe plusieurs versions d'un même langage ou runtime en parallèle sur une machine, et permet de basculer automatiquement de l'une à l'autre selon le projet dans lequel on se trouve, sans affecter le reste du système.

Un tel outil doit être retenu tôt, car il conditionne la façon dont les futurs rôles de ForgeMR installeront et exposeront les langages dont ils ont besoin.

## Problème

Sans gestionnaire de runtimes dédié, deux approches sont possibles, toutes deux problématiques :

- installer les langages directement sur le système (paquets système, installateurs officiels) : une seule version est alors disponible à la fois, ce qui empêche de faire cohabiter des projets ayant des besoins différents, et rend l'environnement moins reproductible d'une machine à l'autre ;
- retenir un gestionnaire différent par langage (par exemple nvm pour Node.js, pyenv pour Python...) : chaque outil a sa propre syntaxe, son propre mécanisme d'activation et son propre fichier de configuration, ce qui multiplie la complexité et va à l'encontre de la simplicité recherchée par la Constitution du projet.

ForgeMR a besoin d'une solution unique, cohérente avec plusieurs langages, simple à installer et à maintenir.

## Décision

ForgeMR retient **mise** comme gestionnaire de runtimes officiel du projet, à la place de nvm.

mise gère un très grand nombre de runtimes et d'outils associés avec une seule commande et une seule configuration (`mise.toml`), quel que soit le langage concerné. Il permet en outre de déclarer des variables d'environnement et des tâches de projet, ce qui dépasse la simple gestion de versions tout en restant cohérent avec elle.

Le choix détaillé, les critères de comparaison et les alternatives sont documentés dans [docs/comparisons/mise-vs-nvm.md](../comparisons/mise-vs-nvm.md).

## Alternatives étudiées

**nvm**

nvm est un gestionnaire de versions limité à Node.js. Il ne répond qu'à une partie du besoin de ForgeMR : le futur rôle `development` nécessitera d'autres langages, ce qui obligerait à ajouter un gestionnaire supplémentaire par langage. Cette approche a été écartée car elle multiplie les outils, les syntaxes et les mécanismes d'activation à maintenir, sans apporter de bénéfice par rapport à une solution unique.

**Installation système**

Installer les runtimes directement via les paquets du système ou leurs installateurs officiels a également été écarté. Cette approche ne permet pas de faire cohabiter plusieurs versions d'un même langage, dépend fortement de la distribution utilisée, et ne garantit pas la reproductibilité d'un environnement à l'autre — un objectif central de ForgeMR (cf. [ADR-0001](0001-perimetre-et-objectifs.md)).

## Conséquences positives

- Un seul outil pour gérer l'ensemble des runtimes du projet, quel que soit le langage.
- Une configuration centralisée et versionnable (`mise.toml`) pour les runtimes, les variables d'environnement et les tâches.
- Une meilleure reproductibilité des environnements construits par ForgeMR, alignée sur l'idempotence recherchée par [ADR-0003](0003-principes-conception-roles.md).
- Un impact généralement plus faible sur le démarrage du shell, cohérent avec les objectifs du rôle `shell`.
- Une compatibilité conservée avec l'écosystème de plugins d'asdf, en cas de besoin futur.
- Un nombre d'outils réduit que les contributeurs doivent apprendre, configurer et maintenir, par rapport à un gestionnaire différent par langage.

## Conséquences négatives

- mise est un outil plus récent que nvm, avec une communauté moins étendue sur le seul périmètre Node.js.
- Introduction d'un concept supplémentaire (les « backends ») à comprendre pour certains outils.
- Nécessite un travail de migration pour les usages qui reposeraient déjà sur nvm.

## Impacts sur ForgeMR

- Le rôle `shell` installe mise et met en place son activation (`mise activate`) dans la configuration du shell, à la place de nvm.
- Le rôle `development`, désormais implémenté, utilise mise comme mécanisme unique de gestion de ses runtimes (Node.js, pnpm). Le rôle `devops`, désormais implémenté sans mise, n'en avait pas besoin : ses outils (Docker, Kubernetes, sécurité) ne sont pas des runtimes de langage.
- La terminologie « gestionnaire de runtimes » devient la terminologie de référence dans la documentation de ForgeMR pour désigner ce type d'outil, et remplace toute mention antérieure de « gestionnaire de versions des outils ».

## Références

- [docs/comparisons/mise-vs-nvm.md](../comparisons/mise-vs-nvm.md) — comparaison détaillée ayant justifié cette décision.
- [Documentation officielle de mise](https://mise.jdx.dev/)
- [Dépôt GitHub de nvm](https://github.com/nvm-sh/nvm)
- [Documentation officielle de Node.js](https://nodejs.org/en/docs)
