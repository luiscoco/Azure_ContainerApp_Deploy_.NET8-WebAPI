# How to deploy .NET8 WebAPI to Azure Container App

## 1. Get started with Azure Container App

We log in to Azure Portal and we create a new resource (a new **Container App**)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/70a88181-20c1-4031-85a8-48fa81e5459c)

We input the basic data 

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/2f36450a-a70b-45eb-9445-ba2e014ba262)

We also create a new **Container Apps Environment**

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/65036eb4-0aed-4c64-8f7f-4a245246c559)

We input the data required for the new **Container Apps Environment**



## 2. Deploy .NET8 WebAPI to Azure Container App (with Azure Portal)

## 3. Deploy .NET8 WebAPI to Azure Container App (with Azure CLI commands)

deploying a .NET Web API to Azure Container Apps using Azure CLI involves several steps. Below is a guide to help you through the process, assuming you have already containerized your application and pushed the image to a container registry (like Azure Container Registry).

Prerequisites
Azure CLI: Ensure you have the Azure CLI installed. If not, you can download it from here.
Azure Subscription: Make sure you are logged into your Azure account and have set the appropriate subscription.
bash
Copy code
az login
az account set --subscription "<your-subscription-name-or-id>"
Steps to Deploy
1. Create a Resource Group (if not already existing)
bash
Copy code
az group create --name myResourceGroup --location eastus
Replace myResourceGroup and eastus with your preferred resource group name and Azure region.

2. Create or Use an Existing Azure Container Registry (ACR)
If you haven't already created an Azure Container Registry:

bash
Copy code
az acr create --resource-group myResourceGroup --name myRegistry --sku Basic
Replace myRegistry with your desired registry name.

3. Log in to ACR
bash
Copy code
az acr login --name myRegistry
4. Push Your Docker Image to ACR
Assuming you have already tagged your image:

bash
Copy code
docker push myRegistry.azurecr.io/mywebapi:v1
5. Create a Container App Environment
bash
Copy code
az containerapp env create --name myEnvironment --resource-group myResourceGroup --location eastus
Replace myEnvironment with your desired environment name.

6. Deploy the Container App
bash
Copy code
az containerapp create --name mywebapi --resource-group myResourceGroup --environment myEnvironment --image myRegistry.azurecr.io/mywebapi:v1 --target-port 80 --ingress external --query configuration.ingress.fqdn
Replace mywebapi with your app name.
--target-port 80 assumes your app is listening on port 80. Adjust if needed.
--ingress external makes the app publicly accessible.
This command will output the FQDN (Fully Qualified Domain Name) of your app, which you can use to access it.

7. Update the App (if needed)
If you need to update the app later with a new image version:

bash
Copy code
az containerapp update --name mywebapi --resource-group myResourceGroup --image myRegistry.azurecr.io/mywebapi:v2
Replace v2 with your new image tag.

Additional Notes
Configuration: You might need to set environment variables or other configurations depending on your application's needs.
Diagnostics and Monitoring: Consider configuring Azure Monitor and Application Insights for diagnostics and monitoring.
CI/CD: For a more automated approach, integrate these steps into a CI/CD pipeline.
This guide provides a basic outline for deploying a .NET 8 Web API to Azure Container Apps using Azure CLI. Depending on your application's complexity and requirements, you may need to adjust or add more steps.

