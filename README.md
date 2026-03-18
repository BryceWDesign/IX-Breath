# IX-Breath

**IX-Breath** is a **rescue-biased regenerative portable life support system (PLSS) architecture study** for astronaut survival after delayed ingress, separation from a station/habitat/vehicle, or other contingency conditions where ordinary backup duration may be insufficient.

This repository does **not** present IX-Breath as flight-qualified hardware.  
It presents IX-Breath as a **bounded, reviewable engineering architecture** built to answer one serious question:

> Can a backpack-class life-support system materially extend astronaut survivability by integrating closed-loop breathing support, layered oxygen continuity, hardened electrical continuity, explicit fault isolation, and sustainment-readiness logic into one coherent rescue-first system?

The repo’s answer is:

- **yes, analytically, for a 24-hour defended case**
- **possibly, under bounded favorable assumptions, for 36–48 hour stretch cases**
- **not yet seriously enough for a 72-hour baseline claim**

That distinction is deliberate.

---

## Repository status

**Status:** Architecture study  
**Maturity posture:** Reviewable systems-engineering concept, not validated flight hardware  
**License:** Apache-2.0

This repository is written to be taken seriously by technical reviewers. That means:

- claims are bounded
- constraints are explicit
- rejected ideas are documented
- failure logic is first-class
- validation requirements are defined before any summary language is allowed to oversell the concept

---

## What IX-Breath is

IX-Breath is a backpack-class concept built around five linked ideas:

1. **Closed-loop breathing support**  
   A recirculating gas loop with CO2 control, humidity handling, circulation continuity, and oxygen make-up.

2. **Layered oxygen continuity**  
   Primary stored oxygen, an **isolated emergency oxygen reserve**, and a **bounded regenerative oxygen extension path** based on qualified water and gated electrolysis.

3. **Hardened electrical continuity**  
   A protected power spine with battery-first architecture, supercapacitor ride-through, source isolation, precharge, bus segmentation, safe-dump behavior, and aggressive load shedding.

4. **Thermal and resource honesty**  
   Oxygen regeneration is only credited when **water, power, thermal margin, and control confidence** all support it at the same time.

5. **Sustainment as a safety function**  
   The repo treats readiness, calibration evidence, life-limited parts, and blocker visibility as real survivability architecture, not admin overhead.

---

## What IX-Breath is not

IX-Breath is **not**:

- a claim of flight qualification
- a replacement claim for NASA or commercial EVA suit programs
- a proof that multi-day backpack survival has been demonstrated
- an excuse for “infinite oxygen” or “infinite power” language
- an exotic-energy project
- a stealth, weapon, or defense concept
- a mashup that blindly imports earlier IX-* repo claims

This repo explicitly rejects:

- Element 115 dependence
- zero-point / vacuum-energy claims
- ambient RF as primary life-support power
- triboelectric / piezoelectric primary power claims
- perpetual-motion framing
- vague field effects used as system justification

Only engineering patterns that survived the donor-feature filter were retained.

---

## Why this repository exists

Publicly available EVA and PLSS materials make two things clear:

1. contingency duration matters
2. backpack volume, mass, center of gravity, and thickness matter just as much

IX-Breath exists in that tension.

The design goal is **not** “make a magical forever backpack.”  
The design goal is **make a more survivable rescue-biased backpack without lying about physics, packaging, or failure behavior.**

---

## Current bounded conclusions

At the current repository maturity level, the following are the correct conclusions:

### Defended analytical conclusion
A **24-hour defended survival target** is analytically plausible in a dark/eclipsed case **without solar credit** and **without depending on PEM regeneration**.

### Stretch analytical conclusion
A **36-hour** stretch case is analytically plausible only with:
- strong rescue-lean load shedding
- modest accepted solar in mixed-light conditions
- bounded resource gating
- disciplined system posture

A **48-hour** favorable sunlit stretch case is analytically plausible only with:
- low activity
- favorable lighting
- useful accepted solar offset
- meaningful but bounded regeneration credit
- preserved thermal margin

### Non-baseline conclusion
A **72-hour** case does **not** close seriously enough to advertise as a baseline repo claim.

That is the honest answer, and it is intentional.

---

## Current physical posture

IX-Breath is constrained to remain **backpack-class**.

### Nominal worn-envelope target
- **Height:** 25 inches
- **Width:** 20 inches
- **Thickness:** 10.5 inches

### Current modeled mass posture
- **Dry mass band:** 74–108 lb
- **Working midpoint:** 89 lb
- **Serviced mass band:** 78–114 lb
- **Working serviced midpoint:** 95 lb

Those are architecture-level model numbers, not measured hardware values.

The repo treats thickness and center-of-gravity discipline as first-order constraints.  
If later features blow up thickness or shift too much mass outward, the architecture has failed its own rules.

---

## System architecture summary

IX-Breath is organized as the following system stack:

- **S0 — Pressure Garment Interface**
- **S1 — Life-Support Core**
- **S2 — Oxygen Storage and Delivery**
- **S3 — Regenerative Extension**
- **S4 — Water Recovery and Conditioning**
- **S5 — Thermal Survivability Layer**
- **S6 — Power Continuity Spine**
- **S7 — Supervisory Control / FDIR / Crew Interface**
- **S8 — Structural Survivability Shell**
- **S9 — Sustainment and Readiness Layer**

The architecture is intentionally layered so that survivability does not depend on one “hero subsystem.”

---

## Control and safety posture

IX-Breath uses six bounded top-level states:

- **Nominal**
- **Conserve**
- **Rescue**
- **Critical**
- **Safe-Hold**
- **Service**

