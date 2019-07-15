---
title: "Azure: CosmosDB Provision"
excerpt: "Azure ARM template is great, but it does not give you everything you need when comes to provision your resource or infrastructure."
tags: 
    - Powershell
    - CosmosDB
    - Azure
date:   2019-5-10 01:00:00 -0600
#categories: Azure
---

Azure ARM (Azure Resource Manager) template is a great way to provision your resource/infrastructure and using it for Infrastructure as Code (IaC). However, often there is a limitation with it. 

In this example, I'm trying to setup a CosmosDB account with Database and Collections. Unfortunately, ARM template only supports CosmosDB account, not Database and Collections provisioning. When comes to CI/CD pipeline setup, we always want to keep it simple, not having too many tasks. We do not want one task using ARM to create CosmosDB account, another task to create Database and task to create Collections. Ideally we want a single task to achieve them. 

Here is a single Azure CLI command that can help you to create
* CosmosDB Account
* Database
* Collections

```bash
$resourceGroup = "{Resource Group Name}"
$accountName = "{Database Account Name}"
$dbName = "{Database Name}"
$collections =  @("Collection1", "Collection2")
az cosmosdb create --name $accountName `
                   --resource-group $resourceGroup
                   
az cosmosdb list-connection-strings --name $accountName `
                                    --resource-group $resourceGroup

az cosmosdb database create --db-name $dbName `
                --name $accountName `
                --resource-group-name $resourceGroup 

foreach ($element in $collections) {
	az cosmosdb collection create --collection-name $element `
                --db-name $dbName `
                --name $accountName `
                --resource-group-name $resourceGroup 
}
```
