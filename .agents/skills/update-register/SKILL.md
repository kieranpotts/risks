---
name: update-register
description: Update the living risk register between sessions — change a tracked risk's mitigation status, severity, residual risk, or review date in place, or promote an audit finding into a tracked risk, or retire a risk that no longer applies. Use when the user says "update the register", "mark risk TA1 as mitigated", "the MFA mitigation shipped", "review the register", or "retire this risk". Do not use this skill to run a new threat model — use draft-session for that.
license: MIT
metadata:
  interactive: yes
---

# Update register

Use this skill to keep the living [`risks/REGISTER.md`](../../../risks/REGISTER.md) current between sessions — updating a tracked risk's status in place as it evolves. Unlike a session report, the register is living documentation: it MUST reflect the current state of the system's security posture at all times.

Do NOT use this skill to run a new threat modeling session — use [`/draft-session`](../draft-session/SKILL.md). Do NOT edit any merged session report; those are immutable.

## When to use

- A mitigation has shipped (or progressed), so a risk's status, severity, or residual risk changes.
- A scheduled review is due — refresh the `Reviewed` date and re-confirm the ratings.
- A risk no longer applies (designed out, component removed, fully mitigated) — retire it.
- A finding from an [audit](https://github.com/kieranpotts/audits) is worth tracking over time — promote it into a new register row.

## Instructions

1.  **Create the branch.**

    ```sh
    git checkout main
    git pull
    git checkout -b register/<slug>
    ```

    Use a slug that names the change, eg. `register/mfa-rollout` or `register/q3-review`.

2.  **Update the affected rows in place.**

    Edit [`risks/REGISTER.md`](../../../risks/REGISTER.md) only. Update the mitigation status, severity, residual risk, and `Reviewed` date of the affected rows. To promote an audit finding, add a new row with a fresh reference number. To retire a risk, move its row to the "Retired risks" section with a closing note — never delete it.

3.  **Commit and open a pull request.**

    ```sh
    git add risks/REGISTER.md
    git commit -m "register: <short lowercase description>"
    git push -u origin register/<slug>
    gh pr create --title "register: <short lowercase description>" --fill
    ```

4.  **Merge when the change is real.**

    Merge only once the change the update reflects is _actually true_ — eg. once a mitigation has shipped to production — so the register never overstates the system's security posture. Squash-merge with a `register: <description>` message and delete the branch.

## Rules

-   **Never touch a merged session report.**

    Session reports are immutable point-in-time snapshots. All ongoing status lives in the register.

-   **Update in place; never delete a row.**

    Reference numbers are stable. Retire a risk by moving it to the "Retired risks" section with a reason, so the register keeps a full account of what was ever tracked.

-   **Don't overstate the posture.**

    Do not mark a risk mitigated, or lower its residual risk, until the mitigation is genuinely in place. Merge the register change alongside — or after — the real-world change it describes.

-   **A reassessment that finds a _new_ threat is a session, not an update.**

    If the change amounts to fresh threat identification rather than status-tracking of a known risk, use [`/draft-session`](../draft-session/SKILL.md) instead.

## Success criteria

- Branch `register/<slug>` exists, and only `risks/REGISTER.md` is changed.

- The affected rows reflect the current, true status of each risk, with an updated `Reviewed` date.

- A pull request titled `register: <short lowercase description>` is open (or merged, once the change is real).

## References

- [`AGENTS.md`](../../../AGENTS.md): The register workflow and rules.

- [`risks/REGISTER.md`](../../../risks/REGISTER.md): The living register and its fields.

- [`draft-session`](../draft-session/): Runs a new threat model, which seeds the register in the first place.
