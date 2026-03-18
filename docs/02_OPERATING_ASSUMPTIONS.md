# IX-Breath Operating Assumptions

**Document ID:** IXB-OPS-02  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study  
**Parent documents:** `docs/00_PROJECT_CHARTER.md`, `docs/01_MISSION_REQUIREMENTS.md`

---

## 1. Purpose

This document records the operating assumptions behind IX-Breath.

The point of writing these down early is simple:

- assumptions drive architecture,
- hidden assumptions create fake performance,
- rescue concepts become nonsense quickly if energy, oxygen, thermal, or human factors are hand-waved.

IX-Breath will not do that.

---

## 2. Assumption policy

Each assumption in this document is one of the following:

- a current design-bounding assumption,
- a simplifying assumption used to keep phase-1 architecture coherent,
- or a conservative assumption intended to prevent maturity inflation.

Any future document that violates one of these assumptions must either:
1. revise this file explicitly, or
2. state why the assumption no longer applies.

---

## 3. Mission-context assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-001 | IX-Breath is primarily aimed at **single-crewmember post-separation survival** while awaiting rescue or delayed ingress. | Drives one-person backpack architecture rather than multi-crew habitat ECLSS scaling. |
| IXB-OPS-002 | The starting case is a suited astronaut in a functional pressure garment with a backpack-class life-support pack. | IX-Breath is not allowed to solve problems by redesigning the entire suit from scratch. |
| IXB-OPS-003 | The primary phase-1 mission context is orbital, cislunar, or station/habitat-associated vacuum operations rather than a planetary surface baseline. | Keeps early thermal and regeneration logic closer to vacuum-assisted assumptions. |
| IXB-OPS-004 | Rescue is delayed enough that public baseline contingency oxygen durations are considered insufficient motivation for the concept. | Justifies the rescue-biased architecture focus. |
| IXB-OPS-005 | Immediate dock or airlock access is **not** assumed after the initiating failure/separation event. | Forces true stand-alone survivability logic. |
| IXB-OPS-006 | IX-Breath is a survivability extension concept, not an unrestricted “normal EVA forever” concept. | Prevents misuse of rescue-mode logic as if it were nominal mission sizing. |

---

## 4. Human-state assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-100 | The crewmember is assumed to be alive, conscious, and able to perform at least limited manual actions at rescue-mode entry. | Supports manual backup controls and crew-commanded rescue entry. |
| IXB-OPS-101 | The crewmember may be stressed, task-loaded, or cognitively narrowed during emergency use. | User interface and alerts must be simple, explicit, and low-ambiguity. |
| IXB-OPS-102 | The crewmember is trained, but the architecture should not require complex troubleshooting trees during active survival use. | Favors bounded modes over clever but fragile control logic. |
| IXB-OPS-103 | IX-Breath is not sized on the assumption of continuous heavy workload throughout extended rescue periods. | Endurance claims must separate low-activity survival from high-workload EVA. |
| IXB-OPS-104 | Short bursts of manual or locomotion effort may occur during contingency operations. | Power and oxygen budgeting must include transient margin, not purely resting assumptions. |

---

## 5. Breathing-loop and oxygen assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-200 | The primary breathing approach is a **closed-loop / recirculating** architecture with oxygen make-up and CO2 removal. | Aligns IX-Breath with rebreather-like endurance logic rather than open-loop purge wastefulness. |
| IXB-OPS-201 | An isolated emergency oxygen reserve is maintained outside normal depletion logic. | Prevents routine rescue extension from silently consuming the true last-resort oxygen source. |
| IXB-OPS-202 | Oxygen regeneration by water electrolysis is treated as a **bounded extension path**, not as an excuse to eliminate reserve oxygen storage. | Forces layered oxygen continuity rather than one fragile “miracle subsystem.” |
| IXB-OPS-203 | Water credited to oxygen generation is limited to water that is either stored explicitly or recovered and conditioned to suitable quality. | Prevents fake oxygen accounting based on undefined water availability. |
| IXB-OPS-204 | Hydrogen produced during electrolysis is assumed to be safely vented or otherwise managed without being counted as a magical secondary energy source. | Keeps the architecture conservative and avoids invented closure loops. |
| IXB-OPS-205 | Carbon dioxide removal and humidity control are assumed to remain necessary even during rescue mode and cannot be ignored simply because activity is low. | Preserves true closed-loop breathing realism. |
| IXB-OPS-206 | Open-loop purge is treated as an emergency fallback, not the normal rescue endurance mode. | Protects consumables and keeps the architecture survival-focused. |
| IXB-OPS-207 | IX-Breath does not assume that backpack-scale oxygen regeneration can fully support arbitrarily high metabolic demand indefinitely. | Prevents infinite-survival language. |

