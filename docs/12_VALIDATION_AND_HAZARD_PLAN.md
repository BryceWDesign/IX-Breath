# IX-Breath Validation Plan and Hazard / FMEA Architecture

**Document ID:** IXB-VAL-12  
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
- `docs/11_MODEL_BASELINE.md`

---

## 1. Purpose

This document defines the IX-Breath validation posture and the baseline hazard / FMEA architecture.

Its job is to answer three questions:

1. **What would have to be tested or analyzed before any serious person should trust this concept?**
2. **What are the major hazards and failure modes that can kill or cripple the architecture?**
3. **What evidence would move IX-Breath from “serious architecture study” to “disciplined prototype program candidate”?**

This document is intentionally skeptical.

That is correct.

A survival architecture should be challenged much harder than it is praised.

---

## 2. Validation philosophy

IX-Breath uses one central validation rule:

> **Every meaningful claim must have an attack path.**

That means the repository does not only ask:
- can the system work,
- can the subsystem work,
- can the math be made to close.

It also asks:
- how does it fail,
- how early does it fail,
- how obviously does it fail,
- and can it fail in ways that mislead the operator before it fails physically.

That is why this document combines:
- verification planning,
- validation planning,
- hazard analysis,
- and FMEA structure.

Those are not separate “later paperwork.”
They are the seriousness backbone.

---

## 3. Validation posture

IX-Breath is still an architecture study.

So the correct validation posture is staged.

| Stage | Meaning |
|---|---|
| V0 | Architecture review and closure only |
| V1 | Analytical verification and subsystem bench validation |
| V2 | Integrated breadboard / engineering model validation |
| V3 | Environmental and abuse-case validation on integrated test article |
| V4 | Mission-specific prequalification planning |

IX-Breath is currently at **V0 with some defined V1 expectations**.

It is not pretending to be farther along.

---

## 4. What counts as evidence

The repository recognizes five evidence types.

| Evidence type | Meaning |
|---|---|
| **A** | analysis |
| **I** | inspection |
| **T** | test |
| **D** | demonstration |
| **R** | review of objective evidence / records |

No later README or summary is allowed to blur those together.

A simulated result is not a bench test.
A bench test is not an environmental qualification.
A citation is not an endorsement.

---

## 5. Validation strategy by claim class

IX-Breath groups claims into six claim classes.

| Claim class | Example | Minimum serious evidence path |
|---|---|---|
| C-1 Architecture coherence | subsystem flows and interfaces make sense | A + I |
| C-2 Resource closure | 24 hr defended case closes analytically | A |
| C-3 Control correctness | FDIR transitions behave as designed | A + D + T |
| C-4 Subsystem viability | blowers, PMU, PEM, valves, sensors operate credibly | T |
| C-5 Integrated survivability | system survives realistic off-nominal sequences | T + D |
| C-6 Readiness / sustainment credibility | logs, blockers, and maintenance evidence prevent quiet degradation | R + D + T |

### 5.1 Important rule

A higher claim class cannot be supported by lower-class evidence alone.

Example:
A neat subsystem demo does not prove integrated survivability.

---

## 6. Validation sequence

The intended validation order is:

1. **architecture and model review**
2. **single-subsystem bench checks**
3. **power-spine abuse and fault testing**
4. **life-support loop test rig**
5. **water / PEM coupling rig**
6. **thermal/resource coupling rig**
7. **integrated non-crew closed-loop pack breadboard**
8. **fault-injection campaign**
9. **service/readiness workflow validation**
10. **environmental and integration planning**

That order is not arbitrary.

It forces the hardest credibility issues early:
- power survival,
- oxygen continuity,
- regeneration honesty,
- fault posture,
- and operator clarity.

---

## 7. Validation article ladder

IX-Breath defines the following validation article families.

