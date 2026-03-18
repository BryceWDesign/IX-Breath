# IX-Breath Mission Requirements

**Document ID:** IXB-REQ-01  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent document:** `docs/00_PROJECT_CHARTER.md`

---

## 1. Purpose

This document defines the first serious requirement baseline for IX-Breath.

These requirements are written to do four things:

1. keep the project bounded,
2. keep claims honest,
3. force traceability into later architecture decisions,
4. give reviewers a clean way to judge whether the concept is disciplined or hand-wavy.

This is a **requirements baseline for an architecture study**, not a certification basis.

---

## 2. Requirement conventions

### 2.1 Requirement language

- **Shall** = mandatory requirement for this repository baseline
- **Should** = preferred design direction, not a locked requirement
- **May** = optional design direction

### 2.2 Verification methods

- **A** = Analysis
- **I** = Inspection
- **T** = Test
- **D** = Demonstration
- **R** = Review of objective evidence

### 2.3 Requirement philosophy

IX-Breath is being written as a rescue-biased regenerative PLSS architecture study.  
That means the requirements must be strict enough to force engineering discipline, while still honest about the fact that no flight-qualified hardware has been built under this repository.

---

## 3. Mission framing requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-001 | IX-Breath shall be presented as a **rescue-biased survivability architecture** for astronaut life support, not as a replacement claim for all existing EVA systems. | I |
| IXB-REQ-002 | IX-Breath shall target delayed-ingress and separation-from-habitat/station/vehicle contingency cases as its primary use case. | I |
| IXB-REQ-003 | IX-Breath shall remain within a **backpack-class system boundary** and shall not depend on a full pressure-garment redesign to justify its core value proposition. | I |
| IXB-REQ-004 | IX-Breath shall be documented as a **non-weapon, life-preserving** architecture only. | I |
| IXB-REQ-005 | IX-Breath shall explicitly identify all performance statements as one of the following: baseline fact, design target, analytical estimate, or unvalidated concept claim. | I |
| IXB-REQ-006 | IX-Breath shall not claim flight qualification, agency endorsement, or operational readiness without objective evidence. | I |

---

## 4. Life-support functional requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-100 | The architecture shall include a **closed breathing-gas circulation loop** as the primary breathing mode. | I |
| IXB-REQ-101 | The architecture shall include controlled **oxygen make-up capability** to maintain breathable loop conditions during nominal closed-loop operation. | I |
| IXB-REQ-102 | The architecture shall include a dedicated **carbon dioxide removal function** suitable for backpack-class EVA life support. | I |
| IXB-REQ-103 | The architecture shall include **humidity/water management** as an integrated function, not as an afterthought. | I |
| IXB-REQ-104 | The architecture shall include an **isolated emergency oxygen reserve** that is protected from normal-mode depletion. | I |
| IXB-REQ-105 | The architecture shall define a **rescue-mode oxygen strategy** that preserves isolated emergency oxygen until defined trigger conditions are met. | A / I |
| IXB-REQ-106 | The architecture shall include a bounded **oxygen-regeneration extension path** based only on real-world mechanisms retained by the repository. | I |
| IXB-REQ-107 | The architecture shall not rely on exotic matter, Element 115, ambient RF as primary power, or perpetual-motion logic to justify oxygen continuity. | I |
| IXB-REQ-108 | The architecture shall define at least one **manual crew-accessible path** to activate emergency breathing support if automation fails. | I / D |
| IXB-REQ-109 | The architecture shall include loop pressure, oxygen-state, and carbon-dioxide-state awareness sufficient to support fault detection and crew alerting. | I |
| IXB-REQ-110 | The architecture shall define a **contingency fallback mode** in which only life-preserving functions remain energized if the system must shed noncritical loads. | A / I |
| IXB-REQ-111 | The architecture shall document which breathing-loop functions are considered **life critical**, **mission critical**, and **noncritical**. | I |
| IXB-REQ-112 | The architecture shall include a design path for **water conditioning or suitability management** before water is credited to oxygen-regeneration hardware. | I |

---

