# How to deploy .NET8 WebAPI to Azure Container App

See these youtube videos for more detailed info: https://www.youtube.com/watch?v=jfYJEcDOOkI&list=PLBmBUIbhAfd-eLB-XGxxhC68MNVl3I-Gi

## 1. Get started with Azure Container App

We log in to Azure Portal and we create a new resource (a new **Container App**)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/70a88181-20c1-4031-85a8-48fa81e5459c)

We input the basic data 

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/2f36450a-a70b-45eb-9445-ba2e014ba262)

We also create a new **Container Apps Environment**

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/65036eb4-0aed-4c64-8f7f-4a245246c559)

We input the data required for the new **Container Apps Environment**

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/d85416f2-46c9-443f-b6a1-bdeb6c09bd68)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/f699a280-4a16-4709-b10e-84999f05efbc)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/f2b5e4d7-ccde-49d7-b6fe-ea4c81196b10)

Let's review the final **basic data**

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/657663eb-d532-4c36-8049-33c60a5fbf63)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/b9e345dd-d916-4960-bfbf-6efe311810c7)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/398a2b25-68a6-4ea7-bfe5-bde76bf29311)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/173edfdd-ec43-4d49-86b8-c7bd6781ba20)

We have create these services: **Container Apps Environment**, **Container App**, **Log Analytics workspace**

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/e66f4fb0-d084-41e4-bd48-76b99a7e0de2)

We verify the **Azure Container App** endpoint

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/25fd2e07-0025-4e0b-ac22-6f5dbc885ebb)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/7f0822df-a6e5-4f45-a54f-92cd90d4647c)

## 2. Deploy .NET8 WebAPI to Azure Container App (with Azure Portal)

### 2.1. Create the Azure Container Registry ACR and grant administator permission

We first build the Docker image

```
docker build -t mywebapi .
```

We **create** the **Azure Container Registry ACR** in Azure Portal

We select the **Azure Subscription**

```
az account set --subscription 99888cc6-c635-4ebd-b0ac-1be1dace0089
```

We **enable the Admin user**

```
az acr update --name myfirstregistry1974.azurecr.io --admin-enabled true --resource-group myRG
```

We **show** the admin user **credentials**

```
az acr credential show --name myfirstregistry1974 --resource-group myRG
```

For example these can be the **ACR admin user credentials**

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    },
    {
      "name": "password2",
      "value": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }
  ],
  "username": "myfirstregistry1974"
}
```

We rename the Docker image including the **Azure ACR name**

```
docker tag mywebapi myfirstregistry1974.azurecr.io/mywebapi:v1
```

We **log in** to **Azure CLI**

```
az login
```

We **log in** to ACR with **Docker**

docker login myfirstregistry1974.azurecr.io --username myfirstregistry1974 --password ++NH/WydR+t3kfMkNBBlyxe9/xdKm6prOh5SL4cFvT+ACRBPgu3f

We **push** the Docker image to Azure ACR

```
docker push myfirstregistry1974.azurecr.io/mywebapi:v1
```

### 2.2. 

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

