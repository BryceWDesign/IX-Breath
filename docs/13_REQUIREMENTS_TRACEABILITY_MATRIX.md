# IX-Breath Requirements Traceability Matrix

**Document ID:** IXB-TRACE-13  
**Revision:** A  
**Status:** Active baseline  
**Repository phase:** Architecture study closeout matrix  
**Parent documents:**  
- `docs/00_PROJECT_CHARTER.md`
- `docs/01_MISSION_REQUIREMENTS.md`
- `docs/03_MASTER_ARCHITECTURE.md`
- `docs/05_PACKAGING_ENVELOPE.md`
- `docs/06_REAL_WORLD_BOM.md`
- `docs/07_POWER_CONTINUITY_SPINE.md`
- `docs/08_LIFE_SUPPORT_CORE.md`
- `docs/09_THERMAL_RESOURCE_COUPLING.md`
- `docs/10_FDIR_AND_STATE_MACHINE.md`
- `docs/11_MODEL_BASELINE.md`
- `docs/12_VALIDATION_AND_HAZARD_PLAN.md`

---

## 1. Purpose

This document closes the loop between:

- stated requirements,
- retained architecture decisions,
- analytical model posture,
- and planned validation/hazard attack paths.

A serious aerospace-facing repository should not force the reviewer to manually guess whether the requirements are actually carried through the design.

This matrix exists so the reviewer can answer four questions quickly:

1. Is each requirement actually represented in the architecture?
2. Which document owns that requirement technically?
3. What kind of evidence is planned for it?
4. Is the requirement only defined, analytically supported, or actually test-backed?

At the current repo maturity, most requirements are **architecturally mapped** and some are **analytically supported**, but none are being passed off as already flight-validated.

That is the correct posture.

---

## 2. Traceability status legend

| Status | Meaning |
|---|---|
| **Mapped** | Requirement is explicitly represented in the architecture and repository structure. |
| **Mapped + Modeled** | Requirement is represented in the architecture and has analytical treatment in the model. |
| **Mapped + Planned Validation** | Requirement is represented and has an explicit validation attack path. |
| **Open** | Requirement exists, but the repository still needs more detailed closure later. |

---

## 3. Verification legend

| Code | Meaning |
|---|---|
| **A** | Analysis |
| **I** | Inspection |
| **T** | Test |
| **D** | Demonstration |
| **R** | Review of objective evidence |

---

## 4. Review-family traceability summary

| Requirement family | Primary requirement IDs | Primary owning docs | Primary evidence posture | Current status |
|---|---|---|---|---|
| Mission framing and claim bounds | IXB-REQ-001 to 006 | `00_PROJECT_CHARTER`, `03_MASTER_ARCHITECTURE`, `README`, `12_VALIDATION_AND_HAZARD_PLAN` | I / R | Mapped |
| Life-support core and oxygen continuity | IXB-REQ-100 to 112 | `08_LIFE_SUPPORT_CORE`, `06_REAL_WORLD_BOM`, `10_FDIR_AND_STATE_MACHINE` | I / A / T | Mapped + Planned Validation |
| Electrical continuity and protected bus logic | IXB-REQ-200 to 210 | `07_POWER_CONTINUITY_SPINE`, `06_REAL_WORLD_BOM`, `10_FDIR_AND_STATE_MACHINE` | I / A / T | Mapped + Planned Validation |
| Thermal survivability and lighting realism | IXB-REQ-300 to 306 | `09_THERMAL_RESOURCE_COUPLING`, `05_PACKAGING_ENVELOPE`, `11_MODEL_BASELINE` | I / A / T | Mapped + Modeled |
| Packaging and human-integration posture | IXB-REQ-400 to 406 | `05_PACKAGING_ENVELOPE`, `03_MASTER_ARCHITECTURE`, `06_REAL_WORLD_BOM` | I / A | Mapped |
| FDIR, state logic, latching, and crew posture | IXB-REQ-500 to 509 | `10_FDIR_AND_STATE_MACHINE`, `07_POWER_CONTINUITY_SPINE`, `08_LIFE_SUPPORT_CORE` | I / A / T / D | Mapped + Planned Validation |
| Sensing, data, and sustainment readiness | IXB-REQ-600 to 606 | `03_MASTER_ARCHITECTURE`, `06_REAL_WORLD_BOM`, `10_FDIR_AND_STATE_MACHINE`, `12_VALIDATION_AND_HAZARD_PLAN` | I / R / D / T | Mapped + Planned Validation |
| Analytical/model discipline | IXB-REQ-700 to 705 | `11_MODEL_BASELINE`, `09_THERMAL_RESOURCE_COUPLING`, `12_VALIDATION_AND_HAZARD_PLAN` | A | Mapped + Modeled |
| Validation seriousness and anti-hype posture | IXB-REQ-800 to 805 | `04_DONOR_FEATURE_FILTER`, `12_VALIDATION_AND_HAZARD_PLAN`, `README` | I / R / A | Mapped |

---

## 5. Architectural ownership map

### 5.1 S1 / S2 / S3 / S4 ownership
The following requirement groups are primarily carried by the life-support and oxygen/water architecture:

