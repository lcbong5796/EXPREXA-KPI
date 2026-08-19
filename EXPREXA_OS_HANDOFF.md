# EXprexa OS — Trial Audit and Improvement Handoff

## Outcome

The original trial is a useful employee engagement layer. It supports worker PINs, role-based daily targets, proof submission, admin review, points, side quests, badges, leaderboards and rewards. It should be retained, but it must sit above verified logistics transactions rather than operate as a standalone points app.

This improvement pass adds:

- EXprexa OS branding and operating-system positioning.
- A Shipment Control Tower for MAWB/job records.
- Seven job stages: Pre-alert, Plan, Declare, Screen, Pull-out, Deliver and Billed.
- Green, Amber, Red and Black operational risk.
- Internal, External and None delay attribution.
- Accountable owner, deadline, exception and next action.
- Recorded job revenue, direct cost and contribution margin.
- EXprexa KPI weighting and incentive gates.
- Explicit source-of-truth and evidence rules.

## What the current trial does well

1. Simple worker experience suitable for mobile use.
2. Role templates automatically generate daily targets.
3. Photo/note proof and admin approval create basic evidence control.
4. Side quests and badges make improvement work visible.
5. Points ledger and reward catalogue are easy to understand.
6. Malaysia-time daily reset is appropriate for Exprexa.

## Critical gaps before production

### 1. Authentication and permissions

The current PIN/admin-password check runs in the browser. Replace it with Firebase Authentication or the company's identity provider and role-based access:

- Managing Director
- Operations Head
- Department Head
- Supervisor / Reviewer
- Employee
- Finance / HR
- System Administrator

Firestore security rules must enforce access on the server side. A user must not be able to read or write another employee's private data merely by changing browser data or URLs.

### 2. Database design

Several datasets are stored as one JSON array in one Firestore document. This creates size limits, overwrite races and weak auditability. Use one document per entity:

- employees
- departments
- roles
- jobs / mawbs
- job_stages
- task_definitions
- task_instances
- submissions
- review_decisions
- exception_events
- kpi_results
- incentive_runs
- ledger_entries
- reward_redemptions
- audit_logs

### 3. KPI linkage

Daily generic tasks may support behaviour scoring, but operational points must be generated from a real source record:

`MAWB/job → stage/task → assignee → SLA → timestamp → evidence → reviewer → score`

No source record means no operational KPI points.

### 4. Fair delay attribution

Every overdue task needs a controlled reason code and evidence. Recommended external codes:

- Customs pending / inspection
- AKPS screening capacity or hold
- Terminal congestion / equipment / forklift constraint
- Airline offload / late arrival / missing cargo
- Customer document or payment pending
- Weather / road closure / force majeure

External Pending remains visible in SLA reporting but does not reduce the employee's controllable performance score after supervisor validation.

### 5. Bonus governance

Use:

`Earned Incentive = Approved Pool × KPI Factor × Company Factor × Compliance Factor`

- KPI factor: 0%–120%.
- Company factor: gross-profit and cash-collection threshold.
- Compliance factor: customs, safety, honesty and evidence gate.
- Serious customs/safety breach: freeze payout pending review.
- Falsified data: zero payout plus investigation.
- Approved external delay: no employee penalty.

### 6. Financial controls

Contribution margin per job:

`Revenue − transport/subcontractor − terminal/handling − direct labour/OT − claims/errors − special job costs`

The production system must import or reconcile with Finance rather than depend on unrestricted manual revenue and cost entry.

## Recommended release sequence

### Phase 1 — Airport + Forwarding pilot (first 30 days)

- MAWB registration and ownership.
- Declare, screen and pull-out missions.
- SLA timestamps and evidence.
- External-delay validation.
- Daily red/black exception meeting.
- KPI simulation only; no bonus payout yet.

### Phase 2 — KPI and incentive control (days 31–60)

- Department and position scorecards.
- Reviewer workflow and audit log.
- Compliance gates.
- Monthly incentive simulation.
- Dispute and correction workflow.

### Phase 3 — Warehouse, Fleet and Finance (days 61–90)

- Receiving, release, dispatch and POD stages.
- Truck allocation, fuel and utilisation.
- Billing within 24 hours of job completion.
- Job contribution margin and unbilled-job alerts.

### Phase 4 — AI and management automation

- Delay risk radar.
- Manpower/truck recommendation.
- Profit leakage detection.
- Customer update drafting.
- Daily MD exception brief.
- Recurring-incident and SOP-learning analysis.

## Month-12 success measures

- 70% fewer routine Managing Director interventions.
- 100% of active jobs have an owner, status and deadline.
- 100% of operational KPI scores are evidence-linked.
- Completed jobs billed within the approved SLA, targeted at 24 hours.
- Customer, service and route contribution margin visible monthly.
- 3–5 percentage-point gross-margin improvement target.

