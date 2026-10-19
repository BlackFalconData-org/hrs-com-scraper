# HRS Hotel Reviews Scraper

Extract structured data from [hrs.com](https://hrs.com) — hotel guest reviews from hrs.com by destination or hotel. Every review carries its score and traveller type with positive and negative comments, plus the hotel's replies. Hotel rows add address, coordinates and rating averages by traveller type.

**[HRS Hotel Reviews Scraper on Apify →](https://apify.com/blackfalcondata/hrs-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from hrs.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from hrs.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "699596",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | An hrs.com destination/list/hotel URL, a location id, or comma-separated hotel ids (e.g. 699596 = Titanic Chaussee Berlin). Required if no startUrls provided. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste www.hrs.com destination, list or hotel URLs. Each URL becomes its own search task; results are merged and deduped by hotel. When provided, startUrls REPLACE the query field (unsupported URLs are skipped; if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch full listing details. |
| `descriptionMaxLength` | integer | `0` | Truncate the combined reviewText to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `lat` | number | — | GPS latitude of the centre point. Filters the hotels of the searched destination by distance; needs lon and radiusKm. |
| `lon` | number | — | GPS longitude of the centre point. Filters the hotels of the searched destination by distance; needs lat and radiusKm. |
| `radiusKm` | integer | — | Radius in kilometres around lat/lon. Only hotels of the searched destination within this distance are returned, each with its distanceKm. |

---

## Output fields

Every record returns the same 80-field schema. Missing values are `null` — never omitted.

- `hotel_id`
- `hotel_name`
- `hotel_status`
- `stars`
- `city`
- `district`
- `country`
- `country_code`
- `street`
- `postal_code`
- `latitude`
- `longitude`
- `source_url`
- `booking_id`
- `locale`
- `language`
- `traveler_type`
- `traveler_age_group`
- `arrival_date`
- `departure_date`
- `review_date`
- `average_rating`
- `recommended`
- `comfort`
- `positive_comment`
- `hotel_positive_response`
- `rating_votes`
- `filter`
- `filtered_average_rating`
- `filtered_ratings_count`
- `filtered_recommendation_rate`
- `category_averages`
- `super_category_averages`
- `reviewer_name`
- `negative_comment`
- `hotel_negative_response`
- `recordType`
- `reviewId`
- `reviewUid`
- `bookingId`
- `reviewerName`
- `travelerType`
- `reviewerAgeGroup`
- `arrivalDate`
- `departureDate`
- `reviewPostedDate`
- `reviewPostedAt`
- `ratingScore`
- `reviewText`
- `positiveComment`
- `negativeComment`
- `hotelPositiveResponse`
- `hotelNegativeResponse`
- `ratingVotes`
- `hotelId`
- `hotelName`
- `hotelUrl`
- `portalUrl`
- `distanceKm`
- `postalCode`
- `countryCode`
- `chainName`
- `brandName`
- `hotelStatus`
- `hotelRating`
- `hotelReviewCount`
- `hotelRecommendationRate`
- `ratingsByTravellerType`
- `reviewCountsByTravellerType`
- `categoryAverages`
- `pricePerNight`
- `currency`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "hotel_id": 699596,
  "hotel_name": "Titanic Chaussee Berlin",
  "hotel_status": "OPEN",
  "stars": null,
  "city": "Berlin",
  "district": "Berlin",
  "country": "DEU",
  "country_code": "DE",
  "street": "Chausseestraße 30",
  "postal_code": "10115",
  "latitude": 52.53232,
  "longitude": 13.38117
}
```

*Truncated — full records contain 80 fields. See Output fields for the complete schema.*

**[Try HRS Hotel Reviews Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/hrs-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00063 |
| Result | $0.00126 |

See the [actor on Apify](https://apify.com/blackfalcondata/hrs-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape hrs.com?**
Use this actor on Apify to extract structured data from hrs.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get hrs.com data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape hrs.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check hrs.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [HRS Hotel Reviews Scraper](https://apify.com/blackfalcondata/hrs-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
