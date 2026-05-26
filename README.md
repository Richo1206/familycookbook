# Richardson Family Cookbook

A private family cookbook site - generations of family favourites.

## Live site

When deployed via GitHub Pages: https://richo1206.github.io/familycookbook/

## Structure

- `index.html` - Cover / home page
- `recipe-index.html` - Recipe index (browse all recipes by category)
- `recipes/` - One HTML file per recipe
  - `_template.html` - Template for adding a new recipe (see comments inside)
- `assets/css/cookbook.css` - Shared stylesheet (A4 layout + mobile + print)
- `assets/images/` - Logo, favicons, olive sprigs, page-frame SVG, family photo
- `assets/recipes-photos/` - Photos for individual recipes

## Adding a new recipe

1. Copy `recipes/_template.html` to `recipes/<your-recipe-slug>.html`
2. Replace every `TEMPLATE` marker in the new file with your content
3. Drop your recipe photo into `assets/recipes-photos/`
4. Update the `<img src>` in your new recipe page
5. Add an entry to:
   - The Recipes dropdown `<ul class="nav-dropdown-menu">` in every HTML file
   - The category list in `recipe-index.html`

## Printing

Each page is sized A4 (210x297mm) and can be printed via the "Print Page" button in the nav (or Ctrl/Cmd+P). The navigation bar is hidden in print so each page prints as a clean cookbook page.
