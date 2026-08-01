# Agent skills for managing the risk register and threat modeling workshop reports

The skills available to agents in this project are:

- **[scaffold-report](./scaffold-report/):** \
  Cuts a `report/<slug>` branch from `main`, prepares a fresh report from the
  template, and opens a pull request in a draft state.

- **[complete-report](./complete-report/):** \
  Checks the workshop report and merges it into the `main` trunk.

- **[update-register](./update-register/):** \
  Updates the living risk register in place — mitigations shipped, reviews
  due, or risks retired — without a workshop.

The **scaffold-report** skill prepares a new, blank workshop report as a
draft PR. After this step, the user runs the threat modeling workshop and
writes up its findings. When the report is done, the **complete-report**
skill can be used to get an agent to check it over and land the report in
the `main` trunk. Independently of any workshop, **update-register** keeps
the living risk register current — marking mitigations shipped, refreshing
reviews, or retiring risks that no longer apply.

```mermaid
flowchart LR
  scaffold["🤖<br/><b>scaffold-report</b>"]:::agentic
  workshop["🧑<br/>threat modeling workshop"]:::anthropic
  complete["🤖<br/><b>complete-report</b>"]:::agentic
  update["🤖<br/><b>update-register</b>"]:::agentic

  scaffold ==> workshop
  workshop ==> complete
  complete -.-> update

  classDef agentic fill:#cce5ff,stroke:#004085,color:#004085,stroke-width:2px
  classDef scripted fill:#e2e3e5,stroke:#4b5157,color:#383d41,stroke-width:2px
  classDef anthropic fill:#fff3cd,stroke:#856404,color:#856404,stroke-width:2px,stroke-dasharray:2 3
```

These skills handle process, not substance: how a workshop report is
scaffolded, reviewed, and landed in `main`. For the threat modeling itself —
running the workshop and identifying the threats — use the
[**probe**](https://github.com/kieranpotts/skills/tree/latest/dev/skills/probe)
skill in my global skills collection.

## Compatibility

These skills are compatible with the [Agent Skills](https://agentskills.io/)
convention. Most agent harnesses support this convention natively, but
workarounds may be required for harnesses that do not.
