# IX-Breath Donor Feature Filter

**Document ID:** IXB-FILTER-04  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent documents:**  
- `docs/00_PROJECT_CHARTER.md`  
- `docs/01_MISSION_REQUIREMENTS.md`  
- `docs/02_OPERATING_ASSUMPTIONS.md`  
- `docs/03_MASTER_ARCHITECTURE.md`

---

## 1. Purpose

This document records the exact donor-feature decisions used to build IX-Breath.

Its purpose is to prevent four common failure modes:

1. **idea pile engineering**  
   importing everything because it sounds interesting

2. **prestige transfer**  
   acting as if a feature is validated merely because it exists in an earlier repo

3. **silent mechanism swapping**  
   keeping a good architecture pattern but failing to say the original mechanism was rejected

4. **maturity inflation**  
   pretending previous concept work already proved IX-Breath

This file makes the filter explicit.

---

## 2. Decision vocabulary

IX-Breath uses four donor decisions.

| Decision | Meaning |
|---|---|
| **KEEP** | Retain the feature or pattern substantially as-is because it is directly useful and physically defensible in IX-Breath. |
| **ADAPT** | Retain the engineering pattern, but change mechanism, scale, packaging, or implementation to make it appropriate for IX-Breath. |
| **REJECT** | Do not use the feature in IX-Breath because it is unsupported, mismatched to the mission, or actively harmful to credibility. |
| **DEFER** | Plausibly useful, but not adopted into the phase-1 baseline because evidence or packaging discipline is not yet strong enough. |

**Important rule:**  
A donor repo may contribute **patterns** without contributing its original **claims**.

That distinction is non-negotiable.

---

## 3. Filter criteria

Every donor feature is screened against the same criteria.

| Filter axis | Question |
|---|---|
| Mission fit | Does this directly improve astronaut survivability in a backpack-class rescue system? |
| Physics defensibility | Can the mechanism be defended without exotic or unsupported claims? |
| Packaging fit | Can it plausibly live within the IX-Breath package envelope? |
| Safety effect | Does it improve fault containment, readiness, or survivability under off-nominal conditions? |
| Control burden | Does it increase dangerous complexity relative to its benefit? |
| Credibility effect | Will aerospace reviewers see disciplined engineering, or eye-roll bait? |

A feature that fails the credibility test is not automatically wrong in the abstract.  
It is wrong **for IX-Breath**.

---

## 4. Donor-repo summary decisions

| Donor repo | IX-Breath decision | Summary |
|---|---|---|
| IX-Legacy | **KEEP / ADAPT** | Strong electrical-spine donor; bus rules, interlocks, thermal derating, and logging posture transfer cleanly. |
| TeslaScythe | **ADAPT / REJECT** | Useful power-buffering, monitoring, and modular protection patterns; exotic energy-source claims rejected. |
| TeslaRake | **ADAPT / REJECT** | Minor passive combiner logic useful; primary harvesting logic rejected for life-support duty. |
| STAG | **KEEP** | Strong measurement discipline, claims-and-limits posture, thermal test realism, and FMEA tone anchor. |
| IX-TunerCore | **ADAPT** | Clean rail segmentation, low-noise sensor rail thinking, star-grounding, and soft-start/posture are useful. |
| IX-HapticSight | **ADAPT** | Strong bounded finite-state logic, dual-channel veto, safe-hold, and emergency-retreat logic map well into suit control. |
| IX-King | **ADAPT / REJECT** | Thermal-isolation and sensor-routing habits useful; stealth, harmonic, and ambient-harvest framing rejected. |
| IX-Singularis | **ADAPT / REJECT** | Watchdog, blackbox logging, emergency shunt, and impact-liner patterns useful; exotic containment logic rejected. |
| IX-Sustainment-OS | **KEEP / ADAPT** | Readiness, evidence, blocker visibility, and auditable state transitions directly improve life-support seriousness. |
| IX-Shimizu | **ADAPT** | Clear operational states, safe-mode behavior, diagnostics, and maintenance scheduling logic are transferable. |
| IX-Sho-Nuff | **ADAPT / REJECT** | Control rigor and isolated feedback habits useful; field/coil/exotic control mission rejected. |
| IX-ZeroCell | **ADAPT / REJECT** | Scaled-down PMU, combiner, fuse, clamp, and wiring discipline useful; giant ambient-harvest architecture rejected. |
| IX-T | **DEFER / REJECT** | Validation/logging tone is useful, but core energy-harvester substance is not needed in phase 1. |
| IX-Secret-Fusion-Family-Recipe | **ADAPT / REJECT** | Layered-control separation and logging style useful; fusion framing rejected. |
| IX-Golden-Trump-Card | **REJECT** | Not relevant to astronaut life-support credibility or survivability architecture. |

