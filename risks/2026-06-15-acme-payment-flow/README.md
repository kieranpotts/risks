# Payment flow threat model

> [!NOTE]
> This is a sample workshop report, included to illustrate the format. It
> describes a fictional system and does not reflect a real workshop.

- **Facilitator:** Jane Doe [@janedoe] (security champion)
- **Participants:**
  - **Business stakeholder:** Sam Lee, Head of Payments
  - **Technical architect:** John Smith
  - **Development lead:** Priya Shah
  - **Security analyst:** Jane Doe
  - **Privacy officer:** Alex Kim
  - **Other stakeholders:** —
- **Workshop date:** 2026-06-15
- **PR:** #22
- **Scope:** acme/payments-service@a1b2c3d, checkout and webhook flows
- **Frameworks:** STRIDE

## Summary

The payment flow's checkout path is well-guarded, but the Stripe webhook
ingestion path acts on callbacks without verifying their signature, and stores
the raw card-network response payload longer than necessary. Both were
raised as risks.

## Business context

The payments service processes customer checkouts and reconciles payment
provider webhooks. A failure here has direct financial and reputational
impact — unauthorized charges, missed refunds, or leaked payment metadata
could trigger regulatory exposure under PCI-DSS.

Key stakeholders are the payments team, customer support (who field
disputes), and legal/compliance.

## Technical scope

In-scope: the checkout API, the Stripe webhook ingestion endpoint, and the
payment event store. Out-of-scope: Stripe's own infrastructure, and the
storefront web client.

Stack: Node.js services behind an API gateway, events persisted to a
Postgres event store. Webhooks arrive from Stripe over the public internet and
are meant to be authenticated via Stripe's request signature.

## System decomposition

### Key components

| Component | Description | Trust level | Data handled |
| --------- | ----------- | ----------- | ------------- |
| Checkout API | Accepts checkout requests, initiates charges | SEMI-TRUSTED | Card tokens, order totals |
| Webhook ingestion | Receives async payment-status callbacks | UNTRUSTED | Payment provider payloads |
| Payment event store | Persists payment events for reconciliation | TRUSTED | Payment metadata, provider payloads |

### Data flows

| Source | Destination | Data type | Protocol | Authentication |
| ------ | ----------- | --------- | -------- | -------------- |
| Storefront web | Checkout API | Card token, order | HTTPS | Session token |
| Stripe | Webhook ingestion | Payment status | HTTPS | Signature header (not verified) |

### Sensitive assets

| Asset | Sensitivity | Integrity req. | Availability req. | Privacy concern |
| ----- | ----------- | --------------- | ------------------ | ---------------- |
| Card tokens | HIGH | HIGH | HIGH | Yes |
| Raw webhook payloads | MEDIUM | MEDIUM | LOW | Yes |

### Entry points

The public checkout API, and the Stripe webhook endpoint.

### Trust boundaries

Internet to API gateway; API gateway to internal services; internal services
to the event store.

## Threat assessment

| Ref | Component / Flow | Description | Type | Countermeasures | Likelihood | Impact | Severity |
| --- | ----------------- | ------------ | ---- | ---------------- | ---------- | ------ | -------- |
| TA1 | Stripe webhook ingestion | The handler acts on payment-status callbacks without verifying the `Stripe-Signature` header, so a forged callback could mark an unpaid order as paid | SPOOFING | None currently | LIKELY | CRITICAL | HIGH |
| TA2 | Payment event store | Raw webhook payloads retained indefinitely, including card-network response fields | DISCLOSURE | Access-controlled DB, no field-level redaction | POSSIBLE | SEVERE | MEDIUM |

## Risks raised

- TA1: Unverified Stripe webhook signatures
- TA2: Indefinite retention of raw webhook payloads

## Mitigation strategies

**TA1:** Verify the Stripe webhook signature — compute the HMAC over the raw
request body using the endpoint's signing secret and reject any event whose
`Stripe-Signature` header does not match — before acting on any status update.
Treated as urgent given the direct spoofing risk to payment state.

**TA2:** Redact card-network response fields from the stored payload before
persistence, retaining only the fields needed for reconciliation. Accepted
as a near-term follow-up rather than urgent, since access to the event
store is already restricted to the payments team.

## Follow-ups

- [ ] Create ticket in acme/payments-service for Stripe webhook signature verification.
- [ ] Create ticket in acme/payments-service for webhook payload redaction.
- [ ] Add TA1 and TA2 to the [register](../REGISTER.md).
- [ ] Schedule the next threat modeling workshop.
