---
name: partner-weekly-performance-digest
description: >
  Build a weekly performance digest for an impact.com Partner account from live MCP data.
  Use when the user wants a recurring performance summary, week-over-week comparison,
  or a leadership-ready brief of clicks, actions, and earnings.
audience: Partner
required_tools:
  - resolve_query_fields
  - query_performance
optional_tools:
  - resolve_program
deliverable: >
  Markdown digest with headline KPIs (period vs prior), top movers/anomalies,
  exactly three recommended next actions, and tools cited
---

# Partner weekly performance digest

## Prerequisites
- impact.com MCP is connected for a **Partner** account (not Brand).
- Do not ask for or pass `accountId` / `partnerId`. The tool scopes by identity.
- Do **not** use Brand-only tools or args: `resolve_partner`, `resolve_partner_group`, or `affiliateOnly`.

## Gather from the user
1. **Date range**‚ default: last 7 **complete** calendar days ending yesterday (UTC or account-local if the user specifies). Confirm if unclear.
2. **Prior period**‚ equal length immediately before the primary range (for period-over-period). Example: primary `2026-07-28`‚`2026-08-03`, compare `2026-07-21`‚`2026-07-27`.
3. **Scope**‚ whole partner account (default), or a named brand/program to filter. If they name a program/brand, resolve it before querying.
4. **Metrics focus**‚ if they name metrics in business language, resolve fields first. Otherwise default intent: clicks, actions, and earnings/revenue (whichever exists on the partner model).

## Steps

### 1) Resolve field names
Call `resolve_query_fields` with terms such as:
`["clicks", "actions", "earnings", "revenue", "program", "brand", "event_date_local"]`

Use only the **exact API names** returned for the partner catalog (`partner_affiliate_performance`). If a term has no match, skip it or ask the user. Never guess field names.

### 2) Optional program filter
If the user names a brand/program:
1. Call `resolve_program` with that name.
2. If confidence is low or multiple matches, ask the user to pick.
3. Pass a filter on later queries: `{"dimension":"program","operation":"in","values":["<numericId>"]}` (use the resolved dimension name if the partner model uses a different API name for program/brand).
4. Never put names in filter `values`. Use IDs only.

### 3) Headline KPIs (period vs prior)
Call `query_performance` with:
- `measures`: resolved click / action / earnings (or revenue) fields ‚Äî at least one measure
- `dimensions`: prefer a low-cardinality breakdown that still returns account-level totals you can summarize (e.g. resolved `event_date_local` for a daily rollup you then total, or resolved program/brand if that is the natural partner grain). If unsure which dimension fits totals, resolve `"date"` / `"day"` / `"event_date_local"` and use the best match; then sum measures across rows for the headline.
- `dateFrom` / `dateTo`: primary range (`YYYY-MM-DD`)
- `compareDateFrom` / `compareDateTo`: prior equal-length range
- `limit`: enough to cover the primary range (e.g. 31 for daily)
- `chartType`: `none` unless the user asks for a chart
- Do **not** set `affiliateOnly`

Present headline KPIs using display names from the response `columns` when available. Show primary vs prior (and % change when both periods have values). If comparison data is missing, say so and show primary only.

### 4) Top movers / anomalies
Call `query_performance` again (same dates; comparison optional) with:
- Same core `measures`
- `dimensions`: resolved program/brand (or the best partner-side entity dimension from step 1) ‚Äî goal is "what moved"
- `orderByField`: primary earnings/revenue measure (or actions if earnings unavailable)
- `orderDirection`: `DESC`
- `limit`: `10`

Call out:
- Top 3 to 5 entities by the primary measure
- Any sharp day-over-day swings if you have the daily series from step 3
- Gaps / zeros / empty result sets explicitly. Do not invent figures

### 5) Write the digest
Produce markdown with these sections **in order**:

#### Headline KPIs
Table or bullets: each measure for the primary period, prior period, and change (when available). State the exact date ranges used.

#### Top movers / anomalies
Short bullets tied to tool rows (entity + measure + direction). If nothing notable, say "No material movers in this range."

#### Recommended next actions
Exactly **three** actions. Each must reference a specific finding from the data (e.g. "Investigate Program X, actions down 40% WoW while clicks were flat"). No generic advice disconnected from the numbers.

#### Tools used
List every MCP tool actually called (e.g. `resolve_query_fields`, `query_performance`, and `resolve_program` if used).

## Empty / error handling
- Empty or all-zero results: say so; suggest a wider date range or removing a program filter. Do not fabricate metrics.
- Unknown field errors: call `resolve_query_fields` again with corrected terms and retry once.
- Auth / tool failures: neutral language ("I couldn't pull performance data just now"); do not explain MCP/DataLab internals.
- Wrong account type (Brand session): stop and tell the user this skill is for Partner accounts.

## Do not
- Invent metrics, currencies, or rankings not present in tool JSON
- Use Brand-only resolvers or `affiliateOnly`
- Put partner/program/brand **names** in `query_performance` filter values
- Include an account / partner_id filter (identity-scoped)
- Call tools outside this skill unless the user explicitly expands the ask
- Describe internal API field names to the user when a display name exists