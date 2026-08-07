# Risk rating

After identifying a threat, rate it by its **likelihood** of occurrence and the
**impact** of a successful attack. Combining the two gives an overall
**severity**, which is what orders the register and decides what gets attention
first.

You may design your own scoring scheme. The important thing is to apply it
_consistently_ – the goal is to rank risks against each other, not to achieve
false precision on any one. The scheme below is a common default, aligned with
[TS-54 §6](https://github.com/kieranpotts/standards/tree/latest/dev/src/054) and the
register's fields.

## Likelihood (probability)

How likely is the threat to be realized?

| Level | Meaning |
| ----- | ------- |
| Probable | Expected to occur; little or nothing stands in the way. |
| Likely | More likely than not over the system's lifetime. |
| Possible | Could occur; requires some conditions to align. |
| Unlikely | Would require unusual circumstances or effort. |
| Rare | Theoretically possible but highly improbable. |

## Impact

How bad is a successful attack – across financial, reputational, regulatory, and
operational dimensions?

| Level | Meaning |
| ----- | ------- |
| Catastrophic | Existential or systemic harm; regulatory breach; mass data loss. |
| Critical | Severe harm to users or business; significant breach. |
| Severe | Meaningful harm; recoverable but costly. |
| Marginal | Limited, contained harm. |
| Negligible | Minimal or no material harm. |

## Severity

Combine likelihood and impact into a single severity. A simple matrix works
well: the higher of the two axes tends to dominate, and a high–high combination
is always Critical.

| Severity | Interpretation |
| -------- | -------------- |
| Critical | Address immediately; do not ship without a mitigation or an explicit, signed-off acceptance. |
| High | Address soon; plan mitigation into the current or next cycle. |
| Medium | Track and mitigate opportunistically. |
| Low | Accept, monitor, or fix if cheap. |

Record the likelihood, impact, and resulting severity in both the session
report's threat assessment table and – for risks worth tracking – the
[register](../risks/REGISTER.md).

## Residual risk

After a mitigation is applied, re-rate the risk to get its **residual risk** –
what remains despite the countermeasures. A mitigation rarely reduces a risk to
zero. The residual risk is what you are actually choosing to live with, so it,
too, belongs in the register and should be reviewed on the same cadence as the
risk itself.
