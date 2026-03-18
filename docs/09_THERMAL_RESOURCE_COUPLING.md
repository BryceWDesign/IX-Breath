# IX-Breath Thermal Survivability and Resource Coupling

**Document ID:** IXB-THERM-09  
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

---

## 1. Purpose

This document defines the detailed architecture for:

- **S5 Thermal Survivability Layer**
- rescue-solar interaction with the power spine
- the coupling logic between **water**, **oxygen regeneration**, **electrical power**, and **thermal margin**

Its job is to kill a lazy design habit:

> pretending oxygen regeneration is useful just because the electrolyzer exists.

It is not useful unless the full resource chain closes honestly.

That means IX-Breath must answer:

1. where the heat comes from,
2. how that heat is moved and rejected,
3. when rescue solar helps and when it hurts,
4. when water is a resource versus a burden,
5. when regeneration is worth the thermal and electrical cost,
6. and when the correct answer is to shut regeneration down and preserve core survival instead.

---

## 2. Core design posture

The thermal/resource posture of IX-Breath is governed by one central rule:

> **No oxygen extension claim counts unless the architecture can also pay for its electrical cost, thermal cost, and water cost at the same time.**

This is the opposite of concept theater.

It means the design will not:

- count recovered water that is not actually available,
- count solar power in eclipse,
- count electrolyzer oxygen without counting electrolyzer heat,
- count thermal-control headroom that is not really there,
- or quietly convert a rescue backpack into a one-person spacecraft.

---

## 3. Architecture statement

**IX-Breath uses a thermal survivability layer that collects and rejects heat from life-support, power, and regeneration hardware, while a resource-coupling gate determines whether rescue solar and oxygen regeneration are genuinely affordable under the current water, power, and thermal state.**

In plain English:

- S5 manages heat,
- S6 tells the system how much power margin really exists,
- S4 tells the system what water is actually usable,
- S3 asks permission to run,
- and S7 decides whether running S3 helps survival or just burns margin.

---

## 4. Why this layer exists

Without this layer, the repo would drift into four bad habits:

1. **free-solar fantasy**  
   treating sunlight as guaranteed and consequence-free

2. **free-regen fantasy**  
   acting like oxygen generation has no heat or power burden

3. **free-water fantasy**  
   assuming condensate automatically becomes PEM-ready feedwater

4. **free-thermal fantasy**  
   pretending extra electronics and electrochemistry create no packaging or rejection burden

This document exists to stop all four.

---

## 5. Thermal and resource flow overview

