# How to deploy .NET8 WebAPI to Azure Container App

See these youtube videos for more detailed and advance info about Azure Container App: 

https://www.youtube.com/watch?v=jfYJEcDOOkI&list=PLBmBUIbhAfd-eLB-XGxxhC68MNVl3I-Gi

## 1. Get started with Azure Container App (deploy a sample Hello app)

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

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/4fdfc1c7-addb-4379-a47f-312e0120d310)

![image](https://github.com/luiscoco/Azure_ContainerApps_Deploy_.NET_8_Web_API/assets/32194879/7f0822df-a6e5-4f45-a54f-92cd90d4647c)

## 2. Deploy .NET8 WebAPI to Azure Container App (with Azure Portal)

### 2.1. Create the Azure Container Registry ACR and grant administator permission

We first build the Docker image

```
docker build -t mywebapi .
```

We **create** the **Azure Container Registry ACR** in Azure Portal

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/e6ac208f-49a6-4ed1-9fd4-d3cfeb1fe2d6)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/a4ddf5ec-df5b-4f0c-a215-6b2c8220c9a3)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/6babbb74-ee85-4f43-b122-af555c10410f)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/ebe7ebb6-a6a3-4eb0-9130-ad91513850df)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/4b0204c1-3dde-4fe6-95b8-fc379b314649)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/4ef47517-131d-4b98-af5d-26f9ac029352)

We can see in the list the new ACR

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/352c21a4-afb7-443e-81ef-41bca6988952)

We enable the Admin User in the Azure ACR, and we copy the username and password for docker login

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/b78e7fd7-1908-4dfd-9374-20d1e85dc691)

We rename the Docker image including the **Azure ACR name**

```
docker tag mywebapi myfirstregistry1974.azurecr.io/mywebapi:v1
```

We **log in** to **Azure CLI**

```
az login
```

We **log in** to ACR with **Docker**

```
docker login myfirstregistry1974.azurecr.io --username myfirstregistry1974 --password XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

We **push** the Docker image to Azure ACR

```
docker push myfirstregistry1974.azurecr.io/mywebapi:v1
```

### 2.2. Create Azure Container App (with Azure Portal)

We log in to Azure portal and we create a new resource (**new Container App**)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/0378be9f-5c5b-4bcb-8856-f61c7c1df217)

We input the **basic data**

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/386a2a79-3be8-40d1-804d-df798816a313)

We select the **Region**

We create the **Container Apps Environment**

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/c426bce9-ac68-47da-a673-8ce2fa81ae7e)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/058abeef-f99a-434e-80e4-42dd60bffe20)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/78ab166a-dd59-4139-90bb-a4764bb05a24)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/5c025a56-37fa-4ca9-a97c-6e3eb6646f99)

We select the container, in this case we load the WebAPI container from Azure ACR

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/7509caa8-5a45-42f3-9539-52333e82e708)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/dfd72b9e-721a-4e8f-b8b8-c3d987656e28)

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/d9a75682-3670-46a1-ab2c-c5e0e747b745)

We can see the new resource in the list

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/6b484dba-45a9-463b-903d-7be3e67e731a)

We can get the access endpoint: 

https://myfirstcontainerapp.whitedesert-93889813.westeurope.azurecontainerapps.io

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/a9ae1006-96f1-42a3-b763-6ac90cb8cd72)

We verify the application endpoint: https://myfirstcontainerapp.whitedesert-93889813.westeurope.azurecontainerapps.io/weatherforecast

![image](https://github.com/luiscoco/Azure_ContainerApp_Deploy_.NET8-WebAPI/assets/32194879/cb05bf9b-e2a8-4fb1-b202-e047d0580b59)

## 3. Deploy .NET8 WebAPI to Azure Container App (with Azure CLI commands)

Deploying a .NET Web API to **Azure Container App** using **Azure CLI** involves several steps

Below is a guide to help you through the process, assuming you have already containerized your application and pushed the image to a container registry (like **Azure Container Registry**)

**Azure CLI**: Ensure you have the Azure CLI installed. If not, you can download it from here.

**Azure Subscription**: Make sure you are logged into your Azure account and have set the appropriate subscription.

```
az login
az account set --subscription "<your-subscription-name-or-id>"
```

**Steps to Deploy**

### 3.1. Create a Resource Group (if not already existing)

```
az group create --name myResourceGroup --location eastus
```

Replace myResourceGroup and eastus with your preferred resource group name and Azure region

### 3.2. Create or Use an Existing Azure Container Registry (ACR)

If you haven't already created an Azure Container Registry:

```
az acr create --resource-group myResourceGroup --name myRegistry --sku Basic
```

Replace myRegistry with your desired registry name

We select the **Azure Subscription**

```
az account set --subscription XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
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

### 3.3. Log in to ACR

```
az acr login --name myRegistry
```

### 3.4. Push Your Docker Image to ACR

We **log in** to ACR with **Docker**

```
docker login myfirstregistry1974.azurecr.io --username myfirstregistry1974 --password XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

We **push** the Docker image to Azure ACR

```
docker push myfirstregistry1974.azurecr.io/mywebapi:v1
```

Assuming you have already tagged your image:

```
docker push myRegistry.azurecr.io/mywebapi:v1
```

### 3.5. Create a Container App Environment

```
az containerapp env create --name myEnvironment --resource-group myResourceGroup --location eastus
```

Replace myEnvironment with your desired environment name.

### 3.6. Deploy the Container App

```
az containerapp create ^
  --name mywebapi ^
  --resource-group myResourceGroup ^
  --environment myEnvironment ^
  --image myfirstregistry1974.azurecr.io/mywebapi:v1 ^
  --target-port 8080 ^
  --ingress external ^
  --query configuration.ingress.fqdn
```

Replace mywebapi with your app name.

**--target-port 8080** assumes your app is listening on port 8080

**--ingress external** makes the app publicly accessible.

This command will output the FQDN (Fully Qualified Domain Name) of your app, which you can use to access it

### 3.7. Update the App (if needed)

If you need to update the app later with a new image version:

```
az containerapp update ^
  --name mywebapi ^
  --resource-group myResourceGroup ^
  --image myfirstregistry1974.azurecr.io/mywebapi:v2
```

Replace v2 with your new image tag.

**Additional Notes**

**Configuration**: You might need to set environment variables or other configurations depending on your application's needs

**Diagnostics and Monitoring**: Consider configuring Azure Monitor and Application Insights for diagnostics and monitoring

CI/CD: For a more automated approach, integrate these steps into a CI/CD pipeline

This guide provides a basic outline for deploying a .NET 8 Web API to Azure Container Apps using Azure CLI

Depending on your application's complexity and requirements, you may need to adjust or add more steps

