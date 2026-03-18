# IX-Breath Fresh Real-World BOM

**Document ID:** IXB-BOM-06  
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

---

## 1. Purpose

This document defines the first serious **real-world bill of materials posture** for IX-Breath.

The point is not to fake a flight-certified procurement package.

The point is to do the adult engineering thing:

- identify the actual hardware classes the architecture depends on,
- separate credible subsystem choices from fantasy,
- distinguish prototype-facing selection from flight-direction intent,
- and make later mass/power/thermal analysis possible.

If a subsystem cannot be represented here by a believable hardware class, it is not mature enough to claim space in IX-Breath.

---

## 2. BOM philosophy

IX-Breath is a **rescue-biased regenerative PLSS architecture study**.

That means the BOM must obey five rules:

1. **Real mechanism first**  
   Every line item must correspond to a defensible physical function.

2. **No hidden magic**  
   Oxygen continuity, power continuity, and thermal control must come from named hardware classes, not vague system words.

3. **No false procurement theater**  
   This document will not pretend final flight vendors are already chosen.

4. **Prototype-facing and flight-direction are different**  
   Ground breadboard / engineering-model hardware and eventual flight-direction hardware are not the same thing.

5. **Rejected donor claims stay rejected**  
   No Element 115, no zero-point, no ambient RF primary power, no perpetual-motion nonsense, no stealth-field filler.

---

## 3. BOM maturity bands

Each line item is tagged by maturity posture.

| Band | Meaning |
|---|---|
| **B1** | Standard real-world component class or industrial hardware family useful for architecture, bench work, or engineering model development. |
| **B2** | Aerospace-relevant or mission-relevant hardware class that may require adaptation, custom packaging, or environmental hardening. |
| **B3** | Custom subsystem or integrated assembly that is physically credible but would need dedicated design, qualification, and testing. |

This keeps the repo honest.

If too much of IX-Breath lands in B3, the project starts looking like a wish list instead of an architecture.

---

## 4. Selection vocabulary

| Tag | Meaning |
|---|---|
| **Required** | Baseline architecture depends on this class of hardware. |
| **Preferred** | Strong baseline choice, but alternates could exist. |
| **Optional** | May improve the architecture but is not required for phase 1. |
| **Deferred** | Plausibly useful later, but not adopted into the phase-1 baseline. |

---

## 5. Top-level BOM summary by subsystem

| Subsystem | BOM posture |
|---|---|
| S1 Life-Support Core | dominated by gas handling, blower, CO2/humidity control, valves, sensors, and manifolds |
| S2 Oxygen Storage and Delivery | dominated by pressure vessels, regulators, isolation valves, check valves, and pressure sensing |
| S3 Regenerative Extension | dominated by PEM electrolysis stack, power electronics, hydrogen vent handling, and oxygen-side conditioning |
| S4 Water Recovery and Conditioning | dominated by condensate separation, storage, filtration/conditioning, and routing |
| S5 Thermal Survivability | dominated by cold plate / heat transfer path, heat exchanger, thermal sensors, insulation, and controlled rejection aids |
| S6 Power Continuity Spine | dominated by battery, pulse buffer, PMU, ideal-diode / OR-ing, branch protection, precharge, dump path, and isolated rails |
| S7 Supervisory Control / FDIR | dominated by supervisory MCU/SBC, watchdog, data logging, isolated I/O, alerts, and mode controls |
| S8 Structural Survivability Shell | dominated by shell panels, partitions, mounting frames, puncture/impact mitigation liner, and protected routing hardware |
| S9 Sustainment and Readiness | dominated by software/data structures and service-side tooling, not backpack mass |

---

## 6. Real-world BOM baseline table

