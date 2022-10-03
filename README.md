# Purview Custom Push Model Solution

## Description
   #### Change streams allow applications to access real-time data changes by watching collections, databases, or deployments for changes. Starting in MongoDB 6.0, change streams also support change notifications for DDL events. This utility shows all the steps to build a Custom Push Solution to listen to the Change Events in MongoDB to capture the DDL's (Create/Drop) of collections for a database being watched in Real Time using the Azure app service and then push those changes to Microsoft Azure Purview App using Purview API's.
    
## Prerequisites
  #### 1: MongoDB Atlas 6.0 cluster.
  #### 2: Azure subscription.
  #### 3: Visual Studio app installed, Click [Download](https://code.visualstudio.com/download) to get the app.
 
# Setup

## 1: Configure MongoDB Atlas Environment

  - Log-on to Atlas account. [Login](https://account.mongodb.com/account/login)

  - Create a M10 Cluster in a cloud provider region of your choice with default settings. Make sure to choose Mongodb version 6.0. Capturing Change events on create and update of collection is introduced as part of 6.0 version
  - In the Security tab:
    - Select Network Access and add a new IP Whitelist for your laptop's current IP address.
    - Select Database Access, choose to add a new user, e.g. main_user,give a password of your choice. For Database User Privileges, from the built-in role dropdown choose "Read and write to any database" (make a note of the password you specify).
    - Click on Browse Collections-> Create Database -> Give a database name and a collection name of your choice and click Create - make a note of this MongoDB URL address to be used in a later step
  - In the Atlas console, for the database cluster you deployed, click the Connect button, select Connect Your Application, and for the latest Node.js version copy the Connection String Only - make a note of this MongoDB URL address to be used in a later step

## 2: Purview Setup

  #### A: Create a purview account in azure. 

   - Reference article for the details on creating a purview account - [Create an Azure Purview account in the Azure portal](https://docs.microsoft.com/en-us/azure/purview/create-catalog-portal)

  #### B: Create a Purview collection in Azure Portal

   - You can follow the steps provided in Microsoft Link - [Create Purview Collection](https://docs.microsoft.com/en-us/azure/purview/quickstart-create-collection)
  
  #### C: Create a service principal which will be used to access the API.

   - For a REST API client to access the catalog, the client must have a service principal (application).
   Reference article for the details on creating the service principal - [Create a Service Principal](
https://docs.microsoft.com/en-us/azure/purview/tutorial-using-rest-apis#create-a-service-principal-application)

  #### D: Set up authentication using service principal

   - Assign the correct role to establish trust between the service principal and the Purview account - make a note of client_id,client secret to be used in a later step.
   You can follow the steps under the section "Set up authentication using service principal" provided in the reference article - [Set up authentication](https://docs.microsoft.com/en-us/azure/purview/tutorial-using-rest-apis#set-up-authentication-using-service-principal)

  #### E: Connect to and manage MongoDB in Microsoft Purview.

   - Reference article outlines how to [register](https://learn.microsoft.com/en-us/azure/purview/register-scan-mongodb#register) MongoDB,authenticate and perform a [initial scan](https://learn.microsoft.com/en-us/azure/purview/register-scan-mongodb#scan) with MongoDB in Microsoft Purview.

## 3: Create an app service in Azure

  - Create a app service in Azure to deploy the Node.js code.
  You can follow the steps under the section "Create Azure resources" provided in Microsoft Link - [Create App Service](https://docs.microsoft.com/en-us/azure/app-service/quickstart-nodejs?tabs=linux&pivots=development-environment-azure-portal#create-azure-resources)

## 4: Connect your local app to Azure

  - Download the code and open the code in Visual Studio.
  - Open the test.env file and add the following details:
  
    - CONNECTION_URI : add the connection string of the your Atlas cluster 6.0+.
    - DATABASE_TO_WATCH : Name of the MongoDB Database you created in Step 1.
    - CLIENT_ID : Client_ID which you got from Step 2D
    - CLIENT_SECRET : Client_secret which you got from Step 2D
    - TENANT_ID : Go to Azure portal -> Microsoft Purview account -> Properties -> Copy the "Managed identity tenant ID"
    - GUID : Go to Azure portal -> Microsoft Purview account. Click on 'Open Microsoft Purview governance portal'. Click on Browse assets -> By Source Type-> Click on the name -> You should see your GUID in the page url.
    - FULLY_QUALIFIED_NAME : Go to Azure portal -> Microsoft Purview account. Click on 'Open Microsoft Purview governance portal'. Click on Browse assets -> By Source Type and copy the "fully_qualified name".
    - ENDPOINT : Go to Azure portal -> Microsoft Purview account -> Properties -> Copy the "Atlas endpoint"
    
  - Go to Extensions on the left menu and type "Azure"
    - Install these extensions for 
      - Azure Account
      - Azure Tools
      - Azure App Service
      
  - Once the extensions are installed you should see a Azure icon on the left menu.Click on that and sign in to Azure account. After Signing in, it should look like this.
 
   <img width="373" alt="Screenshot 2022-09-12 at 3 04 15 PM" src="https://user-images.githubusercontent.com/101181433/189621346-c3d9fef8-7fb4-4235-a5da-6e39bf2624a6.png">

## 5: Deploy your app to Azure app service

  - In Visual Studio under Azure subscription-> app services, you should see your new app service that you created in the Azure portal.
  - Right click on the app service, Click "Deploy to Web App".
    
# Execution

 - Go to Azure portal, Click on the app service created. Start your app service.
 - Click on browse to see if the app service is running. You should see the message 'MongoDB ChangeStream for Purview' with current timestamp.
 
### Create a MongoDB collection:

  - In the Atlas portal, Create a collection under the database you are watching. [Portal](https://account.mongodb.com/account/login)
  - Go to Azure portal -> Microsoft Purview account. Click on 'Open Microsoft Purview governance portal'. Click on Browse assets and click on the root collection. The MongoDB collection that you created in the MongoDB Atlas should be listed here in Purview.

### Drop a MongoDB collection:

  -  In the Atlas portal, Drop a collection under the database you are watching.
  - Go to Azure portal -> Microsoft Purview account. Click on 'Open Microsoft Purview governance portal'. Click on Browse assets and click on the root collection. The MongoDB collection that you dropped from the MongoDB Atlas should be removed from Purview.
 
    
    
    
  


  

  
