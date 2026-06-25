# ADR-0013: Use relative paths for internal callback URLs

- Status: Accepted
- Date: 2026-06-20

## Context

Password-based sign-in passed an absolute `callbackURL` constructed from `request.url`:

```ts
callbackURL: `${url.origin}/sign-in?verification=complete`
```

In production behind a reverse proxy, the `request.url` origin seen by the server (e.g. `http://internal:4173`) differs from the public origin (e.g. `https://example.com`). better-auth's global origin check middleware validates every `callbackURL` against the trusted origins list, which is derived from `process.env.ORIGIN`. When the origins do not match, the middleware rejects the request with `Invalid callbackURL`.

Email OTP sign-in did not pass a `callbackURL` at all and was unaffected. Social sign-in already used a relative path (`/`) and was also unaffected.

## Decision

Internal callback URLs that point back into the application must use relative paths:

```ts
callbackURL: `/sign-in?verification=complete`
```

This rule applies wherever the application constructs a callback URL that redirects to a route within the same deployment. External callbacks (e.g. OAuth provider redirects) may still require absolute URLs, but those are constructed by the auth library from the trusted `baseURL`, not from the incoming request.

## Consequences

- Password sign-in works consistently across all environments (local, behind a proxy, direct).
- No dependency on request URL origin matching the configured `ORIGIN`.
- Relative callback URLs are already permitted by better-auth's origin check middleware (it passes `allowRelativePaths: true` for the `callbackURL` label).
- Reviewers can enforce this by flagging any `${request.url.origin}` or `${url.origin}` usage inside callback URL construction.

## Rejected alternatives

- **Adding `trustedOrigins` to the auth config to include the internal origin** was rejected because it creates a configuration coupling between deployment infrastructure and application code that breaks when the proxy topology changes.
- **Enabling better-auth's `trustedProxyHeaders`** was rejected because it requires the proxy to set `x-forwarded-host` and `x-forwarded-proto` headers, which may not always be available or may need additional proxy configuration.
- **Constructing the callback URL from `process.env.ORIGIN`** was rejected because it duplicates configuration that already exists in the auth config and introduces an environment variable dependency into every route file that triggers auth.
