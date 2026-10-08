# How to Scrape KissKissBankBank: French Crowdfunding Projects (Funding, Backers, Category)

> **Run it now, no code: [kisskissbankbank-projects-scraper on the Apify Store](https://apify.com/bibian/kisskissbankbank-projects-scraper)** &nbsp;|&nbsp; $2.00 per 1,000 projects &nbsp;|&nbsp; no browser, no proxy

Scrape KissKissBankBank, France's crowdfunding platform: projects with category, amount raised, goal, backers, funding percent and end date. KissKissBankBank has no public export and no official API for third parties. This guide shows how to get its project campaigns as clean structured data, with a free Apify Actor, from the console or from code.

Searches this page answers: kisskissbankbank scraper, french crowdfunding data, crowdfunding project scraper, kisskissbankbank api.

## What you get from the KissKissBankBank scraper

- Every project campaign with its title, short description, category and city.
- Funding numbers: amount raised, funding goal (money or backer count), funding percent and number of backers.
- Campaign dates: creation date, end date and duration in days.
- The project owner's name, but only when KissKissBankBank itself tags the campaign as run by a named company or association.

Export the dataset as JSON, CSV or Excel, or read it through the Apify API. A real sample (five results) is in [`sample-output.json`](sample-output.json).

## Output fields

| Field | What it is |
|---|---|
| `id`, `slug`, `url` | KissKissBankBank project id, slug and public link |
| `title`, `shortDesc` | Campaign title and short pitch |
| `categories`, `city`, `country` | Project category, city and country |
| `state`, `isPermanent` | Campaign state (for example `successful`) and whether it runs with no end date |
| `collectedAmount`, `currency` | Amount raised, in the campaign's own currency's major unit |
| `goalType`, `goalAmount`, `goalCount` | `amount` goals set a money target (`goalAmount`); `count` goals set a backer target (`goalCount`) instead |
| `fundingPercent`, `collectedCount` | Percent of the goal reached (can exceed 100) and number of backers |
| `createdAt`, `endAt`, `durationDays` | Campaign creation date, end date and total duration |
| `projectOwner` | Company or association name, only when KissKissBankBank tags the campaign that way; empty for individual creators |

## Use cases

- **Research the French crowdfunding market**: track live and finished campaigns by category, city and funding outcome.
- **Find successful project creators to pitch**: companies and associations running a well-funded campaign are a warm lead for packaging, logistics, PR or follow-up funding offers.
- **Build a crowdfunding dashboard or newsletter**: export current campaigns with their funding percent and days left.
- **Benchmark campaign performance**: compare goal vs. amount raised and backer counts across categories.

## Input

| Field | What it does |
|---|---|
| `maxItems` | The run stops once this many projects are saved (default 50). |

Example input:

```json
{
  "maxItems": 50
}
```

## Three ways to use it

### 1. Apify console, no code

1. Open [the Actor page](https://apify.com/bibian/kisskissbankbank-projects-scraper) and click **Try for free** (a free Apify account is enough).
2. Set the maximum number of projects and click **Start**.
3. When the run finishes, open the **Output** tab, check the table, then **Export** as CSV, Excel or JSON.
4. For a daily watch, open **Schedules**, create a daily schedule for the Actor and, optionally, add an integration that sends new results to Slack, e-mail or Google Sheets.

### 2. Apify API with JavaScript

```bash
npm install apify-client
export APIFY_TOKEN="your token from Apify console, Settings, Integrations"
node examples/run.js
```

```js
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });

const run = await client.actor('bibian/kisskissbankbank-projects-scraper').call({
    "maxItems": 50
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
for (const item of items) {
    console.log(`${item.title} | ${item.city} | ${item.collectedAmount} ${item.currency} (${item.fundingPercent}%)`);
}
```

The complete file is [`examples/run.js`](examples/run.js) (ES module, Node 18 or later).

### 3. Apify API with Python

```bash
pip install apify-client
export APIFY_TOKEN="your token from Apify console, Settings, Integrations"
python examples/run.py
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])

run = client.actor("bibian/kisskissbankbank-projects-scraper").call(run_input={
    "maxItems": 50,
})

for item in client.dataset(run.default_dataset_id).iterate_items():
    print(f"{item['title']} | {item.get('city')} | {item.get('collectedAmount')} {item.get('currency')} ({item.get('fundingPercent')}%)")
```

The complete file is [`examples/run.py`](examples/run.py). Never put your token in the code: read it from the `APIFY_TOKEN` environment variable as above.

## Pricing

Pay per result: **$2.00 per 1,000 projects** (that is $0.002 per project), plus a tiny start fee of $0.00005 per run. You pay only for the results saved to the dataset. Apify's free plan includes monthly credits, enough to try it on a few hundred results. Prices are those shown on the [Store page](https://apify.com/bibian/kisskissbankbank-projects-scraper) when this guide was written; the Store page is the reference.

## Limits and honest notes

- Projects are listed from KissKissBankBank's own sitemap, in the order the site lists them, not sorted by funding or popularity: there is no public search API to filter by.
- If the sitemap cannot be read, the run automatically falls back to the projects featured on the homepage and the default discover page, so a run still succeeds and still saves real projects; it only returns fewer of them.
- `projectOwner` is only ever filled when KissKissBankBank itself tags the campaign as run by a named company or association. For campaigns run by an individual, `projectOwner` is left empty: no personal name, username, phone or email is ever collected.
- Some campaigns set a backer-count goal instead of a money goal (`goalType: "count"`): `goalAmount` is then empty and `goalCount` holds the target number of backers instead.
- `endAt` is empty for "permanent" (subscription-style, `isPermanent: true`) campaigns, which run with no end date.
- A handful of older or subscription-style campaigns have moved to KissKissBankBank's sister site Ulule; those are skipped rather than followed, since they are a different site.
- This is an unofficial tool, not affiliated with KissKissBankBank. Check the site terms and the data protection rules that apply to your use.

## FAQ

**Does it need a browser or a proxy?**
No. The Actor reads the data directly, so runs are fast and cheap.

**Can I filter by category or city?**
Not yet: the input has a single field (the maximum number of projects). Filter the exported data afterward, for example in a spreadsheet.

**Can I get the data in Excel or Google Sheets?**
Yes. Export the dataset as CSV, Excel or JSON from the Apify console, or connect the Apify Google Sheets integration.

**Does it collect backer personal data?**
No. The dataset contains public campaign information only. No backer or individual creator personal data is collected.

## Try it

**[Open kisskissbankbank-projects-scraper on the Apify Store](https://apify.com/bibian/kisskissbankbank-projects-scraper)**

The code in `examples/` is released under the MIT license (see [LICENSE](LICENSE)). The data belongs to its original publishers.
