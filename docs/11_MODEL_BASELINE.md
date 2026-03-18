# IX-Breath Mass, Power, Consumables, and Survivability Model

**Document ID:** IXB-MODEL-11  
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
- `docs/10_FDIR_AND_STATE_MACHINE.md`

---

## 1. Purpose

This document defines the **first bounded analytical model** for IX-Breath.

Its job is to answer the only question that matters after architecture:

> **Do the mass, power, oxygen, water, and thermal assumptions close well enough to justify the concept as a serious rescue-biased PLSS architecture study?**

This document is **not** a test report.  
It is **not** a certification basis.  
It is **not** a flight claim.

It is an architecture-level closure model that does four things:

1. states the working inputs,
2. allocates mass and power honestly,
3. evaluates defended and stretch rescue cases,
4. explicitly says what does **not** close in the current baseline.

---

## 2. Modeling philosophy

IX-Breath uses a blunt modeling rule:

> **Survival time is whichever subsystem runs out first, not whichever subsystem looks best in isolation.**

That means no single result is allowed to hide the real limiter.

The model always treats survivability as the minimum of:

- power-limited duration,
- oxygen-limited duration,
- CO2-control-limited duration,
- thermal-limited duration,
- and fault/posture-limited duration.

### 2.1 Core model equation

At architecture level:

