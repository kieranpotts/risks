# Risks

This directory holds the risk register and reports from threat modeling
workshops for [Project Name].

The workshop reports capture the system context assessed, the threat modeling
frameworks applied (eg. STRIDE), and the threats identified in the workshop.
The findings from workshops feed the risk register, which tracks the status of
current identified security and privacy risks.

## Structure

```
risks/
├── REGISTER.md         # The living register of tracked risks. Updated in place.
├── INDEX.md            # The catalog of merged reports, newest first.
├── TEMPLATE.md         # Template for a new workshop report.
└── YYYY-MM-DD-<slug>/  # One workshop report, dated by when it was held.
    ├── README.md       # The main entry point to the workshop report.
    └── …               # Diagrams, data-flow models, or other evidence.
```

## Workflow

1.  Create a `latest/report/<slug>` branch.

2.  Copy the [template](./TEMPLATE.md) to `YYYY-MM-DD-<slug>/README.md`,
    where `YYYY-MM-DD` is the date of the threat modeling workshop, and `<slug>`
    is a short hyphen-delimited description of the target software component,
    subsystem, or service that is the subject of the risk assessment.

3.  Open a draft PR.

4.  Undertake the threat modeling workshop.

5.  Write up the report. Add supporting artifacts to the
    `YYYY-MM-DD-<slug>/` directory, referenced from the `README.md`.

6.  Add the report to the index in the `INDEX.md` file.

7.  Update the risk register (`REGISTER.md`) with details of new and changed
    risks identified in the report.

8.  Open the PR for comments.

9.  Resolve comments and merge into `latest/main`.
