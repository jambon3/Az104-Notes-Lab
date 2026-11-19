# Azure Key Vault – Secrets, Passwords and Certificate Management

## 1. What is Azure Key Vault?

Azure Key Vault is a cloud service used to securely store and manage sensitive information.

It is designed to protect:

- Passwords
- SSL/TLS certificates
- API keys
- Database connection strings
- Encryption keys
- Application credentials
- Private keys

**Important:** Key Vault is not limited to Azure resources. You can use it to manage credentials for external applications, on-premises servers, network equipment, and third-party services.

Examples:

| System | Information to protect |
|---|---|
| Windows Server | Administrator credentials |
| Linux Server | SSH credentials |
| Cisco Meraki | API keys |
| VMware | Service account credentials |
| SQL Server | Database passwords |
| Website | SSL/TLS certificates |
| Third-party application | API keys |
| VPN | Authentication secrets |

## 2. Three Main Object Types

Azure Key Vault supports three main types of objects.

### A. Secrets

Secrets are used to store sensitive text-based information.

Examples:

- Administrator passwords
- Database passwords
- API keys
- Application client secrets
- Connection strings
- Tokens

A secret is encrypted when stored in Key Vault. Authorized users and applications can retrieve its value.

### B. Certificates

Certificates are used to establish identity, trust, and encrypted communications.

Examples:

- Website SSL/TLS certificates
- Internal company certificates
- Application authentication certificates
- Client authentication certificates

Key Vault supports importing existing certificates, including certificates issued by external Certificate Authorities.

It can also manage certificate lifecycle operations and automatic renewal with supported certificate authorities.

**Best practice:** Store certificates using the Key Vault certificate object rather than storing certificate files as generic secrets.

### C. Keys

Keys are used for cryptographic operations.

Examples:

- Encrypting data
- Decrypting data
- Signing information
- Verifying digital signatures
- Protecting encryption keys

Key Vault can perform certain cryptographic operations without exposing the private key material to the application.

## 3. Example: Centralized IT Credential Management

Imagine an IT department managing the following infrastructure:

```text
                 IT ADMINISTRATOR
                         |
                         v
                 MICROSOFT ENTRA ID
                         |
                         v
                  AZURE KEY VAULT
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
       SECRETS       CERTIFICATES       KEYS
          |              |              |
          v              v              v
     Server Login     Website SSL     Encryption
     Database Login   VPN Certificate  Signing Key
     API Credentials  Application Cert
     VMware Login
     Firewall API Key
```

Instead of keeping passwords in spreadsheets or configuration files, authorized administrators and applications retrieve them from a centralized vault.

## 4. How Access Works

Key Vault uses Microsoft Entra ID to authenticate users and applications.

Authentication answers:

**Who are you?**

Authorization answers:

**What are you allowed to access?**

For example:

| Identity | Permission |
|---|---|
| IT Administrator | Manage approved secrets |
| Application Managed Identity | Retrieve required secrets |
| Security Administrator | Manage access and security |
| Regular Employee | No access unless authorized |

### Recommended access model

Use Azure Role-Based Access Control (RBAC) where possible.

Some relevant built-in roles include:

- Key Vault Administrator
- Key Vault Secrets Officer
- Key Vault Secrets User
- Key Vault Certificates Officer
- Key Vault Certificate User

Follow the principle of least privilege: grant only the permissions necessary to perform the task.

Key Vault access policies are also supported as a legacy authorization model.

## 5. Example: Store a Password

Suppose you need to store a password for an external VMware service account.

You could create a secret with:

| Property | Example |
|---|---|
| Vault | kv-company-it |
| Secret name | VMware-Service-Password |
| Secret value | Your sensitive password |
| Description | VMware service account credential |

The password is encrypted by Key Vault.

An authorized application or administrator can retrieve it when required.

**Never place real passwords, API keys, certificates, or private keys in a public GitHub repository.**

