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

#### Sample Source code used - [Space game App](https://github.com/Aish-Tinkerbell/mslearn-tailspin-spacegame-web-azure-functions) 

### Create the Azure App Service and Azure Functions app using the Azure CLI in Azure Cloud Shell
1. Open cloud shell in Azure Portal:
 <img width="200" alt="image" src="https://user-images.githubusercontent.com/91592578/233174812-8f54823f-4e24-42da-8018-483baa8f2874.png">
 
2. From Cloud Shell, run the following **az account list-locations** command to list the regions that are available from your Azure subscription:
> az account list-locations \
  --query "[].{Name: name, DisplayName: displayName}" \
  --output table

3. From the **Name** column in the output, choose a region that's close to you. I have used **eastus**:                                                               
> az configure --defaults location=<REGION>

4. From Cloud Shell, generate a random number, which we will use to create globally unique names for certain services in the next step.
> resourceSuffix=$RANDOM
  
5. Create three globally unique names for your App Service, Azure Function, and storage accounts. These commands use double quotes, which instruct Bash to resolve the variables using inline syntax:
> webName="tailspin-space-game-web-${resourceSuffix}"
> leaderboardName="tailspin-space-game-leaderboard-${resourceSuffix}"
> storageName="tailspinspacegame${resourceSuffix}"
  
6. Create two more Bash variables to store the names of your resource group and service plan:
  > rgName='tailspin-space-game-rg'
  > planName='tailspin-space-game-asp'
  
7. Create Resource group in Azure-
> az group create --name $rgName
  
8. Create an App Service plan using the name defined in the previous task:
  > az appservice plan create \
  --name $planName \
  --resource-group $rgName \
  --sku B1 \
  --is-linux
 
9. Create the App Service instance:
  > az webapp create \
  --name $webName \
  --resource-group $rgName \
  --plan $planName \
  --runtime "DOTNETCORE|6.0"
  
10. Azure Functions requires a storage account for deployment. So will create a storage account:
  > az storage account create \
  --name $storageName \
  --resource-group $rgName \
  --sku Standard_LRS

11. Create the Azure Functions app instance. Replace the <region> with your preferred region:
  > az functionapp create \
  --name $leaderboardName \
  --resource-group $rgName \
  --storage-account $storageName \
  --functions-version 4 \
  --consumption-plan-location <region>

12. List the host name and state of the App Service instance:
  > az webapp list \
  --resource-group $rgName \
  --query "[].{hostName: defaultHostName, state: state}" \
  --output table

13. List the host name and state of the Azure Functions instance:
  > az functionapp list \
  --resource-group $rgName \
  --query "[].{hostName: defaultHostName, state: state}" \
  --output table

14. Open a browser and enter a host name to verify that it's running. The default home page should appear.

### Create pipeline variables in Azure Pipelines
1. Navigate to your Azure DevOps Project.
2. Open Library and add a new variable group - 'Release'.
  <img width="200" alt="image" src="https://user-images.githubusercontent.com/91592578/233179408-7504ff13-a912-46b2-b582-f8ff40913845.png">
3. Add below 3 variables, the values should be taken from the resources we created in Azure-
  <img width="700" alt="image" src="https://user-images.githubusercontent.com/91592578/233179616-34ead2b8-8426-4f34-918f-13d068fd120b.png">

### Create a service connection
1. In the New service connection pane, select Service principal (automatic).
2. And enter the following settings:
  <img width="900" alt="image" src="https://user-images.githubusercontent.com/91592578/233181347-1ed29bad-1567-4c9f-a81e-aa29a6380834.png">
3. Ensure that Grant access permission to all pipelines is selected. And Save.