### 6.1 S1 — Life-Support Core

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S1-001 | Dual redundant brushless circulation blowers | maintain closed-loop gas circulation | B2 | Required | compact high-reliability blower pair with independent drive paths | dual path supports fail-operational posture for circulation |
| IXB-S1-002 | CO2 removal cartridge assembly | remove CO2 from closed loop | B2 | Required | RCA-style amine swing bed or equivalent backpack-class CO2 control lane | baseline favors real suit-relevant lane, not dive scrubber copy-paste |
| IXB-S1-003 | Humidity removal / condensate interface block | remove water from loop and feed S4 | B2 | Required | integrated condensing interface or humidity extraction stage | must connect honestly into water accounting |
| IXB-S1-004 | Breathing-gas manifold block | distribute return gas, conditioned gas, and O2 make-up | B3 | Required | custom manifold assembly in low-leak metallic or high-grade polymer/metal hybrid | short internal gas path is a package driver |
| IXB-S1-005 | Oxygen partial-pressure sensing set | monitor breathable gas condition | B2 | Required | triplex O2 sensing with voting / cross-check logic | sensor diversity preferred later if practical |
| IXB-S1-006 | CO2 sensing set | monitor scrubber performance and crew hazard state | B2 | Required | dual CO2 sensors with plausibility checks | needed for crew alerting and FDIR |
| IXB-S1-007 | Humidity sensing set | support condensate accounting and loop control | B1 | Required | dual humidity sensors in conditioned locations | supports S4 and thermal logic |
| IXB-S1-008 | Loop pressure transducers | monitor loop pressure and distribution health | B2 | Required | redundant pressure transducers with protected sensing lines | critical for leak/fault detection |
| IXB-S1-009 | Isolation / bypass valve set | isolate failed sections and support fallback gas routing | B2 | Required | latching or normally-safe valve set with manual override posture | key FDIR item |
| IXB-S1-010 | Trace contaminant placeholder volume | future-compatible contaminant control slot | B3 | Deferred | reserved package space only in phase 1 | do not pretend final chemistry is chosen yet |

### 6.2 S2 — Oxygen Storage and Delivery

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S2-001 | Primary oxygen pressure vessel set | nominal stored O2 supply | B2 | Required | compact composite pressure vessel pair or equivalent pressure-rated architecture | exact vessel geometry left open for later packaging study |
| IXB-S2-002 | Isolated emergency oxygen vessel | protected last-line oxygen reserve | B2 | Required | separately valved and physically isolated reserve vessel | may be one independent bottle or hard-isolated chamber |
| IXB-S2-003 | Primary regulator stage | reduce primary storage pressure to controlled delivery pressure | B2 | Required | dual-stage or functionally equivalent regulated path | stable make-up control required |
| IXB-S2-004 | Emergency regulator stage | provide simplified reserve oxygen delivery | B2 | Required | independent regulator path with minimal dependency chain | emergency logic must be simpler than nominal optimization |
| IXB-S2-005 | Check valve and backflow isolation set | prevent backflow and reserve contamination | B1 | Required | oxygen-compatible valve set | simple part, high consequence |
| IXB-S2-006 | Oxygen-rated shutoff / selector valve set | isolate primary vs emergency flow paths | B2 | Required | latching or protected selection valves | supports reserve protection policy |
| IXB-S2-007 | Pressure sensing package | track tank and regulated pressure | B2 | Required | redundant pressure sensors across primary and reserve lines | needed for state estimation |
| IXB-S2-008 | Oxygen-compatible tubing and fittings | carry regulated oxygen safely | B1 | Required | oxygen-cleaned tubing/fittings, metal preferred in high-risk zones | materials compatibility matters |
| IXB-S2-009 | Oxygen make-up metering valve | controlled O2 injection into loop | B2 | Required | fine-metering oxygen-compatible valve | tied into S1/S7 control loop |

