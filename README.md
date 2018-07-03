# Containers and DevOps

Fabrikam Medical Conferences provides conference web site services tailored to the medical community. Their business has grown and the management of many instances of the code base and change cycle per tenant has gotten out of control. The goal of this workshop is to help them build a POC that will migrate their code to a more managable process that involves containerization of tenant code, a better DevOps workflow, and a simple lift-and-shift story for the database back end.

## Target Audience

* Application developer
* Infrastructure architect

## Abstract

### Workshop

In this workshop, you will build a proof of concept (POC) that will transform an existing PaaS application to a container-based application. This POC will deliver a multi-tenant web app hosting solution leveraging Azure Container Service (AKS), Docker containers on Linux nodes and a migration from MongoDB to CosmosDB.

### Whiteboard Design Session

This whiteboard design session is designed to help attendees understand the choices related to building and deploying containerized applications in Azure, critical decisions around this and other aspects of the solution including ways to lift-and-shift parts of the application to reduce applications changes.

By the end of this design session you will be better able to design solutions that target Azure Kubernetes Service (AKS) and define a DevOps workflow for containerized applications.

### Hands-on Lab

This hands-on lab is designed to guide you through the process of building and deploying Docker images to the Kubernetes platform hosted on Azure Container Services (AKS) in addition to learning how to work with dynamic service discovery, service scale-out and HA.

At the end of this lab you will be better able to build and deploy containerized applications to Azure Kubernetes Service and perform common DevOps procedures.

## Azure services and related products

- Azure Kubernetes Service (AKS)
- Azure Container Registry
- Docker
- CosmosDB (including MongoDB API)

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

