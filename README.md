# ForgeMR

ForgeMR est un projet Ansible visant à reconstruire un environnement de travail fiable, reproductible et maintenable — tout en expliquant chaque décision prise pour le construire.

Le projet ne se limite pas à automatiser une installation : il documente le raisonnement derrière chaque choix, pour que le lecteur comprenne ce qu'il met en place plutôt que de le reproduire aveuglément.

Version actuelle : `v0.0.1` — mise en place des fondations du dépôt (structure, conventions, documentation de base).

## Documentation

- [CONSTITUTION.md](./CONSTITUTION.md) — principes, méthode de travail et convictions du projet.
- [ROADMAP.md](./ROADMAP.md) — suivi des versions.
- [CHANGELOG.md](./CHANGELOG.md) — historique des changements.
- [docs/glossaire.md](./docs/glossaire.md) — définition simple de chaque terme technique et acronyme utilisé dans le projet.
- [docs/](./docs) — documentation détaillée et décisions d'architecture (ADR).

## Cible

Première implémentation pour Ubuntu 26.04 LTS, avec une compatibilité recherchée pour TUXEDO OS et WebFAI.

## État du projet

Les rôles (`common`, `shell`, `development`, `devops`, `desktop`, `dotfiles`) ne sont pas encore implémentés : seule l'architecture du dépôt est posée à ce stade.
