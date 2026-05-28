---
marp: true
html: true
---

<!-- This slide works in two modes:
     - Marp presentation (--html flag): iframe shows the live game
     - GitHub.com markdown view: screenshot with clickable link to live game
     
     Replace YOUR_GITHUB_USERNAME and YOUR_REPO_NAME below.
     Replace the screenshot path once you've taken one.
     
     To take a screenshot:
       open https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/game/
       screenshot at 1200x650, save to screenshots/game-preview.png
-->

# Gone in 60 Seconds 🔥

<style scoped>
section {
  padding: 0;
  display: flex;
  flex-direction: column;
}
.slide-title {
  font-size: 28px;
  font-weight: 800;
  padding: 16px 24px 8px;
}
iframe.game {
  flex: 1;
  width: 100%;
  border: none;
}
/* GitHub fallback: hide screenshot when iframe is rendering */
.github-fallback { display: none; }
</style>

<div class="slide-title">Gone in 60 Seconds 🔥</div>

<!-- MARP: shows live game -->
<iframe class="game"
  src="https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/game/"
  allowfullscreen>
</iframe>

<!-- GITHUB FALLBACK: visible on github.com (Marp strips this comment logic,
     but GitHub renders the image+link below the iframe as plain markdown) -->

[![Gone in 60 Seconds — interactive token spending game. Click to play.](./screenshots/game-preview.png)](https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/game/)

*[▶ Open interactive version](https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/game/)*
