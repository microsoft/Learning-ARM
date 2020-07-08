# 7 - Demystifying ARM Templates: Controlling Deployment

In the previous tutorial we were deploying only one resource. In most cases you will be deploying many resources with one template, and it's important to have some control over the deployment.

In this tutorial we will introduce some key feature to keep your deployment in control.

## Dependency

Let's say we want to deploy a website, something simple. In Azure we will need two resources: an App Service and a Service plan. THe Service plan needs to be created before the App Service. We know Azure will paralyzed as much as possible during a deployment, to make sure the order is respected we need to use `dependsOn`. 

Let's start a new ARM template. If you are using Visual Studio Code with the extension it will only take you a few clicks to get started.

1- Create a new file named `temp.json`
1- Type "!arm" to get the skeleton of the template.
1- In the `resources` section type `arm-web-app`

You should end-up with:

```json
    "resources": [
        {
            "name": "webApp1",
            "type": "Microsoft.Web/sites",
            "apiVersion": "2018-11-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/appServicePlan1')]": "Resource",
                "displayName": "webApp1"
            },
            "dependsOn": [
                "[resourceId('Microsoft.Web/serverfarms', 'appServicePlan1')]"
            ],
            "properties": {
                "name": "webApp1",
                "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', 'appServicePlan1')]"
            }
        }
    ]
```

Notice the `dependsOn` property? It's expecting a Service plan 'appServicePlan1'. If you try to validate or deploy this template it would fail because it's expecting a resource 'appServicePlan1'. 

Let's add the service plan. With the extension just type "arm-plan". Now the template should look like this. And even if it's not following the best practices we learn, it is perfectly valid. Even more, we are sure that the service plan will always be deployed before the app service.  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "functions": [],
    "variables": {
    },
    "resources": [
        {
            "name": "webApp1",
            "type": "Microsoft.Web/sites",
            "apiVersion": "2018-11-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/appServicePlan1')]": "Resource",
                "displayName": "webApp1"
            },
            "dependsOn": [
                "[resourceId('Microsoft.Web/serverfarms', 'appServicePlan1')]"
            ],
            "properties": {
                "name": "webApp1",
                "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', 'appServicePlan1')]"
            }
        },
        {
            "name": "appServicePlan1",
            "type": "Microsoft.Web/serverfarms",
            "apiVersion": "2018-02-01",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "F1",
                "capacity": 1
            },
            "tags": {
                "displayName": "appServicePlan1"
            },
            "properties": {
                "name": "appServicePlan1"
            }
        }
    ],
    "outputs": {}
}
```

Dependencies (aka dependsOn) are also very important when you have child resources. As an exemple our website could have a child resources of type `sourceControl`. This is often used in continuous deployment to deploy the code. In those cases the `sourceControl` would have a `dependsOn`referencing his parent the App service. 

To learn more about those particularities read [Define the order for deploying resources in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/define-resource-dependency?WT.mc_id=learnARM-github-frbouche) from the Microsoft documentation.

## Conditional

Now let's say when you are deploying in production, and in this case only, you would like have an extra storage for backups. In ARM this is relatively easy to do with the `condition` property available in most of the resources.

Let's think about the storage used in the previous tutorial. By adding a property `condition` and using the [comparison functions for ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions-comparison?WT.mc_id=learnARM-github-frbouche) we could make the storage account to be deploy only when the parameter `environment` is equal to **PROD**.

```json

    "condition": "[equals(parameters('environment'), 'PROD')]",

```

---

## Exercises

Now let's practice the things we learn in all the chapters and improve our ARM template. You can continue where you left at the previous chapter (ARM03 -Parameter) or start fresh with the files contained in the folder [begin-with]([begin-with/azuredeploy.json)


### 🥖 Apply all the best practices 

Add an App Service and a Service plan with the conditional backup Storage, Add apply all the changes discussed in all the previous chapters.


### 🥖 Apply the conditional deployment of the storage 

Just like discuss apply the condition to deploy the Azure storage only when the parameter is equal to PROD.


---


#### Reference: 

- [Define the order for deploying resources in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/define-resource-dependency?WT.mc_id=learnARM-github-frbouche)
- [Tutorial: Use condition in ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-use-conditions)
- [Comparison functions for ARM templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions-comparison?WT.mc_id=learnARM-github-frbouche)


[<-- Episode/ Module 6](../ARM06/README.md) | [Episode/ Module 8 -->](../ARM08/README.md)