# server-panel-dist

**Public distribution artifacts for the private `server-panel` source repo.**

This repo is the public distribution target for a separate **private** source
repo, `server-panel` — the Go + React server control panel product **"Ppt Server
Panel"** (app binary `mthan-vps`, control binary `mthanctl`).

It must stay **public** so the deployed panel and its installer can pull updates
over unauthenticated URLs (raw.githubusercontent + jsDelivr).

> ⚠️ **Do NOT hand-edit anything in this repo.** It holds **only build
> artifacts** — never source. Everything here is pushed automatically by the
> source repo's `scripts/build.sh` (its `push_dist` step). Manual edits will be
> overwritten by the next build push.

## Layout

```
public/
  install.sh          # installer, curl'd by the install one-liner
  dist/
    mthan-vps         # compiled Linux binary — the panel (app)
    mthanctl          # compiled Linux binary — the control CLI
    version.json      # {"version","buildTime"} — drives the "Update Available" check
    client/           # built React client bundle
```

The `.gitkeep` files in `public/dist/` and `public/dist/client/` exist only to
keep the directory structure present before the first build push. They are
replaced/joined by real artifacts once `build.sh` runs.

## How it's consumed

- **Panel self-update** fetches, over raw GitHub:
  - `https://github.com/<owner>/server-panel-dist/raw/main/public/dist/version.json`
  - `https://github.com/<owner>/server-panel-dist/raw/main/public/dist/mthan-vps`
- **Installer** is fetched via jsDelivr:
  - `https://cdn.jsdelivr.net/gh/<owner>/server-panel-dist@main/public/dist`

## What does NOT belong here

No source code, no build tooling, no CI. This repo only **receives** compiled
artifacts from the private `server-panel` source repo.
