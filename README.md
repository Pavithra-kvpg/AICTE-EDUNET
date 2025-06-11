# 🚀 Deploying a Dockerized Application to Azure Container Instance

This guide walks you through the process of deploying your containerized application to Microsoft Azure using Azure Container Registry (ACR) and Azure Container Instance (ACI).

**🛠️ Prerequisites**
Ensure you have the following installed on your local machine:

**Docker:** To build and manage your application container. Install Docker

**Azure CLI:** For managing Azure resources from the terminal. Install Azure CLI

**Azure Subscription:** Required to create resources on Azure.


## 📦 Step 1: Prepare Code and Dockerfile
Create your application code.

Add a Dockerfile in the root directory with the necessary instructions to build your container.

Example Dockerfile:

Use a base image

FROM python:3.9

**#Set working directory**

WORKDIR /app


**#Copy application code**

COPY . .

**#Install dependencies**

RUN pip install -r requirements.txt

**#Expose application port**

EXPOSE 80

**#Run the application**
CMD ["python", "app.py"]


## ☁️ Step 2: Set Up Azure Resources

**1.Create a Resource Group**

In Azure Portal or using CLI:

az group create --name <ResourceGroupName> --location <Region>

**2.Create Azure Container Registry (ACR)**

az acr create --resource-group <ResourceGroupName> --name <AzureContainerRegistryName> --sku Basic

## 💻 Step 3: Terminal Commands

**1. Login to Azure**

az login

Authenticate via browser and sign into your Azure account.

**2. Login to Azure Container Registry**

az acr login --name <AzureContainerRegistryName>

This authenticates your local Docker CLI to push to ACR.

**3. Build Docker Image**

docker build -t <ImageName> .

Build your Docker image locally.

**4. List Docker Images**

docker images

Verify that the image was built successfully.

**5. Tag Docker Image for ACR**

docker tag <ImageName> <AzureContainerRegistryName>.azurecr.io/<ImageName>:v1

Tag the image for upload to your Azure Container Registry.

**6. Verify Tagged Image**

docker images

Ensure your image is properly tagged for ACR.

**7. Push Docker Image to ACR**

docker push <AzureContainerRegistryName>.azurecr.io/<ImageName>:v1

Push the image to Azure Container Registry.

**8. Create Azure Container Instance**

az container create \

  --resource-group <ResourceGroupName> \
  
  --name <ContainerName> \
  
  --image <AzureContainerRegistryName>.azurecr.io/<ImageName>:v1 \
  
  --dns-name-label <DnsNameLabel> \
  
  --ports 80
  
Deploy your containerized app to Azure.

**9. Check Deployment Status**

az container show \

  --resource-group <ResourceGroupName> \
  
  --name <ContainerName> \
  
  --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
  
  --out table
  
Get the Fully Qualified Domain Name (FQDN) and check if the container is successfully deployed.



