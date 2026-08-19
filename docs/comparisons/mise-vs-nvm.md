# Comparaison — mise vs nvm

**Statut :** Référence
**Date :** 2026-07-30

## Contexte

Un environnement de développement moderne a besoin d'exécuter du code écrit dans différents langages (JavaScript, Python, Ruby, Go...), souvent avec plusieurs versions différentes selon les projets. Installer ces langages directement sur le système, sans outil dédié, pose rapidement problème : impossible d'avoir deux versions de Node.js en même temps, mises à jour qui cassent un projet plus ancien, environnement différent d'une machine à l'autre.

Un **gestionnaire de runtimes** répond à ce besoin : il installe plusieurs versions d'un même outil en parallèle, et bascule automatiquement de l'une à l'autre selon le projet dans lequel on se trouve.

ForgeMR doit choisir un tel outil pour équiper les environnements qu'il construit. Ce document compare les deux candidats étudiés — **nvm** et **mise** — afin de justifier le choix retenu par [ADR-0005](../adr/0005-gestionnaire-de-runtimes.md).

## Présentation de nvm

**nvm** (*Node Version Manager*) est un gestionnaire de versions dédié exclusivement à Node.js. C'est l'outil historique et le plus utilisé dans l'écosystème JavaScript.

**Fonctionnement**

nvm n'est pas un programme exécutable classique : c'est une **fonction shell** que l'on charge (« source ») dans son terminal via `.bashrc` ou `.zshrc`. Cette fonction modifie le `PATH` de la session en cours pour pointer vers la version de Node.js choisie.

Un projet peut définir la version de Node.js qu'il attend dans un fichier `.nvmrc` placé à sa racine. nvm remonte l'arborescence des dossiers pour le trouver et propose d'activer automatiquement la bonne version.

**Commandes principales**

| Commande | Rôle |
|---|---|
| `nvm install <version>` | Télécharge et installe une version de Node.js |
| `nvm use <version>` | Active une version dans la session courante |
| `nvm ls` | Liste les versions installées localement |
| `nvm ls-remote` | Liste les versions disponibles au téléchargement |
| `nvm alias default <version>` | Définit la version utilisée par défaut |

**Compatibilité**

nvm nécessite un shell compatible POSIX (bash, zsh, ksh, dash). Il ne fonctionne pas nativement sous Fish, et son support de Windows passe obligatoirement par WSL, Git Bash ou Cygwin — il n'existe pas de version native pour `cmd.exe` ou PowerShell.

## Présentation de mise

**mise** (prononcé « mize », pour *mise en place*) est un gestionnaire d'environnement de développement polyglotte. Il ne se limite pas à un seul langage : il gère les versions d'un très grand nombre de runtimes ainsi que de nombreux outils associés (Node.js, Python, Ruby, Go, Rust, Java, Terraform, kubectl, entre autres), ainsi que les variables d'environnement et les tâches d'un projet.

**Fonctionnement**

mise est un binaire unique, écrit en Rust, installable indépendamment de tout shell (`curl https://mise.run | sh`). Il propose deux modes d'intégration :

- **`mise activate`** : ajouté au fichier de configuration du shell, il met à jour le `PATH` et les variables d'environnement à chaque nouvelle invite de commande. C'est le mode recommandé pour un usage interactif.
- **Shims** : mise dépose de petits exécutables qui interceptent les appels aux outils gérés, sans nécessiter d'intégration au shell. Ce mode convient mieux aux environnements non interactifs (CI/CD, scripts).

La configuration d'un projet est centralisée dans un fichier `mise.toml`, qui peut définir à la fois les outils requis, les variables d'environnement et des tâches personnalisées (build, test, déploiement...) :

```toml
[tools]
node = "24"
python = "3.13"

[env]
NODE_ENV = "development"

[tasks.test]
run = "npm test"
```

mise reste compatible avec le fichier `.tool-versions` utilisé par asdf (un autre gestionnaire polyglotte plus ancien), ce qui facilite la migration depuis cet outil.

**Commandes principales**