## 6. Example: Use Key Vault with an ARM Template

Key Vault can also supply secrets during Azure Resource Manager deployments.

For example, when deploying virtual machines, an ARM template can receive an administrator password through a Key Vault reference in its parameter file.

The template can declare a secure parameter:

```json
{
  "adminPassword": {
    "type": "secureString"
  }
}
```

The ARM parameters file can reference an existing Key Vault secret:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminPassword": {
      "reference": {
        "keyVault": {
          "id": "/subscriptions/SUBSCRIPTION-ID/resourceGroups/RG-SECURITY/providers/Microsoft.KeyVault/vaults/KV-COMPANY"
        },
        "secretName": "VMAdminPassword"
      }
    }
  }
}
```

This avoids putting the actual password in the template or parameters file.

The Key Vault must be configured to allow ARM template deployments, and the deploying identity must have the required permissions.

## 7. Does Azure Key Vault Cost Money?

Yes. Azure Key Vault has usage-based pricing.

The main vault tiers are:

| Tier | Description |
|---|---|
| Standard | Software-protected keys, secrets, and certificates |
| Premium | Adds HSM-protected keys |

Charges can include:

- Secret operations
- Key operations
- Certificate operations
- Certificate renewal requests

For a small learning environment, Standard is generally the appropriate starting point.

Check current prices before deploying:

https://azure.microsoft.com/en-ca/pricing/details/key-vault/

## 8. Security Best Practices

1. Use Microsoft Entra ID for authentication.
2. Prefer Azure RBAC for authorization.
3. Apply least-privilege permissions.
4. Enable soft delete and purge protection where appropriate.
5. Enable diagnostic logging and monitor access.
6. Use managed identities for supported applications.
7. Monitor certificate expiration dates.
8. Avoid storing credentials in scripts or GitHub repositories.
9. Separate production and development vaults.
10. Review access permissions regularly.

## 9. Azure Key Vault vs. Microsoft Sentinel

These services have different purposes.

| Service | Main purpose |
|---|---|
| Azure Key Vault | Protect passwords, certificates, and encryption keys |
| Microsoft Sentinel | Security Information and Event Management (SIEM) and security analytics |

Sentinel can help monitor security events involving your Azure environment, including relevant Key Vault activity when diagnostic logging is configured.

Sentinel does not replace Key Vault as a secrets-management service.

## 10. AZ-104 Exam Notes

Remember these concepts:

| Exam scenario | Appropriate solution |
|---|---|
| Store a VM administrator password securely | Key Vault Secret |
| Store an SSL certificate | Key Vault Certificate |
| Store an API key | Key Vault Secret |
| Protect encryption keys | Key Vault Key |
| Retrieve a password during an ARM deployment | Key Vault parameter reference |
| Control who can retrieve secrets | Azure RBAC or access policies |
| Monitor security events | Microsoft Sentinel |

### Exam memory trick

- **Secret = Password or API key**
- **Certificate = Digital identity and trust**
- **Key = Encryption and cryptographic operations**
- **RBAC = Who can access it**
- **Sentinel = Monitor security events**

## 11. Official Microsoft Documentation

- [Azure Key Vault Overview](https://learn.microsoft.com/en-us/azure/key-vault/general/overview)
- [Keys, Secrets and Certificates](https://learn.microsoft.com/en-us/azure/key-vault/general/about-keys-secrets-certificates)
- [About Key Vault Secrets](https://learn.microsoft.com/en-us/azure/key-vault/secrets/about-secrets)
- [About Key Vault Certificates](https://learn.microsoft.com/en-us/azure/key-vault/certificates/about-certificates)
- [Azure RBAC for Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/rbac-guide)
- [Key Vault ARM Template Parameters](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/key-vault-parameter)

---

**Learning objective:** Understand how Azure Key Vault can serve as a centralized secrets, certificate, and cryptographic key management service for both cloud and traditional IT infrastructure.
