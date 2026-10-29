# HostMatrix

HostMatrix is a static directory for discovering and comparing free hosting platforms. It is a single-page HTML app built with vanilla JavaScript and Tailwind CSS, with Font Awesome icons and Google Fonts loaded from CDNs.

## Run locally

Open `index.html` in a browser, or serve this directory with a local static server:

```sh
python3 -m http.server 8000
```

Then visit <http://localhost:8000>.

## Deploy to Vercel

This project has no build step or backend. The project root is the directory containing `index.html`.

1. Push this repository to GitHub.
2. In Vercel, choose **Add New Project** and import the GitHub repository.
3. Use the project root as the Root Directory and select **Other** as the Framework Preset.
4. Leave the Build Command and Output Directory unset, then deploy.

## Custom domain: hostmatrix.bunon.xyz

1. In Vercel, open the project → **Settings → Domains** → add `hostmatrix.bunon.xyz`.
2. Vercel shows a DNS record to create. At the DNS provider for `bunon.xyz`, add it:

   | Type  | Name / Host  | Value                                              |
   |-------|--------------|----------------------------------------------------|
   | CNAME | `hostmatrix` | the target Vercel shows (e.g. `cname.vercel-dns.com`) |

   On Cloudflare DNS, set the record to **DNS only** (grey cloud) so Vercel can issue the SSL certificate.
3. Wait for Vercel to show the domain as **Valid Configuration** and the certificate as issued.

## Google Analytics 4

1. In [Google Analytics](https://analytics.google.com), create a GA4 property and a **Web** data stream for `https://hostmatrix.bunon.xyz`.
2. Copy the Measurement ID (`G-…`) and replace every `G-XXXXXXXXXX` in `index.html` and `privacy.html`.
3. Optional: under **Admin → Events**, mark `host_click` as a key event, and under **Custom definitions** register `host_id` and `view_name` as event-scoped dimensions.

Custom events sent by the app:

| Event           | When                                         | Params                         |
|-----------------|----------------------------------------------|--------------------------------|
| `view_change`   | Switching between Explore / Matrix / Wizard  | `view_name`                    |
| `search`        | Typing in the directory search (debounced)   | `search_term`                  |
| `open_guide`    | Opening a provider's setup guide             | `host_id`                      |
| `host_click`    | Clicking through to a provider               | `host_id`, `location`          |
| `quiz_complete` | Finishing the wizard                         | `host_id`, wizard answers      |

## Google AdSense

AdSense does not accept a subdomain as its own site, so the account is set up for the root domain `bunon.xyz` and ads then serve on `hostmatrix.bunon.xyz`.

1. Sign up at [AdSense](https://adsense.google.com) and copy your publisher ID (`ca-pub-…`).
2. Replace every `ca-pub-0000000000000000` in `index.html` and `privacy.html`, and set `adsenseClient` in `SITE_CONFIG` in `index.html`.
3. In `ads.txt`, replace `pub-0000000000000000` with your ID (keep it without the `ca-` prefix).
4. **Serve `ads.txt` from the root domain.** Ad buyers only read `https://bunon.xyz/ads.txt`, not the subdomain's copy. Either:
   - If nothing is hosted at `bunon.xyz`: add `bunon.xyz` as a second domain on this Vercel project (an `A` record for `@` to the IP Vercel shows). `vercel.json` serves `ads.txt` there and redirects every other path to `hostmatrix.bunon.xyz`.
   - If `bunon.xyz` already hosts another site: put the same `ads.txt` line in that site's root.
5. In AdSense → **Sites**, add `bunon.xyz`. Verification uses the `google-adsense-account` meta tag already in the page. Then request a review. Approval can take from a few days to a few weeks.
6. In AdSense → **Privacy & messaging**, create and publish a **European regulations** consent message. Google requires a certified consent tool to serve ads in the EEA, UK and Switzerland, and the page's Consent Mode defaults already wait for it.
7. After approval, either turn on **Auto ads** for the site or create three **Display** ad units (Ads → By ad unit) and paste their slot IDs into `SITE_CONFIG.adSlots` (`explore`, `matrix`, `quiz`). Slots left empty stay hidden.

## Affiliate links

Add an `affiliateUrl` to any entry in `HOSTS_DATABASE` to use a referral link instead of `url`. Those links get `rel="sponsored"` automatically, and the footer and privacy page already include an affiliate disclosure.

## SEO

- Title, meta description, canonical URL, Open Graph/Twitter cards (`og-image.png`), JSON-LD, `favicon.svg`, `robots.txt` and `sitemap.xml` are set up for `https://hostmatrix.bunon.xyz/`.
- Add the site in [Google Search Console](https://search.google.com/search-console) (a **Domain** property for `bunon.xyz` verified by DNS TXT record covers all subdomains), then submit `https://hostmatrix.bunon.xyz/sitemap.xml`.
- Update `<lastmod>` in `sitemap.xml` when content changes.

The app loads Tailwind CSS, Font Awesome, and Google Fonts from external CDNs, so those services must be reachable in the visitor's browser.

Hosting plan details in the directory can change; verify current limits and terms with each provider before relying on them.