## Containers and DevOps

## Whiteboard design session student guide

##

November 2017

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2017 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at [https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx](https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx) are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

# Contents

Containers and DevOps student guide        1

Abstract and learning objectives        1

Step 1: Review the customer case study        1

Step 2: Design a proof of concept solution        8

Step 3: Present the solution        10

Wrap-up        11

Additional references        11

# Containers and DevOps student guide

## Abstract and learning objectives

Build a PoC to deliver a multi-tenant web app hosting solution leveraging Azure Container Service with Kubernetes, Docker containers, and Linux nodes. 

Attendees will be better able to deploy Docker-based applications and scale them with Azure Container Service and Kubernetes orchestration. In addition,

- Create &amp; run a Docker Application
- Deploy to the Azure Container Service with Kubernetes orchestration
- Implement load balancing and service discovery
- Scale the application and test availability

## Step 1: Review the customer case study

**Outcome**

Analyze your customer&#39;s needs.

#### Facilitator/subject matter expert (SME) presentation of customer case study

Timeframe: 15 minutes

Directions: With all participants in the session, the facilitator/SME presents an overview of the customer case study along with technical tips.

1. Meet your table participants and trainer.
2. Read all of the directions for Steps 1–3 in the Student guide.
3. As a table team, review the following customer case study.

#### Customer situation

Fabrikam Medical Conferences provides conference website services tailored to the medical community. They started 10 years ago building a few conference sites for a small conference organizer. Word of mouth has spread since then, and Fabrikam Medical Conferences is now a well-known industry brand. They currently handle over 100 conferences per year and growing.

Medical conferences typically have low budget websites as they are usually between 100 to only 1500 attendees. At the same time, the conference owners have significant customization and change demands that require quick turnaround to the live sites. These changes can impact various aspects of the system from UI to backend, including conferences, registration, and payment terms.

The VP of Engineering at Fabrikam, Arthur Block, has a team of 12 developers who handle all aspects of development, testing, deployment, and operational management of their customer sites. Due to customer demands, they have issues with the efficiency and reliability of their development and DevOps workflows.

They host conference sites in Azure with the following topology and platform implementation:

-
  - They build conference websites with the MEAN stack (Mongo, Express, Angular, Node.js).
  - They host websites and APIs in Azure App Services.
  - MongoDB is a managed service provided by mLab on Azure.

Customers are &quot;tenants,&quot; and each tenant is a unique deployment whereby the following happens:

-
  - Each tenant has a database in the MongoDB cluster with its own collections.
  - A copy of the most recent functional conference code base is taken and configured to point at the tenant database.
    - This configuration includes a website code base and an administrative site code base for entering conference content such as speakers, sessions, workshops, and sponsors
  - Modifications to support the customer&#39;s styles, graphics, layout, and other custom requests are applied.
  - The conference owner is given access to the admin site to enter event details.
    - They will continue to use this admin site each conference, each year.
    - They can add new events and isolate speakers, sessions, workshops, and other details.
  - They deploy the tenant&#39;s code (conference and admin website) to Web Apps in an App Service Plan.
  - Once the conference site is live, the inevitable requests for changes to the website pages, styles, registration requirements, and any number of custom requests begin.

Arthur is painfully aware that this small business that evolved into something bigger has organically grown into what should be a fully multi-tenanted application suite for conferences. However, the team is having difficulty approaching this goal. They are constantly updating the code base for each tenant and doing their best to merge improvements into a core code base they can use to spin up new conferences. The pace of change is fast, the budget is tight, and they simply do not have time to stop and restructure the core code base to support all the flexibilities customers require.

Arthur is looking to take a step in this direction with the following goals in mind:

-
  - Reduce regressions introduced by changes to a single tenant.
    - One of the issues with the code base is that it has many dependencies across features. Seemingly simple changes to an area of code introduce issues with layout, responsiveness, registration functionality, content refresh, and more.
    - To avoid this, he would like to rework the core code base so he can cleanly separate registration, email notifications, and templates, content, and configuration from each other and the front end.
    - Ideally, changes to individual areas would no longer require a full regression test of the site given the number of sites they manage this is not tenable.
  - Improve the DevOps lifecycle
    - The time it takes to onboard a new tenant, launch a new site for an existing tenant, and manage all the live tenants throughout the lifecycle of the conference is highly inefficient.
    - By reducing the effort to onboard customers, manage deployed sites, and monitor health, the company can contain costs and overhead as they continue to grow. This approach may allow for time to improve the multi-tenant platform they would like to build for long-term growth.
  - Increase visibility into system operations and health.
    - The team has little to no aggregate views of health across the websites deployed.

While multi-tenancy is a goal for the code base, even with this in place, Arthur believes there will always be the need for custom copies of code for a particular tenant who requires a one-off custom implementation. Arthur feels that Docker containers may be a good solution to support their short-term DevOps and development agility needs, while also being the right direction once they reach a majority multi-tenant application solution.

#### Customer needs

1. Reduce the overhead in time, complexity and cost for deploying new conference tenants.
2. Improve the reliability of conference tenant updates.
3. Choose a suitable platform for their Docker container strategy on Azure. The platform choice should:
  1. Make it easy to deploy and manage infrastructure.
  2. Provide tooling to help them with monitoring and managing container health.
  3. Be affordable, if possible with no additional licensing.
