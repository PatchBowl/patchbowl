# Patchbowl Landing Page

The site served at [patchbowl.dev](https://patchbowl.dev).

Single-file static landing for the pre-alpha phase. Will graduate to Next.js once we have product screenshots and docs that justify a build step.

## Files

- `index.html` — the whole site. Tailwind via Play CDN, Inter + JetBrains Mono via Google Fonts.
- `_headers` — security headers for Cloudflare Pages.

## Local preview

Any static server works:

```bash
# Python
cd web && python3 -m http.server 8000

# Or via npx
npx serve web
```

Open `http://localhost:8000`.

## Deploy to Cloudflare Pages

One-time setup, ~10 minutes total.

### 1. Connect the repo

1. Go to [Cloudflare dashboard → Workers & Pages](https://dash.cloudflare.com) → **Create application → Pages → Connect to Git**.
2. Authorize Cloudflare to read the `PatchBowl/patchbowl` repo.
3. Configure the build:
   - **Production branch:** `main`
   - **Framework preset:** None
   - **Build command:** *(leave empty)*
   - **Build output directory:** `web`
4. Click **Save and Deploy**.

First deploy takes ~30 seconds. You'll get a `patchbowl.pages.dev` URL.

### 2. Connect the custom domain

1. In the Pages project → **Custom domains → Set up a custom domain**.
2. Enter `patchbowl.dev`.
3. Cloudflare will tell you which DNS records to add.

**If your domain is registered with Cloudflare:** records are added automatically. Done.

**If your domain is registered elsewhere** (Namecheap, Porkbun, Google Domains, etc.): either
- Move DNS to Cloudflare (recommended — change nameservers at the registrar to the ones Cloudflare gives you), *or*
- Add the CNAME record they specify at your current DNS provider.

Either way, also add `www.patchbowl.dev` → redirect to `patchbowl.dev` in Pages → **Redirects**.

### 3. Verify

Wait ~5 min for DNS to propagate. Then:

```bash
curl -I https://patchbowl.dev
# Should return 200 OK with HSTS header
```

## Updating the site

Just commit + push. Cloudflare Pages auto-deploys every push to `main`.

## TODO before v0.1 launch

- [ ] Real Buttondown account — currently form posts to `patchbowl` username, sign up at [buttondown.email](https://buttondown.email) and confirm the username matches.
- [ ] Open Graph image at `/og.png` (1200×630, includes logo + tagline) — currently meta tag points to a 404.
- [ ] Branded favicon SVG to replace the inline emoji.
- [ ] Privacy policy + cookie banner (only if we add analytics).
