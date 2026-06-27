[Houzz Scraper](https://apify.com/fatihtahta/houzz-scraper?fpr=data)

# “All-in-one Houzz Scraper”

**Slug:** fatihtahta/houzz-scraper

## Overview

This actor is built upon the [ValidatedMails.com](https://validatedmails.com) architecture for contact enrichment workflows of pro profiles.

All-in-one Houzz Scraper collects structured data from Houzz search results and listing pages across photos, professionals, stories, and community discussions. Records can include identifiers, URLs, titles, descriptions, locations, review signals, business details, badges, and optional contact enrichment for professional profiles. [Houzz](https://www.houzz.com) is a major platform for home design inspiration, renovation planning, and professional discovery, which makes its public data useful for research, sourcing, and competitive analysis. This actor automates collection across multiple Houzz surfaces in one run, so teams can avoid repetitive manual browsing. The result is a more consistent dataset with less cleanup and less time spent gathering inputs by hand.

## Why Use This Actor

- **Market research and analytics:** Track design trends, service categories, review signals, and regional supply across photos, professionals, stories, and discussions.
- **Product and content teams:** Discover recurring topics, popular aesthetics, editorial angles, and homeowner questions to inform roadmaps and content planning.
- **Developers and data engineers:** Feed analytics stacks, ETL jobs, internal search tools, and downstream APIs with structured Houzz records.
- **Lead generation and enrichment:** Build professional lists with business details, locations, websites, and optional public contact signals for outreach workflows.
- **Monitoring and competitive tracking:** Watch how firms, portfolios, badges, reviews, and category coverage change over time in specific markets.

## Input Parameters

Provide any combination of URLs, queries, and filters to target the Houzz results you need.

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `startUrls` | `string[]` | One or more Houzz search pages, category pages, or individual photo, professional, story, or discussion URLs to scrape directly. | – |
| `queries` | `string[]` | Search keywords to run on Houzz, such as `modern kitchen` or `landscape architect`. Each query follows the selected `queryType`. | – |
| `queryType` | `string` | Houzz section to search when using `queries`. Allowed values: `photos`, `professionals`, `stories`, `discussions`. | `photos` |
| `styles` | `string[]` | Photo-only style filter. Allowed values: `Contemporary`, `Traditional`, `Farmhouse`, `Industrial`, `Rustic`, `Eclectic`, `Tropical`, `Asian`, `Mediterranean`, `Shabby-Chic Style`, `Modern`, `Mid-Century Modern`, `Transitional`, `Scandinavian`, `Coastal`, `Southwestern`, `Craftsman`, `Victorian`, `French Country`. | – |
| `budgets` | `string[]` | Photo-only budget filter. Allowed values: `$`, `$$`, `$$$`, `$$$$`. | – |
| `sizes` | `string[]` | Photo-only size filter. Allowed values: `Compact`, `Medium`, `Large`, `Expansive`. | – |
| `colors` | `string[]` | Photo-only color filter. Allowed values: `Red`, `Orange`, `wood tones`, `yellow`, `blue`, `green`, `turqiuose`, `violet`, `Pink`, `Black & white`, `Black`, `White`, `Gray`, `Beige`, `Brown`. | – |
| `getContacts` | `boolean` | When enabled, professional records include additional public contact signals such as email addresses, phone numbers, and social profile links when available. | `false` |
| `limit` | `integer` | Maximum number of listings to collect across each seed input. Minimum value: `10`. | `50000` |
| `proxyConfiguration` | `object` | Connection settings for the Apify run. Use the default Apify proxy setup or provide your own configuration. | `{"useApifyProxy": true}` |

## Example Input

```
{
  "startUrls": [
    "https://www.houzz.com/professionals/hznb/query/nqrwns/probr2-bo~t_11788~q_landscape-architects-and-landscape-designers"
  ],
  "queries": [
    "landscape architect los angeles"
  ],
  "queryType": "professionals",
  "getContacts": true,
  "limit": 250,
  "proxyConfiguration": {
    "useApifyProxy": true
  }
}
```

## Output

### 6.1 Output destination

The actor writes results to an Apify dataset as JSON records. And the dataset is designed for direct consumption by analytics tools, ETL pipelines, and downstream APIs without post-processing.

### 6.2 Record envelope (all items)

Every dataset item includes these stable identifiers:

- **type** *(string, required)*: Normalized record type. Common values are `photo`, `professional`, `story`, and `discussion`.
- **id** *(number, required)*: Houzz entity identifier.
- **url** *(string, required)*: Canonical or best-available Houzz URL for the entity.

Recommended idempotency key: `type + ":" + id`

Use that key to deduplicate repeated discoveries and to perform reliable upserts in downstream storage.

### 6.3 Examples

Example: photo (`type = "photo"`)

```
{
  "type": "photo",
  "id": 65175,
  "url": "https://www.houzz.com/hznb/photos/presidio-heights-condo-contemporary-dining-room-san-francisco-phvw-vp~65175",
  "title": "Presidio Heights Condo",
  "description": "Trendy dark wood floor dining room photo in San Francisco with white walls",
  "location": "San Francisco",
  "style": 3,
  "ownerId": 34791,
  "ownerName": "Artistic Designs for Living",
  "ownerProfileUrl": "https://www.houzz.com/professionals/interior-designers-and-decorators/artistic-designs-for-living",
  "buzzCount": 14507,
  "questionCount": 13,
  "keywordString": "art arrangement, buffet, chandelier, dining table",
  "imageIds": [
    "20719e0d0c681abc"
  ],
  "altText": "Trendy dark wood floor dining room photo in San Francisco with white walls"
}
```

Example: professional (`type = "professional"`)

```
{
  "type": "professional",
  "state": "CA",
  "estimateDetails": "Custom Homes $2,500,000 to 20 Million. Room Additions $500,000 to $2,000,000. Whole House Renovations from $150,000 to 2 Million. Historical Renovation (HPOZ) $100,000 to 1 Million. ADU $125,000 to $450,000. Outdoor Living Areas $75,000 to $1,500,000. Attic Conversions $250,000 - $500,000. Kitchens $150,000 to $750,000. Estimates based on experience. Your actual costs may vary based on the scope of work for your project. Call today to set up a consultation.",
  "badges": [
    {
      "title": "Houzz Awards",
      "type": "HOUZZ",
      "badgeInfos": [
        {
          "id": 64,
          "title": "Best of Houzz 2024 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2024",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2024-02-13 18:00:19",
          "awardedOnDate": "1707876019",
          "imageNames": [
            "badge_64_7.png",
            "badge_64_8.png"
          ],
          "value": 0
        },
        {
          "id": 60,
          "title": "Best of Houzz 2023 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2023",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2023-01-31 00:51:15",
          "awardedOnDate": "1675155075",
          "imageNames": [
            "badge_60_7.png",
            "badge_60_8.png"
          ],
          "value": 0
        },
        {
          "id": 57,
          "title": "Best of Houzz 2022 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2022",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2022-01-13 23:57:34",
          "awardedOnDate": "1642147054",
          "imageNames": [
            "badge_57_7.png",
            "badge_57_8.png"
          ],
          "value": 0
        },
        {
          "id": 54,
          "title": "Best of Houzz 2021 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2021",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2021-01-19 17:32:11",
          "awardedOnDate": "1611106331",
          "imageNames": [
            "badge_54_7.png",
            "badge_54_8.png"
          ]
        },
        {
          "id": 49,
          "title": "Best of Houzz 2020 - Design",
          "shortTitle": "Best of Houzz 2020",
          "description": "This professional's portfolio was voted most popular by the Houzz community.",
          "selfDescription": "Your portfolio was voted most popular by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2020-02-03 19:31:06",
          "awardedOnDate": "1580787066",
          "imageNames": [
            "badge_48_7.png",
            "badge_48_8.png"
          ]
        },
        {
          "id": 50,
          "title": "Best of Houzz 2020 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2020",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2020-02-03 19:32:52",
          "awardedOnDate": "1580787172",
          "imageNames": [
            "badge_49_7.png",
            "badge_49_8.png"
          ]
        },
        {
          "id": 47,
          "title": "Best of Houzz 2019 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2019",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2019-01-18 16:28:00",
          "awardedOnDate": "1547857680",
          "imageNames": [
            "badge_47_7.png",
            "badge_47_8.png"
          ]
        },
        {
          "id": 43,
          "title": "Best of Houzz 2018 - Design",
          "shortTitle": "Best of Houzz 2018",
          "description": "This professional's portfolio was voted most popular by the Houzz community.",
          "selfDescription": "Your portfolio was voted most popular by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2018-01-14 12:47:42",
          "awardedOnDate": "1515962862",
          "imageNames": [
            "badge_43_7.png",
            "badge_43_8.png"
          ]
        },
        {
          "id": 40,
          "title": "Best of Houzz 2017 - Design",
          "shortTitle": "Best of Houzz 2017",
          "description": "This professional's portfolio was voted most popular by the Houzz community.",
          "selfDescription": "Your portfolio was voted most popular by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2017-01-13 17:26:51",
          "awardedOnDate": "1484357211",
          "imageNames": [
            "badge_40_7.png",
            "badge_40_8.png"
          ]
        },
        {
          "id": 22,
          "title": "Best of Houzz 2016 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2016",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2016-01-11 12:35:15",
          "awardedOnDate": "1452544515",
          "imageNames": [
            "badge_22_7.png",
            "badge_22_8.png"
          ]
        },
        {
          "id": 16,
          "title": "Best of Houzz 2015 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2015",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2015-01-13 19:30:29",
          "awardedOnDate": "1421206229",
          "imageNames": [
            "badge_16_7.png",
            "badge_16_8.png"
          ]
        },
        {
          "id": 13,
          "title": "Best of Houzz 2014 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2014",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2014-01-30 17:36:20",
          "awardedOnDate": "1391132180",
          "imageNames": [
            "badge_13_7.png",
            "badge_13_8.png"
          ]
        },
        {
          "id": 8,
          "title": "Best of Houzz 2013 - Client Satisfaction",
          "shortTitle": "Best of Houzz 2013",
          "description": "This professional was rated at the highest level for client satisfaction by the Houzz community.",
          "selfDescription": "You were rated at the highest level for client satisfaction by the Houzz community.",
          "type": 1,
          "cta": "",
          "modified": "2013-01-19 15:41:16",
          "awardedOnDate": "1358638876",
          "imageNames": [
            "badge_8_7.png",
            "badge_8_8.png"
          ]
        }
      ]
    },
    {
      "title": "Houzz Badges",
      "type": "COMMUNITY",
      "badgeInfos": [
        {
          "id": 52,
          "title": "Houzz Pro Software User",
          "shortTitle": "Houzz Pro Software User",
          "description": "This pro uses Houzz Pro Software, making it easy for clients to stay informed with easy-to-use project communication and collaboration tools.",
          "selfDescription": "You use Houzz Pro Software, making it easy for clients to stay informed with easy-to-use project communication and collaboration tools.",
          "type": 10,
          "cta": "",
          "modified": "2025-05-16 09:35:48",
          "awardedOnDate": "1747413348",
          "imageNames": [
            "badge_52_7.png",
            "badge_52_8.png"
          ]
        },
        {
          "id": 5,
          "title": "Star Houzzer",
          "shortTitle": "Star Houzzer",
          "description": "This user posted a review on Houzz.",
          "selfDescription": "You posted a review on Houzz.",
          "type": 2,
          "cta": "<div class=\"badgeCTA\"><a class=\"colorLink\" href=\"https://www.houzz.com/writeReview/cmd=s\">Review a Pro to Get Started&nbsp;&raquo;</a></div>",
          "modified": "2012-11-30 14:53:06",
          "awardedOnDate": "1354304309",
          "imageNames": [
            "badge_5_5.png",
            "badge_5_7.png",
            "badge_5_8.png"
          ]
        },
        {
          "id": 6,
          "title": "5 Star Houzzer",
          "shortTitle": "5 Star Houzzer",
          "description": "This user posted 5 reviews on Houzz.",
          "selfDescription": "You posted 5 reviews on Houzz.",
          "type": 2,
          "cta": "<div class=\"badgeCTA\"><a class=\"colorLink\" href=\"https://www.houzz.com/writeReview/cmd=s\">Review a Pro to Get Started&nbsp;&raquo;</a></div>",
          "modified": "2013-02-28 16:50:42",
          "awardedOnDate": "1362027521",
          "imageNames": [
            "badge_6_5.png",
            "badge_6_7.png",
            "badge_6_8.png"
          ]
        },
        {
          "id": 19,
          "title": "Houzz Influencer",
          "shortTitle": "Influencer",
          "description": "This professional's knowledge and advice is highly valued by the Houzz community.",
          "selfDescription": "Your knowledge and advice is highly valued by the Houzz community.",
          "type": 4,
          "cta": "",
          "modified": "2015-08-21 04:42:14",
          "awardedOnDate": "1440157334",
          "imageNames": [
            "badge_19_7.png",
            "badge_19_8.png"
          ]
        },
        {
          "id": 20,
          "title": "Recommended on Houzz",
          "shortTitle": "Recommended on Houzz",
          "description": "The Houzz Community recommends this professional.",
          "selfDescription": "The Houzz Community recommends you.",
          "type": 5,
          "cta": "",
          "modified": "2015-08-21 04:42:14",
          "awardedOnDate": "1440157334",
          "imageNames": [
            "badge_20_7.png",
            "badge_20_8.png"
          ]
        },
        {
          "id": 33,
          "title": "10,000 Ideabook Saves",
          "shortTitle": "10,000 Ideabook Saves",
          "description": "This professional's photos have been added more than 10,000 times to ideabooks on Houzz",
          "selfDescription": "Your photos have been added more than 10,000 times to ideabooks on Houzz",
          "type": 8,
          "cta": "",
          "modified": "2016-07-29 09:14:05",
          "awardedOnDate": "1469808845",
          "imageNames": [
            "badge_33_7.png",
            "badge_33_8.png"
          ]
        }
      ]
    },
    {
      "title": "Houzz Pro Highlight Badges",
      "type": "PRO_HIGHLIGHT",
      "badgeInfos": [
        {
          "id": 1000,
          "title": "Best of Houzz winner",
          "shortTitle": "Best of Houzz winner",
          "description": "The annual Best of Houzz Award recognizes the top-rated & top-contributing home pros around the world.",
          "selfDescription": "The annual Best of Houzz Award recognizes the top-rated & top-contributing home pros around the world.",
          "type": 16,
          "cta": "",
          "modified": "2025-01-08 09:06:30",
          "awardedOnDate": "1736355990",
          "imageNames": [
            "badge1_7.png",
            "badge1_8.png"
          ]
        },
        {
          "id": 1002,
          "title": "Family owned",
          "shortTitle": "Family owned",
          "description": "This business is proud to be family owned and operated.",
          "selfDescription": "This business is proud to be family owned and operated.",
          "type": 16,
          "cta": "",
          "modified": "2025-01-08 09:06:31",
          "awardedOnDate": "1736355991",
          "imageNames": [
            "people_three_7.png",
            "people_three_8.png"
          ]
        },
        {
          "id": 1016,
          "title": "High-end",
          "shortTitle": "High-end",
          "description": "This business has self-identified as specializing in luxury craftsmanship.",
          "selfDescription": "This business has self-identified as specializing in luxury craftsmanship.",
          "type": 16,
          "cta": "",
          "modified": "2025-01-08 09:06:32",
          "awardedOnDate": "1736355992",
          "imageNames": [
            "diamond_7.png",
            "diamond_8.png"
          ]
        },
        {
          "id": 4000,
          "title": "Hired on Houzz",
          "shortTitle": "Hired on Houzz",
          "description": "The number of homeowners who contacted a pro through Houzz and then hired that pro for a project.",
          "selfDescription": "The number of homeowners who contacted a pro through Houzz and then hired that pro for a project.",
          "type": 11,
          "cta": "",
          "modified": "2026-03-16 12:26:10",
          "awardedOnDate": "1773689170",
          "imageNames": [
            "handshake_7.png",
            "handshake_8.png"
          ],
          "value": 15
        },
        {
          "id": 4004,
          "title": "Responds Quickly",
          "shortTitle": "Responds Quickly",
          "description": "Show pros who typically respond in the same day",
          "selfDescription": "Show pros who typically respond in the same day",
          "type": 11,
          "cta": "",
          "modified": "2026-03-08 12:07:11",
          "awardedOnDate": "1772996831",
          "imageNames": [
            "timer_7.png",
            "timer_8.png"
          ]
        },
        {
          "id": 4005,
          "title": "Provides 3D Visualization",
          "shortTitle": "Provides 3D Visualization",
          "description": "",
          "selfDescription": "",
          "type": 11,
          "cta": "",
          "modified": "2025-05-16 09:35:48",
          "awardedOnDate": "1747413348",
          "imageNames": [
            "room_7.png",
            "room_8.png"
          ]
        }
      ]
    }
  ],
  "aboutMe": "Los Angeles Area Award Winning Home Builder\r\n1. Custom Homes\r\n2. Fire-Resistant Home Builder\r\n3. Award Winning Kitchen & Bathroom Designs\r\n4. Major Renovations and Additions\r\n5. Amazing Outdoor Living Areas -Seamless Transition from indoors to Outdoors\r\n\r\nIf you are a homeowner who has lost your home in the recent Los Angeles fires, we understand how devastating this experience can be. Here’s how we can help rebuild your home:\r\n1. New Home Design: Choosing the right design for your new home is crucial. We can assist you in selecting a site that minimizes fire risk and maximizes safety.\r\n2.Fire-Resistant Materials: We utilize materials that are specifically designed to resist fire, ensuring that your new home is built to withstand extreme conditions.\r\n3.Landscaping for Safety: Implementing fire-resistant landscaping is essential. This includes:\r\na. Removing highly flammable plants and replacing them with fire-resistant species.\r\nb. Creating buffer zones around your home to reduce fire hazards.\r\nc. Regular maintenance of landscaping to keep it safe and manageable [1].\r\n4.Designing for Fire Safety: Our team will work with you to design a home that incorporates features such as:\r\na. Non-combustible roofing and siding materials.\r\nb. Fire-resistant windows and doors.\r\nc. Proper spacing and arrangement of trees and shrubs to prevent fire spread [1].\r\nSupport Throughout the Rebuilding Process\r\n•Consultation and Planning: We offer comprehensive consultations to understand your needs and preferences, guiding you through Insurance Company, Plans and expediting plans permitting for your rebuild.\r\n•Project Management: Our experienced team will manage the entire construction process, ensuring that your home is built to the highest standards of fire safety and quality.\r\n•Community Resources: We can connect you with local resources and support networks for homeowners affected by the fires, helping you navigate this challenging time.\r\nMoving Forward Together\r\nRebuilding after a fire is not just about constructing a new home; it’s about creating a safe and resilient space for you and your family. At Addition Building & Design, we are committed to helping you every step of the way, ensuring that your new home is not only beautiful but also built to withstand future challenges.\r\nIf you’re ready to start the rebuilding process or have any questions, please reach out to us. Together, we can create a safer future",
  "followingCount": 13783,
  "logo": "https://st.hzcdn.com/simgs/35b3b1890ef0ef4c_3-9543/_.jpg",
  "isInactivePro": false,
  "costEstimate": "$500,000 - 20 million",
  "seoHint": {
    "paths": {
      "ViewProfessional": {
        "command": "professionals",
        "extraSlugs": "design-build-firms",
        "titleSlug": "addition-building-and-design-inc",
        "seoPageType": "pfvwus",
        "indexing": true
      }
    }
  },
  "sameAs": [
    "http://www.AdditionBuildingDesign.com",
    "https://www.facebook.com/AdditionBuildingDesignInc",
    "https://twitter.com/AdditionBuild",
    "https://www.linkedin.com/in/mikerossgeneralcontractor"
  ],
  "spacesCount": 1635,
  "paidProfessional": {
    "shortDescription": "Sherman Oaks Top-Rated Design-Build Firm\n13x Best of Houzz Winner",
    "longDescription": "We are a Design-Build firm providing \"One Stop Shopping\" for all new construction and remodeling projects. We have been voted a top 500 remodeler in the USA 8 times. Call us today for your project!",
    "proStatus": "ENABLED",
    "areasServed": "",
    "offers": "Available for Virtual Meetings!",
    "offersDisclaimer": "",
    "offersExpirationDateTs": 1677625141,
    "proPackagesCount": 11,
    "subscriptions": [
      {
        "id": 216242,
        "professionalType": "General Contractors",
        "subMetroId": 16,
        "location": "CA-Burbank & Glendale",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 216244,
        "professionalType": "General Contractors",
        "subMetroId": 16,
        "location": "CA-Burbank & Glendale",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 212509,
        "professionalType": "General Contractors",
        "subMetroId": 16,
        "location": "CA-Burbank & Glendale",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 212506,
        "professionalType": "General Contractors",
        "subMetroId": 16,
        "location": "CA-Burbank & Glendale",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562216,
        "professionalType": "Home Builders",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367176,
        "professionalType": "General Contractors",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367177,
        "professionalType": "Home Builders",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562221,
        "professionalType": "Design-Build Firms",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562215,
        "professionalType": "General Contractors",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367178,
        "professionalType": "Design-Build Firms",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5163856,
        "professionalType": "Design-Build Firms",
        "subMetroId": 19,
        "location": "CA-LA Westside",
        "packageType": "PRO_SPOTLIGHT",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562220,
        "professionalType": "General Contractors",
        "subMetroId": 21,
        "location": "CA-Los Angeles City",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367182,
        "professionalType": "General Contractors",
        "subMetroId": 21,
        "location": "CA-Los Angeles City",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367183,
        "professionalType": "Design-Build Firms",
        "subMetroId": 21,
        "location": "CA-Los Angeles City",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5163858,
        "professionalType": "Home Builders",
        "subMetroId": 21,
        "location": "CA-Los Angeles City",
        "packageType": "PRO_SPOTLIGHT",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5163857,
        "professionalType": "General Contractors",
        "subMetroId": 21,
        "location": "CA-Los Angeles City",
        "packageType": "PRO_SPOTLIGHT",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367187,
        "professionalType": "General Contractors",
        "subMetroId": 31,
        "location": "CA-San Fernando Valley",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 216241,
        "professionalType": "General Contractors",
        "subMetroId": 31,
        "location": "CA-San Fernando Valley",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 216243,
        "professionalType": "General Contractors",
        "subMetroId": 31,
        "location": "CA-San Fernando Valley",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 212505,
        "professionalType": "General Contractors",
        "subMetroId": 31,
        "location": "CA-San Fernando Valley",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 212508,
        "professionalType": "General Contractors",
        "subMetroId": 31,
        "location": "CA-San Fernando Valley",
        "packageType": "LEGACY",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367186,
        "professionalType": "Home Builders",
        "subMetroId": 33,
        "location": "CA-San Gabriel Valley West",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367185,
        "professionalType": "Design-Build Firms",
        "subMetroId": 33,
        "location": "CA-San Gabriel Valley West",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367184,
        "professionalType": "General Contractors",
        "subMetroId": 33,
        "location": "CA-San Gabriel Valley West",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562208,
        "professionalType": "Home Builders",
        "subMetroId": 33,
        "location": "CA-San Gabriel Valley West",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562209,
        "professionalType": "General Contractors",
        "subMetroId": 33,
        "location": "CA-San Gabriel Valley West",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5562204,
        "professionalType": "General Contractors",
        "subMetroId": 37,
        "location": "CA-Thousand Oaks/Westlake Village/Simi Valley",
        "packageType": "TRUE_SOV",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367181,
        "professionalType": "Design-Build Firms",
        "subMetroId": 37,
        "location": "CA-Thousand Oaks/Westlake Village/Simi Valley",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367180,
        "professionalType": "Home Builders",
        "subMetroId": 37,
        "location": "CA-Thousand Oaks/Westlake Village/Simi Valley",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367179,
        "professionalType": "General Contractors",
        "subMetroId": 37,
        "location": "CA-Thousand Oaks/Westlake Village/Simi Valley",
        "packageType": "TRUE_SOV",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367188,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_ESSENTIAL_GENERAL",
        "status": "ENABLED",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367175,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367162,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5367149,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5361576,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5062415,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_ESSENTIAL_GENERAL",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5062409,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_ESSENTIAL_GENERAL",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 5062274,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_ESSENTIAL_GENERAL",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 4753468,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 4461241,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 2162522,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 2076289,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 1255335,
        "professionalType": "Design-Build Firms",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 1249151,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 1207637,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 1207605,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      },
      {
        "id": 810759,
        "professionalType": "General Contractors",
        "subMetroId": 1000000,
        "location": "United States",
        "packageType": "IVY_UNLIMITED",
        "status": "PENDING",
        "settingsUrl": "https://pro.houzz.com/settings/subscriptions"
      }
    ]
  },
  "profileSlideshow": [
    {
      "photo": {
        "id": 209337443,
        "image": {
          "externalId": "3da1541d07837efb",
          "contentModified": "0972"
        }
      }
    },
    {
      "photo": {
        "id": 182771806,
        "image": {
          "externalId": "fe014eb701e919e7",
          "contentModified": "2618"
        }
      }
    }
  ],
  "galleriesCount": 13,
  "professionalId": 34542,
  "reviewAspectAggregations": [
    {
      "name": "Work Quality",
      "reviewAspectId": 5,
      "sum": 75,
      "count": 15,
      "average": 50
    },
    {
      "name": "Communication",
      "reviewAspectId": 6,
      "sum": 75,
      "count": 15,
      "average": 50
    },
    {
      "name": "Value",
      "reviewAspectId": 7,
      "sum": 75,
      "count": 15,
      "average": 50
    }
  ],
  "projectsCount": 148,
  "formattedPhone": "(818) 423-5655",
  "organizationMembersCount": 0,
  "isProVerified": true,
  "country": "US",
  "id": 34542,
  "city": "Sherman Oaks",
  "isSponsoredResult": false,
  "zip": "91403",
  "image": "https://st.hzcdn.com/simgs/35b3b1890ef0ef4c_3-9543/_.jpg",
  "proType": 11,
  "userId": 649046,
  "formattedAddress": "4335 Van Nuys Blvd. Suite 354<br/>Sherman Oaks, CA 91403",
  "latitude": "34.1514",
  "merits": {
    "meritProfileBadges": [
      {
        "id": 4000,
        "title": "Hired on Houzz",
        "titleWithValue": "15 Hires on Houzz",
        "icon": "handshake",
        "description": "The number of homeowners who contacted a pro through Houzz and then hired that pro for a project.",
        "value": 15
      },
      {
        "id": 4004,
        "title": "Responds Quickly",
        "titleWithValue": "Responds Quickly",
        "icon": "timer",
        "description": "Show pros who typically respond in the same day"
      },
      {
        "id": 4005,
        "title": "Provides 3D Visualization",
        "titleWithValue": "Provides 3D Visualization",
        "icon": "room",
        "description": ""
      }
    ]
  },
  "areasServed": "Bel Air, Beverly Hills, Brentwood Los Angeles, Burbank, Canoga Park, Chatsworth, Culver City, Encino, Glendale, Granada Hills, Hidden Hills, Hollywood, Los Angeles, Mission Hills, North Hollywood, Northridge, Pacific Palisades, San Fernando, Santa Monica, Sherman Oaks, Studio City, Sun Valley, Sunland, Tarzana, Toluca Lake, Topanga, Universal City, Valley Village, West Hollywood, Winnetka, Woodland Hills, West Los Angeles, Century City, South Bay, El Segundo, Marina Del Ray, Silver Lake, Pasadena, La Crescenta, Westlake Village, Thousand Oaks, Moorpark, La Canada Flintridge, Westwood, Windsor Square, Beverly Glen, Los Feliz, Beverly Crest, Holmby Hills, Hancock Park",
  "name": "Addition Building & Design, Inc.",
  "rawDomain": "http://www.AdditionBuildingDesign.com",
  "hasVerifiedLicense": true,
  "reviewsCount": 95,
  "followerCount": 1783,
  "longitude": "-118.4603",
  "companyHighlightVideo": {
    "id": 16086,
    "externalId": "49a0e0880dd85e4b",
    "wochitVideoId": "69330bad118ea37a8e7d97ec",
    "duration": -1,
    "servingUrl": "https://player.vimeo.com/external/1143892327.m3u8?s=e4df45e29323956a20b87c534201f05e340fc786&oauth2_token_id=1384357804",
    "status": "PROCESSED",
    "published": true,
    "modified": "2025-12-05 08:47:28",
    "thumbImage": {
      "externalId": "4b25920509330ca0",
      "contentModified": "3249"
    },
    "entity": {
      "type": "User",
      "id": 649046
    }
  },
  "location": "Sherman Oaks, CA",
  "proSkuId": 67,
  "hasVerifiedKyc": false,
  "licenseNumber": "925471",
  "videosCount": 0,
  "license": {
    "licenseStatus": 2,
    "meshJson": "{\"items\": [{\"claims\": [{\"name\": \"jurisdiction\", \"value\": \"CA\"}, {\"name\": \"licenseIdentifier\", \"value\": \"925471\"}, {\"name\": \"licenseeName\", \"value\": \"Addition Building &amp; Design, Inc.\"}, {\"name\": \"licenseCategory\", \"value\": \"homeServices\"}, {\"name\": \"licenseType\", \"value\": \"other\"}], \"labels\": [\"professional_license_verified\"], \"results\": [{\"name\": \"licenseIdentifier\", \"value\": \"925471\"}, {\"name\": \"jurisdiction\", \"value\": \"CA\"}, {\"name\": \"licenseeName\", \"value\": \"ADDITION BUILDING &amp; DESIGN INC\"}, {\"name\": \"licenseAuthorityBoard\", \"value\": \"Department of Consumer Affairs Contractors State License Board\"}, {\"name\": \"licenseCategory\", \"value\": \"homeServices\"}, {\"name\": \"licenseTypeAsIs\", \"value\": \"B - GENERAL BUILDING\"}, {\"name\": \"licenseStatus\", \"value\": \"Active\"}, {\"name\": \"licenseStatusAsIs\", \"value\": \"This license is current and active.\"}, {\"name\": \"licenseExpirationDate\", \"value\": \"1798675200000\"}, {\"name\": \"licenseIssuedDate\", \"value\": \"1228089600000\"}, {\"name\": \"dbaName\", \"value\": \"ADDITION BUILDING &amp; DESIGN INC\"}], \"vspecId\": \"11fb7baf-7027-4a74-9ff5-f3576e539703\"}], \"labels\": [\"approved\"], \"orderId\": \"dd9ab692-e532-442e-a02e-c592c8b8c916\", \"businessId\": \"b00f7d4b-e0fa-46a9-8e6d-e8d01b122379\", \"orderStatus\": \"monitoring\", \"lastUpdatedTime\": 1774196247996, \"orderCreationTime\": 1702420012838, \"orderCompletionTime\": null}",
    "meshSession": "{\"mesh_url\": \"https://www.one-click-verify.mesh.id?meshUserId=b00f7d4b-e0fa-46a9-8e6d-e8d01b122379\", \"mesh_user_id\": \"b00f7d4b-e0fa-46a9-8e6d-e8d01b122379\"}",
    "businessName": "ADDITION BUILDING &amp; DESIGN INC",
    "licenseNum": "925471",
    "issueBy": "Department of Consumer Affairs Contractors State License Board",
    "licenseType": "B - GENERAL BUILDING",
    "verifiedDate": "1774196247"
  },
  "title": "Addition Building & Design, Inc.",
  "profileImage": {
    "externalId": "35b3b1890ef0ef4c",
    "contentModified": "9543",
    "imageId": 3060032,
    "width": 217,
    "height": 217,
    "whiteBg": 0
  },
  "commentsCount": 1772,
  "breadcrumbs": [
    "Home Improvement Professionals",
    "Design-Build Firms",
    "Accessory Dwelling Units",
    "Home Remodeling",
    "Home Additions",
    "Universal Design",
    "Basement Remodeling"
  ],
  "stats": {
    "galleriesCount": 13,
    "projectsCount": 149,
    "spacesCount": 1635,
    "followerCount": 1783,
    "followingCount": 13783,
    "videosCount": 0
  },
  "numReviews": 95,
  "address": "4335 Van Nuys Blvd. Suite 354",
  "offersFigureFinancing": true,
  "isProVerificationEnabled": false,
  "proDirectoryCardSetting": {
    "showHighlightVideo": true,
    "budgetLevels": [
      "1v3",
      "1v4"
    ],
    "badgeSettings": [
      {
        "badgeOptionId": 4000,
        "presentableMinValue": 1
      },
      {
        "badgeOptionId": 4001,
        "presentableMinValue": 1
      }
    ],
    "badgeSettingsMap": {
      "4000": {
        "presentableMinValue": 1
      },
      "4001": {
        "presentableMinValue": 1
      }
    }
  },
  "socialLinks": [
    {
      "type": "LINK_TYPE_FB",
      "url": "https://www.facebook.com/AdditionBuildingDesignInc",
      "trackedUrl": "https://www.houzz.com/trk/aHR0cHM6Ly93d3cuZmFjZWJvb2suY29tL0FkZGl0aW9uQnVpbGRpbmdEZXNpZ25JbmM/c4f8e4e506f4d9a2908edd6dbde3cb30/ue/NjQ5MDQ2/80fc985bd1ad5fbc7df7aeb8dab35b96"
    },
    {
      "type": "LINK_TYPE_TWITTER",
      "url": "https://twitter.com/AdditionBuild",
      "trackedUrl": "https://www.houzz.com/trk/aHR0cHM6Ly90d2l0dGVyLmNvbS9BZGRpdGlvbkJ1aWxk/8124387203cafb5daf2dc99a1cafbc79/ue/NjQ5MDQ2/80fc985bd1ad5fbc7df7aeb8dab35b96"
    },
    {
      "type": "LINK_TYPE_LINKEDIN",
      "url": "https://www.linkedin.com/in/mikerossgeneralcontractor",
      "trackedUrl": "https://www.houzz.com/trk/aHR0cHM6Ly93d3cubGlua2VkaW4uY29tL2luL21pa2Vyb3NzZ2VuZXJhbGNvbnRyYWN0b3I/45e8b5a6d3bcaff6b2600d16b5ff3a31/ue/NjQ5MDQ2/80fc985bd1ad5fbc7df7aeb8dab35b96"
    }
  ],
  "telephone": "(818) 398-4222",
  "standouts": {
    "profileBadges": [
      {
        "id": 1000,
        "title": "13x Best of Houzz winner",
        "titleWithValue": "Best of Houzz winner",
        "icon": "badge1",
        "description": "The annual Best of Houzz Award recognizes the top-rated & top-contributing home pros around the world."
      },
      {
        "id": 1002,
        "title": "Family owned",
        "titleWithValue": "Family owned",
        "icon": "people_three",
        "description": "This business is proud to be family owned and operated."
      },
      {
        "id": 1016,
        "title": "High-end",
        "titleWithValue": "High-end",
        "icon": "diamond",
        "description": "This business has self-identified as specializing in luxury craftsmanship."
      }
    ]
  },
  "contactName": "Mike Ross",
  "url": "https://www.houzz.com/professionals/design-build-firms/professionals/design-build-firms/addition-building-and-design-inc",
  "isVideoConsultationEnabled": false,
  "coverImage": {
    "externalId": "1952ebb405782e71",
    "contentModified": "9716",
    "width": 1904,
    "height": 545,
    "whiteBg": 1,
    "hires": 5
  },
  "domain": "https://www.houzz.com/trk/aHR0cDovL3d3dy5BZGRpdGlvbkJ1aWxkaW5nRGVzaWduLmNvbQ/63bc848c354598c262583defdb07a764/ue/NjQ5MDQ2/80fc985bd1ad5fbc7df7aeb8dab35b96",
  "proTypeDisplayName": "Design-Build Firms",
  "contactDetails": {
    "sourceUrl": "https://www.additionbuildingdesign.com/",
    "status": "ok",
    "emails": [
      "Christine@additionbuildingdesign.com",
      "Mike@additionbuildingdesign.com"
    ],
    "socialMedia": {
      "facebook": "https://www.facebook.com/AdditionBuildingDesignInc",
      "twitter": "https://twitter.com/mieleusa",
      "youtube": "https://www.youtube.com/@AdditionBuildDesign",
      "instagram": "http://instagram.com/additionbuildingdesign"
    },
    "httpStatus": 200,
    "responseMs": 342,
    "traceId": "b363c58c-3b64-47e6-8432-b15326dff087",
    "requestedUrl": "http://www.additionbuildingdesign.com/"
  },
  "offersGreenSkyFinancing": false,
  "mostRecentReview": {
    "body": "We absolutely love the work Mike did with our bathroom. They laid our new shower floor and put in the shower wall tile in a professional and quick manner. It looks even better than we'd hoped. We would definitely hire this team again.",
    "user": {
      "userName": "webuser_403387691",
      "displayName": "smar zllak"
    }
  },
  "awards": "Nominated Houzz Design/Service National Award Winner 13 times \r\nNominated Top 500 Company in USA Qualified Remodeling\r\nBetter Home Garden Magazine Custom Home Santa Monica \r\nMila Kunis Condo. Makeover!https://www.houzz.com/ideabooks/82192712/video/my-houzz-mila-kunis-surprises-by",
  "houzzLink": "https://www.houzz.com/professionals/design-build-firms/professionals/design-build-firms/addition-building-and-design-inc",
  "website": "http://www.AdditionBuildingDesign.com",
  "budgetLevels": [
    "1v3",
    "1v4"
  ],
  "reviewRating": 49,
  "featuredReview": {
    "textOffset": 0,
    "text": "We recommend Mike and his team without reservation. Their quality of work is excellent, and they work with you to ensure that the process of remodeling your home is handled efficiently and professionally. They can be trusted to follow through on all ",
    "textLength": 250,
    "rawReview": {
      "id": 1344492,
      "rating": 5,
      "modified": 1573163019,
      "projectDate": "2019-09-01",
      "projectPrice": 0,
      "projectPriceAsString": "Choose one",
      "user": {
        "id": 58711005,
        "userId": 58711005,
        "userName": "nancy_cushing_jones",
        "displayName": "Nancy Cushing-Jones"
      }
    }
  },
  "servicesProvided": "3D Rendering, Architectural Design, Architectural Drawings, Building Design, Custom Homes, Design Consultation, Energy-Efficient Homes, Historic Building Conservation, Home Additions, Home Extensions, Home Restoration, Multigenerational Homes, New Home Construction, Pool House Design & Construction, Project Management, Sustainable Design, Universal Design, Custom Home, Design-Build, Major Renovations, Outdoor Living Areas, 2nd Floor Additions"
}
```

Example: story (`type = "story"`)

```
{
  "type": "story",
  "id": 82192712,
  "url": "https://www.houzz.com/ideabooks/82192712",
  "title": "My Houzz: Mila Kunis Surprises By",
  "subtitle": "Editorial story from Houzz",
  "commentCount": 12,
  "topics": [
    "Bathrooms",
    "Renovation"
  ],
  "coverPhoto": {
    "id": 16086
  },
  "user": {
    "displayName": "Houzz Editorial"
  }
}
```

Example: discussion (`type = "discussion"`)

```
{
  "type": "discussion",
  "id": 9876543,
  "url": "https://www.houzz.com/discussions/9876543/kitchen-layout-feedback",
  "title": "Kitchen layout feedback",
  "body": "Would you keep the island or move to a peninsula in this space?",
  "topicTitle": "Kitchens",
  "ownerUserId": 123456,
  "ownerName": "HU-123456",
  "ownerProfileUrl": "https://www.houzz.com/user/webuser_123456",
  "numberOfAnswers": 8,
  "numberOfLikes": 15,
  "lastAnsweredDate": "1732145400",
  "commentsUrl": "https://www.houzz.com/discussions/9876543/comments",
  "bestAnswerId": 7654321
}
```

## Field reference

### All record types

- **type** *(string, required)*: Normalized record type.
- **id** *(number, required)*: Houzz entity identifier.
- **url** *(string, required)*: Best-available Houzz URL.

### Professional fields (`type = "professional"`)

- **professionalId** *(number, optional)*: Houzz professional identifier when provided separately from `id`.
- **userId** *(number, optional)*: Houzz user identifier for the profile owner.
- **name** *(string, optional)*: Business or profile name.
- **title** *(string, optional)*: Listing title or profile heading.
- **proTypeDisplayName** *(string, optional)*: Human-readable professional category.
- **proType** *(number, optional)*: Numeric professional category code.
- **aboutMe** *(string, optional)*: Business description or profile summary.
- **aiDescription** *(string, optional)*: Additional generated summary when present.
- **location** *(string, optional)*: Display location shown on Houzz.
- **formattedAddress** *(string, optional)*: Address string as displayed.
- **address** *(string, optional)*: Street address.
- **city** *(string, optional)*: City.
- **state** *(string, optional)*: State or region.
- **zip** *(string, optional)*: Postal code.
- **country** *(string, optional)*: Country code or name.
- **latitude** *(string or number, optional)*: Latitude.
- **longitude** *(string or number, optional)*: Longitude.
- **distance** *(number, optional)*: Distance value on location-based results.
- **formattedPhone** *(string, optional)*: Primary phone number as displayed.
- **telephone** *(string, optional)*: Alternate phone field when available.
- **website** *(string, optional)*: Primary external website.
- **rawDomain** *(string, optional)*: Website domain captured from the profile.
- **domain** *(string, optional)*: Houzz-tracked outbound website URL.
- **sameAs** *(array[string], optional)*: Related site and social URLs.
- **houzzLink** *(string, optional)*: Houzz profile URL.
- **areasServed** *(string, optional)*: Service area text.
- **servicesProvided** *(string, optional)*: Services list.
- **awards** *(string, optional)*: Awards or recognition text.
- **costEstimate** *(string, optional)*: Budget or pricing range.
- **estimateDetails** *(string, optional)*: Additional pricing notes.
- **budgetLevels** *(array[string], optional)*: Houzz budget bands associated with the professional.
- **numReviews** *(number, optional)*: Review count on directory results.
- **reviewsCount** *(number, optional)*: Review count on profile detail data.
- **reviewRating** *(number, optional)*: Rating value as returned by Houzz.
- **featuredReview.text** *(string, optional)*: Highlighted review excerpt.
- **featuredReview.rawReview.id** *(number, optional)*: Highlighted review identifier.
- **mostRecentReview.body** *(string, optional)*: Most recent review text.
- **mostRecentReview.user.userName** *(string, optional)*: Most recent reviewer username.
- **contactName** *(string, optional)*: Contact person name.
- **contactDetails.status** *(string, optional)*: Contact enrichment status.
- **contactDetails.sourceUrl** *(string, optional)*: Source page used for contact discovery.
- **contactDetails.requestedUrl** *(string, optional)*: Requested website root used for enrichment.
- **contactDetails.emails** *(array[string], optional)*: Public email addresses found.
- **contactDetails.phoneNumbers** *(array[string], optional)*: Public phone numbers found.
- **contactDetails.socialMedia.facebook** *(string, optional)*: Facebook URL.
- **contactDetails.socialMedia.instagram** *(string, optional)*: Instagram URL.
- **contactDetails.socialMedia.linkedin** *(string, optional)*: LinkedIn URL.
- **contactDetails.socialMedia.twitter** *(string, optional)*: Twitter/X URL.
- **contactDetails.socialMedia.youtube** *(string, optional)*: YouTube URL.
- **contactDetails.httpStatus** *(number, optional)*: HTTP status from contact discovery.
- **contactDetails.responseMs** *(number, optional)*: Contact discovery response time in milliseconds.
- **contactDetails.traceId** *(string, optional)*: Trace identifier for contact discovery.
- **badges** *(array[object], optional)*: Badge groups shown on the profile.
- **badges[].title** *(string, optional)*: Badge group title.
- **badges[].type** *(string, optional)*: Badge group type.
- **highlightBadges** *(object, optional)*: Highlight badges shown in directory results.
- **standouts.profileBadges** *(array[object], optional)*: Standout badges from profile detail pages.
- **merits.meritProfileBadges** *(array[object], optional)*: Merit badges and counts.
- **stats.galleriesCount** *(number, optional)*: Gallery count.
- **stats.projectsCount** *(number, optional)*: Project count.
- **stats.spacesCount** *(number, optional)*: Space or photo count.
- **stats.followerCount** *(number, optional)*: Follower count.
- **stats.followingCount** *(number, optional)*: Following count.
- **stats.videosCount** *(number, optional)*: Video count.
- **projectsCount** *(number, optional)*: Project count from the profile store.
- **galleriesCount** *(number, optional)*: Gallery count from the profile store.
- **spacesCount** *(number, optional)*: Photo or space count.
- **followerCount** *(number, optional)*: Follower count.
- **followingCount** *(number, optional)*: Following count.
- **videosCount** *(number, optional)*: Video count.
- **organizationMembersCount** *(number, optional)*: Organization member count.
- **commentsCount** *(number, optional)*: Comment count on the profile.
- **socialLinks** *(array[object], optional)*: Social links listed on the profile.
- **socialLinks[].type** *(string, optional)*: Social link type.
- **socialLinks[].url** *(string, optional)*: Public social URL.
- **licenseNumber** *(string, optional)*: License number shown on the profile.
- **license.licenseNum** *(string, optional)*: Verified license number.
- **license.licenseType** *(string, optional)*: License type.
- **license.issueBy** *(string, optional)*: Issuing authority.
- **hasVerifiedLicense** *(boolean, optional)*: Whether a license verification badge is present.
- **isProVerified** *(boolean, optional)*: Whether the profile is verified.
- **isProVerificationEnabled** *(boolean, optional)*: Whether verification features are enabled.
- **hasVerifiedKyc** *(boolean, optional)*: Whether KYC verification is indicated.
- **isInactivePro** *(boolean, optional)*: Whether the profile is inactive.
- **isSponsoredResult** *(boolean, optional)*: Whether the result is sponsored.
- **localProjectCount** *(number, optional)*: Count of local projects shown in directory results.
- **offersGreenSkyFinancing** *(boolean, optional)*: Financing flag.
- **offersFigureFinancing** *(boolean, optional)*: Financing flag.
- **isVideoConsultationEnabled** *(boolean, optional)*: Video consultation availability flag.
- **profileImage.externalId** *(string, optional)*: Profile image identifier.
- **coverImage.externalId** *(string, optional)*: Cover image identifier.
- **companyHighlightVideo.id** *(number, optional)*: Highlight video identifier.
- **reviewAspectAggregations[].name** *(string, optional)*: Review aspect label.
- **reviewAspectAggregations[].average** *(number, optional)*: Average review aspect score.
- **breadcrumbs** *(array[string], optional)*: Profile category breadcrumb trail.

### Photo fields (`type = "photo"`)

- **title** *(string, optional)*: Photo title.
- **description** *(string, optional)*: Best available description for the photo.
- **comments** *(string, optional)*: Caption or comment text.
- **autoDescription** *(string, optional)*: Auto-generated photo description.
- **location** *(string, optional)*: Metro area display name.
- **metroAreaId** *(number, optional)*: Metro area identifier.
- **categoryId** *(number, optional)*: Houzz category identifier.
- **projectId** *(number, optional)*: Related project identifier.
- **created** *(string, optional)*: Creation timestamp.
- **modified** *(string, optional)*: Last modified timestamp.
- **status** *(number, optional)*: Houzz status code.
- **style** *(number, optional)*: Style code.
- **link** *(string, optional)*: Associated external link when present.
- **trackLink** *(string, optional)*: Tracked outbound link.
- **ownerId** *(number, optional)*: Owner identifier.
- **authorId** *(number, optional)*: Author identifier.
- **ownerName** *(string, optional)*: Owner display name.
- **ownerProfileUrl** *(string, optional)*: Owner Houzz profile URL.
- **recentBuzz** *(string, optional)*: Recent buzz text.
- **recentBuzzUserId** *(number, optional)*: User ID linked to recent buzz.
- **recentBuzzCreated** *(string, optional)*: Recent buzz timestamp.
- **buzzCount** *(number, optional)*: Save or buzz count.
- **buzzCountString** *(string, optional)*: Display version of buzz count.
- **questionCount** *(number, optional)*: Number of related questions.
- **questionCountString** *(string, optional)*: Display version of question count.
- **keywordString** *(string, optional)*: Keyword list associated with the photo.
- **imageIds** *(array[string], optional)*: Image identifiers.
- **altText** *(string, optional)*: Image alt text.
- **isProduct** *(boolean, optional)*: Product flag.
- **isFeatured** *(boolean, optional)*: Featured flag.
- **isFile** *(boolean, optional)*: File flag.
- **isBookmarkletUpload** *(boolean, optional)*: Bookmarklet upload flag.
- **canChangeSpaceAuthor** *(boolean, optional)*: Author edit flag.
- **showBeforeTag** *(boolean, optional)*: Before-photo flag.
- **sourceType** *(number, optional)*: Source type code.
- **permissionMask** *(number, optional)*: Permission mask value.
- **showProPerception** *(boolean, optional)*: Pro perception flag.
- **isBrandAdvertiserPhoto** *(boolean, optional)*: Brand advertiser flag.
- **isMoodboard** *(boolean, optional)*: Moodboard flag.

### Story fields (`type = "story"`)

- **title** *(string, optional)*: Story title.
- **subtitle** *(string, optional)*: Story subtitle or deck.
- **commentCount** *(number, optional)*: Comment count.
- **topics** *(array[string] or array[object], optional)*: Topics associated with the story.
- **coverPhoto.id** *(number, optional)*: Cover photo identifier.
- **user.displayName** *(string, optional)*: Story author display name.
- **user.userName** *(string, optional)*: Story author username.

### Discussion fields (`type = "discussion"`)

- **userId** *(number, optional)*: Discussion author user ID.
- **ownerUserId** *(number, optional)*: Owner user ID.
- **ownerName** *(string, optional)*: Author display name.
- **ownerProfileUrl** *(string, optional)*: Author profile URL.
- **title** *(string, optional)*: Discussion title.
- **body** *(string, optional)*: Plain-text discussion body.
- **htmlBody** *(string, optional)*: HTML-formatted discussion body.
- **typeId** *(number, optional)*: Houzz discussion type identifier.
- **topicId** *(number, optional)*: Topic identifier.
- **topicTitle** *(string, optional)*: Topic title.
- **numberOfAnswers** *(number, optional)*: Answer count.
- **numberOfLikes** *(number, optional)*: Like count.
- **lastAnsweredDate** *(string, optional)*: Last answer timestamp.
- **lastAnsweredByUserId** *(number, optional)*: User ID of the last responder.
- **lastAnsweredByUserIds** *(array[number], optional)*: User IDs of recent responders.
- **allowToVote** *(boolean, optional)*: Voting availability flag.
- **totalVotes** *(number, optional)*: Vote count.
- **tagIds** *(array[number], optional)*: Tag identifiers.
- **attachments** *(array[object], optional)*: Attached assets.
- **status** *(number, optional)*: Houzz status code.
- **thumbImageId** *(string or number, optional)*: Thumbnail image identifier.
- **fallbackThumbImage** *(string, optional)*: Fallback thumbnail URL or identifier.
- **spaceId** *(number, optional)*: Related space identifier.
- **galleryId** *(number, optional)*: Related gallery identifier.
- **urlSlug** *(string, optional)*: URL slug.
- **areaId** *(number, optional)*: Area identifier.
- **privacy** *(number, optional)*: Privacy setting code.
- **isolated** *(boolean, optional)*: Isolation flag.
- **created** *(string, optional)*: Creation timestamp.
- **modified** *(string, optional)*: Last modified timestamp.
- **fullUrl** *(string, optional)*: Full discussion URL when provided separately.
- **commentsUrl** *(string, optional)*: Comments URL.
- **bestAnswerId** *(number, optional)*: Best answer identifier.
- **bestAnswerSource** *(string, optional)*: Best answer source indicator.

## Data guarantees & handling

- **Best-effort extraction:** fields may vary by region, session, availability, or UI experiments on Houzz.
- **Optional fields:** null-check in downstream code because some attributes are not present on every result.
- **Deduplication:** recommend `type + ":" + id`.

## How to Run on Apify

1. Open the Actor in Apify Console.
2. Configure your search parameters by adding `startUrls`, `queries`, a `queryType`, and any optional filters you want to apply.
3. Set the maximum number of outputs to collect with `limit`.
4. Click **Start** and wait for the run to finish.
5. Download results in JSON, CSV, Excel, or another supported format.

## Scheduling & Automation

### Scheduling

**Automated Data Collection**

You can schedule runs to keep your Houzz dataset fresh without manual reruns. This is useful for recurring reporting, lead monitoring, and trend tracking.

- Navigate to **Schedules** in Apify Console
- Create a new schedule (daily, weekly, or custom cron)
- Configure input parameters
- Enable notifications for run completion
- (Optional) Add webhooks for automated processing

### Integration Options

- **Webhooks:** Trigger downstream actions when a run completes
- **Zapier:** Connect to 5,000+ apps without coding
- **Make (Integromat):** Build multi-step automation workflows
- **Google Sheets:** Export results to a spreadsheet
- **Slack/Discord:** Receive notifications and summaries
- **Email:** Send automated reports via email

## Performance

Estimated run times:

- **Small runs (< 1,000 outputs):** ~2–3 minutes
- **Medium runs (1,000–5,000 outputs):** ~5–15 minutes
- **Large runs (5,000+ outputs):** ~15–30 minutes

Execution time varies based on filters, result volume, and how much information is returned per record.

## Compliance & Ethics

### Responsible Data Collection

This actor collects publicly available design inspiration, professional directory, editorial, and community discussion information from [https://houzz.com](https://www.houzz.com) for legitimate business purposes. Common use cases include:

- **Home improvement and interior design** research and market analysis
- **Lead generation and vendor discovery**
- **Content planning and competitive monitoring**

Users are responsible for ensuring their collection and use of data complies with applicable laws, regulations, and site terms. This section is informational and not legal advice.

### Best Practices

- Use collected data in accordance with applicable laws, regulations, and the target site’s terms
- Respect individual privacy and personal information
- Use data responsibly and avoid disruptive or excessive collection
- Do not use this actor for spamming, harassment, or other harmful purposes
- Follow relevant data protection requirements where applicable (e.g., GDPR, CCPA)

## Support

For help, use the Issues tab or the actor page on Apify. Include the input you used (with sensitive values redacted), the run ID, a short description of expected versus actual behavior, and optionally a small output sample that shows the problem.