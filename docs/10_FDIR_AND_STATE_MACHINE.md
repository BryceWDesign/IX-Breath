# IX-Breath FDIR and State-Machine Architecture

**Document ID:** IXB-FDIR-10  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent documents:**  
- `docs/00_PROJECT_CHARTER.md`
- `docs/01_MISSION_REQUIREMENTS.md`
- `docs/02_OPERATING_ASSUMPTIONS.md`
- `docs/03_MASTER_ARCHITECTURE.md`
- `docs/04_DONOR_FEATURE_FILTER.md`
- `docs/05_PACKAGING_ENVELOPE.md`
- `docs/06_REAL_WORLD_BOM.md`
- `docs/07_POWER_CONTINUITY_SPINE.md`
- `docs/08_LIFE_SUPPORT_CORE.md`
- `docs/09_THERMAL_RESOURCE_COUPLING.md`

---

## 1. Purpose

This document defines the IX-Breath **fault detection, isolation, and recovery (FDIR)** architecture and the system-level state machine.

Its job is to answer the questions that determine whether IX-Breath reads like real aerospace work or a concept sketch:

1. How does the pack classify faults?
2. Which functions are allowed to keep running after a fault?
3. Which state transitions are automatic, which are manual, and which are forbidden?
4. When does the pack preserve capability, and when does it simplify aggressively?
5. How does it prevent false returns to normal?
6. How does it behave when sensors disagree or confidence collapses?
7. How does it preserve life-support priority under confusion?

This document is intentionally biased toward clarity over cleverness.

That is not a limitation.
That is the safety posture.

---

## 2. FDIR design posture

IX-Breath uses one central FDIR rule:

> **When the pack is uncertain, it should become more conservative, more explicit, and easier to understand.**

That means IX-Breath does **not** treat autonomy as a prestige feature.

The control system is not trying to look futuristic.
It is trying to avoid killing the astronaut through ambiguity.

So the FDIR posture is:

- detect faults early when possible,
- isolate what can be isolated,
- preserve life-critical continuity first,
- degrade in bounded steps,
- latch hazardous states when auto-recovery would be unsafe,
- and keep the crew aware of what posture the system is actually in.

---

## 3. Architecture statement

**IX-Breath uses a layered FDIR architecture with a primary supervisory controller, an independent watchdog/safety supervisor, latched major-fault behavior, bounded operating states, resource-gated extension logic, and explicit crew-visible transitions.**

In plain English:

- one controller runs the normal system logic,
- one separate safety layer watches for trouble,
- certain faults can be isolated and survived,
- certain states can never quietly auto-clear,
- and the pack must always choose life-support clarity over feature richness.

---

## 4. Top-level FDIR roles

The IX-Breath FDIR architecture is split into four roles.

| Role ID | Role | Purpose |
|---|---|---|
| FDIR-R1 | Primary Supervisory Control | executes nominal state logic, mode management, resource gating, and command outputs |
| FDIR-R2 | Independent Watchdog / Safety Supervisor | detects stalled, invalid, or unsafe supervisory behavior and forces conservative posture |
| FDIR-R3 | Local Protection Functions | local hardware or subsystem protections that do not depend entirely on software |
| FDIR-R4 | Crew Interface and Acknowledgement Layer | exposes system posture, receives bounded manual commands, and preserves human awareness |

These four roles are not equal.
That is intentional.

If the rich controller gets confused, the safety layer must still have teeth.

---

## 5. State-machine statement

IX-Breath uses six top-level system states:

1. **Nominal**
2. **Conserve**
3. **Rescue**
4. **Critical**
5. **Safe-Hold**
6. **Service**

These states are not decorative labels.
They are **contract states**.

Each state defines:

- what the pack is trying to do,
- what loads may remain powered,
- what oxygen policy applies,
- what regeneration policy applies,
- what kinds of transitions are allowed,
- and what level of uncertainty is acceptable.

---

## 6. Why bounded states were chosen

The donor filter strongly favored:

- bounded state machines,
- emergency retreat / safe-hold logic,
- dual-channel veto,
- latched transitions,
- and auditable state changes.

That is exactly the right call for IX-Breath.

