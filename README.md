# Steadyfetch agent tools

One MCP server, 30 Steadyfetch actors on Apify, pinned into a single tool list an agent can use
without browsing a store. Ad, video, reel and audio transcripts; Google Trends and keyword demand;
job boards; Amazon product data; Instagram.

Everything here is pay-per-event on Apify: **you are charged per delivered item, never on a miss.**
A link that cannot be reached, a video with no speech, a keyword Google has no data for, a removed
ad — all come back as rows marked uncharged. Actor listings, input fields and current prices live at
[apify.com/steadyfetch](https://apify.com/steadyfetch).

## Install

See [SETUP.md](SETUP.md). The only setup step is an Apify API token in `APIFY_TOKEN`.

- **Claude Code** — install this repo as a plugin (`.claude-plugin/plugin.json` + `.mcp.json` + two skills).
- **Gemini CLI** — `gemini extensions install https://github.com/steadyfetch/steadyfetch-agent-tools`
- **Any skills-aware client** — `npx skills add steadyfetch/steadyfetch-agent-tools`
- **Any MCP client** — paste the pinned URL below and send your token in the `Authorization` header.

## The pinned URL

```
https://mcp.apify.com/?tools=fetch-actor-details,steadyfetch/google-trends-scraper,steadyfetch/facebook-ads-transcript-scraper,steadyfetch/instagram-reel-transcript-scraper,steadyfetch/media-transcriber,steadyfetch/keyword-search-volume-scraper,steadyfetch/instagram-profile-posts,steadyfetch/youtube-transcript-scraper,steadyfetch/multi-job-board-scraper,steadyfetch/youtube-channel-transcripts,steadyfetch/amazon-search-scraper,steadyfetch/google-keyword-suggest-scraper,steadyfetch/amazon-bestsellers-scraper,steadyfetch/google-trends-now-scraper,steadyfetch/breakout-keywords-scraper,steadyfetch/amazon-seller-scraper,steadyfetch/indeed-jobs-scraper,steadyfetch/amazon-product-scraper,steadyfetch/social-trends-scraper,steadyfetch/google-ads-video-transcript-scraper,steadyfetch/glassdoor-jobs-scraper,steadyfetch/company-jobs-by-domain,steadyfetch/google-ads-creative-text-scraper,steadyfetch/tiktok-ads-transcript-scraper,steadyfetch/google-jobs-scraper,steadyfetch/linkedin-ads-transcript-scraper,steadyfetch/instagram-hashtag-scraper,steadyfetch/instagram-post-details-scraper,steadyfetch/instagram-followers-scraper,steadyfetch/linkedin-jobs-scraper,steadyfetch/instagram-comments-scraper
```

Streamable HTTP. Send `Authorization: Bearer <your Apify API token>`.

## Pin a subset instead

The `tools` query parameter is a plain comma-separated list, so a smaller server is a shorter URL.
Keep `fetch-actor-details` first — it is what lets an agent read an actor's input schema before
calling it — then name only the actors you want:

```
https://mcp.apify.com/?tools=fetch-actor-details,steadyfetch/youtube-transcript-scraper,steadyfetch/media-transcriber
```

## What each actor returns

### Ad creative to text

| Actor | What it returns |
|---|---|
| `steadyfetch/facebook-ads-transcript-scraper` | Meta (Facebook and Instagram) Ad Library ads as text: video transcripts with the first-3-seconds hook, plus the words inside image ads. Search by keyword or advertiser, no ad IDs needed. |
| `steadyfetch/tiktok-ads-transcript-scraper` | TikTok Creative Center top ads as transcripts, with the hook, CTR and brand metadata, by region and industry. |
| `steadyfetch/linkedin-ads-transcript-scraper` | LinkedIn Ad Library ads as text: video transcripts and on-image copy, with CTA and advertiser data per ad. |
| `steadyfetch/google-ads-video-transcript-scraper` | Google Ads Transparency Center video ads as transcripts with the hook and advertiser metadata. |
| `steadyfetch/google-ads-creative-text-scraper` | Google Ads Transparency Center text and image creatives: headline, body, CTA, and all visible text read off image ads. |

### Video and audio to text

| Actor | What it returns |
|---|---|
| `steadyfetch/youtube-transcript-scraper` | YouTube videos, Shorts and live VODs as text — captions first, built-in speech-to-text when a video has none. JSON, plain text, SRT or VTT. |
| `steadyfetch/youtube-channel-transcripts` | A whole YouTube channel in one run: every video, Short and live VOD, de-duplicated and transcribed, newest first. |
| `steadyfetch/instagram-reel-transcript-scraper` | A creator's Instagram reels as text, with the first-3-seconds hook, language and timestamps. A handle, pasted reel links, or a whole scraper run. |
| `steadyfetch/media-transcriber` | Any direct audio or video file link, plus page links on 13 tested hosts, to text, SRT and VTT with timestamped segments, in about 90 languages. |

### Search demand and trends

| Actor | What it returns |
|---|---|
| `steadyfetch/google-trends-scraper` | Google Trends as stable JSON: interest over time, related queries, related topics, region breakdown and compare — one row per term and surface. |
| `steadyfetch/google-trends-now-scraper` | Google's trending searches for any country: rank, the traffic floor Google publishes, when each trend started, and the news stories behind it. |
| `steadyfetch/breakout-keywords-scraper` | Breakout and rising Google Trends queries with the real growth percentage behind the "Breakout" label, plus current interest and the 12-month curve. |
| `steadyfetch/keyword-search-volume-scraper` | Google Ads monthly search volume, competition and top-of-page bid range for a keyword list — real Keyword Planner figures, nothing modelled. |
| `steadyfetch/google-keyword-suggest-scraper` | Autocomplete suggestions for one seed from five engines at once: Google, YouTube, Amazon, Bing and the App Store. |
| `steadyfetch/social-trends-scraper` | What is trending right now on X, TikTok, Pinterest, YouTube Charts and Google — five platforms in one run, one row shape with rank, metric and link. |

### Jobs

| Actor | What it returns |
|---|---|
| `steadyfetch/multi-job-board-scraper` | One keyword across Indeed and company hiring boards, merged and de-duplicated into a single feed — the same role on two boards is one row. |
| `steadyfetch/indeed-jobs-scraper` | Indeed jobs by keyword or search URL: title, company, location, salary, posted date and apply link, with exact-country results. |
| `steadyfetch/google-jobs-scraper` | The full Google Jobs panel for a search rather than the first ten cards: source board, posted age, salary when shown, apply links and the full description. |
| `steadyfetch/glassdoor-jobs-scraper` | Glassdoor listings with the employer's star rating on the same row, so a second run is not needed to get it. |
| `steadyfetch/linkedin-jobs-scraper` | LinkedIn jobs by keyword and location without a login: title, company, location, posted date and the public link; description, salary and seniority on request. |
| `steadyfetch/company-jobs-by-domain` | Paste a company domain and get every live opening from its Greenhouse, Lever, Ashby, Workday or other ATS board — nine platforms, one input. |

### Amazon

| Actor | What it returns |
|---|---|
| `steadyfetch/amazon-product-scraper` | Any product by ASIN or URL as one finished row: price, buy box, stock, rating, reviews, variants, bestseller ranks, images and specs. |
| `steadyfetch/amazon-search-scraper` | A keyword search with that same full product row for every result, not just a title and a price. |
| `steadyfetch/amazon-bestsellers-scraper` | Every rank in a Best Sellers category, or from a pasted Best Sellers link, as a full product row. |
| `steadyfetch/amazon-seller-scraper` | Any seller by ID, storefront or URL: name, feedback from 30 days to lifetime, registered business details, and a row per live listing. |

### Instagram

| Actor | What it returns |
|---|---|
| `steadyfetch/instagram-profile-posts` | Every post and reel from a public profile: captions, hashtags, engagement, media links and tagged accounts, by handle or URL, no login. |
| `steadyfetch/instagram-hashtag-scraper` | Posts and reels under a hashtag, or found by keyword search: captions, hashtags, mentions, engagement, media links, owner and location. |
| `steadyfetch/instagram-post-details-scraper` | Everything one post or reel exposes from its link: plays, saves, reshares, likes, comments, three video renditions, the audio track, carousel slides and the caption. |
| `steadyfetch/instagram-followers-scraper` | The followers and the following list of a public account: handle, name, user ID, verified and private flags. |
| `steadyfetch/instagram-comments-scraper` | Comments and their replies for any post, reel or IGTV link: text, author, likes, reply counts and timestamps. |

## Skills

Two agent skills ship in `skills/`, and they are what an agent should read before calling the
transcript actors — the input field names differ per platform, and the output field for the text
itself is not the same on every actor.

- `skills/apify-video-transcripts/` — YouTube videos and channels, Instagram reels, and any other
  audio or video URL.
- `skills/apify-ad-creative-transcripts/` — Meta, TikTok, LinkedIn and Google ad creative.

Both carry live prices read on 2026-09-11 and the command to re-read them, because prices change.

## License

MIT. See [LICENSE](LICENSE).
