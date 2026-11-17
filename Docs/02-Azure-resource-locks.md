# AZ-104 — Remaining Compute Topics to Study

## Overview

This document tracks the remaining topics in the AZ-104 **Deploy and Manage Azure Compute Resources** section.

The introductory concepts have already been covered. This guide focuses on the advanced configuration, management, deployment, and troubleshooting topics that still require study and hands-on practice.

---

# 1. Azure Virtual Machines — Advanced Management

Previously covered: VM creation, sizes, images, disks, NICs, IP addresses, availability, and basic lifecycle operations.

## 1.1 VM Extensions

- Understand what VM extensions are.
- Install and configure VM extensions.
- Understand how extensions run after VM deployment.
- Troubleshoot extension installation failures.

## 1.2 Custom Script Extension

- Run PowerShell scripts on Windows VMs.
- Run shell scripts on Linux VMs.
- Understand how scripts are downloaded and executed.
- Identify common troubleshooting scenarios.

## 1.3 VM Encryption

- Understand encryption at host.
- Understand the difference between encryption at host and Azure Disk Encryption.
- Know which VM resources and disks are protected.

## 1.4 Moving Virtual Machines

- Move VMs between resource groups.
- Move supported resources between subscriptions.
- Understand resource move limitations and dependencies.
- Understand moving VMs between Azure regions.

## 1.5 VM Images and Capture

- Understand how to capture a VM image.
- Understand managed images and Azure Compute Gallery.
- Understand how to create additional VMs from an image.
- Know the difference between a VM image and a snapshot.

## 1.6 Managed Disk Management

- Change disk size and performance settings.
- Understand disk types and performance tiers.
- Attach and detach managed disks.
- Understand disk snapshots and disk encryption.

---

# 2. VM Scale Sets — Advanced Configuration

Previously covered: The purpose of VM Scale Sets and the basics of autoscaling.

Remaining topics:

- Create and configure VM Scale Sets.
- Configure autoscaling rules.
- Configure minimum, maximum, and default instance counts.
- Understand scaling based on metrics.
- Manage individual VM instances.
- Understand VMSS upgrade policies.
- Understand how load balancing integrates with VM Scale Sets.

---

# 3. Azure App Service — Configuration and Management

Previously covered: App Service Plans, Web Apps, deployment slots, pricing tiers, and basic scaling.

Remaining topics:

- Configure custom DNS names.
- Configure TLS/SSL certificates.
- Configure App Service backups and restoration.
- Configure networking settings.
- Understand VNet integration.
- Understand access restrictions.
- Understand private endpoints for App Service.
- Configure application settings and connection strings.
- Understand deployment slot settings and slot swaps.
- Configure scaling rules and autoscaling.

---

# 4. Azure Containers — Additional Services

Previously covered: Container fundamentals and an introduction to Azure Container Instances (ACI).

The following services and concepts still require study.

## 4.1 Azure Container Registry (ACR)

- Understand what a container registry is.
- Create and configure an Azure Container Registry.
- Store and manage container images.
- Understand image repositories and tags.
- Understand authentication and access to ACR.
- Deploy containers using images stored in ACR.

## 4.2 Azure Kubernetes Service (AKS)

- Understand Kubernetes fundamentals.
- Understand clusters, nodes, and pods.
- Understand deployments and services.
- Understand how AKS manages containerized applications.
- Understand scaling in AKS.
- Understand the relationship between AKS and ACR.

## 4.3 Azure Container Apps

- Understand what Azure Container Apps is.
- Understand environments and container apps.
- Configure container sizing.
- Configure scaling rules.
- Understand revisions and replicas.
- Understand event-driven scaling.
- Compare Azure Container Apps with ACI and AKS.

## 4.4 Container Service Comparison

| Service | Main Purpose |
|---|---|
| Azure Container Instances (ACI) | Run containers without managing VMs |
| Azure Container Registry (ACR) | Store and manage container images |
| Azure Kubernetes Service (AKS) | Orchestrate containers using Kubernetes |
| Azure Container Apps | Run and scale containerized applications without managing Kubernetes infrastructure |

