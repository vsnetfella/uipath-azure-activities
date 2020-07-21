# UiPath Azure Activities
This repository covers the PoCs for UiPath Azure Activities like working with Azure Key Vaults, setting up Azure Storage Accounts,
Azure User Group Authentication etc.,

# Working with UiPath Azure Activities

# Azure Scope

All Azure activity should be wrapped up inside the Azure Scope. You can find the Azure Scope under Cloud Activities  Azure  Azure Scope

![](screenshot\RackMultipart20200721-4-1ga4vcl_html_3b192161e8b2f8b6.png)

Below are the required parameters for defining the Azure Scope.

![](RackMultipart20200721-4-1ga4vcl_html_947ec6e4d69b3698.png)

## Step 1: Getting Subscription Id

1. Login to the [https://portal.azure.com](https://portal.azure.com/)
2. In the Home page you can find the Subscriptions item ![](RackMultipart20200721-4-1ga4vcl_html_6a60606bfba89ce6.png)
3. Click on Subscription
4. You can find the subscription id here ![](RackMultipart20200721-4-1ga4vcl_html_a59ecf79e4411492.png)
5. Update this information in the **SubscriptionId** Field

## Step 2: Getting the Tenant ID and Client ID

1. Go to App Registration in Azure Portal
2. Click on &quot;+ New Registration&quot; ![](RackMultipart20200721-4-1ga4vcl_html_9092d51e25112f03.png)
3. In the new registration page,
  1. Give a name of your choice. Let us give, &quot;my-azure-apps&quot;
  2. In Supported Types, Select &quot;Default Directory Only&quot; as of now.
  3. In Redirect URL, Select Web [http://localhost](http://localhost/)
  4. Click Register

![](RackMultipart20200721-4-1ga4vcl_html_b14f1d81619830f5.png)

1. Open the resource and you can find the below information ![](RackMultipart20200721-4-1ga4vcl_html_1b3e93e52cdccdea.png)
2. Provide the &quot;Directory (tenant) ID&quot; information in the TenantID Field in the Azure Scope
3. Provide the &quot;Application (client) ID&quot; information in the Client ID Filed in the Azure Scope

## Step 3: Creating Client Secret

1. In the app registration page, go to &quot;Certificates and Secrets&quot; (numbered 1) menu on your left

![](RackMultipart20200721-4-1ga4vcl_html_b5b99a808d88bccd.png)

1. Create a new client secret by clicking the button numbered 2 in the above image.
2. Give a meaningful name and Click Add

![](RackMultipart20200721-4-1ga4vcl_html_fe76d1fe2099c802.png)

1. Store the client secret generated safely somewhere
2. Provide that information in the Client Secret Field

**Note:**

**The Client Secret is accepted only as a Secure String. If you are using an orchestrator Create an Asset of Credential Type and save both Client ID and Secret. You can get it using a Get Credential Activity in UiPath Studio. Or you can use Windows Credential Manager if not using Orchestrator.**

With this the Azure Scope Setup is complete.

# Working with Key Vault

Official Documentation : [https://docs.microsoft.com/en-us/azure/key-vault/](https://docs.microsoft.com/en-us/azure/key-vault/)

## Step 1: Creating a Key Vault Resource

1. Go to Home page in Azure Portal
2. Create a New Resource for Azure Key Vault ![](RackMultipart20200721-4-1ga4vcl_html_f268a5342d2de883.png)
3. Give a meaningful name and click Create ![](RackMultipart20200721-4-1ga4vcl_html_9f67956c43b61126.png)
4. Once the resource is created you will get these information in the Overview page ![](RackMultipart20200721-4-1ga4vcl_html_7bc6d4123dc7e270.png)
5. Store the DNS name, which may be useful for future use.

## Step 2: Connecting our Registered App and Key Vault

1. Go to Access Policies menu in the Key Vault

![](RackMultipart20200721-4-1ga4vcl_html_6f8fd5539d9054d8.png)

1. Click on &quot;+ Add Access Policy&quot;. Do not worry about the existing default Policy.
2. In Add Policy page, under Secret Permission, select the 4 items as shown below. ![](RackMultipart20200721-4-1ga4vcl_html_4734306220be2c9d.png)
3. In the Select Principal Option, select the Registered App. In this case my-azure-apps, and click Select ![](RackMultipart20200721-4-1ga4vcl_html_6f8ceae1db433636.png)
4. Now it will appear in the Application Access Policy ![](RackMultipart20200721-4-1ga4vcl_html_cdc65b7df7ce1756.png)

## Step 3: Adding Secrets to your vault

1. Go to Secrets menu on the left-hand side
2. Click on + Generate/Import
3. Give a meaningful secret name and a value
4. Click Create.

![](RackMultipart20200721-4-1ga4vcl_html_29ba6d9f07fa8fc1.png)

1. Now you have created a secret and added into your vault. You can create any many secrets you want in one vault.

## Step 4: UiPath Activity

Your UiPath activity should look like one shown below

![](RackMultipart20200721-4-1ga4vcl_html_2f1331990ef5ccbb.png)

1. Get the Client ID and Secret stored in the Orchestrator
2. Create an Azure Scope with all the required parameters discussed in Azure Scope Section of this document
3. Inside the Do sequence, drag &quot;Get Secrets&quot; Activity.
  1. Input parameters:
    1. KeyVaultName: Give your Key vault name
    2. SecretFilter: give all your required Secret Key Names to get the values. Accepts String[].
  2. Output:
    1. Output is received as &quot;UiPath.Azure.Models.SecretInfo[]&quot;. This will be an array of Secret Values in the Order your queried.
4. The SecretInfo has Name-Value pair. We can get
  1. Name as SecretInfo(0).Name
  2. Secret as SecretInfo(0).Value

![](RackMultipart20200721-4-1ga4vcl_html_303992e4de0ed91a.png)

# Azure File Storage

## Step 1: Create Azure Storage

1. Login to Azure Portal
2. Create a new resource of Storage Account

![](RackMultipart20200721-4-1ga4vcl_html_72e4cef31c611f7b.png)

1. Give the required details in the screen shown below

![](RackMultipart20200721-4-1ga4vcl_html_d527101092d886d6.png)

1. Review and Create the Storage account
2. The account will be created and you can see the below screen ![](RackMultipart20200721-4-1ga4vcl_html_293bf279630f7c5a.png)

## Step 2: Create File Share and get required details

1. Click on File Shares
2. Create new File share by clicking the &quot;+ File Share&quot; button at the top

![](RackMultipart20200721-4-1ga4vcl_html_2af57a9ff4ce8aed.png)

1. Give a proper name and size for the file share

![](RackMultipart20200721-4-1ga4vcl_html_27dc59a9ed025eba.png)

1. Click open the file share. It will be empty at first
2. Go to Properties and Copy the URL
3. Now Go to Storage Account level and click on Access Keys
4. Copy the Storage Account Name and Key 1.Key

## Step 3: Set up local drive

1. Go to the Local Drive

![](RackMultipart20200721-4-1ga4vcl_html_3ee2a5dc837c94b7.png)

1. Click in any available Drives  Go to Computer  Click in Map Network Drive

![](RackMultipart20200721-4-1ga4vcl_html_c3b7f062698bb70a.png)

1. Select any available drive of your choice and give the URL we copied in the last step

![](RackMultipart20200721-4-1ga4vcl_html_ae0a7d62983e1e27.png)

  1. The URL must be supplied as the one shown just below the Folder textbox.
  2. If your URL looks like this https://\&lt;storageaccountname\&gt;.file.core.windows.net/\&lt;filesharename\&gt; this should be modified as

\\\&lt;storageaccountname\&gt;.file.core.windows.net\\&lt; filesharename\&gt;

1. Select &quot;Connect using different Credentials&quot;
2. Give the Storage account name we copied earlier as the user name
3. Give the Key1.key as password
4. Now the folder is setup

## Step 4: Add Files

You can add files in the Azure or in the local folder.