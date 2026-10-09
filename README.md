# Personal academic website

A single static page. No build step, no dependencies.

```
index.html   The whole site: Bio, Publications, Romanizer
style.css    All styling; edit the variables at the top to restyle
photo.jpg    Sidebar portrait (600x600, displays at 152px)
```

A sticky left sidebar carries the photo, name, affiliation, email, and jump
links to the three sections. On a phone it collapses above the content and the
jump links become a horizontal row.

## Editing

### Adding a publication

`index.html` has a copy-paste template in a comment just above the Publications
section. Each entry is one `<article class="pub">`; only the title is required.
Keep newest first.

Abstracts are optional and need no JavaScript — add a `<details class="abstract">`
block inside an entry and it becomes a click-to-expand disclosure. The styles
are already in `style.css`.

### Your photo

`photo.jpg` is a 600x600 square, displayed at 152px in a circle. To swap it,
overwrite the file with another square image. If the new one isn't square, crop
it first — `sips` is built into macOS:

```bash
sips -c 1388 1388 original.jpg --out square.jpg && sips -z 600 600 square.jpg --out photo.jpg
```

### Adding a section

Copy one `<section class="section" id="...">` block, give it a new `id`, and add
a matching `<li><a href="#that-id">` to the sidebar's `.railnav` list. The jump
link works with no further wiring.

### Previewing locally

```bash
cd /Users/genekim/Work/personal_website && python3 -m http.server 4173
```

Then open <http://localhost:4173>. Opening the files directly with `file://`
also works, but a local server matches how the site behaves once deployed.

## Publishing

This repo is <https://github.com/gene-t-kim/gene-t-kim.github.io>. It is
currently **private**, and GitHub Pages is **not** enabled — nothing is public
yet.

To publish edits, commit and push as usual:

```bash
git add -A && git commit -m "Update profile" && git push
```

### Going live

GitHub Pages needs a public repo on the free plan, so going live is two steps.
Run these once the bracketed placeholders are filled in:

```bash
gh repo edit gene-t-kim/gene-t-kim.github.io --visibility public --accept-visibility-change-consequences
```

```bash
gh api -X POST repos/gene-t-kim/gene-t-kim.github.io/pages -f 'source[branch]=main' -f 'source[path]=/'
```

The site appears at <https://gene-t-kim.github.io> within a minute or two.
After that, every `git push` to `main` redeploys automatically.

To take it down again, disable Pages and flip the repo back to private:

```bash
gh api -X DELETE repos/gene-t-kim/gene-t-kim.github.io/pages
```

### A custom domain

If you later want something like `genekim.net`, add a `CNAME` file containing
just the domain, point the domain's DNS at GitHub, and set it under
Settings → Pages. Worth doing before you print the URL on anything, since it
keeps the address stable if you ever move off GitHub.

## Restyling

`style.css` opens with a `:root` block holding every color, font, and width.
Changing `--accent` recolors links, buttons, and heading rules in one edit.
A dark palette is defined below it under `prefers-color-scheme: dark`; if you
want the site to always render light, delete that block.
