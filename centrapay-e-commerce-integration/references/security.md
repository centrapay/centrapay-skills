# Security Considerations

## API Key Storage

API keys must never be exposed to the client side (browser). Store them in environment variables on the backend and proxy all Centrapay API requests through a server endpoint.

**Why this is important**: a client-side API key can be extracted by anyone inspecting network requests or page source. It would allow them to create Payment Requests on behalf of the merchant.

## Payment Request Construction

Always construct the Payment Request body on the server — never accept amount or currency values from the client. A client supplying `amount: 1` would allow them to pay $0.01 for any order.

## Redirect URL Validation

For the redirect flow, Centrapay only redirects to domains registered in advance. Ensure registered domains are kept up to date as environments change (e.g. staging vs production).

## Payment Request ID Persistence

Store the Payment Request ID server-side before redirecting the customer. If the customer never returns (closed tab, network error), the ID must still be retrievable so the Payment Request can be voided.

## Voiding Abandoned Requests

Any Payment Request with status `new` at the end of the flow must be voided. Leaving open Payment Requests can result in unexpected payments if the customer later returns to the Centrapay page.
