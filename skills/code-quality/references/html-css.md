# HTML and CSS Code Quality Standards

Use this reference when creating, modifying, reviewing, or refactoring HTML, CSS, or HTML-like templates. Apply the repository's framework, component library, browser support, formatter, linter, design system, and naming conventions first; use these standards to resolve gaps, not to rewrite established local conventions.

## Agent guidance

- Inspect repository instructions, nearby templates and components, design tokens, browser targets, and formatting/linting configuration before changing markup or styles.
- Preserve existing behavior, responsive breakpoints, interaction states, URL structure, form contracts, and visual language unless the request changes them.
- Make the smallest coherent change. Do not introduce a new CSS architecture, reset, utility system, or dependency for a localized fix.
- Prefer existing components, tokens, utilities, and patterns over creating competing one-off variants.
- Check both the default and relevant responsive states. Do not treat a visually plausible desktop result as sufficient validation.
- Keep generated files, compiled CSS, vendor files, and framework-owned output within their established boundaries.

## HTML structure and semantics

- Use the element that best describes the content or interaction: headings, paragraphs, lists, buttons, links, forms, tables, `nav`, `main`, `header`, `footer`, and `section` where appropriate.
- Maintain a logical heading hierarchy. Do not select a heading level only because it has the desired font size.
- Use links for navigation and buttons for actions. Do not use clickable `div` or `span` elements when a native control is available.
- Keep landmark regions meaningful and avoid adding redundant landmarks.
- Keep markup valid and well nested. Close elements correctly and preserve framework-specific syntax.
- Use lists for collections and tables for tabular data. Do not use layout tables or repeated line breaks for visual spacing.
- Keep content structure independent from presentation where practical so the page remains understandable without CSS.
- Use `lang`, `dir`, `charset`, viewport, and document metadata according to the application and framework conventions.
- Provide a useful document title and page-level metadata when the framework owns those concerns.
- Do not duplicate IDs. Use stable, unique IDs when labels, descriptions, or fragment links depend on them.

```html
<main>
    <h1>Account settings</h1>
    <section aria-labelledby="notification-heading">
        <h2 id="notification-heading">Notifications</h2>
        <p>Choose which updates you want to receive.</p>
    </section>
</main>
```

## Accessibility

- Make every interactive control keyboard accessible and operable without a pointer device.
- Preserve visible focus indicators. Do not remove `outline` without providing an equally clear replacement for `:focus-visible`.
- Associate every form control with a visible label. Use `aria-describedby` for help or error text and `aria-invalid` only when the value is invalid.
- Use native HTML behavior before adding ARIA. Do not add roles, states, or properties that conflict with the element's native semantics.
- Provide meaningful alternative text for informative images. Use empty `alt` text for decorative images and avoid repeating adjacent text.
- Ensure text, controls, focus indicators, and meaningful graphics have sufficient contrast under the project's accessibility target.
- Never communicate information by color alone. Include text, shape, iconography, or another perceivable distinction.
- Respect reduced-motion preferences for non-essential animation and avoid flashing content.
- Do not hide focused, hovered, or error content in a way that prevents keyboard or assistive-technology access.
- Keep accessible names accurate and concise; do not use `aria-label` to hide a useful visible label.
- Test dialogs, menus, tabs, disclosures, custom selects, and other composite widgets for focus order, focus return, escape behavior, and announced state.

## Forms and user input

- Use the correct input type, `autocomplete` value, `name`, and native constraint attributes when applicable.
- Preserve browser validation and keyboard behavior unless there is a documented reason to replace it.
- Place errors near the associated field and identify the affected field programmatically.
- Do not rely on placeholder text as a label or as the only explanation of required input.
- Keep disabled, read-only, loading, invalid, and submitted states visually and semantically distinct.
- Never disable a control solely to hide an error or prevent a user from correcting input.
- Preserve entered values and focus when validation or asynchronous updates rerender a form.

## CSS organization and selectors

- Follow the project's established organization, naming convention, cascade layers, modules, utilities, or CSS-in-JS approach.
- Keep selectors as local and predictable as the component boundary allows. Avoid broad element, descendant, or global selectors for component-specific styling.
- Prefer classes, data attributes, or framework-supported scoping over IDs for styling.
- Keep specificity low and avoid `!important`; use it only for a documented boundary such as an intentional utility override or third-party integration.
- Avoid deeply nested selectors, long selector chains, and selectors coupled to incidental DOM structure.
- Group related declarations consistently with repository conventions and keep duplicate declarations intentional.
- Remove unused styles only when the affected ownership is known. Do not delete styles based solely on a narrow search when dynamic class names or templates are involved.
- Keep state styles explicit and colocated where practical: hover, focus, active, disabled, selected, invalid, loading, and expanded states.

