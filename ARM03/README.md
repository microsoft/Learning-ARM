# ARM03 - Parameters

In the previous tutorials we learn how to create resources using an Azure Resources Manager (ARM) template. However, it's was all static and defined *inside* the template.  In this tutorial you will learn how to use parameters to make your template dynamic and easier to use.

Parameter Types
---------------

To make our template dynamic we can pass some information when we call it using parameters. Remember the section parameters in the top of the templates?

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    ...
```
This is where we will define our parameters. There is multiple type of parameters but before we go list them and see some scenarios, let's understand how parameters are defined.

## Parameters Definition in Azure Resource Manager templates 

It's easy to thinks about parameters like a key pair value. Something like a parameters as a name and a value. But there is much more then that. Let's examine here our parameter section.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Premium_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }
}
```

There is one parameter define here named `storageSKU`. It's a string, but because we didn't when the users to write any values we used the `allowedValues` property to list the allowed value for our parameter. AllowedValues is an array of values of any types.

It's a good practice to used two other properties to help the user understand how to use our parameter: `defaultValue` and `description`.  The default value it's a way to say  to the user if you don;t know just say nothing. It's an excellent way  to make our template easy to use without compromising the flexibility.  Description, part of the metadata section, Is where of  course you give the most detail information about the  expected value and/ or the impact on the deployed resources.

To complement those there is also MinValue, MinLenght, MaxValue, and MaxLenght that will limits what's acceptable as value.

## Parameter Types


## Using Parameters


## Exercices

### 🥖 Add the parameter storageSKU



### 🥖 Add the parameter storageName


### 🥖 Use Parameters



Reference: [Parameters in Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-parameters?WT.mc_id=learnARM-github-frbouche)


[<-- Episode/ Module 2](../ARM02/README.md) | [Episode/ Module 4 -->](../ARM04/README.md)