| Article ID | Article | Purpose |
|---|---|---|
| VA-1 | Analytical reference set | equations, input bands, and case closure review |
| VA-2 | Power-spine bench rig | precharge, ride-through, shedding, dump path, branch faults |
| VA-3 | Gas-loop bench rig | blower continuity, valve routing, CO2 lane behavior, sensor disagreement |
| VA-4 | Water/regen bench rig | water qualification, PEM permission logic, vent handling, deny logic |
| VA-5 | Thermal coupling rig | hotspot growth, derate ladder, shell rejection assumptions |
| VA-6 | Integrated backpack electronics mock | packaging, routing, control-zone contamination, fault latching |
| VA-7 | Integrated nonhuman closed-loop engineering article | full-stack survival logic without human-in-the-loop dependence |
| VA-8 | Service/readiness simulation harness | logs, blockers, inspection workflows, calibration evidence |

### 7.1 Important note

The architecture does **not** jump straight to a “final prototype.”

That would be unserious.

---

## 8. Validation objectives

The repository currently recognizes these core validation objectives.

| Objective ID | Objective |
|---|---|
| VO-1 | Prove the power spine does not collapse under realistic transient and branch-fault stress |
| VO-2 | Prove the life-support loop remains coherent when one path is lost or degraded |
| VO-3 | Prove regeneration permission logic denies operation correctly when it should |
| VO-4 | Prove regeneration cannot silently steal survival margin from B1/B2 |
| VO-5 | Prove thermal ladder and resource gate interact conservatively |
| VO-6 | Prove reserve oxygen logic remains isolated until justified |
| VO-7 | Prove state transitions are bounded, latched where needed, and understandable |
| VO-8 | Prove service/readiness logic catches blocked states before EVA |
| VO-9 | Prove the integrated system does not hide degraded confidence from the crew |
| VO-10 | Prove stretch-case claims collapse appropriately when favorable assumptions disappear |

---

## 9. Validation matrix

### 9.1 Top-level matrix

| Validation ID | Claim under attack | Method | Primary article | Pass posture |
|---|---|---|---|---|
| VAL-001 | 24 hr defended case closes analytically | A | VA-1 | model inputs and equations remain internally consistent and conservative |
| VAL-002 | B1/B2 survive branch faults and transient events | T | VA-2 | no single B3/B4 fault takes down protected buses |
| VAL-003 | precharge and dump path behave safely | T | VA-2 | no damaging inrush or unsafe residual charge behavior |
| VAL-004 | blower redundancy and reroute posture are viable | T | VA-3 | one-path loss still preserves one breathing path |
| VAL-005 | sensor disagreement reduces confidence correctly | T + D | VA-3 | disagreement is detected, logged, and changes state freedom |
| VAL-006 | reserve oxygen remains isolated until allowed trigger | T + D | VA-3 / VA-7 | reserve path is not silently depleted |
| VAL-007 | regeneration deny logic works under weak power or thermal margin | T | VA-4 / VA-5 | PEM remains off when the full gate does not pass |
| VAL-008 | water-quality gate blocks false feedstock credit | T | VA-4 | only qualified water produces regeneration credit |
| VAL-009 | thermal ladder forces derate before hidden control loss | T | VA-5 | system reduces function before hot-zone drift becomes unsafe |
| VAL-010 | watchdog can override unsafe supervisory behavior | T + D | VA-6 / VA-7 | invalid transitions or contradictions force conservative posture |
| VAL-011 | rescue-solar path helps when valid and drops cleanly when not | T | VA-2 / VA-5 | solar instability never contaminates core survival behavior |
| VAL-012 | integrated state machine resists thrash and false recovery | T + D | VA-7 | bounded transitions, latches, and guarded recovery hold |
| VAL-013 | service/readiness workflow catches blocked pack before EVA | D + R | VA-8 | blocked calibration / life-limited components produce no-go posture |
| VAL-014 | 36–48 hr stretch cases collapse honestly when assumptions worsen | A + T | VA-1 / VA-7 | stretch claims only survive under stated favorable conditions |

