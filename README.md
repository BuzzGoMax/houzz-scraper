[Houzz Scraper](https://apify.com/jungle_synthesizer/houzz-scraper?fpr=data)

# Houzz Scraper — Home-Pro Directory Leads

Scrape the Houzz professional directory for contractors, architects, interior designers, landscape architects, kitchen & bath pros, and the rest. Get name, profession, phone, full address, rating, review count, badges, and profile URL per record.

---

## What It Does

Two-phase crawl against [houzz.com](https://www.houzz.com):

1. **Listing walk** — picks a profession slug (e.g. `general-contractor`) or a user-supplied URL and walks the category listing pages (`?fi=15`, `?fi=30`, ... 15 cards per page). Every card on Houzz embeds a `LocalBusiness` JSON-LD block with phone, address, geo coordinates, and the profile URL, so listing mode already carries most of the firmographic payload.
2. **Profile details (optional)** — with `mode: "both"` the scraper also opens each professional's profile page and pulls the long-form bio, company-level JSON-LD, and any additional social links.

Houzz profile pages reject direct-IP requests (HTTP 500 from their anti-bot). The scraper runs behind Apify residential proxy with session keepalive and a canary fetch to confirm the exit IP is healthy before the crawl proper.

---

## Input

| Field | Type | Description |
| --- | --- | --- |
| `mode` | string | `listings` (fast — cards only, all core data from JSON-LD) or `both` (cards + profile bio). Default: `listings`. |
| `profession` | string | Houzz profession slug (e.g. `general-contractor`, `interior-designer`, `architect`). Leave blank with `searchUrls`. |
| `minRating` | number | Drop pros with a Houzz rating below this (0–5). Default: `0`. |
| `searchUrls` | array | Explicit Houzz URLs. Accepts category listing pages and individual profile pages. Overrides `profession`. |
| `maxItems` | integer | Cap on returned records. Default: `15`. Set `0` for unlimited. |
| `proxyConfiguration` | object | Proxy config. Defaults to Apify `RESIDENTIAL` in `US` — required for profile pages. |

### Sample input

```
{
  "mode": "listings",
  "profession": "general-contractor",
  "minRating": 4.5,
  "maxItems": 100,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"],
    "apifyProxyCountry": "US"
  }
}
```

Or with explicit URLs:

```
{
  "mode": "both",
  "searchUrls": [
    { "url": "https://www.houzz.com/professionals/interior-designer/probr0-bo~t_11785" },
    { "url": "https://www.houzz.com/professionals/general-contractors/goldenline-remodeling-pfvwus-pf~28977151" }
  ],
  "maxItems": 50
}
```

---

## Output

One record per professional. Every field is a scalar or `null`, except `social_links` which is an array of external URLs.

```
{
  "name": "Goldenline Remodeling",
  "profession": "General Contractors",
  "houzz_url": "https://www.houzz.com/professionals/general-contractors/goldenline-remodeling-pfvwus-pf~28977151",
  "description": "High-End Design & Luxury Craftsmanship in Greater LA...",
  "phone": "(424) 280-3130",
  "street_address": "2000 Wattles Drive",
  "city": "Los Angeles",
  "region": "CA",
  "postal_code": "90046",
  "country": "US",
  "latitude": 34.1074,
  "longitude": -118.3652,
  "image_url": "https://st.hzcdn.com/simgs/b663c8de0ec59db7_0-6284/_.jpg",
  "rating": 5.0,
  "reviews_count": 72,
  "hires_on_houzz": 8,
  "verified_license": true,
  "best_of_houzz": true,
  "social_links": [
    "https://www.facebook.com/Goldenline-Remodeling-102333461490745"
  ]
}
```

---

## Pricing — Pay Per Result

- **$0.002 per record** (+ a fixed start fee per run).
- No seats, no subscriptions. You only pay for records we actually deliver.
- 100 records ≈ $0.20. 1,000 records ≈ $2. 10,000 records ≈ $20.

If a run fails before any records are saved, you aren't charged for results.

---

## Typical Use Cases

- **Home-services lead generation** — pull contractors, remodelers, or designers in your target metro and pipe phone + address into outreach.
- **Market sizing** — count professionals per profession and region, with rating and hire counts as proxies for firm scale.
- **CRM enrichment** — cross-reference an existing book against Houzz ratings and badges.
- **Vendor shortlisting** — filter by `minRating` and profession, then pass straight into a sales tool.

---

## Notes & Limits

- Houzz paginates listings in chunks of 15. The scraper walks forward until `maxItems` is reached or the page count cap (16 pages ≈ 240 cards) is hit.
- `listings` mode returns everything you need for outreach — name, phone, address, rating, badges. Only opt into `both` when you specifically need the long-form bio; it doubles fetch count per record.
- Residential proxy is not optional. Datacenter IPs and raw direct requests return HTTP 500 from Houzz's profile pages.
- Houzz occasionally reshuffles card HTML classes. The scraper falls back to multiple selector candidates for each field, but if you see missing data, open an issue.

---

## Issues, feature requests, custom data

Open an issue on the actor's Apify page or message us through the console. Happy to add fields, tune selectors, or build a variant (reviews-only, specific region) if your workflow needs it.