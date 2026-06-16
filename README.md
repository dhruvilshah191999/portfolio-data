# portfolio

Content source for my personal portfolio site. The site fetches this repo's
`data/data.json` at runtime (via `VITE_DATA_URL`) so I can update my experience
and projects — and swap their banner images — **without redeploying** the site.
If the URL is unreachable, the site falls back to its bundled static copy.

## Structure

```
portfolio/
├── data/
│   └── data.json                    # experience + projects (the live content)
└── assets/
    ├── images/                      # project banner images
    │   ├── use-shortcut.png
    │   ├── TimeLens.png
    │   ├── crossify.png
    │   └── devtools-playground.png
    └── resume/
        └── Dhruvil_Resume.pdf       # downloadable résumé
```

## data.json shape

```json
{
  "experience": [
    {
      "company": "Acme",
      "role": "Senior Software Developer",
      "period": "Apr 2025 — Present",
      "location": "Bengaluru, India",
      "url": "https://acme.com",
      "achievements": ["…", "…"],
      "stack": ["React", "Node.js", "AWS S3"]
    }
  ],
  "projects": [
    {
      "title": "use-shortcut",
      "role": "Open source · npm package",
      "year": "2025",
      "summary": "…",
      "stack": ["React", "TypeScript"],
      "links": {
        "live": "https://…",
        "source": "https://github.com/…",
        "npm": "https://www.npmjs.com/package/…"
      },
      "image": "https://raw.githubusercontent.com/<user>/portfolio/master/assets/images/use-shortcut.png"
    }
  ],
  "resumeUrl": "https://raw.githubusercontent.com/<user>/portfolio/master/assets/resume/Dhruvil_Resume.pdf"
}
```

### Field notes

- **`links`** — any of `live`, `source`, `npm`, `chrome` may be present or
  empty (`""`). Empty/missing links simply don't render their icon.
- **`image`** — a fully-qualified raw GitHub URL to a file in `assets/images/`.
  To add a banner: drop the file in `assets/images/`, then point `image` at its
  raw URL (see below). Use a wide banner (~2.4:1, e.g. 1000×420).
- Cards render in array order; the visible number (00, 01, …) follows the order
  here.
- **`resumeUrl`** — a top-level field (sibling to `experience`/`projects`)
  holding the raw GitHub URL to the résumé PDF in `assets/resume/`. The site's
  résumé link/download points here.

## Raw URL format

GitHub serves raw files at:

```
https://raw.githubusercontent.com/<user>/portfolio/<branch>/<path>
```

Examples (replace `<user>` and the branch — `main` or `master`):

```
# the data file
https://raw.githubusercontent.com/<user>/portfolio/master/data/data.json

# a project image
https://raw.githubusercontent.com/<user>/portfolio/master/assets/images/crossify.png

# the résumé PDF
https://raw.githubusercontent.com/<user>/portfolio/master/assets/resume/Dhruvil_Resume.pdf
```

Point the site at the data file by setting its `VITE_DATA_URL` env var to the
`data/data.json` raw URL above.

## Updating content

1. Edit `data/data.json` (and/or add an image under `assets/images/`, or replace
   the PDF under `assets/resume/`).
2. Validate the JSON (e.g. `python -m json.tool data/data.json`, or paste into
   any JSON linter).
3. Commit and push to the default branch.

The live site picks up the change on the next load — no rebuild required. Note:
GitHub's raw CDN caches for a few minutes, so updates may take a moment to
appear.
