# ADR-0012: Pass undefined to img src instead of empty string

- Status: Accepted
- Date: 2026-06-20

## Context

Passing an empty string `""` to an `<img src>` attribute causes the browser to re-request the current page. React Router warns at runtime:

> An empty string ("") was passed to the src attribute. This may cause the browser to download the whole page again over the network.

Several components rendered `<img src={value}>` without first coercing empty strings to `undefined`. The `ImageAsset` type (`src: string`) and hook defaults (e.g. `brandLogoSource = navigation.brand.logo.src || ''`) both allow empty strings to flow into the render tree.

## Decision

Every `<img>` tag must coerce its `src` value so that an empty string is replaced with `undefined`:

```tsx
<img src={value || undefined} alt={alt} />
```

The element remains in the tree — only the `src` attribute receives `undefined` when the value is falsy, which React omits from the DOM. This silences the warning without changing the DOM structure.

This applies at every call site. A data layer guarantee alone is insufficient because empty values can originate from defaults, loaders, hooks, or malformed data.

## Consequences

- No browser re-requests the current page from an empty `<img src="">`.
- No React Router SSR or client warning from this source.
- Reviewers can enforce this by checking that every `<img src={` uses `|| undefined` coercion.
- DOM structure is preserved (the element is not conditionally removed), which avoids layout shift or broken sibling selectors in CSS.
- The coercion is zero-cost at runtime and type-safe (`string | undefined` matches React's expected type).

## Rejected alternatives

- **Conditionally rendering the `<img>` element** (`{src ? <img/> : null}`) was rejected because removing the element can cause layout shift and breaks CSS selectors that depend on the element's presence in the tree.
- **Normalizing empty strings to `undefined` in the data layer** was rejected because it is brittle: every data source, loader, and hook would need to normalize independently, and a missed default or a new data path could reintroduce the warning silently.
- **Guarding only in shared components** was rejected because leaf components are free to render `<img>` tags directly; the guard must be at the point of rendering.
