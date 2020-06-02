---
page_type: sample
languages:
- csharp
products:
- dotnet
description: "Add 150 character max description"
urlFragment: "update-this-to-unique-url-stub"
---

# Official Microsoft Sample

<!-- 
Guidelines on README format: https://review.docs.microsoft.com/help/onboard/admin/samples/concepts/readme-template?branch=master

Guidance on onboarding samples to docs.microsoft.com/samples: https://review.docs.microsoft.com/help/onboard/admin/samples/process/onboarding?branch=master

Taxonomies for products and languages: https://review.docs.microsoft.com/new-hope/information-architecture/metadata/taxonomies?branch=master
-->

Give a short description for your sample here. What does it do and why is it important?

## Contents

Outline the file contents of the repository. It helps users navigate the codebase, build configuration and any related assets.

| File/folder       | Description                                |
|-------------------|--------------------------------------------|
| `src`             | Sample source code.                        |
| `.gitignore`      | Define what to ignore at commit time.      |
| `CHANGELOG.md`    | List of changes to the sample.             |
| `CONTRIBUTING.md` | Guidelines for contributing to the sample. |
| `README.md`       | This README file.                          |
| `LICENSE`         | The license for the sample.                |

## Prerequisites

Outline the required components and tools that a user might need to have on their machine in order to run the sample. This can be anything from frameworks, SDKs, OS versions or IDE releases.

## Setup

Explain how to prepare the sample once the user clones or downloads the repository. The section should outline every step necessary to install dependencies and set up any settings (for example, API keys and output folders).

## Running the sample

Outline step-by-step instructions to execute the sample and see its output. Include steps for executing the sample from the IDE, starting specific services in the Azure portal or anything related to the overall launch of the code.

## Key concepts

Provide users with more context on the tools and services used in the sample. Explain some of the code that is being used and how services interact with each other.

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Learning Azure Resource Manager (ARM) Template

In this repository you will find a series of tutorial paired with videos to guide you through learning the best practice about Azure Resource manager (ARM) template.

## Tutorials

### [ARM01 - Introduction to ARM templates](ARM01/README.md)

In this tutorial you will learn the what, the why about Azure Resource manager (ARM) template. Why why talk about declarative, idempotency.


### [ARM02 - Create first template](ARM02/README.md)

Now it's time to code. This session will introduce Visual Studio Code tools, and show you how to create your first template from snippet, and how to deploy it.


### [ARM03 - Parameters](ARM03/README.md)

This is where we will define our parameters. There is multiple type of parameters but before we go list them and see some scenarios, let's understand how parameters are defined.


### [ARM04 - Template functions](ARM05/README.md)

What are template functions, show common functions, add and use a template function.


### [ARM05 - Variables](ARM04/README.md)

Variables are very useful in all king of scenarios to simplify things. Azure Resource Manager (ARM) templates aren't an exception and the variable will offer great flexibility. In this chapter, we will explain how you can use variables inside your template to make them easier to read, or to use. 


### [ARM06 - Template output](ARM06/README.md)

What are outputs and how are they used.


### [ARM07 - Controlling deployment](ARM07/README.md)

Using dependencies to control resource deployment order.


### [ARM08 - Nested and linked templates](ARM08/README.md)

How to modularize templates using nested and linked templates.


### [ARM09 - Finding template examples](ARM09/README.md)

Using vs code snippets, quick start gallery, and exported templates.


### [ARM10 - How to reverse engineer](ARM10/README.md)

How to get a functional ARM template from something that is already deployed.


### [ARM11 - Azure DevOps with ARM template](ARM11/README.md)

Deploy an complete ARM template from Azure DevOps


### [ARM12 - GitHub Actions with ARM template](ARM12/README.md)

Deploy an complete ARM template using GitHub Actions


~
