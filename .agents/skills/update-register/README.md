# Update register

Keeps the living risk register current between sessions.

## What it does

- Creates a `register/<slug>` branch from `main`.

- Updates the affected rows of `risks/REGISTER.md` in place — mitigation status,
  severity, residual risk, and review date — or promotes an audit finding into a
  new tracked row, or retires a risk that no longer applies.

- Commits, pushes, and opens a pull request titled `register: <description>`,
  merged once the change it reflects is real.

Merged session reports are never touched.

## How to invoke

> Update register

> The MFA mitigation for TA1 is shipped.

## Examples

- `/update-register`: The agent asks which risk changed and how, then makes the
  edit.

- `/update-register Mark TA3 as mitigated, residual risk low`: The agent updates
  that row in place and opens the PR.

Use [`/draft-report`](../draft-report/README.md) instead when running a new
threat model that identifies fresh threats.