- IXB-REQ-100 to 112
- IXB-REQ-507 (oxygen / CO2 alert posture)
- IXB-REQ-600
- IXB-REQ-606

Primary documents:
- `docs/08_LIFE_SUPPORT_CORE.md`
- `docs/06_REAL_WORLD_BOM.md`
- `docs/10_FDIR_AND_STATE_MACHINE.md`

### 5.2 S6 ownership
The following requirement groups are primarily carried by the power continuity spine:

- IXB-REQ-200 to 210
- portions of IXB-REQ-500 to 509
- portions of IXB-REQ-600 to 601
- portions of IXB-REQ-700 to 703

Primary documents:
- `docs/07_POWER_CONTINUITY_SPINE.md`
- `docs/06_REAL_WORLD_BOM.md`
- `docs/10_FDIR_AND_STATE_MACHINE.md`
- `docs/11_MODEL_BASELINE.md`

### 5.3 S5 ownership
The following requirement groups are primarily carried by the thermal/resource coupling layer:

- IXB-REQ-300 to 306
- portions of IXB-REQ-205
- portions of IXB-REQ-702 to 703

Primary documents:
- `docs/09_THERMAL_RESOURCE_COUPLING.md`
- `docs/05_PACKAGING_ENVELOPE.md`
- `docs/11_MODEL_BASELINE.md`

### 5.4 S7 ownership
The following requirement groups are primarily carried by control, FDIR, and crew interface:

- IXB-REQ-500 to 509
- portions of IXB-REQ-109 to 110
- portions of IXB-REQ-209 to 210
- portions of IXB-REQ-600 to 604

Primary documents:
- `docs/10_FDIR_AND_STATE_MACHINE.md`
- `docs/07_POWER_CONTINUITY_SPINE.md`
- `docs/08_LIFE_SUPPORT_CORE.md`

### 5.5 S8 ownership
The following requirement groups are primarily carried by shell, routing, and package containment discipline:

- IXB-REQ-400 to 406
- portions of IXB-REQ-301
- portions of IXB-REQ-501
- portions of IXB-REQ-606

Primary documents:
- `docs/05_PACKAGING_ENVELOPE.md`
- `docs/06_REAL_WORLD_BOM.md`
- `docs/03_MASTER_ARCHITECTURE.md`

### 5.6 S9 ownership
The following requirement groups are primarily carried by readiness and sustainment logic:

- IXB-REQ-602 to 605
- portions of IXB-REQ-603 to 606
- IXB-REQ-800 to 805

Primary documents:
- `docs/03_MASTER_ARCHITECTURE.md`
- `docs/12_VALIDATION_AND_HAZARD_PLAN.md`
- `README.md`

---

## 6. Analytical closure map

The following requirement families are not merely stated; they are also attacked analytically in the model baseline:

| Requirement family | Model support |
|---|---|
| IXB-REQ-700 | explicitly satisfied by `docs/11_MODEL_BASELINE.md` |
| IXB-REQ-701 | satisfied by defended, stretch, and outer-edge case structure |
| IXB-REQ-702 | satisfied by LC1 / LC2 / LC3 separation |
| IXB-REQ-703 | satisfied by explicit deny logic and non-credit rules |
| IXB-REQ-704 | satisfied by repeated distinction between architecture study and proven hardware |
| IXB-REQ-705 | satisfied by validation linkage in `docs/12_VALIDATION_AND_HAZARD_PLAN.md` |

---

## 7. Validation ownership map

The following requirement groups already have explicit planned attack paths in the repo:

| Requirement family | Validation ownership |
|---|---|
| Life-support continuity | VA-3 / VA-7 families, especially VAL-004, VAL-005, VAL-006 |
| Power continuity | VA-2 family, especially VAL-002 and VAL-003 |
| Regeneration honesty | VA-4 / VA-5 / VA-7 families, especially VAL-007 and VAL-008 |
| Thermal derating and resource truth | VA-5 / VA-7 families, especially VAL-009 and VAL-014 |
| FDIR and state logic | VA-6 / VA-7 families, especially VAL-010 and VAL-012 |
| Sustainment/readiness | VA-8 family, especially VAL-013 |

---

## 8. Open items

Even with this traceability matrix, the repository remains honest about what is still open.

The biggest open areas are:

1. final subsystem sizing
2. detailed thermal-loop topology
3. final reserve-oxygen trigger thresholds
4. final controller timing and watchdog implementation
5. environmental and integration testing details
6. actual bench evidence to move mapped requirements into test-backed status

These are not defects in the repo.

They are exactly what should remain open at architecture-study maturity.

---

## 9. Closeout statement

With this matrix in place, IX-Breath now has:

- a project charter
- explicit requirements
- defined operating assumptions
- a master architecture
- a donor-feature keep/reject record
- a packaging posture
- a real-world BOM
- detailed subsystem architecture
- an analytical closure model
- a validation and hazard architecture
- and a requirements traceability map tying those pieces together

That is enough for IX-Breath to read like a serious engineering architecture repository rather than a loose concept dump.

It is still not flight hardware.
It is not pretending to be.

It is now, however, structurally complete as a serious phase-1 repo.
