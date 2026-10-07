# henry.css

A small, classless design system for responsive web apps, built to be used with coding agents. Write semantic HTML, link one stylesheet, and it looks right. No build step, no dependencies, and no utility classes. Inspired by Vercel's Geist, GitHub's Primer, and Pico.

Site and reference: https://css.henrypoydar.com/

## How it works

1. **Link the stylesheet.** One file, in cascade layers. Link it from the site, pin a version, or copy [`henry.css`](henry.css) into your project.
2. **Give your agent `DESIGN.md`.** Copy [`DESIGN.md`](DESIGN.md) into your repo and point your agent at it. It lists every element, class, token, and rule, so the markup your agent writes fits the system.
3. **Write semantic HTML.** A form, a table, a nav, or a dialog looks right with no classes. Add a class or custom element only where `DESIGN.md` lists one.

```html
<!-- Always the latest -->
<link rel="stylesheet" href="https://css.henrypoydar.com/henry.css">

<!-- Pinned to a release -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/henrypoydar/henry.css@v1.0.0/henry.css">
```

## Why it's small

Coding agents write better UI when the whole system fits in a page. henry.css keeps the surface area tiny on purpose.

- **Elements first.** Most of the stylesheet styles plain HTML elements.
- **Three custom elements.** `ui-card`, `ui-toast`, and `ui-empty`, for things HTML has no element for. They're unregistered, so there's no JavaScript.
- **A short list of classes.** Button and badge variants, tabs, and four layout helpers.
- **Tokens for every value.** Colors, spacing, type, radii, and shadows are custom properties, with dark mode built in.
- **Overrides without a fight.** Everything sits in cascade layers, so any rule in your own CSS wins.

See every element, component, and layout on the [reference site](https://css.henrypoydar.com/reference/).

## How `henry.css` is organized

Everything sits in cascade layers, declared in this order:

```css
@layer tokens, reset, base, components, utilities;
```

- **tokens:** custom properties on `:root`, plus the dark-mode block. Copy this block alone if a project wants only the values.
- **reset:** about twenty lines. Box sizing, margins, media defaults. Not normalize.css.
- **base:** bare HTML elements. This is most of the file. Text, headings, links, lists, tables, forms, `nav`, `dialog`, `details`, `figure`, `code`.
- **components:** things HTML has no element for. Three unregistered custom elements: `ui-card`, `ui-toast`, and `ui-empty`. Eight classes: button variants `.secondary`, `.ghost`, `.danger`, `.small`, the status label `.badge` with `.success`, `.warning`, `.danger`, and `.tabs`. That's the whole layer. No more.
- **utilities:** layout helpers: `.container` (and `.narrow`), `.grid` (set `--min`), `.stack` and `.cluster` (set `--gap`), `.cluster.between`, `.visually-hidden`. Optional background textures: `.pattern-dots`, `.pattern-grid`, and `.pattern-diagonal`.

Layers mean any project CSS outside a layer beats every rule in `henry.css`, so overriding the system never takes a specificity fight.

## Decisions

**Base font is Inter.** Loaded from Google Fonts by an `@import` at the top of `henry.css` with its optical size axis, so it switches to a display cut at heading sizes the way San Francisco does. The stack falls back to `system-ui`, which is a near metric match, so nothing shifts much if the font fails to load. Self-host the variable file later if the Google Fonts dependency bothers you.

**Headlines are medium weight, not bold.** Weight 650, tracking around `-0.02em` at display sizes, line-height around 1.1. Body is 400, 16px, line-height 1.5. Fewer than seven sizes in the scale.

**Paper, raised, and sunken.** The page is a warm paper tone, not white. Surfaces step from it in both directions: cards, form controls, and dialogs are raised a little lighter, and sidebars, code blocks, and empty states are sunken a little darker. The grays carry a faint warm tint at the light end, and the ink leans faintly cool, so the page reads as paper rather than a screen. Separate things with tone first, whitespace second, and a hairline last. Hairlines are translucent ink, so they sit right on any surface. A surface gets a border or a tone shift, never both. Tables get horizontal hairlines only, no vertical rules, no outer frame.

**Ink for actions, one accent for meaning.** Primary buttons are near-black. The accent, an ultramarine, is reserved for links, focus rings, selection, and checked controls, so color always carries meaning. Eleven-step neutral gray ramp, one accent ramp, semantic success/warning/danger.

**Components are custom elements, modifiers are classes.** A card, a toast, and an empty state are things that stand alone, so they're elements: `<ui-card>`, `<ui-toast>`, `<ui-empty>`. They're unregistered custom elements, which have been valid HTML and rendered as a plain block in every browser since custom elements existed. No JavaScript, no registration, and no `article` pretending to be a box. Button and badge variants stay as classes because they modify an element that already means something. Layout helpers stay as classes too, because layout is an adjective: `<ul class="grid">` keeps the list, and a wrapper element can't go between a `ul` and its `li`.

**Small radii, subtle shadows.** 6px on controls, 8px to 12px on surfaces. Shadows only for things that float: menus, dialogs, and toasts. A card doesn't need one, because it's already lighter than whatever it sits on.

**Icons are Phosphor, regular weight.** Pasted as inline SVG from [phosphoricons.com](https://phosphoricons.com/) with `aria-hidden="true"`, so they take the size and color of surrounding text and need no font or package. Label the button or link, not the icon.

**Background patterns stay quiet and optional.** Dots, grids, and diagonal lines use CSS gradients with faint neutral marks that adapt to dark mode. Apply them as utilities to an existing surface, with tokens for color, spacing, and stroke. They preserve the surface color and content opacity. Use one texture sparingly, without adding a border.

**Mobile navigation is an optional native disclosure.** Add `details` with a `summary` and `nav` inside the page header to replace crowded header links and the scrolling sidebar strip below 48rem. The menu expands in normal flow and needs no JavaScript or component class. Larger screens keep the header links and sidebar.

**Spacing on a 4px grid.** Tokens from 1 to 16, with 4 and 6 doing most of the work.

## Layout and responsiveness

Layout is where classless CSS falls short, so here's the standards we use.

- **Fluid by default.** The base layer makes type, media, tables, and forms respond before any layout rule exists. Type uses `clamp()`. A table inside a `figure` scrolls sideways instead of breaking the page.
- **Page structure from landmarks.** The base layer lays out `body > header`, `main`, `footer`, and `aside`. Header, main, footer is a page. Add an `aside` and it's an app shell: sidebar on wide screens, stacked on narrow ones. No classes.
- **Intrinsic layout classes.** `.container`, `.grid`, `.stack`, and `.cluster` respond to available space on their own, without media queries.
- **Four tiers.** Phone, tablet at 48rem, laptop at 64rem, and desktop at 90rem. The sidebar goes from a scrolling strip, to compact, to full width, and big monitors get a wider container. Touch targets grow on coarse pointers regardless of width. `DESIGN.md` has the details.
- **Container queries inside layouts.** A component that rearranges itself checks its own width, not the viewport's.

Every reference layout must resize from phone width to wide with no horizontal scroll. `bin/snap reference/<page>.html` renders a page at phone, tablet, laptop, desktop, and dark with headless Chrome.

## License

MIT. See `LICENSE`.
