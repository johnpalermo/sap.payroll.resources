# Open questions — CS → CC pension and RESSOP recast, employee 5600486

**Purpose.** These are the points that must be settled before the should-be contributions can be finalized. Each question notes why it matters and what the extracted data already shows. Sections: A (changes the numbers), B (changes scope or mechanics), C (assumptions to confirm), D (documentation discrepancies for the spec owners), E (decisions that follow).

**Working figures, base case (specification formulas, service from CSD 2023‑09‑11):**

| Recalculation point | As-is base (CS) | Recast base (CC) | Basis |
|---|---|---|---|
| Rehire 2023‑09‑11 and pension eligibility 2024‑01‑01 | 35,000.00 | 35,000.00 | Base salary; partial-year 2023 9B06 (11,654.91) cannot exceed it |
| 2025‑01‑01 (service under 2 years) | 40,000.00 | 77,825.07 | Greatest of 40,000; 9B06 = 77,825.07; (77,825.07 + 35,000) ÷ 2; (77,825.07 + 11,654.91) ÷ 2 |
| 2026‑01‑01 (service over 2 years) | 40,000.00 | 83,426.33 | (78,049.13 + 88,803.53) ÷ 2 |

Biweekly base (× 14/365): as-is 1,534.25; recast 2,985.07 (2025) and 3,199.91 (2026). Cumulative deltas to period 202617: DC employer 3% +1,972.30; RESSOP employer 3% +1,981.01; RESSOP employee 6% +3,962.02. If the 2025 recalculation uses the annualized 2024 figure (88,803.53), the 2025 DC employer delta becomes 1,448.66 instead of 1,122.81 and the RESSOP figures scale the same way. 2024 is unchanged unless Q2 changes it.

---

## A. Questions that change the numbers

**Q1. Wage type meanings and the missing REPS stream.**
Please confirm the texts and roles of 2922, 2981, 2147, 214B, 9B21 and 9B24, and state where the REPS 3% election (IT0169 subtype RI40) and any employer share-plan contribution are carried in the RT.
*Why it matters:* it determines which contribution streams enter the reconciliation.
*What the data shows:* 2922 = 3% × 9B09 from 202401 (pension eligibility 2024‑01‑01). 2147 = −6% × biweekly base (80.55) and 2981 = 3% × biweekly base (40.27) both start in 202406, the first period after savings eligibility (2024‑03‑11); 2147 becomes 214B in period 202419, the period containing the IT0169 change from RREG to RTFS (2024‑09‑20). 2981 is unprorated (46.03 in 202501, where 2922 was prorated to 44.89). Reading: 2922 = DC employer automatic, 2147/214B = RESSOP employee 6%, 2981 = RESSOP employer match. REPS does not appear in the extract.

**Q2. Salary-range minimum.**
Value and source of the minimum of the salary range for pay scale type 65 / area P1 / group PL09 as at 2023‑09‑11, 2024‑01‑01 and 2025‑01‑01.
*Why it matters:* the commissioned-employee formula includes the range minimum whenever service or position experience is under two years. Both are under two years at the 2024 and 2025 recalculations (CSD 2023‑09‑11; position 1100845 held since 2023‑09‑11) and over two years at 2026‑01‑01, so the floor is irrelevant in 2026. A minimum above 35,000 changes 2024; a minimum above 77,825.07 changes 2025.

**Q3. What the January 1 program reads.**
(a) Does the annual recalculation read the year just ended before or after the LWOP annualization is applied? For 2025‑01‑01 this is 77,825.07 versus 88,803.53. (b) Which IT0008 record does it read — the record valid on January 1 or the one valid on the run date? The 40,000 salary effective 2025‑01‑01 was created on 2024‑12‑13.
*Why it matters:* (a) moves the 2025 base by 10,978.46; (b) matters only if the run precedes the salary record's validity.

**Q4. Service and position-experience basis.**
Confirm that the "less than 2 years" tests use the continued service date on IT9041 (2023‑09‑11) and the position start date (2023‑09‑11), not the original 2016 hire.
*What the data shows:* the 2016 employment ended 2016‑05‑12; the 2023 rehire (action 71, no reason) was outside 90 days, the CSD was reset, and the accumulators were emptied (CRT 202326 has no 9B12/9B16). Under the spec's worked example service is measured from hire, which supports the CSD reading.

**Q5. The 2024 leave and the 9B17 annualization.**
Leave type, start and end dates, and the number of LWOP days used by the annualization for 2024; and the exact formula in the rule.
*What the data shows:* return action 88 reason 06 on 2024‑07‑01; 9B17 = 10,978.46 posted on IT0015 dated 2025‑01‑01, giving 9B16 (2025) = 77,825.07 + 10,978.46 = 88,803.53. The spec formula (9B06 ÷ (365 − LWOP days) × LWOP days) implies roughly 45 days but does not reproduce exactly: 45 days gives 10,944.15. The spec's own example does not reproduce either (see D1). The recast assumes the same 9B17 would arise under CC because the accumulators are subgroup-independent; please confirm the annualization program does not branch on subgroup.

**Q6. Actions 77 and 88/06.**
Meaning of action 77 (2024‑08‑15) and action 88 with reason 06 (2024‑07‑01), and whether either would place a commissioned employee on the Z981 recalculation table.
*Why it matters:* any additional recalculation point between 2023‑09‑11 and today adds a step to the base sequence. Org unit changed on 2023‑10‑23, 2024‑07‑07 and 2024‑07‑08; the position did not.

