# Conception du rôle `shell`

**Statut :** Validé
**Date :** 2026-07-29

## 1. Objectif du rôle

**Responsabilité**
Installer et mettre en place un environnement shell interactif fonctionnel pour l'utilisateur : interpréteur de commandes, thème/prompt, éditeur de terminal, multiplexeur de terminal, et un ensemble d'outils en ligne de commande modernes qui améliorent l'usage quotidien du terminal.

**Périmètre**
Tout ce qui concerne l'expérience interactive en ligne de commande sur la machine cible : zsh et son écosystème (framework, thème), les outils CLI listés ci-dessous, ainsi que la configuration minimale (alias, fonctions, variables d'environnement) nécessaire pour que cet environnement soit utilisable dès l'installation.

**Ce qu'il ne doit volontairement pas gérer**
- La création ou la gestion des comptes utilisateurs et de leurs permissions.
- L'installation d'outils liés à un langage de programmation ou à des besoins de développement/devops.
- Les particularités liées à une distribution ou un matériel spécifique (ex. TUXEDO OS).
- Toute donnée personnelle ou secret.

## 2. Fonctionnalités couvertes

- **Zsh** — interpréteur de commandes qui remplace le shell par défaut ; c'est la fondation sur laquelle repose tout le reste de l'environnement interactif.
- **Oh My Zsh** — framework de gestion pour zsh (plugins, thèmes) ; il structure et facilite la configuration de zsh, ce qui en fait un composant naturel de son installation.
- **Powerlevel10k** — thème/prompt pour zsh ; il fait partie intégrante de l'expérience du shell et dépend directement de zsh/Oh My Zsh.
- **Neovim (configuration minimaliste)** — éditeur de terminal ; une configuration minimale en fait un outil de base de l'environnement shell, sans empiéter sur un futur rôle dédié au développement.
- **tmux** — multiplexeur de terminal ; il permet de gérer plusieurs sessions/fenêtres dans un même terminal, un besoin central de l'usage quotidien en ligne de commande.
- **fzf** — recherche floue interactive ; s'intègre directement dans zsh pour améliorer la navigation (historique, fichiers...).
- **bat** — alternative à `cat` avec coloration syntaxique ; outil d'affichage de fichiers utilisé au quotidien dans le terminal.
- **eza** — alternative moderne à `ls` ; outil de listing de fichiers, usage quotidien du shell.
- **ripgrep** — recherche de texte rapide dans les fichiers ; complément direct du shell pour explorer le système de fichiers.
- **fd** — alternative moderne à `find` ; recherche de fichiers, usage quotidien du shell.
- **zoxide** — navigation intelligente entre dossiers (complète `cd`) ; s'intègre directement dans le shell interactif.
- **Alias** — raccourcis de commandes qui simplifient l'usage quotidien du shell mis en place par ce rôle.
- **Fonctions shell** — petits scripts réutilisables qui étendent les capacités du shell, dans la continuité directe de sa configuration.
- **Variables d'environnement du shell** — variables nécessaires au bon fonctionnement des outils installés par ce rôle (ex. préférences d'un outil, chemins utilisés par le shell).

## 3. Variables publiques

Uniquement les points de personnalisation exposés à l'utilisateur (sans valeurs, sans YAML) :

- Activation ou non du changement de shell par défaut vers zsh.
- Liste des plugins Oh My Zsh à activer.
- Activation ou non de l'assistant de configuration interactif de Powerlevel10k.
- Activation ou non de la configuration minimaliste de Neovim fournie par le rôle.
- Activation ou non de la configuration de base de tmux fournie par le rôle.
- Activation ou non de chacun des outils CLI (fzf, bat, eza, ripgrep, fd, zoxide) individuellement.
- Liste des alias personnalisés à ajouter.
- Liste des fonctions shell personnalisées à ajouter.
- Liste des variables d'environnement personnalisées à définir.

## 4. Découpage logique des tâches

1. Installation des dépendances nécessaires au rôle.
2. Installation de zsh.
3. Installation du framework Oh My Zsh.
4. Installation du thème Powerlevel10k.
5. Installation de Neovim et mise en place de sa configuration minimaliste.
6. Installation de tmux et de sa configuration de base.
7. Installation des outils CLI modernes (fzf, bat, eza, ripgrep, fd, zoxide).
8. Mise en place des alias, fonctions et variables d'environnement définis par le rôle.
9. Changement du shell par défaut de l'utilisateur, si l'option est activée.
10. Vérification finale que l'environnement est fonctionnel.

## 5. Critères d'acceptation

- zsh est installé et disponible comme interpréteur sur la machine.
- zsh est le shell par défaut de l'utilisateur, si l'option correspondante est activée.
- Oh My Zsh est installé et chargé au démarrage d'une session zsh.
- Le thème Powerlevel10k est actif et visible dans le prompt.
- Neovim est installé et démarre avec la configuration minimaliste du rôle.
- tmux est installé et une session démarre avec la configuration de base du rôle.
- Chaque outil CLI activé (fzf, bat, eza, ripgrep, fd, zoxide) est installé et accessible en ligne de commande.
- Les alias, fonctions et variables d'environnement définis par le rôle sont disponibles dans une nouvelle session shell.
- Ré-exécuter le rôle sur un système déjà configuré ne produit aucune modification supplémentaire (idempotence, cf. ADR-0003).

## 6. Arborescence du rôle

### 6.1 Arborescence complète

