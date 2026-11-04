## Scope and environment

Pre-production test environment with the three services (Lending, Deposits, Digital) deployed. Only test customers are used; no real customer data is involved.

## Test data

- Eligible customer: CUST-1001, loan PL-1001 (outstanding €8,940.27), account CA-2001 (balance €1,250.00)
- Customer in arrears: CUST-1002, loan PL-1002, account CA-2002
- Too new (only 3 payments made): CUST-1003, loan PL-1003, account CA-2003

## Scenarios

### E2E-1: Happy path – top-up offer accepted (R1)
**Given** the customer CUST-1001 requests a top-up of €5,000 for loan PL-1001 (60 months)
**When** the Lending service returns the indicative offer (reference R1) and the Digital app accepts it
**Then** the Deposits service processes a payout of €4,950.00, the account CA-2001 balance becomes €6,200.00, and the app shows the offer and confirmation screens with the exact figures from the rulebook:
- top-up amount €5,000
- term 60 months
- new loan amount €13,940.27
- rate 9.9 %
- arrangement fee €50.00
- amount paid out €4,950.00
- monthly payment €295.50
- APR 10.5 %
- total cost of credit €3,840.05
- total payable €17,780.32

### E2E-2: Customer not eligible – arrears (AC1)
**Given** the customer CUST-1002 requests a top-up
**When** the Lending service evaluates eligibility and returns a 422 error with message "A top-up is not possible while a payment is overdue."
**Then** the Digital app displays that error message and no payout is attempted.

### E2E-3: Offer expired (AC11)
**Given** the customer CUST-1001 has been shown an indicative offer
**When** the offer is older than 24 hours (e.g. created with a past validity timestamp) and the customer tries to accept it
**Then** the acceptance request returns a 410 error with message "This offer has expired. Please ask for a new offer." and the app shows that error.

### E2E-4: Accept twice – idempotent result (AC13)
**Given** the customer CUST-1001 accepts a valid offer
**When** the same acceptance request is sent a second time
**Then** the system returns the same successful result as the first acceptance (200) and no additional payout occurs; the account balance remains €6,200.00.

### E2E-5: Deposits unavailable during accept (component-test-only)
**Given** the Deposits service is down when the Digital app attempts to accept an offer
**When** the acceptance request fails because Deposits is unavailable
**Then** the app reports a failure, but the Lending service records only one payout (the first successful attempt) and no double payout occurs on retry.

## Coverage

| AC | E2E scenario |
|----|--------------|
| AC1 | E2E-2 |
| AC4 | E2E-1 |
| AC5 | E2E-1 |
| AC11 | E2E-3 |
| AC12 | E2E-1 |
| AC13 | E2E-4 |
| AC5 (R1 figures) | E2E-1 |
| AC12 (account balance) | E2E-1 |

## Entry and exit criteria

- All services are reachable and return the contract responses defined in the spec.
- The Digital app renders offer and confirmation screens with the exact figures listed above.
- Error messages match the rulebook texts word for word.
- No duplicate payouts are recorded in the Lending payout client.
