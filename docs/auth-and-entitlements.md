# Sign-in and entitlements (high level)

The connector is its own OAuth 2.1 authorization server, built on the Cloudflare Workers OAuth provider. MCP clients find it through Protected Resource Metadata (RFC 9728) and register themselves with Dynamic Client Registration, so the user never copies an API key. Sign-in uses PKCE.

## Connect and sign in

```mermaid
sequenceDiagram
  autonumber
  actor U as User
  participant C as MCP client (Claude / ChatGPT / Cursor)
  participant W as MCP Worker (OAuth + MCP)
  participant M as Email (Resend)
  participant S as Shopify Admin API
  participant D as D1 / KV

  C->>W: GET /mcp (no token)
  W-->>C: 401 + resource metadata
  C->>W: Register client (DCR)
  C->>U: Open the sign-in page
  U->>W: Enter email (or "Continue with Google")
  W->>M: Send a one-time code
  M-->>U: Code
  U->>W: Enter code
  W->>D: Mark email verified, link to user
  W->>S: Look up this email's orders
  S-->>W: Orders and line items
  W->>D: Store the computed entitlement
  W-->>U: Consent screen (terms + what the client may access, optional CRM access)
  U->>W: Allow
  W-->>C: Authorization code to redirect URI
  C->>W: Exchange code (PKCE) for tokens
  W-->>C: Access + refresh token (scoped)
```

## Every tool call

```mermaid
sequenceDiagram
  autonumber
  participant C as MCP client
  participant W as MCP Worker
  participant K as KV cache
  participant D as D1
  participant P as Product Worker (CRM, receptionist, ...)

  C->>W: POST /mcp (bearer token)
  W->>W: Validate token and scopes
  W->>K: Entitlement for this user (short cache)
  alt cache miss
    W->>D: Order-derived entitlement, grants
  end
  W->>W: Register skill tools for this plan
  W->>P: tools() over service-binding RPC (scope permitting)
  P-->>W: Tool list (failures drop only that family)
  C->>W: tools/call
  alt skill tool
    W->>D: Catalog lookup and usage log
    W-->>C: Skill content (+ MCP Apps card data)
  else product tool
    W->>P: call(user, tool, args)
    P-->>W: Result (the product checks its own plan rules)
    W-->>C: Result
  end
```

## Keeping entitlements fresh

```mermaid
sequenceDiagram
  participant S as Shopify
  participant W as MCP Worker
  participant D as D1 / KV
  participant T as Telegram (owner)

  S->>W: Order webhook (paid / updated)
  W->>W: Verify HMAC signature
  W->>S: Re-read the customer's orders
  W->>D: Recompute entitlement, clear cache
  W-->>T: Alert on failures
  Note over W,S: A scheduled reconcile re-checks recently updated orders,<br/>so a missed webhook never leaves access stale
```

## Notes

- **Fail-safe.** If Shopify cannot be reached during sign-in, the user signs in on the free tier instead of being blocked.
- **Scopes.** Skills access is the default. Access to CRM data is a separate opt-in on the consent screen, because client records are personal data.
- **First-party sites** (the skill builder and the CRM web app) reuse the same sign-in as registered public clients. Their grants are exchanged server-side and revoked right away.
- **User controls.** Users can revoke every session or delete their account from the chat.
- **Abuse protection** applies to the sign-in and catalog paths. Its details are deliberately left out of this document.
