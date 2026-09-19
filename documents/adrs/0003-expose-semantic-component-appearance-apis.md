# ADR-0003: Expose semantic component appearance APIs

- Status: Accepted
- Date: 2026-06-19

## Context

Reusable components can expose styling through props such as `spacingClass`, `surfaceClass`, `roundedClass`, `widthClass`, `sizeClass`, `imageClass`, or `buttonClass`.

Those props expose utility-class and CSS implementation details to callers. Callers must understand a child component's markup to style it correctly, and visual changes require coordinating class strings across many files.

## Decision

Reusable components expose semantic appearance options rather than arbitrary class-string props.

Illustrative component APIs include:

- `SectionFrame.variant`: `default`, `category`, or `article`.
- `ActionStack.width` and `ActionStack.contentSpacing`.
- `InlineInput.size`, `invalid`, and `adorned`.
- `InlineTextArea.size`.
- `AssetPreview.variant`: `default`, `picker`, or `table`.
- `SignOutForm.variant`: `default`, `mobile-menu`, or `navigation`.
- `IconButton.tone` and `IconButton.size`.
- `AuthMessage.spacing`: `default` or `bottom`.
- `BurgerButton.visibility`: `default` or `mobile-only`.
- `CardSection.variant`: `default`, `empty`, or `stacked`.

The component maps those values to modifier classes defined in its own `styles.module.css`, typically by selecting `styles.fooBar` values and combining them with `clsx`. When a reusable component renders another visual primitive internally, such as `IconButton` rendering an icon, the reusable component also owns standard presentation details such as icon size and stroke width.

Public variant types are placed in `types.ts` and exported by the component package when callers need to reference them.

Arbitrary `className` props are not part of reusable component APIs. Internal element cloning may preserve an element's existing class name, but that detail remains private in `models.ts`.

## Consequences

Callers express intent rather than CSS implementation.

The component and its theme retain control of markup, spacing, colors, and responsive behavior.

Adding a new appearance requires updating the component's public type and stylesheet. This is more deliberate than accepting any string, but it prevents undocumented visual states.

Semantic variants can be changed internally without editing every caller.

The set of variants should remain small. A rapidly growing variant list is a signal that the component may represent multiple responsibilities and should be split.

## Rejected alternatives

Continuing to accept arbitrary class strings was rejected because it defeats visual encapsulation.

Using inline style objects was rejected because it would not provide the responsive, state, and theme capabilities expected of themed components.
