---
name: update-register
description: >-
  Update the living risk register without conducting a threat modeling
  workshop. Use this skill when the user says something like "update the
  register", "mark risk TA1 as mitigated", "the MFA mitigation shipped",
  "review the register", or "retire this risk". Do not use this skill to
  identify new threats, which needs a workshop and its own report.
compatibility: >-
  requires Read, Edit, Bash (git, gh)
license: CC0-1.0
---

# Update register

Keep the living risk register at
[`risks/REGISTER.md`](../../../risks/REGISTER.md) current between workshops, by
updating a tracked risk's status in place. Unlike a workshop report, which is a
frozen point-in-time snapshot, the register MUST reflect the true state of the
system's security posture at all times.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Description of the change — REQUIRED.** Which tracked risk changed, and
  how. This is normally one of: a mitigation that has shipped or progressed; a
  scheduled review falling due; a risk that no longer applies and should be
  retired; or a finding from an external audit or review that is worth
  tracking over time as a new row.

- **Slug — OPTIONAL.** A short, hyphen-delimited name for the change, used for
  the branch and derived from the description if the user does not give one,
  eg. `mfa-rollout` or `q3-review`.

- **Reality check — REQUIRED before merging.** Whether the change the update
  describes is actually true yet, eg. whether the mitigation has really
  reached production. Ask the user if this cannot be established from context.

## Success criteria

- Branch `latest/register/<slug>` MUST exist, cut from `latest/main`.

- [`risks/REGISTER.md`](../../../risks/REGISTER.md) MUST be the only file
  changed.

- The affected rows MUST state the current, true status of each risk, and MUST
  carry an updated `Reviewed` date.

- Every register row that existed before the change MUST still be present. A
  risk that no longer applies is moved to the "Retired risks" section rather
  than deleted.

- A pull request titled `update: <short lowercase description>` MUST be
  open, or merged where the change it reflects is already real.

- Every merged workshop report MUST be untouched, along with
  [`risks/INDEX.md`](../../../risks/INDEX.md).

## Instructions

1.  Establish what changed, and confirm it against the register.

    Read [`risks/REGISTER.md`](../../../risks/REGISTER.md) and identify the
    rows affected, by their reference numbers. If the change amounts to fresh
    threat identification rather than status-tracking of a known risk, stop:
    that needs a threat modeling workshop and a report of its own.

2.  Create the branch.

    ```sh
    git checkout latest/main
    git pull --rebase
    git checkout -b latest/register/<slug>
    ```

3.  Update the affected rows in place.

    Edit [`risks/REGISTER.md`](../../../risks/REGISTER.md) and nothing else.
    Revise the mitigation, status, severity, residual risk, and `Reviewed`
    date of each affected row, using the value vocabularies the register
    documents for those fields.

    To promote an external audit or review finding, add a new row with a fresh
    reference number, prefixed by its source. To retire a risk, move its row
    to the "Retired risks" section with a closing note.

4.  Commit and open a pull request.

    ```sh
    git add risks/REGISTER.md
    git commit -m "update: <short lowercase description>"
    git push -u origin latest/register/<slug>
    gh pr create --title "update: <short lowercase description>" --fill
    ```

    Open it ready for review, not as a draft: a register update is a small,
    factual change, and there is no drafting stage to protect.

5.  Merge only once the change the update describes is genuinely real — eg.
    once the mitigation has actually shipped to production — so that the
    register never overstates the security posture. Squash-merge with the
    `update: <description>` message and delete the branch.

    ```sh
    gh pr merge <number> --squash \
      --subject "update: <short lowercase description>" --delete-branch
    ```

6.  Summarize what you did, naming the rows you changed and their new status.

## Rules

- You MUST NOT edit a merged workshop report.

  Reports are immutable point-in-time snapshots of what a workshop found. All
  ongoing status lives in the register, which is the single source of truth
  for where each risk stands right now.

- You MUST update rows in place, and MUST NOT delete a row.

  Reference numbers are stable and are cited from workshop reports. Retire a
  risk by moving it to the "Retired risks" section with a reason, so the
  register keeps a full account of everything it ever tracked.

- You MUST NOT mark a risk mitigated, or lower its residual risk, before the
  mitigation is genuinely in place.

  Merge the register change alongside — or after — the real-world change it
  describes.

- A reassessment that identifies a new threat MUST be written up as a
  workshop report, not filed as a register update.

  Register rows record threats that an assessment actually raised. Adding a
  row for a threat nobody assessed puts an unfounded entry in the living
  register.

- You MUST NOT track the mitigation work itself here.

  This repository is discovery and record-keeping only. Step-by-step
  remediation belongs in the affected code repository's own issue tracker; the
  register row links out to it.

## Edge cases

- The register row cites a mitigation issue in another repository, and its
  state is unclear.

  Do not guess. Report what the row claims, say that you could not confirm the
  mitigation shipped, and leave the row's status unchanged until the user
  confirms.

- Several unrelated risks changed at once, eg. at a scheduled review.

  A single `latest/register/<slug>` branch MAY carry them, provided every row in
  it is true at merge time. Split the change where one row's update depends on
  work that has not yet shipped, so the rest is not held up.