---

## 5. Exact donor-feature matrix

### 5.1 IX-Legacy

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `docs/05_COMMON_BUS_SPEC.md` | OR-ing / no-backfeed bus discipline | **KEEP** | S6 Power Continuity Spine | Directly applicable to multi-source protected bus design. |
| `docs/05_COMMON_BUS_SPEC.md` | Fuse-per-branch fault containment | **KEEP** | S6 / S8 | Strong survivability value with low complexity. |
| `docs/05_COMMON_BUS_SPEC.md` | Controlled precharge for large capacitance | **KEEP** | S6 | Required if IX-Breath uses supercap ride-through or large buffered buses. |
| `docs/05_COMMON_BUS_SPEC.md` | Manual/service dump path for stored energy | **KEEP** | S6 | Important for safe discharge and fault containment. |
| `docs/06_FAULTS_AND_INTERLOCKS.md` | Latched electrical safety faults | **KEEP** | S7 | Correct posture for life-support-adjacent faults. |
| `docs/06_FAULTS_AND_INTERLOCKS.md` | Telemetry requirement for every fault | **KEEP** | S7 / S9 | Strong post-event reconstruction value. |
| `docs/07_THERMAL_GUARDIAN.md` | Derate-before-shutdown philosophy | **KEEP** | S5 / S7 | Excellent fit for rescue survivability. |
| `docs/07_THERMAL_GUARDIAN.md` | Keep telemetry alive through thermal derate if possible | **KEEP** | S5 / S7 | Useful for crew awareness and post-event evidence. |
| `docs/09_LOW_LOSS_RECTIFICATION.md` | Low-loss conversion mindset | **ADAPT** | S6 | Keep the efficiency discipline, not the original harvester mission. |
| `docs/modules/MODULE_R_RF_RECTENNA.md` | RF rectenna source path | **REJECT** | — | Not credible as primary or meaningful rescue power in IX-Breath. |
| `docs/modules/MODULE_A_EFIELD.md` | E-field harvesting module | **REJECT** | — | No mission fit for astronaut life support. |
| `docs/modules/MODULE_B_VIBRATION.md` | Vibration harvesting as meaningful power lane | **REJECT** | — | Power density and mission relevance are too weak. |
| `docs/modules/MODULE_C_THERMAL_TEG.md` | Small thermal-electric harvesting | **DEFER** | S6 housekeeping only | Might matter for ultra-low-power housekeeping, but not phase-1 life-support sizing. |

### 5.2 TeslaScythe

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `docs/teslascythe_power_management.md` | Multi-source monitored power combiner | **ADAPT** | S6 | Keep the bus supervision concept, but use real IX-Breath sources only. |
| `docs/teslascythe_power_management.md` | Supercap + battery layered storage | **ADAPT** | S6 | Strong fit as primary battery plus pulse-buffer architecture. |
| `docs/teslascythe_power_management.md` | Load scheduling to avoid brownouts | **KEEP** | S7 / S6 | Direct survival value. |
| `docs/teslascythe_monitoring_and_safety.md` | Temperature / vibration / charge monitoring posture | **ADAPT** | S7 / S9 | Useful if retargeted to life-support hardware health. |
| `docs/teslascythe_monitoring_and_safety.md` | Manual hardware disconnect | **KEEP** | S6 / S8 | Useful service and emergency isolation feature. |
| `circuits/teslascythe_power_schematic.md` | Star-ground layout | **KEEP** | S6 | Good electrical hygiene for mixed noisy/quiet rails. |
| `circuits/teslascythe_power_schematic.md` | Modular connectors and replaceable subsystems | **ADAPT** | S8 / S9 | Useful, but must be adapted for suit-grade packaging. |
| `README.md`, `circuits/*`, `docs/*` | RF harvesting as meaningful power source | **REJECT** | — | Not credible for crew-survival sizing. |
| `README.md`, `circuits/*`, `docs/*` | MEMS vibration harvesting as meaningful life-support source | **REJECT** | — | Same reason. |
| `README.md`, `circuits/*`, `docs/*` | Radiation-cell power lane | **REJECT** | — | Not appropriate for IX-Breath baseline. |
| `README.md`, `circuits/*`, `docs/*` | Casimir / vacuum-energy cells | **REJECT** | — | Explicitly out of scope for IX-Breath. |

