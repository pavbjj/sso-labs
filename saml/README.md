# Authentication Flow
```mermaid
sequenceDiagram
    autonumber

    participant User as User Browser
    participant Apache as Apache (mod_auth_mellon)<br/>Service Provider (SP)
    participant Keycloak as Keycloak<br/>Identity Provider (IdP)

    %% Initial access
    User->>Apache: GET /protected/app
    Apache->>Apache: Check Mellon session cookie

    %% Not authenticated
    Apache-->>User: 302 Redirect to /mellon/login
    User->>Apache: GET /mellon/login

    %% Redirect to IdP
    Apache->>Keycloak: AuthnRequest (SAML)<br/>(Redirect binding)
    Keycloak->>User: Login page

    %% User authentication
    User->>Keycloak: Submit credentials
    Keycloak->>Keycloak: Validate user & create session

    %% Assertion back to SP
    Keycloak->>User: POST SAMLResponse<br/>(HTML form)
    User->>Apache: POST /mellon/postResponse

    %% Validation
    Apache->>Apache: Validate signature<br/>Validate audience<br/>Validate ACS URL
    Apache->>Apache: Create Mellon session

    %% Success
    Apache-->>User: Set Mellon cookie
    Apache-->>User: 302 Redirect to /protected/app

    %% Authorized access
    User->>Apache: GET /protected/app (with cookie)
    Apache-->>User: 200 OK (protected content)
```
# High-Level Configuration steps
```mermaid
flowchart TD
    A[Start] --> B[Install Apache HTTP Server]
    B --> C[Install Apache Mellon Module]
    C --> D[Generate SP metadata and private keys]
    D --> E[Configure Mellon module in Apache]
    E --> F[Install & configure Keycloak]
    F --> G[Create Realm, Client (SAML), Users]
    G --> H[Upload SP metadata to Keycloak]
    H --> I[Configure Apache virtual host with Mellon directives]
    I --> J[Test SAML authentication flow]
    J --> K[Done: Users can login via Keycloak using SAML]
```
