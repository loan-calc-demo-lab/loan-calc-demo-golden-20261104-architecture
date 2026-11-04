---
id: REQ-001
title: Monthly payment of an annuity loan
status: implemented
owner: Product Owner, Personal Loans
---
## Story
As a customer, I want to see my fixed monthly payment for an amount, rate and term, so that I know whether I can afford the loan.

## Acceptance criteria
- **AC1** Given 10,000 EUR at 5% for 12 months, when I ask for a quote, then the monthly payment is 856.07 EUR.
- **AC2** Given 25,000 EUR at 7.5% for 60 months, when I ask for a quote, then the monthly payment is 500.95 EUR.
- **AC3** Amounts are shown in EUR, rounded to cents.

## Traceability
- Code: `api/src/calc.rs` (`monthly_payment`), `ios/LoanCalculator/QuoteView.swift`, `ios/LoanCalculator/LoanAPI.swift`
- Tests: `req001_known_monthly_payment`, `req001_longer_loan`, `quote_returns_payment_and_schedule`; iOS: `test_req001_decodesQuote`, `test_req001_showsQuoteFromAPI`