### 5.3 TeslaRake

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `hardware/power_bus.md` | Simple diode-isolated bus combining | **ADAPT** | S6 | Useful only as a simplified conceptual pattern. |
| `hardware/power_flow.md` | Source-merge visualization | **ADAPT** | S6 documentation posture | Good explanatory pattern for protected flow paths. |
| `hardware/teg_harvester.md` | Thermal-gradient scavenging concept | **DEFER** | S6 housekeeping only | Could support tiny housekeeping loads in niche cases, not phase 1. |
| `hardware/piezo_harvester.md` | Piezo harvesting | **REJECT** | — | Not mission-relevant at required scale. |
| `hardware/teng_harvester.md` | Triboelectric harvesting | **REJECT** | — | Same reason. |
| `hardware/rf_rectifier.md` | RF harvesting | **REJECT** | — | Same reason. |

### 5.4 STAG

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `docs/CLAIMS_AND_LIMITS.md` | Claims tied only to measurements or defensible models | **KEEP** | Entire repo posture | This is exactly the seriousness filter IX-Breath needs. |
| `docs/FAILURE_MODES.md` | FMEA-style early risk discipline | **KEEP** | Future hazard/FMEA docs | Strong fit for aerospace review culture. |
| `design/CONTROLLED_HEAT_HHX_CONCEPT.md` | Measure thermal input honestly | **KEEP** | S5 / future analysis | Excellent discipline for electrolyzer and thermal loop accounting. |
| `test/TEST_PLAN_OVERVIEW.md` | Calibration-first, logging-first validation culture | **KEEP** | Entire validation plan | High-value seriousness pattern. |
| `bom/*` | Tiered sourcing and proof posture | **ADAPT** | Future BOM notes | Good fit if rewritten for EVA-relevant hardware. |
| Core thermoacoustic engine architecture | Energy-generation mechanism | **REJECT** | — | Not relevant to backpack rescue PLSS. |

### 5.5 IX-TunerCore

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `hardware/power_distribution_schematic.svg` | Four-zone radial bus with isolated rails | **ADAPT** | S6 | Suit pack needs segregated rails, though not the original quadrant geometry. |
| `hardware/power_distribution_schematic.svg` | Low-noise linear sensor rail | **KEEP** | S6 / S7 | Very useful for oxygen/CO2/pressure sensing integrity. |
| `hardware/power_distribution_schematic.svg` | Soft-start per power zone | **KEEP** | S6 | Good for staged startup and transient control. |
| `hardware/power_distribution_schematic.svg` | Star-grounded single return node | **KEEP** | S6 | Strong anti-EMI discipline. |
| `hardware/power_distribution_schematic.svg` | Ferrite choke points / backfeed diodes | **KEEP** | S6 | Good mixed-signal hygiene. |
| `software/utils/logger.py` | Timestamped diagnostics logging | **ADAPT** | S7 / S9 | Useful pattern, retargeted to life-support state and faults. |
| `software/utils/phase_control.py` | Phase-control logic for scalar field loops | **REJECT** | — | Original mission does not map to IX-Breath. |
| `docs/* scalar_*`, `docs/theory_of_operation.md` | Scalar/harmonic field theory as justification | **REJECT** | — | Out of scope and not needed. |

### 5.6 IX-HapticSight

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `docs/state_machine.md` | Bounded FSM with clear states and guards | **KEEP** | S7 | Directly transferable control discipline. |
| `docs/state_machine.md` | SAFE_HOLD state | **KEEP** | S7 | Excellent fit for hazardous ambiguous states. |
| `docs/state_machine.md` | EMERGENCY_RETREAT logic | **ADAPT** | S7 | Adapt into emergency isolate / emergency preserve logic. |
| `src/ohip/safety_gate.py` | Dual-channel veto | **KEEP** | S7 | Strong pattern for software + independent hardware interlock logic. |
| `src/ohip/safety_gate.py` | Edge-latched safety block until explicit clear | **KEEP** | S7 | Exactly right for critical faults. |
| `docs/spec.md` | Audit log on key state transitions | **KEEP** | S7 / S9 | Strong safety and traceability posture. |
| Contact planning / social-touch behaviors | Original human-touch mission | **REJECT** | — | Not relevant to IX-Breath mission. |

