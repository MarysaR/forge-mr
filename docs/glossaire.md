# Glossaire

Ce glossaire fait partie des fondations de ForgeMR et a le même niveau d'importance que les autres documents du projet.

## Règles

- Tout acronyme utilisé pour la première fois dans le projet est ajouté ici.
- Tout terme technique important est ajouté ici.
- Chaque définition est rédigée dans un langage simple, sans supposer de connaissance préalable.
- Un exemple concret est ajouté lorsqu'il améliore la compréhension.
- C'est un document vivant : il est mis à jour au fil des étapes du projet.

## Termes

### ADR (Architecture Decision Record)

Un court document qui explique une décision d'architecture importante : le contexte, les options envisagées, le choix retenu et pourquoi. Il sert de mémoire pour ne pas refaire les mêmes débats plus tard.

Exemple : un ADR pourrait expliquer pourquoi ForgeMR utilise Ansible plutôt qu'un simple script shell.

### Ansible

Un outil qui permet de décrire, dans des fichiers texte, l'état voulu d'une machine (paquets installés, fichiers de configuration...), puis d'appliquer automatiquement cette description. C'est l'outil central de ForgeMR.

### Ansible Galaxy

Une plateforme communautaire où l'on peut trouver et partager des rôles et des collections Ansible réutilisables, un peu comme un magasin d'extensions.

### Ansible Vault

Un mécanisme d'Ansible qui permet de chiffrer des informations sensibles (mots de passe, clés) dans un fichier, pour ne jamais les stocker en clair dans le dépôt.

### BDD (Behavior Driven Development)

Une méthode de travail où l'on décrit d'abord le comportement attendu d'une fonctionnalité ("dans telle situation, il doit se passer telle chose") avant d'écrire le code qui le réalise. ForgeMR utilise le BDD plutôt que le TDD.

### Branche (Git)

Une ligne de développement indépendante dans un dépôt Git, qui permet de travailler sur une modification sans affecter le code déjà validé.

Exemple : `feature/role-shell`.

### Clean Architecture

Une manière d'organiser un projet en séparant clairement les responsabilités (ce qui orchestre, ce qui implémente, ce qui configure), pour que chaque partie puisse évoluer sans casser les autres.

### Collection (Ansible)

Un ensemble empaqueté de rôles, modules et plugins Ansible, distribué via Ansible Galaxy.

### Commit (Git)

Un instantané des modifications apportées à un dépôt, accompagné d'un message qui explique ce qui a changé et pourquoi.

### Convention over Configuration

Un principe qui consiste à adopter des règles par défaut cohérentes (nommage, emplacement des fichiers...) plutôt que de tout configurer manuellement à chaque fois.

### Conventional Commits

Une convention d'écriture des messages de commit Git, sous la forme `type: description` (par exemple `feat: ajoute le rôle shell`), qui rend l'historique du projet plus lisible.

### Dépôt (Git)

L'espace qui contient l'ensemble des fichiers d'un projet ainsi que tout leur historique de modifications.

### DRY (Don't Repeat Yourself)

Un principe qui consiste à éviter de dupliquer une même information ou une même logique à plusieurs endroits du projet.

### EditorConfig

Un format de fichier (`.editorconfig`) qui permet à différents éditeurs de code d'appliquer automatiquement les mêmes règles de mise en forme (indentation, encodage, fin de ligne...).

### .gitkeep

Un fichier vide, sans signification particulière pour Git, utilisé par convention pour forcer un dossier vide à être versionné. Git ne suit jamais un dossier vide par lui-même : il faut donc y placer un fichier, même vide.

### Idempotence

Une propriété qui garantit qu'exécuter une action plusieurs fois produit toujours le même résultat que l'exécuter une seule fois.

Exemple : réinstaller un paquet déjà installé ne doit rien casser ni le réinstaller inutilement.

### INI

Un format de fichier de configuration simple, organisé en sections (`[section]`) et en paires `clé = valeur`. Utilisé par `ansible.cfg`.

### Inventaire (Ansible)

La liste des machines sur lesquelles Ansible doit agir. Pour ForgeMR en v0.0.1, l'inventaire ne contient que `localhost` (la machine locale elle-même).

### Keep a Changelog

Une convention qui définit comment structurer un fichier `CHANGELOG.md` de façon lisible et cohérente entre les projets.

### KISS (Keep It Simple, Stupid)

Un principe qui consiste à toujours préférer la solution la plus simple qui répond au besoin, plutôt qu'une solution plus complexe mais pas nécessairement plus utile.

### LTS (Long Term Support)

Une version d'un logiciel (ici, Ubuntu) qui bénéficie d'un support et de mises à jour de sécurité pendant une durée plus longue que les versions courantes.

### Playbook (Ansible)

Un fichier qui décrit un scénario d'exécution : quelles machines sont concernées, et quels rôles y appliquer. C'est la couche qui orchestre, par opposition au rôle qui implémente.

### Rôle (Ansible)

Un ensemble organisé de tâches, fichiers et variables, responsable d'une seule fonctionnalité bien délimitée (par exemple : installer un shell). C'est la couche qui implémente, par opposition au playbook qui orchestre.

### SemVer (Semantic Versioning)

Une convention de numérotation des versions sous la forme `MAJOR.MINOR.PATCH` (par exemple `1.2.3`), où chaque partie a une signification précise sur l'ampleur du changement apporté.

### Tag (Ansible)

Une étiquette posée sur des tâches ou un rôle, qui permet d'exécuter sélectivement une partie d'un playbook.

Exemple : ne lancer que le rôle `shell` sans exécuter les autres.

### Tag (Git)

Un repère posé sur un commit précis pour marquer une version publiée.

Exemple : `v0.0.1`.

### TDD (Test Driven Development)

Une méthode où l'on écrit d'abord un test qui échoue, puis le code qui le fait réussir. ForgeMR n'utilise pas cette méthode (voir BDD).

### TUXEDO OS

Une distribution Linux basée sur Ubuntu, développée par le fabricant de matériel TUXEDO Computers, livrée par défaut sur leurs ordinateurs. ForgeMR vise une compatibilité avec cette distribution.

### WebFAI

Une interface web pour FAI (Fully Automatic Installation), un outil de déploiement automatisé de systèmes Linux par le réseau, notamment utilisé dans certains établissements d'enseignement. ForgeMR vise une compatibilité avec les environnements déployés de cette manière.

### YAGNI (You Aren't Gonna Need It)

Un principe qui consiste à ne pas construire une fonctionnalité tant qu'elle n'est pas réellement nécessaire, même si elle semble utile "pour plus tard".

### YAML

Un format de fichier texte, lisible par un humain, très utilisé par Ansible pour décrire des playbooks, des rôles et des variables.
