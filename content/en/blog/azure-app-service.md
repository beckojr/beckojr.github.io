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

App Service is a Platform as a Service (PaaS) provided by Azure that allows developers to deploy their web applications without worrying about the underlying infrastructure. By freeing the developer from infrastructure management, the developer can focus on adding features to their applications.

App Service deploys the underlying infrastructure in two types of environments: multi tenant and single tenant.

In a multi tenant environment, applications are deployed in an Azure-managed network environment. There are two types of computes in a multi-tenant environment:

- Shared virtual machines, where the compute power is shared with other Azure customers. Shared virtual machines are available for free and shared plans.
- Dedicated virtual machines, where the compute power is dedicated to the developers' applications. Dedicated virtual machines are available for basic, standard, and premium plans.

In a single tenant environment, the underlying infrastructure is deployed in a network that exists in the developer's Azure account. Single tenant environment is available for isolated App Service plans.

With App Service, Azure deploys under the hoods a farm of servers (Windows or Linux) that host your applications and a load balancer to balance traffic between your virtual machines.

On top of the compute powers, App Service provides additional features that eases streamlines the developer experience. The type of feature available depends on the type App Service plan purchased. The following features are available:

- Enable authentication on top of your application without modifying the application
- Horizontal scaling by changing the VM size (up or down) within the same plan
- Vertical scaling by selecting another plan
- CI/CD integration with popular source code management systems
- High availability with multi-zone deployment
- Global static assets deployment using CDN
- Custom domains and TLS certificates
- Deployment slots that enable multiple deployment strategies

# How does it work ?

## How do you set App Service ?

The first step in working with Azure App Service is to select your plan. The following plans are available:

- Free
- Shared (supported for Windows only)
- Basic
- Standard
- PremiumV2
- PremiumV3
- Isolated
- IsolatedV2

App Service plans are price depends the selected operating system and virtual machine sizes. Each plan provides multiple compute sizes to choose from. Please refer to the [App Service pricing](https://azure.microsoft.com/en-us/pricing/details/app-service/windows/) for more information on compute sizes available and pricing. Please note that listed prices are per individual workers deployed.

The following command creates a free App Service plan using with the Linux Operating system

`az appservice plan create --resource-group rg-blog-playground --name as-getting-started-free --sku Free --is-linux`

{{< figure src="/images/app-service-plan-free.png" alt="App Service plan in Azure Portal" position="center" caption="Figure 1: App Service plan in the Azure Portal" >}}

## How do you deploy a web app ?

Once the underlying compute power is available, it's time to bring up the application.

Deploying our application is a two-step process:

- Create the web app resource and its configuration
- Deploy your application code

Azure App Service supports the following languages and runtimes:

- Node.js
- Python
- Golang
- .NET Core
- ASP.NET
- Java
- Linux Container (Custom containers are supported)
- Windows Containers

### How do you create and configure a web app ?

The following command creates a web app stub using the Azure CLI:

`az webapp create --name gs-node-app --plan as-getting-started-free --resource-group rg-blog-playground --runtime NODE:20-lts`

{{< figure src="/images/web-app-node.png" alt="Web app in the Azure Portal" position="center" caption="Figure 2: Web app in the Azure Portal" >}}

{{< figure src="/images/app-stub.png" alt="App Service plan in Azure Portal" position="center" caption="Figure 3: Application stub created by Azure" >}}

As your application is deployed, you may need to changes the version of the runtime (e.g. from Node.js 20 to version 18), set some environment variables required for your web app to work.

Azure App provides two constructs that enable web app configuration:

- General settings, used to set stack settings (runtime, major and minor versions,...), platform settings (HTTP version, HTTPS redirect, ...), and debugging settings.
- Application settings, used to set values passed as environment variables to the web at startup.
- Connection strings, used to set database connection strings.

**Notes**:

1. Application settings and connection string values override values passed via a configuration file
2. For runtimes other than ASP.NET and .NET Core, it is preferable to use Application settings to set connection strings

App Service is natively integrated with Azure Key Vault and App Configuration. Azure Key Vault is preferable for secret values storage(database password, and connection strings). Azure App Configuration is a service that manages app configuration. It provides a single management pane for application configurations. For you web app to interact with Azure Key Vault and App Configuration, it must have a managed service identity with the required permissions to access the key vault or the App Configuration store.

The following command sets `ENV` and `REGION` environment variables

`az webapp config appsettings set -g rg-blog-playground --name gs-node-app --settings ENV=DEV REGION=FRC`
{{< figure src="/images/app-settings.png" alt="App Service plan in Azure Portal" position="center" caption="Figure 4: App Service plan in the Azure Portal" >}}

### How do you make your application's source code or binary available in your web app ?

Under the hoods, App Service uses a content share (`/home/site/wwwroot` for Linux or `D:\home\site\wwwroot` for Windows) to make your application content on the virtual machine or the web app container. The application is restarted whenever a file in the content share is updated.

App Service provides the following methods to update the content share:

- GitHub, BitBucket, and Azure DevOps continuous deployment integration
- Local git, App Service creates a remote git server that triggers a new deploy on a push on the default branch
- ZIP deployment, an archive made of your application's content
- WAR deployment, a WAR file of a Java application

SCM (GitHub, BitBucket, Azure DevOps) integration requires to setup a service principal ou a deployment profile for your CI/CD platform to have the required permissions to deploy your web app.

Under the hoods, the deployment methods use one of the following mechanisms:

- Kudu, a developer productivity tool that enables continuous deployment. It runs as a separate process on Windows and another container on Linux.
- FTP(S), each web app has two FTP endpoints (read-only, read-write). Uploads to the `read-write` endpoint is a way to deploy your source code.

A new deployment triggers an app restart to pickup the latest changes. In a production environment, this means a downtime for users. To avoid this downtime, App Service has `deployment slots` to the rescue.

A deployment slot is a staging environment with its own configuration and domain name. A deployment slot can be used to validate changes prior to going live in production. Deployment slot swapping enables a seamless blue-green deployment. App Service allows traffic splitting between two slots. This help validate changes on the staging slot before swapping it with the production slot.

## How does App Service integrates with other Azure services ?

Your application may need to access resources available on another Azure services. The way App Service interacts with other Azure services depends on the kind of authentication mechanism the other Azure service supports.

For an application that needs to access a PostgreSQL database deployed on an Azure PotgreSQL instance, the application needs a user/password credential to connect to the target database. In a case like this, you must set your credentials either as a database connection string or an [App Setting](#how-do-you-create-and-configure-a-web-app-). It is a best practice to store your database secrets (and any other secret e.g. certificates, api keys, ...) in Azure Key Vault.

Azure Key Vault is the kind of Azure Service that doesn't support user/password as an authentication mechanism. For your application to access secrets stored in Azure Key Vault, they must have the required access permissions. To set the required permission to your web app, you need to create a `Managed Service Identity (MSI)`. You assign a role with the required access to the managed identity attached to your web app.

# When should you use App Service ?
