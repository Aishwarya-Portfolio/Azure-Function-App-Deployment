# Azure-Function-App-Deployment
A project on Azure DevOps CI-CD to deploy a website on App service and Azure Function App.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/91592578/233173520-e026036a-eb19-4332-ae0e-daea5837f6ab.png">

### About Project:
* We have the Space game website where users are playing this game and competing.
* We need to add a leaderboard where users can see each others score and rank.
* We should not expose the database to the public, only few data should be reflected on the leaderboard.
* The website has been deployed to the Azure App service using Azure DevOps build and release.
* Azure function is used to exposes the leaderboard data via a simple HTTP GET request. I also updated the web site to use that endpoint when requesting the leaderboard.

#### Azure Function App:
Azure Functions is serverless computing technology offered by Azure. It provides an easy way for developers to build straightforward functions that exist in a stateless, serverless environment. Functions can be triggered using a variety of methods, such as HTTP requests, changes to data in storage, receipt of a message from a queue, and more. 

#### Sample Source code used - [Space game App](https://github.com/Aish-Tinkerbell/mslearn-tailspin-spacegame-web-azure-functions) 

### Steps:
1. Add a user to ensure Azure DevOps can connect to your Azure subscription.
2. Set up an Azure DevOps project.
3. Make sure your project is set up locally so that you can push changes to the pipeline.
4. Create the Azure App Service and Azure Functions app using the Azure CLI in Azure Cloud Shell.
5. Create pipeline variables that define the names of your App Service and Azure Functions instance.
6. Create a service connection that enables Azure Pipelines to securely access your Azure subscription.
7. Review the Build stage.
8. Add a task to deploy your function app.
9. Add a task configure the published App Service to use the published function.
10. Save the pipeline to trigger a CI/CD workflow.
