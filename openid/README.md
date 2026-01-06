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
    A[Start] --> B[Install Keycloak]
    B --> C[Configure Realm, Clients, Users]
    C --> D[Install Nginx / Apache]
    D --> E[Install OAuth2 Proxy]
    E --> F[Configure OAuth2 Proxy with Keycloak]
    F --> G[Configure Nginx to reverse-proxy via OAuth2 Proxy]
    G --> H[Test authentication flow]
    H --> I[Done: Users can login via Keycloak]
```