### 6.3 S3 — Regenerative Extension Layer

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S3-001 | PEM electrolyzer stack | generate oxygen from conditioned water | B2 | Required | compact PEM stack sized for bounded rescue extension, not full-mission fantasy | must stay subordinate to package and power reality |
| IXB-S3-002 | Electrolyzer power stage | controlled DC supply for PEM stack | B2 | Required | isolated controlled-current power electronics tied to S6 | noisy load; segregation required |
| IXB-S3-003 | Water feed pump / metering stage | meter conditioned water into PEM stack | B2 | Required | small controlled feed stage with fault detection | dry-run and blockage logic required |
| IXB-S3-004 | Gas-liquid separator | separate produced gases from residual water | B2 | Required | compact compatible separator stage | required for sane downstream routing |
| IXB-S3-005 | Oxygen-side accumulator / conditioning volume | smooth regenerated O2 before injection to storage or loop | B3 | Preferred | small buffer volume with pressure protection | avoids unstable pulsed injection behavior |
| IXB-S3-006 | Hydrogen vent path and check hardware | safely dispose of H2 byproduct | B2 | Required | controlled vent manifold with check/isolation hardware | H2 is not credited as a magical bonus fuel |
| IXB-S3-007 | Water quality sensing / permissive logic | prevent dirty water from being counted blindly | B2 | Preferred | conductivity / contamination proxy sensing with control interlock | honest accounting feature |
| IXB-S3-008 | Electrolyzer thermal interface plate | remove PEM waste heat into S5 | B2 | Required | cold-plate or conduction interface | S3 heat burden must be counted |
| IXB-S3-009 | Oxygen-side purity monitoring placeholder | future quality verification path | B3 | Deferred | reserve interface and package volume only | not needed to fake maturity in phase 1 |

### 6.4 S4 — Water Recovery and Conditioning

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S4-001 | Condensate separator | extract liquid water from humid loop | B2 | Required | compact separator compatible with S1 flow regime | ties loop to water accounting |
| IXB-S4-002 | Condensate collection reservoir | store recovered water before conditioning | B1 | Required | sealed serviceable reservoir with level sensing | real storage path required |
| IXB-S4-003 | Water conditioning cartridge | filter/condition recovered water for reuse | B2 | Required | particulate / ionic / compatibility conditioning path sized for rescue duty | exact chemistry left open |
| IXB-S4-004 | Sterile emergency make-up water reserve | guaranteed clean feed for bounded oxygen regeneration | B1 | Required | isolated clean water reserve | prevents regen claims from depending entirely on condensate success |
| IXB-S4-005 | Level sensing package | monitor recovered and reserve water state | B1 | Required | redundant level sensing or mass-estimation approach | supports survivability model |
| IXB-S4-006 | Water routing valves and tubing | direct water between recovery, conditioning, storage, and PEM | B1 | Required | chemically compatible low-leak routing set | must remain separated from quiet electronics |
| IXB-S4-007 | Drain / service interface | maintenance and inspection support | B1 | Required | simple serviceable access path | sustainment seriousness item |
| IXB-S4-008 | Water quality sampling point | enable test and maintenance evidence | B1 | Preferred | service-port sampling path | strengthens S9 readiness layer |

### 6.5 S5 — Thermal Survivability Layer

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S5-001 | Cold plate / conduction plate set | collect heat from battery, PMU, PEM, and control hotspots | B2 | Required | compact plate network bonded to major heat sources | first serious thermal backbone item |
| IXB-S5-002 | Heat transport loop or equivalent thermal transfer path | move heat to rejection region | B3 | Required | compact pumped loop or equivalent controlled transport concept | exact implementation deferred, function is not |
| IXB-S5-003 | Heat exchanger interface block | couple life-support/water/thermal burdens as required | B3 | Preferred | compact exchanger block integrated with water/humidity handling | high leverage, high packaging sensitivity |
| IXB-S5-004 | Thermal buffer element | absorb short spikes and smooth rescue-mode transitions | B2 | Preferred | PCM or equivalent bounded thermal buffer | helps derate-before-fail posture |
| IXB-S5-005 | Exterior radiator / rejection surface integration | reject heat to environment | B3 | Required | shell-integrated rejection area with honest area accounting | must fit worn envelope |
| IXB-S5-006 | Rescue-only deployable thermal aid | extend rejection capability in contingency | B3 | Optional | compact foldout/shade/radiator-assist element | only if packaging later proves credible |
| IXB-S5-007 | Temperature sensor set | monitor hot zones and thermal margins | B1 | Required | distributed temperature sensing on critical zones | supports thermal derate logic |
| IXB-S5-008 | Insulation package | reduce unwanted thermal coupling and protect reserves | B1 | Required | MLI or equivalent localized insulation strategy | especially important around reserve O2 and control bay |

