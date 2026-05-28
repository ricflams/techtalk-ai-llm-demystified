# Recommended repo structure

```
your-talk-repo/
│
├── slides.md                        ← Main Marp presentation
│
├── game/
│   └── index.html                   ← gone-in-60-seconds.html (renamed)
│
├── screenshots/
│   └── game-preview.png             ← Static screenshot (take after first deploy)
│
├── themes/                          ← Optional: custom Marp theme CSS
│   └── talk.css
│
├── .github/
│   └── workflows/
│       └── deploy.yml               ← Builds slides + deploys game to Pages
│
└── HANDOFF.md                       ← Claude Code brief (can delete after done)
```

## Setup steps

1. Create repo on GitHub, enable Pages:
   Settings → Pages → Source: GitHub Actions

2. Add files from this handoff package:
   - `game/index.html`  (rename gone-in-60-seconds.html)
   - `.github/workflows/deploy.yml`
   - `slides.md` (or paste game-slide-template.md into your existing deck)

3. Push to main — Action builds and deploys automatically

4. After first deploy, visit:
   `https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/game/`
   Take a screenshot at 1200×650, save as `screenshots/game-preview.png`
   Push the screenshot — GitHub.com view now shows the fallback image

5. Update the two placeholder URLs in the slide:
   `YOUR_GITHUB_USERNAME` and `YOUR_REPO_NAME`

## Game iframe sizing

The game needs to fill the Marp slide (default 1280×720) cleanly.
Tell Claude Code:
  "Make the game iframe-friendly: remove body padding,
   fill 100vh, works at 1280×720"

## Local presentation

```bash
# Install once
npm install -g @marp-team/marp-cli

# Present locally (game loads from GitHub Pages URL in iframe)
marp --html slides.md --preview

# Or build to HTML first
marp --html --allow-local-files slides.md -o presentation.html
open presentation.html
```

## Note on GitHub.com view

GitHub.com strips all `<iframe>` and `<script>` tags from Markdown.
The game will never run inline on github.com — this is a platform
restriction with no workaround. The screenshot + link is the correct
fallback. Readers click through to the live Pages deployment.
