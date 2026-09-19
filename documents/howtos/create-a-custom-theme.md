# How to create a custom theme

This guide walks through adding a custom theme to a React application with a
typed theme runtime. You'll need familiarity with React, TypeScript, CSS custom
properties, and Tailwind CSS v4.

## Prerequisites and example conventions

The application should already implement the architecture described in
[ADR-0004](../adrs/0004-use-semantic-design-tokens.md),
[ADR-0005](../adrs/0005-select-themes-through-a-typed-runtime-contract.md), and
[ADR-0014](../adrs/0014-let-themes-declare-font-assets.md):

- a shared token contract and semantic CSS utilities;
- typed shell and view props, a theme registry, and a runtime provider;
- root-level theme and color-mode validation, HTML data attributes, and loading
  of the active theme's declared stylesheets.

The examples use `app/themes/`, a `default` theme, and a new theme named `ocean`.
Paths, theme IDs, view names, and configuration keys are example conventions;
adapt them to your project. `~` denotes an alias for `app/`. The React Router,
`clsx`, branding, authentication, and feedback imports below assume equivalent
dependencies and helpers in your application. Use your project's APIs and
typed props when they differ.

---

## Architecture overview

In this architecture, a theme is the combination of two layers:

| Layer | Location | What it controls |
|---|---|---|
| **CSS tokens** | `app/themes/<name>/theme.css` | Colors, shadows, border radii, gradients, fonts — all as CSS custom properties |
| **React components** | `app/themes/<name>/` | The layout/structure of shells and views |

The CSS tokens are scoped to an HTML data attribute (`data-ui-theme="<name>"`)
set on `<html>`. Each token becomes available to every component via `var(--theme-*)`.
Tailwind v4 aliases them under shorter names (e.g. `bg-surface` resolves
`var(--theme-surface)`) so you never write raw hex values in component styles.

The React components are resolved at runtime from a typed registry. When you
switch the active theme, the application renders the corresponding component tree.

Themes use the application's existing CSS and TypeScript/React build pipeline;
they do not require a separate theme generator.

---

## Step 1 — Register your theme ID

Open `app/themes/core/contract.ts`. Add your theme's slug to the `themeIDs`
array:

```ts
export const themeIDs = ['default', 'ocean'] as const;
```

This makes `'ocean'` (or whatever name you choose) a valid `ThemeID`. The
string must be a valid CSS identifier (lowercase, no spaces).

---

## Step 2 — Create the CSS token file

Create `app/themes/<name>/theme.css`. Define **every token** in your application's
contract for each supported color mode. This example supports three selector
blocks:

1. **Light mode** — `[data-ui-theme='<name>']`
2. **Explicit dark mode** — `[data-ui-theme='<name>'][data-ui-mode='dark']`
3. **System dark mode** — `@media (prefers-color-scheme: dark) { [data-ui-theme='<name>'][data-ui-mode='system'] { ... } }`

Include all three blocks when supporting `light`, `dark`, and `system` modes so
users who explicitly choose "dark" get the correct palette regardless of OS setting.

> Use an existing complete theme, such as `app/themes/default/theme.css`, as a
> starter. Review every value and define every required token explicitly.
> Missing tokens can inherit unintended values or leave declarations invalid.

### Example token contract

The tables below illustrate a shared token contract. Use your application's
actual contract as the source of truth. For this example, every token below
must appear in all three blocks with a value appropriate for that color mode.

#### Colors

| Token | Semantic role |
|---|---|
| `--theme-canvas` | Page background |
| `--theme-canvas-accent` | Accent canvas shade |
| `--theme-surface` | Card/panel background |
| `--theme-surface-raised` | Elevated surface (e.g. dropdown) |
| `--theme-surface-muted` | Muted surface (e.g. muted input bg) |
| `--theme-surface-hover` | Row/item hover background |
| `--theme-surface-selected` | Row/item selected background |
| `--theme-surface-disabled` | Disabled control background |
| `--theme-surface-inverse` | Inverse surface (dark bg for light mode, light bg for dark mode) |
| `--theme-overlay` | Modal/overlay backdrop |
| `--theme-chrome` | Header/footer background |
| `--theme-chrome-muted` | Muted chrome variant |
| `--theme-fg` | Body text |
| `--theme-fg-heading` | Heading text |
| `--theme-fg-muted` | Secondary/muted text |
| `--theme-fg-inverse` | Text on `--theme-surface-inverse` |
| `--theme-on-chrome` | Text on `--theme-chrome` |
| `--theme-on-chrome-muted` | Muted text on chrome |
| `--theme-action` | Primary action (button bg, link color) |
| `--theme-action-hover` | Action hover state |
| `--theme-action-soft` | Soft action background (e.g. info callout) |
| `--theme-action-muted` | Muted action variant |
| `--theme-on-action` | Text/icon on action background |
| `--theme-accent` | Secondary accent color |
| `--theme-accent-hover` | Accent hover state |
| `--theme-accent-soft` | Soft accent background |
| `--theme-on-accent` | Text on accent background |
| `--theme-border-subtle` | Subtle border |
| `--theme-border-strong` | Emphasized border |
| `--theme-success` | Success color |
| `--theme-success-soft` | Soft success background |
| `--theme-on-success` | Text on success background |
| `--theme-danger` | Danger/error color |
| `--theme-danger-hover` | Danger hover state |
| `--theme-danger-soft` | Soft danger background |
| `--theme-on-danger` | Text on danger background |
| `--theme-warning` | Warning color |
| `--theme-warning-soft` | Soft warning background |
| `--theme-on-warning` | Text on warning background |

