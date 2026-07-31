# Agent skills for managing the risk register and threat modeling workshop reports

The skills available to agents in this project are:

- **[scaffold-report](./scaffold-report/):** \
  Scaffolds a report for a threat modeling workshop, ready for the user to write up.

- **[complete-report](./complete-report/):** \
  Lands a threat modeling session's report in the `main` trunk.

- **[update-register](./update-register/):** \
  For _ad hoc_ updates to the risk register.

The **scaffold-report** skill....

```mermaid
flowchart LR
  scaffold["🤖<br/>scaffold report"]:::agentic
  workshop["🧑<br/>threat modeling workshop"]:::anthropic
  complete["🤖<br/>complete report"]:::agentic
  update["🤖<br/>update register"]:::agentic

  scaffold ==> workshop
  workshop ==> complete
  complete -.-> update

  classDef agentic fill:#cce5ff,stroke:#004085,color:#004085,stroke-width:2px
  classDef scripted fill:#e2e3e5,stroke:#4b5157,color:#383d41,stroke-width:2px
  classDef anthropic fill:#fff3cd,stroke:#856404,color:#856404,stroke-width:2px,stroke-dasharray:2 3
```

## Compatibility

These skills are compatible with the [Agent Skills](https://agentskills.io/)
convention. Most agent harnesses support this convention natively, but
workarounds may be required for harnesses that do not.
