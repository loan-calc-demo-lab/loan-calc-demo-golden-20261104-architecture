## Scope and environment

- Pre-production test environment with the three services (Lending, Deposits, Digital) deployed.
- Only test customers are used, no real customer data.

## Test data

| Customer | Loan | Account | Status |
|----------|------|---------|--------|
| CUST-1001 | PL-1001 | CA-2001 | eligible (outstanding €8,940.27, balance €1,250.00) |
| CUST-1002 | PL-1002 | CA-2002 | in arrears |
| CUST-1003 | PL-1003 | CA-2003 | too new (3 payments) |

## Scenarios

### E2E-1 Happy path – top-up R1
**Given** the customer CUST-1001 requests a top-up of €5,000 for loan PL-1001.
**When** the request is processed through the Digital API, which calls Lending to create an offer and then Deposits to payout.
**Then** the offer is accepted, the payout of €4,950 is made, and the account balance becomes €6,200.
- Covers AC1, AC2, AC5 (offer details), AC7 (payout amount), AC9 (new balance).

### E2E-2 Not eligible – arrears
**Given** the customer CUST-1002 requests a top-up.
**When** the request reaches Lending.
**Then** Lending returns status 422 with message "Customer is in arrears" (TU-2a).
- Covers AC3.

### E2E-3 Offer expired
**Given** an offer was created for CUST-1001 and 24 h + 1 s have passed.
**When** the customer tries to accept the offer.
**Then** Lending returns status 410 with message "Offer expired" (TU-4c).
- Covers AC6.

### E2E-4 Accept twice – only one payout
**Given** a valid offer exists for CUST-1001.
**When** the customer accepts the offer twice (duplicate Idempotency-Key).
**Then** the first acceptance triggers a payout of €4,950, the second is ignored and no second payout occurs.
- Covers AC8.

### E2E-5 Deposits unavailable during accept
**Given** a valid offer exists for CUST-1001.
**When** the customer accepts the offer but the Deposits service is down.
**Then** Lending returns status 502 and no payout is recorded; a retry after Deposits is back up still results in a single payout.
- Covers AC8 (retry safety).

## Coverage

| AC | E2E scenario |
|----|--------------|
| AC1 | E2E-1 |
| AC2 | E2E-1 |
| AC3 | E2E-2 |
| AC4 | E2E-1 |
| AC5 | E2E-1 |
| AC6 | E2E-3 |
| AC7 | E2E-1 |
| AC8 | E2E-4, E2E-5 |
| AC9 | E2E-1 |

## Entry and exit criteria

- All services start without errors.
- All scenarios execute against the test environment and produce the expected HTTP status codes and payloads.
- No unexpected side effects (e.g., duplicate payouts) are observed.
