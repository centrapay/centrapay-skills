# Popup Flow

The Centrapay SDK renders a 'Pay with Centrapay' button and manages the checkout popup. The customer stays on the merchant site (desktop) or opens a new tab (mobile).

## How it works

1. Merchant loads the Centrapay SDK via a `<script>` tag with their Merchant ID.
2. SDK renders the 'Pay with Centrapay' button into a `div#centrapay-button-container`.
3. Customer clicks the button — SDK fires the `onClick` callback.
4. Merchant creates a Payment Request in `onClick` and returns it to the SDK.
5. SDK opens the Centrapay Checkout popup and shows the Payment Request.
6. Customer completes (or dismisses) the popup — SDK fires `onComplete`.
7. Merchant retrieves the Payment Request in `onComplete` and acts on its status.

## SDK

Load with your Merchant ID in the query string:
`https://sdk.centrapay.com/ecommerce/centrapay.js?merchantId={merchantId}`

## Callbacks

**`onClick()`**
- Triggered when the customer clicks the button.
- Must create a Payment Request and return it — the SDK uses it to open the popup.
- If this throws, the popup will not open.

**`onComplete(data)`**
- Triggered when the popup closes — whether the payment succeeded, was cancelled, or the customer dismissed it.
- `data.paymentRequestId` is always provided — retrieve the Payment Request to determine actual status.
- Never assume the payment succeeded just because `onComplete` fired.

## Status handling in onComplete

| Status | Action required |
|---|---|
| `paid` | Show confirmation or redirect to order confirmation page |
| `cancelled` | Inform customer payment was not completed |
| `expired` | Inform customer payment expired |
| `new` | Customer dismissed the popup — **must void the Payment Request** |

## Browser considerations

- **Desktop** — popup opens as an overlay on top of the merchant page.
- **Mobile** — popup opens in a new browser tab. `onComplete` fires when the customer returns to the merchant tab.
- **Popup blockers** — the SDK opens the popup in response to a user gesture (button click), so most browsers allow it. Encourage users to allow popups if blocked.
