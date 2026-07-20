# CLAUDE.md — server-panel-dist

## What this repo is

This is an **artifacts-only public mirror**, NOT a source repo.

It is the public distribution target for a separate **private** source repo,
`server-panel` — the Go + React server control panel product **"Ppt Server
Panel"**:

- `mthan-vps` — the compiled panel (app) binary (Linux)
- `mthanctl` — the compiled control CLI binary (Linux)

This repo must remain **public** so the deployed panel and its installer can pull
updates over **unauthenticated** URLs (raw.githubusercontent + jsDelivr). Do not
make it private.

## Golden rule: never hand-edit artifacts

Everything under `public/dist/` is **pushed automatically** by the source repo's
`scripts/build.sh` (its `push_dist` step). Any manual change to the binaries,
`version.json`, or the client bundle will be **overwritten by the next build
push**. Treat this repo as a write-target for CI/build, not a place to author
anything.

The only files intended to be authored by hand are the docs (`README.md`, this
`CLAUDE.md`) and the `.gitkeep` placeholders.

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

`.gitkeep` files keep `public/dist/` and `public/dist/client/` present before the
first build push.

## build.sh push contract

The private `server-panel` repo's `scripts/build.sh` compiles the binaries and
the React client, writes `version.json` (`{"version","buildTime"}`), and pushes
the whole `public/dist/` tree into this repo on `main`. This repo passively
receives that push — it does not build anything itself.

## Consumption URLs

- **Panel self-update** (raw GitHub):
  - `https://github.com/<owner>/server-panel-dist/raw/main/public/dist/version.json`
  - `https://github.com/<owner>/server-panel-dist/raw/main/public/dist/mthan-vps`
- **Installer** (jsDelivr CDN):
  - `https://cdn.jsdelivr.net/gh/<owner>/server-panel-dist@main/public/dist`

## Do NOT

- Do not add source code, Go/React tooling, or CI here.
- Do not hand-edit files under `public/dist/`.
- Do not make this repo private.