| Commande | Rôle |
|---|---|
| `mise use <outil>@<version>` | Installe et active un outil pour le projet courant |
| `mise install` | Installe les outils déclarés dans `mise.toml` |
| `mise ls` | Liste les outils et versions installés |
| `mise exec <outil>@<version> -- <commande>` | Exécute une commande avec une version précise, sans l'activer durablement |
| `mise run <tâche>` | Exécute une tâche définie dans `mise.toml` |
| `mise trust` | Valide un fichier de configuration avant de l'exécuter |

## Comparaison détaillée

**Portée fonctionnelle**

nvm gère un seul outil : Node.js. Pour gérer Python, Ruby ou Go, il faut installer un gestionnaire différent pour chacun (pyenv, rbenv, goenv...), chacun avec sa propre syntaxe et son propre mécanisme d'activation. mise gère l'ensemble de ces outils avec une seule commande, une seule configuration et un seul mécanisme d'activation.

**Performance**

nvm, en tant que fonction shell interprétée, ajoute un temps de chargement mesurable à chaque ouverture de terminal. mise, écrit en Rust et compilé en binaire natif, est conçu pour un impact généralement plus faible sur le démarrage du shell.

**Au-delà des versions**

nvm se limite strictement à l'installation et à l'activation de versions de Node.js. mise va plus loin : il permet aussi de définir des variables d'environnement par projet et d'exécuter des tâches (`mise run`), ce qui recouvre une partie des besoins habituellement couverts par des scripts séparés ou des outils comme `direnv` ou `make`.

**Sécurité**

nvm télécharge les binaires officiels de Node.js depuis les sources publiées par le projet Node.js. mise s'appuie, pour certains outils, sur différents mécanismes d'installation (« backends ») ; la vérification cryptographique des artefacts téléchargés (signatures Cosign, attestations SLSA, Minisign) n'est pas systématique et dépend du backend utilisé et de l'outil concerné.

**Plateformes**

nvm dépend d'un shell POSIX et ne supporte pas nativement Windows. mise fonctionne nativement sur Linux, macOS et Windows, sans dépendre d'un shell compatible POSIX.

**Maturité et adoption**

nvm est un outil plus ancien, très largement utilisé et documenté dans l'écosystème JavaScript, avec une communauté importante. mise est plus récent (anciennement nommé `rtx`) ; son adoption progresse, en particulier dans les contextes polyglottes et DevOps, mais sa communauté reste plus restreinte que celle de nvm sur le seul périmètre Node.js.

## Tableau comparatif

