# Impact (impact)

Impact is a partnership management platform that provides REST APIs for managing affiliate relationships, tracking conversions, paying partners, and reporting on partnership performance across brands, publishers, and agencies. The platform powers creator, affiliate, and B2B partnership automation at scale, processing close to $120 billion in partner-referred GMV annually.

APIs.json: https://raw.githubusercontent.com/api-evangelist/impact/refs/heads/main/apis.yml

Naftiko: https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=impact-api-evangelist&utm_content=repo

## Tags

- Affiliate
- Partnerships
- Performance Marketing
- Commission
- Tracking
- Creator Economy
- Partner Management

## APIs

- **Impact Brand API** — REST API for brands to manage partnership campaigns, track conversions, process commissions, and manage contracts. Base URL: `https://api.impact.com/Advertisers/{AccountSID}/`
- **Impact Partner API** — REST API for media partners and publishers to access campaigns, manage commission tracking, and query earning reports. Base URL: `https://api.impact.com/MediaPartners/{AccountSID}/`
- **Impact Agency API** — REST API for agencies to manage multiple client accounts, consolidate reporting, and automate partnership operations at scale. Base URL: `https://api.impact.com/Agencies/{AccountSID}/`

All APIs use HTTP Basic Auth (Account SID + Auth Token) or OAuth 2.0 with PKCE for multi-customer applications.

## Plans, Rate Limits, and FinOps

- **Plans:** [plans/impact-plans-pricing.yml](plans/impact-plans-pricing.yml) — Starter ($30/mo or 3% revenue), Essential ($500/mo), Pro ($2,500/mo), Publisher (free). A 2.5% transaction fee applies across all tiers on commissions processed.
- **Rate Limits:** [rate-limits/impact-rate-limits.yml](rate-limits/impact-rate-limits.yml) — Per-account hourly limits enforced with HTTP 429 responses. Exponential backoff with jitter recommended.
- **FinOps:** [finops/impact-finops.yml](finops/impact-finops.yml) — FOCUS-aligned cost framework covering subscription fees, transaction fees, commission attribution, and optimization guidance.

## Timestamps

- **Created:** 2026-06-13
- **Modified:** 2026-06-13

## Common

| Type | URL |
|------|-----|
| Website | https://impact.com/ |
| Documentation | https://integrations.impact.com/ |
| GitHub Org | https://github.com/ImpactInc |
| LinkedIn | https://www.linkedin.com/company/impactdotcom |
| Blog | https://impact.com/press-releases/ |
| Pricing | https://impact.com/pricing/ |
| X | https://x.com/impactdotcom |

## Maintainers

**Kin Lane** — kin@apievangelist.com
