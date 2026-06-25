# Redirect Flow

The customer is redirected away from the merchant site to complete payment on Centrapay, then redirected back.

## How it works

1. Merchant creates a Payment Request with `redirectPaidUrl` and `redirectCancelUrl`.
2. Centrapay returns a `url` — the merchant redirects the customer there.
3. Customer completes (or abandons) payment on Centrapay.
4. Centrapay redirects the customer back with `paymentRequestId` appended as a query parameter.
5. Merchant retrieves the Payment Request and acts on its status.

## Requirements

- `redirectPaidUrl` and `redirectCancelUrl` are both required when creating the Payment Request.
- Redirect domains must be registered with Centrapay before go-live — the merchant must provide allowed domains when they contact integrations@centrapay.com.

## Status handling on return

| Status | Redirect destination | Action required |
|---|---|---|
| `paid` | `redirectPaidUrl` | Show confirmation |
| `cancelled` | `redirectCancelUrl` | Inform customer payment was not completed |
| `expired` | `redirectCancelUrl` | Inform customer payment expired |
| `new` | `redirectCancelUrl` | Customer abandoned — **must void the Payment Request** |

## Edge cases

- **Customer clicks back before completing payment** — the customer navigates back to the merchant site using the browser's back button without completing payment. Centrapay does not redirect in this case, so neither redirect URL is hit. The merchant must store the Payment Request ID before redirecting, then on page load check whether there is a pending request. Poll the Payment Request status and void it if it is `new`.
- **Customer closes the tab** — same outcome as clicking back. The Payment Request ID must be stored before redirecting so it can be retrieved and voided if the order is never confirmed.
- **Redirect fails or is blocked** — the customer may be stuck on the Centrapay page. The Payment Request will eventually expire, but voiding proactively is preferable.
- **Duplicate redirect** — a customer may hit the return URL more than once (e.g. using the back button after landing on the paid URL). Always retrieve the Payment Request status rather than assuming it matches the URL they landed on.
