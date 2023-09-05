# Deploy Azure Cognitive Search to your subscription

In this section you will provision all Azure resources required to complete the solution. We will use a pre-defined ARM template with the definition of all Azure services used in this exercise.

## Azure services provisioned using the ARM template

The following Azure services will be deployed in your subscription:

Name                        | Type | Pricing Tier | Pricing Info |
----------------------------|------|--------------|--------------|
acs-*name*-*env parameter*      | Azure Cognitive Search | Standard | https://azure.microsoft.com/en-in/pricing/details/search/
aoai-*name*-*env parameter*     | Azure Open AI Service | Standard | https://azure.microsoft.com/en-us/pricing/details/databricks/
appi-*name*-*env parameter*     | Azure App insights | Pay-as-you-go |  https://azure.microsoft.com/en-in/pricing/details/monitor/
Failure Anamolies*name*-*env parameter*  | Alerts | Refer App Insights Pricing |  https://azure.microsoft.com/en-in/pricing/details/monitor/
kv-*name*-*env parameter*       | Key Vault | Standard | https://azure.microsoft.com/en-in/pricing/details/key-vault/
aml-*name*-*env parameter*      | Azure AI | Machine Learning Studio | Pay-as-you-go | https://azure.microsoft.com/en-in/pricing/details/machine-learning/
st*name**env parameter*       | Azure Storage | Storage V2 | https://azure.microsoft.com/en-in/pricing/details/storage/

<br>**IMPORTANT**: When you deploy the resources in your own subscription you are responsible for the charges related to the use of the services provisioned. If you don't want any extra charges associated with the resources you should delete the resource group and all resources in it.

The estimated time to complete the deployment is: > ~5 minutes.


## Prepare your Azure subscription
In this section you will use the Azure Portal to create a Resource Group for the deployment. 

**IMPORTANT**|
-------------|
**Execute these steps on your host computer**|

1.	Open the browser and navigate to https://portal.azure.com
2.	Log on to Azure using your account credentials
3.	Once you have successfully logged on, locate the **Favourites** menu on the left-hand side panel and click the **Resource groups** item to open the **Resource groups** blade.
4.	On the **Resource groups** blade, click the **+ Add** button to create a new resource group.
5.	On the **Create a resource group** blade, select your subscription in **Subscription** drop down list.
6.	In the Resource group text box enter “AOAIE2E-Lab”

**IMPORTANT**: The name of the resource group chosen is ***not*** relevant to the successful completion of the deployment. If you choose to use a different name, then please proceed with the rest of the deployment using your unique name for the resource group.

8.	In the Region drop down list, select one of the regions from the list below.

**IMPORTANT**: The ARM template you will use to deploy the resources uses the Resource Group region as the default region for all services, except the Azure OpenAI Instance. As of this writing, Azure open AI service is only available in the below set of regions, so please use one of the recommended regions in the list below.

Recommended Regions|
-------------------|
East US|
East US2|
France Central|
Canada East|
Japan East|
North Central US|
West Europe|
UK South|
Australia East|

9.	Proceed to create the resource group by clicking **Review + Create**, and then **Create**.

Note:  As of this writing the model deployment via ARM template had an error, hence we recommend you to manually deploy the model.

* Check the availabiliy of Azure OpenAI [here](https://azure.microsoft.com/en-in/explore/global-infrastructure/products-by-region/?products=cognitive-services)

--------------------------------------
<!-- ## Deploy Azure Services
In this section you will use automated deployment and ARM templates to automate the deployment of all Azure Data Services used in labs 1 through to 5.

1. You can deploy all Azure services required in each lab by clicking the **Deploy to Azure** button below.


[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https://raw.githubusercontent.com/microsoft/acs-promptflow-experiment/main/arm-templates/aml_workspace.json)

2. You will be directed to the Azure portal to deploy the “AOAIE2E ARM template from this repository. On the **Custom deployment** blade, enter the following details:
    <br>- **Subscription**: [your Azure subscription]
    <br>- **Resource group**: [select the resource group you created in the previous section]

    Please review the Terms and Conditions and check the box to indicate that you agree with them.

3. Follow the instructions in the screen and deploy the resources.

Note: Optionally, you may also use AZ Cli to deploy this template using the below commands -->

## Deploy the resources using AZ CLI

1. az login --tenant "myTenantID" // Login to Azure CLI
2. az account show --output table // List the Azure subscriptions
3. az account set --subscription "My Demos" //Set the default subscription
4. az group show --create "Name of the resourcegroup" --location westus //To create a resourcegroup
5.  Deploy the ARM template using the below command

## AZ Cli syntax

az deployment group create \
  --name DeployTest \
  --resource-group 'Name of the resource group' \
  --template-file 'Template file' \
  --parameters 'List of parameters'

Refer to [Microsoft Learn](https://learn.microsoft.com/en-us/cli/azure/deployment/group?view=azure-cli-latest) for more information.

## AZ Cli - Example

  az deployment group create \
  --resource-group 'acs-promptflow-experiment' \
  --template-file arm-templates/aml_workspace.json \
  --parameters name=aml environment=dev aoai_Location=canadaeast

### Example

  az deployment group create --resource-group 'acs-promptflow-experiment' --template-file arm-templates/aml_workspace.json --parameters name=aml environment=dev

Note:
- Name and Environment names are used as suffix
- All the resources are deployed in the same region as the resource gorup, however azure Open AI may not available in the same location, hence you can choose the AOAI location using thhis parameter, aoai_Location = Azure OpenAI Location.