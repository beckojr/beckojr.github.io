+++
title = "Déployer Son Application Web sur Azure Avec App Service | Partie 1: Les bases d'App Service"
date = "2023-11-23T21:26:07+01:00"
author = "Becko Junior Camara"
authorTwitter = "" #do not include @
cover = ""
tags = ["Azure", "App Service", "Cloud", "Web"]
keywords = ["Azure", "App Service", "Cloud", "Web"]
# description = "Introduction à Azure App Service. Présentation du service, éléments constitutifs, fonctionnalités et tarification."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
draft = true
+++

Nous avons été contacté par un client qui a fait le choix d'Azure pour déployer son application de recommendation de restaurants: **FoodAdvisor**. Disposant d'une équipe technique restreinte et voulant se concentrer sur le développement de nouvelles fonctionnalités, nous avons conseillé à la startup le service **Azure App Service**.

L'application FoodAdvisor est basée sur le CMS Node.js [Strapi](https://strapi.io/). Elle dispose d'une interface web et stocke ses données dans une base de données PostgreSQL.

Dans cet article, nous allons procéder étape par étape au déploiement de l'application FoodAdvisor. Ce faisant, nous allons introduire les concepts d'Azure App Service qui nous permettront d'atteindre notre objectif.

Pour suivre et exécuter les instructions de cet article, vous aurez besoin des éléments suivants:

- Un compte Azure. Vous pouvez en créer un si vous n'avez pas de compte Azure,
- Une souscription dans la quelle vous pouvez créer des ressources,
- L'interface en ligne de commande Azure CLI installé sur votre poste de travail.

# De quoi Azure App Service est le nom?

App Service est de la famille des plateformes en tant que service (_PaaS - Platform as a Service_). Un PaaS est un modèle de service du cloud qui permet au développeur de déployer son application web sans se soucier de la gestion de l'infrastructure sous-jacente.

Comme défini dans le [modèle responsabilité partagée](https://learn.microsoft.com/fr-fr/azure/security/fundamentals/shared-responsibility) du cloud, avec un PaaS, le fournisseur de service cloud fournit la puissance de calcul nécessaire au fonctionnement de notre application web, ainsi que des interfaces (Web, API, CLI) de configuration. Le développeur a le choix du langage de programmation, du framework et autres configurations nécessaires au fonctionnement de leurs applications. Le fournisseur de service cloud de son côté a la responsabilité d'assurer la sécurité des machines virtuelles via des mise à jour régulières du système d'exploitation, du framework choisit par le développeurs, de la haute disponibilité du service.

App Service est un service régional qui fournit une ferme de machines virtuelles pour déployer des applications web (des applications accessibles via HTTP). Les niveaux tarifaires (les plans) d'App Service définissent les fonctionnalité disponibles et les tailles des machines virtuelles. Les fonctionnalités suivantes sont disponibles:

- Authentification (sans toucher au code l'application)
- Le déploiement sur plusieurs zones de disponiblités
- Nom de domaine personnalisé
- Intégration dans un VNET
- Un équilibreur de charge intégré
- La mise à l'échelle automatique
- Intégration avec Azure Monitor (pour la surveillance et les alertes)

**Note**

_Pour faire simple, j'utiliserai l'interface en ligne de commande pour créer les ressources Azure et déployer les applications web. Idéalement, il faudrait utiliser un outil d'infrastructure en tant que code comme Azure Resource Manager ou Terraform._

# App Service Plan pour commencer

Un plan App Service définit les caractéristiques des ressources de calcul nécessaires au fonctionnement de notre application web. Il définit les éléments suivants:

- Le système d'exploitation (Windows ou Linux)
- Le nombre d'instances et la taille des machines virtuelles (petite, moyenne, grande)
- Le niveau tarifaire (Gratuit, Partagé(seulement Windows), Basic, Standard, PremiumV2, PremiumV3, Isolé, IsoléV2)

Le plan choisi définit les fonctionnalités disponibles. Selon le plan choisi, notre application web est déployée dans un environnement:

- Partagé (Gratuit et Partagé): Les machines virtuelles et l'environnement réseau sont partagés avec d'autres clients Azure.
- Dédiée (Basic, Standard, PremiumV2, PremiumV3): Les machines virtuelles sont dédiées, mais l'environnement réseau est partagé avec d'autres clients Azure.
- Isolé (Isolé, IsoléV2): ce niveau tarifaire nécessite un environnement App Service (_App Service Environment - ASE_) dans lequel le plan sera créé. L'environnement App Service est utilise un réseau virtuel dédié dans le quel des machines virtuelles dédiées sont déployées.

## Choix du plan App Service pour FoodAdvisor

Pour le fonctionnement de l'application FoodAdvisor en production nous avons besoin des fonctionnalités suivantes:

- La haute disponibilité et la résilience
- Un environnement de qualification
- Un nom de domaine personnalisé
- Un CDN pour servir le contenu de notre interface web

Au regard des exigences, nous avons le choix d'un plan à partir du niveau Premium. C'est à partir du niveau Premium que nous avons le support du déploiement sur plusieurs zones de disponibilité.

Pour notre application FoodAdvisor, nous créerons un plan P0v3 (PremiumV3), avec Linux comme système d'exploitation, sur 3 zones de disponibilités.

**Note**

Il est recommandé d'avoir un plan par environnement (prod, nonprod, etc.). Pour notre client, un plan basic suffit pour les environnements hors production.

### Création du plan App Service

Avant de créer notre plan, il faut d'abord s'authentifier via la CLI Azure: `az login`. La page d'authentification d'Azure s'ouvrira dans votre navigateur par défaut.

```
az appservice plan create \
--resource-group rg-foodadvisor-frc-prod \
--name plan-foodadvisor-frc-prod \
--sku P0V3 --is-linux --zone-redundant
```

Cette commande crée notre plan App Service comme le montre l'image ci-dessous

[INSERT AZURE PORTAL SNIP]

# Création des applications web

Avec la création de notre plan App Service, les ressources nécessaires au fonctionnement de nos applications web sont disponibles. Pour rappel, l'application FoodAdvisor est composé d'une interface web, d'un CMS, et une base de données PostgreSQL.

[INSERT APPLICATION TECHNICAL FLOW]

```
# Création de l'application web 'front' (l'interface web)

az webapp create \
--name foodadvisor-api --plan plan-foodadvisor-frc-prod \
--resource-group rg-foodadvisor-frc-prod \
--runtime NODE:16-lts \
--assign-identity <> --role <> --scope <> \
--https-only # force la redirection en HTTPS

# Création de l'application web 'api' (le CMS)

az webapp create \
--name foodadvisor-api --plan plan-foodadvisor-frc-prod \
--resource-group rg-foodadvisor-frc-prod \
--runtime NODE:16-lts \
--assign-identity <> --role <> --scope <> \
--https-only # force la redirection en HTTPS
```

## Deployments slots

- Présentation
- Utilité

## Modes de déploiements

- Kudu: Webdeploy, zip, REST endpoint
- FTP

## Configuration du web app

- Utilisation de app settings pour les variables d'environnement
- Non utilisation de la connection string
- Intégration avec App Config et KeyVault pour le secret de la base

## Interconexion à la BD

- Rappelle du context multi-tenant de notre plan
- Règles réseaux à prendre en compte

# Day-2 operation

## Monitoring

- Différents niveaux de monitoring
- Diagnostic settings: Sorties possibles, metrics dispo

## Alerting

- Infra alerts
- Alertes disponibilité