**Q7. IT9171 indicator values.**
All three IT9171 records carry 1 in the benefits, pension and savings eligibility-date indicators and 1 in both base indicators. The specification's legend gives 0 = system-calculated and 2 = manual override for the date indicators, and 1 = continuous, 3 = commissioned two-year average, 4 = manual for the base indicator. Please confirm what 1 means for the date indicators, that it does not stop the batch from recalculating, and that a corrected CC record should carry base indicator 3.

---

## B. Questions that change scope or mechanics

**Q8. RESSOP employee side.**
Confirm that the employee's 6% (and 3% REPS) contributions are calculated on the IT9171 benefits base rather than on actual earnings; the RT indicates this (80.55 = 6% × 1,342.47; 92.06 = 6% × 1,534.25). If so, the recast raises the employee's own deductions by about 3,962 to period 202617 (6% only; REPS would add about 1,981). The business needs a position: recover from the employee, waive, or leave the employee side as-is — and in the last case, whether the employer match is recomputed on the should-be base or on the contributions actually made. The first specification (§10.6) states RESSOP changes cannot be processed retroactively, so the mechanics of any catch-up also need confirming.

**Q9. Tax-slip and pension-adjustment scope.**
The employee is in Quebec. The RESSOP employer match is a taxable benefit, and the DC employer contributions feed the pension adjustment. 2025 is a closed T4/RL‑1 year. Should the reconciliation carry the T4/RL‑1 box and PA impacts for 2025 (amended) and 2026 (in-year)? This is a scoping question; the reporting treatment itself is for Tax to confirm.

**Q10. Cut-off and go-forward.**
Reconcile through period 202617 (the latest in the extract) and refresh at delivery? For go-forward, the corrected 2026 base (83,426.33) needs to be on IT9171 before the 2027‑01‑01 recalculation, which will use 2026 earnings (9B06 already 117,430.57 at pay 17).

---

## C. Assumptions to confirm (yes/no)

- **C1.** Pay itself was correct — salary 35,000 / 40,000 and the 1729 and 17B1 payments — and only the subgroup and the base treatment are being recast.
- **C2.** The same elections apply in the should-be scenario: DC employee 0% (plan RIDC, level DC04, no match), RESSOP 6% (RREG, then RTFS) plus 3% REPS.
- **C3.** Pension eligibility (2024‑01‑01) and savings eligibility (2024‑03‑11) are unchanged under CC.
- **C4.** The stored accumulators are the values a CC would have had: 9B06 11,654.91 (2023), 77,825.07 (2024), 78,049.13 (2025), 117,430.57 (2026 YTD); 9B16 88,803.53 for 2025. Check: 2023 = 8 × 1,342.47 + 915.15 exactly. Wage types 1729 and 17B1 are on the 9B11 list.
- **C5.** No STI or bonus wage types were paid (9B42/9B52 absent), so Pension Earnings = Pension Base and the 9B52 addition does not apply.
- **C6.** The 150,000 earnings cap and the ITA maximum are not reached (biweekly cap 5,753.42 against a should-be maximum of 3,199.91); CapMyEarnings is irrelevant.
- **C7.** The base sequence 35,000 → 77,825.07 → 83,426.33 is accepted as the specification-based should-be, subject to Q2 and Q3.

---

## D. Documentation discrepancies noticed (for the specification owners)

- **D1.** Annualization example: 15,000 ÷ (365 − 65) × 65 = 3,250.00; the document states 3,358.33.
- **D2.** Accumulator table: 9B15 and 9B17 are swapped between the 9B14 and 9B16 rows, and the 9B16 row is labelled "9B12 YTD Base Prev Year". The narrative (9B17 adjusts 9B16) is the version used.
- **D3.** Rehire reset window: 42 days in rule 9B00 and the hours section, 90 days in the narrative. Immaterial here (2016 to 2023).
- **D4.** The consolidated specification applies the two-year average only at two or more years of service; the base-calculation specification defines an under-two-years variant (greatest of 9B06, (9B06 + current base) ÷ 2, (9B06 + 9B16) ÷ 2). The latter is used, consistent with its worked example.
- **D5.** Consolidated specification §3.1 (DC03 = 100% match) versus §4.3 (graduated 50/50/50/50/100). Not relevant to this employee (employee rate 0%).
- **D6.** Wage type 1736 (Commission-pensionable) cumulates to both 9B11 (base) and 9B42 (prior-year STI): a double-count risk for commissioned employees who receive it. Not this employee.
- **D7.** Leave handling was changed by SCRs after the specification was written ("need to add logic for other leaves", SCR 8 rotation suppression); production behaviour on leave may differ from the text.

---

## E. Decisions that follow once A and B are answered

- **E1. Correction mechanism.** Either (a) change IT0001 to CC from 2023‑09‑11, load the should-be IT9171 history and let payroll retro re-derive the DC differences (the accumulators rebuild themselves from CRT; IT9171 does not; processing class 83 takes the differences), or (b) compute the differences outside payroll and post them in the current period. The RESSOP portion follows (b) in either case.
- **E2. Employee-side RESSOP recovery** (Q8).
- **E3. Amended 2025 slips, pension adjustment, and plan-administrator file corrections** (Q9).
- **E4. Sandbox validation** on a production copy before anything is posted: set CC from 2023‑09‑11, load the should-be IT9171 records, run the annual base program and a forced retro simulation, and compare the RT to the reconciliation line by line.
