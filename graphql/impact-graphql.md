# Impact GraphQL Schema

This document describes the conceptual GraphQL schema for the Impact partnership automation platform (impact.com). Impact provides REST APIs for brands, publishers, and agencies to manage affiliate and partnership programs, track conversions, process commissions, and report on performance.

## Overview

The Impact GraphQL schema models the full lifecycle of partnership management:

- **Partners and Publishers** — onboarding, profiling, and status management for affiliate and media partners
- **Campaigns and Contracts** — program terms, promotional rules, and contractual agreements between brands and partners
- **Tracking** — clicks, impressions, conversions, and deep links tied to partner activity
- **Commissions and Payouts** — commission rules, adjustments, payment batches, and payout records
- **Creative and Catalog** — ad groups, ad units, product catalogs, coupons, and promotional media
- **Reporting and Segments** — performance reports, segment filters, and analytics
- **Account and Auth** — advertiser/publisher account details, API keys, tokens, and webhooks

## Schema Source

Derived from the Impact REST API surface documented at https://integrations.impact.com/ covering the Brand API, Partner API, and Agency API endpoints.

## Types

### Partner Domain

| Type | Description |
|---|---|
| `Partner` | A media partner or affiliate enrolled in one or more programs |
| `PartnerDetails` | Extended profile data for a partner |
| `PartnerType` | Enum classifying partner category (Content, Coupon, Loyalty, Sub-affiliate, etc.) |
| `PartnerStatus` | Enum for partner program enrollment state (Pending, Approved, Rejected, Suspended) |
| `PartnerProfile` | Public-facing profile including site description, audience, and promotional methods |
| `Publisher` | A publisher/media partner entity in the Impact network |
| `PublisherDetails` | Extended publisher metadata including traffic sources and audience demographics |

### Campaign Domain

| Type | Description |
|---|---|
| `Campaign` | A partnership program or campaign run by a brand |
| `CampaignDetails` | Extended campaign configuration and performance summary |
| `CampaignType` | Enum for campaign model (CPA, CPC, CPL, Flat Fee, etc.) |
| `CampaignStatus` | Enum for campaign lifecycle state (Active, Paused, Archived) |
| `CampaignTerms` | Commission and promotional terms associated with a campaign |

### Action and Conversion Domain

| Type | Description |
|---|---|
| `Action` | A tracked partner-driven event (sale, lead, install, etc.) |
| `ActionDetails` | Extended action metadata including customer and order context |
| `ActionType` | Enum classifying tracked action category |
| `ActionStatus` | Enum for action state (Pending, Approved, Reversed, Locked) |
| `Conversion` | A completed conversion event attributed to a partner |
| `ConversionDetails` | Order-level and item-level details for a conversion |
| `ConversionAmount` | Monetary amounts (order value, commission, taxes) for a conversion |
| `ConversionLock` | Lock state and date for a conversion pending commission processing |
| `Click` | A tracked partner click event |
| `ClickDetails` | Extended click metadata including referrer, IP, and user-agent |
| `Impression` | A tracked ad impression event |
| `ImpressionDetails` | Extended impression metadata |

### Commission and Payment Domain

| Type | Description |
|---|---|
| `Commission` | A commission record tied to a conversion or action |
| `CommissionDetails` | Extended commission context including rule applied and payout schedule |
| `CommissionType` | Enum for commission structure (Percentage, Flat, Tiered, Bonus) |
| `CommissionAmount` | Computed commission monetary value |
| `Adjustment` | A manual or automated adjustment to a commission amount |
| `AdjustmentDetails` | Reason, amount, and approval details for an adjustment |
| `Payment` | A payment batch issued to one or more partners |
| `PaymentDetails` | Batch-level metadata for a payment run |
| `PaymentStatus` | Enum for payment state (Pending, Processing, Completed, Failed) |
| `Payout` | An individual payout record within a payment batch |
| `PayoutDetails` | Partner-level payout breakdown and transaction reference |

### Contract Domain

