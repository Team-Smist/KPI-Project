# Admin (Co-Admin) Role — Onboarding & Configuration Blueprint

*Merged edition — combines the detailed issue-by-issue review (Revision 2) with the
phase-based restructuring, into a single reference document.*

This extends the KPI Integration Platform blueprint (Super Admin onboarding review) down to
the **Admin / Co-Admin level** — the role a Super Admin invites to run a division, sector, or
department.

---

## 1. Purpose

This document reviews the original admin-onboarding roadmap for the KPI Integration
platform (the sequence of steps an admin follows on first login and setup), identifies
sequencing and logic issues, and presents a corrected, phase-based blueprint ready for use
in design and development.

---

## 2. Original Source, Transcribed in Full

1. Username/Password → Strong
2. Profile Creation
3. Department details
4. Admin can make another admins, Assign group members
   - Admin
   - Add team leader
5. Divisions/Sectors
6. Assign divisions to them
7. Data Source Management
   - Data Base
   - File Source
   - API
   - IOT Sources
8. Data Source Validation → Open MetaData
9. KPI Configuration (User) — Team performance measure
   - KPI Creation → Target
   - KPI Formula
10. Dashboard & Report Configuration
11. Alert & Notification configuration
12. Team Creation — KPI can be viewed by team lead & others; others cannot see others'
    performance
13. User creation / User invitation
14. Assign users
15. Assign Data & KPI access
16. Final security & permission
17. Organization architecture

---

## 3. Issues Found in the Original Flow

