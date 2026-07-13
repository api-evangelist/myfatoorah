# MyFatoorah (myfatoorah)

MyFatoorah is a Kuwait-based online payment gateway and invoicing platform serving merchants across the GCC and wider MENA region (Kuwait, Saudi Arabia, UAE, Qatar, Egypt, Bahrain, Oman, and Jordan). Its v2 REST API lets businesses create invoices and payment links, execute and direct-charge card payments, run embedded checkout sessions, issue refunds, manage recurring payments, onboard marketplace suppliers, calculate shipping, and receive webhook notifications. MyFatoorah aggregates regional and international payment methods including KNET, mada, Benefit, Meeza, OmanNet, Visa, Mastercard, American Express, Apple Pay, Google Pay, and STC Pay.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/myfatoorah/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/myfatoorah/refs/heads/main/apis.yml)

## Access Model (Read First)

MyFatoorah's API is **region-scoped** and authenticated with a **Bearer API token**. There is no public self-serve API key for live traffic — you register a merchant account, complete KYC, and generate tokens from the portal (Integration Settings → API Key). Each enabled country requires its own token, and a token only works against endpoints it has been granted permission for (otherwise `401 - The token does not have the required permissions!`).

**Environments and hosts (confirmed from docs):**

| Environment / Region | API Base URL | Portal |
|---|---|---|
| Test / Sandbox (all regions) | `https://apitest.myfatoorah.com/` | `https://demo.myfatoorah.com/` |
| Live — Kuwait, Bahrain, Jordan, Oman | `https://api.myfatoorah.com/` | `https://portal.myfatoorah.com/` |
| Live — Saudi Arabia | `https://api-sa.myfatoorah.com/` | `https://sa.myfatoorah.com/` |
| Live — UAE | `https://api-ae.myfatoorah.com/` | `https://ae.myfatoorah.com/` |
| Live — Qatar | `https://api-qa.myfatoorah.com/` | `https://qa.myfatoorah.com/` |
| Live — Egypt | `https://api-eg.myfatoorah.com/` | `https://eg.myfatoorah.com/` |

Endpoints live under a `/v2` path prefix (e.g. `https://api.myfatoorah.com/v2/ExecutePayment`). The **test** environment uses a published demo token and test cards so you can integrate without moving real funds. **Pick the base host that matches your account's country** — do not assume a single global host.

**No WebSocket API.** MyFatoorah's public surface is REST over HTTPS. Asynchronous updates are delivered as **HTTP POST webhook callbacks** (transaction and refund status changes) to a merchant-configured URL; you can also poll `GetPaymentStatus` (which is rate limited). No `wss://` endpoint is documented. See `review.yml`.

**Confirmed vs modeled.** `InitiatePayment`, `ExecutePayment`, `SendPayment`, `GetPaymentStatus`, and `MakeRefund` are grounded (method, path, core fields) in the public docs. Other operations (sessions, suppliers, shipping, recurring, webhooks, supplier refunds, token cancellation) are documented by name and follow the `/v2/<Endpoint>` POST convention; their schemas in the OpenAPI are representative and marked `x-status: modeled`.

## Tags

- Payments
- Payment Gateway
- Kuwait
- GCC
- MENA
- KNET
- mada
- Benefit
- Invoices
- Cards
- Fintech

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### MyFatoorah Payments API

Discover enabled payment methods and their service charges (InitiatePayment), then create an invoice against a chosen gateway and get a hosted PaymentURL (ExecutePayment). Supports KNET, mada, Benefit, Visa, Mastercard, Apple Pay, Google Pay, and other regional methods, with optional direct card charging.

