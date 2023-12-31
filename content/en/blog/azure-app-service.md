+++
title = "Getting Started with Azure App Service"
date = "2023-11-23T21:26:07+01:00"
author = "Becko Junior Camara"
authorTwitter = "beckojrgong" #do not include @
cover = ""
tags = ["Azure", "App Service", "Cloud", "Web"]
keywords = ["Azure", "App Service", "Cloud", "Web"]
# description = "Introduction à Azure App Service. Présentation du service, éléments constitutifs, fonctionnalités et tarification."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
draft = false
+++

App Service is a Platform as a Service (PaaS) provided by Azure which allows developers to deploy their web applications without worrying about the underlying infrastructure. By freeing the developer from infrastructure management, the developer can focus on adding feature to her applications.

App Service deploys the underlying infrastructure in two types of environments:

- Multi tenant
- Single tenant

In a multi tenant environment, the developer's applications are deployed in two ways:

- On shared virtual machines: The computer power is shared with other Azure customers. This is the case for free and shared plans.
- On dedicated virtual machines: The computer power is delegated to the developers own applications. This is the case for basic, standard, and premium plans.

In a single tenant environment, the underlying infrastructure is deployed in a compute network managed by the developer. The single tenant environment needs a peculiar resource called App Service Environment.

With App Service, Azure deploys under the hoods a farm of servers (Windows or Linux) that host your application and a load balancer to balance traffic between your virtual machines.

On top of the compute powers, App Service provides additional features that eases streamlines the developer experience. The type of feature available depends on the type App Service plan purchased. The following features are available:

- Enable authentication on top of your application without modifying the application
- Auto Scaling: Horizontal (in the same plan) and vertical (across plans)
- CI/CD integration with popular source code management systems
- High availability with multi-zone deployment
- Global static assets deployment using CDN
- Custom domains and TLS certificates
- Deployment slots that enable multiple deployment strategies

# How does it work ?

## How do you set App Service ?

The first step in working with Azure App Service is to select your plan among the following:

- Free
- Shared (supported for Windows only)
- Basic
- Standard
- PremiumV2
- PremiumV3
- Isolated
- IsolatedV2

As you probably have guessed, App Service plans are billed differently. There's a difference of cost between the two available operating systems. Each plan provides multiple compute sizes to choose from. When multiple workers are created in a plan, an instance of our applications are deployed by default on all the workes. Please refer to the [App Service pricing](https://azure.microsoft.com/en-us/pricing/details/app-service/windows/) for more information on compute sizes available and pricing. Please note that listed prices are per individual workers deployed.

Use the following CLI command to create a new App Service plan

`az appservice plan create --resource-group <resource-group-name> --name <app-service-plan-name> --sku <plan-sku> --number-of-workers <number-of-instances> --is-linux|--hyper-v # to select Linux or Windows`

## How do you deploy a web app ?

Once the App Service plan is deployed and ready, it's time to bring up the application.

Deploying our application is a two-step process:

- Create the web app resource
- Deploy your application from source

Azure App Service supports the following languages and runtimes:

- Node.js
- Python
- Golang
- .NET Core
- ASP.NET
- Linux Container (Custom containers are supported)
- Windows Containers

You can create a web app by executing the following Azure CLI command:

`az webapp create --name <web-app-name> --plan <plan-to-use> --resource-group <resource-group-name> --runtime <type-of-runtime> --https-only`

The command above will create a stub web app waiting for the content of your application.

Your web app probably needs some configuration variables to work properly. Azure App Service provides App Settings for the matter. You leverage App Settings to provide configuration values to your web app. Values set through App Settings are passed to the running web app as environment values.
Note that App Settings values are not encrypted by default. You'll want to use Azure Key Vault to store secrets like a database password.

The following command set App Settings for a web app

`az webapp config appsettings set -g <resource-group-name> -n <app-name> --settings <setting-key>=<setting-value> @<path-to-settings-file>.json`

Set your application settings as required by your app and start app deployment.

### Deployment methods

## How do you integrate App Service with other Azure Service ?

# How do you operate it ?

- Monitoring tools integration
- Alerting

# When should you use App Service ?
