# Architecture Notes

## Control layers

| Layer | Controls |
|---|---|
| Preventive | Private containers, selected-network access, TLS, Entra authorization, RBAC, short-lived user-delegation SAS, Key Vault CMK |
| Detective | StorageBlobLogs, Defender for Storage, Defender for Cloud recommendations, KQL analytics, Sentinel incidents |
| Responsive | Sentinel automation rules, Logic Apps, automated malware quarantine, analyst-approved SAS revocation |

## Identity model

The lab intentionally avoided storing application secrets in response automation. System-assigned managed identities were granted only the Blob permissions required for the workflow. Temporary human sharing used user-delegation SAS instead of anonymous access.

## Encryption model

Azure Storage encryption was extended with a customer-managed key held in Azure Key Vault. The storage account's managed identity was used to access the key, demonstrating separation between data storage and key governance.

## Production hardening opportunities

A production implementation should consider Private Link, a Key Vault firewall/private endpoint, removal of Shared Key authorization, Conditional Access, dedicated Entra security groups, formal alert routing/on-call ownership, infrastructure as code, and continuous telemetry-health validation.
