# AZ-104 Study Notes: Azure Networking

> **Focus:** Virtual networks, subnetting, DNS, routing, connectivity, load balancing, network security, monitoring, and AKS networking.
>
> These notes use the **GreenMaze** example environment throughout. Image links are kept as relative paths, so keep the `image/` folder beside this Markdown file in your GitHub repository.

## Table of contents

- [1. Virtual networks and subnets](#1-virtual-networks-and-subnets)
- [2. CIDR and subnet sizing](#2-cidr-and-subnet-sizing)
- [3. Azure-reserved IP addresses](#3-azure-reserved-ip-addresses)
- [4. Creating a VNet in the Azure portal](#4-creating-a-vnet-in-the-azure-portal)
- [5. Static and public IP addresses](#5-static-and-public-ip-addresses)
- [6. Private Link and service endpoints](#6-private-link-and-service-endpoints)
- [7. Azure DNS](#7-azure-dns)
- [8. Routing, Route Server, BGP, and NAT Gateway](#8-routing-route-server-bgp-and-nat-gateway)
- [9. Load balancing and content delivery](#9-load-balancing-and-content-delivery)
- [10. Hybrid connectivity](#10-hybrid-connectivity)
- [11. Network security](#11-network-security)
- [12. Network monitoring and troubleshooting](#12-network-monitoring-and-troubleshooting)
- [13. Container networking in AKS](#13-container-networking-in-aks)
- [14. Azure Bastion](#14-azure-bastion)
- [15. AZ-104 exam review](#15-az-104-exam-review)
- [16. Review questions](#16-review-questions)

---

## 1. Virtual networks and subnets

### What is an Azure Virtual Network?

An **Azure Virtual Network (VNet)** is a logically isolated private network in Azure. It is the foundation for private communication between Azure resources, such as virtual machines and private endpoints.

Think of a VNet as a company's internal network, but hosted in Azure.

A VNet lets you:

- Deploy supported Azure resources into a private network.
- Divide the network into smaller sections called **subnets**.
- Control traffic with network security groups (NSGs), route tables, and other network services.
- Connect to other VNets and to on-premises networks.
- Configure DNS settings for resources in the network.

### What is a subnet?

A **subnet** is a smaller IP address range inside a VNet. Subnets help organize resources and apply network controls to different groups of resources.

For example, the GreenMaze VNet could use:

- VNet address space: `10.0.0.0/16`
- Web subnet: `10.0.1.0/24`
- Database subnet: `10.0.2.0/24`
- Management subnet: `10.0.3.0/24`

Each subnet's address range must fit inside the VNet address space. Subnet ranges in the same VNet must not overlap.

<img src="image/Subnet.png" alt="VNet and subnet diagram" width="600">

### Important VNet concepts

- A VNet has one or more address-space prefixes.
- A subnet uses a smaller prefix from the VNet address space.
- Creating multiple subnets does **not** automatically isolate them from one another.
- Use NSGs, route tables, Azure Firewall, and other controls to enforce the required security boundaries.
- Two separate VNets do not automatically communicate just because they are in the same subscription. You can connect them with VNet peering or other supported connectivity options.

---

## 2. CIDR and subnet sizing

**CIDR** means *Classless Inter-Domain Routing*. CIDR notation describes an IP network and how many bits identify the network portion of an address.

For example, `10.0.1.0/24` means that the first 24 bits identify the network. The remaining 8 bits are available for host addresses.

A simple way to remember it:

- A **smaller prefix number** means a larger network.
- A **larger prefix number** means a smaller network.

For example, a `/16` contains more IP addresses than a `/24`.

### Common IPv4 prefix sizes

| CIDR prefix | Total IPv4 addresses |
|---:|---:|
| `/32` | 1 |
| `/31` | 2 |
| `/30` | 4 |
| `/29` | 8 |
| `/28` | 16 |
| `/27` | 32 |
| `/26` | 64 |
| `/25` | 128 |
| `/24` | 256 |

The number of addresses is calculated using:

\[
2^{(32-\text{prefix length})}
\]

For example, a `/24` has \(2^{8}=256\) total addresses.

### Azure subnet size to remember

For Azure IPv4 subnets, Azure reserves **five addresses per subnet**. Therefore:

- `/24`: 256 total addresses − 5 reserved = **251 usable addresses**
- `/27`: 32 total addresses − 5 reserved = **27 usable addresses**
- `/29`: 8 total addresses − 5 reserved = **3 usable addresses**

Azure's smallest supported IPv4 subnet size is `/29`. The table above is a general CIDR reference; not every prefix is valid for every Azure subnet or service.

---

## 3. Azure-reserved IP addresses

Azure reserves five IPv4 addresses in every subnet: the **first four** addresses and the **last** address.

Example subnet: `192.168.1.0/24`

| Address | Purpose |
|---|---|
| `192.168.1.0` | Identifies the virtual network address. |
| `192.168.1.1` | Used by Azure as the default gateway. |
| `192.168.1.2` | Reserved by Azure for DNS mapping. |
| `192.168.1.3` | Reserved by Azure for DNS mapping. |
| `192.168.1.255` | Reserved as the subnet's broadcast address. |

<img src="image/subnet-reserve.png" alt="Five Azure-reserved IPv4 addresses in a subnet" width="600">

**Remember:** first four + last one = five reserved addresses.

For a `10.0.1.0/24` subnet, the address range is `10.0.1.0` through `10.0.1.255`. Azure reserves five of those addresses, leaving 251 available for allocation.

---

## 4. Creating a VNet in the Azure portal

1. Sign in to the Azure portal.
2. Search for **Virtual networks**.
3. Select **Create**.
4. Choose the subscription and resource group.
5. Enter a name, such as `VNet-GreenMaze`.
6. Select the Azure region.
7. Configure the address space, such as `10.0.0.0/16`.
8. Add non-overlapping subnets, such as `10.0.1.0/24` and `10.0.2.0/24`.
9. Review the configuration and select **Create**.

<img src="image/subnet1.png" alt="Creating a virtual network in the Azure portal" width="600">

**Microsoft Learn:** [Create a virtual network using the Azure portal](https://learn.microsoft.com/en-us/azure/virtual-network/quickstart-create-virtual-network?tabs=portal)

<img src="image/scenariso.png" alt="Virtual network scenario diagram" width="600">

<img src="image/scenario2.png" alt="Additional virtual network scenario diagram" width="600">

### VNet design considerations

Before creating a VNet, consider:

- **Address space:** Choose a range large enough for current and future subnets.
- **IP overlap:** Avoid overlapping ranges with other VNets and on-premises networks that may need to connect.
- **Subnet design:** Separate workloads when different security or routing controls are required.
- **Region:** Select the region where the resources will be deployed.
- **Connectivity:** Decide whether the VNet needs peering, VPN Gateway, ExpressRoute, or Virtual WAN.
- **DNS:** Determine whether Azure-provided DNS, Azure Private DNS, or custom DNS servers are required.
- **Security:** Plan NSGs, route tables, and any centralized firewall requirements.

---

## 5. Static and public IP addresses

### Static private IP addresses

A static private IP address remains assigned to a resource until it is changed or released. Static addressing is useful when other systems depend on a resource's IP address.

Common examples include:

- Domain controllers and DNS servers.
- Network appliances and firewall interfaces.
- Servers whose IP addresses are referenced by other systems.
- Services that use IP-based firewall allowlists.
- Systems where DNS records or application configuration depend on a stable address.

A static IP is not automatically more secure. Use it when stable addressing is operationally necessary.

### Public IP address associations

Depending on the service and configuration, a public IP address can be associated with or used by:

- A virtual machine's network interface.
- A public Load Balancer frontend.
- A VPN Gateway IP configuration.
- An Application Gateway frontend.
- Azure Firewall.

A public IP exposes an internet-reachable endpoint; it does not, by itself, grant access through the resource's security rules.

---

## 6. Private Link and service endpoints

Both **Private Link** and **service endpoints** help control access from a VNet to supported Azure services, but they work differently.

### Azure Private Link

Azure Private Link lets a VNet access a supported Azure service through a **private endpoint**. A private endpoint is a network interface with a private IP address from the VNet address space.

The service can then be accessed privately through that endpoint rather than through its public endpoint.

- **Private Link:** private connectivity to a supported service.
- **Private endpoint:** the private IP-based network interface in your VNet that provides that connectivity.

### VNet service endpoints

A service endpoint extends a VNet subnet's identity to a supported Azure service. You enable it on a subnet, then configure the target service's network rules to allow that subnet.

Important points:

1. Enable the service endpoint on the subnet, not on an individual VM.
2. Configure the Azure service's network firewall to allow the selected subnet.
3. Resources in that subnet can use the permitted service endpoint, subject to the service's other security controls.
4. Correct Azure permissions are still required. For example, access to Storage data still requires appropriate data-plane permissions.
5. The service continues to use its service endpoint; a service endpoint does **not** give the service a private IP address in your VNet.

### Quick comparison

| Feature | Service endpoint | Private endpoint |
|---|---|---|
| Main purpose | Gives a subnet identity for access control | Provides private IP connectivity to a service |
| Private IP in your VNet | No | Yes |
| Service public endpoint | Remains part of the service architecture | Can be disabled when private-only access is required and supported |
| Configured on | Subnet | A private endpoint resource in a VNet/subnet |

**Memory aid:**

- VNet peering = connects networks.
- Service endpoint = subnet-based access to a supported service.
- Private endpoint = private IP connectivity to a supported service.

---

## 7. Azure DNS

DNS (Domain Name System) translates domain names into IP addresses.

Azure provides several DNS capabilities.

### Azure DNS

Azure DNS is a hosting and management service for DNS domains and records. It uses Azure infrastructure and can be managed through the Azure portal, APIs, and other Azure tools.

### Azure Public DNS

Azure Public DNS hosts public DNS zones and records that can be resolved from the internet.

Example: hosting public records for `example.com`.

### Azure Private DNS

Azure Private DNS hosts private DNS zones for name resolution within linked virtual networks. It allows Azure resources to resolve private names without requiring you to deploy and maintain your own DNS servers for that zone.

Example private zone: `greenmaze.local`

| Hostname | Private IP |
|---|---|
| `web.greenmaze.local` | `10.0.1.4` |
| `db.greenmaze.local` | `10.0.1.5` |

Azure VMs in VNets linked to the private DNS zone can resolve those records.

### Azure DNS Private Resolver

Azure DNS Private Resolver allows DNS queries to move between Azure and on-premises networks without deploying your own DNS forwarder VMs.

Imagine this GreenMaze environment:

- Azure VNet: `10.0.0.0/16`
- Azure web VM: `10.0.1.4`
- Azure database VM: `10.0.1.5`
- Montreal office: uses its own on-premises DNS server.
- Azure Private DNS zone: `greenmaze.local`

A computer in the Montreal office may need to resolve `db.greenmaze.local`. The on-premises DNS server does not automatically know the records in the Azure Private DNS zone. DNS Private Resolver can provide the connection between the two DNS environments.

<img src="image/dns-resolver.png" alt="Azure DNS Private Resolver architecture" width="800">

#### Inbound endpoint

An inbound endpoint lets DNS queries **come into Azure** from an on-premises network.

Example flow:

`Montreal PC → On-premises DNS → Azure DNS Private Resolver inbound endpoint → Azure Private DNS`

The on-premises DNS server forwards queries for the Azure private domain to the inbound endpoint's private IP address.

#### Outbound endpoint

An outbound endpoint lets DNS queries **go from Azure to an external DNS server**, such as the company's on-premises DNS server.

Example flow:

`Azure VM → Azure DNS Private Resolver outbound endpoint → On-premises DNS server`

Azure DNS forwarding rules determine which queries are sent to which DNS servers.

**Remember the direction:**

- **Inbound:** on-premises asks Azure.
- **Outbound:** Azure asks on-premises.

---

## 8. Routing, Route Server, BGP, and NAT Gateway

### System routes and user-defined routes

Azure automatically creates **system routes** for network traffic. A **user-defined route (UDR)** lets you override or customize routing behavior for a subnet.

A route can direct traffic to supported next-hop types such as:

- Virtual network.
- Virtual network gateway.
- Virtual appliance.
- Internet.
- None (drop the traffic).

You can use a service tag as an address prefix in a UDR instead of specifying an explicit IP range.

**Exam terms:**

- **System route:** route created and managed by Azure.
- **UDR:** route that you create to influence traffic flow.
- **BGP:** protocol used by network devices to exchange route information dynamically.

### Azure Route Server

Azure Route Server allows Azure and network virtual appliances (NVAs) to exchange routing information automatically using BGP, instead of relying entirely on manually maintained routes.

An **NVA (Network Virtual Appliance)** is a virtual network device that performs networking or security functions.

Examples include:

- Firewall.
- Router.
- VPN appliance.
- Network traffic inspection appliance.

Common vendors include Fortinet, Cisco, and Palo Alto Networks.

<img src="image/route-server.png" alt="Azure Route Server diagram" width="400">

### Border Gateway Protocol (BGP)

BGP is a routing protocol that allows routers and network devices to advertise which IP networks they can reach.

Think of a router telling another router: "I know how to reach this network."

When advertised routes change, BGP can dynamically update routing information.

### Route Server: with and without dynamic routing

Without dynamic route exchange, administrators may need to update route tables manually when network paths change.

With Azure Route Server, supported NVAs can exchange routes with Azure using BGP.

```text
             Azure Route Server
                    |
               BGP routing
                /       \
          Firewall     Router
                \       /
                 Azure VNet
```

Benefits include dynamic route exchange and less manual route maintenance. Route Server does not itself act as a firewall or forward user traffic.

### NAT Gateway

Azure NAT Gateway provides scalable, outbound-only internet connectivity for resources in an associated subnet.

When VMs use the NAT Gateway for outbound connections:

- Their private source IP addresses are translated to the NAT Gateway's public IP address.
- External websites see the NAT Gateway's public IP, rather than the VMs' private IP addresses.
- Multiple VMs can share the NAT Gateway's outbound public IP configuration.

<img src="image/NAT.png" alt="Azure NAT Gateway outbound traffic diagram" width="400">

**Remember:**

1. NAT Gateway is primarily for outbound connectivity.
2. It provides predictable outbound public IP addressing.
3. It does not provide unsolicited inbound internet access to VMs.

---

## 9. Load balancing and content delivery

A load balancer distributes network traffic across multiple servers or application instances. Different Azure services operate at different layers and solve different problems.

### Main services

| Service | Layer / method | Main purpose |
|---|---|---|
| Azure Load Balancer | Layer 4 (TCP/UDP) | Distributes network traffic across backend resources |
| Azure Application Gateway | Layer 7 (HTTP/HTTPS) | Routes and protects web application traffic |
| Azure Front Door | Global Layer 7 | Global web routing, acceleration, and application delivery |
| Azure Traffic Manager | DNS-based | Directs users to appropriate endpoints |
| Azure CDN | Content caching | Caches content closer to users |
| Web Application Firewall (WAF) | Web application inspection | Helps protect web applications from common web attacks |

### Azure Load Balancer

Azure Load Balancer operates at **Layer 4**. It distributes TCP and UDP traffic across backend resources.

It can provide:

- Public load balancing for internet-facing applications.
- Internal load balancing for private applications.
- Health-probe-based distribution across healthy backend instances.

### Azure Application Gateway

Azure Application Gateway is a **Layer 7** web traffic load balancer for HTTP and HTTPS applications.

It can inspect application-level information such as:

- URL paths.
- Host names.
- HTTP headers.
- Cookies.

Features include URL-based routing, TLS termination, and Web Application Firewall (WAF) capabilities, depending on configuration and SKU.

### Azure Front Door

Azure Front Door provides global Layer 7 application delivery. It can route and accelerate web traffic across globally distributed application endpoints and supports WAF capabilities.

### Azure Traffic Manager

Azure Traffic Manager is a DNS-based global traffic-routing service. It directs clients to endpoints based on the configured routing method, such as:

- Priority / failover.
- Performance.
- Geographic routing.

Traffic Manager does **not** proxy the application traffic itself. It responds to DNS queries with an endpoint selection.

It can direct users to supported endpoints such as Azure App Services, public IP-based services, and external endpoints.

Portal path:

```text
Azure portal
  └── Traffic Manager profiles
        └── Create
```

### How to remember the differences

- **Traffic Manager:** Which endpoint or region should DNS direct the user to?
- **Load Balancer:** Which backend server should receive this TCP/UDP connection?
- **Application Gateway:** Which web application or URL path should receive this HTTP/HTTPS request?
- **Front Door:** How should global web traffic be routed and accelerated?
- **CDN:** Can content be cached closer to the user?

### Application Gateway vs. Load Balancer

| Azure Load Balancer | Azure Application Gateway |
|---|---|
| Layer 4 | Layer 7 |
| IP address, protocol, and port | HTTP/HTTPS details such as host name and URL |
| TCP and UDP | Web application traffic |
| Network-level distribution | Application-level routing and web protection features |

---

## 10. Hybrid connectivity

**Hybrid connectivity** means connecting on-premises infrastructure to Azure so that both environments can communicate.

### Which service should you choose?

| Requirement | Azure solution |
|---|---|
| Connect an entire office network to Azure over the internet | Site-to-Site (S2S) VPN |
| Connect an individual employee's device to Azure | Point-to-Site (P2S) VPN |
| Connect two Azure VNets | VNet-to-VNet VPN or VNet peering, depending on the design |
| Private connectivity to Microsoft cloud through a connectivity provider | ExpressRoute |
| Centrally connect many branch offices, users, and VNets | Azure Virtual WAN |
| Resolve private DNS names between Azure and on-premises | Azure DNS Private Resolver |

### VPN Gateway

Azure VPN Gateway creates encrypted connectivity between an Azure VNet and another network over the public internet.

Example:

- Montreal office has computers and servers.
- Azure hosts virtual machines in a Canada region.
- Employees and servers need secure communication between the two environments.

A VPN Gateway provides the VPN tunnel.

Common connection types:

| VPN type | Purpose |
|---|---|
| Site-to-Site (S2S) | Connects an entire on-premises network to Azure |
| Point-to-Site (P2S) | Connects an individual client device to Azure |
| VNet-to-VNet | Connects Azure VNets using VPN connectivity |

**Important exam terms:**

- `GatewaySubnet`: the dedicated subnet required by a VNet VPN Gateway.
- `Local Network Gateway`: an Azure resource that represents the on-premises network gateway and address ranges for a site-to-site connection.

### ExpressRoute

ExpressRoute connects on-premises infrastructure to Microsoft cloud services through a connectivity provider. It provides private connectivity to Microsoft's network rather than sending the connection over the public internet.

Common reasons to use ExpressRoute:

- More predictable network performance.
- Private connectivity.
- High-bandwidth options.
- Enterprise hybrid connectivity requirements.

**Remember:** ExpressRoute = private connectivity to Microsoft's network through a connectivity provider.

### Azure Virtual WAN

Azure Virtual WAN simplifies large-scale connectivity between multiple offices, remote users, datacenters, and Azure VNets.

Imagine a company with offices in Montreal, Toronto, Vancouver, and London, plus multiple Azure VNets.

Instead of managing every connection independently, Virtual WAN provides a centralized hub-based architecture that can connect:

- Branch offices through VPN.
- Datacenters through ExpressRoute.
- Remote users.
- Azure VNets.

### Azure Peering Service

Azure Peering Service improves connectivity between an enterprise network or its users and Microsoft cloud services through participating internet service providers.

It is **not the same as VNet peering**.

Example: employees in Montreal use Microsoft 365, Azure, SharePoint Online, and Teams. Their internet traffic travels through an ISP before reaching Microsoft's network. Peering Service can optimize that path through participating providers.

**Remember:** Peering Service = optimized internet connectivity to Microsoft services.

### VNet peering

VNet peering connects two Azure VNets so resources can communicate over Microsoft's backbone network.

Key points:

- No VPN tunnel is required for ordinary VNet peering.
- Traffic between peered VNets uses Microsoft's private backbone.
- Peered VNets can be in the same or different Azure regions.
- The VNet address spaces must not overlap.
- Peering does not automatically create transitive connectivity through a third VNet.

### Gateway transit

Gateway transit is a feature of VNet peering. It allows one VNet to share its VPN Gateway or ExpressRoute gateway with a peered VNet, subject to the peering configuration.

### Compare the connectivity services

| Service | Main purpose | Connection model |
|---|---|---|
| VPN Gateway | Securely connect networks | Encrypted tunnel over the internet |
| ExpressRoute | Private enterprise connectivity | Private provider connection |
| Virtual WAN | Centrally connect many networks | Hub-based VPN, ExpressRoute, and other connectivity |
| Peering Service | Improve connectivity to Microsoft | Optimized internet routing |
| VNet peering | Connect Azure VNets | Private connectivity over Microsoft's backbone |

---

## 11. Network security

### Azure Firewall Manager

Azure Firewall Manager helps centrally manage firewall policies and security configurations across Azure network environments. It is used to organize and manage network security at scale.

### Azure Firewall

Azure Firewall is a managed, stateful network security service. It can control traffic entering, leaving, and moving between Azure networks according to configured rules.

Think of it as a central security guard for network traffic.

For example, you can configure rules to allow a VM to access approved destinations while blocking other destinations. You may also need UDRs to make sure the relevant traffic is routed through the firewall.

Azure Firewall is **stateful**: it tracks connections and permits return traffic for allowed connections.

### Web Application Firewall (WAF)

WAF helps protect web applications by inspecting HTTP and HTTPS requests and blocking or logging suspicious traffic according to configured rules.

It is designed for web application traffic, rather than being a general replacement for network-level firewall controls.

### Azure DDoS Protection

Distributed Denial of Service (DDoS) attacks attempt to overwhelm an application or network resource with traffic.

Azure DDoS Protection helps protect supported Azure resources against network-layer DDoS attacks.

### Network Security Group (NSG)

An NSG filters inbound and outbound network traffic using rules based on items such as:

- Source and destination IP addresses or service tags.
- Source and destination ports.
- Protocol.
- Allow or deny action.
- Rule priority.

An NSG can be associated with:

1. A subnet.
2. A network interface (NIC).

NSGs are **stateful**: return traffic for an allowed connection is automatically permitted.

### NSGs at the subnet and NIC levels

Traffic can be evaluated by NSGs associated with both the subnet and NIC. For traffic to be admitted, it must be permitted by the applicable rules at both levels.

If you are unsure which rules apply, inspect the VM's effective security rules and effective routes in the Azure portal.

### Application Security Groups (ASGs)

An ASG lets you group virtual machines by application role so that NSG rules can refer to logical groups instead of individual IP addresses.

Example groups:

- `ASG-Web`
- `ASG-App`
- `ASG-Database`

You can then create NSG rules that allow the web tier to communicate with the application tier, without maintaining a separate rule for every VM IP address.

### NSG vs. Azure Firewall vs. Bastion

| Service | Main purpose |
|---|---|
| NSG | Basic network traffic filtering at subnet or NIC level |
| Azure Firewall | Centralized, stateful network traffic inspection and filtering |
| Azure Bastion | Secure administrator access to VMs over RDP or SSH |

An NSG is often sufficient for basic subnet and VM traffic controls. Azure Firewall is useful when centralized inspection and policy across multiple subnets or VNets is required. Bastion is for administrative connectivity, not general traffic filtering.

<img src="image/firewall.png" alt="Azure Firewall network traffic diagram" width="600">

---

## 12. Network monitoring and troubleshooting

### Azure Network Watcher

Azure Network Watcher provides tools to monitor, diagnose, and troubleshoot Azure network connectivity.

One useful tool is **IP flow verify**.

IP flow verify checks whether a packet to or from a VM is allowed or denied by the configured network security rules. It can help identify which security rule is responsible for the result.

When troubleshooting, also consider:

- Effective security rules.
- Effective routes.
- Network topology.
- Connection troubleshooting.
- NSG flow logs, where configured and supported.

Azure Monitor and Azure Virtual Network Manager provide additional monitoring and network-management capabilities.

---

## 13. Container networking in AKS

### What is Azure Kubernetes Service (AKS)?

Azure Kubernetes Service is Microsoft's managed Kubernetes service.

Kubernetes helps automate:

- Container deployment.
- Application scaling.
- Recovery when containers fail.
- Load balancing and service discovery.
- Application networking.
- Workload management.

### Container network security

Imagine an AKS cluster with three application components:

- Frontend.
- Backend.
- Database.

If network communication is not restricted, pods may be able to communicate with more workloads than necessary. This can increase security risk.

For example, if an attacker compromises a frontend container, they may try to communicate directly with the database.

**Kubernetes Network Policies** allow you to define which pods can communicate with other pods, depending on the networking implementation and policy support in the cluster.

### Container network observability

Network observability means monitoring and understanding traffic:

- Into containers.
- Out of containers.
- Between containers and services.

This is important for identifying connectivity issues, unexpected communication, and potential security problems in AKS.

---

## 14. Azure Bastion

Azure Bastion is a platform-managed service deployed in a VNet. It provides secure RDP or SSH connectivity to virtual machines through the Azure portal or supported native clients.

With Azure Bastion:

- VMs do not need public IP addresses for Bastion-based RDP/SSH access.
- Administrators can connect through the Azure portal.
- The connection is protected using TLS.
- You do not need to expose RDP or SSH directly to the public internet.

<img src="image/howwork.png" alt="How Azure Bastion works" width="600">

Think of Bastion as a secure remote-access gateway into your Azure network.

### Bastion subnet requirement

Dedicated Azure Bastion deployments require a separate subnet named:

`AzureBastionSubnet`

For the dedicated deployment described in these notes, the subnet must be `/26` or larger (that is, a prefix length of `/26` or a larger address range, such as `/25`).

Do not deploy Bastion into an ordinary application subnet.

### Bastion SKUs

The available Bastion tiers include Developer, Basic, Standard, and Premium. Features and deployment requirements vary by SKU and region. Check the current Microsoft documentation before deploying.

Bastion deployments generally incur charges while the service is provisioned. The Developer offering has different availability and feature limitations.

### Bastion vs. Azure Firewall

- **Bastion:** lets administrators securely connect to VMs.
- **Azure Firewall:** filters and controls network traffic.

---

## 15. AZ-104 exam review

### Quick decision table

| If the question asks for… | Think about… |
|---|---|
| Divide a VNet into smaller IP ranges | Subnets |
| Find the total number of addresses in a CIDR range | CIDR calculation |
| Remember Azure's reserved IPv4 addresses | First four and last one |
| Give an Azure service a private IP inside your VNet | Private endpoint / Private Link |
| Restrict a service to selected VNet subnets | Service endpoint and service network rules |
| Resolve private DNS names inside Azure | Azure Private DNS |
| Resolve DNS names between on-premises and Azure | Azure DNS Private Resolver |
| Exchange routes dynamically with an NVA | Azure Route Server and BGP |
| Provide predictable outbound internet IP addressing | NAT Gateway |
| Distribute TCP/UDP traffic | Azure Load Balancer |
| Route HTTP/HTTPS traffic using URL or host name | Application Gateway |
| Route users to regional endpoints using DNS | Traffic Manager |
| Globally accelerate and route web traffic | Azure Front Door |
| Cache content closer to users | Azure CDN |
| Connect an entire office to Azure over the internet | Site-to-Site VPN |
| Connect one remote user's device to Azure | Point-to-Site VPN |
| Use private connectivity through a provider | ExpressRoute |
| Centrally connect many branches and VNets | Azure Virtual WAN |
| Connect two Azure VNets privately | VNet peering |
| Filter traffic at subnet or NIC level | NSG |
| Centrally inspect network traffic | Azure Firewall |
| Protect HTTP/HTTPS applications | WAF |
| Protect against network-layer DDoS attacks | Azure DDoS Protection |
| Diagnose whether an NSG allows or denies a flow | Network Watcher IP flow verify |
| Connect to a VM without a public IP for RDP/SSH | Azure Bastion |

### Important distinctions

1. **VNet peering vs. service endpoint:** peering connects VNets; a service endpoint provides subnet-based access to a supported Azure service.
2. **Service endpoint vs. private endpoint:** a service endpoint does not create a private IP in your VNet; a private endpoint does.
3. **NSG vs. Azure Firewall:** NSGs provide basic filtering at subnet/NIC level; Azure Firewall provides centralized network security capabilities.
4. **Load Balancer vs. Application Gateway:** Load Balancer is Layer 4; Application Gateway is Layer 7.
5. **Traffic Manager vs. Front Door:** Traffic Manager uses DNS responses to direct clients; Front Door is a global application delivery service that proxies web traffic.
6. **VPN Gateway vs. ExpressRoute:** VPN Gateway uses encrypted tunnels over the internet; ExpressRoute uses private connectivity through a provider.
7. **Bastion vs. Firewall:** Bastion provides administrator access; Firewall controls network traffic.
8. **Inbound vs. outbound DNS Resolver endpoints:** inbound brings DNS queries from on-premises into Azure; outbound sends queries from Azure to external DNS servers.

---

## 16. Review questions

Try answering these without looking at the notes.

1. What is the difference between a VNet and a subnet?
2. How many total IPv4 addresses are in a `/24`?
3. How many IPv4 addresses does Azure reserve in each subnet?
4. How many usable addresses are available in an Azure `/24` subnet?
5. What is the smallest supported IPv4 subnet size in Azure?
6. What is the difference between a service endpoint and a private endpoint?
7. What is the purpose of Azure DNS Private Resolver's inbound endpoint?
8. What is the purpose of its outbound endpoint?
9. What does Azure Route Server exchange with network virtual appliances?
10. What is BGP used for?
11. What is the primary purpose of NAT Gateway?
12. Which service distributes TCP/UDP traffic?
13. Which service can route web traffic based on URL paths?
14. Which service uses DNS to direct users to different endpoints?
15. What is the difference between Site-to-Site and Point-to-Site VPN?
16. What does ExpressRoute provide?
17. What is the purpose of VNet peering?
18. Where can an NSG be associated?
19. What is the difference between an NSG and Azure Firewall?
20. Which Azure service lets an administrator connect to a VM without assigning it a public IP?

---

## References

- [Create a virtual network using the Azure portal](https://learn.microsoft.com/en-us/azure/virtual-network/quickstart-create-virtual-network?tabs=portal)
- [Azure Virtual Network documentation](https://learn.microsoft.com/en-us/azure/virtual-network/)
- [Azure Private Link documentation](https://learn.microsoft.com/en-us/azure/private-link/)
- [Azure DNS documentation](https://learn.microsoft.com/en-us/azure/dns/)
- [Azure Network Watcher documentation](https://learn.microsoft.com/en-us/azure/network-watcher/)
- [Azure Bastion documentation](https://learn.microsoft.com/en-us/azure/bastion/)
