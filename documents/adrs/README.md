# Architecture Decision Records

This directory records architectural decisions made during the visual and functional separation refactor.

The ADRs are:

1. [ADR-0001: Package each component in a PascalCase directory](0001-package-components-in-pascal-case-directories.md)
1. [ADR-0002: Colocate and scope component styles with CSS Modules](0002-colocate-component-styles.md)
1. [ADR-0003: Expose semantic component appearance APIs](0003-expose-semantic-component-appearance-apis.md)
1. [ADR-0004: Use semantic design tokens](0004-use-semantic-design-tokens.md)
1. [ADR-0005: Select themes through a typed runtime contract](0005-select-themes-through-a-typed-runtime-contract.md)
1. [ADR-0006: Separate controllers from themed views](0006-separate-controllers-from-themed-views.md)
1. [ADR-0007: Maintain one canonical rendering path per screen](0007-maintain-one-canonical-rendering-path-per-screen.md)
1. [ADR-0008: Resolve application branding at the root](0008-resolve-application-branding-at-the-root.md)
1. [ADR-0009: Share application header chrome across public and management areas](0009-share-application-header-chrome.md)
1. [ADR-0010: Keep shadows and gradients in the theme layer](0010-keep-shadows-and-gradients-in-the-theme-layer.md)
1. [ADR-0011: Avoid identifier shadowing in themed wrappers](0011-avoid-identifier-shadowing-in-themed-wrappers.md)
1. [ADR-0012: Pass undefined to img src instead of empty string](0012-guard-image-src-attributes-against-empty-strings.md)
1. [ADR-0013: Use relative paths for internal callback URLs](0013-use-relative-paths-for-internal-callback-urls.md)
1. [ADR-0014: Let themes declare font assets](0014-let-themes-declare-font-assets.md)
1. [ADR-0015: Standardize ESLint and Stylistic tooling](0015-standardize-eslint-and-stylistic-tooling.md)
1. [ADR-0016: Match file names to primary exports](0016-match-files-to-primary-exports.md)
1. [ADR-0017: Use uppercase acronyms in code identifiers](0017-use-uppercase-acronyms-in-code-identifiers.md)
1. [ADR-0018: Place mutable form screens on explicit routes](0018-place-mutable-form-screens-on-explicit-routes.md)
1. [ADR-0019: Keep reusable UI in the elements tier](0019-keep-reusable-ui-in-shared-component-packages.md)
1. [ADR-0020: Standardize form error surfacing](0020-standardize-form-error-surfacing.md)
1. [ADR-0021: Use US English as the standard language for the codebase](0021-use-us-english.md)
1. [ADR-0022: Auto-generate slugs from names with manual override](0022-auto-generate-slugs-from-names.md)
1. [ADR-0023: Name boolean identifiers as plain-English propositions](0023-prefix-boolean-identifiers.md)
1. [ADR-0024: Maintain zero TypeScript diagnostics](0024-maintain-zero-typescript-diagnostics.md)
1. [ADR-0025: Prefer root-cause fixes over fallback data paths](0025-prefer-root-cause-fixes-over-fallback-data-paths.md)

New ADRs should use the next four-digit number. Existing ADRs should not be renumbered.