| Type | Description |
|---|---|
| `Contract` | A signed agreement between a brand and a partner |
| `ContractDetails` | Full contract record including effective dates and signatories |
| `ContractTerms` | Negotiated commission rates, cookie windows, and promotional restrictions |

### Catalog and Creative Domain

| Type | Description |
|---|---|
| `Catalog` | A product catalog shared by a brand with partners |
| `CatalogDetails` | Feed URL, update schedule, and product count for a catalog |
| `AdGroup` | A grouping of ad creatives for a campaign |
| `Ad` | An individual ad creative unit |
| `AdDetails` | Extended ad metadata including approval status and performance |
| `AdSize` | Dimensions of a display ad unit |
| `AdType` | Enum for ad format (Banner, Text, Video, Native, Email) |
| `Link` | A standard affiliate tracking link |
| `TrackingLink` | A generated tracking URL with embedded partner and campaign tokens |
| `Coupon` | A promotional coupon code associated with a campaign |
| `CouponDetails` | Coupon expiry, discount value, and usage restrictions |
| `DeepLink` | A deep link generated for mobile app or page-level tracking |
| `Media` | A media asset (image, video, creative file) |
| `Promotion` | A time-limited promotional offer tied to a campaign |
| `PromotionDetails` | Promotion dates, terms, and eligible partner types |

### Reporting and Segmentation Domain

| Type | Description |
|---|---|
| `Report` | A generated performance report |
| `ReportDetails` | Report configuration, date range, dimensions, and metrics |
| `Segment` | A defined audience or partner segment for targeted reporting |
| `SegmentFilter` | A filter criterion applied to a segment definition |

### Account and Auth Domain

| Type | Description |
|---|---|
| `AccountDetails` | Top-level account metadata for a brand, publisher, or agency |
| `Advertiser` | A brand or advertiser entity in the Impact platform |
| `AdvertiserDetails` | Extended advertiser profile including industry and billing info |
| `APIKey` | An API credential used to authenticate Impact REST API calls |
| `Token` | An OAuth or session token for API access |
| `Webhook` | A configured webhook endpoint for real-time event delivery |

## Key Queries

```graphql
# Retrieve all partners enrolled in a campaign
query CampaignPartners($campaignId: ID!, $status: PartnerStatus) {
  campaignPartners(campaignId: $campaignId, status: $status) {
    partner { id name partnerType status }
    profile { website audienceSize promotionalMethods }
  }
}

# Retrieve conversion report for a date range
query Conversions($startDate: String!, $endDate: String!, $partnerId: ID) {
  conversions(startDate: $startDate, endDate: $endDate, partnerId: $partnerId) {
    id action { id actionType status }
    amount { orderValue commissionValue currency }
    lock { isLocked lockDate }
  }
}

# Retrieve payout details for a payment batch
query PaymentBatch($paymentId: ID!) {
  payment(id: $paymentId) {
    id status details { issueDate totalAmount currency }
    payouts { partner { id name } amount { value currency } }
  }
}
```

## Key Mutations

```graphql
# Approve a pending partner application
mutation ApprovePartner($partnerId: ID!, $campaignId: ID!) {
  approvePartner(partnerId: $partnerId, campaignId: $campaignId) {
    partner { id status }
  }
}

# Create a tracking link for a partner
mutation CreateTrackingLink($partnerId: ID!, $campaignId: ID!, $destinationUrl: String!) {
  createTrackingLink(partnerId: $partnerId, campaignId: $campaignId, destinationUrl: $destinationUrl) {
    id trackingUrl
  }
}

# Submit a commission adjustment
mutation CreateAdjustment($actionId: ID!, $amount: Float!, $reason: String!) {
  createAdjustment(actionId: $actionId, amount: $amount, reason: $reason) {
    id status details { reason amount }
  }
}
```

## Reference

- Impact Developer Documentation: https://integrations.impact.com/
- Impact Brand API: https://api.impact.com/Advertisers/{AccountSID}/
- Impact Partner API: https://api.impact.com/MediaPartners/{AccountSID}/
- Impact Agency API: https://api.impact.com/Agencies/{AccountSID}/
