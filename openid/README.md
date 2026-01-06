# Authentication Flow
``` mermaid
sequenceDiagram
    participant User
    participant SP as Service Provider (Apache)
    participant Mellon as Apache Mellon Module
    participant IdP as Keycloak (Identity Provider)

    User->>SP: Request access to protected resource
    SP->>Mellon: Check authentication
    Mellon-->>SP: Not authenticated, redirect to IdP
    SP->>User: Redirect to IdP login (SAML AuthRequest)
    User->>IdP: Access login page
    IdP->>User: Show login form
    User->>IdP: Submit credentials
    IdP->>IdP: Validate credentials
    IdP-->>User: Issue SAML Response
    User->>SP: Return SAML Response
    SP->>Mellon: Validate SAML Response
    Mellon-->>SP: Authentication successful
    SP-->>User: Access granted to resource
```

# High-Level Configuration steps
``` mermaid
flowchart TD
    A[Start] --> B[Install Apache HTTP Server]
    B --> C[Install Apache Mellon Module]
    C --> D[Generate SP metadata and private keys]
    D --> E[Configure Mellon module in Apache]
    E --> F[Install and configure Keycloak]
    F --> G[Create Realm, Client SAML, and Users]
    G --> H[Upload SP metadata to Keycloak]
    H --> I[Configure Apache virtual host with Mellon directives]
    I --> J[Test SAML authentication flow]
    J --> K[Done: Users can login via Keycloak using SAML]

```
