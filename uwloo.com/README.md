# uwloo.com → uwloo.ca

Sends every uwloo.com URL to the same path on uwloo.ca (`uwloo.com/cs135` → `uwloo.ca/cs135`).

GitHub Pages can't send a real 301 to another domain, so this works the only way Pages allows: `404.html` is served for every path that doesn't exist (which is all of them), and a one-line script forwards to the same path on uwloo.ca. Without JavaScript it falls back to the uwloo.ca home page.

## Setup

1. Push these files to a **second** repo (e.g. `uwloo.com`). Each Pages site gets one custom domain, so it can't share the uwloo.ca repo.
2. Settings → Pages → Source: **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Verify `uwloo.com` under your profile → Settings → Pages, with the TXT record GitHub gives you.
4. At your registrar for `uwloo.com`, add the same records as uwloo.ca: four `A` records (185.199.108.153, .109.153, .110.153, .111.153), four `AAAA` records (2606:50c0:8000::153 through 2606:50c0:8003::153), and `CNAME www → YOUR-GITHUB-USERNAME.github.io`.
5. Settings → Pages → Custom domain: `uwloo.com`, then **Enforce HTTPS** once the certificate is issued.

## Better, if your registrar offers it

A server-side 301 is faster and is what search engines want to see. If your registrar (or Cloudflare) has URL forwarding with "forward path" / "preserve path", use that instead and skip this repo:

- Registrar forwarding: `uwloo.com` and `www.uwloo.com` → `https://uwloo.ca`, permanent (301), path forwarding on.
- Cloudflare: a Redirect Rule with dynamic target `concat("https://uwloo.ca", http.request.uri.path)`, status 301, preserve query string.
