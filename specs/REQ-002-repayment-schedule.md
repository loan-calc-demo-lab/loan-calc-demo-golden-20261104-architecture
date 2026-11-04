---
id: REQ-002
title: Repayment schedule
status: implemented
owner: Product Owner, Personal Loans
---
## Story
As a customer, I want to see how each payment splits into principal and interest, so that I understand how my debt goes down.

## Acceptance criteria
- **AC1** The schedule has one row per month with payment, principal, interest and remaining balance.
- **AC2** In every row, principal + interest = payment.
- **AC3** The last payment clears any rounding remainder, so the final balance is exactly 0.

## Traceability
- Code: `api/src/calc.rs` (`quote`), `ios/LoanCalculator/ScheduleView.swift`
- Tests: `req002_schedule_clears_the_balance`, `req002_each_row_adds_up`