### 5.7 IX-King

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `core/cryocore_mount.md` | Shielded, isolated sensor routing away from PWM lines | **ADAPT** | S6 / S7 | Excellent pattern for noisy power electronics near quiet sensors. |
| `core/cryocore_mount.md` | Thermal isolation ring / controlled internal heat spread | **ADAPT** | S5 / S8 | Useful as packaging and hotspot-management pattern. |
| `core/cryocore_mount.md` | Watchdog and thermal cooldown mode concept | **ADAPT** | S7 / S5 | Transferable as generic protective behavior. |
| `build/BOM.md` | PT1000 temperature sensing habit | **DEFER** | S5 / S7 | Sensor type may or may not be the right final choice, but the discipline is useful. |
| `build/BOM.md`, `power/*` | Ambient energy harvesting architecture | **REJECT** | — | Not credible for IX-Breath life-support power. |
| `defense/*`, `communications/*`, `field/*`, `propulsion/*` | Stealth / harmonic beam / field / defense framing | **REJECT** | — | Explicitly out of scope. |

### 5.8 IX-Singularis

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `control/system-health-daemon.md` | Always-on health daemon | **ADAPT** | S7 | Strong fit if retargeted to real life-support variables. |
| `control/system-health-daemon.md` | Triggered fallback response with bounded latency | **ADAPT** | S7 | Good pattern, but timing will be redefined for suit hardware. |
| `control/system-health-daemon.md` | Internal blackbox logging | **KEEP** | S7 / S9 | Excellent survivability and post-event value. |
| `hardware/blackbox-uart-stream.json` | Structured event log format | **ADAPT** | S7 / S9 | Useful template for fault/event serialization. |
| `hardware/emergency-vector-cap-array.md` | Fail-open emergency shunt / dump path concept | **ADAPT** | S6 | Keep the idea of emergency energy diversion, reject original mission/mechanism. |
| `subsystems/STF-reactive-matrix.md` | STF-based impact / puncture mitigation liner | **ADAPT** | S8 | Useful as protective liner around tanks/electronics if rewritten conservatively. |
| `subsystems/STF-reactive-matrix.md` | CNT additive for field modulation | **REJECT** | — | Not needed; field framing removed. |
| `containment/*`, `subsystems/* element115*`, `hardware/zero-point-regulator-specs.md` | Exotic containment / zero-point / Element 115 features | **REJECT** | — | Explicitly prohibited in IX-Breath. |

### 5.9 IX-Sustainment-OS

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `docs/architecture/system-overview.md` | Evidence and audit trail as first-class architecture | **KEEP** | S9 | Strong direct fit. |
| `docs/architecture/system-overview.md` | Blocker visibility posture | **ADAPT** | S9 | Translate blockers into readiness blockers: calibration, membrane, leak, parts, approval. |
| `docs/workflows/core-workflows.md` | Controlled case-state model | **ADAPT** | S9 | Good fit for maintenance/readiness workflow. |
| `internal/workflow/transitions.go` | Allowed-transition discipline | **KEEP** | S9 | Strong deterministic workflow pattern. |
| `internal/workflow/transitions.go` | DeriveSuggestedState from blockers | **ADAPT** | S9 | Excellent pattern for “go / no-go / hold / service” readiness decisions. |
| `internal/audit/events.go` | Event-generation and evidence logging | **KEEP** | S9 | Ideal for inspection, calibration, and post-fault evidence. |
| CUI / entitlement / procedure-access framing | Original sustainment-security context | **ADAPT** | S9 | Keep access-control and evidence concepts, not the original defense context. |

### 5.10 IX-Shimizu

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `docs/control_system_functional_overview.md` | Clear operational states (Idle/Capture/Regeneration/Safe) | **ADAPT** | S7 | Good state simplification pattern for Nominal/Conserve/Rescue/Critical/Safe-Hold/Service. |
| `docs/control_system_functional_overview.md` | Sensor-validation loop and BIST | **KEEP** | S7 / S9 | Strong fit. |
| `docs/safety_and_shutdown_protocols.md` | Fault-to-safe-mode behavior | **KEEP** | S7 | Directly useful. |
| `docs/safety_and_shutdown_protocols.md` | Physical shutdown button / breaker concept | **ADAPT** | S8 / S6 | Keep as suit-service or maintenance isolation posture. |
| `docs/maintenance_and_regeneration_schedule.md` | Scheduled calibration / overhaul / replacement cadence | **KEEP** | S9 | Very relevant to sustainment discipline. |
| `docs/power_system_overview.md` | Ambient energy independence framing | **REJECT** | — | Not suitable for IX-Breath baseline. |

