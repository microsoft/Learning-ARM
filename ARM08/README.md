# 8 - Demystifying ARM Templates: Linked and Nested Templates

As ARM deployments become more complex, using linked and nested templates allow you to break these deployments down into smaller reusable components.

- Linked templates: create reusable, composable, and modular deployments comprised of many individual arm templates.
- Nested templates: allows for advanced deployments scenarios like deploying to multiple ARM scopes or multiple resource groups from a single template file.

See the following for [documentation on both linked and nested templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/linked-templates?WT.mc_id=learnARM-github-frbouche).


## Azure DevOps - DevOps Lab - Video

[<img src="https://img.youtube.com/vi/rUY7wmz0W10/maxresdefault.jpg" width="50%">](https://channel9.msdn.com/Shows/DevOps-Lab/ARM-Series-8-Linked-and-Nested-Templates?WT.mc_id=learnARM-github-frbouche)
- [YouTube](https://youtu.be/rUY7wmz0W10)
- [Channel9](https://channel9.msdn.com/Shows/DevOps-Lab/ARM-Series-8-Linked-and-Nested-Templates?WT.mc_id=learnARM-github-frbouche)


## Linked template example

To use linked templates, the templates must first be staged on a publically accessible endpoint such as GitHub or an Azure Storage Blob. Consider using an Azure Storage account secured by a SAS token to keep your templates secure from public access. See [this document](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/linked-templates?WT.mc_id=learnARM-github-frbouche) for more details.

To add a linked template to your ARM template, add a *Microsoft.Resources/deployments* resource and the *templateLink* property configured with the *location* of the template. If needed, you can also [pass parameter values into the linked tempalte](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/linked-templates#parameters-for-linked-template?WT.mc_id=learnARM-github-frbouche) and [get output out of](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/linked-templates#parameters-for-linked-template?WT.mc_id=learnARM-github-frbouche) the linked template at deployment time.

In the below example, the main template deploys two linked templates. Notice the two parameters are defined, two variables hold the location of both of the linked templates, and two deployment resources are defined and configured, one for each of the linked templates.

The fist deployment resource, *storage* consumes the parent parameters and deploys the storage template. The second deployment resource consumes the parent parameters, depends on the *storage* deployment, and then deploys the identity template. Finally, output is created for the storage account endpoint.

All three templates can be found in this repository.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string",
            "defaultValue": "linkeddemo001"
        },
        "location": {
            "type": "string",
            "defaultValue": "eastus"
        }
    },
    "variables": {
        "linked-template": "https://raw.githubusercontent.com/neilpeterson/arm-deployment-demo/master/linked-tempate/artifacts/storage.json",
        "linked-template-two": "https://raw.githubusercontent.com/neilpeterson/arm-deployment-demo/master/linked-tempate/artifacts/identity.json"
    },
    "resources": [
        {
            "name": "storage",
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2019-10-01",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('linked-template')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "name": {"value": "[parameters('name')]"},
                    "location": {"value": "[parameters('location')]"}
                }
            }
        },
        {
            "name": "identity",
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2019-10-01",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/deployments','storage')]"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('linked-template-two')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "name": {"value": "[parameters('name')]"},
                    "location": {"value": "[parameters('location')]"}
                }
            }
        }
    ],
    "outputs": {
        "storageURI": {
            "type": "string",
            "value": "[reference('storage').outputs.storageEndpoint.value]"
        }
    }
}
```

## Nested template example

Unlike linked templates, where each template is stored in its own template files, nested templates allow you to store many individual templates in one file. There are several reasons where you might want to do this such as when deploying resources to multiple resource groups or deployment scopes.

To add a linked template to your ARM template, add a *Microsoft.Resources/deployments* resource and the *template* property that contains the nested template. Take note of nested template [expression evaluation scopes](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/linked-templates#expression-evaluation-scope-in-nested-templates?WT.mc_id=learnARM-github-frbouche) which determines at which level template expression are evaluated.

In the following example, a 'parent' template targets a [subscription deployment scope](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-to-subscription?WT.mc_id=learnARM-github-frbouche) and deploys a resource group into the subscription. A nested template is then embedded into the parent template. The nested template is scoped at the resource group level and deploys a storage account into the resource group created in the parent template. Also, note that a dependency is created between the nested deployment resource and the resource group resource. This dependency ensures that the resource group has been deployed before deploying the storage account into it.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "rgName": {
            "type": "string",
            "defaultValue": "nestedouter001"
        },
        "location": {
            "type": "string",
            "defaultValue":"eastus"
        },
        "storageName": {
            "type": "string",
            "defaultValue": "nestedouter001"
        }
    },
    "resources": [
        {
            "name": "[parameters('rgName')]",
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2019-10-01",
            "location": "[parameters('location')]",
            "tags": {
                "displayname": "resource-group"
            }
        },
        {
            "name": "storage",
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2019-10-01",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups', parameters('rgName'))]"
            ],
            "properties": {
                "expressionEvaluationOptions": {
                    "scope": "outer"
                },
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "resources": [
                        {
                            "name": "[parameters('storageName')]",
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2019-06-01",
                            "location": "[parameters('location')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Premium_LRS",
                                "tier": "Premium"
                            }
                        }
                    ]
                }
            }
        }
    ]
}
```

[<-- Episode/ Module 7](../ARM07/README.md) | [Episode/ Module 9 -->](../ARM09/README.md)`