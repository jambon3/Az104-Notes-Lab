
# Azure Ingress and Egress

## 1. Overview

Ingress and egress are fundamental networking concepts used to describe the direction of network traffic.

- **Ingress:** Incoming network traffic entering an application, service, or network.
- **Egress:** Outgoing network traffic leaving an application, service, or network.

These concepts are important when configuring Azure networking, Azure Container Apps, firewalls, network security groups, and application connectivity.

---

## 2. Ingress

Ingress is the mechanism that controls incoming traffic to an application or service.

In Azure Container Apps, ingress determines how external or internal clients can communicate with a containerized application.

### How Ingress Works

Example: Grafana deployed in Azure Container Apps.

1. A user opens a web browser.
2. The user enters the Grafana application URL.
3. The request reaches the Azure Container Apps ingress endpoint.
4. Ingress receives the incoming HTTP or HTTPS request.
5. Azure forwards the request to the Grafana container.
6. Grafana processes the request and returns the dashboard.

Without an appropriately configured ingress endpoint, external users cannot access the application through a public URL.

### Types of Azure Container Apps Ingress

#### External Ingress

External ingress allows an application to receive traffic from outside the Container Apps environment.

Characteristics:

- Provides an externally accessible application endpoint.
- Supports HTTP and HTTPS traffic.
- Allows access from the public internet, subject to network restrictions.
- Suitable for websites, public APIs, and externally accessible dashboards.

Example:

A company deploys Grafana in Azure Container Apps and enables external ingress so development teams can access the monitoring dashboard.

#### Internal Ingress

Internal ingress restricts application access to within the Container Apps environment.

Characteristics:

- Does not expose the application directly to the public internet.
- Allows communication from other applications within the environment.
- Suitable for internal applications and backend services.
- Helps reduce public exposure.

Example:

A company deploys an internal employee management application that must only be accessible through its private application environment.

---

## 3. Egress

Egress refers to outgoing network traffic leaving an application, service, or network.

Example:

1. Grafana runs inside an Azure Container App.
2. Grafana needs to retrieve monitoring data from an external database.
3. Grafana sends a network request to the database.
4. The request leaves the container environment.
5. The database responds to Grafana.

This outgoing communication is egress traffic.

### Examples of Egress Traffic

- Applications connecting to external databases.
- Virtual machines downloading software updates.
- Containers accessing external APIs.
- Applications sending data to external services.
- Servers communicating with cloud-based monitoring platforms.

---

## 4. Ingress vs. Egress

| Characteristic | Ingress | Egress |
|---|---|---|
| Direction | Incoming | Outgoing |
| Traffic | Enters a service or network | Leaves a service or network |
| Example | User accessing Grafana | Grafana accessing an external database |
| Security considerations | Control incoming connections | Control outgoing connections |

### Simple Rule

- Ingress = Traffic coming in.
- Egress = Traffic going out.

---

## 5. Ingress vs. Load Balancer vs. Firewall vs. DNS

These technologies are related to network traffic management but have different responsibilities.

| Technology | Responsibility |
|---|---|
| Ingress | Controls how incoming application traffic reaches a service |
| Load Balancer | Distributes incoming traffic across multiple backend resources |
| Firewall | Allows or blocks network traffic according to security rules |
| DNS | Resolves domain names into IP addresses |

### Important Distinction

Ingress is not the same as a firewall or load balancer.

Ingress determines how application traffic enters a service. Firewalls enforce network security policies, while load balancers distribute traffic between backend resources.

---

## 6. Azure Container Apps Configuration

When deploying an application in Azure Container Apps, ingress configuration determines its network accessibility.

Important configuration options include:

- **Ingress:** Enable or disable application ingress.
- **Ingress type:** External or internal.
- **Target port:** Port on which the container application listens.
- **Transport:** HTTP, HTTP/2, or TCP, depending on the application and configuration.
- **IP restrictions:** Restrict access based on client IP addresses.
- **Custom domain:** Associate a custom DNS name with the application.
- **TLS certificate:** Secure HTTPS communication.

### Example: Grafana

A company deploys Grafana in Azure Container Apps.

Configuration:

- Resource Group: tdrg-grafana
- Region: Canada Central
- Ingress: External
- Target Port: 3000 (if Grafana is configured to listen on port 3000)
- Transport: HTTP
- IP Restrictions: Allow

Azure provides an application FQDN that users can access.

Example:

https://grafana.example.region.azurecontainerapps.io

The actual endpoint is generated by Azure.

---

## 7. Security Considerations

Ingress and egress configurations should follow the principle of least privilege.

Recommended practices:

1. Use internal ingress for applications that do not require public access.
2. Use external ingress only when necessary.
3. Configure IP restrictions when appropriate.
4. Use HTTPS to encrypt application traffic.
5. Use managed identities where supported instead of hardcoded credentials.
6. Restrict unnecessary outbound connections.
7. Monitor network activity and application access.
8. Apply appropriate firewall and network security controls.

---

## 8. AZ-104 Exam Notes

Understand these distinctions:

| Exam Scenario | Appropriate Concept |
|---|---|
| Users need to access a public application | External ingress |
| An application must remain private | Internal ingress |
| An application connects to an external database | Egress |
| Restrict access based on client IP addresses | IP restrictions |
| Distribute traffic across multiple backend servers | Load Balancer |
| Resolve a domain name to an IP address | DNS |
| Allow or block network traffic | Firewall / NSG |

### Common Exam Question

A company deploys Grafana in Azure Container Apps. Development teams need to access the application from outside the Container Apps environment.

**Solution:** Configure external ingress.

External ingress provides an externally accessible application endpoint without requiring users to connect directly to the container's internal IP address.

---

## 9. Additional Learning Resources

- Microsoft Learn: Azure Container Apps networking
- Microsoft Learn: Azure Container Apps ingress
- Microsoft Learn: Azure networking fundamentals
- Microsoft Learn: Network Security Groups
- Microsoft Learn: Azure Firewall
- Microsoft Learn: Azure Load Balancer

---

## 10. Summary

Ingress and egress describe the direction of network traffic.

- Ingress: Incoming traffic.
- Egress: Outgoing traffic.
- External ingress: Allows access from outside the Container Apps environment.
- Internal ingress: Restricts access to the internal Container Apps environment.

Understanding these concepts is essential for managing application connectivity, cloud infrastructure, and network security in Microsoft Azure.
