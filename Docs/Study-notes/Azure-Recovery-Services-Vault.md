# Azure Recovery Services Vault – AZ-104 Study Notes

## 1. What is a Recovery Services Vault?

A **Recovery Services vault** is an Azure resource used to manage and organize backup and disaster recovery operations.

It provides a centralized location for managing:

- Azure VM backups.
- Azure Site Recovery (ASR).
- Replication of virtual machines.
- Disaster recovery plans.
- Recovery and failover operations.

Think of the Recovery Services vault as the management container for Azure Backup and Azure Site Recovery.

## 2. Azure Backup vs. Azure Site Recovery

These two services have different purposes.

| Feature | Azure Backup | Azure Site Recovery (ASR) |
|---|---|---|
| Main purpose | Data protection | Disaster recovery |
| Protects against | Accidental deletion, corruption, data loss | Datacenter outages and regional failures |
| VM recovery | Restore a VM or its data | Fail over to another location |
| Replication | Not its primary purpose | Continuous replication |
| Recovery objective | Restore backed-up data | Restore application availability |

### Azure Backup

Azure Backup protects your data by creating recovery points.

Examples:
- Recover a deleted file.
- Restore a virtual machine.
- Recover data after accidental modification.
- Restore a database from a backup.

### Azure Site Recovery (ASR)

ASR provides disaster recovery by replicating workloads to another location.

Examples:
- Your primary Azure region becomes unavailable.
- Your on-premises datacenter experiences an outage.
- Your business-critical application needs to run from another region.

ASR allows you to fail over workloads to a secondary location.

## 3. Typical Azure Site Recovery Configuration

For Azure VM disaster recovery, the general configuration process is:

1. **Create a Recovery Services vault**
   - Select the appropriate Azure region.
   - Create the vault.

2. **Configure Site Recovery**
   - Select the source and target environment.
   - Configure the replication settings.

3. **Enable VM replication**
   - Select the virtual machines to protect.
   - Configure the target region, storage and network settings.

4. **Create a Recovery Plan**
   - Organize virtual machines into recovery groups.
   - Define the order in which VMs should start.
   - Configure dependencies between application components.

5. **Test Failover**
   - Test recovery without disrupting production workloads.

6. **Perform Failover when necessary**
   - Move workloads to the recovery environment during an actual disaster.

7. **Reprotect and Failback**
   - Re-establish replication and return workloads to the primary location when appropriate.

## 4. Important AZ-104 Exam Concepts

| Exam scenario | Correct concept |
|---|---|
| Back up Azure virtual machines | Azure Backup |
| Create backup schedules | Backup Policy |
| Manage backup operations | Recovery Services vault |
| Replicate VMs to another region | Azure Site Recovery |
| Configure disaster recovery | Recovery Services vault |
| Recover from a regional outage | ASR Failover |
| Test disaster recovery | Test Failover |
| Define VM startup order | Recovery Plan |

## 5. Recovery Services Vault vs. Backup Policy

These are two different Azure resources/configurations.

**Recovery Services vault**
- Provides centralized management.
- Stores backup recovery points.
- Manages protected workloads.
- Supports Azure Site Recovery configuration.

**Backup Policy**
- Defines when backups occur.
- Defines backup frequency.
- Defines retention periods.
- Controls backup scheduling.

A backup policy is associated with a backup solution; it is not a replacement for a Recovery Services vault.

## 6. AZ-104 Memory Tricks

Remember these associations:

- **Backup = Restore data**
- **ASR = Disaster recovery**
- **Recovery Services vault = Management foundation**
- **Backup Policy = Schedule and retention**
- **Recovery Plan = VM startup order**
- **Failover = Switch to recovery environment**
- **Failback = Return to primary environment**

### Example Exam Question

Your company operates business-critical virtual machines in Azure. You need to configure disaster recovery to another Azure region.

What should you configure first?

A. Backup Policy  
B. Recovery Services vault  
C. Microsoft Azure Backup Server (MABS)  
D. Azure Site Recovery (ASR)

**Correct answer: B. Recovery Services vault**

Explanation: The Recovery Services vault is the standard starting point for configuring Azure Site Recovery for Azure virtual machines. ASR replication and recovery settings are configured through the vault.

---

## 7. Practical Recommendation

For production environments, remember that backup and disaster recovery complement each other.

A robust strategy generally includes:

- Azure Backup for data protection.
- Azure Site Recovery for workload availability.
- Regular recovery testing.
- Documented Recovery Plans.
- Defined Recovery Point Objective (RPO) and Recovery Time Objective (RTO).

**RPO:** How much data loss (measured in time) the business can tolerate.

**RTO:** How long the business can tolerate an application being unavailable.

These two metrics help determine the appropriate backup frequency and disaster recovery architecture.