### 6.6 S6 — Power Continuity Spine

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S6-001 | Primary battery pack | main stored electrical energy | B2 | Required | high-specific-energy rechargeable pack with conservative safety posture | exact chemistry not locked here |
| IXB-S6-002 | Battery management system | monitor and protect battery | B2 | Required | BMS with cell monitoring, fault reporting, and controlled disconnects | must integrate with S7 but not depend on it entirely |
| IXB-S6-003 | Supercapacitor pulse-buffer module | ride through transients and pulsed loads | B2 | Required | compact buffered module with controlled precharge | donor logic kept, fantasy claims removed |
| IXB-S6-004 | Power management unit | route, regulate, and protect power sources and loads | B3 | Required | custom PMU with segmented rails and survivability logic | central electrical architecture item |
| IXB-S6-005 | Ideal-diode / OR-ing stage | source combining without dangerous backfeed | B2 | Required | ideal-diode controller or equivalent protected OR-ing | core retained donor feature |
| IXB-S6-006 | Precharge network | manage inrush into buffered buses | B1 | Required | resistor / switch / controller precharge path | non-negotiable with supercaps and big rails |
| IXB-S6-007 | Branch protection set | protect bus segments by criticality tier | B1 | Required | fuse / electronic fuse / resettable protection mix | branch-level isolation is a survival feature |
| IXB-S6-008 | Dump resistor / safe-discharge path | safely bleed stored electrical energy | B1 | Required | controlled dump path with service-safe access | important for faults and maintenance |
| IXB-S6-009 | Isolated sensor rail converter | keep quiet sensing alive through noisy bus events | B2 | Required | low-noise isolated or tightly filtered rail | strong TunerCore carryover |
| IXB-S6-010 | Critical control rail converter | preserve S7 minimum awareness and logging | B2 | Required | hardened low-power control rail | supports safe-hold and blackbox |
| IXB-S6-011 | Rescue solar interface controller | accept optional rescue solar input | B2 | Preferred | MPPT-capable controlled input stage | rescue-only augmentation, not guaranteed power |
| IXB-S6-012 | Rescue solar blanket | supplemental sunlit power | B2 | Optional | compact high-efficiency foldout array sized for bounded support | only counts when scenario allows |
| IXB-S6-013 | Dockside / habitat recharge interface | recharge battery and support turnaround | B1 | Required | protected external service connector path | serious operations item |
| IXB-S6-014 | Reverse-polarity / TVS / surge protection | protect against wiring and transient faults | B1 | Required | distributed protection set | small parts, major seriousness |

### 6.7 S7 — Supervisory Control / FDIR / Crew Interface

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S7-001 | Supervisory controller | run state machine and command logic | B2 | Required | deterministic embedded controller with strong watchdog support | bounded logic only |
| IXB-S7-002 | Independent watchdog / safety supervisor | detect stuck/faulted primary control | B2 | Required | separate safety monitor or hardware watchdog chain | prevents single-controller wishful thinking |
| IXB-S7-003 | Blackbox event logger | preserve fault/event history | B1 | Required | nonvolatile event log store with timestamp source | strong Singularis carryover |
| IXB-S7-004 | Sensor conditioning and A/D front end | acquire critical sensor data cleanly | B2 | Required | isolated/filtered front-end modules | must survive power-noise environment |
| IXB-S7-005 | Crew alert package | warn crew of oxygen, CO2, thermal, and power states | B1 | Required | simple visual / auditory / haptic alert bundle | must remain interpretable under stress |
| IXB-S7-006 | Crew emergency controls | allow rescue entry, acknowledge alarms, invoke isolation behaviors | B1 | Required | gloved-use capable controls with guarded logic | simple and explicit |
| IXB-S7-007 | Service / maintenance data port | export logs and readiness evidence | B1 | Required | protected service interface | supports S9 |
| IXB-S7-008 | Timebase / event sequencing source | support reconstructable logs | B1 | Required | stable local timing source | useful for post-event review |
| IXB-S7-009 | Redundant power-fault latching path | preserve major-fault posture even if rich UI dies | B2 | Preferred | hardwired or low-level latch path | supports clear safe-hold posture |

