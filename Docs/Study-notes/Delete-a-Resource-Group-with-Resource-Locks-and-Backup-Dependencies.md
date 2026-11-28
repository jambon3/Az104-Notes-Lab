# AZ-104 – Delete a Resource Group with Resource Locks and Backup Dependencies

## Scenario

Your Azure subscription contains a resource group named `TD-Cebu`.

The resource group contains four resources:

| Resource | Type | Configuration |
|---|---|---|
| TD-VNET | Virtual Network | Delete lock |
| TD-VM | Virtual Machine | Configured to back up to RSV daily |
| TD-SA | Storage Account | Read-only lock |
| TD-RSV | Recovery Services Vault | Contains all backups of TD-VM |

**Objective:** Delete the `TD-Cebu` resource group and all its resources.

## Correct Answer

**Option 4 – Delete all the resource locks and backup data in TD-RSV.**

## Explanation

There are two types of blockers preventing the resource group from being deleted.

### 1. Resource locks

Azure resource locks prevent accidental modification or deletion.

Two resources have locks:

- `TD-VNET`: Has a Delete lock.
- `TD-SA`: Has a Read-only lock.

Both locks must be removed before deleting the resource group.

| Lock type | Effect |
|---|---|
| CanNotDelete (Delete) | Allows modifications but prevents deletion |
| ReadOnly | Prevents modifications and deletion |

### 2. Recovery Services Vault (RSV)

`TD-RSV` contains the backup data for `TD-VM`.

You cannot delete a Recovery Services Vault while it still contains protected backup items or backup data.

Required actions:

1. Stop backup protection for `TD-VM`.
2. Delete the existing backup data from `TD-RSV`.
3. Remove the resource locks from `TD-VNET` and `TD-SA`.
4. Delete the `TD-Cebu` resource group.

**Important:** Stopping backup protection does not automatically delete existing backup data.

---

## Why the Other Answers Are Wrong

### Option 1 – Stop TD-VM and delete the resource lock of TD-VNET

**Incorrect.**

- Stopping `TD-VM` does not remove its existing backup data from `TD-RSV`.
- Removing the Delete lock from `TD-VNET` resolves only one blocker.
- `TD-SA` still has a Read-only lock.

There are still multiple blockers preventing resource group deletion.

### Option 2 – Change the resource lock type of TD-VNET and modify the backup configuration of TD-VM

**Incorrect.**

- Changing the lock type does not necessarily remove the lock.
- `TD-VNET` would still have a resource lock.
- Modifying the backup configuration does not automatically delete existing backup data.
- `TD-SA` still has its Read-only lock.

### Option 3 – Set the resource lock of TD-SA to Delete

**Incorrect.**

- `TD-SA` already has a Read-only lock.
- Changing it to Delete would still prevent deletion of the storage account.
- The Delete lock on `TD-VNET` remains.
- Backup data still exists in `TD-RSV`.

This option does not resolve the deletion blockers.

### Option 4 – Delete all the resource locks and backup data in TD-RSV

**Correct.**

This addresses both categories of blockers:

- Remove the Delete lock from `TD-VNET`.
- Remove the Read-only lock from `TD-SA`.
- Remove the backup data from `TD-RSV`.

The resource group can then be deleted once all dependencies and deletion requirements are satisfied.

---

## AZ-104 Exam Notes

### Resource locks

| Lock | Can modify? | Can delete? |
|---|---|---|
| CanNotDelete | Yes | No |
| ReadOnly | No | No |

### Recovery Services Vault

Remember these points:

- Stopping a backup job is not the same as deleting backup data.
- Existing recovery points can prevent vault deletion.
- Remove protected items and backup data before deleting the vault.
- Soft delete and other backup protection features may introduce additional deletion steps.

### Key Exam Takeaway

**Before deleting an Azure resource group, check:**

1. Are there resource locks?
2. Are there Recovery Services Vault backup dependencies?
3. Are there other protected or dependent resources?

**Correct answer: Option 4.**