---

# 5. Azure Functions — Serverless Computing

**Status:** Not covered yet.

## 5.1 Fundamentals

- Understand serverless computing.
- Understand how Azure Functions works.
- Understand Function Apps.
- Understand triggers and bindings.

## 5.2 Function Triggers

- HTTP triggers.
- Timer triggers.
- Queue triggers.
- Blob Storage triggers.
- Event-driven execution.

## 5.3 Management

- Create and configure Function Apps.
- Understand hosting plans.
- Configure application settings.
- Understand scaling.
- Monitor and troubleshoot function execution.

### Example: Blob Storage Trigger

A user uploads a file to Azure Blob Storage.

An Azure Function automatically executes code when the upload event occurs. The function can then process the file without requiring an administrator to manually start a server or script.

```text
User uploads file
       |
       v
Azure Blob Storage
       |
       v
Blob Storage Trigger
       |
       v
Azure Function executes
       |
       v
File processing
```

---

# 6. Infrastructure as Code — ARM Templates and Bicep

Previously covered: The purpose of ARM templates and Bicep, including a basic Bicep example.

The following topics still require study.

## 6.1 ARM Templates

- Understand ARM template JSON structure.
- Understand parameters, variables, and resources.
- Understand outputs and template functions.
- Modify existing ARM templates.
- Deploy resources using ARM templates.
- Export existing deployments as ARM templates.
- Troubleshoot deployment failures.

## 6.2 Bicep

- Understand Bicep syntax.
- Define parameters and variables.
- Define Azure resources.
- Understand resource dependencies.
- Modify existing Bicep templates.
- Deploy resources using Bicep.
- Understand how Bicep compiles into ARM JSON.
- Convert ARM templates to Bicep.

## 6.3 Automation Tools

- Deploy resources using Azure PowerShell.
- Deploy resources using Azure CLI.
- Understand when to use the Azure Portal, PowerShell, CLI, or Infrastructure as Code.

---

# 7. Recommended Study Order

To build knowledge progressively, follow this order:

1. **VM Extensions and Custom Script Extension** — Learn how to configure and automate VM management.
2. **VM Encryption and Disk Management** — Understand security and storage configuration.
3. **Moving VMs and VM Images** — Learn resource migration and image deployment.
4. **VM Scale Sets** — Study advanced autoscaling and instance management.
5. **App Service Advanced Configuration** — Focus on DNS, TLS, networking, backups, and deployment slots.
6. **Azure Container Registry** — Understand image storage and management.
7. **Azure Kubernetes Service** — Learn Kubernetes fundamentals and orchestration.
8. **Azure Container Apps** — Understand managed container hosting and scaling.
9. **Azure Functions** — Study serverless computing and event-driven execution.
10. **ARM Templates and Bicep** — Learn infrastructure automation and deployment.
11. **Azure PowerShell and CLI** — Practice deploying and managing resources through commands.

---

# 8. Final Study Objective

By completing these topics, you should be able to:

- Configure and troubleshoot Azure virtual machines.
- Automate VM configuration using extensions and scripts.
- Manage VM storage, encryption, images, and migration.
- Configure and manage VM Scale Sets.
- Configure App Service networking, security, and deployment.
- Understand the differences between ACI, ACR, AKS, and Azure Container Apps.
- Deploy and manage serverless applications using Azure Functions.
- Automate Azure infrastructure deployments using ARM templates, Bicep, PowerShell, and Azure CLI.

**Important:** Understanding a concept is only the first step. For AZ-104 preparation, practice deploying resources, changing configurations, troubleshooting failures, and answering scenario-based questions through the Azure Portal and command-line tools.

This document is a personal study tracker. Always cross-check the current Microsoft AZ-104 skills measured document for the official exam objectives.