```mermaid
flowchart LR

    S1[Life-Support Core]
    S3[Regenerative Extension]
    S4[Water Recovery and Conditioning]
    S5[Thermal Survivability Layer]
    S6[Power Continuity Spine]
    S7[Supervisory Control / FDIR]
    ENV[External Environment]

    SOL[Rescue Solar Input]
    BAT[Battery Energy]
    H2O[Qualified Water]
    O2[Regenerated Oxygen]

    S1 -->|process heat| S5
    S3 -->|electrolyzer heat| S5
    S6 -->|battery/PMU heat| S5
    S4 -->|water state| S7
    S6 -->|power margin| S7
    S5 -->|thermal margin| S7
    S7 -->|regen allow/deny| S3
    S7 -->|load shed / derate| S6
    S3 -->|oxygen| O2
    S4 -->|qualified water| H2O
    SOL --> S6
    BAT --> S6
    S5 -->|heat rejection| ENV

6. Heat-source map

IX-Breath does not treat “the backpack” as one thermal blob.

It identifies heat by source family.

6.1 Primary heat-source families
Source family	Typical cause	Thermal posture
T1	circulation and gas-path hardware	continuous operational heat
T2	CO2/humidity-control hardware	process heat / pressure-drop / regeneration-related burden
T3	battery and BMS	storage inefficiency, charge/discharge burden, protection events
T4	PMU and converters	switching/conversion loss
T5	electrolyzer stack and electrolysis electronics	high-consequence intermittent thermal burden
T6	control electronics and sensor conditioning	lower absolute heat, but high sensitivity to hotspot drift
T7	rescue solar intake electronics	added conversion burden when solar path is active
6.2 Heat-priority rule

The system cares most about heat that threatens:

B1 life-support continuity,

B2 critical control continuity,

oxygen reserve integrity,

sensor confidence,

then endurance-extension hardware.

That order is locked.

7. S5 — Thermal Survivability Layer
7.1 Role

S5 exists to keep IX-Breath alive long enough for rescue to matter.

Its role is to:

collect heat from critical sources,

move heat away from local hotspots,

reject heat to the environment when possible,

smooth short thermal spikes,

and force derating before hidden thermal damage becomes system collapse.

7.2 S5 internal functions

At architecture level, S5 includes the functional equivalents of:

thermal pickup plates / cold plates,

heat transport path or compact loop,

shell-integrated rejection area,

local insulation and thermal isolation barriers,

thermal sensors on critical bays,

rescue-only thermal aid interface if later justified,

thermal derate outputs into S7.

7.3 Thermal posture rule

S5 is not an optional efficiency helper.
It is a survival function.

If S5 is overstressed, IX-Breath must reduce capability rather than pretend everything is fine.

8. Thermal zones

To keep packaging honest, IX-Breath tracks distinct thermal zones.

Zone	Primary contents	Thermal concern
TH-Z1	blower / gas processing zone	continuous moderate heat near breathing loop
TH-Z2	oxygen continuity zone	protect regulators, valves, and reserve oxygen from adjacent hot hardware
TH-Z3	water / regeneration zone	highest intermittent thermal burden when PEM runs
TH-Z4	power spine zone	converter and battery hotspot management
TH-Z5	control / sensor zone	low heat, high sensitivity to contamination and drift
TH-Z6	shell / rejection zone	controlled export of heat to environment
8.1 Thermal separation rule

The pack may not treat TH-Z3 and TH-Z5 as casually adjacent without isolation.

Electrolyzer heat and quiet control electronics are not roommates.

9. Thermal transport posture
9.1 Why transport matters

Heat that cannot move is heat that accumulates.

The architecture therefore requires a deliberate transport path from hot zones to rejection zones.

9.2 Transport functions

The transport layer must:

pull heat from battery/PMU hotspots,

pull heat from PEM and related power electronics,

support the gas-processing side where needed,

avoid dumping avoidable heat into the reserve oxygen region,

and support bounded transient smoothing.

9.3 Transport rule

IX-Breath phase 1 does not lock one exact transport topology.

What is locked is the function:

heat must be collected,

moved,

and rejected by a real path.

No “the shell will probably soak it” language is allowed.

10. Rejection posture
10.1 Baseline rejection

The baseline rejection posture is:

shell-integrated rejection area first,

controlled thermal path second,

rescue-only deployable help only if later packaging justifies it.

10.2 Why shell-first was chosen

Shell-integrated rejection survives the seriousness filter because it:

keeps the normal pack geometry sane,

avoids giant emergency appendages,

and fits the packaging discipline already locked.

10.3 Rejection rule

Any later design that claims major thermal rejection gains without:

area accounting,

transport-path accounting,

and deployable/stowed geometry accounting

is not acceptable in IX-Breath.

11. Thermal buffering posture
11.1 Why buffering exists

Some loads are spiky.
The PEM path in particular may be intermittent and harsh.

Thermal buffering exists to:

absorb short spikes,

reduce rapid-control oscillation,

buy time for derating decisions,

and avoid false trips from every brief surge.

11.2 What buffering is not

A buffer is not a magic heat sink.
It does not erase the eventual rejection burden.

It buys time.
That is all.

11.3 Buffer rule

The architecture may use bounded thermal storage or phase-buffer logic, but may not use buffering as an excuse to ignore steady-state thermal limits.

12. Rescue solar posture
12.1 Why rescue solar exists

Rescue solar exists because it can extend survival in some cases.

It does not exist because it solves all cases.

12.2 Permitted rescue solar role

The rescue solar path may:

reduce battery drain in sunlit rescue cases,

help keep B3 alive when conditions justify it,

support limited oxygen regeneration when the full resource gate permits it,

and preserve battery margin for later dark intervals.

12.3 Forbidden rescue solar rhetoric

The rescue solar path may not be used to claim:

indefinite life support,

eclipse-proof operation,

“battery-free” rescue,

guaranteed regeneration,

or permanent self-sustainment.

That language is forbidden.

12.4 Solar penalty rule

Solar help is not free.

When the solar path is active, the architecture must count:

intake electronics burden,

conversion inefficiency,

thermal burden of conversion,

deployment risk,

and the possibility that the solar contribution is weak or unstable.

13. Lighting-condition classes

IX-Breath treats lighting honestly by splitting rescue into three classes.

Class	Meaning	Solar posture
LC1	Sunlit favorable	solar path may help materially
LC2	Mixed-light / intermittent	solar path may help inconsistently and must not anchor survival assumptions
LC3	Eclipse / dark interval	solar path contributes nothing useful
13.1 Rule

Any endurance discussion that does not say which lighting class applies is incomplete.

That is locked now.

14. Water-resource posture
14.1 Water has dual meaning

In IX-Breath, water is both:

a resource for bounded oxygen regeneration,

and a burden that must be collected, stored, conditioned, and thermally accounted for.

That means water is never “just plumbing.”

14.2 Water classes
Water class	Meaning
WQ1	sterile reserve water, highest-confidence feedstock
WQ2	recovered and conditioned condensate, usable if quality gate passes
WQ3	recovered but not yet qualified water, not creditable to PEM
WQ4	unavailable / contaminated / uncertain water, denied to PEM
14.3 Water rule

Only WQ1 and WQ2 may be counted as feedstock for oxygen regeneration.

Anything else is denied.

15. The resource-coupling gate
15.1 Why the gate exists

The electrolyzer should not be controlled by hope.

It should be controlled by a gate that asks whether the full bargain is positive.

15.2 Inputs to the gate

The minimum input set is:

power margin from S6

thermal margin from S5

qualified water state from S4

life-support stability from S1/S2

mode state from S7

lighting class / solar validity when applicable

hydrogen vent availability

sensor confidence

15.3 Output of the gate

The gate produces one of four postures:

Gate result	Meaning
RG-0 Deny	regeneration forbidden
RG-1 Hold	regeneration possible later, but not now
RG-2 Limited	regeneration allowed in a bounded conservative mode
RG-3 Enabled	regeneration allowed at planned rescue setting
15.4 Gate rule

The default bias is conservative.

If the system is uncertain, the gate should deny or hold, not pretend the gamble is fine.

16. Regeneration affordability logic
16.1 Regeneration is only affordable if all four are true

A regeneration request is only considered affordable when:

Power margin exists

Thermal margin exists

Qualified water exists

Survival payoff is positive

If one of those is false, regeneration is not honestly affordable.

16.2 Survival payoff definition

“Positive payoff” means:

running S3 preserves or extends net survivability more than it degrades battery, thermal, or control margin.

This is not emotional.
It is accounting.

16.3 Negative-payoff examples

Regeneration should be denied when:

the pack is already thermal-limited,

the battery is being preserved for dark interval survival,

recovered water is not qualified,

the hydrogen vent path is questionable,

or B1/B2 stability is not strong enough.

17. Thermal derating ladder
17.1 Why a ladder exists

Thermal response should not jump directly from “fine” to “dead.”

It should degrade in an orderly way.

17.2 Thermal ladder
Level	Posture	Typical action
TD-0	nominal	no thermal restriction
TD-1	caution	tighten monitoring, restrict nonessential B4 loads
TD-2	constrained	reduce or suspend B3 extension loads, bias against S3
TD-3	severe	shut down regeneration, preserve B1/B2, simplify control posture
TD-4	survival-only	minimum life-support and control continuity only
17.3 Important rule

At TD-2 and beyond, regeneration must justify itself explicitly.

At TD-3, the default posture is off.

18. Thermal interaction with reserve oxygen
18.1 Why reserve oxygen gets special handling

Reserve oxygen is the last-line continuity layer.
It must not be casually exposed to neighboring heat.

18.2 Reserve-protection posture

The architecture must:

thermally isolate reserve oxygen from PEM and PMU hotspots,

keep reserve actuation hardware out of the hottest bays,

and prevent rescue-solar electronics from baking the reserve path during long sunlit rescue exposure.

18.3 Rule

If a future package layout puts reserve oxygen next to the hottest intermittent load without protective isolation, that layout fails IX-Breath architecture rules.

19. Thermal interaction with sensing and control
19.1 Why this matters

Control hardware usually survives lower absolute heat than power hardware, but it fails credibility much sooner when drift and noise rise.

19.2 Control-bay posture

The control/sensor zone should be:

thermally quiet,

isolated from PEM and PMU hotspots,

protected from aggressive solar-intake converter heat,

and monitored for rising drift risk.

19.3 Rule

A design that keeps the breathing loop alive while cooking the control bay into unreliable state estimation is not actually safe.

20. Rescue solar and regeneration joint behavior
20.1 Allowed joint behavior

In favorable sunlit rescue conditions, the system may:

accept rescue solar input,

reduce net battery drain,

and allow limited or enabled regeneration if the thermal and water gates also pass.

20.2 Disallowed joint behavior

The system may not assume:

“sunlight present” automatically means “run PEM,”

or “solar active” automatically means “battery concern solved.”

Those are lazy assumptions.

20.3 Joint-rule summary

Solar validity is necessary for some sunlit extension cases.
It is not sufficient by itself.

21. State-by-state thermal/resource posture
21.1 Nominal

thermal system supports normal breathing and electronics

rescue solar typically stowed or ignored

regeneration usually off or tightly limited

water is recovered and logged

reserve oxygen fully protected

21.2 Conserve

noncritical heat sources are reduced

B4 shedding begins

recovery water becomes more survival-accounted

regeneration usually held or denied unless payoff is obvious

solar may be accepted only if simple and beneficial

21.3 Rescue

thermal and power logic become survival-first

solar may deploy if lighting class supports it

regeneration may enter RG-2 or RG-3 only if the full gate passes

water reserve allocation becomes explicit

reserve oxygen remains isolated unless separate trigger logic says otherwise

21.4 Critical

extension loads are strongly biased off

thermal ladder usually forces TD-2 or worse

regeneration typically denied

only core breathing, control, and thermal survival needed for those remain

solar may still help battery preservation if safe, but is not allowed to complicate control posture

21.5 Safe-Hold

latched degraded posture

regeneration denied

deployables may remain stowed or be retracted if they are now a liability

B1/B2 defended as long as physically possible

22. Failure cases this layer is trying to survive better

This document is not a full FMEA, but it does define target failure families.

Failure ID	Failure family	Intended posture
THR-1	PEM thermal runaway tendency or excessive stack heating	cut S3, preserve B1/B2, maintain logging
THR-2	PMU / converter hotspot growth	derate noncritical loads, preserve critical rails
THR-3	battery temperature excursion	invoke pack protection and survival-only bias
THR-4	rejection capacity collapse or degradation	climb thermal ladder, deny S3, simplify operation
THR-5	solar-path instability with added converter heat	drop solar path cleanly
THR-6	water recovered but not qualified	deny S3 despite apparent water availability
THR-7	qualified water present but thermal margin absent	deny S3 despite water availability
THR-8	good solar but poor thermal state	use solar only if it helps core loads without forcing more heat than it saves
THR-9	sensor confidence degraded in high-heat condition	bias conservative, deny extension loads, alert crew
THR-10	deployable thermal aid failure	continue with shell-baseline rejection and deeper derate if needed
23. Resource truth model
23.1 Why this model exists

Any survivability estimate is garbage if the inputs are fuzzy.

So IX-Breath uses a truth model:

count only what is real

count only what is available now

count only what can be paid for thermally and electrically

23.2 Resource-credit rules
Resource	May be credited when...
Battery energy	available, healthy, and not already reserved for deeper survival need
Solar input	valid under current lighting and converter acceptance
Recovered water	captured, stored, conditioned, and not thermally/power-forbidden to use
Regenerated oxygen	actually producible under active gate approval
Thermal headroom	supported by real transport and rejection capacity
23.3 Explicit rejection

The following may not be credited:

hypothetical future sunlight,

unmeasured condensate,

“probably okay” thermal headroom,

oxygen that would require denied regeneration to exist,

or any resource path blocked by a current fault.

24. Verification posture

This document does not claim IX-Breath has already proven thermal/resource closure.

It defines what later seriousness will require.

24.1 Future verification themes

A serious validation path should eventually challenge:

regeneration affordability under sunlit and eclipse cases,

thermal ladder entry/exit behavior,

rescue-solar benefit versus penalty,

water-class gating correctness,

solar-path rejection and clean dropout,

PEM shutdown under rising thermal stress,

control-bay stability near hot loads,

reserve oxygen protection near adjacent thermal events,

and mixed-light rescue cases where solar is intermittent.

24.2 Seriousness rule

Any later analysis or demo that shows oxygen production without showing:

power source,

water class,

thermal burden,

and allowed gate state

is not serious enough for IX-Breath.

25. Locked design decisions from this document

The following decisions are now locked unless later evidence forces revision:

Thermal margin is a gating resource, not a side note.

Rescue solar is conditional help, not guaranteed salvation.

Water must be classified before it is credited to regeneration.

Regeneration requires simultaneous power, thermal, and water affordability.

Thermal derating happens in staged levels, not magical last-second recovery.

Reserve oxygen must be thermally protected from nearby hotspots.

Control/sensor integrity must be preserved against thermal drift from power and PEM hardware.

Solar-active does not automatically imply regeneration-allowed.

A denied regeneration decision is a valid survival decision, not a design failure.

All endurance claims must state lighting class and resource-gate posture.

26. What this document deliberately does not claim

This document does not claim:

final radiator area,

final thermal-loop topology,

final PEM duty cycle,

final solar blanket size,

final rejection coefficients,

final water recovery rates,

final endurance numbers.

Those belong to the model and validation work later.

What this document does claim is the important part for this stage:

IX-Breath now has a coherent thermal/resource architecture that honestly couples oxygen regeneration to electrical, water, and thermal reality.

That is enough for this commit.

27. What this file forces next

Because thermal/resource coupling is now defined, the next commits must show:

the exact system state machine and FDIR logic,

the mass/power/consumables/survivability model,

the validation plan and hazard logic that try to break those claims honestly.

That pressure is intentional.
