---
name: update-register
description: >-
  Update the living risk register without conducting a threat modeling workshop.
  Use this skill when the user says something like "update the register",
  "mark risk TA1 as mitigated", "the MFA mitigation shipped",
  "review the register", or "retire this risk".
license: CC0-1.0
metadata:
  interactive: yes
  preferred_model: ollama/WORKFLOW_STANDARD
---

# Update register

Use this skill to keep the living
[`risks/REGISTER.md`](../../../risks/REGISTER.md) current between sessions —
updating a tracked risk's status in place as it evolves. Unlike a session
report, the register is living documentation: it MUST reflect the current
state of the system's security posture at all times.

Use it when:

- A mitigation has shipped (or progressed), so a risk's status, severity, or
  residual risk changes.
- A scheduled review is due — refresh the `Reviewed` date and re-confirm the
  ratings.
- A risk no longer applies (designed out, component removed, fully
  mitigated) — retire it.
- A finding from an [audit](https://github.com/kieranpotts/audits) is worth
  tracking over time — promote it into a new register row.

Do NOT use this skill to run a new threat modeling session — use
[draft report](../draft-report/SKILL.md). Do NOT edit any merged
session report; those are immutable.

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Description of the change — REQUIRED.** Which risk changed, and how
  (mitigation shipped, severity change, scheduled review, retirement, or an
  audit finding to promote). Prompt the user if not provided.

## Success criteria

You will achieve the following outcomes:

<!-- A `register/<slug>` branch, with the affected rows of `risks/REGISTER.md`
updated in place, committed to a pull request opened against `main`,
merged once the change it reflects is real. -->

- Branch `register/<slug>` MUST exist, and only `risks/REGISTER.md` MUST be
  changed.

- The affected rows MUST reflect the current, true status of each risk, with
  an updated `Reviewed` date.

- A pull request titled `register: <short lowercase description>` MUST be
  open (or merged, once the change is real).

## Instructions

1.  Create the branch.

    ```sh
    git checkout main
    git pull --rebase
    git checkout -b register/<slug>
    ```

    Use a slug that names the change, eg. `register/mfa-rollout` or
    `register/q3-review`.

2.  Update the affected rows in place.

    Edit [`risks/REGISTER.md`](../../../risks/REGISTER.md) only. Update the
    mitigation status, severity, residual risk, and `Reviewed` date of the
    affected rows. To promote an audit finding, add a new row with a fresh
    reference number. To retire a risk, move its row to the "Retired risks"
    section with a closing note — never delete it.

3.  Commit and open a pull request.

    ```sh
    git add risks/REGISTER.md
    git commit -m "register: <short lowercase description>"
    git push -u origin register/<slug>
    gh pr create --title "register: <short lowercase description>" --fill
    ```

4.  Merge when the change is real.

    Merge only once the change the update reflects is actually true — eg.
    once a mitigation has shipped to production — so the register never
    overstates the system's security posture. Squash-merge with a
    `register: <description>` message and delete the branch.

## Rules

- You MUST NOT edit a merged session report.

  Session reports are immutable point-in-time snapshots. All ongoing status
  lives in the register.

- You MUST update rows in place and MUST NOT delete a row.

  Reference numbers are stable. Retire a risk by moving it to the "Retired
  risks" section with a reason, so the register keeps a full account of what
  was ever tracked.

- You MUST NOT mark a risk mitigated, or lower its residual risk, until the
  mitigation is genuinely in place.

  Merge the register change alongside — or after — the real-world change it
  describes, so the register never overstates the posture.

- A reassessment that finds a new threat MUST be drafted as a session,
  not filed as an update.

  If the change amounts to fresh threat identification rather than
  status-tracking of a known risk, use
  [draft report](../draft-report/SKILL.md) instead.
