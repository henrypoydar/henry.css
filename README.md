# henry.css

A small, opinionated design system for responsive web apps. Inspired by Vercel's Geist and GitHub's Primer. Classless by default, like Pico: write semantic HTML, link the stylesheet, and it looks right. Classes exist only where HTML has no element for the thing.

Reference site: https://henrypoydar.github.io/henry.css/

## How it works

- Add `DESIGN.md` to your project and link it or copy it, so you and your coding agent knows what's what.
- Add `henry.css` to your project and link it or copy it. Write semantic HTML. Add a class only where the reference shows one. 

## How it looks and feels

See how it all looks here: 

## How `henry.css` is organized

Everything sits in cascade layers, declared in this order:

```css
@layer tokens, reset, base, components, utilities;
```

- **tokens:** custom properties on `:root`, plus the dark-mode block. Copy this block alone if a project wants only the values.
- **reset:** about twenty lines. Box sizing, margins, media defaults. Not normalize.css.
- **base:** bare HTML elements. This is most of the file. Text, headings, links, lists, tables, forms, `article`, `nav`, `dialog`, `details`, `figure`, `code`.
- **components:** classes for things HTML has no element for. Button variants `.secondary`, `.ghost`, `.danger`, `.small`. Status label `.badge` with `.success`, `.warning`, `.danger`. Then `.tabs`, `.toast`, `.empty`. That's ten. No more.
- **utilities:** layout helpers: `.container` (and `.narrow`), `.grid` (set `--min`), `.stack` and `.cluster` (set `--gap`), `.cluster.between`, `.visually-hidden`.

Layers mean any project CSS outside a layer beats every rule in `henry.css`, so overriding the system never takes a specificity fight.

## Decisions

**Base font is Inter.** Loaded from Google Fonts by an `@import` at the top of `henry.css` with its optical size axis, so it switches to a display cut at heading sizes the way San Francisco does. The stack falls back to `system-ui`, which is a near metric match, so nothing shifts much if the font fails to load. Self-host the variable file later if the Google Fonts dependency bothers you.

**Headlines are medium weight, not bold.** Weight 650, tracking around `-0.02em` at display sizes, line-height around 1.1. Body is 400, 16px, line-height 1.5. Fewer than seven sizes in the scale.

**One border level.** A surface may have a border or a background shift, never both, and never nested. Use whitespace and a single hairline to group things. Tables get horizontal hairlines only, no vertical rules, no outer frame.

**Ink for actions, one accent for meaning.** Primary buttons are near-black. The accent, an ultramarine, is reserved for links, focus rings, selection, and checked controls, so color always carries meaning. Eleven-step neutral gray ramp, one accent ramp, semantic success/warning/danger.

**Small radii, subtle shadows.** 6px on controls, 8px to 12px on surfaces. Shadows only for things that float: menus, dialogs, toasts.

**Icons are Phosphor, regular weight.** Pasted as inline SVG from [phosphoricons.com](https://phosphoricons.com/) with `aria-hidden="true"`, so they take the size and color of surrounding text and need no font or package. Label the button or link, not the icon.

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