#### Border radii

| Token | Typical values |
|---|---|
| `--theme-shape-panel` | `0`, `0.75rem`, `2rem` |
| `--theme-shape-utility` | `0`, `0.25rem`, `0.375rem` |
| `--theme-shape-branded` | `0`, `0.25rem`, `2rem` |

#### Shadows

Use any valid `box-shadow` value (strings are fine — no commas-in-strings
issues since CSS custom properties preserve them).

| Token |
|---|
| `--theme-shadow-flat` |
| `--theme-shadow-panel` |
| `--theme-shadow-utility` |
| `--theme-shadow-floating` |
| `--theme-shadow-accent` |
| `--theme-shadow-showcase-surface` |
| `--theme-shadow-showcase-item` |

#### Typography

| Token | Example |
|---|---|
| `--theme-font-heading` | `'My Custom Font', ui-sans-serif` |
| `--theme-font-body` | `'My Custom Font', ui-sans-serif` |

#### Gradients

Use any valid `background-image` value (gradients, multiple gradients, etc.).

| Token | Purpose |
|---|---|
| `--theme-gradient-page-backdrop` | Page background radial gradient |
| `--theme-gradient-brand` | Brand CTA gradient |
| `--theme-gradient-brand-hover` | Brand CTA hover gradient |
| `--theme-gradient-surface-highlight` | Surface highlight gradient |
| `--theme-gradient-placeholder` | Placeholder/skeleton gradient |
| `--theme-gradient-media-scrim` | Media overlay gradient (bottom-to-top) |
| `--theme-gradient-hero-scrim` | Hero section overlay gradient |

#### Application chrome

These can reference other theme tokens. Assign separate values when the app
bar should diverge from the page surface.

| Token |
|---|
| `--app-top-bar-bg` |
| `--app-top-bar-fg` |
| `--app-menu-bar-bg` |
| `--app-menu-bar-fg` |
| `--app-menu-bar-accent` |
| `--app-menu-bar-muted` |

### Example: minimal theme.css skeleton

```css
[data-ui-theme='ocean'] {
    --theme-canvas: #f0f7ff;
    --theme-canvas-accent: #e0edfa;
    /* ... all tokens above ... */
    --app-menu-bar-muted: var(--theme-fg-muted);
}

[data-ui-theme='ocean'][data-ui-mode='dark'] {
    --theme-canvas: #0a1628;
    --theme-canvas-accent: #13203b;
    /* ... all tokens above ... */
    --app-menu-bar-muted: var(--theme-fg-muted);
}

@media (prefers-color-scheme: dark) {
    [data-ui-theme='ocean'][data-ui-mode='system'] {
        --theme-canvas: #0a1628;
        --theme-canvas-accent: #13203b;
        /* ... all tokens above ... */
        --app-menu-bar-muted: var(--theme-fg-muted);
    }
}
```

---

## Step 3 — Create theme components

A theme must supply a component for each shell and view in the application's
contract. Use an existing theme as a reference for the required props, then
create implementations owned by the new theme. Reuse theme-agnostic helpers
and controls from outside the theme layer.

### Directory structure

```
app/themes/ocean/
  AuthShell/
    index.ts
    component.tsx
    props.ts
    styles.module.css   (only if this component has custom styles)
  ManageShell/
    index.ts
    component.tsx
    props.ts
    styles.module.css
  HomeView/
    index.ts
    component.tsx
    props.ts
  ArticleView/
    index.ts
    component.tsx
    props.ts
    styles.module.css
  AuthView/
    index.ts
    component.tsx
    props.ts
  ProfileView/
    index.ts
    component.tsx
    props.ts
```

These shells and views are examples; implement the entries in your own theme
contract. Each component's `index.ts` exports its component and public props,
following [ADR-0001](../adrs/0001-package-components-in-pascal-case-directories.md):

```ts
// app/themes/ocean/AuthShell/index.ts
export { AuthShell } from './component';
export type { AuthShellProps } from './props';
```