| # | Issue | Fix |
|---|---|---|
| 1 | Organization architecture was listed last (step 17), but departments and divisions can't be meaningfully created without an org structure defined first. | Move org architecture to the **start** of the flow, not the end. At the Admin level this is a **read-only** auto-generated view — structure is owned by the Super Admin's org structure builder, not authored here. |
| 2 | Step 1 gives the Admin credentials with no mention of how they entered the system. | Admin receives **temp credentials at invite time** (Super Admin's "Invite co-admins" step), then forced password reset + MFA per org policy on first login. "Strong" becomes an enforced password-policy check. |
| 3 | Step 4 bundles three distinct actions — creating another Admin, assigning group members, and adding a team leader — into one line, with no stated limit on nested-admin creation. | Split into three explicit actions. Any Admin created by another Admin must inherit a **scope no broader than the creator's own** and a **permission set no broader than the creator's own**. All admin-creates-admin events stay visible to the Super Admin in the audit log, regardless of delegation depth. |
| 4 | "Add team leader" appears in step 4 *and* again implicitly in step 12 (Team Creation), risking conflicting leads set at different times. | Consolidate: team lead is assigned **once, at team creation**. |
| 5 | Team creation appeared after KPI configuration, even though KPI visibility rules ("team lead can view, others cannot") depend on teams already existing. | Move **team creation before KPI configuration** in the corrected flow. |
| 6 | Steps 5–6 ("Divisions/Sectors," "Assign divisions to them") don't say whether the Admin creates new divisions or assigns existing ones. | Org structure (divisions/branches/departments) is created once by the Super Admin's org structure builder. The Admin **selects from and assigns existing** divisions/sectors within their own scope — not authoring new top-level structure. |
| 7 | Step 7 (Database / File Source / API / IoT) lists source types with no credential capture, connection test, or field mapping. | Reuse the Data Integration Service as specified at Super Admin level: **encrypted credential capture, mandatory "Test Connection," field mapping to KPI variables, refresh frequency per source** — scoped to sources authorized for the Admin's division. |
| 8 | Step 8 ("Data Source Validation → Open MetaData") conflates data *validation* with data *cataloging* — OpenMetadata is a governance/catalog tool, not a validation engine. | Split: **(a)** data quality rules (nulls, duplicates, schema checks) the Admin configures for their sources, and **(b)** catalog/governance tooling, inherited from whatever the Super Admin already selected org-wide. |
| 9 | Step 9 has KPI Creation → Target and KPI Formula, but no **direction** (higher/lower-is-better), **warning/critical thresholds**, or **owner**. | KPI creation must capture: target value, direction, warning band, critical band, and an owner accountable for that KPI at team level — same schema as the Super Admin's KPI step, scoped to the Admin's team(s). |
| 10 | Step 10 doesn't say whether report definitions are new or inherit the org-wide reporting model. | Reuse the same frequency/recipients/format model; recipients and dashboards are scoped to the Admin's division/team, org-wide templates are inherited. |
| 11 | Step 11 doesn't say whether the Admin can define their own escalation policy or must plug into the org-wide one. | Admin configures **trigger conditions and recipients within their scope**; the **escalation structure itself is org-wide**, set once by the Super Admin. |
| 12 | Step 12's visibility rule was a comment, not an enforceable rule. | Express as an explicit permission-matrix entry: Team Lead → view own team's KPI performance; Team member → view own performance only; cross-team visibility off by default, overridable only by Admin/Super Admin. |
| 13 | User creation and "assign users" were separated from team creation despite belonging to the same workflow phase, and had no stated difference from each other. | Group into one phase. **User creation/invitation** = inviting a person and assigning role at invite time. **Assign users** = assigning an *existing* user to a team/division, usable any time. |
| 14 | Step 15 ("Assign Data & KPI access") needs a grant vs. revoke distinction. | Two actions writing to the same permission matrix used everywhere else: **grant** access to a data source/KPI for a user or team, and **modify/revoke** existing access. |
| 15 | Security & permissions appeared only as a single final step. | Apply access control incrementally as each entity (admin, user, data source, KPI) is created, **then** review everything at the end as a gate before go-live — not configured only once. |
| 16 | No explicit role-based access control (RBAC) step, though multiple permission levels are implied (Super Admin, Admin, Team Lead, User). | **Added**: explicit RBAC step formalizing these permission levels. |
| 17 | No audit logging / activity tracking step. | **Added**: audit logging step — standard for any admin system managing KPI and organizational data. |

---

## 4. Corrected Blueprint — 6 Phases

### Phase 1: Organizational Foundation

| Step | Item | Notes |
|---|---|---|
| 1 | **Organization Architecture** | Auto-generated, **read-only** at the Admin level (owned by Super Admin's org structure builder); shown up front for context, not authored here. |
| 2 | **Department Details** | Admin's own department context within the existing structure. |
| 3 | **Divisions / Sectors** | Admin **selects from and is assigned** existing divisions/sectors — does not create new top-level structure. |

### Phase 2: Admin Account Setup

| Step | Item | Notes |
|---|---|---|
| 4 | **Username / Password** | Temp credentials at invite time → forced reset → MFA per org policy. Enforced password-policy check. |
| 5 | **Admin Profile Creation** | Capture profile details (name, contact, designation). |
| 6 | **Create Sub-Admins** (optional) | Scope and permissions capped at ≤ creator's own; every event logged and visible to Super Admin regardless of delegation depth. |
| 7 | **Assign Group Members** | Assign members to admin groups. |

### Phase 3: Team & User Management

| Step | Item | Notes |
|---|---|---|
| 8 | **Team Creation** | Must exist before KPI visibility rules are applied. |
| 9 | **Assign Team Lead** | Assigned **once, at team creation** — single source of truth. |
| 10 | **User Creation / Invitation** | Create or invite users; role assigned at invite time. |
| 11 | **Assign Users** | Assign *existing* users to teams/divisions; usable any time, not just at invite. |
| 12 | **Define Role Permissions (RBAC)** | Formalize permission levels: Super Admin, Admin, Team Lead, User. Visibility rule: Team Lead → own team; Member → own performance only; cross-team view off by default. |

### Phase 4: Data Integration

| Step | Item | Notes |
|---|---|---|
| 13 | **Data Source Management** | Database, File Source, API, IoT — encrypted credential capture, mandatory Test Connection, field mapping, refresh frequency; scoped to sources authorized for the Admin's division. |
| 14 | **Data Source Validation** | Schema/metadata + data-quality rules (nulls, duplicates, schema checks) before a source feeds KPIs. Catalog/governance tooling is inherited org-wide, not chosen here. |

### Phase 5: KPI & Reporting Configuration

| Step | Item | Notes |
|---|---|---|
| 15 | **KPI Configuration** | Formula, target, **direction**, **warning band**, **critical band**, and **owner** per KPI, scoped to the Admin's team(s). |
| 16 | **Dashboard & Report Configuration** | Inherits org-wide templates; recipients/dashboards scoped to Admin's division/team. |
| 17 | **Alerts & Notifications** | Admin sets trigger conditions and recipients within scope; escalation structure itself is org-wide. |

### Phase 6: Access Governance

| Step | Item | Notes |
|---|---|---|
| 18 | **Assign Data & KPI Access** | Grant and modify/revoke access for a user or team — same permission matrix as elsewhere. |
| 19 | **Final Security & Permission Review** | Editable summary of every step above, click-to-jump-back-and-edit, before go-live. |
| 20 | **Audit Logging** | Activity/access logs enabled for admin and data-access actions (compliance & traceability). |

---

## 5. Flow Diagram

```
Organization architecture (read-only review, owned by Super Admin)
        │
Invited by Super Admin (role pre-assigned)
        │
Temp credentials → forced password reset → MFA (per org policy)
        │
Admin profile completion
        │
Department details
        │
Select assigned Division(s)/Sector(s)   ← read-only, structure owned by Super Admin
        │
Team creation
   └─ Assign team lead (single source of truth)
   └─ Auto-applies performance-visibility rule (Lead sees team; members see self only)
        │
User invitation (role assigned at invite) ──┐
Assign existing users to teams/divisions ───┤  both feed the same
Grant / modify / revoke data & KPI access ──┘  permission matrix
        │
Define role permissions (RBAC)
        │
Data source setup (within authorized scope)
   ├─ Credentials + connection test per source
   ├─ Field mapping (source fields → KPI variables)
   ├─ Refresh frequency per source
   └─ Data quality rules (+ inherited governance tier)
        │
KPI configuration (team-level)
   ├─ KPI creation: formula, owner
   └─ Target, direction, warning band, critical band
        │
Dashboard & report config (scoped, inherits org templates)
        │
Alert & notification config (scoped, plugs into org escalation policy)
        │
Create additional Admins (optional)
   └─ Scope + permissions capped at ≤ creator's own; visible in Super Admin audit log
        │
Final security & permission review (editable summary)
        │
Audit logging enabled
        │
Go live — Admin dashboard
        │
(ongoing) "Add user" quick-action available from Team screen
```

---

## 6. Data Model — Admin-Scoped Additions

Extends the entities already defined for the platform; no parallel system.

- **User** — gains `invited_by` (Admin or Super Admin), `scope` (division/sector IDs).
- **Team** — belongs to a Division/Sector, has one Team Lead (User), member Users.
- **Role** — Admin-assignable roles (including further Admin roles) are constrained to
  `scope ⊆ creator.scope` and `permissions ⊆ creator.permissions`.
- **Permission** — team performance visibility expressed as a scoped `view` right on the KPI
  module: `own_team_only` vs. `own_data_only`.
- **DataSource** — unchanged structure; each instance carries a `scope` limiting which Admin
  can see/manage it.
- **AuditLog** — every admin-creates-admin event, and every data/permission-access change,
  is logged here and surfaced to the Super Admin regardless of delegation depth.

No new services required — this reuses Onboarding, Data Integration, KPI Engine, Access
Control, and Notification services from the main blueprint, invoked with a narrower scope.

---

## 7. Boundaries vs. the Super Admin Role

- **Cannot** create new organizations, top-level divisions/branches, or change org-wide
  security policy (password policy, MFA requirement, SSO/SAML, data residency).
- **Cannot** create an admin, team, or role with broader scope/permissions than their own.
- **Cannot** redefine the org-wide notification escalation structure — only plug into it.
- **Can** manage users, teams, KPIs, data sources, dashboards, and access within their
  assigned division(s)/sector(s), including delegating a constrained subset of that to
  further Admins.

---

## 8. Summary of Changes

- **Reordered:** Organization architecture moved from last to first (for context), while
  remaining a read-only, Super-Admin-owned view rather than an authoring step.
- **Reordered:** Team creation moved before KPI configuration, since visibility rules depend
  on teams already existing.
- **Grouped:** User creation, assignment, and team-lead designation combined into one phase.
- **Added:** Explicit RBAC step.
- **Added:** Audit logging step.
- **Reframed:** Security & permissions as both an ongoing practice (each phase) and a final
  review gate.
- **Clarified:** Admin-creates-admin delegation is scope/permission-capped and fully
  audit-logged.
- **Clarified:** Data source validation split into data-quality rules vs. inherited
  governance/cataloging.
- **Clarified:** KPI schema requires direction, warning/critical thresholds, and an owner.

---

## 9. Open Questions Worth Resolving Before Build

1. Is there a **maximum delegation depth** (Super Admin → Admin → Admin → …), or is it
   unbounded as long as scope/permissions only shrink at each level?
2. Can one Admin manage **multiple** divisions/sectors, or exactly one?
3. Is the KPI **owner** always the Team Lead, or can it be any user regardless of team role?
4. For "Organization architecture" — is a read-only auto-generated view sufficient, or is
   there a specific editing capability intended here that isn't captured elsewhere?
