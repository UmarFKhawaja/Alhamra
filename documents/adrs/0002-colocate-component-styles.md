# ADR-0002: Colocate and scope component styles with CSS Modules

- Status: Accepted
- Date: 2026-06-19

## Context

Presentation was primarily expressed as long Tailwind utility strings embedded in JSX. This mixed visual implementation with component structure, obscured the markup, and made it difficult to identify which component owned a style.

The first stage of the refactor moved component CSS into colocated `styles.css` files with globally unique component-prefixed selectors. Colocation clarified ownership, but the selectors still occupied the global CSS namespace and depended on naming conventions to avoid collisions.

Vite supports CSS Modules without an additional dependency. Tailwind's `@reference` and `@apply` directives can also be used inside a CSS Module.

## Decision

Styles owned by a component live in `styles.module.css` inside that component's directory.

`component.tsx` imports the generated class map:

```ts
import styles from './styles.module.css';
```

JSX resolves component-owned class keys directly through the module export and combines them with `clsx` when needed:

```tsx
import clsx from 'clsx';

<button className={clsx(styles.button, active && styles.active)}/>
```

Component-local CSS Module names use camelCase. They stay short and semantic within the module instead of repeating the component name. For example, prefer names such as `root`, `header`, `title`, `mobileOnly`, or `withTitle` over names such as `menu-bar__header` or `menu-bar--mobile-only`.

Tailwind utilities may be composed inside the module with `@apply`. A component stylesheet uses `@reference` to access the application's Tailwind theme without duplicating it:

```css
@reference '../../../app.css';
```

Conditional visual states are represented by local modifier classes selected by the component. Tailwind utility strings are not assembled in JSX.

CSS Module class names do not need to be globally unique. Because the module already scopes them, local names should not encode component identity just to avoid collisions.

Shared global CSS is limited to genuinely application-wide concerns:

- palette and semantic token registration;
- base document styles;
- shared theme primitives;
- theme variable mappings.

A component that owns no styles does not require an empty `styles.module.css`.

## Consequences

JSX emphasizes structure, content, accessibility, and behavior instead of visual utility lists.

Changing a component's appearance normally requires editing only its colocated stylesheet.

Component selectors are locally scoped and hashed by the build. Identically named classes in different component modules cannot collide.

The component must import the module class map and use it when assigning local classes. A plain local class string will not match the generated selector, so component-owned classes should be referenced as `styles.fooBar`.

Intentional global classes, including `theme-page`, `theme-panel`, and `theme-page-backdrop`, remain global and are passed directly to `clsx` as plain strings.

Tailwind marker utilities that cannot be used with `@apply`, such as `group`, should be replaced with module-scoped parent/child selectors where practical.

Build output may contain additional CSS chunks because styles are imported by the components that use them.

Reusable components do not accept caller-owned class names. They expose semantic variants as described in ADR-0003.

## Exceptions

Theme palette files remain at `app/themes/<theme>/theme.css` because they configure an entire theme rather than one component.

`app/app.css` remains the root stylesheet for Tailwind registration, semantic token aliases, base styles, and shared theme primitives.
