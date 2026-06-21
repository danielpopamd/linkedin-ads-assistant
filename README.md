# LinkedIn Ads Assistant

Connect your AI assistant to **LinkedIn Ads** through the AdPlug Marketing Control Plane.
This is a thin bundle: it points your agent at AdPlug's hosted, multi-tenant MCP server
and ships a usage skill. All 10 LinkedIn Ads tools run server-side at AdPlug.

## What you get (10 tools)

- **Reporting**: one `linkedin_ads_performance` tool covering campaign, creative, daily,
  conversion, lead-gen, demographics, and reach report types.
- **Entities**: `linkedin_ads_list` and `linkedin_ads_get` for campaign groups,
  campaigns, creatives, conversions, lead forms, and audiences.
- **Targeting**: `linkedin_ads_targeting_lookup` for search, URN resolution, and
  audience-count estimates.
- **Competitor research**: `linkedin_ad_library_search` over LinkedIn's public Ad Library.
- **Safe edits**: preview-then-execute for all writes, plus the Conversions API.
- **Status**: `adplug_get_status` for connection state and next steps.

## Install (OpenClaw)

```bash
openclaw plugins install clawhub:linkedin-ads-assistant
```

## Getting started (free, one-time)

To read or optimise your LinkedIn Ads data you need a free AdPlug account. AdPlug is the
secure bridge between your AI assistant and the LinkedIn Ads API.

1. **Install** this plugin (above). It adds the LinkedIn Ads Assistant MCP server.
2. **Create your free AdPlug account.** The first time you use the plugin, your client
   opens an AdPlug sign-in page. Sign up, or sign in if you already have an account. That
   signup authenticates the connection over OAuth, with no API keys or tokens to paste.
3. **Connect LinkedIn Ads (free).** In the AdPlug dashboard at
   https://adplug.app/dashboard/connections, click "Connect LinkedIn Ads" and authorize.

After that, just ask your assistant about your LinkedIn Ads and the tools work. Until an
account is connected they return a clear "connect first" message. Each user authenticates
individually and sees only their own accounts.

## Endpoint

```
https://api.adplug.app/mcp/linkedin-ads   (Streamable HTTP, OAuth 2.1)
```

The same endpoint also accepts a personal URL token if you prefer not to use OAuth:
`https://api.adplug.app/mcp/linkedin-ads/<your-mcp-token>` (find the token on your AdPlug
dashboard). OAuth is recommended.

## Naming note

LinkedIn's UI "Campaigns" are the API's `campaign_groups`, UI "Ad sets" are `campaigns`,
and UI "Ads" are `creatives`. The bundled skill explains how to map them.

## Pricing

AdPlug has a free tier for reads and paid tiers for edits and higher volume. See
https://adplug.app for current plans and terms.

## Links

- Product: https://adplug.app
- LinkedIn Ads integration: https://adplug.app/integrations/linkedin-ads
- Companion plugin: `google-ads-assistant`
- Both platforms in one connection: https://api.adplug.app/mcp
