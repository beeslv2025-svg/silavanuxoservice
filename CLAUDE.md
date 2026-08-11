# Silavan UXO Services — website

Live: **https://silavanuxoservice.pages.dev**

Plain static site. No framework, no build step, no `package.json`, no dependencies.
Everything is hand-written HTML/CSS/JS served exactly as it sits in this folder.

## Deploying — pushing to `main` IS the deploy

```
edit files  →  git push origin main  →  live in ~1 minute
```

GitHub `beeslv2025-svg/silavanuxoservice` → Cloudflare Pages project `silavanuxoservice`,
wired through the Cloudflare GitHub App. Verified working end-to-end.

There is no dashboard step and no `wrangler` command. Do not add a deploy script, a
GitHub Actions workflow, or a build tool — they would duplicate or break this.

Cloudflare build settings, if they ever need re-entering: framework preset **None**,
build command **empty**, build output directory **`/`**.

## Rules for changing this site

- **No build step.** Keep files servable as-is from the repo root.
- **`_headers` must keep working.** Cloudflare Pages reads it natively from the root.
  It sets the security headers, the 1-year immutable cache on `/images/*` and `/videos/*`,
  and `noindex` on the internal handbook. Moving the site into a subdirectory silently
  disables all of it.
- **Replacing an image? Bump its `?v=` query too.** `_headers` serves `/images/*` and
  `/videos/*` with `max-age=31536000, immutable`, so a browser that has seen the old file
  keeps it for a year and never asks again. Overwriting the file is not enough — the
  reference must change. `slv-logo.png` and `favicon.png` are at `?v=2`; raise the number
  in **every** page that links them (`index.html`, `privacy.html`, `404.html`) plus the
  absolute `og:image` and the JSON-LD `logo`. Verify with a cache-busted fetch, not a
  normal reload, because a reload can be served from cache.
- **Relative paths only** (`images/foo.jpg`, not `/images/foo.jpg` or a full URL) — this is
  how every existing reference is written.
- **Comments are bilingual**, Lao first then English. Match that when editing.
- **Changing the domain means editing two files together**: `sitemap.xml` (`<loc>` values)
  and `robots.txt` (the `Sitemap:` line).
- Videos live in `videos/` as `<name>.mp4` plus a matching `<name>.jpg` poster, and are
  registered in the `slvVideos` array near the bottom of `index.html`.

## Layout

| Path | What |
|---|---|
| `index.html` | The entire site — all sections, styles, and scripts in one file |
| `privacy.html`, `404.html` | Standalone pages |
| `images/`, `videos/` | Media (long-cached, treated as immutable) |
| `docs/nra-ns/`, `docs/slv-sops/` | Published PDF standards and SOPs |
| `qr/` | QR codes pointing at the site |
| `README-TH.md` | Internal handbook — served but `noindex` |
| `_headers` | Cloudflare Pages HTTP headers |

## Do not

- Delete the superseded GitHub repo `Silavan-UXO` or the Cloudflare project `silavan-uxo`.
  They are kept deliberately; removing them is the owner's call, not an agent's.
- Touch the private repo `Silavan-UXO-Service` — unrelated to this site.
