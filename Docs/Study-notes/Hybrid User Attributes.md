# AZ-104 — Microsoft Entra ID: Hybrid User Attributes

## 1. Overview

In a hybrid Microsoft Entra ID environment, user identities are managed across two directories:

- **Windows Server Active Directory (On-premises AD)**
- **Microsoft Entra ID (Cloud)**

When Microsoft Entra Connect synchronizes users from on-premises AD to Entra ID, certain user attributes are controlled by the on-premises directory.

This is called **Source of Authority (SoA)**.

Understanding which directory controls an attribute is important when modifying user properties.

---

## 2. Understanding Source of Authority

In a typical hybrid identity environment:

- On-premises AD is authoritative for synchronized attributes.
- Microsoft Entra ID receives the synchronized values.
- Cloud-managed attributes can be modified directly in Entra ID.
- Changing an on-premises-authoritative attribute in Entra ID does not write the change back to AD.

### Synchronization direction

```text
Windows Server Active Directory
              |
              | Microsoft Entra Connect
              | Synchronization
              v
       Microsoft Entra ID
```

**Important:** Standard directory synchronization is not general-purpose two-way synchronization.

---

## 3. Department vs. UsageLocation

These two attributes behave differently in a hybrid environment.

### Department

- Represents the department to which a user belongs.
- Generally synchronized from on-premises Active Directory.
- On-premises AD is authoritative for synchronized users.
- Modify the attribute in Windows Server AD.

Example:

```text
On-premises AD:
Department = IT

        |
        | Synchronization
        v

Microsoft Entra ID:
Department = IT
```

If you change Department directly in Entra ID, the change will not synchronize back to on-premises AD.

The next synchronization can overwrite the cloud-side value with the authoritative on-premises value.

### UsageLocation

- Represents the geographical location associated with a user for Microsoft 365 licensing.
- Managed directly in Microsoft Entra ID.
- Can be modified in Entra ID even when the user is synchronized from on-premises AD.

Example:

```text
Microsoft Entra ID:
UsageLocation = CA
```

This attribute is cloud-managed and does not require modification in on-premises AD.

---

## 4. AZ-104 Exam Comparison

| Attribute | Cloud-only user | Synchronized user |
|---|---|---|
| Department | Modify in Entra ID | Modify in on-premises AD |
| UsageLocation | Modify in Entra ID | Modify in Entra ID |

**Remember:**

- Department = Generally on-premises authoritative for synchronized users.
- UsageLocation = Cloud-managed attribute.

---

## 5. Example Exam Question

Your company plans to implement a hybrid Microsoft Entra ID environment that includes the following users.

You have been assigned to modify the Department and UsageLocation attributes.

Which attributes can you modify from Microsoft Entra ID?

| User | Identity type | Identity source |
|---|---|---|
| Dev1 | Guest | Microsoft Account |
| Dev2 | Member | Microsoft Entra ID |
| Dev3 | Member | Windows Server AD |
| Dev4 | Guest | Windows Server AD |

### Expected answers

| User | Department | UsageLocation |
|---|---|---|
| Dev1 | Yes | Yes |
| Dev2 | Yes | Yes |
| Dev3 | No | Yes |
| Dev4 | No | Yes |

### Explanation

- **Dev1:** Guest object managed in your Entra tenant. Its Department property can be managed in the tenant.
- **Dev2:** Cloud-only member. Both attributes can be modified in Entra ID.
- **Dev3:** Synchronized member. Department is controlled by on-premises AD, while UsageLocation is cloud-managed.
- **Dev4:** Synchronized guest object. Department is controlled by its on-premises source, while UsageLocation is cloud-managed.

---

## 6. Important Concepts for AZ-104

### Source of Authority (SoA)

Source of Authority determines which directory controls an object's attributes.

### Microsoft Entra Connect

Synchronizes identities and supported attributes from on-premises AD to Microsoft Entra ID.

### Cloud-managed attributes

Some attributes can be managed directly in Entra ID even when the user is synchronized from on-premises AD.

### Attribute writeback

Do not assume that modifying an attribute in Entra ID automatically updates on-premises AD. Writeback is a separate capability and depends on the attribute and configuration.

---

## 7. Quick Knowledge Check

**Question 1**

A user is synchronized from Windows Server AD to Microsoft Entra ID.

Where should you modify the Department attribute?

- A. Microsoft Entra ID
- B. Windows Server Active Directory
- C. Microsoft 365 Admin Center
- D. Azure Resource Manager

**Answer: B — Windows Server Active Directory**

**Question 2**

A synchronized user needs a different UsageLocation value.

Where should you modify it?

- A. Windows Server AD
- B. Domain Controller Group Policy
- C. Microsoft Entra ID
- D. Microsoft Entra Connect server

**Answer: C — Microsoft Entra ID**

**Question 3**

You modify the Department attribute of a synchronized user directly in Entra ID. What happens?

- A. The change automatically synchronizes to AD.
- B. The change writes back to AD.
- C. The on-premises value remains authoritative and can overwrite the cloud-side change.
- D. Entra Connect disables synchronization.

**Answer: C**

---

## 8. Final Exam Takeaway

When answering hybrid identity questions, always ask:

1. Where was the user created?
2. Is the user synchronized from on-premises AD?
3. Which directory is authoritative for the attribute?
4. Is the attribute cloud-managed or on-premises-managed?

**Golden rule:**

> On-premises-authoritative attributes must generally be modified in on-premises AD. Cloud-managed attributes can be modified directly in Microsoft Entra ID.

Official Microsoft documentation:

- [Microsoft Entra Connect Sync](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/whatis-entra-connect)
- [Microsoft Entra Connect: Sync attributes](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-sync-attributes-synchronized)
- [Microsoft Graph: Update user](https://learn.microsoft.com/en-us/graph/api/user-update)

**Certification:** AZ-104 — Manage Microsoft Entra identities and hybrid identity.