### 6.8 S8 — Structural Survivability Shell

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S8-001 | Primary structural shell panels | carry loads and define worn envelope | B3 | Required | lightweight high-strength shell construction | exact material stack not finalized here |
| IXB-S8-002 | Internal partition frame set | separate hazard zones and mount subsystems | B3 | Required | metallic/composite internal support frame with isolated bays | core seriousness item |
| IXB-S8-003 | Puncture / impact mitigation liner | improve survivability around tanks and power bay | B2 | Preferred | conservative STF-based or equivalent protective liner approach | no invulnerability claims |
| IXB-S8-004 | Protected routing channels | shield gas, water, and wire runs | B2 | Required | dedicated routed channels with separation rules | supports containment and serviceability |
| IXB-S8-005 | Thermal-isolation barriers | reduce unwanted thermal migration between bays | B1 | Required | local barriers and insulation inserts | reserve O2 and control bay benefit most |
| IXB-S8-006 | Service panels / access covers | enable maintenance without destructive disassembly | B1 | Required | bounded access features sized to package discipline | sustainment seriousness |
| IXB-S8-007 | External rescue deployable mounts | stow and support optional solar/thermal rescue aids | B3 | Optional | low-profile shell-integrated mounts | only if later justified |
| IXB-S8-008 | Shock/vibration mounting set | reduce damage transfer into control and sensor bay | B1 | Preferred | localized isolators for sensitive electronics | supports survivability and data integrity |

### 6.9 S9 — Sustainment and Readiness Layer

| BOM ID | Item | Function | Band | Selection | Selection posture | Notes |
|---|---|---|---|---|---|---|
| IXB-S9-001 | Readiness state database / record model | track calibration, blockers, inspections, and life-limited parts | B1 | Required | software-side structured data model | offboard system item |
| IXB-S9-002 | Event ingestion pipeline | pull blackbox and service logs into sustainment record | B1 | Required | deterministic import path with audit trail | direct donor carryover from Sustainment OS logic |
| IXB-S9-003 | Go / no-go rules engine | derive readiness from blockers and evidence | B1 | Required | controlled transition logic | prevents “looks okay to me” operations culture |
| IXB-S9-004 | Maintenance scheduling logic | track service intervals and part replacement cadence | B1 | Required | rules-driven schedule model | supports life-limited components |
| IXB-S9-005 | Calibration evidence package | prove sensor/service status before EVA | B1 | Required | evidence records, not verbal memory | serious operations posture |
| IXB-S9-006 | Fault review and anomaly workflow | formalize post-event review and corrective action | B1 | Required | explicit workflow/state model | keeps degradation from hiding |

---

## 7. Baseline material and chemistry posture

This repository does not pretend final chemistry/material trades are closed, but it does lock the credible families.

### 7.1 Oxygen path materials
Preferred posture:
- oxygen-compatible metals and fittings in high-risk pressure zones,
- oxygen-clean assembly discipline,
- chemically compatible seals selected conservatively.

### 7.2 Water path materials
Preferred posture:
- chemically compatible tubing and reservoirs,
- serviceable conditioning cartridge architecture,
- conservative contamination control.

### 7.3 Shell and partition materials
Preferred posture:
- lightweight structural shell with localized metal reinforcement where needed,
- internal bay partitions separating power, oxygen, control, and water risks,
- no unsupported “miracle material” claims.

### 7.4 Electrical storage posture
Preferred posture:
- rechargeable battery chemistry selected for safety, packaging, and energy density trade balance,
- supercapacitors used for pulse buffering and ride-through only,
- no exotic primary power claims.

---

## 8. Explicit non-BOM items

The following are **not** allowed to sneak into IX-Breath as if they were ordinary BOM choices:

- Element 115 cores,
- zero-point regulators,
- vacuum-energy cells,
- ambient RF rectenna arrays as primary life-support power,
- triboelectric or piezoelectric primary life-support supply,
- stealth-field hardware,
- offensive/defense payloads,
- giant coil harvesters,
- undefined “energy compression” blocks.

Those are rejected by architecture and remain rejected here.

---

## 9. Why these BOM choices survived the donor filter

This BOM exists because certain donor patterns were useful **after** their mythology was removed.

### 9.1 Donor pattern carryover into S6
Retained from donor logic:
- source OR-ing,
- precharge,
- dump path,
- branch protection,
- isolated quiet rail,
- supercap ride-through.

