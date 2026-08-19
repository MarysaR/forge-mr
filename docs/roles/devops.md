# Rôle `devops`

**Statut :** Documentation officielle
**Date :** 2026-08-19

## Objectif

Le rôle `devops` installe et configure les outils permettant d'exécuter des services sur la machine : conteneurs Docker, cluster Kubernetes local, analyse de sécurité des images.

Il ne gère jamais les services applicatifs eux-mêmes (par exemple Directus ou PostgreSQL) ni les dépendances propres à un projet : ces responsabilités restent hors de son périmètre, conformément à [ADR-0006](../adr/0006-perimetre-des-roles.md).

## Fonctionnalités

Le rôle installe :

- Docker : Docker Engine et le plugin Docker Compose (V2, `docker compose`), via le dépôt apt officiel de Docker ; buildah, via les dépôts Ubuntu (composant universe) ;
- Kubernetes : kubectl via son dépôt apt officiel ; Helm via son script d'installation officiel (`get-helm-3`) ; k9s et Minikube (si l'option est activée) par téléchargement direct de leur paquet `.deb` ;
- Sécurité : trivy, via le dépôt apt officiel d'Aqua Security.

Le rôle configure :

- l'appartenance de l'utilisateur au groupe `docker`, si l'option est activée.

## Variables publiques

| Variable | Valeur par défaut | Description |
|---|---|---|
| `devops_install_minikube` | `true` | Installe Minikube, qui provisionne un cluster Kubernetes local. À désactiver sur une machine qui se connecte uniquement à un cluster distant, où Minikube n'a aucune utilité. |
| `devops_set_docker_group` | `true` | Ajoute l'utilisateur au groupe `docker`. |

## Utilisation

```yaml
- hosts: localhost
  roles:
    - role: devops
```

Pour personnaliser le rôle, surcharger les variables publiques dans le playbook ou l'inventaire :

```yaml
- hosts: localhost
  roles:
    - role: devops
      vars:
        devops_install_minikube: false
```

## Vérifications

Le rôle vérifie :

- que Docker Engine, le plugin Docker Compose V2 (`docker compose` — jamais l'ancien binaire autonome `docker-compose`) et buildah sont installés ;
- que, si `devops_set_docker_group` est activé, l'utilisateur appartient au groupe `docker` ;
- que kubectl, Helm et k9s sont installés ;
- que, si `devops_install_minikube` est activé, Minikube est installé ;
- que trivy est installé.

## Limites de la v0.0.1

- Aucun template de configuration n'est fourni : aucun des outils du rôle n'a besoin d'un fichier de configuration généré par ForgeMR pour fonctionner.
- Le rôle cible les distributions basées sur apt officiellement supportées par ForgeMR (Ubuntu 24.04 LTS, TUXEDO OS) ; aucune autre famille de distribution n'est prise en charge.
- Le périmètre est volontairement figé : la liste des outils explicitement exclus tant qu'aucun besoin réel n'apparaît est documentée dans [ADR-0006](../adr/0006-perimetre-des-roles.md), qui reste la référence unique pour ne pas la dupliquer ici.

## Évolutions envisagées

- Confirmer, pour chaque outil, que la méthode d'installation retenue reste la recommandation officielle la plus récente (revue prévue séparément, après le gel de cette version du rôle).

## Références

- [ADR-0003 — Principes de conception des rôles Ansible](../adr/0003-principes-conception-roles.md)
- [ADR-0004 — Gestion de la configuration des rôles](../adr/0004-gestion-configuration-roles.md)
- [ADR-0006 — Périmètre des rôles pour la v0.0.1](../adr/0006-perimetre-des-roles.md)
