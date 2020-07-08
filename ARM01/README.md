# 1 - Demystifying ARM Templates: Introduction to ARM Templates

In this tutorial you will learn the what, the why about Azure Resource manager (ARM) template. Why why talk about declarative, idempotency.

## Azure DevOps - DevOps Lab - Video

[<img src="https://img.youtube.com/vi/VWe-stknCIM/maxresdefault.jpg" width="50%">](https://youtu.be/VWe-stknCIM)
- [YouTube](https://youtu.be/VWe-stknCIM)
- [Channel9](https://channel9.msdn.com/Shows/DevOps-Lab/Demystifying-ARM-Templates-Intro-to-ARM-Templates?term=Demystifying%20ARM&lang-en=true)

---


## What it is

Azure Resource manager (ARM) template is a way to describe your infrastructure....

```
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {},
    "functions": []
}
```


## Why

Here are some advantages to use ARM template :

- A template file is light and easy to keep in a repository.
- It's very simple to have the exact same template deployed in multiple environments.
- The template is idempotent.
- ARM templates are really fast to deploy.
- Easy to edit/ customize/ expand.
- Easy to delete.

## References

- https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview


[Episode/ Module 2 -->](../ARM02/README.md)