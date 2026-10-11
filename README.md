# uwloo.com → uwloo.ca

Sends every uwloo.com address to the same one on uwloo.ca: `uwloo.com/cs135` → `uwloo.ca/cs135`,
`www.uwloo.com/2627/cs` → `uwloo.ca/2627/cs`.

GitHub Pages can't send a real redirect to another domain, so `404.html` (served for every path, since
none exist) runs one line of script that forwards to the same path on uwloo.ca. Without JavaScript it
falls back to the uwloo.ca home page.

## Setup (GitHub Pages, DNS on Cloudflare)

1. **Verify the domain on GitHub.** https://github.com/settings/pages_verified_domains/new, enter
   `uwloo.com`. GitHub shows a TXT record: `_github-pages-challenge-UmarPatel77.uwloo.com` and a code.
2. **DNS on Cloudflare.** Paste that code into `uwloo.com-cloudflare-dns.txt` in place of
   `PASTE_CODE_HERE`. In Cloudflare, open uwloo.com → DNS → Records, delete any `A`, `AAAA` or `CNAME`
   records for `uwloo.com` and `www` that Cloudflare added on its own, then Import and Export → Import
   the file, with **Proxy imported DNS records** unchecked (GitHub Pages needs them DNS only, grey cloud).
   Back on GitHub's page, click **Verify**.
3. **Push this repo** to `UmarPatel77/uwloo.com` (a second repo: each Pages site gets one custom domain).
4. **Settings → Pages:** Source **Deploy from a branch**, branch `main`, folder `/ (root)`, Save. The
   `CNAME` file sets the custom domain to `uwloo.com`. Once the certificate is issued (usually within an
   hour), tick **Enforce HTTPS**.

## Without this repo

Since uwloo.com's DNS is on Cloudflare, a Cloudflare Redirect Rule can do this instead with a real 301,
which search engines prefer: a proxied (orange cloud) `A` record `uwloo.com → 192.0.2.1` and `CNAME
www → uwloo.com`, then Rules → Redirect Rules → a rule for all incoming requests, dynamic target
`concat("https://uwloo.ca", http.request.uri.path)`, status 301, preserve query string.
