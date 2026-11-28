# AZ-104 – Identify Unattached Disks to Reduce Costs

## Scenario

Your company has an Azure subscription with multiple virtual machines, some of which were deployed using `az vm` commands.

You are responsible for managing all compute resources in the subscription.

You need to identify unattached disks that can be safely deleted to reduce costs.

**Question:** Which option should you use?

## Correct Answer

**Option 1 – Use Azure Cost Management to view Azure Advisor recommendations.**

## Explanation

Azure Advisor analyzes Azure resources and provides recommendations to optimize:

- Cost
- Performance
- Security
- Reliability
- Operational excellence

For this scenario, Azure Advisor can identify **unattached managed disks** that are no longer associated with virtual machines.

These disks can continue generating storage costs even when they are not attached to a VM.

By reviewing Azure Advisor's cost recommendations, you can identify potentially unused disks and delete them to reduce Azure costs.

---

## Why the Other Answers Are Wrong

### Option 2 – Use Azure Cost Management cost analysis to download resource data

**Incorrect.**

- Cost Analysis shows Azure spending and resource consumption.
- You can identify which resources are costing money.
- However, it does not directly identify unattached disks as a specific optimization recommendation.

### Option 3 – Use Azure Monitor VM Insights

**Incorrect.**

- VM Insights monitors virtual machine performance.
- It collects information about CPU, memory, disk activity, and network performance.
- It is designed for monitoring VM health, not identifying unattached disks.

### Option 4 – Use Azure Advisor to identify low-usage virtual machines

**Incorrect.**

- Azure Advisor identifies underutilized virtual machines.
- It recommends resizing or shutting down VMs to reduce costs.
- It does not address the specific requirement of identifying unattached disks.

---

## AZ-104 Exam Notes

### Azure Services Comparison

| Azure service | Main purpose |
|---|---|
| Azure Advisor | Cost, security, reliability and performance recommendations |
| Cost Management – Cost Analysis | Analyze and track Azure spending |
| Azure Monitor | Monitor resource health and performance |
| VM Insights | Monitor VM performance and dependencies |

### Important Distinction

- **Unattached disk:** Disk not connected to a VM.
- **Underutilized VM:** VM running but using fewer resources than necessary.
- Both can generate unnecessary costs, but they require different recommendations.

### Key Exam Takeaway

When the question asks you to identify unattached disks to reduce costs, think:

**Azure Advisor → Cost recommendations**

**Important:** An unattached disk is not necessarily an unused disk. Always verify that its data is no longer needed before deleting it.

## Microsoft Documentation

- [Azure Advisor – Cost recommendations](https://learn.microsoft.com/en-us/azure/advisor/advisor-cost-recommendations)
- [Find and delete unattached Azure managed disks](https://learn.microsoft.com/en-us/azure/virtual-machines/disks-find-unattached-portal)
