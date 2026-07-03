# Froots Releases

The community registry for [Froots](https://froots.ai) plugins and themes —
the same model as Obsidian's `obsidian-releases`: **the registry stores no
code**. Each entry is a small pointer to a GitHub repo; the app fetches
release assets directly from that repo's GitHub Releases, which act as the
CDN. Moderation happens here, in pull-request review.

## Submitting a plugin

Append an entry to the end of [`community-plugins.json`](community-plugins.json):

```json
{
  "id": "your-plugin-id",
  "name": "Your Plugin",
  "author": "you",
  "description": "One line on what it does.",
  "repo": "your-github-name/your-plugin-repo"
}
```

Requirements:

- `id` matches your `manifest.json` and your install folder name
  (lowercase letters, digits, hyphens).
- Your repo has `README.md`, `LICENSE`, and `manifest.json` at the root.
- A GitHub release exists whose **tag exactly equals the manifest version**,
  with `main.js`, `manifest.json` (and `styles.css` if used) attached as
  individual assets.
- Your submission complies with [`plugin-review.md`](plugin-review.md).

Start from the
[sample plugin template](https://github.com/Froots-ai/froots-sample-plugin).

## Submitting a theme

Append an entry to the end of
[`community-css-themes.json`](community-css-themes.json):

```json
{
  "name": "Your Theme",
  "author": "you",
  "repo": "your-github-name/your-theme-repo",
  "screenshot": "screenshot.png",
  "modes": ["light", "dark"]
}
```

- `screenshot` is a 16:9 image in your repo (512×288 recommended) — themes
  are browsed visually.
- Release ritual is the same, with `manifest.json` + `theme.css` as assets.

Start from the
[sample theme template](https://github.com/Froots-ai/froots-sample-theme).

## How the app consumes this

1. Froots reads `community-plugins.json` / `community-css-themes.json` —
   name, author, and description power in-app search.
2. Install fetches your repo's HEAD `manifest.json` for the latest version
   and compatibility floor; if the user's app is older than your
   `minAppVersion`, the newest compatible entry in your `versions.json` is
   installed instead (Obsidian's fallback mechanism).
3. The release assets for that tag are downloaded into the user's
   `workspace/plugins/<id>/` (or `themes/<Name>/`) — atomically; nothing is
   written until every required asset has arrived.

In-app browse + install shipped in **Settings → Plugins → Browse** (plugins)
and **Settings → Themes** (themes). Manual folder-drop installs keep working
and skip the version gate — handy for development.

## Security model

Froots plugins are **not sandboxed** — they run with full app access, and the
app says so to users in plain language (community plugins are off by default;
enabling them and each individual plugin is an explicit choice). Registry
review exists to keep obvious bad actors out, not to certify safety. See
[`plugin-review.md`](plugin-review.md) for the policy.
