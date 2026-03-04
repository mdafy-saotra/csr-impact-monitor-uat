# UAT Plan — CSR Impact Monitor
**CommCare Mobile Application & Web Dashboard**
Version: 1.0 | March 2026
Prepared by: QA / Implementation Team
Reference Documents: Functional Spec v1.0 · User Scenarios v1.0

---

## 1. UAT Objectives

- Verify that all CommCare forms capture, validate, and save data correctly as specified in the Functional Spec v1.0
- Confirm case hierarchy (community → beneficiary → activity cases) is correctly created and closed
- Validate conditional logic, auto-calculations, and field-level validations across all modules
- Confirm role-based access and permissions work as intended for all four user roles
- Verify that the web dashboard displays accurate KPIs, filters, and exports correctly
- Confirm offline data capture and sync behaviour on mobile devices

---

## 2. Test Environment

| Item | Details |
|---|---|
| Application | CSR Impact Monitor — CommCare HQ (UAT project space) |
| Mobile Device | Samsung A32 (Android) — representative of field agent devices |
| Connectivity Test | Online (WiFi), Offline (airplane mode), then sync |
| Test Users | One per role: Field Agent, Site Supervisor, CSR Manager, ESG Director |
| Lookup Tables | mine_sites table pre-loaded with at least 2 test sites (incl. Koumba Gold Mine) |
| Languages | Test in both English and French |

---

## 3. UAT Roles & Responsibilities

| Role | Responsibilities |
|---|---|
| Field Agent Tester (e.g. Fanja) | Execute mobile form scenarios (Modules 1–6) |
| Site Supervisor Tester (e.g. Jean-Pierre) | Execute grievance follow-up, consultation log, supervisor dashboard review |
| CSR Manager Tester (e.g. Marie) | Validate web dashboard KPIs, filters, and report exports |
| ESG Director Tester (e.g. Edouard) | Validate read-only access and cross-site aggregated views |
| UAT Coordinator | Log defects, track pass/fail, manage sign-off |

---

## 4. Entry & Exit Criteria

**Entry Criteria (UAT may begin when):**
- App is deployed to UAT project space in CommCare HQ
- All lookup tables (mine_sites) are pre-loaded
- Test user accounts created with correct roles and location assignments
- Test devices have latest app version installed

**Exit Criteria (UAT is complete when):**
- All P1 (critical) test cases pass with no open defects
- All P2 (major) defects resolved or formally accepted
- Dashboard KPIs validated against known test data
- Sign-off obtained from CSR Manager and Site Supervisor representatives

---

## 5. Defect Severity Classification

| Severity | Definition | Example |
|---|---|---|
| P1 — Critical | Blocks core workflow; data loss risk | Form fails to submit; case not created |
| P2 — Major | Incorrect behaviour but workaround exists | Conditional field appears at wrong time |
| P3 — Minor | Cosmetic or UX issue | Label typo; field ordering |
| P4 — Enhancement | Out of scope for MVP | New field request |

---

## 6. UAT Test Scenarios

---

### MODULE 1 — Community & Beneficiary Registration

---

#### UAT-01 · Community Registration (Form 1A)

**Role:** Field Agent | **Device:** Mobile (online)
**Reference:** Functional Spec §4.1 · User Scenario 1

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 1.1 | Open app, sync over WiFi. Navigate to Community & Beneficiary Registration → Community Registration | Form 1A opens with blank fields | | |
| 1.2 | Enter community name "Tsarahonenana" | Field accepts up to 100 chars; saved as case name | | |
| 1.3 | Tap GPS field; stand at community center | Coordinates auto-captured (lat/lon populated) | | |
| 1.4 | Select "Koumba Gold Mine" from mine_site lookup | Dropdown populated from lookup table; selection saved as parent reference | | |
| 1.5 | Enter estimated_population = 350, num_households = 72 | Integers accepted; validation passes (within 1–999,999 and 1–99,999) | | |
| 1.6 | Enter distance_from_mine_km = 12.5 | Decimal accepted; validation passes (0.1–500) | | |
| 1.7 | Select primary_livelihoods: farming + fishing (multi-select) | Both values saved | | |
| 1.8 | Set has_health_facility = No, has_school = Yes, has_clean_water = No | All three saved correctly | | |
| 1.9 | Enter community leader name and phone number | Fields accept text/phone format | | |
| 1.10 | Take a photo of the village entrance | Photo attached successfully | | |
| 1.11 | Submit form | Community case created in CommCare; registration_date auto-set to today | | |
| 1.12 | Log in as Site Supervisor; verify community appears in supervisor case list | Case visible to supervisor | | |

