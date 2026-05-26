# Richardson Family Cookbook

A private family cookbook website - generations of family favourites, designed to look like a printed cookbook (cream paper, olive sprigs, page frame) and printable as A4.

- **Live site**: https://richo1206.github.io/familycookbook/
- **GitHub repo**: https://github.com/Richo1206/familycookbook
- **Local working copy**: this iCloud folder (`Documents/Richardson Family Cookbook/`)

---

## How to pick this project back up in a new Cowork session

If you're starting a fresh Cowork session, paste this context to the assistant:

> I'm continuing work on the Richardson Family Cookbook static site. Source files live in my iCloud folder `Documents/Richardson Family Cookbook/` (read/write that folder). It deploys to GitHub Pages at https://richo1206.github.io/familycookbook/ from the `main` branch of https://github.com/Richo1206/familycookbook. The README in the folder has the full structure and workflow.

Then the assistant has full context and can keep adding recipes / refining the site.

---

## Site structure

```
Richardson Family Cookbook/
├─ index.html                    # Cover / home page
├─ recipe-index.html             # Index page (all recipes grouped by category)
├─ recipes/                      # One HTML file per recipe
│  ├─ _template.html             # Template - copy this for new recipes
│  ├─ tomato-relish-2024.html    # Gran's Tomato Relish (Preserves)
│  ├─ chocolate-cake.html        # Chocolate Cake (Dessert)
│  ├─ ming-ling.html             # Ali's Ming Ling (Mains)
│  ├─ yorkies.html               # Amazing Yorkies (Sides)
│  ├─ apple-cake.html            # Spiced Apple Cake (Dessert)
│  ├─ grans-mayo.html            # Gran's Mayo (Sauces & Dressings)
│  └─ pea-and-ham.html           # Pea & Ham Soup (Soup)
├─ assets/
│  ├─ css/
│  │  └─ cookbook.css            # The whole site stylesheet (A4 + mobile + print)
│  ├─ images/                    # Logo, favicons, decorative sprigs, page-frame SVG, family photo
│  └─ recipes-photos/            # One photo per recipe
├─ .gitignore                    # Excludes Graphics/, Examples/, source PSDs, font-preview.html
└─ README.md                     # This file
```

Working / source files at the root (`Graphics/`, `Examples/`, `Logo.png`, `favicon.png`, `font-preview.html`, ChatGPT exports) are git-ignored - the repo only contains the published site.

---

## Adding a new recipe (step by step)

For each new recipe, the assistant should:

1. **Read the source** (handwritten note, photo, typed page, etc.) and reformat into clear numbered method steps if needed.
2. **Copy the template**: `recipes/_template.html` -> `recipes/<recipe-slug>.html`. Slugs are lowercase, hyphenated, URL-safe (e.g. `pea-and-ham.html`, not `Pea & Ham.html`).
3. **Replace every `TEMPLATE` marker** in the new file:
   - `<title>`
   - `<meta name="description">`
   - Schema.org JSON-LD block (`name`, `description`, `recipeYield`, `recipeCategory`, `recipeCuisine`, `image`, `author`, `recipeIngredient`, `recipeInstructions`)
   - `<h1>` title (all caps)
   - Subtitle (`<em>Credit: Family Member Name</em>`)
   - Ingredients table - default is a **single quantity column** (no scaling). Only Gran's Tomato Relish uses scaling (Base + 1.5x columns).
   - Photo `<img src>` and `alt`
   - Method `<ol>` steps
4. **Drop the photo** into `assets/recipes-photos/` named to match the slug (e.g. `pea-and-ham.png`). Rename if it arrives with spaces or capitals.
5. **Wire the new recipe into the Recipes dropdown menu** - the dropdown lives inside every HTML file (index.html, recipe-index.html, _template.html, and every existing recipe page). Add an `<li>` with `<a role="menuitem" href="..." >Recipe Name <span class="menu-meta">Category</span></a>` to ALL of them in alphabetical order.
6. **Add to `recipe-index.html`** under the appropriate category section. If the category doesn't exist yet, add a new `<section class="index-category">` block. Format:
   ```html
   <li>
     <div><a href="recipes/<slug>.html">Recipe Name</a></div>
     <span class="index-yield">Credit: Contributor Name</span>
   </li>
   ```
