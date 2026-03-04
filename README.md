# CSR Impact Monitor — UAT Test Suite
> CommCare MVP | Mining CSR Program | Dimagi
> Version: 1.0 | March 2026

---

## 📁 Repository Structure

```
csr-impact-monitor-uat/
│
├── README.md                        ← This file
│
├── jira/
│   └── jira_uat_import.csv          ← Jira CSV import (48 test cases)
│
├── excel/
│   └── CSR_Impact_Monitor_UAT_Workbook.xlsx   ← Excel workbook (4 sheets)
│
└── docs/
    ├── UAT_Plan.md                  ← Full UAT plan (narrative + test scenarios)
    └── functional_spec_v1.0.pdf     ← Reference: Functional Specification
```

---

## 📋 What's in This Repo

| File | Description |
|---|---|
| `jira/jira_uat_import.csv` | Ready for Jira CSV importer. 48 test cases with Summary, Priority, Labels, Module, Test Steps, Expected Results. |
| `excel/CSR_Impact_Monitor_UAT_Workbook.xlsx` | 4-sheet workbook: UAT Summary, Test Cases, Defect Log, Sign-off Tracker. |
| `docs/UAT_Plan.md` | Full narrative UAT plan with objectives, entry/exit criteria, roles, and all test scenarios. |

---

## 🚀 How to Use

### Import into Jira

1. Go to your Jira project → **Project Settings → Import Issues**
2. Select **CSV**
3. Upload `jira/jira_uat_import.csv`
4. Map fields:
   - `Summary` → Summary
   - `Issue Type` → Issue Type (map "Test" to your test issue type, e.g. via Zephyr or Xray)
   - `Priority` → Priority
   - `Labels` → Labels
   - `Component/s` → Component
   - `Description` → Description
   - `Custom Field (Module)` → custom field of your choice
   - `Custom Field (Test ID)` → custom field of your choice
5. Complete the import and assign test cases to your UAT sprint.

> **Tip:** If using **Xray**, import via Xray's own CSV importer for native test case linking.
> If using **Zephyr Scale**, use the Zephyr CSV template and remap columns accordingly.

### Use the Excel Workbook

Open `excel/CSR_Impact_Monitor_UAT_Workbook.xlsx`. It contains 4 sheets:

| Sheet | Purpose |
|---|---|
| **UAT Summary** | Overall metrics, module coverage, defect summary, Go/No-Go decision |
| **Test Cases** | All 48 test cases with steps, expected results, status, and defect ID columns |
| **Defect Log** | Log defects found during UAT with severity, steps to reproduce, and resolution tracking |
| **Sign-off Tracker** | Per-module sign-off with tester name, pass/fail counts, and approval date |

---

## 👥 Test Roles

| Role | CommCare User | Responsible For |
|---|---|---|
| Field Agent | `fanja.tester` | Modules 1–6 mobile forms, offline sync |
| Site Supervisor | `jeanpierre.tester` | Grievance follow-up, consultation log |
| CSR Manager | `marie.tester` | Dashboard, KPIs, GRI 14 export |
| ESG Director | `edouard.tester` | Read-only access validation |

---

## ✅ UAT Scope (MVP v1.0)

| Module | Forms | Test IDs |
|---|---|---|
| Module 1 — Registration | 1A Community, 1B Beneficiary | UAT-01, UAT-02 |
| Module 2 — Health | 2A Activity, 2B Follow-up | UAT-03, UAT-04 |
| Module 3 — Education | 3A Activity | UAT-05 |
| Module 5 — Environment | 5A Monitoring | UAT-06 |
| Module 6 — Grievances | 6A Register, 6B Resolve, 6C Consultation | UAT-07, UAT-08, UAT-09 |
| Offline & Sync | Cross-module | UAT-10 |
| RBAC | Cross-module | UAT-11 |
| Dashboard & Reporting | Web Dashboard | UAT-12, UAT-13 |

> ⚠️ **Module 4 (Livelihood)** is out of scope for MVP UAT. Scenarios will be added in Q2 2026 when livelihood activities begin.

---

## 🐛 Defect Severity

| Level | Definition |
|---|---|
| P1 Critical | Blocks core workflow; data loss risk |
| P2 Major | Incorrect behaviour; workaround exists |
| P3 Minor | Cosmetic or UX issue |
| P4 Enhancement | Out of MVP scope |

---

## 📤 Exit Criteria

- [ ] All P1 Critical test cases pass with zero open defects
- [ ] All P2 Major defects resolved or formally accepted
- [ ] Dashboard KPIs validated against known test data
- [ ] Sign-off obtained from CSR Manager and Site Supervisor
- [ ] GRI 14 export validated for column structure

---

## 🔗 References

- Dimagi Functional Specification v1.0 (February 2026)
- User Scenarios Document v1.0 (March 2026)
- CommCare HQ Documentation: https://dimagi.com/commcare/

---

*Maintained by: Implementation / QA Team*
*Last updated: March 2026*
