# Plugin & Theme Review Policy

What we check before merging a registry submission, and the standing rules
every listed plugin/theme must follow. Modeled on Obsidian's developer
policies.

## Hard requirements (rejected without these)

1. **No code obfuscation.** Bundling and minification are fine; deliberately
   obfuscated source is not. The repo must contain readable source for
   everything shipped in the release assets.
2. **No self-updating or remote code loading.** Plugins update only through
   new tagged releases. Fetching and executing code at runtime (eval of
   downloaded scripts, remote plugin loaders) is banned.
3. **No undisclosed telemetry.** Client-side analytics are banned. If your
   plugin talks to your own server for functionality, the README must say
   so, name every service contacted, and link a privacy policy if any user
   data is stored.
4. **No undisclosed network use.** Any network access must be listed in the
   README: which services, why, and what data is sent.
5. **Disclose file access outside the workspace.** Plugins that read or
   write outside the Froots workspace directory must say so in the README
   and justify it.
6. **Disclose paid features / accounts.** If the plugin requires a paid
   service or account, the README says so up front.
7. **License required.** A LICENSE file at the repo root. Closed-source
   components are case-by-case and must be declared in the submission PR.

## Code-quality expectations

- Register every side effect through the plugin API's registrars
  (`commands.add`, `events.on`, `registerInterval`, `registerDomEvent`) so
  disable = clean teardown. Plugins that leak listeners or timers across
  disable/enable cycles will be asked to fix it.
- Don't touch other plugins' folders or `data.json`.
- Scope injected CSS (`styles.css`) to your own selectors.
- Themes: CSS only. A theme submission containing JavaScript is rejected.

## Process

1. Open a PR appending your entry to the end of the relevant JSON array.
2. Automated checks validate the JSON shape, repo layout, manifest/tag
   match, and release assets.
3. A maintainer reviews the code. Popular, featured, and flagged
   plugins get re-reviewed periodically.
4. Address feedback by pushing new tagged releases — the registry entry
   itself rarely needs to change.

Violations found after listing get one warning and a deadline; unresolved
violations are removed from the registry (entries move to a `-removed.json`,
so installs stop but history is preserved).