```text
t_survival = min(
  t_power,
  t_oxygen,
  t_CO2_control,
  t_thermal,
  t_fault_posture
)

If one branch does not close, the case does not close.

3. Model use policy

This repository uses three kinds of numbers:

Number type	Meaning
Baseline fact	external or internal fact already fixed elsewhere in the repo
Working model input	a bounded analytical assumption used to test closure
Result	the model outcome from the chosen inputs

The numbers in this document are primarily working model inputs and results.

That distinction matters.

They are not being passed off as validated hardware performance.

4. Model boundaries

This model evaluates IX-Breath as:

a backpack-class rescue-biased system,

in the locked 25 x 20 x 10.5 inch worn envelope family,

with the subsystem set already defined in prior commits,

under low-activity to very-low-activity emergency survival posture,

with no catastrophic suit breach,

and with no free credit from magical energy or hidden consumables.

4.1 What this model does include

battery energy

accepted rescue-solar contribution when the lighting class permits it

oxygen stored in primary and isolated reserve paths

bounded regenerated oxygen when the full gate permits it

water required for regenerated oxygen

thermal/resource gate denial of regeneration

state-dependent load shedding

4.2 What this model does not include

catastrophic suit puncture survival

high-workload EVA continuation

unbounded locomotion duty

unmodeled external rescue propulsion help

agency-specific certification margins

unverified miraculous water recovery

unverified multi-day steady PEM operation in a final flight package

5. Governing model relationships
5.1 Power-limited duration
t_power = E_usable / P_net

Where:

E_usable = battery_usable + solar_accepted_over_interval - protected_reserve_margin

P_net = total_enabled_load - accepted_solar_offset

5.2 Oxygen-limited duration
t_oxygen = O2_total_available / O2_metabolic_rate

Where:

O2_total_available = O2_primary + O2_reserve_accessible + O2_regenerated_credited

5.3 Regenerated oxygen credit

O2_regenerated_credited <= min(
  water_qualified / 1.125,
  E_regen_allocated / regen_specific_energy,
  gate_limited_output
)

5.4 Water stoichiometry

For the retained electrolysis path:

2 H2O -> 2 H2 + O2

By mass, that means:
1.125 lb water -> 1.000 lb oxygen

That relationship is chemistry, not opinion.

5.5 Important model rule

Regenerated oxygen is credited only if:

qualified water exists,

electrical energy is allocated,

thermal margin exists,

and the state machine actually permits S3 to run.

If any one of those is false, regenerated oxygen credit becomes zero.

6. Baseline working inputs

These are the phase-1 working model inputs used to test closure.

6.1 Energy storage inputs
Parameter	Working input band	Baseline midpoint used for cases
Usable battery energy	1.8–2.2 kWh	2.0 kWh
Protected non-spend margin	0.1–0.2 kWh	0.15 kWh
Supercap energy	not used for endurance credit	not credited to endurance
Accepted rescue-solar average, LC1	50–90 W	case-specific
Accepted rescue-solar average, LC2	15–40 W	case-specific
Accepted rescue-solar average, LC3	0 W	0 W
6.2 State power inputs
State	Working average pack load without PEM	Working average additional PEM/regen load when enabled
Nominal	95–120 W	not normally credited
Conserve	70–90 W	15–35 W if limited
Rescue	60–80 W	20–45 W if limited; 45–80 W if strongly enabled
Critical	40–55 W	denied
Safe-Hold	35–50 W	denied
6.3 Oxygen inventory inputs
Parameter	Working input band	Baseline midpoint used for cases
Primary usable oxygen	1.8–2.3 lb	2.1 lb
Isolated reserve oxygen	0.4–0.7 lb	0.5 lb
Regenerated oxygen credit in practical stretch cases	0.1–0.5 lb	case-specific
Low-activity contingency O2 demand	0.055–0.075 lb/hr	0.065 lb/hr
6.4 Water inputs
Parameter	Working input band	Baseline midpoint used for cases
Sterile reserve water	0.6–1.4 lb	1.0 lb
Recoverable and conditionable condensate over 24 hr rescue window	0.2–0.8 lb	case-specific
Water qualified for PEM in defended dark case	0–0.4 lb	case-specific
Water qualified for PEM in favorable sunlit stretch case	0.4–1.0 lb	case-specific
6.5 Regeneration-specific energy input

For the phase-1 model, regenerated oxygen is bounded using a working system-level electrical input band of:

Parameter	Working input band
System-level electrical input per lb O2 delivered	3.2–4.5 kWh/lb O2

This is a working model assumption, not a validated pack-level test result.

6.6 Lighting classes used by the model
Class	Meaning
LC1	favorable sunlit rescue
LC2	mixed-light or intermittent solar help
LC3	eclipse or dark interval, no solar help
7. Mass budget model

The phase-1 mass model is designed to answer one question:

Can IX-Breath stay backpack-class while carrying the subsystems it is claiming?

7.1 Dry mass allocation
Subsystem family	Working mass band
S1 Life-Support Core	12–16 lb
S2 Oxygen Storage and Delivery hardware only	12–16 lb
S3 Regenerative Extension hardware	7–12 lb
S4 Water Recovery and Conditioning hardware	5–8 lb
S5 Thermal Survivability hardware	7–11 lb
S6 Power Continuity Spine hardware	16–22 lb
S7 Control / FDIR / Crew Interface	3–5 lb
S8 Shell / partitions / routing / protection	12–18 lb
7.2 Dry mass subtotal
Dry mass band = 74–108 lb
Working baseline midpoint = 89 lb
7.3 Serviced mass additions
Added serviced mass item	Working mass band
Oxygen fill mass	2.2–3.0 lb
Water fill mass	0.8–2.0 lb
Misc. service overhead / margin	1–3 lb
7.4 Serviced mass subtotal
Fully serviced mass band = 78–114 lb
Working baseline midpoint = 95 lb
7.5 Packaging/mass conclusion

At the architecture-study level, IX-Breath does not obviously break backpack-class mass and volume discipline, but it is already living in a serious regime.

That means:

there is room for a real study,

there is not room for casual feature creep,

and any later subsystem that casually adds 5–10 lb should be treated as a major review event.

8. Power model by state

The purpose of this section is to show the pack’s average enabled load posture by state.

8.1 Baseline average load model
Load family	Nominal	Conserve	Rescue	Critical	Safe-Hold
S1 gas loop + sensing	42 W	38 W	36 W	30 W	28 W
S2 oxygen delivery / valves / sensors	9 W	8 W	8 W	7 W	6 W
S4 water handling	7 W	5 W	5 W	2 W	1 W
S5 thermal-control essentials	18 W	14 W	14 W	10 W	8 W
S6 PMU / conversion losses	10 W	8 W	8 W	6 W	5 W
S7 control / watchdog / alerts / logging	8 W	7 W	7 W	6 W	5 W
B4 noncritical support	12 W	4 W	1 W	0 W	0 W
8.2 Total non-PEM average load by state

Nominal   = 106 W
Conserve  = 84 W
Rescue    = 79 W
Critical  = 61 W
Safe-Hold = 53 W

8.3 Bounded lower-power rescue posture

A more aggressive rescue derate posture is also modeled:
Rescue-lean = 68 W average
Critical-lean = 48 W average

This lower-power posture assumes:

B4 essentially gone,

B3 mostly denied,

constrained thermal-control operation,

simplified alerting,

and no PEM credit unless separately enabled.

8.4 PEM load posture

The model uses three PEM operating bands:

PEM mode	Additional average pack load	Intended use
PEM-DENIED	0 W	no regen credit
PEM-LIMITED	20–35 W average	intermittent bounded regeneration
PEM-ENABLED	45–70 W average	favorable stretch case only
9. Oxygen and water model
9.1 Stored oxygen baseline

The baseline midpoint case assumes:
Primary oxygen = 2.1 lb
Reserve oxygen = 0.5 lb
Total stored oxygen = 2.6 lb

9.2 Oxygen-only survival window at low-activity midpoint

Using the working midpoint metabolic input:
O2 demand = 0.065 lb/hr
Stored-only duration = 2.6 / 0.065 = 40.0 hr

9.3 Why that number is not the answer by itself

Because power and thermal limits may end the case earlier.

That is exactly why this repo uses a multi-limiter model.

9.4 Water-to-oxygen conversion ceiling

At stoichiometric best case:
1.0 lb qualified water -> 0.889 lb oxygen

But the model does not credit that full amount automatically.

It further limits regenerated oxygen by:

electrical energy allocated,

thermal affordability,

gate permission,

and realistic partial-duty operation.

9.5 Practical credited regen bands in phase-1 model
Rescue case quality	Credited regenerated oxygen
Dark/eclipsed defended case	0.0–0.1 lb
Mixed-light stretch case	0.1–0.3 lb
Favorable sunlit stretch case	0.2–0.5 lb
Outer-edge aspirational analytical case	up to ~0.8 lb but not baseline-claimable
10. Analytical rescue cases

IX-Breath phase 1 evaluates four cases.

10.1 Case D24 — Defended dark case

Purpose:
Prove the architecture can support a serious 24-hour survival target without solar credit and without leaning on PEM magic.

Inputs:

Lighting class: LC3

Average state posture: Rescue-lean

Average pack load: 68 W

Battery usable after protected margin: 1.85 kWh

Solar accepted: 0 W

Regenerated oxygen credited: 0.0 lb

Stored oxygen available: 2.6 lb

O2 demand: 0.065 lb/hr

Power result:
t_power = 1.85 kWh / 0.068 kW = 27.2 hr

Oxygen result:
t_oxygen = 2.6 / 0.065 = 40.0 hr

Analytical case result:
Defended dark case closes a 24 hr target with power as the dominant limiter.

Important note:
This is an analytical closure, not a hardware proof.

10.2 Case S36 — Mixed-light stretch case

Purpose:
Test whether IX-Breath can reach a serious 36-hour stretch case when solar help is intermittent and regeneration is only modestly useful.

Inputs:

Lighting class: LC2

Average state posture: Rescue

Average non-PEM load: 79 W

Average accepted solar: 20 W

Limited PEM average load: 15 W additional averaged across interval

Net average load:
79 + 15 - 20 = 74 W

Battery usable after protected margin: 1.85 kWh

Credited regenerated oxygen: 0.20 lb

Total credited oxygen:
2.6 + 0.20 = 2.80 lb

O2 demand: 0.065 lb/hr

Power result:
t_power = 1.85 / 0.074 = 25.0 hr

This does not close 36 hr.

So the architecture needs a more aggressive rescue-lean posture in this class if 36 hr is the goal.

Revised mixed-light rescue-lean posture:

Rescue-lean load: 68 W

Limited PEM averaged: 12 W

Accepted solar average: 28 W

Net average load:
68 + 12 - 28 = 52 W

Revised power result:
t_power = 1.85 / 0.052 = 35.6 hr

Revised oxygen result:
t_oxygen = 2.80 / 0.065 = 43.1 hr

Analytical case result:
A disciplined mixed-light 36 hr stretch case is analytically plausible,
but only with aggressive load shedding, modest accepted solar,
and only limited PEM credit.

10.3 Case S48 — Favorable sunlit stretch case

Purpose:
Test whether IX-Breath can plausibly reach 48 hours in a favorable rescue condition without lying about what actually limits it.

Inputs:

Lighting class: LC1

Average state posture: Rescue-lean

Average non-PEM load: 68 W

Average accepted solar: 45 W

Limited-to-moderate PEM average added load: 20 W

Net average load:
68 + 20 - 45 = 43 W

Battery usable after protected margin: 1.85 kWh

Credited regenerated oxygen: 0.45 lb

Total credited oxygen:
2.6 + 0.45 = 3.05 lb

O2 demand: 0.065 lb/hr

Power result:
t_power = 1.85 / 0.043 = 43.0 hr

That is still short of 48 hr.

So the model must either:

reduce average load further,

improve accepted solar,

or reduce metabolic O2 demand modestly.

Improved favorable-sunlit case:

Rescue-lean load: 64 W

PEM average added load: 16 W

Accepted solar average: 48 W

Net average load:
64 + 16 - 48 = 32 W

Improved power result:
t_power = 1.85 / 0.032 = 57.8 hr

Improved oxygen result:
t_oxygen = 3.05 / 0.065 = 46.9 hr

This still misses 48 hr very slightly on oxygen.

Further oxygen adjustment by lower activity / better water closure:

O2 demand reduced to 0.062 lb/hr in a very-low-activity survival posture

t_oxygen = 3.05 / 0.062 = 49.2 hr

Analytical case result:
A 48 hr favorable sunlit stretch case is analytically plausible,
but it is not a broad baseline claim.
It requires low activity, favorable solar acceptance,
strict rescue-lean power posture, and meaningful but bounded PEM credit.

10.4 Case O72 — Outer-edge analytical case

Purpose:
Stress-test the architecture and decide whether a 72-hour claim belongs in the serious baseline.

Inputs tested:

Lighting class: LC1 favorable most of the window

Rescue-lean non-PEM average: 60 W

PEM averaged: 18 W

Accepted solar average: 55 W

Net average load:
60 + 18 - 55 = 23 W

Power result:
t_power = 1.85 / 0.023 = 80.4 hr

Power can close under a favorable assumed solar case.

But oxygen is the problem.

To close 72 hr at very-low-activity 0.060 lb/hr, the required oxygen is:
72 * 0.060 = 4.32 lb O2

With stored oxygen at 2.6 lb, the additional oxygen needed is:
4.32 - 2.6 = 1.72 lb O2

That would require, at stoichiometric minimum:
1.72 * 1.125 = 1.94 lb qualified water

And still substantial electrical and thermal headroom.

10.4.1 O72 conclusion
72 hr does not close as a baseline IX-Breath claim.

It may remain an outer-edge analytical exploration, but it is not serious enough to advertise as a repo baseline.

That is the correct answer.

11. Case summary table
Case	Lighting	Power closure	O2 closure	Baseline conclusion
D24	LC3	~27.2 hr	~40.0 hr	24 hr defended case closes analytically
S36	LC2	~35.6 hr	~43.1 hr	36 hr stretch case can close with strict rescue-lean posture
S48	LC1	~57.8 hr	~49.2 hr	48 hr case can close only under favorable low-activity sunlit assumptions
O72	LC1 favored	~80.4 hr	~<72 hr unless very aggressive extra O2 credit is assumed	72 hr remains non-baseline exploratory
12. Dominant sensitivity drivers

The model says IX-Breath is most sensitive to the following five drivers:

12.1 Metabolic O2 demand

Small changes in activity level move the oxygen limit dramatically.

12.2 Accepted solar, not theoretical solar

The important number is accepted average electrical offset, not nameplate panel output.

12.3 Rescue-state average load

A sloppy rescue load posture destroys closure fast.

12.4 Qualified water, not recovered moisture hand-waving

Water that is not captured, conditioned, and permitted by the gate does not count.

12.5 Thermal gate denials

Even when power and water look decent, thermal denial can zero out regeneration credit.

13. What the model actually says

This model supports the following bounded conclusions:

13.1 Supported analytical conclusion A

A 24-hour defended survival target is analytically plausible in a dark/eclipsed case without solar credit and without relying on PEM regeneration.

13.2 Supported analytical conclusion B

A 36-hour stretch target is analytically plausible in mixed-light conditions only if IX-Breath enters a disciplined rescue-lean posture with strong load shedding and modest solar acceptance.

13.3 Supported analytical conclusion C

A 48-hour favorable sunlit stretch target is analytically plausible, but only under a narrower set of assumptions:

low activity,

favorable lighting,

useful accepted solar,

limited bounded regeneration,

and preserved thermal margin.

13.4 Unsupported baseline conclusion

A 72-hour claim is not supported as a baseline repo claim at this stage.

That is the honest answer.

14. What would break closure quickly

The following conditions break IX-Breath analytical closure fast:

higher-than-modeled activity

any meaningful gas leak

solar acceptance weaker than modeled

thermal gate denying PEM more often than assumed

qualified water lower than assumed

rescue-state power drifting back toward nominal behavior

reserve oxygen becoming inaccessible or uncertain

Any of those can collapse a stretch case into a much shorter one.

15. Model-based design pressure

This model now puts pressure on the architecture.

15.1 Pressure on S6

The power spine must preserve true rescue-lean loads, not nominal luxury behavior in disguise.

15.2 Pressure on S5

Thermal performance must be good enough that S3 is not denied constantly in the only cases where it would help.

15.3 Pressure on S4

Water recovery and conditioning cannot stay vague.
They materially affect oxygen stretch capability.

15.4 Pressure on S8

Mass creep is already dangerous.
The architecture does not have room for casual additions.

16. Model limitations

This model is deliberately honest about its limits.

It does not yet include:

detailed transient thermal simulation,

validated blower or scrubber pressure-drop models,

full reserve-trigger threshold optimization,

mission-specific orbital lighting simulation,

suit-specific human factors penalties,

full leak-rate sensitivity sweep,

full probabilistic failure analysis.

Those are future work items.

That does not make this model useless.
It makes it appropriately scoped for the current repo phase.

17. Locked design decisions from this document

The following decisions are now locked unless later evidence forces revision:

IX-Breath may seriously target 24 hr defended analytical survival.

IX-Breath may seriously study 36–48 hr stretch survival under bounded favorable assumptions.

IX-Breath may not advertise 72 hr as a baseline claim.

Power and oxygen must both close; one is never enough.

Regenerated oxygen credit is denied unless water, power, and thermal state all permit it.

Accepted solar, not theoretical solar, is the only solar number that matters.

Rescue-lean load posture is mandatory for stretch cases.

Mass creep beyond the current baseline band is a major threat to concept seriousness.

Any future README must state the difference between defended and stretch cases clearly.

The repo now has enough model closure to justify serious validation planning, but not enough to claim proof.

18. What this document deliberately does not claim

This document does not claim:

validated flight survivability,

verified human metabolic closure for every astronaut and every activity level,

proven multi-day thermal closure in hardware,

proven long-duration PEM closure in the final package,

final part sizing,

or final mass certainty.

What it does claim is narrower and important:

IX-Breath now has an honest architecture-level closure model showing that a 24-hour defended case is plausible, 36–48 hour stretch cases are plausible only under bounded favorable assumptions, and 72 hours is not yet serious enough to claim.

