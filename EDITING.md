# Editing this site

What each file does, and how a change gets from your laptop to the web.

## How it works

You write plain text files. Jekyll wraps each one in the shared design and turns
it into a web page. Content and design stay separate, so you can rewrite every
word without touching the layout, and change every colour without touching the
words.

This site has 18 files. You will realistically ever edit **four** of them.

## The four files

| File | What it controls |
| --- | --- |
| `index.md` | The homepage text. |
| `_config.yml` | Your name, role, affiliation, email, profile links. Change your email here and it changes everywhere at once. |
| `_data/navigation.yml` | The menu along the top. Currently empty, which is why no menu shows. |
| `assets/css/style.css` | Colours and fonts — all of them, in the block at the very top. |

**Safe to ignore:** `_layouts/` `_includes/` `Gemfile` `Gemfile.lock` `.github/`
`404.html` `_site/`

## Front matter

Open `index.md` and the first thing you see is this:

```
---
layout: home
---

I am a postdoctoral researcher in physics at Harvard University.
```

The fenced part is settings, not content. `layout: home` tells Jekyll which
template to wrap this page in. Below the closing `---` is your actual writing.

> **Keep both `---` lines.** They aren't decoration. Delete them and Jekyll stops
> treating the file as a page — it gets copied to the site untouched, raw text
> and all.

## Common edits

### Reword the homepage

Edit `index.md`, below the front matter. Write normally; blank lines separate
paragraphs.

### Change your email, or add a link

In `_config.yml`, under `profiles`:

```yaml
profiles:
  email:    "erincrawley@g.harvard.edu"
  scholar:  "https://scholar.google.com/..."
  orcid:    "https://orcid.org/0000-0000-0000-0000"   # <- added
  github:   "https://github.com/erin-crawley"
```

An ORCID link now appears in the footer and on the homepage. Leave a value as
`""` and that link simply doesn't render.

### Add a new page

Create `research.md` in the main folder:

```
---
layout: page
title: "Research"
permalink: /research/
---

Your first paragraph here.

## A subheading

More writing.
```

Then add it to the menu in `_data/navigation.yml`:

```yaml
main:
  - title: "Research"
    url: /research/
```

The menu appears in the header automatically. It's a separate step on purpose —
a half-finished page can exist without being linked to.

### Write a blog post

Create a file in `_posts/` named `2026-08-12-my-first-post.md`. The date in the
filename is required and sets the post's URL.

```
---
title: "My first post"
---

Post body goes here.
```

### Change a colour

Top of `assets/css/style.css`. Every colour on the site is defined once, there:

```css
:root {
  --bg:     #F3F7F4;   /* page background */
  --band:   #14332F;   /* header, hero, footer */
  --accent: #43713C;   /* links */
}
```

Change `--accent` and every link on every page changes together.

## Publishing a change

**1. Preview it locally**

```
cd ~/Library/CloudStorage/OneDrive-Personal/Documents/Website/erin-crawley.github.io
bundle exec jekyll serve --livereload
```

Open <http://localhost:4000>. Save a file and the browser refreshes itself.
`Ctrl+C` stops it.

**2. Save the change to your history**

```
git add -A
git commit -m "Add research page"
```

The message is a note to your future self about what changed.

**3. Send it to GitHub**

```
git push
```

**4. Wait about forty seconds**

GitHub rebuilds the site and publishes it. Watch progress on the
[Actions tab](https://github.com/erin-crawley/erin-crawley.github.io/actions) —
a green tick means it's live.

## Things that will bite you

**Indentation in `.yml` files is meaningful.** Spaces only, never tabs, and the
amount matters. A misaligned line breaks the build. If a change to `_config.yml`
or `navigation.yml` fails, indentation is the first suspect.

**Never edit anything in `_site/`.** That folder is generated fresh on every
build. Your edits there are overwritten and lost.

**Post filenames must start with a date.** `2026-08-12-title.md`. Miss the date
and the post silently won't appear — no error, it just isn't there.

**Changes to `_config.yml` need a restart.** Every other file live-reloads. That
one doesn't: stop the preview with `Ctrl+C` and start it again.

**A page isn't linked just because it exists.** Creating `research.md` makes the
page reachable at its URL, but it won't appear in the menu until you add it to
`_data/navigation.yml`.

## If something breaks

Nothing is lost. Every previous version is in git:

```
git log --oneline        # see the history
git checkout <commit>    # look at an older version
git restore <file>       # undo uncommitted changes to one file
```

The original Academic Pages template site is preserved in full at the
`pre-rebuild` tag.

## Toolchain notes

This site uses **Jekyll 4**, not the `github-pages` gem. That gem pins Jekyll
3.9 → Liquid 4.0.3 → `String#tainted?`, which Ruby removed in 3.2, so it cannot
build on any current Ruby.

Because GitHub's built-in Pages builder only runs that legacy stack, the site
builds in GitHub Actions instead (`.github/workflows/pages.yml`) and the repo's
Pages source is set to "GitHub Actions" rather than "Deploy from a branch".

Ruby comes from Homebrew. If `bundle exec jekyll serve` can't be found, your
shell is resolving Apple's ancient system Ruby instead; the fix is this line in
`~/.zshrc`:

```
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
```