## 5. Electrical power and continuity requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-200 | IX-Breath shall include a primary electrical energy storage source sized for backpack-class operation. | I |
| IXB-REQ-201 | IX-Breath shall include a **fast-response electrical buffer layer** for ride-through of transients, inrush, brief interruptions, or pulsed loads. | I |
| IXB-REQ-202 | IX-Breath shall include controlled **source combining / source isolation** so that one branch fault does not automatically backfeed into every other branch. | I |
| IXB-REQ-203 | IX-Breath shall include **precharge / soft-start / inrush control** wherever the architecture contains capacitive or high-transient loads. | I |
| IXB-REQ-204 | IX-Breath shall include branch-level protection for life-support-critical and noncritical loads. | I |
| IXB-REQ-205 | IX-Breath shall define a **load-shedding hierarchy** that favors life-support continuity over convenience, telemetry richness, or optional payload behavior. | A / I |
| IXB-REQ-206 | IX-Breath shall not credit triboelectric, piezoelectric, RF scavenging, or similar low-density ambient harvesting concepts as the primary life-support power source. | I |
| IXB-REQ-207 | If a solar rescue augmentation path is included, the architecture shall treat it as **condition-dependent supplemental power**, not as guaranteed power. | I |
| IXB-REQ-208 | IX-Breath shall include a safe means to dump, isolate, or otherwise manage unsafe stored electrical energy during major fault states. | I |
| IXB-REQ-209 | The architecture shall preserve enough powered state awareness to support fault logging during derated operation when physically practical. | A / I |
| IXB-REQ-210 | The architecture shall define how critical sensors remain powered or gracefully degraded during major bus disturbances. | A / I |

---

## 6. Thermal survivability requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-300 | IX-Breath shall include a defined thermal-control strategy for crew support electronics and life-support hardware. | I |
| IXB-REQ-301 | IX-Breath shall identify thermally sensitive elements whose performance or safety may degrade before total system failure. | I |
| IXB-REQ-302 | IX-Breath shall define at least one **thermal derating path** for rescue mode. | A / I |
| IXB-REQ-303 | IX-Breath shall not assume “free” heat rejection or unsupported exotic cooling mechanisms. | I |
| IXB-REQ-304 | If deployable thermal surfaces, shades, or radiator-assist features are used, they shall be treated as packaging and failure-mode drivers, not as free add-ons. | I |
| IXB-REQ-305 | The architecture shall define how thermal-control capability changes across sunlit, mixed-light, and eclipse rescue cases. | A |
| IXB-REQ-306 | The architecture shall define what happens when thermal-control performance is insufficient to support normal mode and how the system falls back. | A / I |

---

## 7. Packaging and human-integration requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-400 | IX-Breath shall be designed to remain plausibly wearable by a suited astronaut as a backpack-class unit. | I |
| IXB-REQ-401 | IX-Breath shall explicitly track **height, width, front-to-back thickness, and center-of-gravity implications** as first-order design constraints. | A / I |
| IXB-REQ-402 | The architecture shall not justify survivability gains by silently accepting obviously impractical bulk growth. | I |
| IXB-REQ-403 | IX-Breath shall define a preliminary package-envelope target that can be inspected by reviewers before detailed CAD exists. | I |
| IXB-REQ-404 | IX-Breath shall identify external appendages or deployables that are only used in rescue mode and shall distinguish them from nominal-wear geometry. | I |
| IXB-REQ-405 | IX-Breath shall preserve a clean distinction between backpack-contained functions and pressure-garment-contained functions. | I |
| IXB-REQ-406 | IX-Breath shall identify any architecture choices likely to affect don/doff complexity, airlock integration, or EVA mobility. | A / I |

---

## 8. Fault detection, isolation, recovery, and control requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-500 | IX-Breath shall include a top-level **fault detection, isolation, and recovery (FDIR)** concept. | I |
| IXB-REQ-501 | IX-Breath shall identify at least one fail-safe or fail-operational response for each major subsystem category: breathing loop, power, thermal, sensing, and control. | I |
| IXB-REQ-502 | IX-Breath shall include a defined **system state model** with at minimum: Nominal, Conserve, Rescue, Critical, Safe-Hold, and Service states. | I |
| IXB-REQ-503 | IX-Breath shall include latching behavior for major faults where silent auto-reentry into nominal operation would be unsafe. | I |
| IXB-REQ-504 | IX-Breath shall include objective event logging sufficient to reconstruct major off-nominal behavior after the fact. | I |
| IXB-REQ-505 | IX-Breath shall include a concept for watchdog or supervisory logic independent of the main control path where practical. | I |
| IXB-REQ-506 | IX-Breath shall not depend on a single undiagnosed controller fault being “unlikely” as a safety argument. | I |
| IXB-REQ-507 | IX-Breath shall define how the crew is alerted to at least the following conditions: oxygen concern, carbon-dioxide concern, major power derate, thermal concern, and safe-hold entry. | I |
| IXB-REQ-508 | IX-Breath shall provide a crew-understandable control posture under stress and shall avoid mode logic that requires excessive interpretation during emergency use. | I / D |
| IXB-REQ-509 | IX-Breath shall define which failures immediately force safe-hold, which allow rescue mode, and which permit degraded nominal operation. | A / I |

