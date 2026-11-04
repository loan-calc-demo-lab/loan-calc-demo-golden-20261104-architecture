---
id: REQ-008
title: Top-up of an existing personal loan in the mobile app
status: proposed
owner: Product Owner, Personal Loans
---
## Epic
As a personal-loan customer, I want to request a top-up from the mobile app, see an indicative offer and receive the net amount in my current account, so that I can obtain additional funds conveniently.

## User stories
**US1** As a customer, I want to see whether I am eligible for a top-up, so that I only receive offers when I meet the criteria.
**US2** As a customer, I want to view an indicative offer showing the new loan amount, rate, arrangement fee, monthly payment, APR, total cost of credit and total payable, so that I can decide to accept.
**US3** As a customer, I want the offer to be valid for a limited time, so that I act promptly.
**US4** As a customer, I want to accept the offer and have the net top-up amount paid into my current account, and see confirmation of the new monthly payment and first payment date, so that I know the outcome.
**US5** As a customer, I want to be prevented from accepting the same offer twice, so that I am not charged twice.

## Acceptance criteria
**AC1 (US1)** Given a loan that is in arrears, when the customer requests a top-up, then the system shows *Customer message TU-1a* "A top-up is not possible while a payment is overdue.".
**AC2 (US1)** Given a loan with fewer than 6 payments made, when the customer requests a top-up, then the system shows *Customer message TU-1b* "A top-up is possible after your sixth monthly payment.".
**AC3 (US1)** Given a loan without a repayment current account, when the customer requests a top-up, then the system shows *Customer message TU-1c* "We could not find your current account. Please contact us.".
**AC4 (US1)** Given a loan that meets eligibility, when the customer requests a top-up, then the system proceeds to calculate the indicative offer.

**AC5 (US2)** Given a top-up request of €5,000 for 60 months (reference R1), when eligibility is met, then the indicative offer displays:
- top-up amount €5,000
- term 60 months
- new loan amount €13,940.27
- rate 9.9 %
- arrangement fee €50.00
- amount paid out €4,950.00
- monthly payment €295.50
- APR 10.5 %
- total cost of credit €3,840.05
- total payable €17,780.32
as per **Top-up reference value R1**.
**AC6 (US2)** Given a top-up request of €7,000 for 72 months (reference R2), when eligibility is met, then the indicative offer displays the figures from **Top-up reference value R2**.
**AC7 (US2)** Given a top-up request of €1,000 for 48 months (reference R3), when eligibility is met, then the indicative offer displays the figures from **Top-up reference value R3**.
**AC8 (US2)** Given a top-up amount outside €1,000-€20,000, when the customer enters the amount, then the system shows *Customer message TU-2a* "Please choose a top-up amount between €1,000 and €20,000.".
**AC9 (US2)** Given a new loan amount that would exceed €30,000, when the calculation is performed, then the system shows *Customer message TU-2b* "The new loan amount can be at most €30,000.".
**AC10 (US2)** Given a term outside 12-84 months, when the customer selects the term, then the system shows *Customer message TU-2c* "Please choose a term between 12 and 84 months.".

**AC11 (US3)** Given an indicative offer has been shown, when 24 hours pass without acceptance, then the offer expires and the system shows *Customer message TU-4c* "This offer has expired. Please ask for a new offer.".

**AC12 (US4)** Given the customer accepts a valid offer, when the acceptance is processed, then the system:
- deducts the arrangement fee from the payout
- credits the net amount to the repayment account (e.g., account balance after payout R1: €6,200.00)
- creates a new loan with the combined amount and new monthly payment
- shows a confirmation containing the amount paid in, the account, the new monthly payment, the first payment date and a 14-day withdrawal reminder (TU-5f).

**AC13 (US5)** Given the customer attempts to accept the same offer a second time, when the second acceptance is received, then the system returns the result of the first acceptance and no additional payout occurs (TU-5e).

## Decisions
- Q1 (eligibility rules) → AC1-AC4 (as confirmed).
- Q2 (offer validity) → AC11 (24 h validity confirmed).
- Q3 (credit decision simulation) → out of scope for this release (REG-2).
- Q4 (pre-contract document) → out of scope (REG-1 full document).
- Q5 (withdrawal processing) → out of scope (REG-3 processing).
- Q6 (SCA) → out of scope (REG-4).
- Q7 (store offer) → in scope (REG-5) – captured in AC12.
- Q8 (AML monitoring) → out of scope (REG-6 monitoring).

## Out of scope (this release)
- Full pre-contract SECCI document (REG-1).
- Credit-worthiness assessment decision (REG-2) – simulated.
- Withdrawal processing after the 14-day period (REG-3).
- Strong Customer Authentication (REG-4).
- AML monitoring beyond account check (REG-6).

## Source
Issue #1, product-owner answers; Rulebook references: TU-1a-TU-5f, REG-1-REG-6, Top-up reference values R1-R3, Account balance after the payout (R1).