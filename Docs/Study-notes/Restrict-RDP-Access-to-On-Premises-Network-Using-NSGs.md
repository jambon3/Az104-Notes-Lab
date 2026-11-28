# AZ-104 – Restrict RDP Access to On-Premises Network Using NSGs

## Scenario

You have an Azure subscription containing:

- Virtual network: `TDVNet1`
- Subnet: `TDSubnet1`
- Three virtual machines deployed in `TDSubnet1`
- Each VM has a public IP address.
- Azure Bastion is deployed in `TDVNet1`.
- The network security team has the Network Contributor role.
- Applications are accessible over HTTPS (443) and RDP (3389).
- A site-to-site VPN connects the on-premises network to Azure.

**Objective:**

Restrict RDP (TCP 3389) access to the on-premises network while allowing Internet users to continue accessing applications over HTTPS (TCP 443).

## Correct Answer

**Option 3 – Add NSG rules to allow incoming RDP connections from the on-premises network and deny RDP connections from the Internet, using an NSG linked to TDSubnet1.**

## 1. Explanation

You need to restrict RDP access based on the source IP address while keeping HTTPS accessible from the Internet.

A Network Security Group (NSG) allows you to filter inbound and outbound network traffic using:

- Source IP address
- Destination IP address
- Source port
- Destination port
- Protocol
- Allow or Deny action
- Priority

### Recommended NSG rules

Create the following inbound rules:

| Priority | Rule | Source | Destination port | Action |
|---|---|---|---|---|
| 100 | Allow-OnPrem-RDP | On-premises IP range | 3389 | Allow |
| 110 | Deny-Internet-RDP | Internet | 3389 | Deny |
| Existing | Allow-HTTPS | Internet | 443 | Allow |

**Important:** Lower priority numbers are evaluated first.

The allow rule must come before the deny rule.

The existing HTTPS rule should remain unchanged.

---

## 2. Why the Other Answers Are Wrong

### Option 1 – Reconfigure the IP ranges for the site-to-site VPN

**Incorrect.**

- The VPN is already configured and connected.
- The on-premises IP range is already known.
- Changing VPN address ranges does not directly control which sources can access RDP.
- NSGs are designed to filter network traffic.

### Option 2 – Add a route table with custom routes to TDSubnet1 to redirect RDP traffic

**Incorrect.**

- Route tables control traffic routing.
- They determine where network packets travel.
- They do not provide the required source-based access control.
- NSGs are the appropriate solution for allowing or denying specific traffic.

### Option 4 – Change the address space of TDSubnet1

**Incorrect.**

- Changing the subnet address space does not restrict RDP access.
- It could affect existing VM network configurations.
- It does not solve the security requirement.

---

## 3. AZ-104 Exam Notes

### Azure Networking Services

| Azure feature | Purpose |
|---|---|
| NSG | Filter inbound and outbound network traffic |
| Route table (UDR) | Control network traffic routing |
| Site-to-site VPN | Connect on-premises networks to Azure |
| Azure Bastion | Secure VM access without exposing RDP/SSH directly to the Internet |

### NSG Priority

- Lower number = higher priority.
- Rules are evaluated in priority order.
- Once a matching rule is found, subsequent rules are not evaluated.

### Default NSG Rules

| Rule | Direction | Action |
|---|---|---|
| AllowVNetInBound | Inbound | Allow |
| AllowAzureLoadBalancerInBound | Inbound | Allow |
| DenyAllInBound | Inbound | Deny |

---

## 4. Traffic Flow – Expected Result

| Traffic | Expected result |
|---|---|
| Internet → HTTPS (443) | Allow |
| Internet → RDP (3389) | Deny |
| On-premises → RDP (3389) | Allow |

## 5. Important Security Consideration

Azure Bastion is already deployed.

Administrators should preferably use Bastion instead of exposing RDP through public IP addresses.

Bastion provides secure administrative access to VMs without requiring public IP addresses on the VM network interfaces.

## Key Exam Takeaway

When a question asks you to restrict network access based on **source IP addresses and ports**, think:

**Network Security Group (NSG).**

- NSG = Traffic filtering
- UDR = Traffic routing
- VPN = Private network connectivity
- Bastion = Secure VM administration
