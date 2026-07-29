# ADR-0003 — Principes de conception des rôles Ansible

**Statut :** Accepté
**Date :** 2026-07-24

## Contexte

ForgeMR sera composé de plusieurs rôles Ansible, développés à des moments différents, pour des besoins différents. Sans règles communes, chaque rôle risque d'être conçu selon une logique propre à son auteur ou au moment de sa création, ce qui nuirait à la cohérence du projet.

Définir dès maintenant un cadre de qualité partagé permet de garantir que tous les rôles, présents et futurs, restent cohérents entre eux, de bonne qualité, réutilisables et faciles à maintenir — conformément aux principes fixés par la Constitution et par l'ADR-0002.

## Décision

**Responsabilité**

Un rôle doit avoir une responsabilité claire et unique : il répond à un seul besoin bien défini (par exemple : gérer la configuration du shell). Il ne doit pas chercher à résoudre plusieurs problématiques sans lien direct entre elles.

**Idempotence**

L'idempotence est une propriété simple à comprendre : exécuter une action plusieurs fois produit toujours le même résultat que l'exécuter une seule fois. Concrètement, réinstaller un paquet déjà présent ne doit rien casser, ni le réinstaller inutilement.

Un rôle doit pouvoir être exécuté plusieurs fois de suite sans produire d'effets de bord ni modifier un système déjà conforme à l'état voulu.

**Réutilisabilité**

Un rôle est réutilisable lorsqu'il peut être utilisé dans des contextes différents sans que son code interne ait besoin d'être modifié — seul son paramétrage (ses variables) change d'un usage à l'autre.

Un même rôle doit donc pouvoir être appelé depuis plusieurs playbooks différents sans modifier son code.

Un rôle ne doit pas dépendre d'un environnement spécifique pour fonctionner, sauf si cette contrainte fait explicitement partie de sa responsabilité. Par exemple, un rôle dédié à TUXEDO OS peut naturellement s'appuyer sur cet environnement, alors qu'un rôle générique doit rester indépendant de la plateforme.

**Configuration**

Le comportement d'un rôle doit être piloté par des variables lorsque cela apporte une réelle valeur (par exemple : permettre de choisir quelle version d'un outil installer). Il faut éviter de multiplier les options de configuration sans besoin concret : chaque variable ajoutée est une complexité supplémentaire à comprendre et à maintenir.

**Dépendances**

Les dépendances entre rôles doivent rester exceptionnelles et être justifiées. L'orchestration — décider quels rôles s'exécutent, dans quel ordre, sur quelles machines — reste la responsabilité des playbooks, pas des rôles (cf. ADR-0002).

**Documentation**

Chaque rôle doit être documenté afin d'expliquer :

- son objectif ;
- son fonctionnement ;
- les variables qu'il expose ;
- les éventuels prérequis.

## Principes

- Simplicité avant complexité.
- Lisibilité avant optimisation.
- Privilégier les fonctionnalités natives d'Ansible plutôt que des contournements.
- Éviter les dépendances inutiles.
- Favoriser la réutilisation plutôt que la duplication.

## Conséquences

L'application de ces règles à tous les rôles de ForgeMR permettra :

- des rôles cohérents entre eux ;
- une maintenance facilitée ;
- une meilleure évolutivité ;
- une réduction des effets de bord ;
- une meilleure compréhension du projet, y compris pour un lecteur qui le découvre ;
- une réduction du risque de régression lors des évolutions.
