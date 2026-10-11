# moodboard

A minimal, static image moodboard: a masonry grid of the things that inspire you, each with
its own detail page. Plain HTML/CSS/JS — no framework, no build step, no database.

It ships with a **local-only tool** so you can add, edit and publish posts from a browser
instead of typing JSON. The published site stays pure static; the tool runs on your machine.

## Quickstart

1. **Create your repo from this template** (green "Use this template" button), or clone it.
2. **Enable Pages once:** Settings → Pages → Source → **GitHub Actions**.
   Nothing deploys until you do this — the Action has a `deploy` job but no target.
3. **Run the tool:**
   ```bash
   python3 tools/upload_server.py          # → http://localhost:8080
   ```
4. Open <http://localhost:8080/upload>, add an image, and press **push to repo**.
   The Action rebuilds and deploys in a minute or two.

## The three pages

| Page | What it does |
| --- | --- |
| `/` | the moodboard grid |
| `/upload` | add a post — image + title + description |
| `/manage` | edit, rename a post, delete it (**undo** until you push), and push to the repo |

Pressing **push** stages `js/posts.json` and `assets/`, commits, and pushes. Nothing is
published until you push.

## Your content

- **`js/posts.json`** — the store: a JSON array of
  `{slug, title, description, cover, gallery, related}`.
- **`assets/`** — your images. Committed to the repo and served by Pages as-is.
- Descriptions render as HTML and keep their line breaks. `related` takes a list of slugs.

**This template ships empty.** `js/posts.json` is `[]` and `assets/` holds no images, so
the grid stays blank until you add something. Add your first post from `/upload` and
press **push to repo** when you are happy with it.

**Renaming the site** from "inspiration": the name appears in the `<title>`, the header link
and the footer of `index.html` and `post.html`. Everything else is generic.

## How authentication works — the tool stores no credentials

The tool never asks for a password and keeps no token. When you press push it runs plain
`git push`, so it uses **whatever your git is already set up with** — a credential helper
(macOS Keychain, Windows Credential Manager), an SSH key, or the GitHub CLI:

```bash
gh auth login          # one way: installs a credential helper and stores a token
```

If a push fails because git has no credentials for the host, the tool tells you and stops —
it never hangs waiting for a prompt.

## Requirements

Python 3 (standard library only). **No `node`, no npm, no dependencies.**

The site loads its store with `fetch()`, so it must be served over HTTP. Opening
`index.html` straight from disk shows a message telling you to run the tool server.

## Where images live

In the repo, by default: zero setup, versioned with your content, no external accounts. That
is the right default up to a few thousand optimised images. If your collection outgrows it,
the field can hold an absolute URL as easily as a relative path, so moving to S3 /
Cloudflare R2 / Backblaze B2 later is a content change, not a rewrite.

Two notes: keep uploaded images reasonably sized (GitHub Pages serves them as-is, and its
soft limit is ~1GB per site, ~100GB/month of bandwidth), and **do not use Git LFS** — Pages
does not serve LFS objects.

## Tests

```bash
python3 -m unittest discover -s tools -p "test_*.py"
```

## License

MIT — see [LICENSE](LICENSE). Replace the copyright line with your own name.
