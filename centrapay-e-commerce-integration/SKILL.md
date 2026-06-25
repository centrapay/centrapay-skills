---
name: centrapay-e-commerce-integration
description: Use when implementing a Centrapay e-commerce integration. Guides through prerequisites, choosing between redirect and popup payment flows, optional extensions, and the integration checklist required before Centrapay enables live payments.
---
# Centrapay E-commerce Integration

## Prerequisites - confirm before proceeding
The user must have already contacted integrations@centrapay.com and received:
- API Key, Merchant ID, Merchant Config ID

If not, stop — this must happen first.

> API keys must never be client-side. All Centrapay API requests go through a backend server.

## Gather requirements
Ask the user:
1. **Flow**: Redirect (simpler, customer leaves page) or Popup (SDK required, customer stays on page)?
2. **Line items**: Do they need itemised order details sent with the payment request?

Once answered, you can implement.

## Implementation

Reference the [official guide](https://docs.centrapay.com/guides/ecommerce-website/)

Based on the chosen flow, read the corresponding reference before writing any code:
- Redirect → `references/redirect-flow.md`
- Popup → `references/popup-flow.md`

## Before going live
Remind the user that live payments require completing a Centrapay-provided integration checklist.
