# Employee Details to Provide

Only the changes. Here is the consolidated list, everything in one place, superseding both earlier versions.

A. Employment history and status

1. Previous termination date and reason; the 2023‑09‑19 rehire action type and reason (MASSN 71 / MASSG), and whether it was treated as within the rehire window or as a new hire.
2. CSD on IT9041 as set at rehire, and position start date from IT0001.
3. IT9171 full history from the prior employment to today, all fields: pension and benefits eligibility dates and their override indicators, pension base, benefits base, benbseind, ltdind, GRTW indicator, penjoindt, savings eligibility date.
4. IT0377 history: plan, level (DC03/DC04), contribution rate, match date, auto-escalation date, CapMyEarnings.
5. Payroll area/frequency (BW/KS/KM), business area, and Global Grade in 2023 (only matters if contributions started before 2024‑01‑01).
6. Every leave since rehire: type (maternity/parental, STD, LWOP, LTD, salary continuance), start and end dates, LWOP days per calendar year, and WPBP employment status 1/2 during the leave.

B. Base inputs

7. IT0008 by wage type (0001, 0005, 0006) and annual amount at rehire and at every change since.
8. Salary-range minimum for the position/grade at rehire and at each January 1 (2024, 2025, 2026), with the table or source it comes from.
9. If anything carried over from the prior employment: last pension base, benefits base and base salary before termination.

C. Payroll accumulators, as stored (CRT)

10. 9B06, 9B16, 9B07, 9B17, 9B10, 9B11, 9B42, 9B52 at the last pay of 2023, 2024 and 2025, and current.
11. 9B01 per period, only if any period had less than full-time hours.
12. The commission, variable-pay and top-up wage types actually paid to the employee, with amounts per period, so I can match them against the 9B10/9B11/9B42 lists.

D. As-is results per period since eligibility (Wage Type Reporter, including off-cycles)

13. 9B08, 9B24, 9B09.
14. EE pension contribution wage type (2101/2105 etc.), ER automatic and ER match wage types, and any 2400-series waived amounts.
15. RESSOP EE and ER wage types.

E. Savings plan

16. IT0169 history since rehire.

Not from the employee, but still needed

17. The RESSOP contribution rule: election options, ER match and cap, and which base it uses.
18. How 9B52 enters the per-pay contribution base (the rule or one payroll log for a commissioned employee).
19. Answers to the open questions: service measured from CSD or rehire date; what the annual calculation did at the January 1 during or after the maternity leave; confirmation that pay itself was correct and the same election is assumed in the should-be scenario.

Items 1–15 are the minimum for the DC recalculation; 16–17 add RESSOP; 18–19 remove assumptions from the result.


