# Shopify App Store and Chrome Web Store Listings MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-shopify-app-store-chrome-web-store-listings)](https://smithery.ai/servers/mambabuilt/mcp-shopify-app-store-chrome-web-store-listings) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-shopify-app-store-chrome-web-store-listings%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-shopify-app-store-chrome-web-store-listings&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings)](https://www.npmjs.com/package/@mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings)](https://www.npmjs.com/package/@mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings)](https://github.com/mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings)

An MCP server for the Mamba Labs Shopify App Store and Chrome Web Store Listings actor on Apify. Give it categories, keywords, or listing URLs. It returns one row per app with the app's numbers and its developer's own website and domain.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

- **Shopify App Store**: apps per category (24 per page, paged), or by keyword matched against every app name in the store's app list. Rating, reviews, pricing line, Built for Shopify flag, launch date, developer page, website, address.
- **Chrome Web Store**: extensions per category (about 32 per category page), or by keyword across the categories. Rating, exact rating count, users, version, last update, developer website, address, email, and phone.
- **Developer domain**: the registrable domain of the developer website on every row, ready to join to a company list.

Apps below your rating or review filter are skipped and never billed. Input with nothing to run comes back as one free row whose `error_reason` starts with `input_invalid:`.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-shopify-app-store-chrome-web-store-listings": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-shopify-app-store-chrome-web-store-listings"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `list_store_apps` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "List the top 20 email marketing apps in the Shopify App Store with their developer websites."
- "Which productivity extensions in the Chrome Web Store have over 100,000 users, and how do I contact their developers?"
- "Find Shopify apps with 'reviews' in the name rated 4.5 or higher."

## Tool

`list_store_apps`. One tool. Example arguments:

```json
{
  "categories": [
    "marketing-and-conversion-marketing-email-marketing",
    "productivity/workflow"
  ],
  "max_apps": "10"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `categories` | array | One per line. Shopify App Store: a category URL or slug from apps.shopify.com/categories, for example marketing-and-conversion-marketing-email-marketing. Chrome Web Store: a category URL or one of productivity/communication, productivity/developer, productivity/education, productivity/tools, productivity/workflow, lifestyle/art, lifestyle/entertainment, lifestyle/fun, lifestyle/games, lifestyle/household, lifestyle/news, lifestyle/shopping, lifestyle/social, lifestyle/travel, lifestyle/well_being, make_chrome_yours/accessibility, make_chrome_yours/functionality, make_chrome_yours/privacy |
| `keywords` | array | Optional. Shopify: matched against every app name in the Shopify App Store app list. Chrome: matched against the name and summary of the extensions on the categories you gave, or on all 18 categories when you gave none. Every word of the keyword must match |
| `app_urls` | array | Optional. apps.shopify.com or chromewebstore.google.com listing URLs to read directly. Always read, outside the max apps cap |
| `stores` | array | Which stores a keyword is searched in: shopify_app_store, chrome_web_store, or both. Default both. Categories and listing URLs name their own store |
| `max_apps` | string | Most apps per category and per keyword. Default 50, up to 500. A Chrome category page lists about 32 extensions |
| `include_details` | string | Default true. Keyword and URL rows always read the listing page |
| `min_rating` | string | Optional. Skip apps rated below this, for example 4 |
| `min_reviews` | string | Optional. Skip apps with fewer reviews or ratings than this |
| `max_charge_usd` | string | Optional. The run stops before it would charge more than this |
| `proxy_mode` | string | A reliability choice. Default auto |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `app-listing`, one Shopify app or Chrome extension returned, with its listing page read unless include_details is false | $0.005 | $0.00475 | $0.0045 | $0.00425 |
| `apify-actor-start`, per run start | $0.0006313 | $0.0006313 | $0.0006313 | $0.0006313 |

Rows for a category, sitemap, or listing that could not be read are free, and so is the no_results row.

## Your keys

The actor needs no key. It reads the public category and listing pages of both stores and holds no key of any kind. Proxy traffic adds nothing to your price: Mamba Labs pays the platform costs of the run. `proxy_mode` is a reliability setting.

## Full actor documentation

[apify.com/mambalabs/shopify-app-store-chrome-web-store-listings](https://apify.com/mambalabs/shopify-app-store-chrome-web-store-listings)

## License

MIT. See [LICENSE](LICENSE).