**Negative Tests:**
- Enter population = 0 → validation error expected
- Enter distance = 600 → validation error expected
- Leave community_name blank → form should not submit

---

#### UAT-02 · Beneficiary Registration (Form 1B)

**Role:** Field Agent | **Device:** Mobile (online)
**Reference:** Functional Spec §4.2 · User Scenario 2

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 2.1 | Open Beneficiary Registration; select "Tsarahonenana" from case list | Form 1B opens with community pre-selected | | |
| 2.2 | Enter full name: Rasoa Vololomanga | Saved as case name | | |
| 2.3 | Enter DOB: 15/03/1991 | Date picker works; no future date accepted | | |
| 2.4 | Verify age is auto-calculated as 34 | age field = 34 (calculated from today – DOB) | | |
| 2.5 | Confirm vulnerability_status field appears (age ≥ 5) | Field visible; select widow + chronic_illness | | |
| 2.6 | Set primary_occupation = farmer (age ≥ 18, so no default to student) | Farmer selected; no auto-default to student | | |
| 2.7 | Set csr_programs_enrolled = health, education, livelihood | Multi-select; all three saved | | |
| 2.8 | Set consent_given = Yes | Form allows submission | | |
| 2.9 | Submit form | Beneficiary case created as child of Tsarahonenana community case | | |
| 2.10 | Verify beneficiary case is visible under Tsarahonenana in supervisor view | Case hierarchy correct | | |

**Negative Tests:**
- Set consent_given = No → form should not submit
- Enter DOB as future date → validation error expected
- Enter household_size = 51 → validation error expected (max 50)
- Verify vulnerability_status does NOT appear for a beneficiary aged < 5

---

### MODULE 2 — Health Program Monitoring

---

#### UAT-03 · Health Activity Report (Form 2A)

**Role:** Field Agent | **Device:** Mobile (online)
**Reference:** Functional Spec §5.1 · User Scenario 3

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 3.1 | Navigate to Health Program Monitoring → select Tsarahonenana → Health Activity Report | Form 2A opens | | |
| 3.2 | Confirm activity_date defaults to today | Date pre-filled with today's date | | |
| 3.3 | Select activity_type = health_education | health_topic field appears; num_children_under5 does NOT appear | | |
| 3.4 | Select health_topic = maternal_care | Field accepts selection; saved correctly | | |
| 3.5 | Enter num_beneficiaries_male = 0, num_beneficiaries_female = 28 | Both fields accept values ≥ 0 | | |
| 3.6 | Verify num_beneficiaries_total auto-calculates as 28 | Total = 0 + 28 = 28 | | |
| 3.7 | Enter referrals_made = 3 | Value saved | | |
| 3.8 | Select supplies_distributed = vitamins | Multi-select saves correctly | | |
| 3.9 | Capture GPS at activity location | GPS auto-captured | | |
| 3.10 | Submit form | New health_visit case created under Tsarahonenana; visible in supervisor dashboard | | |
| 3.11 | Select activity_type = vaccination | Verify num_children_under5 field NOW appears | | |
| 3.12 | Enter activity_date = 31 days ago | Validation error expected (not > 30 days past) | | |

---

#### UAT-04 · Health Visit Follow-up (Form 2B)