- **Human URL:** [https://docs.myfatoorah.com/docs/execute-payment](https://docs.myfatoorah.com/docs/execute-payment)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Payments
- Cards
- KNET
- Checkout

#### Properties

- [Documentation](https://docs.myfatoorah.com/docs/initiate-payment)
- [API Reference](https://docs.myfatoorah.com/reference)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Invoicing API

Create a MyFatoorah invoice and generate a shareable payment link (SendPayment), delivered to the customer by email, SMS, or link, with line items, display currency, and success/error callback URLs.

- **Human URL:** [https://docs.myfatoorah.com/docs/send-payment](https://docs.myfatoorah.com/docs/send-payment)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Invoices
- Payment Links
- Billing

#### Properties

- [Documentation](https://docs.myfatoorah.com/docs/send-payment)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Payment Status API

Verify whether an invoice was paid and retrieve its transactions by InvoiceId, PaymentId, or CustomerReference (GetPaymentStatus). Returns invoice status plus masked card, gateway, authorization, and error details for reconciliation.

- **Human URL:** [https://docs.myfatoorah.com/docs/get-payment-status](https://docs.myfatoorah.com/docs/get-payment-status)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Payment Status
- Inquiry
- Reconciliation

#### Properties

- [Documentation](https://docs.myfatoorah.com/docs/get-payment-status)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Refunds API

Issue full or partial refunds against a paid invoice or payment (MakeRefund), track refund processing (GetRefundStatus), and handle marketplace supplier refunds (MakeSupplierRefund).

- **Human URL:** [https://docs.myfatoorah.com/docs/make-refund](https://docs.myfatoorah.com/docs/make-refund)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Refunds
- Reversals

#### Properties

- [Documentation](https://docs.myfatoorah.com/docs/make-refund)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Embedded Sessions API

Create a session for the embedded/hosted card-entry component (InitiateSession), then execute it, and manage saved-card tokens (CancelToken). Powers PCI-friendly in-page checkout that keeps card data off the merchant server.

- **Human URL:** [https://docs.myfatoorah.com/docs/initiate-session](https://docs.myfatoorah.com/docs/initiate-session)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Embedded Payment
- Sessions
- Tokenization

#### Properties

- [Documentation](https://docs.myfatoorah.com/docs/initiate-session)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Suppliers API

Onboard and manage marketplace suppliers for split settlements — create and edit suppliers, list them and their details, set per-method commissions, transfer balances, and view supplier deposits and dashboards.

- **Human URL:** [https://docs.myfatoorah.com/reference](https://docs.myfatoorah.com/reference)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Marketplace
- Suppliers
- Split Payments

#### Properties

- [API Reference](https://docs.myfatoorah.com/reference)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Shipping API

Look up shipping countries and cities, calculate DHL/Aramex shipping charges, request pickups, update shipping status, and list shipping orders alongside a MyFatoorah invoice.

- **Human URL:** [https://docs.myfatoorah.com/docs/shipping-sample-code](https://docs.myfatoorah.com/docs/shipping-sample-code)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Shipping
- Logistics
- DHL
- Aramex

#### Properties

- [Documentation](https://docs.myfatoorah.com/docs/shipping-sample-code)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Recurring Payments API

Manage recurring card payments — retrieve a recurring payment record (GetRecurringPayment), cancel a schedule (CancelRecurringPayment), and resume or retry a failed cycle (ResumeRecurringPayment).

- **Human URL:** [https://docs.myfatoorah.com/reference](https://docs.myfatoorah.com/reference)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Recurring
- Subscriptions

#### Properties

- [API Reference](https://docs.myfatoorah.com/reference)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### MyFatoorah Webhooks API

Retrieve webhook events MyFatoorah has triggered (GetWebhooks). MyFatoorah delivers transaction and refund status changes as HTTP POST callbacks to a merchant-configured endpoint; there is no WebSocket transport.

- **Human URL:** [https://docs.myfatoorah.com/reference](https://docs.myfatoorah.com/reference)
- **Base URL:** `https://api.myfatoorah.com/v2`

#### Tags

- Webhooks
- Notifications
- Events

#### Properties

- [API Reference](https://docs.myfatoorah.com/reference)
- [OpenAPI](openapi/myfatoorah-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/myfatoorah.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/myfatoorah.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Domain Security](security/myfatoorah-domain-security.yml)
- [Authentication](authentication/myfatoorah-authentication.yml)
- [GitHub Organization](https://github.com/MyFatoorahHub)
- [LinkedIn](https://www.linkedin.com/company/myfatoorah)
- [Website](https://myfatoorah.com/)
- [Documentation](https://docs.myfatoorah.com/)
- [Plans](plans/myfatoorah-plans-pricing.yml)
- [Rate Limits](rate-limits/myfatoorah-rate-limits.yml)
- [Fin Ops](finops/myfatoorah-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
