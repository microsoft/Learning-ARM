# 11 - Demystifying ARM Templates: GitHub Actions With ARM Templates
If you need Infrastructre as Code for Azure, ARM templates are an extremely powerful and flexible solution. This power and flexibility becomes even more impressive once you add IaC into your CI/CD Pipelines. 

Some key benefits
- having your IaC right alongside your source code means everything gets versioned together
- infrastructure changes can be reviewed with the same pull request process as application code
- being able to go from nothing in Azure, to a fully provisioned and configured environment in Azure, and then deploying your App into that environment is priceless
- protection from configuration drift in subsdquent releases is awesome

## Azure DevOps - DevOps Lab - Video

[<img src="https://img.youtube.com/vi/DHFjc3UWvx4/maxresdefault.jpg" width="50%">](https://channel9.msdn.com/Shows/DevOps-Lab/ARM-Series-11-GitHub-Actions?WT.mc_id=learnARM-github-abewan)
- [YouTube](https://youtu.be/DHFjc3UWvx4)
- [Channel9](https://channel9.msdn.com/Shows/DevOps-Lab/ARM-Series-11-GitHub-Actions?WT.mc_id=learnARM-github-abewan)


## Deploy a complete ARM template using GitHub Actions
GitHub Actions is GitHub's workflow engine where you can automate all sorts of things including CI/CD. A very basic explanation of actions is it's a task engine that does one task after another after another. Tasks in GitHub Actions are called actions. 

Out of the box, GitHub comes with a bunch of actions that do all sorts of things. There is also a marketplace where the community has created actions people can just reference and use. You can even write your own custom actions. And you can run commands from the command line, either using bash or powershell.

## Crafting the YAML
The general strategy I use when building out my CI/CD pipelines is how would I do this completely from the command line. 

First, in parallel, I would build my application, and also provision the resources I needed up in Azure. And then, when both the building of my application and the provisioning of my environment were both done, I would then deploy my app. 

Based off of that, my YAML should have 3 jobs, a build job and a provision job running in parallel. And a deploy job which waits for both the build and deploy job to finish.

```
name: DevOps Lab ARM Demo - CICD

on: 
  push:
  workflow_dispatch:

jobs:
  build:
    ...

  provisionAndConfigure:
    ...

  deploy:
    needs: [build, provisionAndConfigure] 
    ...  
```

### Provision and Configure Job
For this tutorial, we will be concentrating on the provisionAndConfigure job. Here is where we provision and configure our infrastructure up in Azure using our ARM template.

At the time of filming this video, there were no Deply Azure Resource Manager templates GithHub Actions from Microsoft. Therefor, the tactic I used was again asking myself how would I do this from the command line. And the answer to that was by using the Azure CLI. The Azure CLI is pre-installed on the GitHub runners so this should be easy peasy right? 

_Note: Microsoft very recently published their GitHub Actions for ARM templates deployment. If I was doing this video today, I would use the Deploy Azure Resource Manager templates by using the GitHub actions published by Microsoft._

All that said, to use the Azure CLI to deploy an arm template, I needed 2 things:

- ARM template describing my environment
- Service Principal to log in with

The demo environment consists of an app service plan and an app service in Azure. [Here](./tt-iac.json) is the ARM template which describes this environment. For the demo, this arm template sat at ~/Deploy/ARM/tt-iac.json.

I [created a service principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest&WT.mc_id=learnARM-github-abewan) using the Azure CLI. This returned me:

- Service principal name - http://AbelDemoSubscriptionSP
- Service principal tenant - 72f988bf-86f1-41af-91ab-2d7cd011db47
- Service principal secret - some randomly generated GUID.

I added the service principal secret to the GitHub project Secrets section with the name SERVICE_PRINCIPAL_SECRET.

Here is my yaml

```
  provisionAndConfigure:
      runs-on: ubuntu-latest
      steps:
        # checkout code from repo
        - name: Checkout code
          uses: actions/checkout@v1

        # provision and configure infrastructure using azure cli deploying arm template
        - name: Deploy ARM template
          env:
            SERVICE_PRINCIPAL: http://AbelDemoSubscriptionSP
            SERVICE_PRINCIPAL_TENANT: 72f988bf-86f1-41af-91ab-2d7cd011db47
            RESOURCE_GROUP: DevOpsLabARMDemoRG
            WEB_APP_NAME: devopslabtt
            LOCATION: "westus"
            ARM_FILE: ./Deploy/ARM/tt-iac.json
          run: |
            az login \
                --service-principal \
                --username ${SERVICE_PRINCIPAL} \
                --password ${{ secrets.SERVICE_PRINCIPAL_SECRET }} \
                --tenant ${SERVICE_PRINCIPAL_TENANT}
            az deployment sub create \
                --location ${LOCATION} \
                --template-file ${ARM_FILE} \
                --parameters webAppName=${WEB_APP_NAME} name=${RESOURCE_GROUP}
```

Using an ubuntu runner, I do the following

- Checkout my code for my repo. This is so I download the ARM template from source control onto the runner so that I can use this ARM template to provision my infrastrucutre.

- Deploy my arm template by using the Azure CLI from the command line.
    - create environment variables to hold
        - Service principal name
        - Service principal tenant
        - Azure resource group name
        - Web app name
        - Location
        - Path to the ARM file
    - use the Azure CLI
        - Log into azure with my service principal
        - Deploy my ARM template

## Conclusion
GitHub Actions is a great workflow automation engine inside of GitHub where we can automate all sorts of things, including CI/CD. Because of the ease of using command line commands with Actions, it's super easy to use the Azure CLI to do whatever Azure-y tasks needed, including deploying ARM templates using GitHub Actions.

_Note: Again, with the newly published ARM Template Deploy Actions from Microsoft, I would totally use those to deploy my ARM templates. However, this technique of using the Azure CLI and Service Principlas is still a very valuable technique which can be used to do whatever Azure-y type tasks needed._



## References
- [Azure CLI](https://docs.microsoft.com/cli/azure/what-is-azure-cli?view=azure-cli-latest&WT.mc_id=learnARM-github-abewan)
- [Install the Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&WT.mc_id=learnARM-github-abewan)
- [Service Principal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals?WT.mc_id=learnARM-github-abewan)
- [Creating service principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest&WT.mc_id=learnARM-github-abewan)
- [logging into azure cli with service principal](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest&WT.mc_id=learnARM-github-abewan)
- [Deploying ARM template using Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-cli?WT.mc_id=learnARM-github-abewan)
- [Microsoft's Deploy ARM Templates GitHub Actions](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-github-actions?WT.mc_id=learnARM-github-abewan)





[<-- Episode/ Module 10](../ARM10/README.md) | [Episode/ Module 12 -->](../ARM12/README.md)