### 5.11 IX-Sho-Nuff

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `README.md`, `src/*` | Tight feedback-control posture | **ADAPT** | S7 / S3 / S5 | Control rigor is useful when detached from the original mission. |
| `src/test_scripts/IX_ShoNuff_Test_Suite.py` | Sensor-feedback loop testing habit | **ADAPT** | Future control validation | Useful testing attitude, not direct subsystem content. |
| `README.md`, `src/electromagnetic_system/*` | Coil-driver / field-control mission | **REJECT** | — | Out of scope. |
| `src/thermal_regulation/*` | Thermal monitoring habits | **DEFER** | S5 | Might offer generic ideas later, but not required in phase 1. |

### 5.12 IX-ZeroCell

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `hardware/energy_bus_combiner_logic.md` | Modular diode-isolated combiner rail | **ADAPT** | S6 | Useful after scaling down to real IX-Breath sources. |
| `hardware/energy_bus_combiner_logic.md` | Shared bulk-cap smoothing before storage stage | **ADAPT** | S6 | Good pattern for source smoothing. |
| `hardware/fuse_and_protection.md` | Polyfuse / TVS / reverse-polarity protection | **KEEP** | S6 | Directly useful. |
| `hardware/fuse_and_protection.md` | Ideal diode controller option | **KEEP** | S6 | Strong fit for no-backfeed and output protection. |
| `docs/ix-zerocell_wiring_diagram_notes.md` | Loop avoidance and wire spacing discipline | **ADAPT** | S6 / S8 | Good packaging rule, rewritten for suit geometry. |
| `hardware/boost_converter_block.md` | Buffered regulated output stage concept | **ADAPT** | S6 | Keep the concept, not the exact converter selection yet. |
| `hardware/coil_winding.md`, 99-coil architecture | Large ambient kinetic harvest core | **REJECT** | — | Not suitable for backpack rescue PLSS. |
| `docs/power_output_specs.md` | Ambient-harvest output framing | **REJECT** | — | Same reason. |

### 5.13 IX-T

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `validation/validation_plan.md` | Logging and environmental-conditions discipline | **DEFER** | Future validation sections | Some tone value, but redundant with stronger STAG material. |
| Harvester-specific core content | Primary concept substance | **REJECT** | — | Not needed for IX-Breath phase 1. |

### 5.14 IX-Secret-Fusion-Family-Recipe

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `src/utils/simulation_logger.py` | Simple structured logging posture | **ADAPT** | S7 / S9 | Low-level utility pattern only. |
| Layered subsystem separation implied across repo | Architectural layering habit | **ADAPT** | S7 / S6 / S9 | Useful as a documentation and control separation pattern. |
| Fusion framing and implied energy mechanism | Core mission framing | **REJECT** | — | Out of scope and not needed. |

### 5.15 IX-Golden-Trump-Card

| Donor source | Donor feature | Decision | IX-Breath target | Why |
|---|---|---|---|---|
| `hardware/BOM.md`, `signal/*`, `analysis/*` | Entire donor set | **REJECT** | — | No meaningful relevance to astronaut life-support survivability. |

---

## 6. Cross-repo retained patterns

After filtering, the retained patterns are not random.  
They cluster into a coherent IX-Breath shape.

### 6.1 Retained for S6 — Power Continuity Spine
Retained patterns:
- no-backfeed source combining,
- per-branch protection,
- precharge,
- soft-start,
- supercap pulse buffering,
- safe dump path,
- low-noise sensor rail separation,
- star-ground / single-return discipline,
- TVS / reverse-polarity protection.

Primary donors:
- IX-Legacy
- TeslaScythe
- IX-TunerCore
- IX-ZeroCell

### 6.2 Retained for S7 — Supervisory Control / FDIR / Crew Interface
Retained patterns:
- bounded state machine,
- dual-channel veto,
- watchdog/health daemon,
- fault latching,
- emergency safe-hold,
- event logging,
- deterministic operator-readable behavior.

Primary donors:
- IX-HapticSight
- IX-Singularis
- IX-Shimizu
- TeslaScythe
- IX-Sho-Nuff

### 6.3 Retained for S8 — Structural Survivability Shell
Retained patterns:
- compartmentalization,
- protective liner thinking,
- internal routing discipline,
- hotspot isolation,
- damage containment posture.

Primary donors:
- IX-Singularis
- IX-King
- IX-ZeroCell

### 6.4 Retained for S9 — Sustainment and Readiness Layer
Retained patterns:
- auditable event history,
- controlled workflow transitions,
- blocker visibility,
- calibration/readiness evidence,
- maintenance scheduling,
- inspection cadence.

