# Workshop title, eg. "Payment flow threat model"

- **Facilitator:** Your Name [@your-github-handle] (security champion)
- **Participants:**
  - **Business stakeholder:** _____
  - **Technical architect:** _____
  - **Development lead:** _____
  - **Security analyst:** _____
  - **Privacy officer:** _____
  - **Other stakeholders:** _____
- **Workshop date:** YYYY-MM-DD
- **PR:** #...
- **Scope:** owner/repo@<commit>, or the subsystems / services / data flows assessed
- **Frameworks:** STRIDE, LINDDUN, OWASP Top 10, …

## Summary

A short, single-paragraph verdict on the security and privacy posture of the
scoped system, and the headline risks this workshop surfaced. What is the shape
of the exposure?

## Business context

Why does the system exist, and what business value does it provide? What are the
critical business functions?

Who are the key stakeholders?

What is the business impact of a security or privacy failure – financial,
reputational, regulatory, operational?

## Technical scope

What is being threat modeled – the system boundaries and in-scope components?
What is out of scope?

What is the technology stack? What are the deployment environments and the
integration points with other (out-of-scope) systems?

## System decomposition

How does the system work? Include or link to architecture diagrams, data-flow
diagrams, and other models used in the workshop.

### Key components

| Component | Description | Trust level                        | Data handled |
| --------- | ----------- | ---------------------------------- | ------------ |
| …         | …           | TRUSTED / SEMI-TRUSTED / UNTRUSTED | …            |

### Data flows

| Source | Destination | Data type | Protocol | Authentication |
| ------ | ----------- | --------- | -------- | -------------- |
| …      | …           | …         | …        | …              |

### Sensitive assets

| Asset | Sensitivity | Integrity req. | Availability req. | Privacy concern |
| ----- | ----------- | -------------- | ----------------- | --------------- |
| …     | …           | …              | …                 | …               |

### Entry points

The external interfaces, APIs, and user interfaces to the system.

### Trust boundaries

Where trust changes, eg. internet to DMZ, DMZ to internal network.

## Threat assessment

The core of the workshop. Assess each component, data flow, and asset against
the chosen framework(s). Rate each threat by likelihood and impact to yield a
severity, using a consistent scoring scheme.

| Ref | Component / Flow | Description | Type      | Countermeasures | Likelihood | Impact   | Severity |
| --- | ---------------- | ----------- | --------- | --------------- | ---------- | -------- | -------- |
| TA1 | …                | …           | SPOOFING  | …               | POSSIBLE   | CRITICAL | HIGH     |
| TA2 | …                | …           | TAMPERING | …               | …          | …        | …        |
| TA3 | …                | …           | …         | …               | …          | …        | …        |

Alternatively, assess each critical component against the full list of threat
types (the STRIDE categories, or the OWASP Top 10), one table per component,
to check whether each attack vector can compromise a sensitive asset.

## Component/Flow: _____

| Threat type | Threat description | Asset at risk | Countermeasures | Likelihood | Impact | Rating |
| ----------- | ------------------ | ------------- | --------------- | ---------- | ------ | ------ |
| Spoofing    | ...                | ...           | ...             | ...        | ...    | ...    |
| Tampering   | ...                | ...           | ...             | ...        | ...    | ...    |
| Repudiation | ...                | ...           | ...             | ...        | ...    | ...    |
| Disclosure  | ...                | ...           | ...             | ...        | ...    | ...    |
| DoS         | ...                | ...           | ...             | ...        | ...    | ...    |
| Elevation   | ...                | ...           | ...             | ...        | ...    | ...    |

## Risks raised

Which of the threats above are worth tracking over time, and were therefore
promoted into the [risk register](./REGISTER.md)? List their register references
here. The register owns their ongoing status.

- TA1: Short name
- TA2: Short name

## Mitigation strategies

For each risk raised, the agreed mitigation strategy – or the reasoned decision
to accept the risk with no mitigation. Record enough rationale that a future
reader understands _why_ this response was chosen. Detailed step-by-step
remediation belongs in the code repository's own issue tracker; link out to it.

## Follow-ups

- [ ] Create tickets for mitigation work in the relevant code repositories.
- [ ] Add / update the corresponding rows in the [`register](./REGISTER.md).
- [ ] Schedule the next threat modeling workshop.
