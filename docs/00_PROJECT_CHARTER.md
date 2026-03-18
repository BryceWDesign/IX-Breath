# IX-Breath Project Charter

**Document ID:** IXB-CHARTER-00  
**Revision:** A  
**Status:** Active baseline  
**Repository posture:** Architecture study, not flight-qualified hardware  
**License:** Apache-2.0  

---

## 1. One-line definition

**IX-Breath is a rescue-biased regenerative portable life support system (PLSS) architecture study intended to extend astronaut survivability after separation from a habitat, vehicle, or station by integrating bounded life-support regeneration, hardened power continuity, fault isolation, and sustainment-readiness logic into a backpack-class system.**

---

## 2. Purpose

The purpose of this repository is to define, bound, and evaluate a serious astronaut-survival architecture that is readable by aerospace reviewers, systems engineers, safety engineers, and technically literate decision-makers without overstating maturity.

This repository exists to answer the following questions clearly:

1. What problem is IX-Breath trying to solve?
2. What is the system architecture?
3. What technical lanes are in scope?
4. What claims are explicitly **not** being made?
5. What donor concepts were kept, rejected, or scaled down?
6. What evidence would be required before any performance claim could be taken seriously?
7. What failure logic governs the design?
8. What validation path would move the concept from architecture study toward disciplined prototyping?

---

## 3. Problem statement

Public NASA xEVA material describes a suit system intended to support EVAs up to 8 hours in duration, with an additional 1 hour of secondary/backup oxygen for off-nominal operations. Public NASA PLSS roadmap material also makes clear that suit backpack design is constrained by volume, on-back mass, center of gravity, and front-to-back dimension. These constraints create a hard design tension: survival extension is valuable, but packaging, mobility, and integration limits are real.

IX-Breath is aimed at the specific off-nominal case in which a suited crewmember becomes separated from immediate ingress capability and requires materially more survival time than a conventional contingency reserve is designed to provide.

IX-Breath is **not** a generic “better spacesuit” claim.
IX-Breath is a **rescue-survivability architecture study**.

---

## 4. Program objective

The primary objective is to define a backpack-class architecture that can materially improve post-separation survivability by integrating the following functions into a coherent system:

- closed-loop breathing-gas circulation,
- carbon-dioxide and humidity control,
- bounded oxygen replenishment by real-world regenerative means,
- hardened multi-source electrical continuity,
- fault detection, isolation, and recovery,
- thermal survivability support,
- sustainment and readiness logic that reduces the chance of hidden degradation before EVA.

The design objective is **life preservation under contingency conditions**, not novelty for its own sake.

---

## 5. Bounded design hypothesis

IX-Breath proceeds from the following bounded engineering hypothesis:

> A rescue-biased backpack can extend astronaut survivability beyond current public contingency baselines if it treats oxygen, water, thermal control, electrical continuity, and maintenance readiness as one integrated survival system instead of five loosely coupled subsystems.

This hypothesis is based on public, real-world technical lanes already visible in NASA work, including:

- regenerative life support by water electrolysis,
- water recovery and atmospheric revitalization practice,
- Rapid Cycle Amine (RCA) CO2/humidity management,
- Spacesuit Water Membrane Evaporator (SWME) thermal control,
- high-efficiency space solar,
- disciplined PLSS packaging and safety constraints.

This repository does **not** assume any breakthrough in fundamental physics.

---

## 6. Explicit claim boundaries

### 6.1 Claims this repository **does make**

This repository claims only that:

1. the problem is real,
2. the system architecture is worth serious review,
3. the retained subsystem choices are grounded in real technical lanes,
4. the donor-feature screening process will be explicit,
5. the design will be subjected to mass, power, consumables, packaging, safety, and validation discipline,
6. no performance claim will be treated as established unless it is traceable to a model, calculation, test, or cited external baseline.

### 6.2 Claims this repository **does not make**

This repository does **not** claim that IX-Breath:

- is flight-qualified,
- is NASA-approved,
- is xEVAS-compliant,
- can provide infinite oxygen,
- can provide infinite power,
- can guarantee survival under all leak, thermal, orbital, or eclipse conditions,
- can replace existing NASA or commercial suit programs,
- has completed prototype testing,
- has solved EVA life support as a whole.

### 6.3 Claims explicitly prohibited in this repository

The following claims are out of scope and should not appear as architectural justification in IX-Breath:

- Element 115 performance claims,
- zero-point or vacuum-energy claims,
- ambient RF as a primary life-support power source,
- perpetual motion or over-unity claims,
- “free energy” language,
- stealth, strike, weapon, or offensive military framing,
- any assertion that exotic mechanisms are required for the core survival function.

---

## 7. Operating boundary

IX-Breath is designed as a **non-weapon, life-preserving, astronaut-survival architecture**.

It is in scope for:

- contingency astronaut survival after delayed ingress,
- habitat/station/vehicle separation scenarios,
- lunar-orbital or microgravity rescue waiting periods,
- backpack-class life-support resilience studies,
- sustainment-readiness and health-monitoring logic,
- packaging and systems-engineering trade studies.

It is out of scope for:

- weaponization,
- military engagement concepts,
- offensive dual-use framing,
- speculative propulsion,
- stealth/field manipulation concepts,
- unsupported radiation/field shielding claims,
- policy claims outside engineering scope.

---

## 8. Technical posture

IX-Breath will be written as an engineering repository, not as a pitch deck and not as fiction.

That means:

- constraints will be explicit,
- assumptions will be declared,
- rejected ideas will remain documented,
- donor-repo influences will be screened rather than blindly imported,
- failure logic will be treated as first-class architecture,
- the validation plan will matter as much as the concept diagram,
- the README will come last, after the serious technical core exists.

---

## 9. Preliminary architecture direction

At charter level, the current intended architecture direction is:

1. **Life-support core**
   - closed breathing loop,
   - oxygen partial-pressure control,
   - carbon-dioxide and humidity control,
   - water handling and conditioning,
   - isolated emergency oxygen reserve.

2. **Regenerative extension layer**
   - bounded oxygen replenishment through water electrolysis,
   - docking or habitat recharge compatibility,
   - rescue-mode power and load-shedding behavior.

3. **Power continuity spine**
   - primary battery,
   - pulse-buffer/supercapacitor stage,
   - controlled source combining,
   - inrush management,
   - fault isolation,
   - soft-start and safe-dump capability.

4. **Thermal survivability layer**
   - thermal management,
   - protected component hotspots,
   - power derating logic,
   - external heat-rejection strategy consistent with suit-scale constraints.

5. **FDIR and sustainment layer**
   - fault detection, isolation, and recovery,
   - blackbox logging,
   - health daemon/watchdog logic,
   - maintenance readiness scoring,
   - calibration and inspection evidence tracking.

A later commit will bind this to a block architecture and exact subsystem interfaces.

---

## 10. Top-level design targets

The following are **program targets**, not validated performance claims:

### 10.1 Survivability posture
- materially extend post-separation survival time relative to public baseline contingency duration,
- preserve a meaningful emergency oxygen reserve until late in the rescue timeline,
- prevent a single electrical transient or brownout from taking down life-support essentials.

### 10.2 Packaging posture
- remain backpack-class,
- remain plausibly wearable with suit integration discipline,
- avoid architecture choices that obviously explode front-to-back depth, center of gravity, or thermal burden.

### 10.3 Systems posture
- fail operational where practical,
- fail safe where continued operation is unsafe,
- preserve telemetry through derating/fault states where physically possible,
- reject subsystem complexity that does not pay for itself in survivability.

### 10.4 Repository posture
- no filler documents,
- no fake CAD maturity,
- no fake firmware maturity,
- no placeholder “simulations” that do not actually simulate the claimed phenomenon,
- no performance claims without traceability.