**Role:** Field Agent | **Device:** Mobile
**Reference:** Functional Spec §5.2 · User Scenario 4

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 4.1 | Open Health Visit Follow-up; select the open health_visit from UAT-03 | Form 2B opens with case details | | |
| 4.2 | Set followup_status = completed | close_case auto-set to true | | |
| 4.3 | Enter outcome_notes describing referral results | Free text saved | | |
| 4.4 | Submit form | health_visit case auto-closed; no longer appears in open case list | | |
| 4.5 | Verify referral completion rate updates in Health dashboard | Marie's dashboard reflects updated rate | | |
| 4.6 | Repeat with followup_status = cancelled | Case also auto-closes | | |
| 4.7 | Repeat with followup_status = ongoing | Case remains open | | |

---

### MODULE 3 — Education Program Monitoring

---

#### UAT-05 · Education Activity Report (Form 3A)

**Role:** Field Agent | **Device:** Mobile
**Reference:** Functional Spec §6.1 · User Scenario 5

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 5.1 | Navigate to Education Program Monitoring → Tsarahonenana → Education Activity Report | Form 3A opens | | |
| 5.2 | Select education_type = school_supplies | scholarship_amount and training_duration_days do NOT appear | | |
| 5.3 | Enter institution_name = "École Primaire Tsarahonenana" | Text saved | | |
| 5.4 | Enter num_students_male = 22, num_students_female = 23 | Fields accept values | | |
| 5.5 | Verify num_students_total auto-calculates as 45 | Total = 22 + 23 = 45 | | |
| 5.6 | Select age_group = 6_to_12 | Saved correctly | | |
| 5.7 | Set completion_status = completed | Case should auto-close on submit | | |
| 5.8 | Submit form | education_activity case created and immediately auto-closed | | |
| 5.9 | Select education_type = scholarship | scholarship_amount field appears | | |
| 5.10 | Select education_type = vocational_training | training_duration_days field appears | | |

---

### MODULE 5 — Environmental Monitoring

---

#### UAT-06 · Environmental Monitoring Report (Form 5A)

**Role:** Field Agent | **Device:** Mobile
**Reference:** Functional Spec §8.1 · User Scenario 6

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 6.1 | Navigate to Environmental Monitoring → Tsarahonenana → Environmental Monitoring Report | Form 5A opens | | |
| 6.2 | Select monitoring_type = water_quality | water_ph and water_turbidity fields appear; reforestation/biodiversity fields do NOT appear | | |
| 6.3 | Enter water_ph = 5.2 | Decimal accepted; validation passes (0–14) | | |
| 6.4 | Select water_turbidity = turbid | Value saved | | |
| 6.5 | Set risk_level = high | Saved correctly | | |
| 6.6 | Set corrective_action_needed = yes | corrective_action_desc field appears | | |
| 6.7 | Enter corrective action description | Free text saved | | |
| 6.8 | Capture photo_evidence_1 (required) | Photo attached; form allows submission | | |
| 6.9 | Submit without photo_evidence_1 | Validation error — photo required | | |
| 6.10 | Submit form | env_monitoring case created with risk_level = high; case remains OPEN | | |
| 6.11 | Verify Jean-Pierre receives alert for high-risk case | Supervisor notified (per workflow config) | | |
| 6.12 | Select monitoring_type = reforestation | trees_planted field appears; water fields do NOT appear | | |
| 6.13 | Enter water_ph = 15 | Validation error expected (0–14 only) | | |

---

### MODULE 6 — Stakeholder Engagement & Grievances

---

#### UAT-07 · Grievance Registration (Form 6A)