Primary donors:
- IX-Sustainment-OS
- IX-Shimizu
- IX-Legacy
- STAG

### 6.5 Retained for repo seriousness
Retained patterns:
- claims tied to evidence,
- explicit limits,
- validation discipline,
- early FMEA posture,
- anti-hand-waving documentation.

Primary donors:
- STAG
- IX-Legacy

---

## 7. Explicitly rejected feature families

The following feature families are rejected across the board for IX-Breath.

### 7.1 Exotic energy claims
Rejected:
- zero-point energy,
- Casimir/vacuum-energy harvesters,
- Element 115 power or field logic,
- perpetual-motion framing,
- “free energy” language,
- any harvest claim that is not supportable at suit-class power density.

### 7.2 Ambient scavenging as primary life-support power
Rejected as primary power:
- ambient RF,
- piezoelectric harvesting,
- triboelectric harvesting,
- vague vibration scavenging,
- unspecified “radiation cells.”

Reason:
These may be interesting in other contexts, but they are not credible anchors for astronaut oxygen continuity and fault-tolerant suit power.

### 7.3 Defense / stealth / beam / field framing
Rejected:
- stealth language,
- field scramblers,
- beam shaping,
- offensive or defense mission language,
- strike/weapon or non-life-preserving posture.

Reason:
Wrong mission, wrong review audience, wrong credibility profile.

### 7.4 Hidden mechanism substitution
Rejected:
- keeping an architecture block while quietly changing its mechanism without saying so.

Rule:
If IX-Breath adapts a donor pattern, the adaptation must be stated explicitly.

---

## 8. Donor features scaled down for IX-Breath

Some donor features survive only after aggressive scale-down.

| Donor feature family | IX-Breath adaptation |
|---|---|
| ZeroCell multi-source combiner | Shrunk into a suit-class PMU combiner for battery / rescue-solar / buffered buses, not ambient-harvest fantasy. |
| Singularis emergency vector-cap array | Reframed as an emergency electrical dump/shunt path, not field-collapse hardware. |
| TunerCore quadrant power zones | Reframed as critical / control / recovery / noncritical rail tiers rather than field quadrants. |
| HapticSight emergency-retreat logic | Reframed as emergency isolate / emergency preserve / safe-hold control behavior. |
| King CryoCore routing and thermal isolation | Reframed as sensor routing, hotspot isolation, and thermal-buffer layout, not stealth logic. |
| TeslaScythe multi-harvester PMU | Reframed as a protected multi-source suit PMU using real sources only. |

---

## 9. Features intentionally not carried into phase 1

Not every plausible feature is adopted immediately.

These are held out of phase 1 because they would add complexity before the baseline is earned:

- secondary housekeeping scavenging channels,
- optional advanced liner materials beyond conservative puncture mitigation,
- highly detailed predictive maintenance algorithms,
- deployable rescue-only appendages beyond what packaging can honestly support,
- any advanced autonomy beyond bounded state logic.

IX-Breath phase 1 is trying to be serious, not maximalist.

---

## 10. Donor-feature rules now locked

The following rules are now baseline IX-Breath rules.

1. **Keep architecture patterns, not mythology.**
2. **If the mechanism is unsupported, it is rejected even if the packaging idea is interesting.**
3. **Life-support power may not be justified by ambient scavenging rhetoric.**
4. **Every adapted donor feature must declare what changed.**
5. **Readiness and maintenance logic count as real safety architecture.**
6. **Control behavior must remain bounded and inspectable.**
7. **Anything that harms credibility without paying back survivability gets removed.**

---

## 11. Net result of the filter

After filtering, IX-Breath is no longer “a blend of old repos.”

It becomes something much cleaner:

- a closed-loop rescue-biased PLSS architecture,
- with layered oxygen continuity,
- a hardened protected power spine,
- bounded fault-management states,
- a survivability shell,
- and an offboard sustainment OS that prevents quiet degradation from becoming a life-support surprise.

That is the point of this file.

Without this filter, IX-Breath would look unserious.
With it, IX-Breath earns the right to keep going.

---

## 12. What this file forces next

Because the donor filter is now explicit, the next repository documents must now commit to:

1. the package envelope and suit-integration posture,
2. the fresh real-world BOM,
3. detailed power-spine architecture,
4. detailed life-support-core architecture.

There is no room now to “accidentally” re-import rejected ideas later.

This document is the line in the sand.
