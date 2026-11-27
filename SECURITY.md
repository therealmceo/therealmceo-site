# Security — therealmceo.vercel.app

This is the personal website of Marvellous Christopher (Therealmceo). It is a static, single-page site hosted on Vercel. This file explains how the site is protected and how to report a problem.

## How the site is built to be safe

- **No backend, no database, no forms.** The site stores and collects nothing about visitors, so there is no data to leak.
- **No third-party scripts.** No analytics, trackers or ad scripts are loaded. The only outside resource is Google Fonts.
- **Images are embedded in the page itself**, so nothing can be swapped out from another server.
- **External links** open in a new tab with `rel="noopener"`, so other sites can't take control of this page.
- **Contact** happens through WhatsApp and email links only. No contact details are collected on the site.

## Security headers (set in `vercel.json`)

| Header | What it does |
|---|---|
| `Content-Security-Policy` | Only the site's own script (pinned by its SHA-256 hash), its own styles and Google Fonts may load. Everything else is blocked: no injected scripts, no plugins, no forms, no outside connections. |
| `frame-ancestors 'none'` + `X-Frame-Options: DENY` | Other websites can't load this site inside a frame (protects against clickjacking). |
| `Strict-Transport-Security` | Browsers always use HTTPS for this site. |
| `X-Content-Type-Options: nosniff` | Browsers won't guess file types, which blocks a class of injection tricks. |
| `Referrer-Policy: strict-origin-when-cross-origin` | Other sites only see the domain, never the full page address. |
| `Permissions-Policy` | Camera, microphone, location, payment and USB access are switched off. |
| `Cross-Origin-Opener-Policy: same-origin` | Isolates the page from windows opened by other sites. |

A copy of the Content-Security-Policy is also set in the page itself (`<meta http-equiv>`), so the main protections still apply if the file is ever deployed without `vercel.json`.

## Important: when you edit the page script

The CSP allows only the exact script that is in the page, identified by its SHA-256 hash. **If you change anything inside the main `<script>` tag, you must update the hash** in both the `<meta http-equiv="Content-Security-Policy">` tag and `vercel.json`, or the animations will stop working.

To get the new hash, run this on the file (Python 3):

```python
import re, hashlib, base64
html = open("index.html", encoding="utf-8").read()
js = re.search(r"<script>(.*?)</script>", html, re.S).group(1)
print("sha256-" + base64.b64encode(hashlib.sha256(js.encode()).digest()).decode())
```

Changing text, images or styles does not need a new hash.

## Account protection

- Turn on **two-factor authentication** for the Vercel account (therealmceo@gmail.com) and for GitHub.
- Only deploy from the Vercel dashboard while signed in to that account. Never share Vercel verification codes with anyone.

## Reporting a problem

If you find a security issue on this site, please email **marvellouschristopher17@gmail.com** with the details. Please don't post it publicly until it has been fixed.