A survival pack should not behave like a self-optimizing gadget with fuzzy intentions.

It should behave like a disciplined aerospace system with a narrow vocabulary and traceable decisions.

---

## 7. Top-level state diagram

```mermaid
stateDiagram-v2
    [*] --> Service
    Service --> Nominal: pre-EVA qualification passed
    Nominal --> Conserve: caution resource/fault trigger
    Nominal --> Rescue: crew command or delayed-ingress trigger
    Nominal --> Critical: major survival threat
    Nominal --> Safe-Hold: hazardous ambiguity or unsafe fault

    Conserve --> Nominal: only if fully cleared and safe
    Conserve --> Rescue: rescue objective asserted
    Conserve --> Critical: worsening survival/resource condition
    Conserve --> Safe-Hold: unsafe ambiguity or invalid recovery

    Rescue --> Conserve: only if rescue objective no longer dominant and state is clean
    Rescue --> Critical: worsening survival margin or major subsystem loss
    Rescue --> Safe-Hold: control ambiguity, major fault, or invalid state confidence

    Critical --> Safe-Hold: unsafe to continue dynamic control
    Critical --> Rescue: only by explicit guarded recovery conditions
    Critical --> Conserve: normally disallowed
    Critical --> Nominal: disallowed

    Safe-Hold --> Service: post-event controlled service entry
    Safe-Hold --> Rescue: guarded exceptional recovery only
    Safe-Hold --> Nominal: disallowed
    Safe-Hold --> Conserve: disallowed
    Safe-Hold --> Critical: possible if new hazard emerges while latched

    Service --> Service: maintenance / diagnostics / recharge

8. State hierarchy and seriousness

Not all states are peers.

IX-Breath organizes them by control freedom.

State	Freedom level	Safety posture
Nominal	highest	full bounded functionality, strong margins expected
Conserve	reduced	margin preservation takes priority over convenience
Rescue	reduced and survival-biased	survivability extension becomes primary objective
Critical	very low	only essential survival actions remain justified
Safe-Hold	minimal and latched	prevent dangerous ambiguity and false recovery
Service	non-EVA controlled	inspection, recharge, calibration, maintenance only

The key point is this:

moving downward in seriousness is easy; moving upward requires evidence.

9. State definitions
9.1 Nominal
Purpose

Support normal closed-loop life-support operation with strong margins and full bounded functionality.

Allowed posture

B1 and B2 fully active

B3 available when justified

B4 available

primary oxygen path active

reserve oxygen isolated

regeneration typically off or tightly limited

rescue solar normally stowed or ignored

Expected confidence

High confidence in sensing, control, power, and thermal posture.

Exit triggers

growing resource concern,

delayed-ingress concern,

explicit rescue condition,

significant subsystem degradation,

unsafe ambiguity.

9.2 Conserve
Purpose

Preserve survivability margin before the pack is in deep trouble.

Allowed posture

B4 reduced or partially shed

B3 scrutinized aggressively

primary oxygen preservation policy tightened

reserve oxygen remains isolated

regeneration usually held, denied, or tightly bounded

thermal ladder may enter caution or constrained levels

Expected confidence

Moderate-to-high confidence, but one or more margins are no longer comfortable.

Why this state exists

Conserve is what keeps IX-Breath from having only two moods:
fine and dying.

9.3 Rescue
Purpose

Shift system logic from routine support to survival extension while awaiting rescue or delayed ingress.

Allowed posture

B4 largely off

B3 available only when the full affordability gate passes

rescue solar may be accepted if valid

primary oxygen preserved more aggressively

reserve oxygen still isolated unless separate trigger logic says otherwise

regeneration may be allowed, but only under gate approval

Expected confidence

Good enough to run survival extension logic, but not necessarily good enough for broad nominal behavior.

Important note

Rescue does not mean panic mode.
It means survivability-first mode.

9.4 Critical
Purpose

Preserve only what is needed for immediate survival when margin loss or subsystem failure is serious.

Allowed posture

B1 and B2 defended as long as physically possible

B3 typically cut or heavily limited

regeneration usually denied

thermal ladder often severe

reserve oxygen criteria may be approaching or active depending on fault context

crew alerts persistent and simple

Expected confidence

Confidence may be degraded, but the pack still has a coherent survival posture.

Important rule

Critical is not allowed to auto-return to Nominal.

That would be reckless.

9.5 Safe-Hold
Purpose

Enter a latched conservative posture when continued dynamic control would be unsafe, ambiguous, or untrustworthy.

Allowed posture

only minimum survivability functions remain if safe

dynamic extension behavior is denied

regeneration denied

auto-optimization denied

ambiguous auto-recovery blocked

fault latches preserved

crew informed clearly that the pack is in degraded latched posture

Why this state exists

Safe-Hold is what prevents the pack from acting healthy just because one signal flickered back into range.

Important rule

Safe-Hold is not a convenience pause.
It is a protected boundary.

9.6 Service
Purpose

Support maintenance, recharge, calibration, log extraction, checkout, and readiness determination outside EVA operations.

Allowed posture

controlled external power or battery-supported diagnostics

non-EVA tests

calibration procedures

component isolation and inspection

no survival claims active

no EVA-mode regeneration claims active

Important rule

Service is the only normal entry point to Nominal.
That preserves readiness discipline.

10. FDIR input families

The state machine cannot behave intelligently unless its inputs are classified clearly.

IX-Breath groups inputs into six families.

Input family	Examples
IF-1 Life-support condition	O2 state, CO2 state, loop pressure, blower viability, gas-routing status
IF-2 Power condition	battery state, bus health, branch faults, converter status, source validity
IF-3 Thermal condition	hot-zone temperatures, thermal ladder state, rejection capacity status
IF-4 Water/regeneration condition	water class, PEM health, hydrogen vent path, affordability gate
IF-5 Control integrity	watchdog status, controller heartbeat, sensor disagreement, data plausibility
IF-6 Human/mission context	crew rescue command, acknowledgement input, delayed-ingress condition, service authorization

These families matter because not all bad inputs deserve the same reaction.

11. Fault classes

To make FDIR serious, IX-Breath defines explicit fault classes.

Fault class	Meaning	Examples
FC-1 Advisory degradation	reduced margin, but no immediate survival threat	partial telemetry loss, elevated but manageable thermal trend
FC-2 Resource or performance degradation	survivability margin is shrinking	battery decline, constrained thermal margin, B3 affordability loss
FC-3 Redundant path loss	one path failed, backup remains	one blower lost, one sensor path disqualified
FC-4 Major subsystem loss	essential function compromised or near compromise	primary oxygen path loss, CO2 control degraded seriously
FC-5 Control ambiguity	pack cannot safely trust its own command posture	watchdog mismatch, inconsistent state estimation, unsafe controller behavior
FC-6 Structural/containment concern	leak, routing breach, or packaging-zone threat	gas leak, water leak into control bay, electrical bay hazard
FC-7 Protected reserve event	reserve oxygen continuity is implicated	reserve-path actuation failure, reserve path required unexpectedly
11.1 Fault-class rule

Not every fault forces Safe-Hold.
But control ambiguity and certain unsafe combinations do.

12. Fault detection posture
12.1 Detection methods

IX-Breath allows detection by:

threshold crossing,

rate-of-change abnormality,

redundancy disagreement,

cross-domain inconsistency,

hardware self-check / built-in-test,

watchdog timeout,

invalid transition attempt,

impossible resource accounting result.

12.2 Detection philosophy

The pack is not allowed to detect faults only when numbers go out of range.

It must also notice when the story stops making sense.

Example:
if solar input appears valid, but power margin does not improve while thermal burden rises, that is not “normal enough.”
That is a cross-domain anomaly.

13. Confidence model

IX-Breath explicitly tracks confidence, not just raw readings.

13.1 Confidence bands
Band	Meaning
C0	high confidence
C1	reduced confidence but still operationally useful
C2	degraded confidence, conservative action required
C3	unsafe confidence, dynamic optimization forbidden
13.2 Why this matters

Two systems can both read “oxygen okay” and still deserve different decisions if:

one reading comes from a healthy voted sensor set,

and the other comes from one surviving suspect channel.

13.3 Confidence rule

When confidence falls:

state freedom falls,

aggressive optimization falls,

regeneration permission tightens,

and Safe-Hold becomes more likely.

14. Major FDIR actions

The FDIR layer uses a bounded action vocabulary.

Action ID	Action	Meaning
A1	Alert	inform crew of significant but not yet system-redefining condition
A2	Derate	reduce power/load/behavior intensity while keeping current state
A3	Shed	remove lower-priority loads or functions
A4	Isolate	cut off a branch, source, path, or subsystem
A5	Reconfigure	switch to alternate viable path
A6	Deny	refuse a requested function such as regeneration or solar acceptance
A7	Latch	preserve degraded posture until explicit guarded clear
A8	Escalate	force transition into more conservative state
A9	Preserve	hold protected core functions regardless of lower-tier losses

These are simple on purpose.
Simple action vocabularies are easier to validate.

15. Command authority hierarchy

Who gets the last word?

IX-Breath defines that clearly.

Priority	Authority	Scope
1	Local hard protection	battery self-protect, branch protection, hard fault latches
2	Independent watchdog / safety supervisor	force conservative posture, deny unsafe continuation
3	Primary supervisory controller	normal state logic, resource gating, reconfiguration
4	Crew bounded manual commands	rescue entry, acknowledgement, limited guarded commands
5	Service-side commands	only in Service or authorized maintenance contexts
15.1 Important rule

Crew commands matter, but they are not allowed to override hard protections that would create an obviously unsafe electrical or life-support condition.

That is serious design, not disrespect to the operator.

16. Transition philosophy

IX-Breath uses three transition classes.

Transition class	Meaning
T-N	Normal transition
T-G	Guarded transition
T-F	Forbidden transition
16.1 Core principle

As the system becomes more degraded, transitions become:

fewer,

more guarded,

and more heavily latched.

That is deliberate.

17. Allowed transition posture
17.1 Service -> Nominal

Class: T-G
Allowed only when:

pre-EVA checks pass,

readiness evidence is complete,

no blocked major subsystem,

no unresolved major fault latch,

pack qualifies clean startup.

17.2 Nominal -> Conserve

Class: T-N
Allowed when:

resource margin declines,

advisory or resource-degradation faults persist,

mission context suggests delayed ingress risk,

proactive survival preservation is warranted.

17.3 Nominal -> Rescue

Class: T-N / T-G
Allowed when:

crew commands rescue,

delayed-ingress or separation logic triggers,

survival extension becomes the objective,

no contradiction exists that would force immediate Critical or Safe-Hold instead.

17.4 Nominal -> Critical

Class: T-G
Allowed when:

major subsystem loss,

major oxygen continuity threat,

serious pressure or CO2-control issue,

or life-support margin is directly threatened.

17.5 Nominal -> Safe-Hold

Class: T-G
Allowed when:

control ambiguity is unsafe,

controller/watchdog disagreement indicates hazardous command trust loss,

invalid state logic persists,

or multiple critical uncertainties make dynamic continuation unsafe.

17.6 Conserve -> Nominal

Class: T-G
Allowed only when:

the original caution trigger is resolved,

margins are re-established,

confidence is restored,

and no latched major fault remains uncleared.

17.7 Conserve -> Rescue

Class: T-N
Allowed when:

rescue objective becomes explicit,

delayed-ingress risk rises,

or crew commands rescue.

17.8 Conserve -> Critical

Class: T-G
Allowed when:

margin loss continues,

major function degrades,

or thermal/power/life-support posture crosses into survival-threat territory.

17.9 Conserve -> Safe-Hold

Class: T-G
Allowed when:

ambiguity becomes unsafe,

a major control conflict emerges,

or reconfiguration cannot be trusted.

17.10 Rescue -> Conserve

Class: T-G
Allowed only when:

rescue objective is no longer dominant,

margins and confidence are healthy,

solar/regen dependence has ended cleanly if used,

and no major latches block partial recovery.

17.11 Rescue -> Critical

Class: T-G
Allowed when:

survival margin worsens,

primary oxygen path degrades sharply,

thermal ladder reaches severe,

B1/B2 are at risk,

or regeneration/solar no longer help enough to justify continued rescue posture.

17.12 Rescue -> Safe-Hold

Class: T-G
Allowed when:

the pack can no longer trust its own dynamic control,

repeated conflicting commands or impossible resource states appear,

or extension behavior becomes unsafe to continue.

17.13 Critical -> Rescue

Class: T-G (exceptional)
Allowed only when:

life-support core stabilizes,

confidence improves materially,

major contradictions are cleared,

and the safety supervisor agrees the pack can re-enter bounded dynamic survival mode.

17.14 Critical -> Conserve

Class: T-F by default
Reason:
The architecture does not skip from severe survival threat into a mild-margin-preservation posture without passing through a stronger guarded recovery argument.

17.15 Critical -> Nominal

Class: T-F
Reason:
Too much hidden risk.

17.16 Critical -> Safe-Hold

Class: T-N / T-G
Allowed when:

continued dynamic survival control is unsafe,

or the watchdog/safety layer forces a latched protected posture.

17.17 Safe-Hold -> Rescue

Class: T-G (rare)
Allowed only when:

a human or supervised recovery process has re-established minimum trust,

the fault cause is bounded,

no local hard protection blocks the transition,

and the system can justify a simpler survival mode better than staying latched.

17.18 Safe-Hold -> Nominal

Class: T-F
Reason:
Unsafe optimism.

17.19 Safe-Hold -> Conserve

Class: T-F
Reason:
If the system was unsafe enough to latch, it does not get to pretend everything is merely “conservative” again without controlled service-side evaluation.

17.20 Safe-Hold -> Service

Class: T-G
Allowed when:

EVA operations are over or terminated,

controlled maintenance access is established,

logs and latches are preserved for review.

18. Latching philosophy
18.1 Why latches exist

Latches exist because some faults do not become safe merely because the signal looks better for a moment.

18.2 Latch categories
Latch category	Example use
L1 Informational latch	preserve anomaly history
L2 Operational latch	hold degraded function until explicit clear
L3 Safety latch	prevent unsafe return to richer operation
L4 Protected-state latch	keep Safe-Hold or major-fault posture active until controlled resolution
18.3 Latch rule

The following conditions should normally produce at least an L2 or stronger latch:

control ambiguity,

watchdog conflict,

reserve-oxygen event,

major bus fault affecting B1/B2,

serious sensor-confidence collapse,

gas leak classification,

major failed reconfiguration attempt.

19. Watchdog / safety-supervisor posture
19.1 Role

The watchdog layer exists to answer one ugly question:

What if the main controller is alive enough to command things, but not sane enough to be trusted?

That is why IX-Breath keeps the safety layer separate.

19.2 Minimum watchdog observations

The watchdog should monitor at minimum:

supervisory heartbeat,

state-transition validity,

contradictory bus commands,

contradictory oxygen-path commands,

impossible resource-gate results,

control loop stall or runaway behavior,

invalid sensor-confidence handling,

repeated transition thrash.

19.3 Watchdog authority

The watchdog may:

deny certain transitions,

force Conserve,

force Critical,

force Safe-Hold,

and preserve event logging if possible.

19.4 Important rule

A disagreement between the supervisor and watchdog on a major safety question is not treated as a tie.

It is treated as a loss of confidence.

That usually pushes the pack downward in state freedom.

20. Sensor disagreement handling
20.1 Why this matters

IX-Breath is not allowed to pretend that bad sensors are fine so long as one number remains pretty.

20.2 Disagreement posture

When a sensor set disagrees, the system should:

identify the disagreement,

cross-check with remaining channels and related variables,

downgrade confidence,

deny or reduce extension behavior if required,

tell the crew confidence is degraded if it affects survival decisions.

20.3 Examples

O2 sensor disagreement may still permit breathing support, but may deny regeneration and tighten oxygen policy.

CO2 sensor disagreement may bias toward conservative CO2-control posture and stronger alerts.

pressure-sensor disagreement may elevate leak concern and push toward simpler routing.

20.4 Rule

Sensor disagreement is not automatically catastrophic.
But it is never invisible.

21. Reconfiguration philosophy
21.1 Goal

Reconfiguration exists to preserve survivability after partial loss.

21.2 What may be reconfigured

The architecture may reconfigure:

blower path selection,

valve/routing posture,

bus participation,

regeneration enable/deny,

solar acceptance/rejection,

alert posture,

logging richness.

21.3 What may not be reconfigured casually

The architecture may not casually reconfigure:

reserve oxygen integrity,

local hard protections,

safety latches,

readiness evidence history,

or fundamental state restrictions.

21.4 Rule

Reconfiguration is a tool, not a religion.

If a reconfiguration increases ambiguity, it is the wrong move.

22. Reserve oxygen FDIR posture
22.1 Why reserve oxygen gets special treatment

Reserve oxygen is the last-line continuity layer.
It cannot be treated like an ordinary managed resource.

22.2 Reserve-related trigger families

Reserve behavior must consider:

primary-path availability,

loop oxygen state,

loop pressure trend,

control confidence,

crew emergency command,

whether reserve actuation hardware is healthy,

whether dynamic extension logic is still credible.

22.3 Reserve event classes
Event	Meaning
RO-1	reserve isolated and healthy
RO-2	reserve armed / close to possible use
RO-3	reserve active
RO-4	reserve path fault or actuation problem
RO-5	reserve accounting uncertainty
22.4 Rule

Any transition into RO-3 or RO-4 should normally produce a major operational latch.

That is not a routine event.

23. Regeneration FDIR posture
23.1 Why regeneration needs its own FDIR stance

The PEM path is useful only when conditions support it.
It should fail politely and be denied aggressively when conditions do not.

23.2 Regeneration-specific faults
Fault	Meaning
RG-F1	water unavailable or unqualified
RG-F2	thermal margin insufficient
RG-F3	power margin insufficient
RG-F4	hydrogen vent path suspect
RG-F5	PEM hardware fault
RG-F6	control-confidence too low for safe permission
23.3 Regeneration response posture

In general:

deny first,

isolate if necessary,

preserve core life support,

and never let failed regeneration poison B1/B2 continuity.

23.4 Rule

A denied regeneration decision is a normal successful safety action, not a “system failure.”

That point stays locked.

24. Power-state and system-state coupling

The state machine and the bus map are tightly linked.

24.1 Coupling rules
System state	Expected bus posture
Nominal	B1/B2/B3/B4 all potentially available within resource limits
Conserve	B4 reduced first, B3 more tightly controlled
Rescue	B4 mostly off, B3 only when resource gate passes
Critical	B3 usually off, B1/B2 protected
Safe-Hold	only minimum bounded bus posture preserved
Service	controlled service power and diagnostics posture
24.2 Rule

No software state is valid if the bus posture needed to support it does not actually exist.

This prevents fake “Nominal” behavior on crippled power.

25. Thermal-state and system-state coupling

The thermal ladder also couples directly into state logic.

Thermal level	Expected state pressure
TD-0	no forced state pressure
TD-1	may push Nominal to Conserve
TD-2	strongly pressures Conserve/Rescue behavior
TD-3	pressures Critical and denies extension
TD-4	survival-only bias, Safe-Hold risk rises sharply
25.1 Rule

A severe thermal posture can overrule extension logic even when power and water would otherwise support it.

Thermal truth wins.

26. Crew interface posture
26.1 What the crew must know

The crew should be able to know, at minimum:

current top-level state,

whether reserve oxygen is isolated or active,

whether regeneration is denied, limited, or active,

whether the pack is thermally constrained,

whether the pack is in a latched degraded posture.

26.2 What the crew may command

The crew may have bounded authority to:

acknowledge alerts,

command Rescue entry,

request guarded emergency behavior,

invoke certain isolation behaviors,

enter Service only under appropriate conditions.

26.3 What the crew should not have to do

The crew should not be asked to:

debug voting logic,

manually balance bus tiers,

decode hidden confidence models,

or interpret ambiguous “smart” automation.

26.4 Rule

If the UI requires explanation under stress, it is too clever.

27. Logging and evidence posture
27.1 Why logging matters

A survival system that cannot explain what it did is not serious enough for sustainment.

27.2 Logged event families

The FDIR layer should log at minimum:

state entries and exits,

latch assertions and clears,

major fault detections,

reserve-oxygen events,

regeneration allow/deny changes,

solar acceptance/rejection events,

branch isolation events,

watchdog interventions,

service entry and exit,

confidence drops below major thresholds.

27.3 Logging rule

Logs should persist even when rich UI and noncritical telemetry are gone, to the extent physically practical.

28. Transition-thrash prevention
28.1 Why this matters

A state machine that bounces between modes is dangerous and confusing.

28.2 Anti-thrash posture

The architecture should use:

hysteresis,

dwell requirements where appropriate,

latching for major hazards,

guarded recovery criteria,

and explicit confidence restoration before re-enabling richer states.

28.3 Rule

No state should repeatedly enter and exit based on one noisy sensor without additional logic that proves the transition is real.

29. Recovery philosophy
29.1 Recovery is not optimism

IX-Breath only credits recovery when:

the root problem is bounded,

confidence is restored,

bus posture supports the richer state,

thermal posture supports it,

and no major latches forbid it.

29.2 Recovery ladder

In general, recovery should follow:

preserve survival,

stabilize,

verify,

then cautiously regain capability.

It should not jump straight from hazard to normal.

30. Example state-driving scenarios
30.1 Scenario A: resource decline without major fault

battery margin falls

thermal state still healthy

no major hardware loss

delayed ingress is becoming likely

Expected posture: Nominal -> Conserve, then possibly Rescue if mission context justifies.

30.2 Scenario B: sunlit separation with healthy core

crew separated

life-support healthy

power and water moderate

solar valid

full gate passes for limited regeneration

Expected posture: Nominal or Conserve -> Rescue, with limited or enabled regeneration allowed.

30.3 Scenario C: primary oxygen-path fault with healthy reserve

major primary-path issue

reserve healthy

confidence acceptable

dynamic control still coherent

Expected posture: Rescue or Critical depending on severity, with reserve policy engaged under guarded logic.

30.4 Scenario D: controller/watchdog disagreement

main controller commands behavior the watchdog considers invalid

confidence in dynamic control drops sharply

Expected posture: immediate escalation, typically toward Safe-Hold or at least Critical with major latches.

30.5 Scenario E: hot PEM plus weak battery in mixed light

solar intermittent

thermal ladder rising

B3 expensive

core still stable

Expected posture: deny regeneration, likely Rescue or Conserve depending on mission context, never force PEM just because sunlight flickers on.

31. Verification posture

This document does not claim IX-Breath has already proven its FDIR logic in hardware.

It defines what later seriousness requires.

31.1 Future verification themes

A serious validation path should eventually test:

valid and invalid state transitions,

watchdog intervention on bad controller behavior,

sensor disagreement and confidence degradation,

reserve-oxygen trigger logic,

denial of regeneration under weak thermal/power conditions,

anti-thrash behavior,

bus/posture consistency under faults,

latch behavior and controlled clearing,

Safe-Hold entry and guarded exit behavior,

crew interface clarity during degraded operation.

31.2 Seriousness rule

If later testing only proves that the state machine works when everything is healthy, it has not tested the thing that matters.

It must be challenged under confusion.

32. Locked design decisions from this document

The following decisions are now locked unless later evidence forces revision:

IX-Breath uses six top-level bounded states: Nominal, Conserve, Rescue, Critical, Safe-Hold, Service.

Major hazardous states may latch and must not auto-clear casually.

The independent watchdog/safety supervisor has real authority.

Control ambiguity is treated as a safety problem, not a cosmetic software bug.

Confidence degradation reduces state freedom and extension behavior.

Safe-Hold is a protected latched boundary, not a decorative pause state.

Recovery into richer states requires evidence, not hope.

Reserve-oxygen events are major operational events and should normally latch.

Denied regeneration is a valid protective action.

The crew must see a clear, bounded posture under stress.

33. What this document deliberately does not claim

This document does not claim:

final firmware implementation,

final watchdog hardware topology,

final heartbeat timings,

final debounce/hysteresis values,

final reserve trigger thresholds,

final alert phrasing,

final controller processor selection.

Those are downstream details.