**Role:** Field Agent | **Device:** Mobile
**Reference:** Functional Spec §9.1 · User Scenario 7

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 7.1 | Navigate to Stakeholder Engagement → Tsarahonenana → Grievance Registration | Form 6A opens | | |
| 7.2 | Leave reporter_name blank (anonymous) | Form accepts blank — anonymous reporting allowed | | |
| 7.3 | Select grievance_category = infrastructure_damage, severity = high | Both saved | | |
| 7.4 | Enter description with fewer than 20 characters | Validation error — min 20 chars required | | |
| 7.5 | Enter description with 28+ characters | Validation passes | | |
| 7.6 | Capture GPS at affected location and attach photo | Both saved | | |
| 7.7 | Set assigned_to = Jean-Pierre Mbeki | Saved correctly | | |
| 7.8 | Submit form | Grievance case created with resolution_status = open (auto-set) | | |
| 7.9 | Verify Open Grievances count increments on Marie's dashboard | Dashboard count updated | | |

---

#### UAT-08 · Grievance Follow-up / Resolution (Form 6B)

**Role:** Site Supervisor | **Device:** Mobile or Web
**Reference:** Functional Spec §9.2 · User Scenario 8

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 8.1 | Open Grievance Follow-up; select the open infrastructure_damage grievance | Form 6B opens with case details | | |
| 8.2 | Enter action_taken description | Free text saved | | |
| 8.3 | Set resolution_status = resolved | community_satisfied field appears; days_to_resolve auto-calculated | | |
| 8.4 | Verify days_to_resolve = correct number of days since report_date | Calculation accurate (followup_date − report_date) | | |
| 8.5 | Set community_satisfied = yes | Value saved | | |
| 8.6 | Submit form | Grievance case auto-closed | | |
| 8.7 | Verify Marie's dashboard shows updated resolution rate and avg. resolution time | KPIs updated | | |
| 8.8 | Set resolution_status = closed_unresolved | Case also auto-closes (no community_satisfied field) | | |
| 8.9 | Set resolution_status = investigating | Case remains open | | |

---

#### UAT-09 · Community Consultation Log (Form 6C)

**Role:** Site Supervisor | **Device:** Mobile
**Reference:** Functional Spec §9.3 · User Scenario 9

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 9.1 | Open Community Consultation Log | Form 6C opens; no case pre-selection required | | |
| 9.2 | Select consultation_type = town_hall | Value saved | | |
| 9.3 | Select community from community lookup: Tsarahonenana | Community referenced but no case opened | | |
| 9.4 | Enter num_attendees_male = 20, num_attendees_female = 25 | Values saved | | |
| 9.5 | Select topics_discussed: employment, environment, compensation, infrastructure | Multi-select; all four saved | | |
| 9.6 | Enter key_outcomes text | Free text saved | | |
| 9.7 | Set follow_up_required = yes | Saved correctly | | |
| 9.8 | Submit form | Form submitted as standalone data — NO case created | | |
| 9.9 | Verify consultation appears in Edouard's GRI 14 / ICMM stakeholder engagement count | Dashboard count updated | | |

---

### OFFLINE & SYNC TESTING

---

#### UAT-10 · Offline Data Capture and Sync

**Role:** Field Agent | **Device:** Mobile
**Reference:** Functional Spec §1.1

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 10.1 | Enable airplane mode on device | No network connection | | |
| 10.2 | Open app — verify previously synced cases are accessible | Case list loads from local storage | | |
| 10.3 | Complete Form 1A (Community Registration) offline | Form completes and queues for sync | | |
| 10.4 | Complete Form 2A (Health Activity) offline | Form queues for sync | | |
| 10.5 | Re-enable WiFi; sync app | All queued forms submitted; cases created in CommCare HQ | | |
| 10.6 | Verify data integrity — GPS, photos, and case properties all correctly saved post-sync | No data loss | | |

---

### ROLE-BASED ACCESS CONTROL

---

#### UAT-11 · User Role & Permissions Verification