---

## 9. Sensing, data, and maintainability requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-600 | IX-Breath shall identify the minimum critical sensor set required to support safe operation and fault detection. | I |
| IXB-REQ-601 | IX-Breath shall separate sensor-critical integrity from convenience telemetry wherever practical. | I |
| IXB-REQ-602 | IX-Breath shall include a readiness / sustainment concept that tracks calibration state, inspection state, and life-limited component status. | I |
| IXB-REQ-603 | IX-Breath shall define how objective evidence of readiness is created before EVA. | I / R |
| IXB-REQ-604 | IX-Breath shall define how objective evidence of degradation is recorded after EVA or after major fault events. | I / R |
| IXB-REQ-605 | IX-Breath shall not depend on informal operator memory as the only protection against creeping hardware degradation. | I |
| IXB-REQ-606 | IX-Breath shall identify any consumables or life-limited parts whose status materially affects rescue survivability. | I |

---

## 10. Analysis and model requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-700 | IX-Breath shall include a mass, power, consumables, and survivability model before the repository makes any strong endurance claim. | I |
| IXB-REQ-701 | IX-Breath shall evaluate at minimum three analytical rescue cases: defended case, stretch case, and outer-edge case. | I |
| IXB-REQ-702 | IX-Breath shall treat sunlit and eclipse cases separately in energy accounting. | A |
| IXB-REQ-703 | IX-Breath shall not combine incompatible assumptions to manufacture a misleading endurance result. | I |
| IXB-REQ-704 | IX-Breath shall clearly distinguish between “architecture supports analysis of this case” and “hardware has proven this case.” | I |
| IXB-REQ-705 | IX-Breath shall identify what evidence would be required to move each major claim from architecture-level to test-backed. | I |

---

## 11. Validation and seriousness requirements

| ID | Requirement | Verification |
|---|---|---|
| IXB-REQ-800 | IX-Breath shall include a validation plan that maps key claims to analysis, test, inspection, or demonstration methods. | I |
| IXB-REQ-801 | IX-Breath shall include an explicit donor-feature keep/reject record. | I |
| IXB-REQ-802 | IX-Breath shall include an honest hazard analysis / FMEA path before the README makes any “serious” summary claim. | I |
| IXB-REQ-803 | IX-Breath shall not include fake maturity artifacts such as pretend CAD finalization, pretend firmware completion, or placeholder “simulation” results presented as evidence. | I |
| IXB-REQ-804 | IX-Breath shall document unresolved technical risks instead of hiding them. | I |
| IXB-REQ-805 | IX-Breath shall preserve the distinction between a serious architecture proposal and a proven engineering product. | I |

---

## 12. Derived review questions

A reviewer should be able to use these requirements to ask:

1. Is the architecture still backpack-class, or did it quietly become a mini spacecraft?
2. Is oxygen continuity achieved by real accounting, or by vague optimism?
3. Does the power architecture actually isolate faults, or just add parts?
4. Is rescue mode coherent, or is it a marketing phrase?
5. Is thermal control modeled honestly across lighting cases?
6. Is readiness treated as a system function, or left to luck?
7. Are unsupported donor ideas rejected explicitly?
8. Are the claims bounded tightly enough to survive technical scrutiny?

If the repository cannot answer those questions, then the requirements have not been met in spirit even if the words exist on paper.

---

## 13. Requirement maturity note

These requirements are intentionally conservative.

They do not try to lock the final hardware too early.
They do force the repository to behave like a serious engineering study.

That is the point.

Later documents will refine:
- exact architecture,
- donor-feature decisions,
- package envelope,
- BOM,
- analysis targets,
- validation criteria,
- hazard logic.

But those later documents must remain traceable to this requirement baseline.
