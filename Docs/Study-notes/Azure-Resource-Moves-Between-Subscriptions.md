
# Azure Resource Moves Between Subscriptions

## 1. Overview

Azure allows certain resources to be moved between resource groups and subscriptions.

However, not all Azure resources support cross-subscription moves.

When planning a resource migration, administrators must understand:
- Which resources support moving.
- Resource dependencies.
- Subscription limitations.
- Backup and recovery restrictions.
- Destination subscription requirements.

---

## 2. Scenario

A company has an Azure subscription named `TD-Sub1`.

The subscription contains the following resources:

| Resource Name | Resource Type |
|---|---|
| tutorialsdojovnet | Virtual Network |
| tutorialsdojovm | Virtual Machine |
| tutorialsdojovault | Recovery Services Vault |
| tutorialsdojostorage | Storage Account |

The company creates a second subscription named `TD-Sub2`.

The objective is to move the resources from `TD-Sub1` to `TD-Sub2`.

### Correct Answer

**Virtual Machine, Virtual Network, and Storage Account.**

The Recovery Services vault cannot be moved using the standard cross-subscription Azure Resource Manager move operation.

---

## 3. Resource Move Compatibility

| Azure Resource | Cross-Subscription Move |
|---|---|
| Virtual Machine | Supported, subject to dependencies |
| Virtual Network | Supported, subject to dependencies |
| Recovery Services Vault | Not supported through standard resource move |
| Storage Account | Supported, subject to limitations |

### Important

Resource move support can depend on the resource configuration, region, subscription type, and dependencies.

Always verify the current Microsoft documentation before performing a production migration.

---

## 4. Why Can't You Move a Recovery Services Vault?

A Recovery Services vault manages backup and disaster recovery operations.

It can contain:

- Azure Virtual Machine backups.
- Backup policies.
- Recovery points.
- Replication information.
- Protected workloads.

Moving a Recovery Services vault between subscriptions is not supported through the standard ARM resource move operation.

### Alternative Approach

When migrating backup workloads:

1. Review existing backup policies.
2. Identify protected workloads.
3. Review backup retention requirements.
4. Plan the destination Recovery Services vault.
5. Configure backup protection in the destination environment.
6. Verify that recovery requirements are maintained.

Important: Existing backup data and recovery points may not automatically transfer to the new vault.

---

## 5. Resource Dependencies

Moving a virtual machine may require moving associated resources.

Examples include:

- Managed disks.
- Network interfaces (NICs).
- Public IP addresses.
- Network security groups (NSGs).
- Virtual networks.
- Other dependent resources.

Azure may require related resources to be moved together.

Always validate the move operation before executing it.

---

## 6. Azure Resource Move Types

Azure supports different resource move operations.

### A. Resource Group Move

Move resources from one resource group to another within the same subscription.

Example:

`TD-Sub1/RG-Production`

to:

`TD-Sub1/RG-Development`

### B. Subscription Move

Move supported resources from one Azure subscription to another.

Example:

`TD-Sub1/RG-Production`

to:

`TD-Sub2/RG-Production`

### Important Considerations

Before moving resources:

- Verify destination subscription permissions.
- Check resource dependencies.
- Verify resource provider support.
- Review regional restrictions.
- Check whether the resource supports cross-subscription moves.
- Review service-specific limitations.

---

## 7. AZ-104 Exam Notes

| Exam Scenario | Key Concept |
|---|---|
| Move a VM to another subscription | Generally supported, subject to dependencies |
| Move a VNet to another subscription | Supported, subject to limitations |
| Move a Storage Account | Generally supported, subject to limitations |
| Move a Recovery Services vault | Not supported through standard cross-subscription move |
| Move resources between resource groups | Supported for eligible resources |
| Move resources between subscriptions | Supported for eligible resources |

### Exam Memory Trick

Remember:

- Virtual Machines: Generally movable.
- Virtual Networks: Generally movable.
- Storage Accounts: Generally movable.
- Recovery Services Vaults: Cannot use standard cross-subscription move.

---

## 8. Microsoft Documentation

Official Microsoft Learn resources:

- Azure Resource Manager resource move operations:
  https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-resource-group-and-subscription

- Recovery Services vault overview:
  https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview

---

## 9. Summary

Azure resource migration requires checking resource compatibility and dependencies.

In this scenario, the supported resources are:

1. Virtual Machine.
2. Virtual Network.
3. Storage Account.

The Recovery Services vault is excluded because it cannot be transferred using the standard cross-subscription ARM move operation.

**AZ-104 Key Takeaway:**

Always verify resource move support and dependencies before migrating Azure resources between subscriptions.
