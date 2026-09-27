# Agent instructions

This repo is henry.css, a classless design system. It has no build step, no package manager, and no dependencies. `CLAUDE.md` is a symlink to this file.

## What's here

- `henry.css` is the product. One file, in cascade layers: `tokens`, `reset`, `base`, `components`, `utilities`.
- `DESIGN.md` is the usage guide that other projects copy in. It must match `henry.css` exactly.
- `reference/` is the static reference site, served by GitHub Pages at https://henrypoydar.github.io/henry.css/. Pages serves the repo root, and the root `index.html` redirects to `reference/`.
- `bin/snap` renders a page with headless Chrome at phone, tablet, laptop, desktop, and dark.
- `README.md` explains the project and records design decisions.

## Rules for changing the system

- **Classless first.** Style an element before adding a class. A class is only for something HTML has no element for.
- **Ten component classes, no more.** The components layer is capped. Adding one means removing one or getting explicit approval.
- **Everything uses tokens.** No raw colors or spacing values outside the tokens layer.
- **Dark mode lives in two blocks.** The `prefers-color-scheme` block and the `[data-theme="dark"]` block hold the same overrides. Change both, every time.
- **Stay in the layers.** Every rule goes in one of the five layers. Unlayered CSS belongs to consuming projects, not here.
- **Four tiers, three breakpoints.** 48rem, 64rem, and 90rem, written as literals. Don't add others. Touch adjustments go in `@media (pointer: coarse)`, never a width query.
- **No build tooling.** Don't add Tailwind, PostCSS, npm, or a bundler. Plain CSS and HTML only.
- **Icons are Phosphor, regular weight,** pasted as inline SVG with `aria-hidden="true"`.

## Every change

1. Edit `henry.css`.
2. Update the reference page that shows the change, or add an example if none exists. Specimen-only styles go in that page's `<style>` block, never in `henry.css`.
3. Update `DESIGN.md` when anything a consumer uses changes: an element's behavior, a class, a token, or a breakpoint.
4. Update the decisions section of `README.md` when a design decision changes.
5. Verify visually. Run `bin/snap reference/<page>.html` and read the PNGs it writes. Check every tier and dark mode. Headless Chrome won't shrink a window below 500px, so the script loads the page in a 390px iframe for the phone shot. Don't trust a narrow `--window-size` screenshot.

## Writing style

Follow the Google developer documentation style guide for prose in docs, comments, and commits. Use plain, direct, second person, present tense, active voice, sentence-case headings, and serial commas.

## Git

- The default branch is `main`, and pulls rebase.
- Pushing to `main` deploys the reference site.
- Write short imperative commit subjects, like the existing history.
