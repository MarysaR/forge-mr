# Rôle `shell`

**Statut :** Documentation officielle
**Date :** 2026-07-30

## Objectif

Le rôle `shell` installe et configure un environnement shell interactif pour l'utilisateur : interpréteur de commandes, thème, éditeur de terminal, multiplexeur de terminal et outils CLI modernes.

Il agit uniquement sur le compte existant qui exécute Ansible : pas de création de comptes, pas d'outils de développement ou DevOps, pas de données personnelles ni de secrets.

## Fonctionnalités

Le rôle installe :

- zsh ;
- Oh My Zsh et le thème Powerlevel10k, si l'option est activée ;
- Neovim, si l'option est activée ;
- les outils CLI listés par `shell_tools` (par exemple fzf, bat, eza, ripgrep, fd, zoxide, tmux) ;
- mise, le gestionnaire de runtimes retenu par ForgeMR (cf. [ADR-0005](../adr/0005-gestionnaire-de-runtimes.md)), si l'option est activée.

Le rôle configure :

- le fichier `.zshrc` de l'utilisateur ;
- les alias déposés par le rôle ;
- le shell par défaut de l'utilisateur, si l'option est activée ;
- la configuration de base de tmux, si `tmux` fait partie de `shell_tools` ;
- la configuration minimaliste de Neovim, si l'option est activée.

Le rôle vérifie que chacun de ces éléments est effectivement en place (cf. section [Vérifications](#vérifications)).

## Variables publiques

| Variable | Valeur par défaut | Description |
|---|---|---|
| `shell_set_default` | `true` | Change le shell par défaut de l'utilisateur vers zsh une fois installé. |
| `shell_install_oh_my_zsh` | `true` | Installe le framework Oh My Zsh ainsi que le thème Powerlevel10k. |
| `shell_plugins` | `[git]` | Liste des plugins Oh My Zsh à activer. |
| `shell_tools` | `[fzf, bat, eza, ripgrep, fd, zoxide, tmux]` | Liste des outils CLI à installer aux côtés du shell. Retirer un outil de cette liste pour ne pas l'installer. |
| `shell_install_neovim` | `true` | Installe Neovim avec la configuration minimaliste du rôle. |
| `shell_install_mise` | `true` | Installe mise, le gestionnaire de runtimes retenu par ForgeMR. |

## Fichiers et répertoires créés

Dans le répertoire personnel de l'utilisateur :

- `.zshrc` — configuration zsh.
- `.config/zsh/aliases.zsh` — alias déposés par le rôle.
- `.oh-my-zsh/` — installation d'Oh My Zsh, avec le thème Powerlevel10k, si activé.
- `.tmux.conf` — configuration de tmux, si activé.
- `.config/nvim/init.vim` — configuration de Neovim, si activé.

## Utilisation

```yaml
- hosts: localhost
  roles:
    - role: shell
```

Pour personnaliser le rôle, surcharger les variables publiques dans le playbook ou l'inventaire :

```yaml
- hosts: localhost
  roles:
    - role: shell
      vars:
        shell_install_neovim: false
        shell_tools:
          - fzf
          - ripgrep
```

Désactiver une variable après une première exécution (par exemple `shell_install_mise: false`) ne désinstalle pas le composant déjà en place.

## Vérifications

Le rôle vérifie :

- que zsh est installé, que `.zshrc` et le fichier d'alias sont présents et que, si `shell_set_default` est activé, zsh est le shell par défaut de l'utilisateur ;
- que, si `shell_install_oh_my_zsh` est activé, Oh My Zsh et le thème Powerlevel10k sont installés ;
- que, si `tmux` fait partie de `shell_tools`, il est installé et sa configuration est déployée ;
- que, si `shell_install_neovim` est activé, Neovim est installé et sa configuration est déployée ;
- que, si `shell_install_mise` est activé, le gestionnaire de runtimes `mise` est installé ;
- que les fichiers déposés par le rôle (`.zshrc`, alias, configuration tmux, configuration Neovim) sont bien ceux générés par ForgeMR.

## Limites de la v0.0.1

- Le thème Oh My Zsh est figé à Powerlevel10k : il n'est pas configurable.
- Aucune personnalisation des alias, fonctions ou variables d'environnement du shell au-delà de ce que le rôle fournit lui-même.
- Le rôle cible les distributions basées sur apt officiellement supportées par ForgeMR (Ubuntu 24.04 LTS, TUXEDO OS) ; aucune autre famille de distribution n'est prise en charge.

## Évolutions envisagées

- Rendre le thème Oh My Zsh configurable, si un besoin concret apparaît.
- Étendre la personnalisation du shell (alias, fonctions, variables d'environnement propres à l'utilisateur).
- Les futurs rôles `development` et `devops` s'appuieront sur mise pour la gestion de leurs runtimes (cf. [ADR-0005](../adr/0005-gestionnaire-de-runtimes.md)).

## Références

- [ADR-0003 — Principes de conception des rôles Ansible](../adr/0003-principes-conception-roles.md)
- [ADR-0004 — Gestion de la configuration des rôles](../adr/0004-gestion-configuration-roles.md)
- [ADR-0005 — Gestionnaire de runtimes](../adr/0005-gestionnaire-de-runtimes.md)
