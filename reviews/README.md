# Marly Creative Concepts — Ad Review Portal

Static site for hosting client ad set reviews. Each client gets a folder, each campaign gets a subfolder.

## Folder structure

```
/
├── index.html                          ← Landing page (no direct access)
├── vercel.json                         ← Config: static hosting, noindex headers
├── {client-slug}/
│   └── {campaign-slug}/
│       ├── index.html                  ← Review page showing all sizes
│       ├── 300x250/index.html          ← Individual ad files
│       ├── 728x90/index.html
│       ├── 160x600/index.html
│       ├── 320x50/index.html
│       └── assets/                     ← Shared images, fonts, GSAP
└── _example-client/
    └── sample-campaign/
        └── index.html                  ← Template/reference
```

## Adding a new review

1. Create a folder: `{client-slug}/{campaign-slug}/`
2. Drop the production build (index.html + ad size folders) into it
3. Push to main — Vercel auto-deploys
4. Share: `review.marlycreative.com/{client-slug}/{campaign-slug}`

## Notes

- All pages have `X-Robots-Tag: noindex` to prevent search indexing
- Review URLs are unlisted but public — use obscured slugs for sensitive work
- The `_example-client` folder is a reference template, not a real client
