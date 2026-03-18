# IX-Breath Power Continuity Spine

**Document ID:** IXB-PWR-07  
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

---

## 1. Purpose

This document defines the detailed electrical architecture for **S6 — Power Continuity Spine**.

Its purpose is to answer the questions a serious reviewer will immediately ask:

1. What are the actual power sources?
2. How are those sources combined without dangerous backfeed?
3. Which buses stay alive longest?
4. How does the pack survive transients, brownouts, branch faults, and controller faults?
5. What gets shed first, and what is never allowed to sit behind noncritical hardware?
6. How does the electrical architecture support rescue survivability instead of merely powering features?

This file is intentionally conservative.

It is not trying to be clever.
It is trying to keep the astronaut alive.

---

## 2. Electrical design posture

The IX-Breath electrical system is built around one central rule:

> **Life-support continuity is more important than full-feature continuity.**

That means the electrical design is not optimized for:
- maximum feature richness,
- uninterrupted convenience telemetry,
- pretty bus symmetry,
- or speculative energy capture.

It is optimized for:
- clean power to life-critical loads,
- fault containment,
- graceful degradation,
- minimum retained awareness during failures,
- and predictable behavior under stress.

---

## 3. Architecture statement

**S6 is a layered, protected, multi-tier electrical spine built around a primary battery source, a fast pulse-buffer stage, source-isolated power conversion, criticality-tiered buses, protected startup sequencing, and explicit safe-dump behavior.**

In plain English:

- one main battery carries the system,
- one supercapacitor stage smooths violent transient behavior,
- optional rescue solar may help only when it actually can,
- no source is allowed to backfeed into the others,
- life-critical buses are kept ahead of everything else,
- and the pack must remain electrically intelligible even when heavily derated.

---

## 4. Source stack

IX-Breath uses a **layered source stack**, not a magical “always-on” energy source.

### 4.1 Baseline electrical sources

| Source ID | Name | Role | Baseline posture |
|---|---|---|---|
| P1 | Primary battery pack | main stored electrical energy | mandatory |
| P2 | Supercapacitor pulse buffer | transient ride-through and pulsed-load support | mandatory |
| P3 | Dockside / habitat recharge path | recharge and service power | mandatory |
| P4 | Rescue solar augmentation | optional sunlit rescue support only | optional |

### 4.2 Source hierarchy

The power spine treats sources in the following hierarchy:

1. **P1 battery is the primary mission source**
2. **P2 supercap is the transient stabilizer**
3. **P3 recharge is a service / turnaround input**
4. **P4 rescue solar is conditional supplemental input**

Critical rule:

**P4 is never allowed to be rhetorically promoted into guaranteed life-support power.**

It is help, not salvation.

---

## 5. Top-level electrical block diagram