---

## 6. Power-system assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-300 | Primary backpack power is assumed to come from stored electrical energy supported by disciplined power conversion and bus protection. | Battery + buffer + protected bus remain mandatory. |
| IXB-OPS-301 | A high-response pulse buffer layer is assumed necessary because valves, pumps, blowers, and control electronics do not all behave like gentle steady loads. | Justifies supercapacitor / pulse-buffer architecture. |
| IXB-OPS-302 | Any rescue solar augmentation is assumed to be **attitude-, lighting-, and deployment-dependent**. | Solar credit must be scenario-based, not universal. |
| IXB-OPS-303 | Eclipse or shadow operations must be survivable for the modeled interval **without** solar input credit. | Forces honest worst-case energy analysis. |
| IXB-OPS-304 | Ambient RF, triboelectric, piezoelectric, and similar scavenging mechanisms are assumed to be negligible for primary life-support power. | Removes false hope and keeps the PMU architecture grounded. |
| IXB-OPS-305 | Small scavenging or housekeeping contributions may be discussed only if they are clearly segregated from life-critical power claims. | Prevents rhetorical inflation. |
| IXB-OPS-306 | Bus segmentation, source isolation, precharge, soft-start, and safe energy dump behavior are assumed mandatory for serious survivability hardware. | Makes electrical fault tolerance first-class architecture. |
| IXB-OPS-307 | Rescue-mode power budgeting assumes noncritical loads can be shed aggressively. | Enables survivability-first derating logic. |

---

## 7. Thermal assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-400 | Thermal control is assumed to be a survival-limiting function, not a secondary convenience function. | Thermal logic gets equal status with breathing and power logic. |
| IXB-OPS-401 | Backpack-class thermal hardware has finite rejection capacity and finite working margin. | Prevents magical thermal claims. |
| IXB-OPS-402 | Rescue mode may require reduced activity, reduced noncritical electronics usage, and thermal derating to remain credible. | Links endurance claims to operational behavior. |
| IXB-OPS-403 | Sunlit and eclipse cases are assumed to create materially different thermal and power conditions. | Requires separate analysis cases. |
| IXB-OPS-404 | Deployable thermal surfaces or shades, if used, are assumed to create additional packaging, failure, and handling considerations. | Prevents pretending that deployables are “free.” |
| IXB-OPS-405 | Passive protection layers may improve survivability margins but are not assumed to make the backpack invulnerable to puncture, overheating, or catastrophic damage. | Keeps shell/survivability claims bounded. |

---

## 8. Packaging assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-500 | IX-Breath must remain **backpack-class** to stay relevant to astronaut wearability. | Prevents slow drift into one-person spacecraft mass/volume. |
| IXB-OPS-501 | Preliminary phase-1 packaging is assumed to target approximately **24–26 inches tall, 18–21 inches wide, and 10–11 inches thick folded**. | Gives reviewers an inspectable envelope before detailed CAD exists. |
| IXB-OPS-502 | Rescue-only deployables are assumed to stow within or close to the nominal package envelope and not permanently define the backpack’s worn thickness. | Keeps normal wear geometry sane. |
| IXB-OPS-503 | Center of gravity and front-to-back thickness are assumed to be critical accept/reject drivers, not minor housekeeping details. | Forces packaging discipline early. |
| IXB-OPS-504 | The architecture will not be considered credible if its survivability gains depend on bulk growth that would obviously cripple EVA practicality. | Creates a hard seriousness filter. |

---

