# Team Leader Role — Onboarding & Configuration Blueprint

*Reviewed & enhanced — extends the KPI Integration Platform blueprint (Super Admin and
Admin/Co-Admin roles already documented) down to the **Team Leader** level: the role
assigned by an Admin to run a single team.*

---

## 1. Purpose

This document reviews the original Team Leader roadmap for the KPI Integration platform —
the process a Team Leader goes through on first login and in ongoing use — identifies
missing steps, logical gaps, and ordering issues, and presents a corrected, phase-based
blueprint ready for design and development.

---

## 2. Original Source, Transcribed in Full

**Team Leader**

- Here first team leader wants to assign team members to team activities. That should be
  the first one.
- And also he should want to have the ability to request member transfer.

**Features**

- Mark missed targets & let them know why they miss?

---

## 3. Issues Found in the Original Draft

| # | Issue | Fix |
|---|---|---|
| 1 | The draft jumps straight to "assign team members to activities" with no login, credential, or profile step — no account-entry point is defined for this role. | Add an onboarding phase consistent with the Admin/Super Admin pattern: Team Leader is **invited by an Admin** (role pre-assigned as Team Lead) with temp credentials, forced password reset, and MFA per org policy, then profile completion. |
| 2 | "Assign team members to team activities" is stated as the *first* action, but doesn't say whether the Team Leader creates the team itself. | Per the Admin blueprint, the **team and Team Lead assignment are created by the Admin**, not the Team Leader. The Team Leader's first real action is viewing their assigned team roster (read-only origin), then assigning existing members to activities/tasks within it — not team formation. |
| 3 | "Team activities" is undefined — unclear whether this means task assignment, KPI ownership assignment, scheduling, or something else. | Define explicitly: **activities are work items/tasks tied to the KPIs the Admin has already configured for the team** — assigning a member to an activity links that member's work to a measurable KPI, which is what makes "missed target" attribution possible later. |
| 4 | "Request member transfer" is phrased as if it's a direct, self-service action, but moving a member across teams/divisions is outside a Team Leader's own scope. | Express as a **request routed to the Admin for approval** — Team Leader submits, Admin approves/rejects. Never a unilateral transfer. |
| 5 | "Mark missed targets & let them know why they miss?" bundles detection, categorization, and notification into one ambiguous, self-questioning line — and implies the Team Leader manually decides what counts as "missed." | Split into three explicit steps: **(a)** the system auto-flags a missed target when actual performance falls outside the warning/critical bands already defined on the KPI (per the Admin-level KPI schema — target, direction, warning band, critical band); **(b)** Team Leader records a **reason/root-cause** for the miss; **(c)** system notifies the affected member automatically once the reason is logged. |
| 6 | No KPI or dashboard visibility step exists anywhere in the draft — a Team Leader can't meaningfully "mark a missed target" without first being able to see performance data. | Add a **KPI monitoring phase**: Team Leader views their team's KPI dashboard (own team only, per the visibility rule already established at Admin level: Team Lead → view own team's performance). |
| 7 | No alerts, notifications, or reporting step for this role. | Add scoped **alerts** (team-level threshold breaches, inherited from the org-wide escalation policy) and **team-level reports**, consistent with how Admin-level alerts/reports are scoped and inherited. |
| 8 | No stated boundaries — nothing says what a Team Leader *cannot* do (e.g., create teams, add new users to the org, manage data sources, configure KPIs from scratch). | Add an explicit boundaries section (Section 7) so the role's limits are unambiguous before build. |
| 9 | No audit trail for Team Leader actions (activity assignment, transfer requests, missed-target records). | All Team Leader actions write to the same **AuditLog** used elsewhere in the platform, visible to the Admin. |

---

## 4. Corrected Blueprint — 6 Phases

### Phase 1: Onboarding & Access

| Step | Item | Notes |
|---|---|---|
| 1 | **Invited by Admin** | Role pre-assigned as Team Lead at team-creation time (per Admin blueprint). |
| 2 | **Temp Credentials → Forced Reset → MFA** | Per org policy, same pattern as Admin onboarding. |
| 3 | **Profile Completion** | Name, contact, designation. |
| 4 | **View Assigned Team** | Read-only: team, division/sector, and existing members — created and assigned by the Admin, not authored here. |

### Phase 2: Team & Member Management

| Step | Item | Notes |
|---|---|---|
| 5 | **Assign Members to Activities** | Link existing team members to activities/tasks tied to the team's configured KPIs. Does **not** create new team members or teams. |
| 6 | **Request Member Transfer** | Submitted to the Admin for approval; status tracked (pending / approved / rejected). Not a unilateral action. |
| 7 | **View Team Roster** | Member profiles, current activity assignments, transfer-request status. |

### Phase 3: KPI Monitoring

| Step | Item | Notes |
|---|---|---|
| 8 | **Team KPI Dashboard** | Scoped to own team only, per the visibility rule set at Admin level. |
| 9 | **Individual Member Performance View** | Performance per member against target, direction, warning/critical bands — inherited from Admin-configured KPI schema. |
| 10 | **Status Indicators** | On-track / warning / critical, computed automatically from the KPI thresholds — not manually judged. |

### Phase 4: Target & Performance Management

| Step | Item | Notes |
|---|---|---|
| 11 | **Missed-Target Flagging** | Auto-flagged by the system when actual performance breaches the warning/critical band — Team Leader doesn't decide this manually. |
| 12 | **Record Reason / Root Cause** | Team Leader logs a reason (structured category + free-text note) against the flagged instance. |
| 13 | **Notify Affected Member** | Triggered automatically once the reason is logged. |
| 14 | **Track Follow-Up / Remediation** (optional) | Lightweight status field (e.g., open / addressed) so repeated misses are visible over time. |

### Phase 5: Alerts & Reporting

| Step | Item | Notes |
|---|---|---|
| 15 | **Team Alerts & Notifications** | Threshold-breach alerts scoped to own team; escalation structure itself is inherited org-wide, not configured here. |
| 16 | **Team-Level Reports** | Frequency/recipients/format inherited from org-wide templates, scoped to this team. |
| 17 | **Team Analytics / Trends** | Historical performance trends for own team only — no cross-team comparison. |

### Phase 6: Escalation & Governance

| Step | Item | Notes |
|---|---|---|
| 18 | **Escalate to Admin** | For persistent misses, unresolved transfer requests, or issues outside Team Leader's authority. |
| 19 | **Audit Logging** | All activity assignments, transfer requests, and missed-target records logged and visible to the Admin. |

---

## 5. Flow Diagram

```
Invited by Admin (role pre-assigned: Team Lead)
        │
Temp credentials → forced password reset → MFA (per org policy)
        │
Profile completion
        │
View assigned team (read-only — team & roster owned by Admin)
        │
Assign members to activities (tied to existing team KPIs)
        │
Request member transfer ──► Admin approval (pending/approved/rejected)
        │
Team KPI dashboard (own team only)
   └─ Individual member performance vs. target/direction/bands
        │
Missed target auto-flagged (system, based on KPI thresholds)
        │
Record reason / root cause ──► Notify affected member (automatic)
        │
Track follow-up / remediation (optional)
        │
Team alerts & notifications (scoped, org-wide escalation inherited)
        │
Team-level reports & analytics (scoped, org templates inherited)
        │
Escalate unresolved issues to Admin
        │
(ongoing) Audit log of all Team Leader actions — visible to Admin
```

---

## 6. Data Model — Team-Leader-Scoped Additions

Extends the entities already defined at Admin/Super Admin level; no parallel system.

- **TeamActivity** — `id`, `team_id`, `assigned_members[]`, `linked_kpi_id`, `status`.
- **TransferRequest** — `id`, `member_id`, `from_team_id`, `to_team_id`, `requested_by`
  (Team Leader), `status` (pending/approved/rejected), `approved_by` (Admin).
- **MissedTargetRecord** — `id`, `kpi_instance_id`, `member_id`, `period`,
  `reason_category`, `notes`, `flagged_at` (system), `logged_by` (Team Leader),
  `member_notified_at`, `follow_up_status`.
- **Permission** — Team Leader inherits the existing `own_team_only` view right on the KPI
  module; write access limited to activity assignment, transfer requests, and missed-target
  reason logging — never KPI definitions, data sources, or team creation.
- **AuditLog** — every activity assignment, transfer request, and missed-target record is
  logged and surfaced to the Admin.

No new services required — reuses the KPI Engine, Access Control, and Notification services
already defined, invoked with Team-Leader scope.

---

## 7. Boundaries vs. the Admin Role

- **Cannot** create teams, divisions, or add new users to the organization.
- **Cannot** approve their own member-transfer requests — Admin approval required.
- **Cannot** create or edit KPI definitions (target, direction, thresholds) — only view them
  and log reasons against missed instances.
- **Cannot** manage data sources, dashboards templates, or the org-wide escalation policy.
- **Cannot** see other teams' members, activities, or performance data.
- **Can** assign existing team members to activities, request transfers, monitor own-team
  KPI performance, record reasons for missed targets, and view own-team alerts/reports.

---

## 8. Summary of Changes

- **Added:** Full onboarding phase (invite → credentials → MFA → profile) — entirely absent
  from the original draft.
- **Clarified:** Team and roster are Admin-owned; Team Leader's first action is activity
  assignment within an existing team, not team formation.
- **Defined:** "Team activities" as work items tied to configured KPIs.
- **Reframed:** Member transfer as a request-and-approve workflow, not a direct action.
- **Split:** "Mark missed targets & let them know why" into auto-flagging (system),
  reason-logging (Team Leader), and notification (system) — three distinct steps.
- **Added:** KPI dashboard, individual performance view, and status indicators — missing
  entirely from the original draft, and required before a "missed target" can mean anything.
- **Added:** Alerts, reports, and analytics phase, scoped to the Team Leader's own team.
- **Added:** Explicit boundaries and audit logging, matching the pattern already established
  at Admin and Super Admin level.

---

## 9. Open Questions Worth Resolving Before Build

1. Can a Team Leader assign members to *any* activity freely, or only to activities/templates
   pre-defined by the Admin?
2. For a member transfer, does only the requesting Admin need to approve, or does the
   receiving team's Admin also need to sign off when the transfer crosses divisions?
3. Is "missed target" purely system-detected from the KPI thresholds, or should the Team
   Leader also be able to flag a miss manually in edge cases the system wouldn't catch?
4. Should Team Leaders be able to define lightweight, team-internal tracking metrics, or are
   they restricted to viewing only the KPIs the Admin has configured?