4. Continue to use its managed MongoDB cluster for data storage.
5. Continue to use Git repositories for source control and have this integrated into a CICD workflow.
6. As there are team members who are already familiar with Chef, they are hoping to use this with the container solution.
7. Prefer a complete suite of operational management tools with:
  1. UI for manual deployment and management during development and initial POC work.
  2. APIs for integrated CICD automation.
  3. Container scheduling and orchestration.
  4. Health monitoring and alerts, visualizing status.
8. Complete an implementation of the proposed solution for a single tenant to train the team and perfect the process.

#### Customer objections

1. With so many platforms and tools for Docker and container orchestration, how should we choose an option for Azure?
2. What is the simplest way to move containers on Azure, based on our PaaS experience, while at the same time considering our scale and growth requirements?

#### Infographic for common scenarios

**Azure Container Service with Kubernetes**

![Alt Text](\././img/acs-architecture.png)

[https://azure.microsoft.com/en-us/blog/automating-azure-virtual-machine-deployment-with-chef/](https://azure.microsoft.com/en-us/blog/automating-azure-virtual-machine-deployment-with-chef/)



## Step 2: Design a proof of concept solution

**Outcome**

Prepare to present a solution to the target customer audience in a 15-minute chalk-talk format.

Timeframe: 60 minutes

**Business needs**

Directions: With all participants at your table, answer the following questions and list the answers on a flip chart.

1. Who should you present this solution to? Who is your target customer audience? Who are the decision makers?
2. What customer business needs do you need to address with your solution?

**Design**

Directions: With all participants at your table, respond to the following questions on a flip chart.

_High-level architecture_

1. Based on the customer situation, what containers would you propose as part of the new microservices architecture for a single conference tenant?
2. Without getting into the details (the following sections will address the particular details), diagram your initial vision of the container platform, the containers that should be deployed (for a single tenant), and the data tier.

_Choosing a container platform on Azure_

1. List the potential platform choices for deploying containers to Azure.
2. Which would you recommend and why?
3. Describe how the customer can provision their Azure Container Service environment to get their POC started.

_Containers, discovery, and load balancing_

1. Describe the high-level manual steps developers will follow for building images and running containers on Azure Container Service as they build their POC. Include the following components in the summary:


  - The Git repository containing their source
  - Docker image registry
  - Steps to build Docker images and push to the registry
  - Run containers using the Marathon UI

1. What options does the customer have for a Docker image registry, and what would you recommend?
2. How will the customer configure web site containers so that they are reachable publicly at port 80/443 from Azure Container Service?
3. Explain how Azure Container Service can route requests to multiple web site containers hosted on the same node at port 80/443.

_Scalability considerations_

- Explain to the customer how Azure Container Service and their preconfigured Scale Sets support cluster auto-scaling.

_Automating DevOps workflows_

- Describe how Chef can help the customer automate their continuous integration and deployment workflows and the Azure Container Service infrastructure.

**Prepare**

Directions: With all participants at your table:

1. Identify any customer needs that are not addressed with the proposed solution.
2. Identify the benefits of your solution.
3. Determine how you will respond to the customer&#39;s objections.

Prepare a 15-minute chalk-talk style presentation to the customer.

## Step 3: Present the solution

**Outcome**

Present a solution to the target customer audience in a 15-minute chalk-talk format.

**Presentation**

Timeframe: 30 minutes

**Directions**

1. Pair with another table.
2. One table is the Microsoft team and the other table is the customer.
3. The Microsoft team presents their proposed solution to the customer.
4. The customer makes one of the objections from the list of objections.
5. The Microsoft team responds to the objection.
6. The customer team gives feedback to the Microsoft team.
7. Tables switch roles and repeat Steps 2–6.

## Wrap-up

Timeframe: 15 minutes

- Tables reconvene with the larger group to hear a SME share the preferred solution for the case study.



## Additional references

| **Item** | **Description** | **Links** |
| --- | --- | --- |
| Azure Kubernetes Service (AKS) | Azure Kubernetes Service overview. | [https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-intro-kubernetes](https://docs.microsoft.com/en-us/azure/container-service/kubernetes/container-service-intro-kubernetes) |
| Azure Container Service (ACS) with Kubernetes | Azure Container Service with Kubernetes overview. |
##### [https://azure.microsoft.com/en-us/services/container-service/](https://azure.microsoft.com/en-us/services/container-service/)
 |
| Docker Enterprise Edition (Docker EE) | The enterprise grade container cluster management solution from Docker. |
##### [https://docs.docker.com/enterprise/](https://docs.docker.com/enterprise/)
 |
| DC/OS | DC/OS overview documentation |
##### [https://docs.mesosphere.com/1.9/overview/](https://docs.mesosphere.com/1.9/overview/)
 |
| Kubernetes | Documentation for Kubernetes. |
##### [https://kubernetes.io/docs/home/](https://kubernetes.io/docs/home/)
 |
| Chef Automate | An overview of Workflow in Chef Automate |
##### [https://docs.chef.io/workflow.html](https://docs.chef.io/workflow.html)
 |
| Chef Provisioning for Docker | Documentation describing support for provisioning and related plug ins including Docker support. |
##### [https://docs.chef.io/provisioning.html](https://docs.chef.io/provisioning.html)
 |