### Component patterns

#### Shells (AuthShell, ManageShell) — full custom layout

These define the page chrome around a route. Build them from scratch using
Tailwind semantic utilities:

```tsx
// app/themes/ocean/AuthShell/component.tsx
import { Link } from 'react-router';
import clsx from 'clsx';
import { BrandImage } from '~/elements/BrandImage';
import { hasModeAwareImageAsset } from '~/lib/branding';
import type { AuthShellProps } from './props';
import styles from './styles.module.css';

export function AuthShell({
  branding,
  title,
  description,
  footerText,
  footerLinkHref,
  footerLinkLabel,
  children
}: AuthShellProps) {
  return (
    <div className={clsx('theme-page', styles.root)}>
      <header className={styles.header}>
        <Link to={branding.href}>
          {hasModeAwareImageAsset(branding.logo) ? (
            <BrandImage asset={branding.logo} altFallback={branding.name} />
          ) : (
            <span>{branding.name}</span>
          )}
        </Link>
      </header>
      <main className={styles.main}>
        <h1>{title}</h1>
        <p>{description}</p>
        {children}
        {footerText && (
          <p>{footerText} <Link to={footerLinkHref!}>{footerLinkLabel}</Link></p>
        )}
      </main>
    </div>
  );
}
```

```css
/* app/themes/ocean/AuthShell/styles.module.css */
@reference '../../../app.css';

.root {
  @apply flex min-h-screen flex-col items-center justify-center px-4;
}

.header {
  @apply mb-10;
}

.main {
  @apply w-full max-w-md;
}
/* ... etc */
```

The `props.ts` file just re-exports the contract type:

```ts
// app/themes/ocean/AuthShell/props.ts
export type { AuthShellProps } from '../../core/contract';
```

#### Contract views (AuthView, ProfileView) — theme-owned composition

These views are part of the theme contract, so each theme owns its own
implementation. They may compose theme-agnostic helpers from
`app/elements` or feature packages, but they must not delegate to a
cross-theme implementation in `app/themes/shared/` or branch on a `variant`
prop.

```tsx
// app/themes/ocean/AuthView/component.tsx
import { Form } from 'react-router';
import { SocialSignInButton } from '~/components/auth/SocialSignInButton';
import { ToastTrigger } from '~/components/feedback/ToastTrigger';
import type { AuthViewProps } from './props';

export function AuthView({ model }: AuthViewProps) {
  return (
    <Form method="post">
      {/* theme-owned auth form layout */}
      <ToastTrigger
        message={model.feedback.message}
        tone={model.feedback.tone}
        title={model.feedback.title}
        trigger={model.feedback.trigger}
      />
      <SocialSignInButton provider="google" />
    </Form>
  );
}
```

The `props.ts` file can re-export the contract type directly:

```ts
// app/themes/ocean/AuthView/props.ts
export type { AuthViewProps } from '../../core/contract';
```

If the same helper is useful across themes, move that helper out of the theme
layer so it stays theme-agnostic.

#### Full views (HomeView, ArticleView) — theme-owned layout

Apply the same ownership rule to full-page views. Each theme controls the
layout and composition of its contract views. Extract reusable content
renderers or widgets into theme-agnostic component or feature packages, then
compose them inside each theme's view. Keep workflow behavior in controllers,
as described in [ADR-0006](../adrs/0006-separate-controllers-from-themed-views.md).

---

## Step 4 — Create the theme index file

```ts
// app/themes/ocean/index.ts
import type { ThemeDefinition } from '../core/contract';
import { ArticleView } from './ArticleView';
import { AuthShell } from './AuthShell';
import { HomeView } from './HomeView';
import { ManageShell } from './ManageShell';
import { AuthView } from './AuthView';
import { ProfileView } from './ProfileView';

export const oceanTheme: ThemeDefinition = {
  id: 'ocean',
  assets: {
    stylesheets: []  // add font-face stylesheet paths here if needed
  },
  shells: {
    Auth: AuthShell,
    Manage: ManageShell
  },
  views: {
    Home: HomeView,
    Article: ArticleView,
    Auth: AuthView,
    Profile: ProfileView
  }
};
```

If your theme uses a custom font, add the stylesheet path to `assets.stylesheets`:

```ts
assets: {
  stylesheets: ['/fonts/my-custom-font/stylesheet.css']
}
```

In this example, the font-face stylesheet lives under
`public/fonts/my-custom-font/` and is served at the URL above. Adapt the
location to your application's static asset pipeline. Reference the font
family by name in your `theme.css` tokens:

```css
--theme-font-heading: 'My Custom Font', ui-sans-serif;
--theme-font-body: 'My Custom Font', ui-sans-serif;
```

---

## Step 5 — Wire the theme into the application

### Register in the theme registry

