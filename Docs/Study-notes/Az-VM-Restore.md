# Azure VM Restore Question

## Scenario

An Azure virtual machine named **TD-BGC** was backed up using an **Azure Backup Recovery Services vault**.

After the backup, the following changes were made:

1. Changed the local administrator password.
2. Created and attached a new disk.
3. Resized the virtual machine.
4. Re-attached an existing data disk.

Later, an administrator restored **TD-BGC** from a previous recovery point using **Restore Configuration (Replace Existing)**.

---

## Correct Answer

✅ **Create and attach a new disk.**

---

## Explanation

When a VM is restored using **Restore Configuration (Replace Existing)**, Azure restores the virtual machine to the exact state captured in the selected recovery point.

Any changes made **after the backup was taken** are not included in the restored configuration.

### Review of Each Change

| Change | Result After Restore | Action Required |
|----------|----------|----------|
| Change local admin password | Reverted to the password stored in the backup | Change the password again if needed |
| Create and attach a new disk | The disk did not exist when the backup was taken and is not restored | **Recreate and reattach the disk** ✅ |
| Resize the VM | VM returns to the size captured in the backup | Resize the VM again |
| Re-attach existing data disk | Restored as part of the backed-up VM configuration | No action required |

---

## Why "Create and Attach a New Disk" Is Correct

The newly created disk was added **after** the backup was taken. Since it was not part of the recovery point, Azure Backup cannot restore it.

Therefore, to bring back that change, you must:

1. Create a new managed disk.
2. Attach it to the restored VM.

---

## AZ-104 Exam Tip

**Azure Backup restores a VM to the state recorded in the selected recovery point.**

Anything created after the backup, such as:

- New disks
- Additional NICs
- Configuration changes
- VM size changes

must be recreated or reconfigured after the restore.

**Answer: ✅ Create and attach a new disk**
