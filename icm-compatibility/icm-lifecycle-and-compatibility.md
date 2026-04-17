# Identity and Consent Management (ICM)
## ICM Compatibility, Versioning, and Lifecycle Governance

## Scope and Purpose

This guideline establishes Identity and Consent Management (ICM) as a shared security capability with an independent lifecycle. It covers:
- how ICM versions evolve through lifecycle states (planned, security and consent requirements driven, etc.)
- how API-ICM compatibility is established and governed (via Minimum ICM Version declarations by APIs).

This guideline replaces implicit ICM compatibility assumptions with explicit, governed rules, ensuring that API semantics, security lifecycle, and compatibility evolution remain cleanly decoupled.

The guideline positions the yearly Signal meta-release as a coordination, communication and design evolution anchor, while ensuring that ICM lifecycle governance and compatibility enforcement operate continuously and independently of release cadence.

In this guideline, the term API version refers to the version of the API specification (not implementation).

## Definitions

- **API specification**: The definition of an API, including its structure, semantics, and version information.
- **API version**: an API specification identified by a specific `info.version` value.
- **API version evolution**: Changes to an API version that modify the functional API contract and result in a new API version.
- **API implementation**: A deployed realization of an API specification operated by an operator or other API provider.
- **ICM (Identity and Consent Management)**: The CAMARA guideline providing authentication, authorization, consent handling, and access profile definitions.
- **ICM lifecycle state**: The governance classification of an ICM version (Supported, Grace, Deprecated, Unsupported).
- **Preferred**: The ICM governance designation identifying an ICM version as the current design baseline for new API version development.
- **Supported**: An ICM lifecycle state indicating the ICM version is acceptable for new API version development and existing usage.
- **Grace**: An ICM lifecycle state indicating "design freeze": this ICM version SHALL NOT be use for new API version development. Existing API versions remain valid for deployment and operation.
- **Deprecated**: An ICM lifecycle state indicating enforced migration; continued compatibility requires an explicit, time-bound exception.
- **Unsupported**: An ICM lifecycle state indicating the end of the ICM version support; use is not permitted.
- **`minIcmVersion` (Minimum ICM Version)**: The lowest ICM version on which an API version is required to operate correctly. This is a property of the API version.
- **ICM compatibility for an API version**: The governed condition under which an API version can rely on a given ICM version higher or equal to its `minIcmVersion`, and the lifecycle state of that ICM version.
- **Signal meta-release**: The yearly CAMARA meta-release providing a planned Commonalities and ICM functional delivery point and a coordinated snapshot of the lifecycle state of all ICM versions. versions It is a communication and planning point for ICM, not an ICM lifecycle gate.
- **Sync meta-release**: The yearly CAMARA meta-release consolidating API version deliveries aligned with the preceding Signal Commonalities.

## Responsabilities

| Concept               | Who decides       | Scope                    |
| --------------------- | ----------------- | ------------------------ |
| ICM lifecycle state   | ICM governance    | Global (per ICM version) |
| Preferred ICM version | ICM governance    | Global design baseline   |
| `minIcmVersion`       | API Sub Project   | Per API version    |

## ICM Lifecycle States

Each ICM version SHALL be assigned exactly one lifecycle state: Supported, Grace, Deprecated, or Unsupported.
Exactly one ICM version SHALL be assigned the Preferred designation.

The Preferred indicator and the lifecycle state assignment for an ICM version is a governance action.