### 9.2 Donor pattern carryover into S7
Retained from donor logic:
- bounded state machine,
- watchdog,
- latching faults,
- audit log,
- simple crew emergency interface.

### 9.3 Donor pattern carryover into S8/S9
Retained from donor logic:
- compartmentalization,
- protected routing,
- serviceability,
- readiness blockers,
- evidence-driven maintenance posture.

The key point is simple:
**the useful parts were architecture habits, not exotic mechanisms.**

---

## 10. Baseline BOM counts by hardware family

This is a seriousness check, not a purchasing order.

| Hardware family | Approx. count posture |
|---|---|
| Pressure vessels / accumulators | 3–4 primary items |
| Regulators / valve bodies | 8–14 functional items |
| Sensors (pressure, temperature, O2, CO2, humidity, water state) | 18–30 sensing points / channels |
| Pumps / blowers / moving-fluid devices | 3–6 |
| Battery / supercap / PMU major electrical assemblies | 4–7 |
| Controllers / watchdog / logging assemblies | 3–5 |
| Structural partitions / access assemblies | 8–15 fabricated items |
| Service-side software/data modules | 5–10 logical modules |

If future subsystem docs imply wildly more than this without strong justification, the pack is probably bloating.

---

## 11. Major BOM risk items

A serious BOM should admit where the pain is.

### 11.1 High-risk integration items
These are credible, but they are the hardest line items in the architecture:

1. **PEM electrolysis subsystem in backpack envelope**
2. **thermal management inside the locked package thickness**
3. **oxygen reserve isolation without excessive plumbing complexity**
4. **quiet sensing inside a noisy high-transient power environment**
5. **shell/partition design that improves survivability without eating too much volume**
6. **rescue solar augmentation that helps without becoming deployment theater**

### 11.2 Why this matters
These are the items most likely to force later trade changes.
That is normal.
Pretending otherwise would be dishonest.

---

## 12. Procurement posture

This repo is not pretending to issue a final procurement package.

So the correct procurement posture for phase 1 is:

| Procurement category | Posture |
|---|---|
| Commodity sensors, valves, protection parts, connectors | reference-class selection acceptable |
| Battery cells / supercaps / controllers | reference-class selection acceptable, but environment hardening remains open |
| Pressure vessels / regulators / manifolds | mission-relevant class only; exact part numbers remain open |
| PEM stack and thermal transfer assemblies | class-level selection only until package and power model tighten |
| Shell / partitions / manifolds | mostly custom-fabricated direction, not vendor-picked fantasy |
| Sustainment software modules | architecture-level definition first, implementation later |

That keeps the repo grounded.

---

## 13. Phase-1 BOM rules now locked

The following are baseline IX-Breath BOM rules:

1. **Every major subsystem must map to named physical hardware classes.**
2. **No line item may rely on exotic unsupported physics.**
3. **Battery + protected PMU + supercap buffer are baseline, not optional.**
4. **Isolated emergency oxygen reserve is baseline, not optional.**
5. **PEM-based bounded regeneration is baseline architecture, but must stay honestly bounded.**
6. **Water recovery must include real storage and conditioning hardware, not rhetorical recycling.**
7. **Thermal hardware must be counted as real package and mass burden.**
8. **Readiness tooling and data logic count as system architecture, not paperwork fluff.**

---

## 14. What this BOM still does not claim

This BOM does **not** claim:

- final vendor lock,
- final pressure ratings,
- final tank size,
- final battery chemistry,
- final PEM stack rating,
- final thermal loop topology,
- final mass,
- final power draw,
- final survival duration.

Those belong to later commits.

What this BOM **does** claim is narrower and more valuable right now:

**IX-Breath can now be described as a real hardware architecture built from credible subsystem classes.**

That is the point of this file.

---

## 15. What this file forces next

Now that the BOM exists, the next detailed commits must show how it is wired together and how it behaves.

That means the repo must next provide:

1. detailed power-spine architecture,
2. detailed life-support-core architecture,
3. thermal / rescue-solar / water-oxygen interaction logic,
4. FDIR and state model,
5. analysis of mass, power, consumables, and survivability.

From this point on, no later document gets to pretend a subsystem exists without hardware behind it.

