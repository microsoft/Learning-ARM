# ARM02 - Create first template

Now it's time to code. This session will introduce Visual Studio Code tools, and show you how to create your first template from snippet, and how to deploy it.

The Azure Resource Manager (ARM) Tools for Visual Studio Code, is available from the marketplace (for free), or directly from VSCode. Click the **Extensions** button from the Side Bar and search for "**ARM**".

![Visual Studio extension][extension] 

Now from any JSon (.json) file the extension will automatically be active. Create a new File and save it as a JSON file (ex: firstARMTemplate.json). Start typing `arm`, you should see the context menu popping.

![Create an ARM template][skeleton]

- That will add an empty template.
- Now type `arm-storage`, you should see a list of Azure resources filtered down to show only the storage. Click it and you should have now your first template done.

![armStorage][armStorage]

Now the tools is asking you to enter a name for your resource. Enter: learningARMStorage.


```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "name": "learningARMStorage",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "learningARMStorage"
            },
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ],
    "outputs": {},
    "functions": []
}

```

- You have now a template
- show auto-complete stuff
- validation in the outputs section.
- Notice the `location` is using an expression. In fact it's a ARM function, we will learn more about it in the episode/ module 5.


## References

- https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools

[extension]:  medias/extension.png
[skeleton]:  medias/skeleton.gif
[armStorage]:  medias/arm-storage.png


[<-- Episode/ Module 1](../ARM01/README.md) | [Episode/ Module 3 -->](../ARM03/README.md)