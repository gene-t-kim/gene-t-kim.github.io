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

The site is live at <https://gene-t-kim.github.io>, served from the `main`
branch of <https://github.com/gene-t-kim/gene-t-kim.github.io>.

To publish a change, commit and push — Pages redeploys within a minute or two:

```bash
git add -A && git commit -m "Update bio" && git push
```

Two `gh` accounts are authenticated on this machine. The repo belongs to
`gene-t-kim`, so if a `gh` command returns 404, check which one is active:

```bash
gh auth status && gh auth switch --user gene-t-kim
```

To take the site down:

```bash
gh api -X DELETE repos/gene-t-kim/gene-t-kim.github.io/pages
```

Note that making the repo private again does not un-publish anything already
cached or indexed by search engines.

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