---

## 10. Acceptance posture

IX-Breath uses qualitative acceptance posture at this repo stage.

### 10.1 Acceptance bands

| Band | Meaning |
|---|---|
| AP-0 | fail clearly |
| AP-1 | works only in a happy-path demo; not serious |
| AP-2 | partially credible but major hazards or contradictions remain unresolved |
| AP-3 | serious engineering-model candidate; remaining risks are explicit and bounded |
| AP-4 | strong candidate for environmental prequalification planning |

### 10.2 Current target

This repository is trying to define a path to **AP-3**, not pretend it already achieved it.

That distinction stays locked.

---

## 11. Hazard analysis philosophy

IX-Breath uses one central hazard rule:

> **The most dangerous failure is often not immediate loss of function. It is hidden loss of trust while the pack still appears to be working.**

That means the hazard analysis must include:
- hard failures,
- soft failures,
- misleading failures,
- state-estimation failures,
- and service/readiness failures.

This is why IX-Breath treats:
- confidence,
- latches,
- watchdog intervention,
- and readiness blockers

as actual safety architecture rather than paperwork.

---

## 12. Hazard severity posture

The repo uses a qualitative severity scale.

| Severity | Meaning |
|---|---|
| S1 | nuisance / non-safety annoyance |
| S2 | reduced mission capability / manageable degradation |
| S3 | serious degradation requiring immediate corrective posture |
| S4 | crew survival risk if not corrected quickly |
| S5 | potentially catastrophic crew survival consequence |

### 12.1 Likelihood posture

At this stage, likelihood is treated qualitatively:

| Likelihood | Meaning |
|---|---|
| L1 | unlikely / architecture-level remote |
| L2 | possible |
| L3 | credible and expected to require direct attention in validation |
| L4 | highly credible without strong controls |

### 12.2 Risk priority posture

Risk is not given a fake exact RPN at this repo stage.
Instead, hazards are prioritized by:
- severity,
- detectability difficulty,
- and whether they can mislead the operator.

That is more honest at architecture maturity.

---

## 13. Hazard families

The architecture recognizes eight major hazard families.

| Hazard family | Meaning |
|---|---|
| H-1 | oxygen continuity hazard |
| H-2 | CO2-control hazard |
| H-3 | pressure / leak hazard |
| H-4 | electrical continuity hazard |
| H-5 | thermal survivability hazard |
| H-6 | regeneration/water-handling hazard |
| H-7 | control / confidence / state-estimation hazard |
| H-8 | sustainment / readiness hazard |

---

## 14. Top hazard register

### 14.1 Major hazards

| Hazard ID | Hazard | Severity | Primary detection posture | Primary mitigation posture |
|---|---|---|---|---|
| HZ-001 | primary oxygen path loss | S5 | pressure + O2 state + path feedback | preserve loop, guarded reserve policy, escalate state |
| HZ-002 | reserve oxygen unavailable when needed | S5 | actuation/path health + state logic | reserve-path health checks, latching, guarded triggers |
| HZ-003 | CO2-control degradation not recognized early | S5 | CO2 sensing + confidence model | conservative alerts, simplified fallback, service blockers |
| HZ-004 | gas leak or abnormal pressure loss | S5 | pressure trend and routing consistency | isolate what can be isolated, escalate, preserve oxygen logic |
| HZ-005 | B1 or B2 collapse from branch fault or brownout | S5 | bus health + local protections | segmentation, precharge, branch protection, shedding |
| HZ-006 | PMU or converter heat drives hidden control degradation | S4 | thermal sensing + control confidence | thermal zoning, derate ladder, Safe-Hold pressure |
| HZ-007 | PEM runs when it is not affordable | S4 | resource gate cross-checks | deny logic, B3 isolation, conservative bias |
| HZ-008 | water falsely credited as PEM feedstock | S4 | water class and conditioning evidence | qualification gate, deny if uncertain |
| HZ-009 | watchdog and supervisor disagree on safe action | S4 | direct state/control contradiction | confidence collapse handling, escalate, latch |
| HZ-010 | sensor disagreement hidden from crew | S4 | sensor voting + confidence tracking | degraded-confidence alerts, deny rich optimization |
| HZ-011 | thermal ladder too slow, causing late derate | S4 | hot-zone trends + response timing | earlier thresholds, buffer, validation attack cases |
| HZ-012 | service/readiness gap lets degraded pack leave airlock | S5 | S9 blocker logic + record review | go/no-go rules, calibration evidence, maintenance workflow |

