---
id: REQ-005
title: Zero-rate loans
status: implemented
owner: Product Owner, Personal Loans
---
## Story
As the bank, I want to quote promotional 0% loans correctly.

## Acceptance criteria
- **AC1** Given 1,200 EUR at 0% for 12 months, then the monthly payment is 100.00 EUR and the total interest is 0.

## Traceability
- Code: `api/src/calc.rs` (`monthly_payment`)
- Tests: `req005_zero_rate_loan`
