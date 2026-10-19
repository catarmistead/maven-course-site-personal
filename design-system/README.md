# My Design System

Plain HTML and CSS built from the Figma file **Project File** using the Figma MCP. No frameworks, no build step, no JavaScript.

## Open it

Double-click `index.html`, or run a tiny local server from this folder:

```bash
python3 -m http.server 8080
```

then open <http://localhost:8080>.

## What's here

| File | What it is |
|---|---|
| `index.html` | The design system documentation: colours, type, spacing, Button, Navigation and Posters |
| `posters.html` | Example page built only from the system, on the navy surface (the Figma "Posters" draft) |
| `css/tokens.css` | Every Figma variable as a CSS custom property, with Light/Dark and responsive sizes |
| `css/components.css` | One section per Figma component, using tokens only |
| `css/docs.css` | Layout for `index.html` only |
| `assets/images/` | Logo and posters exported from Figma |
| `gaps.md` | What's missing, approximated or worth fixing, **including 8 contrast failures on navy** |

## Using it on a page

```html
<link rel="stylesheet" href="css/tokens.css">
<link rel="stylesheet" href="css/components.css">

<a class="button button--primary" href="#">contact</a>
```

Change how things look by editing the tokens in `css/tokens.css`. Figma's `text/default` is `var(--text-default)` here.
