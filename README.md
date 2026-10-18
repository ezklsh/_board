# _board

A minimal, static image moodboard: a masonry grid of the things that inspire you, each with
its own detail page. Plain HTML/CSS/JS — no framework, no build step, no database.

It ships with a **local-only tool** so you can add, edit and publish posts from a browser
instead of typing JSON. The published site stays pure static; the tool runs on your machine.

## Quickstart

1. **Create your repo from this template** (green "Use this template" button), or clone it.
2. **Enable Pages once:** Settings → Pages → Source → **GitHub Actions**.
3. **Run the tool:**
   ```bash
   python3 tools/upload_server.py          # → http://localhost:8080
   ```
4. Open <http://localhost:8080/upload>, add an image, and press **push to repo**.
   The Action rebuilds and deploys in a minute or two.

## Pages

| Page | What it does |
| --- | --- |
| `/` | the moodboard grid |
| `/upload` | add a post — image + title + description |
| `/manage` | edit, rename a post, delete it (**undo** until you push), and push to the repo |

Pressing **push** stages `js/posts.json` and `assets/`, commits, and pushes. Nothing is
published until you push.

## Authentication

The tool never asks for a password and keeps no token. When you press push it runs plain
`git push`, so it uses **whatever your git is already set up with** — a credential helper
(macOS Keychain, Windows Credential Manager), an SSH key, or the GitHub CLI:

```bash
gh auth login          # one way: installs a credential helper and stores a token
```

If a push fails because git has no credentials for the host, the tool tells you and stops —
it never hangs waiting for a prompt.

## License

MIT — see [LICENSE](LICENSE). Replace the copyright line with your own name.