7. **Push to GitHub** (see "Pushing changes" below).

---

## Current recipes (7) and categories (6)

| Category | Recipe | Contributor |
|---|---|---|
| Mains | Ali's Ming Ling (Chow Mein) | Alison Richardson |
| Soups | Pea & Ham Soup | Janice Richardson |
| Sides | Amazing Yorkies | Jamie Oliver |
| Sauces & Dressings | Gran's Mayo | Janice Richardson |
| Preserves | Gran's Tomato Relish | Janice Richardson |
| Desserts | Spiced Apple Cake | Richardson Family |
| Desserts | Chocolate Cake | Alison Richardson |

The Recipes dropdown lists items alphabetically by name. The Index page groups them by category in a fixed order (Mains, Soups, Sides, Sauces & Dressings, Preserves, Desserts).

---

## Pushing changes to GitHub

Open PowerShell on Windows and run:

```powershell
cd "$env:USERPROFILE\iCloudDrive\Documents\Richardson Family Cookbook"
git add .
git status                  # review what's about to be committed
git commit -m "Short description of what changed"
git push
```

GitHub Pages rebuilds the site automatically (~30 seconds). Hard-refresh https://richo1206.github.io/familycookbook/ to see changes (Ctrl/Cmd+F5 to bypass cache).

To see deployment history with commit messages: https://github.com/Richo1206/familycookbook/actions

---

## Key technical notes

- **A4 layout**: every page is sized 210mm x 297mm via `@page` and `.page` / `.cover` width/height. Print Page button (`window.print()`) prints each page as a standalone A4 sheet.
- **Print rules**: `@media print` hides the navigation bar so prints are clean cookbook pages.
- **Mobile breakpoint**: `@media (max-width: 820px)` - rearranges the cover photo to flow normally, stacks recipe grid to one column, hides the Print Page button, replaces the decorative page-frame SVG with a simple border (the A4-shaped SVG distorts when stretched to mobile aspects).
- **Cover photo top fade**: the family photo (`assets/images/family-photo.png`) has an alpha gradient baked into the PNG itself (top 25% fades to transparent). This is reliable across browsers and prints clean.
- **Cover photo bottom corners**: clipped via CSS `clip-path: path(...)` using hardcoded pixel coordinates matching the A4 inner page-frame curve.
- **Favicon**: `assets/images/favicon-32.png`, `favicon-192.png`, `favicon.ico` are linked in every page's `<head>`.
- **Logo**: `assets/images/logo.png` (R F C monogram with olive sprigs, transparent background).
- **Dropdown menu**: HTML lives in all 9 HTML files - editing one is not enough, all need updating. Sort alphabetically.

---

## Common gotchas

- **Photo filenames**: must be URL-safe. No spaces, no uppercase. Rename `Mayo.png` -> `grans-mayo.png` etc. On case-insensitive filesystems (Windows/iCloud), do a two-step rename through a `_tmp.png` to actually change case.
- **CSS file truncation**: the CSS file has been silently truncated several times during heavy editing. If the print preview shows the nav bar, or styles look broken, check the file ends properly with the `@media print` block and the heart-glyph rules.
- **iCloud sync delay**: files saved from another device may take a moment to appear locally. Check `assets/recipes-photos/` and the project root after dropping new files in.
- **CSS scaling columns** (`Base / 1.5x / 2x`) are ONLY on `tomato-relish-2024.html`. All other recipes use a single quantity column. Don't accidentally restore scaling when editing other recipes.
