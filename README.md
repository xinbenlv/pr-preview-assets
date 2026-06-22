# pr-preview-assets

Public host for PR preview screenshots / GIFs / walkthrough videos
(safe-to-delete mock previews) referenced from pull requests and issues in other
repos.

**Adding or linking assets? Read [AGENTS.md](AGENTS.md).** In short:

- Layout: `<repo>/pr-<num>/<path>` (a PR's "after") and `<repo>/latest/<path>`
  (the baseline "before", promoted on merge).
- Everything binary is in **Git LFS** — reference it via
  `https://media.githubusercontent.com/media/xinbenlv/pr-preview-assets/main/<path>`,
  **not** `raw.githubusercontent.com` (which returns the LFS pointer).
- GIFs/images embed inline (`![](media-url)`); videos are links.