```mermaid
flowchart LR

    P1[Primary Battery Pack]
    P2[Supercapacitor Pulse Buffer]
    P3[Dockside / Habitat Recharge]
    P4[Optional Rescue Solar Input]

    A1[BMS and Battery Isolation]
    A2[Precharge Network]
    A3[Ideal-Diode / OR-ing Stage]
    A4[PMU Core]
    A5[Safe Dump Path]

    B1[B1 Critical Life-Support Bus]
    B2[B2 Critical Control Bus]
    B3[B3 Recovery / Extension Bus]
    B4[B4 Noncritical Support Bus]

    L1[S1 Life-Support Core]
    L2[S2 Oxygen Storage and Delivery]
    L3[S3 Regenerative Extension]
    L4[S4 Water Recovery and Conditioning]
    L5[S5 Thermal Survivability]
    L6[S7 Supervisory Control / FDIR]

    P1 --> A1 --> A2 --> A3
    P2 --> A2
    P3 --> A3
    P4 --> A3
    A3 --> A4
    A4 --> B1
    A4 --> B2
    A4 --> B3
    A4 --> B4
    A4 --> A5

    B1 --> L1
    B1 --> L2
    B2 --> L6
    B2 --> L1
    B3 --> L3
    B3 --> L4
    B3 --> L5
    B4 --> L6

    L6 --> A4

6. Electrical criticality tiers

The power spine only works if criticality is brutally explicit.

6.1 Bus tier definitions
Bus	Name	Purpose	Allowed posture
B1	Critical Life-Support Bus	powers functions that keep the breathing loop alive right now	highest protection, last to shed
B2	Critical Control Bus	powers supervisory logic, watchdog, minimum sensing, event logging, essential crew alerts	second-highest protection, kept alive with B1 as long as practical
B3	Recovery / Extension Bus	powers regenerative extension hardware, water handling support, selected thermal management hardware	shed before B1/B2
B4	Noncritical Support Bus	powers rich telemetry, service-rich features, convenience diagnostics, optional extras	first to shed
6.2 Bus rule

No B3 or B4 hardware may sit electrically upstream of B1 or B2 in a way that can block survival loads.

That rule is locked.

7. Bus contents
7.1 B1 — Critical Life-Support Bus

B1 exists to keep breathing viable.

It is expected to feed:

at least one circulation blower path,

essential valve actuation needed for safe gas routing,

primary loop pressure sensing support,

essential O2 and CO2 sensing support,

minimum oxygen metering path support,

minimum emergency oxygen isolation / actuation support.

B1 is not allowed to feed luxury loads.

7.2 B2 — Critical Control Bus

B2 exists to keep the pack coherent even when the rest is degrading.

It is expected to feed:

supervisory controller,

watchdog / safety supervisor,

blackbox event logger,

minimal sensor front-end support,

crew alert hardware,

fault latches,

safe-hold state retention logic.

7.3 B3 — Recovery / Extension Bus

B3 exists to support longer survival when the power budget allows it.

It is expected to feed:

electrolyzer power stage,

water feed pump / metering,

selected water conditioning hardware,

selected thermal-control hardware,

rescue-mode augmentation controllers.

B3 is useful, but it is not sacred.

If the system must choose between B3 and B1/B2, B3 loses.

7.4 B4 — Noncritical Support Bus

B4 exists for helpful but nonessential functions.

It may feed:

rich service telemetry,

nonessential diagnostics,

expanded UI/indicator behavior,

maintenance-only interfaces,

optional analytics.

B4 is the first bus sacrificed during serious power stress.

8. Source-combining architecture
8.1 Why source combining matters

Multiple sources sound good until they start feeding each other in dumb ways.

That is why IX-Breath does not use loose source tying.

It uses protected source combining.

8.2 Combining strategy

IX-Breath uses an ideal-diode / OR-ing stage ahead of the PMU core.

Its job is to:

block reverse current,

preserve source independence,

let valid inputs contribute without uncontrolled backfeed,

support dock recharge and optional solar entry without destabilizing the battery path.

8.3 Source-specific behavior
Source	Combine posture
P1 battery	dominant primary source under normal operation
P2 supercap	tied through controlled precharge and managed buffer interface, not blindly paralleled
P3 recharge	service path; may power controlled service modes but is not assumed during EVA
P4 rescue solar	enters through controlled MPPT/interface stage and only contributes when conditions support it
8.4 Explicit rejection

The power spine does not assume:

ambient RF harvesting,

vibration harvesting,

triboelectric harvesting,

piezo harvesting,

or any other low-density scavenging scheme as a meaningful source in this architecture.

Those are out.

9. Battery architecture
9.1 Role of the battery

The battery is the real backbone.

Everything else is support.

The battery must therefore be sized and managed as the dominant energy reservoir for:

nominal operation,

initial rescue entry,

eclipse or no-solar cases,

and ugly transient conditions where rescue solar is irrelevant.

9.2 Battery posture

The battery subsystem includes:

main pack,

BMS,

contactor / isolation behavior,

pack voltage sensing,

current sensing,

temperature sensing,

controlled disconnect behavior,

integration with the PMU and fault manager.

9.3 Battery safety rule

The battery must be able to protect itself without depending entirely on rich supervisory software.

That means certain pack-protection functions remain local and hard-defended.

10. Supercapacitor pulse-buffer architecture
10.1 Why the pulse buffer exists

Suit systems are not gentle constant loads.

Blowers, valves, pumps, startup converters, and fault transitions can all create ugly transients.

The supercap stage exists to:

absorb inrush stress,

reduce rail sag,

support short-duration pulses,

reduce the chance that a brief spike becomes a life-support reset event.

10.2 What the pulse buffer is not

It is not the primary energy reservoir.
It is not a fake extra battery.
It is not an excuse to undersize the battery.

It is a survivability stabilizer.

10.3 Buffer integration rule

The supercap stage must enter the system through controlled precharge and must be protected by a defined isolation path.

Blindly slamming a large capacitance onto the bus is forbidden.

11. Precharge and startup behavior
11.1 Why precharge matters

Without precharge, buffered buses can produce destructive or destabilizing inrush.

That is unacceptable in a life-support pack.

11.2 Startup sequence posture

The intended startup order is:

verify pack-safe conditions,

energize minimal control path,

precharge buffered rails,

verify rail stabilization,

bring up B1,

bring up B2,

qualify minimum sensing,

bring up B3 only if conditions permit,

bring up B4 last.

11.3 Startup rule

If B1 or B2 cannot be qualified cleanly, the system should not proceed into full nominal mode just because downstream buses are technically energizable.

False startup confidence is worse than delayed startup.

12. Rail segmentation and electrical hygiene
12.1 Why segmentation matters

The same pack contains:

pulsed loads,

quiet analog sensing,

control logic,

switching conversion,

and safety-critical alerting.

If those live on one dirty rail, the design is unserious.

12.2 Segmentation posture

At minimum, IX-Breath uses:

one protected critical life-support rail family,

one protected control/sensing rail family,

one recovery/extension rail family,

one noncritical rail family.

12.3 Quiet sensing rule

Critical sensor front-end power must be isolated or tightly filtered from:

blower motor noise,

pump switching noise,

electrolyzer power-stage noise,

PMU switching spikes,

and transient dump events.

12.4 Grounding posture

The design inherits a single-return / star-ground discipline wherever practical.

The goal is simple:

keep quiet references quiet,

keep noisy returns local,

stop power hardware from polluting life-critical sensing.

13. Load-shedding hierarchy
13.1 Load-shedding statement

When power is stressed, IX-Breath does not “try its best” vaguely.

It sheds in a defined order.

13.2 Shedding order

The intended order is:

B4 noncritical support

selected B3 recovery / extension loads

remaining nonessential B3 loads

thermal and regeneration functions that do not immediately threaten crew survival

only then evaluate deeper survival-only fallback posture

13.3 Protected functions

The following are intended to survive the longest:

one viable gas-circulation path,

essential gas-state sensing,

essential O2 delivery actuation,

minimum supervisory logic,

minimum crew alerting,

event/fault latching.

13.4 Important constraint

Load shedding is not allowed to become self-sabotage.

Example:
if shedding a thermal function would guarantee short-term overheating of B1/B2, that shed action is not actually protective.

That logic must be evaluated by S7.

14. Safe-dump architecture
14.1 Why safe dump exists

Stored electrical energy is not harmless just because it is helpful.

The battery, supercap stage, and energized buses can all become hazards during:

maintenance,

major faults,

stuck power-stage behavior,

partial shorts,

or damaged wiring states.

14.2 Dump-path function

The safe-dump path exists to:

bleed stored bus energy intentionally,

reduce post-fault electrical hazard,

enable safer service access,

prevent dangerous retained charge on buffered rails.

14.3 Dump rule

The dump path must be:

controlled,

deliberate,

visible to the control layer if possible,

and unavailable for accidental activation during normal life-support demand.

15. Recharge and service power path
15.1 Service path role

The recharge path is not just for filling the battery.

It is also part of the seriousness posture of IX-Breath.

It supports:

battery recharge,

controlled service mode,

fault log extraction,

diagnostics,

readiness workflows.

15.2 Service path rule

The recharge path must not create a backdoor that defeats source isolation, bypasses battery protection, or energizes unsafe internal states without control qualification.

16. Rescue solar augmentation path
16.1 Why solar is included at all

Solar is included because it can materially help some rescue cases.

It is not included because it solves all rescue cases.

16.2 Allowed solar role

The rescue solar path may:

reduce battery depletion rate in sunlit rescue cases,

support selected B3 functions,

help preserve battery state,

potentially extend survivability time under favorable geometry.

16.3 Disallowed solar role

The rescue solar path may not be used to claim:

guaranteed survival extension regardless of lighting,

eclipse immunity,

“no battery changes ever” rhetoric,

or independence from battery sizing.

16.4 Solar integration rule

The solar path must enter through a controlled interface stage, with:

source qualification,

controlled MPPT or equivalent regulated intake,

isolation from backfeed,

clear ability to be ignored if it misbehaves.

17. Fault philosophy
17.1 Power-fault philosophy

The electrical architecture assumes faults will happen.

Its job is not to prevent every fault from ever existing.
Its job is to stop one fault from taking the whole pack down.

17.2 Power-fault classes

At architecture level, S6 recognizes these major fault families:

Fault class	Example
F1	branch overload / short
F2	bus sag / brownout risk
F3	converter failure
F4	battery overcurrent / undervoltage / overtemperature
F5	supercap interface fault
F6	sensor rail contamination / noisy measurement environment
F7	stuck load or runaway actuator demand
F8	source combine anomaly / backfeed attempt
F9	recharge path fault
F10	rescue solar path instability or invalid input
17.3 Intended fault response posture
Fault class	Primary response posture
F1	isolate branch, preserve upstream buses
F2	shed B4 first, then B3 as needed
F3	isolate failed converter stage and preserve unaffected buses
F4	invoke pack-protect behavior, notify S7, preserve minimum survivable posture if physically possible
F5	isolate supercap stage if it becomes a liability
F6	preserve alert-worthy sensing if possible, flag degraded confidence explicitly
F7	cut or limit faulted load path, prevent repeated brownout cycle
F8	block source path, log anomaly, continue on remaining valid sources
F9	isolate service input and prevent contamination of on-pack operation
F10	drop solar path cleanly with no survival dependence
18. Power-state model

The power spine supports the global IX-Breath state machine, but it also has its own internal electrical posture.

18.1 Nominal electrical state

battery primary

supercap buffered

B1 and B2 active

B3 active as required

B4 active

solar ignored unless valid and useful

18.2 Conserve electrical state

B4 reduced or partially shed

B3 scrutinized hard

high-draw optional functions reduced

transient margins protected more aggressively

18.3 Rescue electrical state

B4 largely off

B3 only enabled when its survival payoff justifies it

solar input may be accepted if valid

supercap reserved for stability, not showmanship

logging remains minimal but alive if possible

18.4 Critical electrical state

only minimum survivable buses remain

B3 may be cut entirely

B1 and B2 are defended as long as physically possible

fault latches preserved

crew alerted clearly

18.5 Safe-Hold electrical state

the system enters a latched protected posture

invalid auto-reentry is prevented

only bounded minimum functions remain energized if safe

19. Power allocation posture

This repo does not yet lock final watt numbers.

It does lock the allocation logic.

19.1 Allocation rule

Power must be budgeted in this order:

breathing-loop viability,

essential sensing and supervisory coherence,

thermal protection needed to preserve the above,

oxygen-regeneration extension,

nonessential functions.

19.2 Forbidden budgeting habit

The design may not claim impressive rescue endurance by budgeting regeneration or convenience features first and assuming life-support essentials will “probably still fit.”

That is dishonest budgeting.

20. Interface to S7 supervisory control
20.1 Why this interface matters

The PMU should not be a black box and S7 should not be blind.

They must cooperate without becoming mutually fragile.

20.2 S6 to S7 signals

S6 is expected to provide S7 with at least:

battery state,

bus voltages,

source validity,

converter health,

branch fault indicators,

precharge status,

dump-path status,

solar-input validity,

power-derate flags.

20.3 S7 to S6 commands

S7 is expected to command at least:

bus enable / disable by tier,

load shed actions,

rescue-solar acceptance policy,

safe-dump request in service/fault conditions,

precharge sequencing,

fault acknowledgement / state latch handling.

20.4 Control independence rule

Even with this integration, certain protection actions must remain local to S6/BMS hardware so that a bad supervisory decision cannot destroy the pack.

21. Interface to other subsystems
21.1 S1 Life-Support Core

Receives the highest-priority rail support and minimum-sag protection.

21.2 S2 Oxygen Storage and Delivery

Receives protected power for metering, sensing, and reserve actuation.

21.3 S3 Regenerative Extension

Receives controlled power only when survival math supports it.

21.4 S4 Water Recovery and Conditioning

Receives power according to whether water handling is survival-essential in the current state.

21.5 S5 Thermal Survivability

Receives power according to whether heat management is needed to prevent immediate degradation of higher-priority functions.

22. Internal zoning implications

Because packaging is locked, the power spine must obey placement discipline.

22.1 Preferred physical placement

battery low and close to body plane

PMU near battery but thermally monitored

supercap close enough to critical distribution to be effective

quiet sensor-rail hardware separated from noisy switching hardware

service access to isolation and dump features preserved

22.2 Forbidden placement habits

large buffered energy volumes placed far aft for convenience

quiet sensor conditioning routed through noisy power bay

solar interface electronics buried in inaccessible locations

dump path hidden behind destructive service access

23. Verification posture

This repository phase does not claim the power spine is already tested.

It does define how seriousness will later be judged.

23.1 Future verification themes

The power spine should eventually be evaluated for:

startup and precharge stability,

transient ride-through,

branch-fault containment,

bus-sag behavior under pulsed loads,

correct load-shedding order,

sensor-rail cleanliness under heavy switching activity,

safe-dump effectiveness,

rescue-solar graceful acceptance and rejection,

minimum logging survival during deep derate.

23.2 Seriousness rule

A future test plan must try to break the power spine in ugly ways.

If it only proves happy-path operation, it is not serious enough.

24. Locked design decisions from this document

The following electrical decisions are now locked unless later evidence forces revision:

Battery is the real primary source.

Supercap buffer is mandatory as a transient stabilizer.

Source combining must block backfeed.

Precharge is mandatory.

B1/B2/B3/B4 bus tiers are mandatory.

Life-support loads sit ahead of convenience loads.

Quiet sensing must be electrically protected from noisy power hardware.

Safe-dump capability is mandatory.

Rescue solar, if present, is conditional supplemental power only.

Certain electrical protections remain local and hard-defended, not software-only.

25. What this document deliberately does not claim

This document does not claim:

final voltage levels,

final converter topologies,

final battery chemistry,

final pack energy,

final solar wattage,

final wire gauges,

final connector families,

final thermal losses,

final fault-clear times.

Those are downstream engineering details.

What this document does claim is narrower and more important:

IX-Breath now has a coherent survivability-grade electrical architecture with explicit source logic, bus priority, transient control, and fault posture.

That is enough for this stage.

26. What this file forces next

Because the power spine is now defined, the next commits must show:

how the life-support core actually uses this electrical backbone,

how thermal and water/O2 regeneration loads are coordinated,

how fault states and mode transitions command shedding and recovery,

how mass/power/consumables analysis will judge whether the architecture is honest.

That is exactly the pressure this file is supposed to create.