| ICM version     | ICM lifecycle state | Preferred      | Grace Period           |
| --------------- | ------------------- | -------------- | ---------------------- |
| **0.5.0**       | Supported           | yes (Signal26) |                        |
| 0.4.0           | Supported           | no (Fall25)    |                        |
| 0.3.0           | Grace               | no (Spring25)  | 18 months (Mar25-Sep26 |
| 0.2.1           | Deprecated          | no (Fall24)    | 8 months (Feb26-Sep26 - exception) |                       |
| 0.2.0           | Deprecated          | no  (2024)     |                        |
| 0.1.0           | Unsupported         | no  (2024)     |                        |

ICM versions oter than the one planned for the Signal meta-release MAY be delivered anytime due to changes in security or regulatory requirements and may override the state assignment.

### Supported state

An ICM version in Supported state MAY be used as the base for existing API version evolution, if no new functionality introduced by the Preferred Supported ICM version is neededis needed by that API version.
However, new APIs (v0.1.0) SHALL use the Preferred Supported ICM version.

### Grace state

An ICM version in Grace state indicates that any new API versions SHALL NOT be based on this ICM version.
An explicit Grace period is associated to this ICM version to allow API implementors to plan migration to a Supported ICM version. 

### Deprecated and Unsupported states

ICM versions in Deprecated or Unsupported state SHALL NOT be considered compatible for any API version.

Deprecated and Unsupported lifecycle states do not have an inherent duration. Neither state implies a grace period or automatic expiry.

### Time-Bound Exceptions

Any continued compatibility with an ICM version in a Grace, Deprecated or Unsupported state SHALL exist only through an explicitly approved, **time-bound exception**.

## Minimum ICM Version

ICM governance **does not** define a single, global “Minimum ICM Version” applicable to all APIs. 
Instead, ICM governance defines:
- lifecycle states for each ICM version (Supported, Grace, Deprecated, Unsupported), and
- the Preferred ICM version as the current design baseline.

ICM lifecycle states:
- constrain which ICM versions an API version **MAY or SHALL NOT** reference as  `minIcmVersion`
- **do not** mandate which ICM version an API version **SHALL** choose as `minIcmVersion`

The **Minimum ICM Version (`minIcmVersion`)** is a property of an **API version**, not a designation or property of an ICM version.

API Sub Projects **SHALL independently declare** the `minIcmVersion` for each API version, subject to the constraints imposed by the ICM lifecycle states.

Each CAMARA API version SHALL declare a **Minimum ICM Version** (`minIcmVersion`)
- it defines the lowest ICM version on which the specification is required to operate correctly.
- The Minimum ICM Version used at the time of declaration (API public release) SHALL be in Supported state.
- Below this Minimum ICM Version, this API version SHALL NOT be claimed to work.

Advancing the Minimum ICM Version is an API-level compatibility decision made by the API Sub Project, constrained by ICM lifecycle governance. 
(Please note that ICM governance does NOT define a global Minimum ICM Version.)

### Minimum ICM Version declaration by an API version

Declaration SHALL be done for a public API version by including the following in the API version:

```
x-camara-icm:
    minIcmVersion: x.y.z
    supportedAccessProfiles: // to be discussed if needed or if already covered by securityScheme info present in OAS
        - openId
        - oauth2-authorization-code (just an example)
        - bearer-xxx
    consentModel: standardized // possibly for later```
```

NOTE: If the additional fields are not needed (for now) we could just use: `x-camara-min-icm-version: x.y.z`

### API version compatibility with ICM versions

 To be **ICM-compatible**, development of API versions:
- (v0.1.0) SHOULD target the Preferred ICM version
- (> v0.1.0) MAY target any Supported ICM version
- SHALL NOT target ICM versions in Grace, Deprecated, or Unsupported state

ICM-compatibility:
- SHALL be guaranteed only down to and including the Minimum ICM Version
- SHALL NOT be guaranteed below the Minimum ICM Version
- SHALL only apply while the Minimum ICM Version is in Supported or Grace state (or by exception in Deprecated state)
- SHALL NOT extend indefinitely

### Advancing the Minimum ICM Version declared by an API version

When an ICM version enters in Grace state, this impacts the ICM-compatibility of the API versions that have declared this ICM version as their `minIcmVersion`.

To maintain ICM-compatibility, the impacted API versions
- SHALL NOT be used as a baseline for further API version evolutions
- MAY plan development of an new API version referencing a Supported `minIcmVersion`
- MAY plan an update of the `minIcmVersion` to a more recent ICM version after validating that the API version is compatible with that ICM version (re-release without API `info.version` change)
- MAY declare the API version as Deprecated

An ICM-compatibility exception is a governance-approved, time-bound authorization allowing temporary compatibility with a Deprecated or Unsupported ICM version for a defined scope. 

Any exception allowing APIs to depend on Deprecated or Unsupported ICM versions:
- SHALL be time‑bound
- SHALL be scoped
- SHALL have an explicit owner
- SHALL include documented risk acceptance

#### Mandatory advancement

An API Sub Project **SHALL advance** the `minIcmVersion` declared in an API version when the currently declared `minIcmVersion` enters a lifecycle state that (ventually ) does no longer permit compatibility (e.g. Grace, Deprecated or Unsupported).

This advancement:
- updates the API–ICM compatibility contract of the API version
- **does not** by itself require creation of a new API version

If the API is compatible with a newer Supported version of ICM
  - plan to advance the API version's `minIcmVersion` to the Supported ICM version 
  - do a new API release **without changing the API version**. This release only changes the `minIcmVersion` value, but the API `info.version` stays the same.

If the `minIcmVersion` of this API version cannot be advanced in a compatible way to a newer ICM version,
  - this API version becomes **ICM-incompatible**
  - plan to evolve the API version to a new API version with a `minIcmVersion` in Supported state 
  - API implementors should plan to migrate deployments to the newer ICM-compatible API version.

#### Optional advancement

An API API Sub Project **MAY** choose to advance the declared `minIcmVersion` proactively, for example to:
- align with the Preferred ICM version,
- reduce long-term maintenance burden,
- simplify future API evolution.

Such decisions remain **API-specific**.

## Decoupling of API Version Evolution and ICM

Multiple versions of the same CAMARA API MAY reference the same ICM version in their `minIcmVersion`.

API version evolution (impacting `info.version`) SHALL be governed by changes to the functional API contract, not by ICM version changes.

The change of the declared Minimum ICM Version (`minIcmVersion`) SHALL be governed by ICM lifecycle rules, not by ICM release cadence.

## Role of the Signal Meta-Release

The yearly Signal meta-release SHALL deliver planned functional evolutions of the ICM baseline. 
Other ICM versions MAY be delivered anytime due to changes in security or regulatory requirements.

The Signal meta-release SHALL serve as a coordination point. 
It SHALL NOT act as a mandatory approval, blocking, or authorization step for ICM lifecycle transitions nor for API ICM compatibility changes. It

- SHALL designate the Preferred ICM version. There is always only one Preferred ICM version.
- SHALL remove the Preferred designation from the previous Preferred ICM version
- SHALL report snapshot on lifecycle state of all ICM versions

The reporting snapshot SHALL communicate the following information for all ICM versions:
- the lifecycle state
- Preferred assignment 
- time period for ICM versions in Grace state
- time-bound exceptions for ICM versions in Deprecated/Unsupported state

It SHALL update the API-ICM compatibility matrix.

API Sub Projects developing new API versions for the subsequent Sync meta-release or on an independent track MAY use any ICM version in Supported state. 
They select the applicable ICM version based on API required ICM functionality.

## API Implementor Responsibilities

API implementors MAY continue running implementations of API versions on an ICM version until this version enters Deprecated state. 

An ICM version that enters Grace state signals to API implementors that 
- the API versions referencing this ICM version remain ICM compatible deployments for the duration of the Grace period
- they SHALL plan implementation migration of these API versions to a Supported ICM version within the Grace period

Time-bound exceptions MAY be assigned following a recorded governance action.

## ICM Lifecycle State Transitions

This section defines the complete lifecycle state machine for ICM versions.

### Definitions

- **Preferred** (label): A label indicating the current design baseline for new API versions development. Preferred is a designation applied to exactly one Supported ICM version at a time and is not a lifecycle state.
- **Supported** (state): Acceptable for evolved API version development and existing usage.
- **Grace** (state): Design-freeze state. Existing API versions remain valid for deployment and operation, but no new API versions may be based on this version.
- **Deprecated** (state): Enforcement state. General compatibility is not permitted; continued compatibility requires an explicit, time-bound exception.
- **Unsupported** (state): Terminal state. Support obligation for ICM version has ended; compatibility is not permitted and no exceptions are allowed.

### Allowed Transitions

Under normal governance conditions, ICM lifecycle state transitions SHALL follow the sequence:
Supported → Grace → Deprecated → Unsupported.

Normal ICM lifecycle transitions:
- New ICM Version → **Supported**: The new ICM version is designated Preferred, and the previously Preferred Supported version SHALL loose the Preferred designation.
- **Supported → Grace**: Allowed only via explicit governance decision. The Grace period SHALL be determined and documented at the moment of transition.
- **Grace → Deprecated**: Allowed after the Grace period has elapsed and migration paths are available. This transition enforces migration.
- **Deprecated → Unsupported**: Allowed via explicit governance approval when ICM version support obligation ends.

### Forbidden Transitions

The following direct transitions are forbidden under normal ICM lifecycle state governance:
- Supported → Deprecated
- Supported → Unsupported
- Grace → Supported
- Deprecated → Grace
- Unsupported → any other state

### Security and Regulatory Overrides

Security, privacy, or regulatory requirements MAY require exceptional lifecycle state assignment overrides. 
In such cases, governance MAY mandate an immediate transition from Supported directly to Deprecated or Unsupported without a preceding Grace period.

Such overrides:
- SHALL be explicitly approved and documented
- SHALL be communicated as exceptional measures
- SHALL NOT establish precedent for normal lifecycle transitions

Condistions that may trigger such exceptional overrides are for example:
- critical security vulnerabilities
- legal non‑compliance
- regulatory prohibition
- systemic risk

### Lifecycle Overview Diagram

![ICM Lifecycle State Transitions](ICM_Lifecycle_State_Transitions.png)

## API–ICM Compatibility Matrix

### Purpose
The API–ICM Compatibility Matrix provides the authoritative, ecosystem-wide view of which CAMARA API versions are compatible with which ICM versions at a given point in time.
It is a time-based view of compatibility derived from API declarations, ICM lifecycle states, and explicit exceptions.

### Scope and Authority
The matrix SHALL be owned and maintained by CAMARA governance responsible for ICM lifecycle management. 
It SHALL be authoritative and SHALL NOT be owned or edited by individual API Sub Projects.

### Relationship to API Versions
Each API version SHALL declare its own `minICMVersion`. 
The matrix derives compatibility from the API-declared `minICMVersion`, ICM lifecycle states, and approved exceptions. 
It does not introduce new rules.

### Relationship to ICM Lifecycle States
- Preferred / Supported: compatible subject to `minICMVersion`
- Grace: compatible for existing API specifications only
- Deprecated / Unsupported: not compatible by default; compatibility only via explicit, time-bound exception

### Ownership and Update Timing
The matrix SHALL be updated whenever:
- an ICM lifecycle state changes
- an API version is published or advances its `minICMVersion`
- an exception is approved, modified, or expires
- a security or regulatory override is applied

The matrix SHALL NOT be gated by Signal or Sync cadence.

### Exceptions
Exceptions SHALL be explicitly represented, time-bound, scoped, and removed automatically on expiry.

## Compatibility Matrix Update Checklist
- Trigger identified (lifecycle state change, API version update, exception)
- Matrix updated immediately
- Validation of lifecycle constraints
- Communication to impacted API Sub Projects/API implementaors (operators)

## Example

```
apiIcmCompatibilityMatrix:
  metadata:
    schemaVersion: "1.2"
    publishedAt: "2026-03-15T10:00:00Z"
    validAsOf: "2026-03-15"
    publishedBy: "CAMARA ICM Governance"
    notes: "Post-Signal26 compatibility snapshot"

  icmVersions:
    - icmVersion: "0.5.0"
      lifecycleState: Supported
      preferred: true
      lifecycleEffectiveDate: "2026-02-01"

    - icmVersion: "0.4.0"
      lifecycleState: Supported
      preferred: false
      lifecycleEffectiveDate: "2025-10-01"

    - icmVersion: "0.3.0"
      lifecycleState: Grace
      preferred: false
      lifecycleEffectiveDate: "2025-03-01"
      gracePeriod:
        startDate: "2025-03-01"
        endDate: "2026-09-30"
        indicativeDuration: "18 months"
        plannedNextState: Deprecated

    - icmVersion: "0.2.1"
      lifecycleState: Deprecated
      preferred: false
      lifecycleEffectiveDate: "2024-10-01"

    - icmVersion: "0.1.0"
      lifecycleState: Unsupported
      preferred: false
      lifecycleEffectiveDate: "2024-01-01"

  apiVersions:
    - apiName: "QualityOnDemand"
      apiVersion: "v1.2"
      owningWorkingGroup: "QoD WG"
      minIcmVersion: "0.3.0"

      compatibility:
        - icmVersion: "0.5.0"
          compatible: true
          compatibilityType: Normal
          rationale: "Meets minIcmVersion and lifecycle allows compatibility"

        - icmVersion: "0.3.0"
          compatible: true
          compatibilityType: GraceExistingOnly
          rationale: "ICM version in Grace; existing API version allowed"

        - icmVersion: "0.2.1"
          compatible: true
          compatibilityType: Exception
          rationale: "Temporary regulatory transition"
          exception:
            exceptionId: "EX-2026-02"
            approvedBy: "CAMARA ICM Governance"
            approvalDate: "2026-02-15"
            expiry:
              type: Date
              value: "2026-09-30"
            migrationPlan: "Migrate to ICM 0.4.0"
            owner: "QoD WG"

        - icmVersion: "0.1.0"
          compatible: false
          compatibilityType: NotCompatible
          rationale: "ICM version Unsupported"
```

## yaml schema for compatibility matrix

```
apiIcmCompatibilityMatrix:
  metadata:
    schemaVersion: string          # Version of this schema (e.g. "1.1")
    publishedAt: string            # ISO-8601 timestamp of this snapshot
    validAsOf: string              # ISO-8601 date from which this snapshot is authoritative
    publishedBy: string            # Governance owner (e.g. "CAMARA ICM Governance")
    notes: string                  # Optional free text notes
  icmVersions:
    - icmVersion: string           # e.g. "0.5.0"
      lifecycleState:              # Note: Preferred is NOT a lifecycle state
        enum:
          - Supported
          - Grace
          - Deprecated
          - Unsupported
      preferred: boolean             # true if this version is the Preferred design baseline
      lifecycleEffectiveDate: string # ISO-8601 date lifecycleState became effective
      gracePeriod:                   # REQUIRED if lifecycleState == Grace, not present otherwise
        startDate: string            # ISO-8601 date Grace period started
        endDate: string              # ISO-8601 date Grace period ends
        indicativeDuration: string   # Human-readable (e.g. "18 months")
        plannedNextState: Deprecated # e.g. "Deprecated"
  apiVersions:
    - apiName: string                # e.g. "QualityOnDemand"
      apiVersion: string             # e.g. "1.2.0"
      owningApiRepository: string    # API repository name (or API Sub Project name 
      minIcmVersion: string          # Declared minIcmVersion of the API version
      compatibility:
        - icmVersion: string         # ICM version from ICM versions list
          compatible: boolean        # true / false
          compatibilityType:
            enum:
              - Normal               # Meets minIcmVersion and lifecycle allows compatibility
              - GraceExistingOnly    # Allowed only for existing API versions
              - Exception            # Allowed via explicit, time-bound exception
              - NotCompatible        # Not compatible under governance rules
          rationale: string          # Short explanation (human- or machine-readable)
          exception:                 # Required only if compatibilityType == Exception
            exceptionId: string
            approvedBy: string
            approvalDate: string     # ISO-8601 date
            expiry:
              type:
                enum:
                  - Date
                  - Condition
              value: string          # ISO-8601 date or condition
            migrationPlan: string
            owner: string
```
