[Houzz Scraper](https://apify.com/abotapi/houzz-scraper?fpr=data)

# Houzz Pros Scraper

Pull every interior designer, architect, builder, kitchen remodeler, photographer, contractor and home stylist on Houzz, with 35+ fields per pro: ratings, reviews, awards, services, full address, GPS, contact, social links, project counts, and a featured review excerpt. Three modes (search by category + location, free-text keyword, paginate by URL) plus a fourth `review` mode for deep-fetching specific pros. Works on every Houzz country site (.com, .com.au, .co.nz, .ie, .com.sg, .in, .it, .es, .dk, .se).

## Why This Scraper?

- **35+ fields per pro** vs 8-10 in alternatives: per-aspect ratings (communication / on time / quality / value), Best of Houzz awards by year, full services list, operating hours, GPS, social links across six platforms.
- **Four modes in one actor**: search by category + city, free-text keyword, paste pro URLs, or paste any directory URL.
- **Worldwide coverage**: same input shape for ten Houzz country sites; the actor maps your category choice to the right country category id automatically.
- **Filter at the source**: minimum rating, minimum review count, verified-only toggle. Sort by rating, review count, newest, or alphabetical.
- **15 pros per fetch + gzip on the wire**: under one cent of bandwidth per 1000 results on standard plans.
- **Free plan compatible**: the default proxy setting works on Apify's free tier without paid Residential.
- **Self-recovering**: connections are refreshed automatically on the rare soft-rejection; pagination resumes seamlessly.

## Data You Get

> Sample shape, values are illustrative placeholders, not from a live listing.

| Field | Example |
| --- | --- |
| `id` | `"00000001"` |
| `userId` | `00000001` |
| `professionalId` | `0000` |
| `displayName` | `"Sample Design Studio"` |
| `userName` | `"sample_studio"` |
| `proType` | `2` |
| `proTypeDisplayName` | `"Interior Designers & Decorators"` |
| `country` | `"US"` |
| `state` | `"NY"` |
| `city` | `"New York"` |
| `zip` | `"10001"` |
| `latitude` | `40.0000` |
| `longitude` | `-74.0000` |
| `formattedAddress` | `"100 Sample Ave, New York, NY 10001"` |
| `formattedPhone` | `"(000) 000-0000"` |
| `website` | `"sample-studio.com"` |
| `numReviews` | `0` |
| `reviewRating` | `0.0` |
| `aspectCommunication` | `0.0` |
| `aspectOnTime` | `0.0` |
| `aspectQuality` | `0.0` |
| `aspectValue` | `0.0` |
| `featuredReviewText` | `"Sample featured review excerpt appears here when present in the page."` |
| `mostRecentReview` | `{ "id": "0", "rating": 0.0, "text": "...", "author": "Reviewer Name", "date": "2026-01-01" }` |
| `verified` | `false` |
| `hasVerifiedLicense` | `false` |
| `awards` | `[]` (Best of Houzz year + category when present) |
| `badges` | `[]` |
| `merits` | `[]` |
| `aboutMe` | `"Sample about-me text describing the pro's specialty and approach."` |
| `aiDescription` | `"Sample AI summary if Houzz pre-generated one."` |
| `servicesProvided` | `["3D Rendering", "Bathroom Design", "..."]` |
| `areasServed` | `["..."]` |
| `costEstimate` | `null` |
| `socialFacebook` | `"https://facebook.com/..."` |
| `socialInstagram` | `"https://instagram.com/..."` |
| `socialLinkedin` | `null` |
| `profileImage` | `{ ... }` (Houzz CDN image record) |
| `coverImage` | `{ ... }` |
| `siteOrigin` | `"houzz.com"` |
| `url` | `"https://www.houzz.com/professionals/interior-designers-and-decorators/sample-design-studio-pfvwus-pf~00000001"` |
| `mode` | `"search"` |
| `category` | `"interior-designers"` |
| `location` | `"New-York--NY"` |

## How to Use

**Search by category + location (most common):**

```
{
  "mode": "search",
  "site": "houzz.com",
  "category": "interior-designers",
  "locations": ["New-York--NY", "Los-Angeles--CA"],
  "maxPages": 5,
  "maxListings": 100
}
```

**Search Australia for Sydney architects:**

```
{
  "mode": "search",
  "site": "houzz.com.au",
  "category": "architects",
  "locations": ["Sydney--NSW"],
  "minRating": 4,
  "minReviewCount": 5,
  "sortBy": "rating-desc",
  "maxPages": 10
}
```

**Free-text keyword search:**

```
{
  "mode": "keyword",
  "site": "houzz.com",
  "queries": ["mid century kitchen designer", "sustainable architect"],
  "locations": ["San-Francisco--CA"],
  "maxPages": 3
}
```

**Deep-fetch specific pros (review mode, with full reviews + aggregates):**

```
{
  "mode": "review",
  "proUrls": [
    "https://www.houzz.com/professionals/interior-designers-and-decorators/sample-pro-pfvwus-pf~00000001",
    "https://www.houzz.com/pro/another-pro/__public"
  ]
}
```

**Paste any Houzz directory URL:**

```
{
  "mode": "url",
  "urls": [
    "https://www.houzz.com/professionals/kitchen-and-bath/probr0-bo~t_11790",
    "https://www.houzz.com.au/professionals/interior-designers-and-decorators/c/Sydney--NSW"
  ],
  "maxPages": 3
}
```

## Input Parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `mode` | enum | `"search"` | `"search"`, `"keyword"`, `"review"`, or `"url"`. |
| `site` | enum | `"houzz.com"` | Country site: houzz.com, houzz.com.au, houzz.co.nz, houzz.ie, houzz.com.sg, houzz.in, houzz.it, houzz.es, houzz.dk, houzz.se. |
| `category` | enum | `"interior-designers"` | Service category. Auto-mapped per country. |
| `locations` | array of strings | `[]` | Format: `City--State`, `City--Country`, or just `State` / `Region`. Empty = all locations for the category. |
| `queries` | array of strings | `[]` | Free-text queries (keyword mode). |
| `proUrls` | array of strings | `[]` | Houzz pro profile URLs or raw pro IDs (review mode). |
| `urls` | array of strings | `[]` | Any Houzz directory URLs (url mode). |
| `minRating` | integer | `0` | Filter to pros with rating >= this (0-5). |
| `minReviewCount` | integer | `0` | Filter to pros with at least N reviews. |
| `verifiedOnly` | boolean | `false` | Skip pros without a verified company profile. |
| `sortBy` | enum | `"default"` | `default`, `rating-desc`, `review-count-desc`, `newest`, `name-asc`. |
| `maxPages` | integer | `5` | Max pagination pages per search. 15 pros per page. |
| `maxListings` | integer | `0` | Total cap across all searches. 0 = unlimited (still capped by maxPages). |
| `fetchDetails` | boolean | `false` | Visit each pro detail page for richer fields. Auto-on in review mode. |
| `proxy` | object | Apify Datacenter | Proxy configuration. Datacenter works fine; Residential recommended for very large runs. |

## Output Example

> Sample shape, values are illustrative placeholders, not from a live listing.

```
{
  "id": "00000001",
  "userId": 1,
  "professionalId": 0,
  "displayName": "Sample Design Studio",
  "userName": "sample_studio",
  "proType": 2,
  "proTypeDisplayName": "Interior Designers & Decorators",
  "country": "US",
  "state": "NY",
  "city": "New York",
  "zip": "10001",
  "latitude": 40.0,
  "longitude": -74.0,
  "formattedAddress": "100 Sample Ave, New York, NY 10001",
  "formattedPhone": "(000) 000-0000",
  "website": "sample-studio.com",
  "numReviews": 0,
  "reviewRating": 0.0,
  "aspectCommunication": null,
  "aspectOnTime": null,
  "aspectQuality": null,
  "aspectValue": null,
  "featuredReview": {
    "id": "0",
    "rating": 0.0,
    "text": "Sample featured review excerpt appears here.",
    "author": "Reviewer Name",
    "date": "2026-01-01"
  },
  "mostRecentReview": {
    "id": "0",
    "rating": 0.0,
    "text": "Sample most-recent review excerpt.",
    "author": "Reviewer Name",
    "date": "2026-01-01"
  },
  "verified": true,
  "awards": [],
  "badges": [],
  "aboutMe": "Sample about-me text describing the pro's specialty.",
  "servicesProvided": ["3D Rendering", "Bathroom Design", "Kitchen Design"],
  "areasServed": ["Manhattan", "Brooklyn"],
  "socialFacebook": null,
  "socialInstagram": null,
  "siteOrigin": "houzz.com",
  "url": "https://www.houzz.com/professionals/interior-designers-and-decorators/sample-design-studio-pfvwus-pf~00000001",
  "mode": "search",
  "category": "interior-designers",
  "location": "New-York--NY"
}
```

## Plan Requirement

Works on every Apify plan including the free tier. The default proxy setting (Apify Datacenter) is enabled on every plan. Residential proxy is optional and recommended only for very large runs (10k+ results) where you want extra exit-IP rotation. Country-pin the proxy to match the Houzz country site you are scraping for the most stable results.