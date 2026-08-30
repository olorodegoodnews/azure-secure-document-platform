# azure-secure-document-platform

## Project Overview

The **Secure Azure Document Platform** is a two-week team project designed to give beginners practical, hands-on experience with Microsoft Azure.

The team will build a simple document platform that allows users to securely access, upload, view, and manage documents stored in Azure.

The project brings together **Azure Storage, Microsoft Entra ID, Azure RBAC, Managed Identity, Docker, Azure Container Apps, networking, and monitoring** into one connected environment.

The main goal is to understand how these Azure services work together to create a secure and monitored cloud application.

## Project Goals

By the end of this project, the team will be able to:

* Create and manage Azure Blob Storage
* Configure storage security and data protection
* Use Microsoft Entra ID for identity and authentication
* Create users and groups
* Configure Azure RBAC and least-privilege access
* Use Managed Identity for application access
* Build a simple application
* Containerize the application using Docker
* Store container images in Azure Container Registry
* Deploy the application using Azure Container Apps
* Create and configure an Azure Virtual Network
* Create and manage subnets
* Configure Private Endpoint and Private DNS
* Secure communication between Azure resources
* Use Azure Monitor and Log Analytics
* Test and troubleshoot the environment
* Document the project using GitHub

## Azure Services

The project will use the following Azure services:

* Azure Blob Storage
* Microsoft Entra ID
* Azure RBAC
* Managed Identity
* Azure Container Registry
* Azure Container Apps
* Azure Virtual Network
* Subnets
* Private Endpoint
* Private DNS
* Azure Monitor
* Log Analytics

## Team Roles

| Team Member | Role                             | Main Responsibility                                         |
| ----------- | -------------------------------- | ----------------------------------------------------------- |
| Person 1    | Storage Engineer                 | Blob Storage, data protection and storage security          |
| Person 2    | Identity Engineer                | Microsoft Entra ID, users, groups and authentication        |
| Person 3    | IAM & Security Engineer          | RBAC, Managed Identity and access control                   |
| Person 4    | Application & Container Engineer | Application, Docker, ACR and Container Apps                 |
| Person 5    | Networking & Monitoring Engineer | VNet, subnets, Private Endpoint, Private DNS and monitoring |

## How the Project Works

The final environment will connect the different Azure services together.

```text
                         USER
                           |
                           v
                  Microsoft Entra ID
                     Authentication
                           |
                           v
                Azure Container Apps
                    Simple Application
                           |
                           v
                  Managed Identity
                           |
                           v
                     Azure RBAC
                           |
                           v
                     Azure Network
                           |
                           v
                   Private Endpoint
                           |
                           v
                    Azure Blob Storage
                           |
              +------------+------------+
              |            |            |
              v            v            v
          Incoming     Processed      Archive
          Documents    Documents     Documents

                 Azure Monitor
                       |
                       v
              Logs and Monitoring
```

## GitHub Team Workflow

GitHub will be used to manage the project, tasks, documentation, and collaboration.

Each team member will:

* Receive assigned GitHub Issues
* Create a branch for their work
* Complete their assigned Azure task
* Document what they did and learned
* Commit their changes
* Create a Pull Request
* Review another team member's work
* Merge completed work into the main branch

The team will work on **one shared project**, while each person will have their own responsibilities and individual learning documentation.

## Documentation

The repository will contain both team documentation and individual documentation.

### Team Documentation

The `docs` folder will contain the final technical documentation for the project.

### Individual Documentation

The `team` folder will contain each person's learning record.

Each team member will document:

* Tasks completed
* Azure resources created
* What they learned
* Problems encountered
* How problems were solved
* Testing performed
* Important screenshots and evidence

## Final Project Outcome

At the end of the two weeks, the team will have built and tested a simple secure document platform running on Azure.

The final environment will demonstrate:

**Identity → Authentication → Application → Managed Identity → RBAC → Networking → Private Endpoint → Blob Storage → Monitoring**

The project will give each team member practical experience while also allowing the entire team to understand how the complete Azure environment works together.