---

## 11. External baseline used by this repo

This repository is intentionally framed against public technical baselines rather than hype.

The baseline assumptions informing IX-Breath include:

- public NASA xEVA duration and contingency framing,
- public NASA PLSS packaging constraints,
- public NASA ECLSS oxygen-generation and water-recovery practice,
- public NASA/NASA-partner RCA work for suit-scale CO2 and humidity handling,
- public NASA SWME work for spacesuit thermal control,
- public NASA space-power state-of-the-art material on high-efficiency solar cells.

These sources are cataloged in `docs/99_REFERENCES.md`.

---

## 12. Donor-repo posture

This repository may draw **patterns** from earlier concept repositories authored by Bryce Lovell.

That does **not** mean the earlier claims are accepted wholesale.

The rule for donor adoption is:

1. extract the engineering pattern,
2. discard unsupported mechanism claims,
3. retain only what survives real systems review,
4. document the keep/reject decision explicitly.

A dedicated donor-feature keep/reject matrix will be added in a later commit.

---

## 13. Program non-negotiables

The following rules are locked for IX-Breath:

1. **Reality first**  
   If a mechanism is not buildably defensible, it does not enter the core design.

2. **Safety before elegance**  
   A simpler architecture with clearer failure behavior is preferred over a clever architecture with ambiguous failure modes.

3. **No single-point fantasy dependence**  
   No subsystem may be justified by “it probably works” language.

4. **No invisible accounting**  
   Energy, oxygen, water, and thermal burdens must be counted honestly.

5. **No maturity inflation**  
   Architecture study means architecture study.

6. **No borrowed prestige**  
   Referencing NASA work is not the same thing as NASA endorsement.

7. **No README-first theater**  
   Marketing summary comes after technical substance, not before it.

---

## 14. Evidence standard

No performance statement in IX-Breath should be treated as established unless it is backed by at least one of the following:

- a cited external technical baseline,
- a first-principles calculation shown in-repo,
- a testable requirement linked to a validation method,
- a bench test plan with measurable acceptance criteria,
- a hazard or failure analysis constraining the claim,
- a mass/power/consumables model consistent with the rest of the repository.

This rule applies especially to:

- survival duration claims,
- oxygen regeneration claims,
- thermal-control claims,
- power-availability claims,
- packaging claims,
- fault-tolerance claims.

---

## 15. What this repository must contain before it can be taken seriously

The minimum serious-repo content for IX-Breath is:

- mission requirements,
- master architecture,
- donor-feature keep/reject table,
- size and packaging envelope,
- real-world BOM,
- power architecture,
- life-support core architecture,
- thermal/control architecture,
- FDIR and state machine,
- sustainment/readiness architecture,
- mass/power/consumables model,
- validation plan,
- hazard analysis / FMEA,
- README last.

Anything less reads as an idea.
This repo is being built to read as a reviewable engineering proposal.

---

## 16. Exit criteria for repository phase 1

Repository phase 1 is complete only when:

1. the system architecture is coherent,
2. the donor-feature filter is explicit,
3. the retained subsystems are physically defensible,
4. the size target is bounded,
5. the BOM is real-world,
6. the failure logic is documented,
7. the validation plan is credible,
8. the README accurately reflects the bounded maturity level.

---

## 17. Authorship

**Author:** Bryce Lovell  
**Project name:** IX-Breath  
**License:** Apache-2.0  

IX-Breath is original systems-integration and architecture work intended to improve the seriousness, clarity, and survivability focus of an astronaut rescue-biased PLSS concept without overstating technical maturity.

---

## 18. Immediate next documents

The next repository documents after this charter are expected to define:

1. mission requirements and operating assumptions,
2. master system architecture,
3. donor-feature keep/reject matrix,
4. package envelope and suit integration posture,
5. real-world BOM.

That sequence is intentional.
It forces the repo to earn credibility before summary prose is written.