---

## 15. FMEA posture

### 15.1 Why FMEA is included now

Because IX-Breath is making survival claims, even bounded ones.
That means the repo should not wait until the end to ask how parts and functions fail.

### 15.2 FMEA structure

The baseline FMEA fields are:

| Field | Meaning |
|---|---|
| Item / function | what is supposed to happen |
| Failure mode | how it can fail |
| Local effect | what that failure does locally |
| System effect | what it does to survivability |
| Detection means | how the system notices |
| Containment / mitigation | what the architecture does about it |
| Residual concern | what still remains dangerous |
| Validation target | what test or analysis should attack it |

### 15.3 Important rule

A mitigation is not accepted just because it exists on paper.

If it is safety-relevant, it must eventually have a validation target.

---

## 16. Representative FMEA entries

### 16.1 Blower path failure

| Field | Entry |
|---|---|
| Item / function | S1 circulation blower path |
| Failure mode | one blower path stalls or degrades |
| Local effect | reduced or lost circulation on one path |
| System effect | breathing viability threatened if no alternate path remains |
| Detection means | blower command/response mismatch, flow proxy, pressure behavior |
| Containment / mitigation | reconfigure to surviving blower path, shed lower loads, alert crew |
| Residual concern | surviving path may still be overloaded or partially degraded |
| Validation target | VA-3 one-path loss test |

### 16.2 Reserve oxygen actuation failure

| Field | Entry |
|---|---|
| Item / function | S2 isolated reserve oxygen path |
| Failure mode | reserve cannot open or cannot regulate correctly |
| Local effect | last-line oxygen continuity unavailable or unstable |
| System effect | catastrophic survival risk if primary path is already lost |
| Detection means | path health checks, actuation feedback, pressure response |
| Containment / mitigation | guarded diagnostics, latch on reserve-path fault, service blocker |
| Residual concern | no true substitute if reserve path is physically unavailable |
| Validation target | VA-3 / VA-7 reserve trigger and actuation fault campaign |

### 16.3 PEM affordability-control failure

| Field | Entry |
|---|---|
| Item / function | S3 regeneration permission logic |
| Failure mode | PEM enabled despite insufficient power or thermal margin |
| Local effect | extra load and heat injected into stressed system |
| System effect | can shorten survival instead of extending it |
| Detection means | gate contradiction, bus sag, thermal ladder rise, watchdog mismatch |
| Containment / mitigation | deny by default, watchdog override, B3 cutout, state escalation |
| Residual concern | repeated oscillation or late denial could still consume margin |
| Validation target | VA-4 / VA-5 / VA-7 gate-violation tests |

### 16.4 Sensor-confidence collapse

| Field | Entry |
|---|---|
| Item / function | S7 confidence model for O2/CO2/pressure |
| Failure mode | disagreement exists but is not escalated correctly |
| Local effect | wrong confidence posture |
| System effect | unsafe optimization or delayed transition |
| Detection means | cross-domain inconsistency, watchdog checks, logged contradictions |
| Containment / mitigation | confidence bands, degraded alerts, latched conservative state |
| Residual concern | if multiple channels fail coherently, detection is harder |
| Validation target | VA-3 / VA-6 / VA-7 disagreement injection |

---

## 17. Validation attack themes

A serious validation plan does not just test whether functions work.
It attacks where the architecture is most likely to lie to itself.

