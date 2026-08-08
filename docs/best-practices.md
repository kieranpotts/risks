# Best practices

Practical guidance for running a good threat modeling session and keeping the
register honest.

## Model early, and iterate

The cheapest vulnerability to fix is the one caught in the design phase. Begin
threat modeling during design, and repeat at the break point of each development
cycle, rather than postponing a single big session until just before a release.
Security and privacy are cross-cutting concerns – expensive to retrofit into an
established architecture.

## Bring diverse perspectives

A session run by one person sees the system one way. Draw participants from
architecture and security, product and business, and development, testing, and
operations. Different viewpoints reveal different attack vectors. The security
champion facilitates; everyone else plays security analyst.

## Use more than one model

Feed the session several representations of the system – infrastructure, data
flows (at rest and in transit), services and components, component boundaries and
interfaces, business logic, and external dependencies. Each model surfaces a
different category of threat.

## Classify with a named framework

STRIDE is the recommended minimum. Add LINDDUN for systems handling personal
data (especially under GDPR or CCPA), and OWASP categories for web-facing
systems. A consistent classification makes the register searchable and the
coverage auditable. An unclassified threat is an unfinished one.

## Rate consistently, not precisely

The point of a likelihood × impact rating is to _rank_ risks against each other,
so the worst get attention first. Consistency across the register matters far
more than the illusion of precision in any single score. Pick one scheme (see
[risk-rating.md](./risk-rating.md)) and apply it the same way every time.

## "Accept the risk" is a valid outcome

Not every threat warrants a mitigation. If the cost of mitigating exceeds the
cost of the risk, record an explicit, reasoned decision to accept it, with the
residual risk stated. A register where every risk is "mitigation pending" is not
being honest about trade-offs.

## The register is the single source of truth

A risk's current status lives in exactly one place: its register row. Do not
scatter status across workshop reports – those are frozen. When a mitigation
ships or a review happens, update the row in place with
[`/update-register`](../.agents/skills/update-register/).

## Don't overstate the posture

Merge a register change that lowers a risk – "mitigated", residual risk down –
only once the mitigation is genuinely in production. A register that claims
protection the system doesn't yet have is worse than one that admits the gap.

## Retire, don't delete

When a risk no longer applies, move it to the register's "Retired risks" section
with a closing note. Reference numbers are stable and rows are never deleted, so
the register keeps a full account of everything that was ever tracked.

## Keep workshop reports immutable

Do not edit a merged workshop report as the system changes underneath it. To
reassess, hold a new session. The [`INDEX`](../risks/INDEX.md) is a trail of when
the system was examined and what was found each time, not a single living
verdict – that is the register's job.
