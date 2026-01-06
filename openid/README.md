# Authentication Flow
``` mermaid
sequenceDiagram
    participant User
    participant Apache as Apache/Nginx
    participant OAuth2 as OAuth2 Proxy
    participant Keycloak as Identity Provider

    User->>Apache: Request access to protected resource
    Apache->>OAuth2: Check authentication cookie/session
    OAuth2-->>Apache: Not authenticated, redirect to Keycloak
    Apache->>User: Redirect to Keycloak login page
    User->>Keycloak: Enter credentials
    Keycloak->>Keycloak: Validate credentials
    Keycloak-->>User: Issue ID token + access token
    User->>OAuth2: Return with token (OIDC callback)
    OAuth2->>OAuth2: Validate token
    OAuth2-->>Apache: Set authentication cookie
    Apache-->>User: Access granted to requested resource
```

# High-Level Configuration steps
``` mermaid
flowchart TD
    A[Start] --> B[Install Keycloak]
    B --> C[Create Realm, Clients, and Users]
    C --> D[Install Apache or Nginx]
    D --> E[Install OAuth2 Proxy]
    E --> F[Configure OAuth2 Proxy with Keycloak Client]
    F --> G[Configure Apache/Nginx to protect routes via OAuth2 Proxy]
    G --> H[Test authentication flow]
    H --> I[Done: Users can login via Keycloak using OpenID Connect]


```