The pack is intentionally biased toward **conservative, explicit, latched behavior** when confidence drops.

Key safety behaviors include:

- reserve oxygen isolation from routine depletion
- watchdog authority over unsafe supervisory behavior
- confidence-aware state freedom
- explicit denial of regeneration when affordability is not proven
- anti-thrash transition logic
- safe-hold as a real protected state, not a cosmetic pause mode

---

## Resource-gating posture

One of the central rules of IX-Breath is:

> Oxygen regeneration does not count unless water, power, thermal headroom, and control confidence all permit it at the same time.

That means:

- sunlight alone does not justify PEM operation
- recovered moisture alone does not justify PEM operation
- the existence of an electrolyzer alone does not justify PEM operation
- denied regeneration is treated as a **valid survival decision**, not a design failure

This is one of the repo’s main anti-hype guardrails.

---

## Donor-repo posture

IX-Breath was informed by earlier IX-* concept work, but only after an explicit keep/adapt/reject filter.

What survived were mainly:

- bus segmentation and fault isolation patterns
- precharge / no-backfeed / safe-dump architecture
- watchdog and bounded-state logic
- thermal zoning and compartmentalization habits
- readiness / evidence / blocker workflow patterns
- structured logging and post-event review posture

What did **not** survive were the unsupported mechanisms and speculative energy claims.

The donor-feature filter is a core seriousness document in this repo, not a footnote.

---

## Repository map

The core documents are arranged so a technical reviewer can move from purpose to architecture to evidence posture without digging through fluff.

### Core program definition
- `docs/00_PROJECT_CHARTER.md`
- `docs/01_MISSION_REQUIREMENTS.md`
- `docs/02_OPERATING_ASSUMPTIONS.md`

### Architecture core
- `docs/03_MASTER_ARCHITECTURE.md`
- `docs/04_DONOR_FEATURE_FILTER.md`
- `docs/05_PACKAGING_ENVELOPE.md`
- `docs/06_REAL_WORLD_BOM.md`

### Detailed subsystem architecture
- `docs/07_POWER_CONTINUITY_SPINE.md`
- `docs/08_LIFE_SUPPORT_CORE.md`
- `docs/09_THERMAL_RESOURCE_COUPLING.md`
- `docs/10_FDIR_AND_STATE_MACHINE.md`

### Analytical and validation posture
- `docs/11_MODEL_BASELINE.md`
- `docs/12_VALIDATION_AND_HAZARD_PLAN.md`

### References
- `docs/99_REFERENCES.md`

### Machine-readable support files
- `data/ix_breath_bom.csv`
- `data/ix_breath_power_bus_map.csv`
- `data/ix_breath_life_support_modes.csv`
- `data/ix_breath_resource_gating_matrix.csv`
- `data/ix_breath_state_transition_matrix.csv`
- `data/ix_breath_model_inputs.csv`
- `data/ix_breath_case_results.csv`
- `data/ix_breath_validation_matrix.csv`
- `data/ix_breath_hazard_register.csv`

---

## What this repo presently supports

At its current maturity, this repository supports:

- serious architecture review
- subsystem and system-level critique
- packaging review
- donor-feature screening transparency
- analytical closure review
- validation planning
- hazard and FMEA discussion
- future breadboard / engineering-model planning

It does **not** yet support:

- flight claims
- mission certification claims
- environmental qualification claims
- human-rated hardware claims
- production-readiness claims

---

## What a serious reviewer should find here

A serious reviewer should be able to conclude the following after reading the repo:

- the problem statement is bounded
- the architecture is coherent
- the subsystem flows are explicit
- the donor influences were filtered instead of blindly inherited
- the survivability claims are limited by real resource accounting
- the design does not rely on exotic unsupported mechanisms
- the repo admits what it has **not** yet proven
- the next logical step would be disciplined validation, not marketing

That is the target outcome.

---

## Current strongest claim

The strongest honest claim this repository makes right now is:

> **IX-Breath is a serious rescue-biased regenerative PLSS architecture study with enough systems structure, bounded modeling, and validation planning to justify deeper engineering review and disciplined bench-level investigation.**

That is where the repo stops.

Anything stronger would be maturity inflation.

---

## Recommended reading order

For a fast technical review:

1. `docs/00_PROJECT_CHARTER.md`
2. `docs/03_MASTER_ARCHITECTURE.md`
3. `docs/04_DONOR_FEATURE_FILTER.md`
4. `docs/05_PACKAGING_ENVELOPE.md`
5. `docs/06_REAL_WORLD_BOM.md`
6. `docs/11_MODEL_BASELINE.md`
7. `docs/12_VALIDATION_AND_HAZARD_PLAN.md`

For a deeper subsystem review, continue through:
- `docs/07_POWER_CONTINUITY_SPINE.md`
- `docs/08_LIFE_SUPPORT_CORE.md`
- `docs/09_THERMAL_RESOURCE_COUPLING.md`
- `docs/10_FDIR_AND_STATE_MACHINE.md`

---

## Development posture

If IX-Breath advances beyond repo phase, the next serious move is **not** a polished render or a marketing deck.

The next serious move is a staged validation program:

- power-spine bench rig
- gas-loop bench rig
- water/regen bench rig
- thermal/resource coupling rig
- integrated nonhuman engineering article
- fault-injection campaign
- service/readiness workflow validation

That sequence is already defined in the repo.

---

## Author

**Bryce Lovell**

---

## License

Apache License 2.0.  
See `LICENSE` and `NOTICE`.
