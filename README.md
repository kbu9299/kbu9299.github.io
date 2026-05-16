# Adult English Speaking Club

Hugo site for the **Adult English Speaking Club** — bilingual (English / Korean) group-discussion agendas, deployed to GitHub Pages.

- **Live site:** https://kbu9299.github.io/
- **Theme:** [PaperMod](https://github.com/adityatelange/hugo-PaperMod) (submodule), restyled to match the [design.md](https://github.com/google-labs-code/design.md) aesthetic — limestone background, deep ink type, Boston-clay accent, Public Sans + Space Grotesk fonts.

---

## Set up on a new MacBook

```bash
# 1. Install Hugo (extended) and GitHub CLI
brew install hugo gh

# 2. Authenticate with GitHub (supports 2FA via browser)
gh auth login        # choose: GitHub.com → HTTPS → login with web browser

# 3. Clone WITH submodules (theme lives in themes/PaperMod)
git clone --recurse-submodules https://github.com/kbu9299/kbu9299.github.io.git
cd kbu9299.github.io

# 4. Start the local preview server
hugo server -D -F
```

Open http://localhost:1313/ — the dev server live-reloads on save.

> If you already cloned without `--recurse-submodules`, run `git submodule update --init --recursive` to pull the theme.

---

## Daily workflow

### Create a new agenda

```bash
hugo new content/group-discussion/20260523.md
```

The filename pattern is `YYYYMMDD.md`. The archetype auto-generates a date-formatted title (e.g. `"May 23, 2026"`).

Open the file, set `draft = false` when ready, and write content. See the existing `20260516.md` as a template.

### Preview locally

```bash
hugo server -D -F    # -D builds drafts, -F builds future-dated posts
```

### Publish

```bash
git add content/group-discussion/20260523.md
git commit -m "Add 2026-05-23 agenda"
git push
```

GitHub Actions rebuilds and deploys to https://kbu9299.github.io/ in ~45–60 seconds. Watch progress at https://github.com/kbu9299/kbu9299.github.io/actions.

---

## Custom shortcodes

### `{{</* icon NAME */>}}` — inline SVG icon

```markdown
### {{</* icon book */>}} Reading
```

Available names: `book`, `note`, `chat`, `lightbulb`, `comment`.

Add new icons by editing `layouts/_partials/svg-icon.html` (one inline SVG per `else if eq $name "..."` block).

### `{{</* alert icon="lightbulb" */>}}...{{</* /alert */>}}` — editorial callout

```markdown
{{</* alert icon="lightbulb" */>}}
**Discussion Tip:** Useful conversation starters...
{{</* /alert */>}}
```

`icon` accepts the same names as above.

---

## Project layout

```
.
├── content/group-discussion/   # markdown agendas (the only content section)
├── archetypes/default.md       # template for `hugo new`
├── assets/css/extended/        # custom CSS (loaded by PaperMod)
│   └── design-md.css           # color tokens, typography, table mobile-stack
├── layouts/
│   ├── _partials/
│   │   ├── extend_head.html    # Google Fonts loading
│   │   └── svg-icon.html       # SVG icon library
│   └── _shortcodes/
│       ├── alert.html          # editorial callout
│       └── icon.html           # inline icon
├── themes/PaperMod/            # theme (git submodule)
├── hugo.toml                   # site config (title, menu, params)
└── .github/workflows/hugo.yml  # auto-deploy on push to main
```

---

## Customization quick reference

| Want to change... | Edit... |
|---|---|
| Site title | `hugo.toml` → `title` and `[params] title` |
| Homepage tagline | `hugo.toml` → `[params.homeInfoParams]` |
| Menu items | `hugo.toml` → `[[menu.main]]` blocks |
| Colors (limestone, ink, clay accent) | `assets/css/extended/design-md.css` → `:root { --theme, --primary, --accent, ... }` |
| Fonts | `layouts/_partials/extend_head.html` (Google Fonts URL) + `design-md.css` font-family rules |
| Icons | `layouts/_partials/svg-icon.html` |
| Hugo version on CI | `.github/workflows/hugo.yml` → `HUGO_VERSION` |

After any change: `git commit && git push` → live in ~1 minute.

---

## Troubleshooting

**Theme folder is empty after clone.** You forgot `--recurse-submodules`. Run `git submodule update --init --recursive`.

**Local preview shows old CSS.** Hugo dev server doesn't always pick up structural config changes. Stop (`Ctrl+C`) and restart `hugo server -D -F`.

**Push fails with `Authentication failed`.** GitHub no longer accepts passwords. Use `gh auth login` (easiest), an SSH key, or a Personal Access Token with `repo` + `workflow` scopes as the password.

**Deploy succeeded but site is 404.** Pages source must be set to "GitHub Actions" at https://github.com/kbu9299/kbu9299.github.io/settings/pages → Build and deployment → Source.