```ts
// app/themes/core/registry.ts
import { defaultTheme } from '../default';
import { oceanTheme } from '../ocean';                    // ← add
import type { ThemeDefinition, ThemeID } from './contract';

const themes: Record<ThemeID, ThemeDefinition> = {
  default: defaultTheme,
  ocean: oceanTheme                                       // ← add
};

export function getTheme(themeID: ThemeID): ThemeDefinition {
  return themes[themeID];
}
```

### Import the CSS

```css
/* app/app.css */
@import 'tailwindcss';
@import './themes/default/theme.css';
@import './themes/ocean/theme.css';   /* ← add */
```

> The order of imports does not matter — each theme's tokens are scoped to
> its own `[data-ui-theme]` selector, so there's no specificity conflict.

---

## Step 6 — Activate the theme

Configure the active theme and mode through your application's validated
configuration. Following the example environment-variable convention, a
`.env` file contains:

```dotenv
UI_THEME=ocean
UI_MODE=system
```

Valid values for `UI_MODE` are `'light'`, `'dark'`, or `'system'`.

The root runtime must read and validate these values, resolve the selected
theme, and write `data-ui-theme` and `data-ui-mode` to `<html>`. Setting an
environment variable alone does not implement that wiring. Use the deployment
system's configuration mechanism when a `.env` file is not used.

---

## How components consume theme tokens

Component styles use Tailwind `@apply` with the semantic utilities registered
in `app/app.css`. You never write a raw color or shadow — always use a token.

### In a CSS Module (`styles.module.css`)

```css
@reference '../../../app.css';

.myCard {
  @apply border border-border-subtle bg-surface shadow-panel theme-shape-panel;
}

.myButton {
  @apply bg-action text-on-action font-semibold transition hover:bg-action-hover;
}
```

### In JSX with the global classes

If the application defines global layout helpers, components can use them
alongside semantic utilities. For example:

```tsx
<div className="theme-page">           {/* full-page layout */}
  <div className="theme-page-backdrop" />  {/* page background gradient */}
  <div className="theme-panel">        {/* card/panel surface */}
    ...
  </div>
</div>
```

### Example Tailwind semantic utilities

Register the utilities corresponding to your token contract in the application's
shared stylesheet; these names are examples, not built-in Tailwind utilities.

| Prefix | Examples |
|---|---|
| Colors | `bg-surface`, `text-fg-heading`, `border-border-subtle`, `bg-action`, `text-on-action`, `bg-danger-soft`, `text-success` |
| Fonts | `font-heading`, `font-body` |
| Shadows | `shadow-panel`, `shadow-utility`, `shadow-floating`, `shadow-accent` |
| Shapes | `theme-shape-panel`, `theme-shape-utility`, `theme-shape-branded` |
| Gradients | `theme-gradient-brand`, `theme-gradient-surface-highlight`, `theme-gradient-media-scrim`, `theme-gradient-hero-scrim` |

---

## Conventions

- **All visual values live in `theme.css`.** Components must not contain raw
  hex codes, box-shadows, or gradient definitions. If a component needs a
  special color, it belongs in the token contract.
- **Use `clsx` for combining classes** in JSX. Import it from the `clsx`
  package if your project uses it, or use the project's class-composition helper.
- **Use `@reference '../../../app.css'`** at the top of every CSS module so
  Tailwind utilities resolve correctly. Adjust the `..` depth based on your
  file's location.
- **Theme components receive props from the contract types.** Shells get
  `branding` (name, href, logos/icons) plus layout-specific props. Views get
  their data model. Do not change the prop interface — the calling code in
  pages/layouts expects the contract types exactly.
- **Shared components expose semantic appearance props** such as `tone` or
  `emphasis` and consume theme tokens. Keep theme-specific composition in the
  theme's own views instead of branching shared controls on theme IDs.
- **Test both light and dark.** Set `UI_MODE` to `'light'`, `'dark'`, and
  `'system'` and verify all three render correctly.

---

## Troubleshooting

**Colors look wrong or are missing in some mode.** Check that every token
appears in all three selector blocks in `theme.css`. A missing token in the
dark block means the light value persists.

**Components look identical to the default theme.** Verify your theme is
registered in `registry.ts`, the CSS is imported in `app/app.css`, and
`UI_THEME` is set to your theme's ID.

**Tailwind utilities like `bg-surface` don't resolve.** Make sure every
CSS module starts with `@reference '../../../app.css';` (adjust path as
needed). Without this, Tailwind can't resolve the theme tokens at build time.

**Custom font doesn't load.** Confirm the font-face stylesheet and font files
are served by your static asset pipeline, the stylesheet URL is listed in
`assets.stylesheets`, and the root renders its stylesheet link for the active
theme. Check that the `--theme-font-*` tokens use the `font-family` name declared
in the `@font-face` rule.
