[Houzz Scraper](https://apify.com/alizarin_refrigerator-owner/houzz-scraper?fpr=data)

# Houzz Professional Scraper

Scrape Houzz home design professionals, contractors, interior designers, and architects. Extract ratings, reviews, project portfolios, and contact info. Built by John Rippy ([https://www.linkedin.com/in/johnrippy/](https://www.linkedin.com/in/johnrippy/)).

---

## Quick Start

### Test with Demo Mode (free, no API key needed)

```
{
  "demoMode": true,
  "professionalUrl": "https://example.com"
}
```

### Run with real data

```
{
  "demoMode": false,
  "scrapeType": "search",
  "professionalUrl": "https://example.com",
  "searchTerm": "General Contractor",
  "location": "Los Angeles, CA",
  "professionalType": "all",
  "minRating": 0,
  "includeReviews": true,
  "maxReviewsPerProfessional": 20,
  "includeProjects": false,
  "maxResults": 50,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": [
      "RESIDENTIAL"
    ]
  },
  "cookieKvStoreName": "cookie-sessions"
}
```

---

## Input Parameters

| Parameter | Type | Default | Required | Description |
| --- | --- | --- | --- | --- |
| `scrapeType` | string | `"search"` | No | What type of data to scrape |
| `professionalUrl` | string | - | No | Direct Houzz professional page URL |
| `searchTerm` | string | `"General Contractor"` | No | Professional type to search for (e.g., 'Interior Designer', 'General Contractor') |
| `location` | string | `"Los Angeles, CA"` | No | City, state or zip code (e.g., 'Los Angeles, CA') |
| `professionalType` | string | `"all"` | No | Filter by professional category |
| `minRating` | number | `0` | No | Minimum star rating (0-5) |
| `includeReviews` | boolean | `true` | No | Scrape customer reviews for each professional |
| `maxReviewsPerProfessional` | integer | `20` | No | Maximum reviews to scrape per profile |
| `includeProjects` | boolean | `false` | No | Extract portfolio project images |
| `maxResults` | integer | `50` | No | Maximum number of professionals to scrape |
| `proxyConfiguration` | object | `{"useApifyProxy":true,"apifyProxyGroups":["RESIDENTIAL"]}` | No | Proxy settings for the scraper |
| `demoMode` | boolean | `true` | No | Return sample data without actual scraping (for testing) |
| `webhookUrl` | string | - | No | Optional webhook URL to receive data as it's scraped |
| `sessionCookies` | string | - | No | JSON array of cookies from Cookie-Editor browser extension. Export cookies while logged in to Houzz for full data access. |
| `cookieStorageKey` | string | - | No | Key name to load cookies from the Cookie Manager KV store. If set and no manual sessionCookies are provided, the actor loads cookies from the named KV store automatically. Use this with the Cookie Manager actor for automated cookie rotation. |
| `cookieKvStoreName` | string | `"cookie-sessions"` | No | Name of the Apify Key-Value store where Cookie Manager saves cookies. Defaults to 'cookie-sessions' if not set. |

---

## Pricing

This actor uses **pay-per-event** billing:

| Event | Description | Price |
| --- | --- | --- |
| Professional Scraped | Each Houzz professional profile scraped | $0.05 |

**Demo mode is free** -- no charges for sample data.

---

## Troubleshooting

### "API error 429" or "Rate limit"

Too many requests. Wait a minute and try again, or reduce the number of items per run.

### No results or empty dataset

Check the run log for error messages. Common causes:

- Invalid input format (check the examples above)
- The target data doesn't exist or is too small to track

### How do I test without an API key?

Enable **Demo Mode** in the input. This returns realistic sample data so you can verify the output format works for your workflow.

---

**Built by John Rippy | [Actor Arsenal](https://actorarsenal.com)**