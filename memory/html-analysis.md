---
name: html-analysis-md
description: Analysis of index.html image paths and CSS for light/dark mode and hover states
metadata:
  type: reference
---

# HTML File Analysis: index.html

## Image Directory Summary

**Files found in `images/`:**
- `images/dark/` - 29 files (SVG and PNG icons in dark theme colors)
- `images/color/` - 7 files (color variants for contact logos)
- Root `images/` - various project previews, audio, PDF

**Local image files that exist and are referenced in HTML:**
- `images/dark/java.svg` ✓
- `images/dark/python.svg` ✓
- `images/dark/javascript.svg` ✓
- `images/dark/html5.svg` ✓
- `images/dark/css.svg` ✓
- `images/dark/tailwindcss.svg` ✓
- `images/dark/mysql.svg` ✓
- `images/dark/vscode.svg` ✓
- `images/dark/git.svg` ✓
- `images/dark/github.svg` ✓
- `images/dark/npm.svg` ✓
- `images/ecommerce-preview.png` ✓
- `images/todo-preview.png` ✓
- `images/sathar.mp3` ✓
- `images/resume.pdf` ✓

**Color variant files that exist but are NOT referenced in HTML skill icons:**
- `images/color/vscode.svg` ✓
- `images/color/gmail-color.png` ✓
- `images/color/linkedin-color.png` ✓
- `images/color/instagram-color.png` ✓
- `images/color/github-color-dark.png` ✓

## Image Path Analysis

### Skills Section Icons

The HTML uses a two-image swap pattern for skill icons (lines 347-362):

**Structure per skill item:**
```html
<span class="ic"><img class="ic-mono" src="images/dark/java.svg"><img class="ic-color" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/java/java-original.svg"></span>
```

**CSS hover behavior (lines 353-356):**
```css
@media (hover: hover) and (pointer: fine) {
    .ic:hover .ic-mono { opacity: 0; }
    .ic:hover .ic-color { opacity: 1; }
}
```

**CSS default (non-hover, lines 359-362):**
```css
@media (max-width: 768px), (hover: none), (pointer: coarse) {
    .ic .ic-mono { opacity: 0; }
    .ic .ic-color { opacity: 1; }
}
```

**Result:** On desktop with mouse (fine pointer), icons start as mono (grayscale) and turn color on hover. On phones/touch, color is always shown.

**All skill icon mono files use `images/dark/...`** - all 11 exist ✓
**All skill icon color URLs are CDN devicons** - these are web URLs that should load

### Hobby Section Icons

**Chess king (lines 860-865):**
```html
<img class="hobby-icon mono-icon" src="https://api.iconify.design/mdi/chess-king.svg">
<img class="hobby-icon chess-hover" src="https://cdn-icons-png.flaticon.com/512/657/657896.png">
```
- **Mono:** Web URL (Iconify mdi chess-king) - should load
- **Hover:** Web URL (Flaticon PNG) - should load
- **Hover CSS** (lines 367-370): On desktop hover, `.chess-item .hobby-icon.chess-hover { display: block; }` and base icon hidden

**Video Editing (lines 867-872):**
```html
<img class="hobby-icon" src="https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/svg/1f3ac.svg">
```
- Single web URL (twemoji 1f3ac - video camera) - should load

**Cubing (lines 873-878):**
```html
<img class="hobby-icon" src="https://cdn-icons-png.flaticon.com/512/16507/16507149.png">
```
- Single web URL (Flaticon PNG) - should load

### Contact Section Icons

All use Wikipedia web URLs + local mono logos:

**Gmail (lines 888-893):**
- Mono: `<span class="brand brand-gmail mono-logo">` with `--logo: url('https://cdn.jsdelivr.net/npm/simple-icons@latest/icons/anthropic.svg')` — wait, no. Let me re-read.

Actually, looking more carefully at the contact section:

```html
<span class="contact-icon-box">
    <span class="brand brand-gmail mono-logo" role="img" aria-label="Gmail"></span>
    <img class="color-logo" src="https://upload.wikimedia.org/wikipedia/commons/7/7e/Gmail_icon_%282020%29.svg" alt="Gmail logo">
</span>
```

The `.brand` class uses `--logo` CSS custom property for masking. The `mono-logo` span has a `--logo` set to a simple-icons URL, and the `color-logo` img has a direct Wikipedia URL.

**CSS for contact hover (lines 561-564):**
```css
@media (hover: hover) and (pointer: fine) {
  .contact-icon-link:hover .mono-logo { display: none; }
  .contact-icon-link:hover .color-logo { display: block; }
}
```

**Default (lines 565-568):**
```css
@media (max-width: 768px), (hover: none), (pointer: coarse) {
  .contact-icon-box .mono-logo { display: none; }
  .contact-icon-box .color-logo { display: block; }
}
```

### Project Preview Images

- `images/ecommerce-preview.png` ✓ (exists)
- `images/todo-preview.png` ✓ (exists)

### Other Media

- `images/sathar.mp3` ✓ (exists for pronunciation feature)
- `images/resume.pdf` ✓ (referenced in education but not directly in image tags)

## Likely Issues Found

1. **Color variant images exist but aren't used for skill icons** - The `images/color/` folder has 7 color logo files (vscode, gmail-color, linkedin-color, instagram-color, github-color-dark, etc.) but the skill sections use `images/dark/...` for mono and CDN devicons for color. The color folder files may be intended for a different purpose or were left over from a design change.

2. **All local `images/dark/...` SVG files exist** - Good, no missing mono icons.

3. **All web CDN URLs for devicons should load** - These are from jsDelivr which is generally reliable.

4. **Hobby icons use web URLs** - All are from public CDNs (Iconify, Flaticon, Twitter/Twemoji) which should load.

5. **Contact icons use Wikipedia web URLs** - Should load, but are external dependencies.

6. **No local image paths are broken** - All `images/dark/...` references have corresponding files.

## Summary Table (Mental Prep)

| Section | Icon/Item | Light Mode Result | Dark Mode Result | Hover Result | Phone Result | Problem | Likely Cause |
|---|---|---|---|---|---|---|---|
| Skills | Java mono icon | Visible (gray) | Visible (gray) | Turns color on hover | Shows color by default | None - all files exist | CSS two-image swap pattern |
| Skills | Color icon | Visible | Visible | Mono hidden on hover | Shows mono by default | None - CDN URLs | CSS opacity swap |
| Hobbies | Chess king | Visible | Visible | Hover shows flaticon, hides base | Shows base only on touch | None | Web URLs load |
| Contact | Gmail mono | Visible | Visible | Color shows on hover | Color shows by default on phone | None | Web Wikipedia URLs |