| Critère | nvm | mise |
|---|---|---|
| Langages/outils gérés | Node.js uniquement | Un très grand nombre d'outils et de runtimes (Node.js, Python, Ruby, Go, Rust, Java, Terraform...) |
| Implémentation | Fonction shell (bash/zsh/ksh/dash) | Binaire natif (Rust) |
| Configuration déclarative du projet | `.nvmrc` (version Node.js uniquement) | `mise.toml` (outils, variables d'environnement, tâches) |
| Gestion des variables d'environnement | Non | Oui |
| Gestion de tâches | Non | Oui (`mise run`) |
| Support Windows natif | Non (WSL/Git Bash/Cygwin requis) | Oui |
| Support Fish | Non nativement | Oui |
| Impact sur le démarrage du shell | Mesurable | Généralement plus faible |
| Vérification cryptographique des installations | Non documentée | Variable (dépend du backend et de l'outil) |
| Compatibilité asdf | Non | Oui (`.tool-versions`, plugins asdf) |
| Maturité / communauté | Élevée, spécifique à Node.js | Croissante, polyglotte |

## Avantages / Inconvénients

**nvm**

Avantages :
- outil de référence historique pour Node.js, très documenté ;
- communauté large, nombreux exemples et retours d'expérience ;
- simple à comprendre pour qui ne gère qu'un seul langage.

Inconvénients :
- limité à Node.js : un outil supplémentaire est nécessaire par langage ;
- pas de gestion des variables d'environnement ni des tâches ;
- ralentit le démarrage du shell ;
- support Windows indirect uniquement.

**mise**

Avantages :
- un seul outil pour tous les langages et runtimes du projet ;
- centralise versions, variables d'environnement et tâches dans un seul fichier ;
- démarrage de shell quasiment sans surcoût ;
- support natif multiplateforme, y compris Windows ;
- compatible avec l'écosystème de plugins existant d'asdf.

Inconvénients :
- outil plus récent, communauté moins étendue que celle de nvm sur le périmètre Node.js seul ;
- introduit un concept supplémentaire (« backends ») à comprendre pour les outils hors du cœur natif ;
- pour une équipe qui ne travaille que sur du Node.js, une partie de ses fonctionnalités reste inutilisée.

## Pourquoi ForgeMR choisit mise

ForgeMR construit un environnement de travail destiné à couvrir plusieurs besoins : shell, développement, DevOps. Ces besoins impliquent, à terme, plusieurs langages et outils en ligne de commande différents (Node.js, Python, outils DevOps comme Terraform ou kubectl...).

Retenir un gestionnaire limité à un seul langage obligerait à empiler un outil différent par écosystème, chacun avec sa propre logique d'activation — ce qui va à l'encontre des principes de simplicité et de cohérence posés par la Constitution du projet. mise répond à ce besoin avec un seul outil, une seule configuration et un seul mécanisme d'activation, quel que soit le langage concerné.

Ce choix est cohérent avec les principes d'ingénierie retenus par la Constitution : un outil unique pour tous les langages va dans le sens du principe KISS et évite la duplication de configuration (DRY) qu'imposerait un gestionnaire différent par langage, tandis que sa capacité à reproduire un même environnement à l'identique sur plusieurs machines s'inscrit dans la recherche d'idempotence qui structure ForgeMR.

Son impact généralement plus faible sur le démarrage du shell est également cohérent avec le rôle `shell` de ForgeMR (cf. [docs/roles/shell.md](../roles/shell.md)), qui vise un environnement interactif réactif dès l'ouverture du terminal.

## Cas d'usage DevOps

Le contexte DevOps illustre bien l'intérêt d'un gestionnaire de runtimes polyglotte :

- un pipeline d'intégration continue peut avoir besoin de Node.js pour builder un frontend, de Python pour des scripts d'automatisation, et de Terraform pour l'infrastructure — mise gère les trois avec une seule configuration versionnée (`mise.toml`) ;
- les tâches (`mise run`) permettent de définir dans le même fichier les commandes de build, de test ou de déploiement, plutôt que de les disperser dans des scripts séparés ;
- la reproductibilité entre postes de développement, serveurs de CI et environnements de production est facilitée : la même version d'un outil est installée partout à partir de la même déclaration, un principe directement lié à l'idempotence recherchée par ForgeMR (cf. [ADR-0003](../adr/0003-principes-conception-roles.md)) ;
- l'activation automatique par dossier évite les erreurs classiques de « mauvaise version active » lors du passage d'un projet à un autre.

## Impact sur le projet

L'adoption de mise dans ForgeMR implique :

- l'installation de mise remplace celle de nvm dans les rôles concernés ;
- l'activation de mise (`mise activate`) est ajoutée à la configuration du shell mise en place par le rôle `shell` ;
- le futur rôle `development` s'appuiera sur mise pour déclarer les versions d'outils dont il a besoin, plutôt que sur des gestionnaires spécifiques à chaque langage ; le rôle `devops`, désormais implémenté, n'en a pas eu besoin (ses outils ne sont pas des runtimes de langage) ;
- la terminologie « gestionnaire de runtimes » est désormais celle employée dans la documentation de ForgeMR pour désigner ce type d'outil (cf. [ADR-0005](../adr/0005-gestionnaire-de-runtimes.md)).

## Références officielles

- [Documentation officielle de mise](https://mise.jdx.dev/)
- [Dépôt GitHub de mise](https://github.com/jdx/mise)
- [Comparaison officielle mise vs asdf](https://mise.jdx.dev/dev-tools/comparison-to-asdf.html)
- [Dépôt GitHub de nvm](https://github.com/nvm-sh/nvm)
- [Documentation officielle de Node.js](https://nodejs.org/en/docs)
- [Documentation officielle d'asdf](https://asdf-vm.com/guide/introduction.html)
