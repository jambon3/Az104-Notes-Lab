# Azure Availability Sets - Update Domains vs Fault Domains

## Exam Question

You plan to migrate your business-critical application to Azure virtual machines.

You need to make sure that **at least two VMs are available during planned Azure maintenance**.

### Correct Answer

✅ Create an Availability Set with:

- `platformUpdateDomainCount = 3`
- `platformFaultDomainCount = 1`

---

# Key Concepts

## Update Domains (UD)

Update Domains protect against **planned Azure maintenance**.

When Microsoft performs maintenance:

- Azure updates one Update Domain at a time.
- Only the VMs in that Update Domain are rebooted.
- Other Update Domains continue running.

Think:

> Update Domain = Planned Maintenance

---

## Fault Domains (FD)

Fault Domains protect against **hardware failures**.

Examples:

- Rack failure
- Power failure
- Network switch failure
- Physical host failure

Think:

> Fault Domain = Hardware Failure

---

# Why 3 Update Domains?

Imagine we have 3 VMs.

| VM | Update Domain |
|------|------|
| VM1 | UD1 |
| VM2 | UD2 |
| VM3 | UD3 |

### During Maintenance

Azure patches UD1:

```text
VM1 = DOWN
VM2 = UP
VM3 = UP
```

✅ Two VMs still available

---

Azure patches UD2:

```text
VM1 = UP
VM2 = DOWN
VM3 = UP
```

✅ Two VMs still available

---

Azure patches UD3:

```text
VM1 = UP
VM2 = UP
VM3 = DOWN
```

✅ Two VMs still available

---

# Why Not 2 Update Domains?

Suppose:

| VM | Update Domain |
|------|------|
| VM1 | UD1 |
| VM2 | UD2 |

When Azure patches UD1:

```text
VM1 = DOWN
VM2 = UP
```

Only ONE VM remains available.

❌ Requirement not met.

The question requires:

> At least TWO VMs available.

Therefore:

✅ Need 3 Update Domains.

---

# Why Fault Domains Don't Matter Here

The question says:

> Planned Azure maintenance

It does NOT say:

- Hardware failure
- Rack failure
- Physical host failure
- Power failure

Therefore the important setting is:

✅ Update Domains

Not:

❌ Fault Domains

---

# AZ-104 Memorization Trick

| Question Mentions | Think |
|------------------|--------|
| Planned Maintenance | Update Domains |
| Azure Patching | Update Domains |
| Reboot During Maintenance | Update Domains |
| Rack Failure | Fault Domains |
| Power Failure | Fault Domains |
| Hardware Failure | Fault Domains |

---

# Quick Exam Rule

### Planned Maintenance

```text
Update Domains
```

### Hardware Failure

```text
Fault Domains
```

### This Question

```text
Need at least 2 VMs available
during planned maintenance
```

Result:

```text
3 Update Domains
1 Fault Domain
```

✅ Correct Answer: Option 1
