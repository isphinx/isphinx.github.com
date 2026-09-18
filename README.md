# isphinx.github.io

Personal homepage and resume sources for Xin Li — full stack and systems software engineer, Sydney.

Live at **<https://isphinx.github.io/>**.

## What's here

| Path | What it is |
| --- | --- |
| `index.html` | The homepage. One self-contained file: markup, styles, and scripts inline. |
| `resume/resume.md` | Resume content in Markdown. The source of truth for everything on the homepage. |
| `resume/resume.css` | Stylesheet paired with `resume.md` for PDF export. |
| `resume/resume-dev.html` | Code-editor-themed resume, laid out for A4 print. |
| `resume/resume-dev.css` | Styles for `resume-dev.html`. |
| `resume/*.pdf` | Exported PDFs. The homepage links to `resume/resume.pdf`. |
| `previews/` | Archived design explorations from the homepage rebuild. Not part of the site. |

## The homepage

The design borrows from a game engine debug viewport — a nod to the C++ and Lua
game server work early in the career. A particle wireframe field responds to the
pointer (push on move, pull on hold, shockwave on click), the cursor is a reticle
that draws debug bounding boxes over whatever it hovers, the corner HUD graphs
real frame times, and the work history is presented as a scene hierarchy with an
inspector panel.

There is no build step and no framework. The only external request is Google
Fonts (Chakra Petch and JetBrains Mono). Editing means opening `index.html`.

The page honours `prefers-reduced-motion` by dropping the canvas and entrance
animations entirely, and swaps back to the native cursor on touch devices.

## Archived explorations

`previews/` holds the three directions considered during the rebuild: a dark
terminal with a working command prompt, an oscilloscope whose waveform the
pointer drives, and the engine viewport that shipped. They are kept for
reference only — `previews/c-engine.html` is the draft that `index.html` grew
out of, not a copy of it.

## Working on it locally

Open the file directly:

```sh
open index.html
```

Or serve the directory, which also makes the relative link to `resume/resume.pdf`
behave exactly as it does in production:

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Keeping the resume in sync

`resume/resume.md` is the source of truth. Nothing regenerates automatically, so
when it changes, the homepage copy and both PDFs need updating by hand.

The PDFs are Chrome print-to-PDF output (`Producer: Skia/PDF`), A4, with
background graphics on so the editor chrome in `resume-dev.html` survives.
`resume.pdf` comes from `resume.md` rendered by a GitHub-flavoured Markdown
renderer with `resume.css` attached; `resume-dev.pdf` comes from
`resume-dev.html` directly.

Two things that are easy to get wrong:

- **Turn background graphics on.** Without it `resume-dev.pdf` loses the title
  bar tint and the three window dots.
- **`resume-dev.html` needs print scale 0.97 to stay within two pages.** At 100%
  the InfiGaming block overhangs the first page boundary by about 30px, and
  `break-inside: avoid` on `.job` pushes the whole block over, cascading the tail
  onto a third page. The content itself only occupies 1.9 pages. `resume.md`
  needs no scaling.

To check a PDF actually picked up an edit, look at its link annotations rather
than the text — the fonts are subsetted with custom encodings, so grepping for
strings finds nothing even when the text is there:

```sh
strings resume/resume.pdf | grep -o 'URI (.*)'
```

## Deployment

GitHub Pages serves the default branch from the repository root. Pushing to it
publishes; there is no workflow or build to wait on.

The repository name (`isphinx.github.com`) is the legacy form of the user-site
convention. GitHub still publishes it at `isphinx.github.io`.
