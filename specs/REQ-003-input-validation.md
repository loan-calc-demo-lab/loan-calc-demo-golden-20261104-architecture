---
id: REQ-003
title: Input validation against product limits
status: implemented
owner: Product Owner, Personal Loans
---
## Story
As the bank, I want quotes only for loans we offer, so that customers don't see figures for loans they cannot get.

## Acceptance criteria
- **AC1** Principal: more than 0 and at most 1,000,000 EUR.
- **AC2** Nominal annual rate: 0% to 30%.
- **AC3** Term: 1 to 360 months.
- **AC4** Upfront fee: 0 or more, and less than the principal.
- **AC5** An invalid input is rejected with the name of the field and a readable message; no figures are shown.

## Traceability
- Code: `api/src/calc.rs` (`validate`), `api/src/lib.rs`, `ios/LoanCalculator/QuoteViewModel.swift`, `ios/LoanCalculator/QuoteView.swift`
- Tests: `req003_rejects_out_of_range_inputs`, `invalid_input_is_rejected_with_the_field_name`; iOS: `test_req003_mapsValidationError`, `test_req003_nonNumericAmountIsRejectedWithoutCallingAPI`, `test_req003_apiValidationErrorIsShownOnTheField`
