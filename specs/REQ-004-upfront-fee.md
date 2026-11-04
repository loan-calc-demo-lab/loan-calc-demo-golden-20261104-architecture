---
id: REQ-004
title: Upfront arrangement fee
status: implemented
owner: Product Owner, Personal Loans
---
## Story
As a customer, I want the one-off arrangement fee included in what I will pay in total, so that there are no surprises.

## Acceptance criteria
- **AC1** The fee is optional and defaults to 0.
- **AC2** The fee does not change the monthly payment.
- **AC3** Total payable = all monthly payments + the fee.

## Traceability
- Code: `api/src/calc.rs` (`quote`), `ios/LoanCalculator/QuoteViewModel.swift`
- Tests: `req004_upfront_fee_is_in_total_payable`, `upfront_fee_is_optional`; iOS: `test_req004_emptyFeeIsSentAsZero`
