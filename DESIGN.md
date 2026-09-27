# Design system: henry.css

This project uses [henry.css](https://github.com/henrypoydar/henry.css), a classless stylesheet. Read this file before you write markup or CSS. The [reference site](https://henrypoydar.github.io/henry.css/) shows every element, class, and layout.

## The rules

1. **Write semantic HTML first.** Most things need no class. Pick the element that means what you're building, and the stylesheet styles it.
2. **Use a class only when this file lists one.** There are about fifteen classes in total. Don't invent new ones in markup to restyle an element.
3. **Use tokens, never raw values.** Colors, spacing, type sizes, radii, and shadows all come from the custom properties listed below. No hex codes, no pixel spacing.
4. **One border level.** A surface gets a border or a background shift, never both, and never a bordered box inside another bordered box.
5. **Ink for actions, accent for meaning.** Buttons are near-black. The accent color is reserved for links, focus, selection, and checked controls. Don't use it for decoration.
6. **Override in your own CSS, outside any layer.** henry.css lives in cascade layers, so any unlayered rule in your stylesheet wins without a specificity fight. Don't edit henry.css in place.

## Install

Link the stylesheet in every page's `<head>`, after the viewport meta:

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://henrypoydar.github.io/henry.css/henry.css">
```

That URL always serves the latest version. To avoid surprise restyles in a shipped app, copy `henry.css` into the project, or pin a tag through jsDelivr:

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/henrypoydar/henry.css@v1.0.0/henry.css">
```

The stylesheet imports Inter from Google Fonts and falls back to the system font. Dark mode follows the operating system. Set `data-theme="light"` or `data-theme="dark"` on `<html>` to force one.

## Page structure

The stylesheet lays out direct children of `<body>` by landmark. No classes needed.

```html
<body>
  <header>…</header>   <!-- top bar: title on the left, nav on the right -->
  <aside>…</aside>     <!-- optional sidebar -->
  <main>…</main>
  <footer>…</footer>
</body>
```

- **Header, main, footer** is a page.
- **Add an `aside`** and it becomes an app shell. On phones the aside is a strip of links under the header that scrolls sideways. On tablets it's a compact sidebar beside the content, and on laptops and desktops it's full width. From tablet up, the sidebar nav stays pinned while the page scrolls.
- **Wrap `main` content in `.container`** for a centered max width, or `.container.narrow` for forms and reading.

## Elements as components

Reach for these before anything else.

| You want | Write |
| --- | --- |
| Card | `<article>` with optional `<header>` and `<footer>` |
| Page title with tagline | `<hgroup><h1>…</h1><p>…</p></hgroup>` |
| Top nav or sidebar nav | `<nav><ul><li><a>` inside `<header>` or `<aside>` |
| Current nav item | `aria-current="page"` on the link |
| Sidebar section label | `<h3>` inside the sidebar `<nav>` |
| Primary button | `<button>` |
| Link styled as a button | `<a role="button">` |
| Toggle switch | `<input type="checkbox" role="switch">` |
| Field with label | `<label for>` then the control, or the control inside the `<label>` |
| Help text under a field | `<small>` right after the control |
| Invalid field | `aria-invalid="true"` on the control, message in the following `<small>` |
| Group of checkboxes or radios | `<fieldset>` with a `<legend>` |
| Accordion | `<details><summary>` |
| Modal | `<dialog>` with optional `<header>` and `<footer>`, opened with `showModal()` |
| Table that scrolls on phones | `<table>` inside `<figure>` |
| Highlight | `<mark>` |
| Keyboard shortcut | `<kbd>` |
| Muted fine print | `<small>` |

## Classes

These are all of them.

**Buttons.** Add to `<button>` or `<a role="button">`.

- `.secondary` is outlined, for most actions in an app.
- `.ghost` has no background, for low-emphasis and icon-only actions.
- `.danger` is red, for destructive actions only.
- `.small` is the compact size. It combines with the others.

**Components.**

- `.badge` is an inline status label. Add `.success`, `.warning`, or `.danger` for meaning.
- `.tabs` goes on a `<nav>`. Mark the current link with `aria-current`.
- `.toast` is a floating message fixed to the bottom-right corner. Give it `role="status"`.
- `.empty` is an empty state. Put a heading, a sentence, and an optional button inside.

**Layout.** These respond to available space, not the viewport.

- `.container` sets a centered max width. `.container.narrow` is reading width.
- `.grid` makes columns that wrap on their own. Set `--min` to change the minimum column width, and `--gap` to change spacing.
- `.stack` spaces children vertically. Set `--gap`.
- `.cluster` lays children inline and wraps them. Set `--gap`. Add `.between` to push the ends apart.
- `.visually-hidden` hides content from sight but not from screen readers.

## Icons

Use [Phosphor](https://phosphoricons.com/), regular weight. Paste the SVG inline and add `aria-hidden="true"`. Icons take the size and color of the surrounding text.

```html
<button type="button"><svg aria-hidden="true" viewBox="0 0 256 256">…</svg> New invoice</button>
<button type="button" class="ghost" aria-label="Delete"><svg aria-hidden="true" viewBox="0 0 256 256">…</svg></button>
```

Label the button or link, not the icon. An icon-only button needs an `aria-label`.

## Tokens

Use semantic color tokens in project CSS. The raw ramps exist to build them, not to use directly.

**Color.**

| Token | Use |
| --- | --- |
| `--color-bg` | Page background |
| `--color-bg-subtle` | Sidebars, code blocks, empty states |
| `--color-bg-muted` | Hover and selected backgrounds, inline code |
| `--color-bg-emphasis` | Primary buttons, toasts |
| `--color-fg` | Body text |
| `--color-fg-muted` | Secondary text, nav links, table headers |
| `--color-fg-subtle` | Placeholders, captions, icons in nav |
| `--color-fg-on-emphasis` | Text on emphasis backgrounds |
| `--color-border` | Hairlines and card borders |
| `--color-border-strong` | Form control borders |
| `--color-accent` | Links, focus, checked controls |
| `--color-accent-subtle` | Accent tint behind selection |
| `--color-success`, `--color-warning`, `--color-danger` | Status fills |
| `--color-*-subtle` and `--color-*-fg` | Status tint and its text, as used by badges |

Every color token flips automatically in dark mode.

**Type.** `--font-sans` and `--font-mono`. Sizes are `--text-xs`, `--text-sm`, `--text-md`, `--text-lg`, `--text-xl`, `--text-2xl`, and `--text-3xl`. The two largest are fluid. Weights are `--weight-normal` (400), `--weight-medium` (500), `--weight-semibold` (600), and `--weight-heading` (650).

**Spacing.** A 4px grid: `--space-1`, `-2`, `-3`, `-4`, `-5`, `-6`, `-8`, `-10`, `-12`, and `-16`. `--space-4` and `--space-6` do most of the work. `--gutter` is the fluid page edge.

**Radius.** `--radius-sm` (4px), `--radius-md` (6px, controls), `--radius-lg` (8px, cards), `--radius-xl` (12px, dialogs), `--radius-full`.

**Shadow.** `--shadow-sm`, `--shadow-md`, `--shadow-lg`. Only things that float get a shadow: menus, dialogs, toasts.

**Layout.** `--container-max` (72rem, 80rem on desktop monitors), `--container-narrow` (40rem), `--sidebar-width` (16rem), `--sidebar-width-compact` (13rem, tablets).

## Responsiveness

The system targets phones, tablets, laptops, and desktop monitors.

| Tier | Width | What changes |
| --- | --- | --- |
| Phone | below 48rem (768px) | Sidebar becomes a scrolling link strip. Grids collapse to one column. |
| Tablet | 48rem (768px) | Compact 13rem sidebar beside the content. |
| Laptop | 64rem (1024px) | Full 16rem sidebar. |
| Desktop | 90rem (1440px) | Wider container and page gutters. |

- **Everything is fluid first.** Controls fill their container, media never overflows, and large type scales with `clamp()`.
- **Match the tiers with literal values.** Custom properties don't work in media queries, so write `@media (min-width: 48rem)`, `64rem`, or `90rem`. Don't add other breakpoints.
- **Touch is separate from width.** On a coarse pointer, buttons and nav links grow to 44px targets and checkboxes get bigger, at any screen size. Use `@media (pointer: coarse)` for your own touch adjustments, not a width query.
- **Prefer intrinsic layout** with `.grid` and `.cluster` over new media queries.
- **Use container queries** when a component in your project must rearrange itself. Check its own width, not the viewport's.
- **Check every page at phone width.** There should be no horizontal scroll.

## Don't

- Don't add borders to things inside a card, or cards inside cards.
- Don't add vertical rules or an outer frame to tables.
- Don't use the accent color for backgrounds, headings, or decoration.
- Don't make buttons blue. Primary is ink, and most app actions are `.secondary`.
- Don't use bold (700) for headings. They're 650.
- Don't add utility classes for spacing or color. Write a small unlayered rule with tokens instead.
- Don't use an icon font or another icon set.
