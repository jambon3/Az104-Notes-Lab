
# Azure ARM Template – Deployment History

## 1. Overview

Azure Resource Manager (ARM) templates allow administrators to deploy and manage Azure resources using Infrastructure as Code (IaC).

When an administrator deploys resources using an ARM template, Azure records deployment information that can be reviewed through the Azure Portal.

For deployments at the resource group scope, the deployment history is available in the Resource Group's **Deployments** blade.

---

## 2. Scenario

A company has an Azure subscription containing the following resources:

| Name | Resource Type |
|---|---|
| tutorialsdojoVM01 | Virtual Machine |
| tutorialsdojoStorage01 | Storage Account |
| tutorialsdojoShare01 | File Share |
| tutorialsdojoRG01 | Resource Group |
| tutorialsdojoBC01 | Blob Container |

Another Azure Administrator created several virtual machines and storage accounts using an ARM template.

The objective is to identify the template used to deploy the resources.

### Correct Answer

**tutorialsdojoRG01 – Resource Group**

Navigate to the Resource Group's **Deployments** blade to review the deployment history and associated ARM templates.

---

## 3. How to Find an ARM Template in Azure Portal

Follow these steps:

1. Sign in to the Azure Portal.
2. Navigate to **Resource Groups**.
3. Select `tutorialsdojoRG01`.
4. Under the Settings section, select **Deployments**.
5. Review the deployment history.
6. Select the deployment created by the other administrator.
7. Review the deployment details, template and parameters.

### Navigation Path

```text
Azure Portal
    |
    └── Resource Groups
            |
            └── tutorialsdojoRG01
                    |
                    └── Deployments
                            |
                            └── Select Deployment
                                    |
                                    └── Review Template
```

---

## 4. Why Are the Other Options Incorrect?

| Option | Explanation |
|---|---|
| tutorialsdojoBC01 | Blob container stores unstructured data. It does not provide resource group deployment history. |
| tutorialsdojoRG01 | Correct. Resource groups provide deployment history for resource group deployments. |
| tutorialsdojoShare01 | Azure Files share stores file data, not ARM deployment history. |
| tutorialsdojoStorage01 | Storage account manages storage services, not resource group deployment history. |

---

## 5. Azure Deployment Scopes

ARM templates support multiple deployment scopes.

| Deployment Scope | Deployment History Location |
|---|---|
| Resource Group | Resource Group → Deployments |
| Subscription | Subscription → Deployments |
| Management Group | Management Group → Deployments |
| Tenant | Tenant-level deployments |

### Important Distinction

The deployment history is associated with the scope at which the ARM template was deployed.

For example:

- A resource group deployment is recorded at the resource group scope.
- A subscription deployment is recorded at the subscription scope.
- A management group deployment is recorded at the management group scope.

Do not confuse the deployment scope with the individual resources created by the deployment.

---

## 6. Important AZ-104 Exam Notes

Remember these concepts:

- ARM templates are used for Infrastructure as Code (IaC).
- Azure Resource Manager handles resource deployment.
- Resource Group → Deployments displays resource group deployment history.
- Deployment history provides information about previous deployments.
- Deployment details can include the template, parameters and deployment status.
- Deployment history has retention limitations.
- Access depends on the user's Azure permissions.

### Exam Memory Trick

**ARM Template Deployment History = Deployments Blade**

If an exam question asks where to find an ARM template previously used to deploy resources into a resource group, look for:

`Resource Group → Deployments`

---

## 7. Practical Application

As an Azure Administrator, deployment history is useful for:

1. Troubleshooting failed deployments.
2. Reviewing resources created by other administrators.
3. Understanding infrastructure changes.
4. Auditing deployment operations.
5. Reviewing deployment parameters.
6. Investigating configuration inconsistencies.

This is particularly useful in environments where multiple administrators manage the same Azure subscription.

---

## 8. Microsoft Documentation

Official Microsoft Learn resources:

- ARM template deployment history:
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/deployment-history

- ARM template deployment scopes:
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-rest

- ARM templates overview:
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview

---

## 9. Summary

To find an ARM template previously used to deploy virtual machines and storage accounts into a resource group:

**Navigate to:**

`Azure Portal → Resource Groups → tutorialsdojoRG01 → Deployments`

The Deployments blade provides access to deployment history and associated deployment information.

**AZ-104 Key Takeaway:**

Always identify the deployment scope before looking for ARM template deployment history.
