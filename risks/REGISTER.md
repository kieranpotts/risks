# Risk register

The living register of security and privacy risks currently tracked for the
Acme Catalog & Storefront platform. This is the single source of truth for where
each risk stands right now.

The register is living documentation. Rows are updated in place as mitigations
are applied, risks are reassessed, and scheduled reviews are performed. A row
is merged (or updated) to `main` alongside the change it describes, so the
register never overstates or understates the system's actual security posture.

Each row is sourced from a threat modeling session — see the [index](./INDEX.md).

## Fields

- **Ref:** Unique reference number, eg. `TA1`. Prefix by source where useful, eg.
  `TA` = threat assessment, `AS` = AppSec/audit, etc.

- **Risk:** Short, descriptive, unique name. Must say what is impacted.

- **Type:** Threat classification, using a consistent framework (STRIDE,
  LINDDUN, OWASP, etc.).

- **Details:** Where and how the threat occurs. Be specific.

- **Probability:** PROBABLE | LIKELY | POSSIBLE | UNLIKELY | RARE

- **Impact:** CATASTROPHIC | CRITICAL | SEVERE | MARGINAL | NEGLIGIBLE

- **Severity:** CRITICAL | HIGH | MEDIUM | LOW — combined probability × impact.

- **Mitigation:** The mitigation steps, or an explicit decision to accept the
  risk. Link out to the code repo's issue/PR tracking the work.

- **Status:** PENDING | IN PROGRESS | COMPLETED

- **Residual risk:** CRITICAL | HIGH | MEDIUM | LOW — what remains after
  mitigation.

- **Reviewed:** Date the risk was last reviewed (`YYYY-MM-DD`).

## Register

Sort by severity (critical first), then by residual risk. Retire a risk by
moving its row to the "Retired risks" section, below, with a closing note —

| Ref | Risk                    | Type     | Details                 | Probability | Impact   | Severity | Mitigation                        | Status  | Residual risk | Reviewed   |
| --- | ----------------------- | -------- | ----------------------- | ----------- | -------- | -------- | --------------------------------- | ------- | ------------- | ---------- |
| TA1 | Unverified Stripe webhook signatures | SPOOFING | Stripe webhook handler in `acme/payments-service` accepts payment-status updates without verifying the `Stripe-Signature` header, so a forged callback could mark an unpaid order as paid | LIKELY | CRITICAL | HIGH | Verify the Stripe webhook signature (HMAC over the raw body against the endpoint signing secret) before acting on any event; see [workshop report](./2026-06-15-acme-payment-flow/) | PENDING | LOW | 2026-06-15 |
| TA2 | Indefinite retention of raw webhook payloads | DISCLOSURE | Payment event store retains full Stripe webhook payloads, including card-network response fields, with no redaction or retention limit | POSSIBLE | SEVERE | MEDIUM | Redact card-network fields before persistence; see [workshop report](./2026-06-15-acme-payment-flow/) | PENDING | LOW | 2026-06-15 |

## Retired risks

Risks that no longer apply. The threat has been designed out, the component
removed, or the risk fully mitigated with negligible residual risk.

| Ref | Risk | Type | Retired | Reason |
| --- | ---- | ---- | ------- | ------ |
