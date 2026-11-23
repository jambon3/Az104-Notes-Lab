# Azure Storage Accounts - AZ-104 Notes

## Question Answers

### 1. How many copies of your data will be maintained at the minimum?

✅ **6 copies** (if configured with Geo-Redundant Storage - GRS)

### 2. The files hosted in `tutorialsdojostorage` are frequently accessed. What setting should you modify?

✅ **Access Tier = Hot**

---

# Azure Storage Redundancy Options

Understanding storage redundancy is a common AZ-104 exam topic.

## Locally Redundant Storage (LRS)

LRS keeps:

- 3 copies of data
- Within a single datacenter
- Lowest cost option

```text
Datacenter
├─ Copy 1
├─ Copy 2
└─ Copy 3
```

✅ Protects against:
- Disk failure
- Server failure
- Rack failure

❌ Does NOT protect against:
- Datacenter outage
- Regional outage

**Copies: 3**

---

## Zone-Redundant Storage (ZRS)

ZRS keeps:

- 3 copies of data
- Across multiple Availability Zones in the same Azure region

```text
Region
├─ Zone 1 → Copy 1
├─ Zone 2 → Copy 2
└─ Zone 3 → Copy 3
```

✅ Protects against:
- Disk failure
- Datacenter failure
- Availability Zone failure

❌ Does NOT protect against:
- Regional outage

**Copies: 3**

---

## Geo-Redundant Storage (GRS)

GRS keeps:

- 3 copies in the primary region
- 3 copies in a paired secondary region

```text
Primary Region
├─ Copy 1
├─ Copy 2
└─ Copy 3

      ↓

Secondary Region
├─ Copy 4
├─ Copy 5
└─ Copy 6
```

✅ Protects against:
- Datacenter failure
- Regional outage

**Copies: 6**

---

## Read-Access Geo-Redundant Storage (RA-GRS)

RA-GRS provides:

- Everything in GRS
- Read-only access to the secondary region

```text
Primary Region
Read + Write

Secondary Region
Read Only
```

✅ Protects against:
- Regional outage

✅ Allows reading data from the secondary region

**Copies: 6**

---

## Geo-Zone-Redundant Storage (GZRS)

GZRS combines:

- ZRS in the primary region
- Geo-replication to a secondary region

```text
Primary Region
├─ Zone 1
├─ Zone 2
└─ Zone 3

      ↓

Secondary Region
├─ Copy 4
├─ Copy 5
└─ Copy 6
```

✅ Protects against:
- Disk failure
- Datacenter failure
- Zone failure
- Regional outage

**Copies: 6**

---

## Read-Access GZRS (RA-GZRS)

RA-GZRS provides:

- Everything in GZRS
- Read-only access to the secondary region

✅ Highest level of availability for Azure Storage.

**Copies: 6**

---

# Azure Access Tiers

Access tiers determine storage cost and retrieval cost.

| Tier | Best For | Storage Cost | Access Cost |
|--------|--------|--------|--------|
| Hot | Frequently accessed data | Higher | Lowest |
| Cool | Infrequently accessed data (30+ days) | Lower | Higher |
| Cold | Rarely accessed data (90+ days) | Very Low | Higher |
| Archive | Long-term storage (180+ days) | Lowest | Highest |

### Examples

**Hot**
- Website images
- Video files
- Active documents

**Cool**
- Monthly reports
- Backup files

**Cold**
- Compliance data
- Older backups

**Archive**
- Long-term retention
- Legal records
- Data rarely retrieved

---

# AZ-104 Exam Memory Trick

```text
LRS  = Local copies (3)

ZRS  = Zone copies (3)

GRS  = Geo copies (6)

RA-GRS = GRS + Read Access

GZRS = Zones + Geo (6)

RA-GZRS = GZRS + Read Access
```

## For This Question

| Requirement | Answer |
|------------|---------|
| Minimum copies maintained | **6** |
| Frequently accessed media files | **Hot Access Tier** |

✅ **Final Answers: 6 copies and Hot Access Tier**
