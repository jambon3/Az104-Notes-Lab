# AZ-104 — Conditional Access: Grant Controls vs. Session Controls

## 1. Scenario

Your organization has an Azure AD subscription associated with the directory `TD-Siargao`.

You must implement a Conditional Access policy with the following requirements:

- Members of the **DevOps** group must use Multi-Factor Authentication (MFA).
- Users must connect from a **Microsoft Entra hybrid joined device**.
- These requirements apply when users connect from **untrusted locations**.

### Proposed Solution

Create a Conditional Access policy and enforce Session Controls.

**Question:** Does the solution meet the goal?

### Correct Answer: NO

The proposed solution does not meet the requirements because Session Controls do not enforce the required authentication and device conditions.

You must configure **Grant Controls**.

---

## 2. Understanding Conditional Access Controls

Microsoft Entra Conditional Access has two important control categories.

### A. Grant Controls

Grant Controls determine whether access should be allowed and what requirements must be satisfied before access is granted.

Common Grant Controls:

| Grant Control | Purpose |
|---|---|
| Require multifactor authentication | Requires MFA during authentication |
| Require authentication strength | Requires a specified combination of authentication methods |
| Require device to be marked as compliant | Requires a compliant device |
| Require Microsoft Entra hybrid joined device | Requires a hybrid joined device |
| Require approved client app | Requires an approved client application |
| Require app protection policy | Requires applicable app protection policies |
| Block access | Denies access |

### B. Session Controls

Session Controls determine how users can interact with applications after access has been granted.

Common Session Controls:

| Session Control | Purpose |
|---|---|
| Sign-in frequency | Determines how often users must authenticate again |
| Persistent browser session | Controls whether browser sessions remain signed in |
| Conditional Access App Control | Applies session monitoring and restrictions through Defender for Cloud Apps |
| App-enforced restrictions | Limits application functionality based on the session |

---

## 3. Correct Conditional Access Configuration

To satisfy the scenario, configure the following policy.

### Step 1 — Users

- Include: DevOps group.

### Step 2 — Conditions

Configure Locations:

- Include: Any location.
- Exclude: Trusted locations.

This ensures the policy applies to untrusted locations.

### Step 3 — Grant

Select:

- Require multifactor authentication.
- Require Microsoft Entra hybrid joined device.

Select:

**Require all the selected controls.**

This is important because both requirements must be satisfied.

### Step 4 — Enable Policy

Enable the policy after testing it in report-only mode.

---

## 4. Visual Configuration Summary

```text
Microsoft Entra ID
       |
       v
Conditional Access Policy
       |
       +-- Users
       |      |
       |      +-- DevOps Group
       |
       +-- Conditions
       |      |
       |      +-- Locations
       |             |
       |             +-- Any location
       |             +-- Exclude trusted locations
       |
       +-- Grant Controls
       |      |
       |      +-- Require MFA
       |      +-- Require Hybrid Joined Device
       |      |
       |      +-- Require ALL selected controls
       |
       +-- Enable Policy
```

---

## 5. Why the Proposed Solution Fails

The question proposes:

> Create a Conditional Access policy and enforce Session Controls.

This is incorrect because:

- MFA is an authentication requirement.
- Hybrid joined device is a device authentication requirement.
- Both are enforced through Grant Controls.
- Session Controls do not replace Grant Controls.

Therefore, the proposed solution does not meet the requirements.

---

## 6. AZ-104 Exam Memory Trick

Remember:

**Grant Controls = Before access is granted**

Examples:
- MFA
- Compliant device
- Hybrid joined device
- Block access

**Session Controls = Control the session**

Examples:
- Sign-in frequency
- Persistent browser session
- App-enforced restrictions
- Conditional Access App Control

### Quick Rule

If the question asks:

"What must the user satisfy before accessing the application?"

Think: **Grant Controls**

If the question asks:

"How should the user's session behave after access is granted?"

Think: **Session Controls**

---

## 7. Quick Knowledge Check

**Question 1**

You want users to provide MFA before accessing an application. Which control should you configure?

- A. Session Controls
- B. Grant Controls
- C. Sign-in frequency
- D. Persistent browser session

**Answer: B — Grant Controls**

**Question 2**

You want users to access an application only from a compliant device. Which control should you configure?

- A. Grant Controls
- B. Session Controls
- C. App-enforced restrictions
- D. Persistent browser session

**Answer: A — Grant Controls**

**Question 3**

You want users to authenticate again every 12 hours. Which control should you configure?

- A. Require MFA
- B. Require hybrid joined device
- C. Sign-in frequency
- D. Block access

**Answer: C — Sign-in frequency**

---

## 8. Official Microsoft Documentation

- [Conditional Access Overview](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview)
- [Conditional Access Grant Controls](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-grant)
- [Conditional Access Session Controls](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-conditional-access-session)
- [Conditional Access Locations](https://learn.microsoft.com/en-us/entra/identity/conditional-access/concept-assignment-network)

**Certification:** AZ-104 — Manage Microsoft Entra identities and access.
