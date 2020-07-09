# 5 - Demystifying ARM Templates: Variables

Variables are very useful in all king of scenarios to simplify things. Azure Resource Manager (ARM) templates aren't an exception and the variable will offer great flexibility. In this chapter, we will explain how you can use variables inside your template to make them easier to read, or to use. 

How to define Variables in am ARM template
------------------------------------------

In the previous chapter, we created a suffix to make sure the resource name was unique. However, we needed to copy our "formula" all over the ARM template. Let's create a variable `suffix` with our formula and another one with the name of our resource.

The variables are defined in the `variables` section of the ARM template.

```json
    "variables": {
        "suffix": "[uniqueString(resourceGroup().id)]",
        "primeStorageName":"[toLower(concat(parameters('storageName'), variables('suffix')))]"
    }
```
And now to use those variables I will use the very handy function `variables()` that returns the value of the variable name passed in parameter. See now how clearer is our template!

```json
    "resources": [
        {
            "name": "[variables('primeStorageName')]",
            "type": "Microsoft.Storage/storageAccounts",
        }
```

Make our ARM template it easy to use
-------------------------------------

Sometimes, the person who needs to deploy our ARm template doesn't know all the details about the resources. When deploying a Virtual machine by example what the difference between the SKU: D2V2, F16sv2, or NV24?! What the user knows is that he/she needs a VM for X scenario. We can use variables to make those choices easier and make our template easier to use with the T-Shirt size pattern.

In our current template, we need to pass the `storageSKU` this information is technical, and some person may hesitate to pick one, or just pick the biggest to avoid missing something. We could instead replace the parameter **storageSKU** by one named **environment**. Base on that information we know as the creator of the template what is the good SKU to use in each scenario.


```json
    "parameters": {
        "storageName": {
           "type": "string",
           "metadata": {
                "description": "Name of your storage account. Text only no space."
            },
            "maxLength": 10
        },
        "environment": {
            "type": "string",
            "allowedValues": [
                "DEV",
                "PROD"
            ],
            "defaultValue": "DEV",
            "metadata": {
            "description": "Define the type of environment and resource to deploy. "
            }
        }
    }
```


What we need is a complex variable that will contain the SKU and tier for each deployment environment that we will support. Let say we have **DEV** and **PROD**.

```json
    "variables": {
        "suffix": "[uniqueString(resourceGroup().id)]",
        "primeStorageName":"[toLower(concat(parameters('storageName'), variables('suffix')))]",
        "storageInfo": {
            "DEV": {
                "storageSKU": "Standard_LRS",
                "storageTier": "Standard"
            },
            "PROD": {
                "storageSKU": "Premium_LRS",
                "storageTier": "Premium"
            }
        }
    }
```

The last step is to define a variable for SKU and Tier and assigned them a value base on the parameter environment.


```json
    "variables": {
        "suffix": "[uniqueString(resourceGroup().id)]",
        "primeStorageName":"[toLower(concat(parameters('storageName'), variables('suffix')))]",
        "storageInfo": {
            "DEV": {
                "storageSKU": "Standard_LRS",
                "storageTier": "Standard"
            },
            "PROD": {
                "storageSKU": "Premium_LRS",
                "storageTier": "Premium"
            }
        },
        "storageSKU": "[variables('storageInfo')[parameters('environment')].storageSKU]",
        "StorageTier": "[variables('storageInfo')[parameters('environment')].storageTier]"
    }
```

This is very powerful and useful. Now we just need to use those variables inside the template.


```json
    "resources": [
        {
            "name": "[variables('primeStorageName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "tags": {
                "displayName": "[variables('primeStorageName')]"
            },
            "location": "[resourceGroup().location]",
            "kind": "StorageV2",
            "sku": {
               "name": "[variables('storageSKU')]",
               "tier": "[variables('storageTier')]"
             },
            "properties": {
                "supportsHttpsTrafficOnly": true
            }
        }
    ]
```


Best practices
--------------

Here a few of the best practices that can be helpful when you work with variables (complete list [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-best-practices#variables?WT.mc_id=learnARM-github-frbouche)):

- Use camel case for variable names.
- Use variables for values that you need to use more than once in a template. If a value is used only once, a hard-coded value makes your template easier to read.
- Use variables for values that you construct from a complex arrangement of template functions. Your template is easier to read when the complex expression only appears in variables.
- Don't use variables for apiVersion on a resource. The API version determines the schema of the resource. Often, you can't change the version without changing the properties for the resource.
- Remove unused variables.


--- 


## Exercises

Now let's practice the things we learn in all the chapters and improve our ARM template. You can continue where you left at the previous chapter (ARM03 -Parameter) or start fresh with the files contained in the folder [begin-with](begin-with/azuredeploy.json)


### 🥖 Clean-up the ARM tempalte

Apply all the changes discussed in this chapter.


### 🥖 Different name by the environment

When the resources are deployed in production apply an extra suffix to the storage name: "stg" (ex: armdemo stg xxxxx).


### 🥖 Resource name with a fix length

Using what you learn in this module and the previous one makes sure the name of the storage account is sixteen characters long when in production. 

You will need the function [length()](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions-array#length?WT.mc_id=learnARM-github-frbouche) and [min()](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions-numeric#min?WT.mc_id=learnARM-github-frbouche)



#### Reference: 

- [Parameters in Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-variables?WT.mc_id=learnARM-github-frbouche)

- [Best practices - variables](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-best-practices#variables?WT.mc_id=learnARM-github-frbouche)

---


[<-- Episode/ Module 4](../ARM04/README.md) | [Episode/ Module 6 -->](../ARM06/README.md)