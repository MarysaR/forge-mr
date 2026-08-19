# Rôle `development`

**Statut :** Documentation officielle
**Date :** 2026-08-19

## Objectif

Le rôle `development` installe les outils de développement nécessaires au projet préparé par ForgeMR : Node.js et pnpm, tous deux gérés par mise.

Il ne gère jamais les dépendances propres à un projet (frameworks, bibliothèques...) ni Git, déjà fourni par le rôle `shell` : ces responsabilités restent hors de son périmètre, conformément à [ADR-0006](../adr/0006-perimetre-des-roles.md).

## Fonctionnalités

Le rôle installe :

- Node.js, via mise ;
- pnpm, via mise.

npm n'est jamais installé explicitement (fourni par Node.js), et aucune installation globale via npm n'est effectuée.

## Variables publiques

Aucune pour la v0.0.1. En particulier, la version de Node.js n'est pas configurable : ForgeMR prépare une machine pour un projet précis, la version retenue est une décision d'architecture du rôle, pas un paramètre utilisateur (YAGNI — à revoir si un besoin multi-projets réel apparaît).

## Utilisation

```yaml
- hosts: localhost
  roles:
    - role: development
```

## Vérifications

Le rôle vérifie que mise, Node.js et pnpm sont correctement installés.

## Limites de la v0.0.1

- Aucune variable publique : rien à personnaliser sans modifier le rôle.
- Aucun template de configuration n'est fourni.
- Le rôle cible les distributions basées sur apt officiellement supportées par ForgeMR (Ubuntu 24.04 LTS, TUXEDO OS) ; aucune autre famille de distribution n'est prise en charge.

## Références

- [ADR-0003 — Principes de conception des rôles Ansible](../adr/0003-principes-conception-roles.md)
- [ADR-0005 — Gestionnaire de runtimes](../adr/0005-gestionnaire-de-runtimes.md)
- [ADR-0006 — Périmètre des rôles pour la v0.0.1](../adr/0006-perimetre-des-roles.md)
- [docs/roles/shell.md](shell.md) — fournit Git et mise, prérequis de ce rôle.
