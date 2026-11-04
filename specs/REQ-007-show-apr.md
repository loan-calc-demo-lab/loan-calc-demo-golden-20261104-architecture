---
id: REQ-007
title: Show APR and total cost of credit
status: proposed
owner: Product Owner, Personal Loans
---

## Story
As a customer, I want to see the **APR** and the **Total cost of credit** displayed next to the monthly payment on the quote screen, so that I can understand the full cost of the loan before applying.

## Acceptance criteria
**AC1**
Given a loan of **10,000.00** EUR, nominal rate **5%**, term **12** months, arrangement fee **250.00** EUR
When the quote is calculated
Then the API response includes `apr_pct` = **10.2%** and `total_cost_of_credit` = **522.89** EUR (rounded half‑up to one decimal for APR and two decimals for cost)【source: lending-rulebook-apr.html】

**AC2**
Given a loan of **10,000.00** EUR, nominal rate **5%**, term **12** months, arrangement fee **0.00** EUR
When the quote is calculated
Then the API response includes `apr_pct` = **5.1%** and `total_cost_of_credit` = **272.89** EUR【source: lending-rulebook-apr.html】

**AC3**
Given a loan of **1,200.00** EUR, nominal rate **0%**, term **12** months, arrangement fee **60.00** EUR
When the quote is calculated
Then the API response includes `apr_pct` = **10.0%** and `total_cost_of_credit` = **60.00** EUR【source: lending-rulebook-apr.html】

**AC4**
Given the quote screen displays the monthly payment
When the UI is rendered
Then the APR and Total cost of credit appear in the same section as the monthly payment, directly below it, with the same visual weight, and only on the quote screen (not on the repayment schedule screen)【source: UI‑placement‑rule】

**AC5**
Given the APR and Total cost of credit are displayed
When the UI is rendered
Then the labels are exactly "APR" and "Total cost of credit", with the explanation "Annual percentage rate, including fees" shown under the APR【source: UI‑labels‑rule】

**AC6**
Given the APR value is displayed
When the UI formats the APR
Then it shows one decimal place, rounded half‑up【source: UI‑format‑rule】

## Traceability
- Code: `api/src/calc.rs`, `ios/LoanCalculator/LoanAPI.swift`, `ios/LoanCalculator/QuoteSummary.swift`
- Tests: Rust `req007_show_apr`, Swift `REQ007ShowAPRTests`

## Source
Issue #1 – Show the APR and total cost of credit in the loan calculator.
Rulebook sections: APR‑R4, APR‑R5, APR‑R6, Reference values table.
