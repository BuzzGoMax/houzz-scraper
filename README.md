[Houzz Scraper](https://apify.com/lulzasaur/houzz-scraper?fpr=data)

Apify Actor for scraping the Houzz professional directory. Extracts structured data from contractor, architect, designer, and other home professional listings using CheerioCrawler for fast HTML parsing without a browser.

## Features

- Scrape any Houzz professional category (general contractors, architects, designers, plumbers, etc.)
- Extract data from JSON-LD structured data embedded in every search page
- Get phone numbers, full addresses, GPS coordinates, star ratings, and review counts
- Collect social media links (Facebook, Instagram, LinkedIn, Twitter) and business websites
- Automatic pagination through all search results
- Optional detail page scraping for richer profile data

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `searchUrls` | string[] | `["https://www.houzz.com/professionals/general-contractor/probr0-bo~t_11786"]` | Houzz professional search URLs |
| `maxListings` | integer | `100` | Max professionals per search URL (0=unlimited) |
| `scrapeDetails` | boolean | `false` | Visit each profile for extra data |
| `proxyConfiguration` | object | `{}` | Proxy settings (optional) |

### Example Input

```
{
    "searchUrls": [
        "https://www.houzz.com/professionals/general-contractor/probr0-bo~t_11786",
        "https://www.houzz.com/professionals/architect/chicago-il-us-probr0-bo~t_11784~r_4887398"
    ],
    "maxListings": 50,
    "scrapeDetails": false
}
```

> **Note:** Old-format URLs (e.g. `/general-contractor/new-york-ny-us`) are automatically converted to the current Houzz format. To get location-specific results, browse [houzz.com/professionals](https://www.houzz.com/professionals), navigate to your desired category and city, and copy the URL from your browser.

## Output

Each professional record contains:

```
{
    "name": "HOBBS INC",
    "telephone": "(203) 349-8126",
    "imageUrl": "https://st.hzcdn.com/simgs/dd937832065381c7_0-5059/_.jpg",
    "street": null,
    "city": "NEW CANAAN",
    "state": "CT",
    "zip": "06840",
    "country": "US",
    "latitude": 41.151,
    "longitude": -73.4944,
    "areaServed": "New York",
    "rating": 5.0,
    "reviewCount": 15,
    "profileUrl": "https://www.houzz.com/professionals/home-builders/hobbs-inc-pfvwus-pf~403833763",
    "professionalType": "General Contractor",
    "facebook": "https://www.facebook.com/HobbsIncorporated/?ref=aymt_homepage_panel",
    "instagram": null,
    "linkedin": null,
    "twitter": null,
    "website": "https://hobbsinc.com",
    "scrapedAt": "2026-03-17T12:00:00.000Z"
}
```

## How It Works

1. Fetches Houzz professional search pages (server-rendered HTML with no bot protection)
2. Parses JSON-LD `LocalBusiness` entries embedded in each page (up to 30 per page)
3. Extracts star ratings and review counts from HTML elements within each card
4. Classifies social media links from the `sameAs` JSON-LD property
5. Deduplicates sponsored/promoted listings that appear twice
6. Paginates via the `?fi=N` offset parameter (15 unique results per page)
7. Optionally visits individual profile pages for additional details

## Professional Categories

Browse `https://www.houzz.com/professionals` for the full list. Common categories include:

| Category | URL Path |
| --- | --- |
| General Contractors | `general-contractor/probr0-bo~t_11786` |
| Architects | `architect/probr0-bo~t_11784` |
| Kitchen & Bath Designers | `kitchen-and-bath/probr0-bo~t_11790` |
| Interior Designers | `interior-designer/probr0-bo~t_11785` |
| Landscape Architects | `landscape-architect/probr0-bo~t_11788` |
| Home Builders | `home-builders/probr0-bo~t_11823` |
| Plumbers | `plumbing-contractors/probr0-bo~t_11817` |
| Electricians | `electrical-contractors/probr0-bo~t_11818` |
| Painters | `painters/probr0-bo~t_27105` |
| Flooring | `carpet-and-flooring/probr0-bo~t_11799` |

## Quick Start

```
$apify run --purge
```

## Deploy to Apify

```
apify login
apify push
```

## Related Scrapers

More marketplace scrapers and data tools by [lulzasaur](https://apify.com/lulzasaur):

- [AbeBooks Scraper](https://apify.com/lulzasaur/abebooks-scraper) — Rare and used books
- [Bonanza Scraper](https://apify.com/lulzasaur/bonanza-scraper) — Online marketplace listings
- [Contractor License Verifier](https://apify.com/lulzasaur/contractor-license-scraper) — Multi-state license verification
- [Craigslist Scraper](https://apify.com/lulzasaur/craigslist-scraper) — Classifieds and for-sale posts
- [Goodreads Scraper](https://apify.com/lulzasaur/goodreads-scraper) — Book ratings and reviews
- [Grailed Scraper](https://apify.com/lulzasaur/grailed-scraper) — Luxury fashion resale
- [IMDb Scraper](https://apify.com/lulzasaur/imdb-scraper) — Movie and TV show data
- [Nurse License Verifier](https://apify.com/lulzasaur/nurse-license-scraper) — State nursing board verification
- [OfferUp Scraper](https://apify.com/lulzasaur/offerup-scraper) — Local marketplace listings
- [Poshmark Scraper](https://apify.com/lulzasaur/poshmark-scraper) — Fashion resale marketplace
- [PSA Population Report](https://apify.com/lulzasaur/psa-pop-scraper) — Card grading data
- [Redfin Scraper](https://apify.com/lulzasaur/redfin-scraper) — Real estate listings and prices
- [Reverb Scraper](https://apify.com/lulzasaur/reverb-scraper) — Music gear marketplace
- [StubHub Scraper](https://apify.com/lulzasaur/stubhub-scraper) — Event ticket prices
- [Swappa Scraper](https://apify.com/lulzasaur/swappa-scraper) — Used electronics marketplace
- [TCGPlayer Scraper](https://apify.com/lulzasaur/tcgplayer-scraper) — Trading card prices
- [ThriftBooks Scraper](https://apify.com/lulzasaur/thriftbooks-scraper) — Used book prices
- [Thumbtack Scraper](https://apify.com/lulzasaur/thumbtack-scraper) — Local service professionals