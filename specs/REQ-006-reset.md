---
id: REQ-006
title: Reset the calculator
status: implemented
owner: Product Owner, Personal Loans
---
## Story
As a customer, I want to start a new calculation in one tap, so that an old quote is never mistaken for the new one.

## Acceptance criteria
- **AC1** Given a quote is shown, when I tap Reset, then the quote disappears and the inputs return to the starting values (10,000 EUR, 5%, 12 months, no fee).
- **AC2** Given an error message is shown, when I tap Reset, then the message disappears.
- **AC3** Given the calculator is already in its starting state, then Reset is disabled.
- **AC4** While a calculation is running, Reset is disabled.

## Traceability
- Code: `ios/LoanCalculator/QuoteViewModel.swift` (`reset`, `canReset`), `ios/LoanCalculator/QuoteView.swift`
- Tests: iOS: `test_req006_resetRestoresStartingValuesAndClearsQuote`, `test_req006_resetClearsErrorMessages`, `test_req006_nothingToResetAtStart`
