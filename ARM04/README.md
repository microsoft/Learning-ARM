# 4 - Demystifying ARM Templates: Template Functions

So now you are happy about all the things you learn in those first chapters. But their good chance is that if try to execute the PowerShell or Bash command on your side you will end up with: 

> 
> ____ with given name ____ already exists.
> 

The error message saying that this name is already taken because some names needed to be unique. In this chapter, you will learn how to fix this problem and so much more using the [ARM templates functions](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions?WT.mc_id=learnARM-github-frbouche).

Meet the functions for ARM templates
------------------------------------

The ARM language is very close to JSON, but this is where there is a major distinction ARM language contains tons of functions to work with an array, string, date, deployment, number, object, resource, and more. Here just a few of them that we will covert in this chapter (check the full list [here](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions?WT.mc_id=learnARM-github-frbouche))

- reference
- resourceGroup
- resourceId
- parameters
- variables
- concat
- utcNow


Because one of the best ways to learn, let's apply some of those functions in different scenarios.

Function: concat and utcNow
---------------------------

Let's start with this problem where we need a unique global name for our resources. We could add some dynamic string, like the current time, at the name of the resource.

The function `utcNow(format)` can be used as default value for a parameter, we could add a parameter and use the function `concat()` to append this value to the name of our resource.

```json
    "parameters": {
        "mydate": {
           "type": "string",
           "defaultValue": "[utcNow('yyyyMMddTHHmm')]",
           "metadata": {
                "description": "description"
            }
        }
    },
    "resources": [
         {
            "name": "[concat(parameters('storageName'), parameters('mydate'))]",
            "type": "Microsoft.Storage/storageAccounts",
         }
    ]
```

That will make the name more unique, but there is still a chance that someone else deploys at the same time. And if I would like to re-depoy I will lose my omnipotence, because new resources will be created every time. Let's see how we could improve this situation.

Function: resourceGroup and uniqueString
----------------------------------------

The function `uniqueString` is one of those functions that are very simple and extremely useful. As the name suggests this function will create a deterministic hash string based on the values provided as parameters. This means the same hash every time... Wonderful! Now what we need is a unique value that doesn't change and that we could use to get a unique string.

When deploying in a resource group the thing that doesn't change IS the resourceGroup. And by using the function `resourceGroup()` we can retrieve the object properties. 

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "type":"Microsoft.Resources/resourceGroups",
  "location": "{resourceGroupLocation}",
  "managedBy": "{identifier-of-managing-resource}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

You access those properties by adding a "." and the name of the property. So `resourceGroup().id` will the ID, and `resourceGroup().location` the location. 

So a better way to create a unique suffix for our resource name would be to use the resourceGroupId as a parameter for uniqueString and concatenate that to our parameter.

```json
    "resources": [
         {
            "name": "[concat(parameters('storageName'), uniqueString(resourceGroup().id))]",
            "type": "Microsoft.Storage/storageAccounts",
         }
    ]
```

You could when deploying the same solution to two different locations include the location in the unique string generation. `uniqueString(resourceGroup().id, resourceGroup().location)`, this will make sure that each location has the respective unique name.


---

## Exercises

Now let's practice the things we learn in the little chapter. You can continue where you left at the previous chapter (ARM03 -Parameter) or start fresh with the files contained in the folder [begin-with](begin-with/azuredeploy.json)


### 🥖 Make the suffix in lowercase

In some cases, the resource name can only contain lower case characters. Using the function [toLower()](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions-string#tolower?WT.mc_id=learnARM-github-frbouche) make sure the suffix is all in lower case.



### 🥖 Make the suffix in 5 characters long

Having a very long resource name could be confusing. In this next exercise use the function [substring()](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions-string#substring?WT.mc_id=learnARM-github-frbouche) make sure the suffix is only five characters long.


### 👀 Did you noticed?

Did you notice, instead of entering a value "location" to each resource we are using `resourceGroup().location`?

---

#### References

- [ARM templates functions](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions?WT.mc_id=learnARM-github-frbouche)

[<-- Episode/ Module 3](../ARM03/README.md) | [Episode/ Module 5 -->](../ARM05/README.md)
