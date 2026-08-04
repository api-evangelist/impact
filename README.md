# Impact (impact)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