```css
.menuItem {
    color: var(--color-text-muted);
}

.menuItem:hover,
.menuItem:focus-visible,
.menuItem[data-selected='true'] {
    color: var(--color-text-primary);
}

.menuItem:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 2px;
}
```

## Cascade, layout, and responsive design

- Prefer normal document flow, Flexbox, and Grid for layout. Use absolute positioning only when the element is intentionally anchored to a positioned context.
- Do not use fixed heights for content that can grow unless overflow behavior is explicit and usable.
- Keep layout resilient to longer text, localization, zoom, user font settings, and narrow viewports.
- Use the project's breakpoints and design tokens. Add a breakpoint only when the content requires it, not for a particular device model.
- Prefer mobile-first rules when consistent with the repository's convention, with overrides that remain easy to follow.
- Test intermediate widths, not only named breakpoint widths.
- Avoid horizontal scrolling on the page unless it is an intentional, accessible pattern such as a data table or carousel.
- Do not use viewport units, `overflow: hidden`, or clipping to conceal layout defects.
- Use `min()`, `max()`, `clamp()`, flexible tracks, and intrinsic sizing when they improve resilience without obscuring intent.
- Keep stacking contexts understandable. Document unusual `z-index` values and avoid arbitrarily increasing them.

## Design tokens and visual consistency

- Reuse the project's colors, spacing, typography, radii, shadows, motion, and breakpoint tokens.
- Prefer semantic custom property names such as `--color-surface` over raw color names such as `--blue-500` when the token system supports them.
- Avoid introducing near-duplicate values. If a value is intentionally new, define it at the correct design-system scope and explain the need.
- Preserve typography metrics, line height, and text wrapping unless a visual change is explicitly requested.
- Use `currentColor` and inherited properties when they correctly keep icons and related content synchronized.

## Images, fonts, and media

- Set meaningful dimensions or aspect-ratio constraints for media to reduce layout shift.
- Use the framework's image and font optimization mechanisms when the repository provides them.
- Use responsive image sources and appropriate formats when performance requirements or project conventions call for them.
- Do not stretch, crop, or hide important content unintentionally with `object-fit` or background images.
- Keep decorative backgrounds out of the accessibility tree and do not use them for essential information.
- Respect `prefers-reduced-motion` and avoid autoplaying audio or disruptive video.

## Performance and maintainability

- Avoid large global stylesheets, expensive broad selectors, unnecessary animations, and layout-triggering effects for simple state changes.
- Prefer `transform` and `opacity` for animations when appropriate, and avoid animating layout-heavy properties without a reason.
- Do not add `will-change` speculatively; apply it only to a measured, short-lived performance issue.
- Keep critical rendering paths small and follow the project's bundling and code-splitting conventions.
- Avoid duplicating markup solely to support styling. Use a clear structure and pseudo-elements for purely decorative content.
- Do not encode important content or behavior in CSS generated content.

## Security and content handling

- Treat interpolated HTML, URLs, class names, style values, and attribute values as untrusted unless validated by the framework or application boundary.
- Do not use unsafe HTML injection, inline event handlers, or dynamically constructed style markup without an established sanitization and security model.
- Do not place secrets, tokens, or sensitive data in HTML comments, data attributes, CSS, source maps, or client-visible markup.
- Validate external URLs and preserve safe link behavior. Use the repository's established policy for external links and referrer handling.

## Comments and documentation

- Comment intent, constraints, browser workarounds, accessibility decisions, and design-system exceptions—not obvious declarations.
- Include the affected browser or framework limitation and, where useful, a removal condition for compatibility workarounds.
- Keep comments synchronized with the implementation and remove obsolete commented-out markup or CSS.

```css
/* Required for the embedded provider, which creates its own stacking context. */
.providerFrame {
    isolation: isolate;
}
```

## Validation checklist

- Run the repository's formatter, linter, build, and focused tests when available.
- Check semantic markup, keyboard navigation, focus visibility, accessible names, form errors, and reduced-motion behavior.
- Inspect the changed component at its supported viewport sizes and at an intermediate width.
- Verify long labels, empty states, loading states, validation errors, localization, zoom, and increased text size where relevant.
- Check that images do not cause unexpected layout shift and that responsive media remains usable.
- Confirm that no unrelated styles, global tokens, generated files, or framework conventions were changed.
- Report checks that could not run and distinguish static validation from browser, visual, and assistive-technology testing.
