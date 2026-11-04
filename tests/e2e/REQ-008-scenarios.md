## Scope and environment

Pre-production test environment with the three services (Lending, Deposits, Digital) deployed. Only test customers are used; no real customer data is involved.

## Test data

- Eligible customer: CUST-1001, loan PL-1001 (outstanding €8,940.27), account CA-2001 (balance €1,250.00)
- Customer in arrears: CUST-1002, loan PL-1002, account CA-2002
- Too new (only 3 payments made): CUST-1003, loan PL-1003, account CA-2003

## Scenarios

### E2E-1: Happy path – top-up offer accepted (R1)
**Given** the customer CUST-1001 requests a top-up of €5,000 for loan PL-1001
**When** the Lending service returns an offer (R1) and the Digital app accepts it
**Then** the Deposits service processes a payout of €4,950.00, the account CA-2001 balance becomes €6,200.00, and the app shows the offer and confirmation screens with the exact figures from the rulebook (monthly payment €295.50, APR 10.5%, total cost of credit €3,500.00, etc.)

### E2E-2: Customer not eligible – arrears
**Given** the customer CUST-1002 requests a top-up
**When** the Lending service evaluates eligibility and returns a 422 error with message "TU-2a: Customer in arrears"
**Then** the Digital app displays the error message and no payout is attempted.

### E2E-3: Offer expired
**Given** the customer CUST-1001 receives an offer
**When** more than 24 h passes before acceptance (state.advance_clock(24*3600+1) in Lending)
**Then** the acceptance request returns a 410 error with message "TU-4c: Offer expired" and the app shows the appropriate error.

### E2E-4: Accept twice – only one payout
**Given** the customer CUST-1001 accepts the same offer twice
**When** the first acceptance triggers a payout and the second acceptance is processed
**Then** the second acceptance returns a 409 conflict (or similar) and no second payout occurs; the account balance remains €6,200.00.

### E2E-5: Deposits unavailable during accept
**Given** the Deposits service is down when the Digital app attempts to accept an offer
**When** the acceptance request fails due to Deposits unavailability
**Then** the app retries or fails gracefully, but only one payout is ever made (no double payout on retry).

## Coverage

| AC | E2E scenario |
|----|--------------|
| AC1 (eligible offer) | E2E-1 |
| AC2 (arrears) | E2E-2 |
| AC3 (offer expiry) | E2E-3 |
| AC4 (double accept) | E2E-4 |
| AC5 (deposits down) | E2E-5 |

## Entry and exit criteria

- All services are reachable and return the expected contract responses.
- The Digital app correctly renders offer and confirmation screens with the exact figures.
- Error messages match the rulebook texts word for word.
- No duplicate payouts are recorded in the Lending payout client.
