# Azure Site Recovery - Hyper-V Replication to Azure

## Question

Your company created several Azure virtual machines and a file share in the subscription **TD-Boracay**. The VMs are all part of the same virtual network.

You have been assigned to manage the **on-premises Hyper-V server replication to Azure**.

To support the planned deployment, which additional resources should you create?

**Select THREE.**

### Options

- Hyper-V site
- Replication Policy
- Azure Recovery Services Vault
- Azure Storage Account
- Azure ExpressRoute
- VNet Service Endpoint

---

# Correct Answers

✅ **Hyper-V Site**  
✅ **Replication Policy**  
✅ **Azure Recovery Services Vault**

---

# Explanation

Azure Site Recovery (ASR) is used to replicate on-premises Hyper-V virtual machines to Azure for disaster recovery.

For Hyper-V replication to Azure, the following resources are required:

## 1. Azure Recovery Services Vault ✅

The Recovery Services Vault is the central management component for Azure Site Recovery.

It stores:

- Replication configuration
- Recovery plans
- Replication metadata
- Site Recovery settings

Without a Recovery Services Vault, Azure Site Recovery cannot be configured.

---

## 2. Hyper-V Site ✅

A Hyper-V Site represents one or more on-premises Hyper-V hosts managed together for replication.

The Hyper-V Site allows Azure Site Recovery to:

- Discover Hyper-V hosts
- Register Hyper-V servers
- Organize protected virtual machines

---

## 3. Replication Policy ✅

A Replication Policy defines:

- Recovery Point Objective (RPO)
- Snapshot frequency
- Recovery point retention
- App-consistent snapshot settings

The policy is associated with the Hyper-V Site and controls how replication occurs.

---

# Why the Other Answers Are Incorrect

## Azure Storage Account ❌

Historically, Azure Site Recovery used storage accounts as replication targets.

Modern deployments use **Managed Disks**, and Azure Site Recovery automatically handles the storage requirements.

A Storage Account is not one of the required components in this scenario.

---

## Azure ExpressRoute ❌

ExpressRoute provides private connectivity between on-premises environments and Azure.

While it can improve replication performance, it is **not required** to configure Azure Site Recovery.

Replication can occur over the public internet using HTTPS.

---

## VNet Service Endpoint ❌

Service Endpoints are used to secure Azure PaaS services such as:

- Azure Storage
- Azure SQL
- Azure Key Vault

They have no role in configuring Hyper-V replication with Azure Site Recovery.

---

# AZ-104 Exam Tip

For **Hyper-V → Azure Site Recovery**, remember the three core components:

1. **Recovery Services Vault**
2. **Hyper-V Site**
3. **Replication Policy**

These are the resources most commonly tested in AZ-104 and Azure disaster recovery scenarios.

**Answer: ✅ Hyper-V Site, ✅ Replication Policy, ✅ Azure Recovery Services Vault**