### 17.1 Required attack themes

1. **good-looking bad state**
   - system appears healthy while confidence is actually degraded

2. **partial fault with misleading recovery**
   - one path recovers enough to look okay, but not enough to be trusted

3. **resource contradiction**
   - solar, battery, thermal, and water inputs tell conflicting stories

4. **load-shed inversion**
   - noncritical load survives at the expense of core survival

5. **reserve misuse**
   - reserve depletes silently or triggers badly

6. **service escape**
   - pack should be blocked from EVA but slips through weak readiness logic

These are the kinds of failures that make professionals roll their eyes if they are missing.

So they are not missing.

---

## 18. Minimum serious test campaign

At a minimum, a serious phase-1 engineering campaign should eventually include:

| Campaign ID | Campaign | Purpose |
|---|---|---|
| TC-1 | power abuse and brownout campaign | prove bus segmentation and shedding behavior |
| TC-2 | gas-loop degraded-path campaign | prove closed-loop survival under one-path loss |
| TC-3 | sensor disagreement and confidence campaign | prove explicit degradation behavior |
| TC-4 | reserve oxygen guarded-trigger campaign | prove reserve protection and actuation logic |
| TC-5 | PEM deny/allow campaign | prove resource gate discipline |
| TC-6 | thermal ladder campaign | prove staged derate before unsafe drift |
| TC-7 | watchdog conflict campaign | prove safety supervisor authority |
| TC-8 | service blocker campaign | prove readiness logic can actually stop a bad pack from going out |
| TC-9 | integrated rescue timeline campaign | prove modeled state/resource transitions behave coherently over time |

---

## 19. Exit criteria for “serious engineering-model candidate”

IX-Breath should not claim to be a serious engineering-model candidate until all of the following are true:

1. architecture and model remain internally consistent
2. no single B3/B4 failure can trivially kill B1/B2
3. reserve oxygen protection logic is demonstrably bounded
4. regeneration deny logic works conservatively
5. watchdog intervention is real, not decorative
6. thermal/resource contradictions are handled explicitly
7. service/readiness blockers are functional
8. hazard register and FMEA are not contradicted by observed behavior
9. unresolved risks are explicit and bounded

That is the minimum bar.

---

## 20. What this validation plan would prove if executed well

If the validation path defined here were executed well, the result would **not** prove:
- flight qualification,
- NASA endorsement,
- or mission readiness.

What it **would** prove is something narrower and still valuable:

- IX-Breath is not just a pretty architecture
- its main claims survive attack
- its failure logic is real
- and its stretch claims are bounded by evidence instead of imagination

That is the right target.

---

## 21. Locked design decisions from this document

The following are now locked unless later evidence forces revision:

1. **Every meaningful IX-Breath claim must have an explicit attack path.**
2. **Validation must include misleading partial-failure cases, not only hard failures.**
3. **Reserve oxygen and regeneration logic are first-class validation targets.**
4. **Watchdog/supervisor disagreement is itself a major safety event.**
5. **Readiness / sustainment failures are treated as real hazards, not paperwork issues.**
6. **Stretch-case claims must be attacked under worsening assumptions.**
7. **A serious validation campaign must include power, gas-loop, thermal, control, and service workflows.**
8. **The repo aims for disciplined engineering-model candidacy, not fake flight maturity.**
9. **Hazard analysis and FMEA are part of the design, not an afterthought.**
10. **The final README must summarize the project without claiming any validation that has not actually occurred.**

---

## 22. What this document deliberately does not claim

This document does **not** claim:

- a completed test campaign,
- closed hazard risk,
- full probabilistic safety analysis,
- final environmental qualification criteria,
- or final certification readiness.

What it **does** claim is the critical thing for this stage:

**IX-Breath now has a serious validation and hazard architecture that makes clear what would have to be proven, what can fail, how it can fail, and how the repo intends to attack those failure modes honestly.**
