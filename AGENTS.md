# pr-preview-assets — conventions (read before adding or linking assets)

Public host for **preview assets** — screenshots, walkthrough GIFs, and Critical
User Journey (CUJ) walkthrough videos — referenced from pull requests and issues
in **other repos** (often private), where GitHub's native attachment rendering
isn't reliably visible to every viewer or bot. Everything here is a disposable
mock preview; treat all of it as safe-to-delete.

## Directory layout

```
<repo>/pr-<num>/<semantic/path>      # assets for ONE pull request (the "after")
<repo>/latest/<semantic/path>        # current baseline per asset (the "before")
<repo>/<topic>-<YYYY-MM-DD>/...       # one-off bundles (audits, funnels, etc.)
```

- `<repo>` = the source repo the asset belongs to (e.g. `namefi-astra`).
- `pr-<num>/` mirrors the source PR number; `<semantic/path>` mirrors what's
  shown (e.g. `cuj/CUJ-Owner.1.gif`).
- **`latest/` holds the most recently _merged_ version of each asset.** A
  reviewer diffs a PR's `pr-<num>/` ("after") against `latest/` ("before").
  **On merge, copy the PR's assets into `latest/`** (overwrite) so they become
  the next change's baseline.

## Git LFS — every binary asset is in LFS

`.gitattributes` tracks all asset types via LFS:

```
*.png *.jpg *.jpeg *.gif *.webp *.mp4 *.mov *.webm  → LFS
```

Consequence: **`raw.githubusercontent.com` returns the LFS _pointer text_, not
the bytes.** Always reference assets through the **media** endpoint:

```
https://media.githubusercontent.com/media/xinbenlv/pr-preview-assets/<ref>/<path>
```

- **Image / GIF** → embed inline:
  `![](https://media.githubusercontent.com/media/xinbenlv/pr-preview-assets/main/<path>.gif)`
  (GIFs animate; the media endpoint serves the correct image content-type).
- **MP4 / video** → link it, don't try to embed:
  `[walkthrough](https://media.githubusercontent.com/media/.../<path>.mp4)`.
  GitHub renders an inline **video player only for files uploaded as comment
  attachments**, never for raw/media URLs — so videos are links, GIFs are the
  inline preview.

## No symlinks for `latest/`

A git symlink is served over raw/media as a text file containing the **target
path**, not the target's content — so it can't be hotlinked. `latest/` must hold
a **real copy** of the file. With LFS this is free: identical content shares one
content hash, so the copy reuses the same LFS object (deduplicated) — no extra
storage.

## Before/after CUJ previews (namefi-astra)

When a PR touches a CUJ, a walkthrough is recorded and committed under
`namefi-astra/pr-<num>/cuj/CUJ-<Area>.<n>.{gif,mp4}`. The PR comment shows
`latest/` (before) beside `pr-<num>/` (after) using media-endpoint URLs. On
merge, the PR's assets promote to `namefi-astra/latest/cuj/…`, becoming the
baseline for the next change to that journey.

## Cleanup

Delete `<repo>/pr-<num>/` once the PR is closed and its assets are promoted to
`latest/` or no longer referenced. Keep the repo lean.