```
roles/shell/
├── README.md
├── defaults/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   ├── main.yml
│   ├── dependencies.yml
│   ├── zsh.yml
│   ├── oh_my_zsh.yml
│   ├── powerlevel10k.yml
│   ├── neovim.yml
│   ├── tmux.yml
│   ├── cli_tools.yml
│   ├── shell_config.yml
│   ├── default_shell.yml
│   └── verify.yml
└── templates/
    ├── zshrc.j2
    ├── aliases.j2
    ├── functions.j2
    ├── environment.j2
    ├── neovim.j2
    ├── tmux.conf.j2
    └── p10k.zsh.j2
```

### 6.2 Rôle de chaque élément

**`README.md`**
Documente le rôle : son objectif, son fonctionnement, les variables qu'il expose, ses prérequis. Existe en application du principe de documentation posé par l'ADR-0003 et par l'ADR-0001.

**`defaults/main.yml`**
Contient les valeurs par défaut de toutes les variables publiques identifiées dans la conception fonctionnelle (section 3) : activation du changement de shell par défaut, plugins Oh My Zsh, activation des outils CLI, listes d'alias/fonctions/variables d'environnement, etc. Permet au rôle de fonctionner sans configuration obligatoire (ADR-0004).

**`meta/main.yml`**
Décrit l'identité du rôle (description, plateformes supportées) et déclare ses dépendances — ici, aucune. Pratique officielle recommandée pour tout rôle Ansible, cohérente avec l'ADR-0002 (« les rôles sont les plus indépendants possible »).

**`tasks/`**
Contient la logique d'exécution du rôle, découpée en plusieurs fichiers (détail en 6.3).

**`templates/`**
Contient l'ensemble des fichiers de configuration déposés par le rôle — que leur contenu dépende de variables aujourd'hui (`.zshrc`, alias, fonctions, variables d'environnement) ou soit actuellement fixe (Neovim, tmux, Powerlevel10k). Un mécanisme unique est utilisé pour tous ces fichiers plutôt que de séparer `files/` et `templates/` : cela évite une migration si ces configurations statiques deviennent un jour paramétrables, et limite le rôle à un seul mécanisme de dépôt de fichier à retenir.

### 6.3 Découpage du dossier `tasks/`

`main.yml` sert uniquement de point d'entrée : il inclut les fichiers suivants, dans cet ordre, qui reprend le découpage validé en section 4 :

1. **`dependencies.yml`** — installe les dépendances nécessaires au rôle.
2. **`zsh.yml`** — installe l'interpréteur zsh, sans toucher à la configuration ni au shell par défaut.
3. **`oh_my_zsh.yml`** — installe le framework Oh My Zsh.
4. **`powerlevel10k.yml`** — installe le thème.
5. **`neovim.yml`** — installe Neovim et dépose sa configuration minimaliste.
6. **`tmux.yml`** — installe tmux et dépose sa configuration de base.
7. **`cli_tools.yml`** — installe les outils CLI complémentaires (fzf, bat, eza, ripgrep, fd, zoxide), regroupés dans un seul fichier car ils partagent la même responsabilité fonctionnelle (enrichir l'usage du terminal par des utilitaires indépendants).
8. **`shell_config.yml`** — crée `~/.config/zsh/` et y dépose les fichiers d'alias, de fonctions et de variables d'environnement, ainsi que le `.zshrc` à la racine de `$HOME` qui les charge. Responsabilité unique : assembler la configuration finale du shell à partir de ce qui a été installé précédemment.
9. **`default_shell.yml`** — change le shell par défaut de l'utilisateur si l'option est activée. Séparé du reste car il modifie un compte utilisateur, une nature d'action différente d'une installation ou d'un dépôt de fichier.
10. **`verify.yml`** — vérifie que les critères d'acceptation définis en section 5 sont remplis. Centralisé plutôt que dispersé dans chaque fichier d'installation, pour rester directement traçable par rapport à cette section, et parce que l'échec d'une installation est déjà interrompu nativement par Ansible sans vérification manuelle supplémentaire.

Chaque fichier correspond à une seule étape de la conception validée en section 4, et à une seule nature d'action (installer, configurer, modifier un compte, ou vérifier), conformément au principe de responsabilité unique fixé par l'ADR-0003.

### 6.4 Organisation de la configuration Zsh déployée

Les fichiers d'alias, de fonctions et de variables d'environnement sont déployés dans `~/.config/zsh/` (`aliases.zsh`, `functions.zsh`, `environment.zsh`), chargés depuis un `.zshrc` unique à la racine de `$HOME`. Cette organisation regroupe toute la configuration du shell dans un emplacement dédié plutôt que de multiplier les fichiers cachés à la racine de `$HOME`, ce qui la rend plus lisible, plus maintenable et plus évolutive si le rôle gagne de nouveaux fragments de configuration.

### 6.5 Dossiers volontairement absents

- **`files/`** — fusionné dans `templates/` (cf. 6.2), pour ne conserver qu'un seul mécanisme de dépôt de fichier dans le rôle.
- **`handlers/`** — aucun composant du rôle ne fonctionne comme un service à redémarrer ; aucune tâche n'a besoin de déclencher un handler.
- **`vars/`** — aucune valeur interne n'est aujourd'hui partagée entre plusieurs fichiers de tâches ; elles peuvent rester directement dans le fichier de tâche qui les utilise (ADR-0004).
- **`library/`, `module_utils/`, `filter_plugins/`** — le rôle n'a besoin d'aucun module ou filtre Ansible personnalisé ; les modules natifs suffisent (ADR-0003).
- **`tests/`** — aucun ADR ne définit encore de stratégie de test pour les rôles ForgeMR ; l'ajouter maintenant anticiperait une décision non prise.
