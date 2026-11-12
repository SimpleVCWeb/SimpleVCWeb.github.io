# SimpleVCWeb.github.io

Short redirect domain for <https://SimpleVoiceChatWeb.github.io>.

## Purpose

`https://SimpleVCWeb.github.io` is a shorter public URL for the same project.
It does not host any content of its own — every request is forwarded to the
canonical GitHub Pages site, keeping path, query and hash unchanged.

## Files

| File | Purpose |
|---|---|
| `index.html` | Redirect for `/` |
| `404.html` | Redirect for unknown paths (GitHub Pages fallback) — must stay identical to `index.html` |
| `docs/index.html` | Redirect for `/docs` — required so the path returns HTTP 200 instead of 404 |
| `.nojekyll` | Disables Jekyll processing |
| `.github/workflows/pages.yml` | Deploys the repository with GitHub Actions Pages |

## Link previews

Discord, WhatsApp, Slack and Telegram do not execute JavaScript. Their
crawlers only read the static HTML, so every redirect page carries Open Graph
and Twitter Card tags for a proper title and description. Without them a
shared link shows up as a bare "Redirecting …".

`og:url` is the short domain, because that is the address people share.

## Redirect logic

```js
window.location.replace(
    "https://SimpleVoiceChatWeb.github.io"
    + location.pathname
    + location.search
    + location.hash
);
```

Examples:

| Request | Forwarded to |
|---|---|
| `/?code=ABC123` | `https://SimpleVoiceChatWeb.github.io/?code=ABC123` |
| `/#/login` | `https://SimpleVoiceChatWeb.github.io/#/login` |
| `/docs` | `https://SimpleVoiceChatWeb.github.io/docs` |
| `/docs/` | `https://SimpleVoiceChatWeb.github.io/docs/` |

## Why `docs/index.html` exists

GitHub Pages serves `404.html` for unknown paths, but with the HTTP status
`404`. Browsers still run the redirect, yet link previews, crawlers and some
QR scanner apps treat the link as broken. `docs/index.html` gives `/docs` a
real `200` response.

## Deployment

Pages is configured with `build_type: workflow`. Every push to `main` deploys
the site; a manual run is also possible:

```bash
gh workflow run pages.yml -R SimpleVCWeb/SimpleVCWeb.github.io
```

## Notes

- Keep `index.html` and `404.html` in sync with `docs/index.html`.
- Do not add tracking, analytics or third-party scripts here.
- The canonical site remains `SimpleVoiceChatWeb.github.io`; `ALLOWED_ORIGINS`
  for the web client must stay on the long domain because the browser lands
  there after the redirect.