**Reference:** Functional Spec §2

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 11.1 | Log in as Field Agent (Fanja) | Mobile app only; cannot access web dashboard | | |
| 11.2 | Verify Field Agent can only see cases in their assigned community (case sharing OFF) | No access to other agents' cases | | |
| 11.3 | Log in as Site Supervisor (Jean-Pierre) | Mobile + web access; can see all cases in their mine site location group | | |
| 11.4 | Log in as CSR Manager (Marie) | Web dashboard access; can view all data and export reports | | |
| 11.5 | Log in as ESG Director (Edouard) | Dashboard access only; read-only; cannot edit or export raw data | | |
| 11.6 | Verify ESG Director cannot submit forms or modify cases | No edit access | | |

---

### WEB DASHBOARD & REPORTING

---

#### UAT-12 · Overview Dashboard & Filters

**Role:** CSR Manager | **Tool:** CommCare Web Dashboard
**Reference:** Functional Spec §10 · User Scenario 10

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 12.1 | Log in as Marie; navigate to CSR Impact Monitor dashboard | Dashboard loads with Overview tab | | |
| 12.2 | Apply filter: Date Range = Q1 2026, Mine Site = Koumba Gold Mine | Dashboard updates to show filtered data only | | |
| 12.3 | Verify 4 summary cards: Total Communities, Total Beneficiaries, Total Activities, Open Grievances | All four cards populate with correct counts | | |
| 12.4 | Verify community map widget plots registered communities with GPS coordinates | All communities displayed on map | | |
| 12.5 | Verify bar chart shows activities by program area (current vs. previous quarter) | Chart renders correctly | | |
| 12.6 | Verify beneficiary gender split pie chart | Correct % female displayed | | |
| 12.7 | Navigate to Health tab | Beneficiary reach, referral rate, top health topics displayed | | |
| 12.8 | Navigate to Environment tab | High-risk events flagged; open corrective actions visible | | |
| 12.9 | Navigate to Grievances tab | Open vs. resolved count, avg. resolution time, satisfaction rate shown | | |
| 12.10 | Click "Export GRI 14 Report" | CSV downloads with correct column structure | | |
| 12.11 | Verify Worker Performance tab shows forms submitted per agent and last submission date | Data matches field activity | | |

---

#### UAT-13 · KPI Accuracy Verification

**Role:** CSR Manager | **Reference:** Functional Spec §11

| # | Test Step | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 13.1 | Verify Beneficiary Gender Ratio = (female count / total count) × 100 | Matches manual calculation from test data | | |
| 13.2 | Verify Health Referral Rate = (sum referrals_made / count health_visits) × 100 | Matches manual calculation | | |
| 13.3 | Verify Grievance Resolution Rate = (resolved / total) × 100 | Matches manual calculation | | |
| 13.4 | Verify Community Satisfaction Rate = (satisfied 'yes' / count resolved) × 100 | Matches manual calculation | | |
| 13.5 | Verify Education Gender Parity Index = female students / male students (target 0.97–1.03) | Displayed correctly; flagged if out of range | | |
| 13.6 | Verify Communities with No Activity (last 30 days) correctly identifies inactive communities | Only communities with zero activity cases in 30 days listed | | |

---

## 7. UAT Sign-off Checklist

| Area | Tester | Sign-off Date | Status |
|---|---|---|---|
| Module 1 — Registration | Field Agent | | |
| Module 2 — Health | Field Agent | | |
| Module 3 — Education | Field Agent | | |
| Module 5 — Environment | Field Agent | | |
| Module 6 — Grievances (Form 6A/6B) | Field Agent + Supervisor | | |
| Module 6 — Consultation (Form 6C) | Site Supervisor | | |
| Offline & Sync | Field Agent | | |
| Role-Based Access | UAT Coordinator | | |
| Web Dashboard & KPIs | CSR Manager | | |
| GRI 14 Export | CSR Manager | | |
| Overall UAT Sign-off | CSR Manager + Project Lead | | |

---

*Document Version 1.0 — March 2026 | Based on Functional Spec v1.0 and User Scenarios v1.0*
*Module 4 (Livelihood) UAT scenarios to be added in Q2 2026 once livelihood program activities begin.*
