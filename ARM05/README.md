# ARM05 - Variables

Variables are very useful in all king of scenarios to simplify things. Azure Resource Manager (ARM) templates aren't an exception and the variable will offer a great flexibility. In this chapter we will explains how you can use variables inside your template to make them easier to read, or to use. 




The following information can be helpful when you work with variables:

- Use camel case for variable names.
- Use variables for values that you need to use more than once in a template. If a value is used only once, a hard-coded value makes your template easier to read.
- Use variables for values that you construct from a complex arrangement of template functions. Your template is easier to read when the complex expression only appears in variables.
- Don't use variables for apiVersion on a resource. The API version determines the schema of the resource. Often, you can't change the version without changing the properties for the resource.
- You can't use the reference function in the variables section of the template. The reference function derives its value from the resource's runtime state. However, variables are resolved during the initial parsing of the template. Construct values that need the reference function directly in the resources or outputs section of the template.
- Include variables for resource names that must be unique.
- Use a copy loop in variables to create a repeated pattern of JSON objects.
- Remove unused variables.

#### Reference: 

- [Parameters in Azure Resource Manager templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-variables?WT.mc_id=learnARM-github-frbouche)

- [Best practices - variables](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-best-practices#variables?WT.mc_id=learnARM-github-frbouche)

---


[<-- Episode/ Module 4](../ARM04/README.md) | [Episode/ Module 6 -->](../ARM06/README.md)