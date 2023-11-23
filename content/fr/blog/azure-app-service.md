+++
title = "Déployer Son Application Web Sur Azure Avec Azure App Service | Partie 1: Présentation d'Azure App Service"
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
+++

## Introduction

Qui veut déployer une application web aujourd'hui a l'embarra du choix tant les solutions de déploiement d'applications web foisonnent. Rien que sur AWS par exemple, il existe un peu plus de 17 façons différentes de déployer un conteneur. Les fournisseurs services cloud rivalisent d'innovations pour faciliter et moins pénible le déploiement d'applications web.

Face à cette panoplie de solutions de déploiement d'applications web, on peut se poser la question du meilleur service à choisir pour son application. La réponse à cette question (comme à beaucoup d'autres en informatique) va dépendre de votre cas d'utilisation et de votre contexte.

Pour néanmoins aider dans la réflexion, cette série d'article en 3 parties se propose de présenter une solution de déploiement d'application web dans le cloud: **Azure App Service**. Dans cette première partie, je vais introduire le service Azure App Service.

Cet article commencera par une présentation du service Azure App Service. Je présenterai ensuite les différents concepts pour construire notre mind-map, puis je parlerai de certaines fonctionnalités intéressantes d'Azure App Service. Pour finir, il sera question de la tarification et enfin la conclusion.

## C'est quoi Azure App Service ?

Dans le cloud (dont Azure), on distingue trois modèles de services qui sont:

- Infrastructure en tant que Service (IaaS - _Infrastructure as a Service_)
- Plateforme en tant que Service (PaaS - _Platform as a Service_)
- Logiciel en tant que Service (SaaS - _Software as a Service_)

Selon le modèle de service choisit, l'effort de gestion de nos ressources (qu'elles soient IaaS, PaaS ou SaaS) va beaucoup différer. C'est ce qui constitue la base du [modèle de partage de responsabilité](https://learn.microsoft.com/fr-fr/azure/security/fundamentals/shared-responsibility) dans le cloud.

[INSERT SHARED RESPONSIBILITY DIAGRAM]

Azure App Service appartient à la famille des Plateformes en tant que Service (PaaS). Avec ce modèle de service, le fournisseur de service cloud (CSP - _Cloud Service Provider_) a pour responsabilité de gérer, sécuriser, toute l'infrastructure sous-jacente. Le CSP met à disposition du développeur une puissance de calcul préconfiguré avec le langage de programmation cible du développeur avec la version souhaitée.

Azure App Service concrètement est une ferme d'un ou plusieurs machines virtuelles que fournit Azure et au dessus des quelles Azure nous propose un ensemble de fonctionnalités qui améliore l'expérience de développeur dans le déploiement et l'exploitation de son application web. Azure App Service permet au développeur de se concentrer sur la création de valeur et laisse à Azure le soin de la gestion, de la disponibilité et de la mise à l'échelle automatique de l’infrastructure sous-jacente.

## Quels sont les éléments constitutifs d'Azure App Service ?

Pour bien prendre en main Azure App Service, il est important de comprendre ses éléments constitutifs et comment ceux-ci s'intègrent pour former le service.

### App Service Plan

- What is app service plan?
- The structure of the shared environment
- Integration with VNET
- Available service tiers

### App Service Environment

- What is an App Service environment
- How it differs from App Service Plan

### Web App

- What is a web app in App Service context
- Deployment modes and number of instances
- Deployment slots

## Fonctionnalités

- List of available functionalities out-of-the-box
- Describe 5-6 main functionalities

## Conclusion

- Brief summary
- Introduce part 2 of the series: Deployment steps of a web on App Service

## Annexe

### Tarification

- Table with the pricing of each service tier

## Références
