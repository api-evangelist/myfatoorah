# MyFatoorah (myfatoorah)

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