## 9. Sensing, control, and software assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-600 | The backpack requires a supervisory control layer that is distinct from raw actuator driving. | Supports watchdog / health-daemon / state-machine architecture. |
| IXB-OPS-601 | At least some failure detection must continue even when the system is heavily derated. | Preserves blackbox and minimum awareness. |
| IXB-OPS-602 | The architecture should prefer deterministic bounded modes over “smart” autonomy that is difficult to audit. | Favors serious aerospace-style control posture. |
| IXB-OPS-603 | Major off-nominal events are assumed to require event logging for later reconstruction and sustainment feedback. | Justifies blackbox logging. |
| IXB-OPS-604 | Silent automatic recovery into nominal mode after certain major faults is assumed unsafe. | Supports latched fault states. |
| IXB-OPS-605 | Crew alerting must remain understandable even if the rich telemetry display layer degrades. | Supports layered human-machine interface design. |

---

## 10. Sustainment and readiness assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-700 | Survival performance depends on pre-EVA system health, not only on architecture on paper. | Makes sustainment a required subsystem concept. |
| IXB-OPS-701 | Sensor drift, membrane aging, valve wear, scrubber state uncertainty, and battery degradation are assumed to matter materially to rescue credibility. | Forces readiness tracking and life-limited component visibility. |
| IXB-OPS-702 | Pre-EVA readiness should be evidence-based rather than trust-based. | Supports calibration, leak-check, and inspection record logic. |
| IXB-OPS-703 | Post-event and post-EVA data should feed maintenance and readiness scoring. | Ties operations back into sustainment OS concepts. |
| IXB-OPS-704 | IX-Breath should never depend on “nobody noticed the degradation” as an acceptable operational state. | Forces explicit sustainment architecture. |

---

## 11. Donor-feature screening assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-800 | Prior IX-* concept repositories may contain useful patterns even when their original framing was broader, stranger, or not fully bounded. | Justifies screening rather than dismissing everything wholesale. |
| IXB-OPS-801 | A donor feature is retained only if its engineering pattern survives real-world scrutiny in the IX-Breath context. | Keep/reject matrix becomes mandatory. |
| IXB-OPS-802 | Unsupported claims are assumed rejected by default unless independently justified in IX-Breath using real mechanisms. | Prevents prestige transfer from earlier exploratory work. |
| IXB-OPS-803 | Bus architecture, fault isolation, readiness logic, thermal survivability, and state-machine patterns are assumed more transferable than donor energy-source claims. | Guides the feature filter toward what actually helps save lives. |

---

## 12. Survivability-analysis assumptions

| ID | Assumption | Design implication |
|---|---|---|
| IXB-OPS-900 | The first serious endurance discussion for IX-Breath will be analytical, not experimental. | Future duration claims must stay bounded. |
| IXB-OPS-901 | Phase-1 analysis will examine at least three rescue cases: defended case, stretch case, and outer-edge case. | Creates a serious review structure. |
| IXB-OPS-902 | Duration estimates are assumed to depend on activity level, leak state, thermal load, lighting condition, and subsystem availability. | Prevents single-number marketing claims. |
| IXB-OPS-903 | Any survival-time number presented without those conditions is assumed to be incomplete and potentially misleading. | Forces condition-tagged analysis. |

---

## 13. What these assumptions deliberately do not assume

IX-Breath does **not** assume:

- infinite oxygen,
- infinite power,
- perfect water recovery,
- perfect fault detection,
- perfect leak tolerance,
- perfect thermal control,
- guaranteed rescue lighting conditions,
- guaranteed immediate comms or docking,
- magical miniaturization,
- unsupported exotic materials or exotic physics.

That is intentional.

The repository is being forced to earn credibility the hard way.

---

## 14. Assumption retirement criteria

An assumption in this document may be retired or revised only when one of the following happens:

1. a later analysis proves it too conservative,
2. a later subsystem definition proves it too loose,
3. a later validation plan requires a sharper statement,
4. a later hazard review shows the assumption hides risk,
5. a better public technical baseline justifies a revision.

Any such change should be explicit and traceable.

---

## 15. Closing note

These assumptions are not decorative.
They are the anti-hand-waving layer of IX-Breath.

If later documents drift away from them without explanation, the repo stops reading like aerospace work and starts reading like concept theater.

That is exactly what this project is trying to avoid.
