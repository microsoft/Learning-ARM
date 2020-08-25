# 6 - Demystifying ARM Templates: Template Output


## Azure DevOps - DevOps Lab - Video

[<img src="https://img.youtube.com/vi/T-GnsabXcGY/maxresdefault.jpg" width="50%">](https://channel9.msdn.com/Shows/DevOps-Lab/ARM-Series-6-Template-Output?WT.mc_id=learnARM-c9-frbouche)
- [YouTube](https://youtu.be/T-GnsabXcGY)
- [Channel9](https://channel9.msdn.com/Shows/DevOps-Lab/ARM-Series-6-Template-Output?WT.mc_id=learnARM-c9-frbouche)


In the Outputs section of your template, you can specify values that will be returned after a successful deployment.  You can use those outputs in your templates to return properties from the resources you are deploying. For example, it might be helpful to get the endpoints for a newly deployed storage account, or the public IP address or a newly deployed resource.

The Output section uses the reference function to get the runtime state of the resources. To get the runtime state of a resource, you pass in the name or ID of a resource.

there are certain elements that make up the outputs section.

```JSON
"outputs": {
    "<output-name>": {
        "condition": "<boolean-value-whether-to-output-value>",
        "type": "<type-of-output-value>",
        "value": "<output-value-expression>",
        "copy": {
            "count": <number-of-iterations>,
            "input": <values-for-the-variable>
        }
    }
}
```

- **output-name**: Must be a valid JavaScript identifier.
- **condition**: (Optional)	Is a boolean value that indicates whether this output value is returned. When true, the value is included in the output for the deployment. When false, the output value is skipped for this deployment. When not specified, the default value is true.
- **Type**: The types of the output value supported are the same as the types of template input parameters.
    - NOTE: If you specify securestring for the output type, the value isn't displayed in the deployment history and can't be retrieved from another template. To use a secret value in more than one template, store the secret in a Key Vault and reference the secret in the parameter file. For more information, see Use Azure Key Vault to pass secure parameter value during deployment.
- **Value**: (Optional) Template language expression that is evaluated and returned as output value.
- **Copy**: (Optional) the Copy is used to return more than one value for an output.

For more information, and for the full documentation on outputs you can refer to [Output iteration in Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/templates/copy-outputs?WT.mc_id=learnARM-github-frbouche).

And for a complete set of examples of outputs, refer to the [Outputs in Azure Resource Manager template](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-outputs?WT.mc_id=learnARM-github-frbouche) document.


[<-- Episode/ Module 5](../ARM05/README.md) | [Episode/ Module 7 -->](../ARM07/README.md)
