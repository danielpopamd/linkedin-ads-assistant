---
name: linkedin-ads-assistant
description: Manage LinkedIn Ads through AdPlug. Use whenever the user asks about LinkedIn Ads, sponsored content, sponsored messaging, lead gen forms, campaign groups, creatives, audience targeting, conversions, ad spend, CPC, CTR, impressions, clicks, leads, demographic breakdowns, or competitor ads. Covers reporting, targeting lookups, the public Ad Library, and safe preview-then-execute edits.
metadata:
  homepage: https://adplug.app/integrations/linkedin-ads
  provider: AdPlug
  transport: remote-mcp-oauth
---

# LinkedIn Ads Assistant

This skill ships with a remote MCP server (`linkedin-ads-assistant`) that connects your
AI assistant to LinkedIn Ads through AdPlug. The tools appear automatically once the
server is installed and you have authenticated.

## Connecting (first run, free)

Using LinkedIn Ads data requires a free AdPlug account. AdPlug is the secure bridge
between the assistant and the LinkedIn Ads API. There are no tokens to paste.

1. **Create a free AdPlug account.** On the first LinkedIn Ads request, the client opens
   an AdPlug sign-in page. The user signs up, or signs in if they already have an account.
   That signup authenticates the connection over OAuth.
2. Call `adplug_get_status` once. If LinkedIn Ads shows as disconnected, follow the
   `action_required` steps it returns.
3. **Connect LinkedIn Ads (free)** at https://adplug.app/dashboard/connections (click
   "Connect LinkedIn Ads"). After that, every tool works automatically.

Call `adplug_get_status` only once per session. Do not retry platform tools while a
platform is disconnected, they will fail until the account is linked.

## Naming note (important)

LinkedIn's UI names differ from the API names this server uses:

- UI "Campaigns" map to API `campaign_groups` (use `entity_type="campaign_groups"`).
- UI "Ad sets" map to API `campaigns` (use `entity_type="campaigns"`).
- UI "Ads" map to API `creatives` (use `entity_type="creatives"`).

When a user says "campaigns" they usually mean campaign groups.

## Recommended workflow

1. `linkedin_ads_list_accounts` to discover available accounts.
2. `linkedin_ads_performance` for all analytics. Set `report_type` to one of: campaign,
   creative, daily, conversion, lead_gen, demographics, or reach.
3. `linkedin_ads_list` to list any entity. Set `entity_type` to one of: campaigns,
   creatives, campaign_groups, conversions, lead_forms, saved_audiences,
   targeting_audiences, or targeting_facets.
4. `linkedin_ads_get` for full detail on a single entity. Set `entity_type` to one of:
   campaign, campaign_group, creative, targeting_audience, saved_audience, or
   lead_form_responses.
5. `linkedin_ads_targeting_lookup` for targeting work. Use action `search` to find job
   titles, companies, or skills, `resolve_urns` to resolve ids, or `audience_count` for
   reach estimates.
6. `linkedin_ad_library_search` for competitor and advertiser research (not your own
   accounts). Pass a keyword and/or advertiser. Use `mode="summary"` to detect activity
   ("is X advertising, how many ads"), or `mode="standard"` for full results. Ad copy,
   CTA text, and media URLs are not in the API response, so direct the user to the
   returned `ad_url` for the creative.

## Making changes safely

Edits always run as a two-step preview then execute, so you can confirm the change
before it touches the account.

- All writes: `linkedin_ads_preview_mutation`, then `linkedin_ads_execute_mutation`.
  Supports create, update, and delete for campaign groups, campaigns, creatives,
  targeting audiences, conversion rules, saved audiences, and more.
- Conversions API: `linkedin_ads_send_conversion_event` to send conversion events in bulk.

## Targeting URN gotchas

- Matched audiences (DMP segments): list with `linkedin_ads_list entity_type=saved_audiences`.
  Pagination is complete (no 10-row cap). Use `name_contains` for substring search,
  LinkedIn's typeahead does not support matched-audience facets.
- The targetable URN is the `destinationSegmentUrn` from the listing (or
  `urn:li:adSegment:<destinationSegmentId>`). The top-level dmpSegment numeric id is not
  directly targetable.
- `staffCountRanges` uses compound URNs of the form `urn:li:staffCountRange:(MIN,MAX)`,
  for example `(501,1000)` or `(10001,2147483647)` for the open-ended "10K+" bucket.

## Good to know

- Costs come back formatted with currency symbols (for example "$1,234.56").
- Entity names are always included, so never show a raw id without its name.
- For Google Ads too, install the companion plugin `google-ads-assistant`. For both
  platforms in one connection, use the unified endpoint at https://api.adplug.app/mcp.
