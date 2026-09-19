# ADR-0013: Use relative paths for internal callback URLs

- Status: Accepted
- Date: 2026-06-20

## Context

Authentication flows can construct an absolute callback URL from the incoming request. For example:

```ts
callbackURL: `${url.origin}/sign-in?verification=complete`
```

Behind a reverse proxy, the origin in `request.url` seen by the server (e.g. `http://internal:4173`) can differ from the public origin (e.g. `https://example.com`). An authentication library that validates callback URLs against configured trusted origins can reject a callback constructed from the internal origin.

This guideline applies when the authentication library supports application-relative callback paths and resolves them against a configured public base URL.

## Decision

Internal callback URLs that point back into the application must use root-relative paths when the authentication library supports them:

```ts
callbackURL: `/sign-in?verification=complete`
```

This rule applies wherever the application constructs a supported callback URL that redirects to a route within the same deployment. External callbacks (e.g. OAuth provider redirects) may require absolute URLs; construct those through the authentication library's configured public base URL. If a library requires an absolute internal callback URL, centralize its construction using the same trusted base URL instead of the incoming request origin.

## Consequences

- Internal callback construction works consistently across local, proxied, and direct deployments.
- Callback construction does not depend on the request URL origin matching the configured public origin.
- Adoption requires checking the authentication library's support for relative callbacks and configuring its trusted public base URL.
- Reviewers can enforce this by flagging any `${request.url.origin}` or `${url.origin}` usage inside callback URL construction.

## Rejected alternatives

- **Adding the internal origin to the authentication library's trusted origins** was rejected because it creates a configuration coupling between deployment infrastructure and application code that breaks when the proxy topology changes.
- **Relying on forwarded proxy headers solely to construct internal callbacks** was rejected because it adds proxy configuration and header-trust dependencies when supported relative paths already express the destination.
- **Constructing absolute URLs from an environment variable in every route** was rejected because it duplicates authentication configuration and spreads deployment-specific dependencies across callers.
