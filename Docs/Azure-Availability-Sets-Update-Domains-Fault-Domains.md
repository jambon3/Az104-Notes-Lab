
# Azure Availability Sets – Update Domains and Fault Domains

## 1. Overview

Azure Availability Sets help improve the availability of virtual machines by distributing them across different infrastructure groups within an Azure datacenter.

They protect workloads against:

- Planned maintenance.
- Unplanned hardware failures.

Azure Availability Sets use two important concepts:

1. Update Domains (UD)
2. Fault Domains (FD)

Understanding the difference between these concepts is important for the AZ-104 certification exam.

---

## 2. Scenario

A company plans to migrate its business-critical application to Azure Virtual Machines.

The requirement is to ensure that at least two VMs remain available during planned Azure maintenance.

### Question

Which Availability Set configuration should be used?

### Important Concepts

| Configuration | Purpose |
|---|---|
| Update Domains | Protect against planned maintenance |
| Fault Domains | Protect against unplanned hardware failures |

For planned maintenance, the important configuration is the number of Update Domains.

---

## 3. Update Domains (UD)

Update Domains determine how Azure groups VMs for planned maintenance.

Azure performs maintenance on one Update Domain at a time.

VMs belonging to other Update Domains can continue running while maintenance is performed.

### Example: Three Update Domains

```text
          Azure Availability Set
                   |
        +----------+----------+
        |          |          |
       UD 0       UD 1       UD 2
        |          |          |
       VM1        VM2        VM3
```

Example:

1. Azure performs maintenance on UD 0.
2. VM1 may temporarily become unavailable.
3. VM2 and VM3 remain available.
4. Azure proceeds to another Update Domain after the maintenance process.

This helps maintain application availability during planned maintenance.

### Important

Having multiple Update Domains allows VMs to be distributed across separate maintenance groups.

The maximum number of Update Domains supported by an Availability Set depends on the configuration and Azure platform limits.

---

## 4. Fault Domains (FD)

Fault Domains protect VMs against unplanned hardware failures.

A Fault Domain represents a group of infrastructure that shares common physical components.

Examples include:

- Physical power sources.
- Network switches.
- Server racks.
- Physical infrastructure.

If one Fault Domain experiences a hardware failure, VMs in other Fault Domains can continue operating.

### Example: Two Fault Domains

```text
          Azure Availability Set
                   |
             +-----+-----+
             |           |
            FD 0        FD 1
             |           |
          +--+--+     +--+--+
          |     |     |     |
         VM1   VM2   VM3   VM4
```

The VMs are distributed across different infrastructure groups to reduce the impact of hardware failures.

---

## 5. Update Domains vs. Fault Domains

| Characteristic | Update Domains | Fault Domains |
|---|---|---|
| Purpose | Planned maintenance | Unplanned hardware failures |
| Protects against | Maintenance events | Physical infrastructure failures |
| Controls | Maintenance grouping | Physical infrastructure grouping |
| Example | Azure host updates | Server rack failure |
| Main objective | Keep other VMs running during maintenance | Reduce the impact of hardware failures |

### Memory Trick

- UD = Updates = Planned maintenance.
- FD = Faults = Hardware failures.

---

## 6. Evaluating the Exam Answers

The question provides four configurations:

| Option | Update Domains | Fault Domains | Assessment |
|---|---:|---:|---|
| 1 | 2 | 3 | Two maintenance groups |
| 2 | 3 | 1 | Three maintenance groups |
| 3 | 1 | 3 | Only one maintenance group |
| 4 | 3 | 2 | Three maintenance groups |

### Important Exam Clarification

Options 2 and 4 both have three Update Domains.

Since the question only asks about planned maintenance, both configurations provide three maintenance groups.

The number of Fault Domains does not determine planned maintenance availability.

Therefore, the question is ambiguous as written. It does not establish a unique correct answer between Options 2 and 4.

If the intention is to maximize physical fault isolation as well, Option 4 provides more Fault Domains than Option 2.

---

## 7. Availability Sets vs. Availability Zones

These are two different Azure high-availability solutions.

### Availability Sets

- Distribute VMs across Update Domains.
- Distribute VMs across Fault Domains.
- Protect against planned maintenance and localized hardware failures.
- Operate within a single Azure datacenter infrastructure boundary.

### Availability Zones

- Distribute resources across physically separate datacenters within an Azure region.
- Protect against datacenter-level failures.
- Provide greater infrastructure isolation.
- Require a region that supports Availability Zones.

### Comparison

| Feature | Availability Set | Availability Zone |
|---|---|---|
| Update Domains | Yes | Not configured by the user |
| Fault Domains | Yes | Separate physical zones |
| Protect against maintenance | Yes | Yes, through zonal resilience |
| Protect against datacenter failure | Limited | Yes |
| Infrastructure scope | Within a datacenter | Across separate datacenters |

---

## 8. AZ-104 Exam Notes

Remember these important concepts:

| Exam Requirement | Azure Feature |
|---|---|
| Planned maintenance protection | Update Domains |
| Hardware failure protection | Fault Domains |
| Datacenter-level failure protection | Availability Zones |
| Distribute VMs across maintenance groups | Availability Set |
| Improve VM availability across zones | Zonal deployment |

### Common Exam Questions

**Q1. Which Availability Set feature protects against planned maintenance?**

Answer: Update Domains.

**Q2. Which Availability Set feature protects against hardware failures?**

Answer: Fault Domains.

**Q3. Which Azure feature protects workloads against datacenter-level failures?**

Answer: Availability Zones.

**Q4. If an Availability Set has three Update Domains, how many maintenance groups are available?**

Answer: Three.

---

## 9. Microsoft Documentation

Official Microsoft Learn resources:

- Availability Sets overview:
  https://learn.microsoft.com/en-us/azure/virtual-machines/availability-set-overview

- Availability Zones:
  https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview

- Azure Virtual Machine availability:
  https://learn.microsoft.com/en-us/azure/virtual-machines/availability

---

## 10. Summary

Azure Availability Sets improve VM availability using two mechanisms:

1. Update Domains protect against planned maintenance.
2. Fault Domains protect against unplanned hardware failures.

For the AZ-104 exam, remember:

**UD = Planned maintenance**

**FD = Hardware failure**

**Availability Zones = Datacenter-level resilience**

Always identify the type of failure described in the question before selecting an Azure high-availability solution.
