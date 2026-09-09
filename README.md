# Personal academic website

Three static pages sharing one stylesheet. No build step, no dependencies.

```
index.html          Profile & research interests
publications.html   Publications with collapsible abstracts and links
cv.html             Live Dropbox CV (embedded + download link)
style.css           All styling; edit the variables at the top to restyle
```

## Filling in your content

Every spot that needs your text is marked with an `EDIT` comment in the HTML.
To find them all:

```bash
grep -n "EDIT" *.html
```

Three things appear on all three pages, so change them in all three:

- your name in `.masthead__name`
- your affiliation in `.masthead__affiliation`
- the email address in the nav and the footer year

## Wiring up the Dropbox CV

1. In Dropbox, right-click the CV PDF → **Copy link**. You get something like
   `https://www.dropbox.com/scl/fi/abc.../CV.pdf?rlkey=xyz&dl=0`.
2. Make two versions by swapping only the last parameter:
   - `&raw=1` — streams the raw PDF, used for the embed and "Open in new tab"
   - `&dl=1` — forces a download, used for the "Download PDF" button
3. In `cv.html`, replace the three URLs marked `EDIT-CV-URL`.

Because the page points at the live file, replacing the PDF in Dropbox (keeping
the same filename) updates the site with no code change.

**If the embed shows blank:** Dropbox occasionally serves the `raw=1` URL with
headers that stop it being framed, and mobile Safari/Chrome often refuse to
render framed PDFs regardless. The Download and Open-in-new-tab buttons above
the frame always work, which is why they're there. If you'd rather not risk a
blank box, delete the `<iframe>` element and keep just the buttons.

## Adding a publication

`publications.html` starts with a copy-paste template in a comment. Each entry
is one `<article class="pub">`; the abstract is a `<details>` element, so the
expand/collapse works with no JavaScript. Keep each section newest-first.

Citations are formatted in Chicago notes-and-bibliography style, and the page
is divided into Book / Peer-reviewed articles / Book chapters / Works in
progress / Book reviews / Public writing. Delete any section you don't need —
the Book section in particular is worth removing until there's a contract.

Single-authored entries omit the author line entirely, since your name is
already the site. For a co-authored piece, add a line above the venue and wrap
your own name so it stands out:

```html
<p class="pub__authors">With <span class="me">Gene Kim</span> and A. Coauthor</p>
```

## Previewing locally

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
