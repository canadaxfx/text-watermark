# Text Watermark Tool — Claude Context

Single-file HTML tool for watermarking Chinese social media content.

## Files

| File | Purpose |
|------|---------|
| `watermark.html` | Source of truth — edit this one |
| `index.html` | GitHub Pages entry point — always sync from watermark.html before pushing |
| `watermark_README.md` | User-facing README displayed on GitHub repo |
| `DEVELOPMENT_LOG.md` | Full version history and design decisions |

## Critical: Two-File Sync

`index.html` is a copy of `watermark.html` for GitHub Pages. Always run before pushing:
```
cp watermark.html index.html
```
Never edit `index.html` directly.

## GitHub

- Repo: https://github.com/canadaxfx/text-watermark
- Pages (online tool): https://canadaxfx.github.io/text-watermark/
- Token: in `C:\Users\yvonz\Desktop\Tools\AI tools\xz-gallery-autosync\config.json` → `github_token`
- Push command:
  ```
  git push https://canadaxfx:{token}@github.com/canadaxfx/text-watermark.git main
  ```

## Before Every Push Checklist

1. `cp watermark.html index.html`
2. Update `watermark_README.md` if features changed
3. Update `DEVELOPMENT_LOG.md` if features changed
4. Update about modal inside `watermark.html` if features changed
5. `git add watermark.html index.html watermark_README.md DEVELOPMENT_LOG.md`

## Architecture

- Zero dependencies — pure HTML/CSS/JS, single file, works offline
- All logic in `<script>` tag
- Key functions: `generateWatermark()`, `generateImage()`, `saveEvidenceImage()`, `zwcEncode()`, `zwcDecode()`, `verifyWatermark()`, `setUiFont()`
- Settings persisted to `localStorage`
- Auto-generates 600ms after input stops (`scheduleGenerate()`)

## Known Gotchas

- ZWC (zero-width characters) survive Weibo copy-paste; Unicode combining marks (U+0300–U+036F) do NOT — they render as □
- Watermark font size = `fontSize * wmSizeMult` (user-selectable 0.7/1.0/1.3)
- Evidence image (`saveEvidenceImage`) uses hardcoded `fontSize = 17`, separate from the image tab's font setting
- Bottom tags bar renders above timestamp bar in generated images
