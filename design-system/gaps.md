# Gaps and notes

What I couldn't build exactly, what I had to guess, and what's worth knowing.
Source: Figma file **Project File** (`GFAqHOEWDyAwKiheiuw4RC`), read on 18 Oct 2026.

---

## 1. Contrast problems on navy (fix these first)

I checked 102 colour pairs against WCAG AA in both Light and Dark mode.
**94 pass. All 8 failures are on `surface-inverse`** (the navy nav and the Posters page).

This started when `red/500`–`red/800` were changed to darker reds (for example `red/500` went from `#D14C5B` to `#A6313F`). Dark red is lovely on white but disappears on navy.

| What | Mode | Contrast | Needs |
|---|---|---|---|
| "contact" button edge on the navy nav (`action-primary-default`) | Light | 2.54 | 3.0 |
| Button hover on navy (`action-primary-hover`) | Light | 2.93 | 3.0 |
| Focus ring on the navy nav (`border-focus`) | Light | 2.93 | 3.0 |
| "Posters" heading on navy (`text-display`) | Light | 2.93 | 3.0 |
| The same four in Dark mode (inverse surface is light there) | Dark | 2.10–2.97 | 3.0 |

**Suggested fix:** add a small "on inverse" set of tokens for things that sit on navy, for example
`action/primary/on-inverse` → `red/400`, `border/focus-inverse` → `white`, `text/display-inverse` → `red/400`.
`red/400` on navy is 6.3:1. I didn't change your Figma values: they were your edit.

Also: in Light mode the primary hover (`red/500`, `#A6313F`) is only 1.16:1 different from the default (`red/600`, `#9D2232`). The hover barely shows. Pointing hover at `red/700` would make it visible.

Body text, muted text, links and buttons on light surfaces all pass.

## 2. Things that aren't Figma variables yet

I added these in `css/tokens.css` and marked them `NOT IN FIGMA`:

- **Spacing** (`--space-100` … `--space-900`). The file has no spacing variables of its own. The 64px section padding comes from variables in a linked library (`padding XL`, `padding-left-right`), which I can't edit and which won't travel with this file. Every other gap (24, 40, 20px…) is typed in by hand.
- **Corner radius** (`--radius-button: 8px`).
- **Line heights.** Set per text layer in Figma (120%, 125%, 145%, 170%), not as variables.
- **Font weight "medium" (500).** Nav links and button labels use Poppins Medium, but only regular, semibold and bold exist as variables.

**Suggestion:** add `space/*`, `radius/*` and `line-height/*` collections in Figma so these come from one source.

## 3. Where the code differs from the Figma components

- **Most Figma components don't use your text styles.** The text styles (XL Display, Display, Headline, Body…) exist, but the component layers mostly have sizes typed in (for example the Hero headline is 80 / 64 / 50px, which matches no size token). Only the "Posters" heading uses a style. Worth fixing before the remaining sections are converted (see section 5).
- **Text style line heights are fixed pixels** (for example Display is 72px with an 80px line height). Because the sizes change per breakpoint, I converted them to ratios (80 ÷ 72 ≈ 1.1) so they scale.
- **Breakpoints.** Figma's modes say lg ">1280", md ">800", sm "<800". In CSS, md starts at 1280px and below, and sm at 799px and below. At exactly 1280px wide you see the md sizes.
- **The menu icon:** the Figma "menu" component is two loose vectors. I drew it in CSS (two lines that become an ×), and the menu opens with `<details>` so no JavaScript is needed.
- **The poster layout** is a 12-column grid that roughly follows the draft; the Figma draft overlaps posters freely, and overlapping doesn't adapt well to small screens.
- **Nav links are placeholders:** "about.", "work." and "contact" point to `#about`, `#work` and `#contact`, which don't exist yet. "blog." goes to the Posters page.

## 4. Dark mode behaviour to be aware of

`surface-inverse` flips in Dark mode (navy → near-white). That means **the nav and the whole Posters page turn light in Dark mode.** That's how the token is set up in Figma, and the code follows it. If the navy bar should stay navy in both modes, give `surface/inverse` the same value in both modes and add matching text tokens.

Light is the default here (it's Figma's default mode). The page doesn't follow the computer's dark-mode setting; use the switch, or add `data-theme="dark"` to `<html>`.

## 5. Not included

- **Hero, ProjectCard, About, Skills and Footer.** These come from the pre-existing portfolio template and only hold placeholder content, so they were left out on purpose. Their colours in Figma are already linked to the semantic tokens, so converting them later only needs the CSS.
- The **Thumbnail** and **prompt** frames, and the loose "about." / "Display" type studies on the canvas: course material, not part of the site.
- The **device mockup** on the Thumbnail (a third-party component with its own colours).
- **Images are exports from Figma** (JPEG, compressed): about 1.4 MB in total. For a real site you'd want smaller, responsive versions (`srcset`) and real alt text written by you; mine describe what I could see.
- **No JavaScript** anywhere, by design. The theme switch uses CSS `:has()`, which works in all current browsers but not in very old ones (those just stay in Light mode).
