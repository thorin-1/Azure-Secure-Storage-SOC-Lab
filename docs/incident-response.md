# Incident Response Design

## Suspicious access workflow

```mermaid
flowchart LR
    A[Blob telemetry] --> B[KQL analytic]
    B --> C[Sentinel incident]
    C --> D[Logic App]
    D --> E[Add investigation context]
    E --> F[Analyst triage]
```

This workflow enriches an incident without automatically modifying data when the evidence is ambiguous.

## Malware containment workflow

```mermaid
flowchart TD
    A[Defender malware alert] --> B[Sentinel incident]
    B --> C[Logic App]
    C --> D{Source URI is protected container?}
    D -- No --> X[Stop safely]
    D -- Yes --> E[GET source Blob]
    E --> F[PUT copy in quarantine]
    F --> G[DELETE original]
    G --> H[Preserve quarantined evidence]
```

The source-container check is a safety control. It prevents the playbook from responding recursively to a malware alert generated when Defender scans the quarantined copy.

## Compromised SAS response

The multi-IP SAS detection intentionally requires analyst validation before revocation. Network changes, proxies, roaming, and browser-side services can create benign multi-IP behavior. Once compromise is confirmed, revoking user-delegation keys invalidates affected user-delegation SAS credentials.
