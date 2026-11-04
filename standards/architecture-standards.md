# Architecture standards (demo)

Owner: Enterprise Architecture · Version 1.2 · Last reviewed 2026-10-01

These rules apply to every change that crosses a domain boundary. Each rule has an ID; designs and reviews cite it.

## Contracts

- **ARC-1 Contract first.** A new or changed interface between applications is designed as an OpenAPI 3.1 contract
  in this repository (`interfaces/<provider-domain>-<name>-v<major>.yaml`) and listed in `interfaces/catalogue.yaml`
  before any domain implements it. Provider and consumers implement exactly that contract.
- **ARC-2 Versioning.** Adding optional fields or new operations keeps the major version. Removing or renaming a field,
  or changing its meaning, needs a new major version; the old one stays until every consumer has moved.
- **ARC-3 JSON conventions.** snake_case field names; amounts in EUR as JSON numbers with two decimals; percentages as
  numbers in percent; dates and times in ISO 8601 UTC. Errors: `{"error": <code>, "message": <text>, "field": <name, optional>}`
  with 404 (unknown resource), 409 (conflict, e.g. an offer already used), 410 (expired), 422 (validation or rule).

## Money movement

- **ARC-4 Idempotency.** Every request that moves money carries an `Idempotency-Key` header chosen by the caller. The
  provider stores the key with the result; a repeated request with the same key returns the first result and never
  moves money twice. A repeated key with a different body is rejected with 409.
- **ARC-5 Server-side truth.** The provider recalculates every amount from its own data. Amounts or rates sent by a
  client are never trusted (a client sends only identifiers and the customer's choices).
- **ARC-6 One owner per record.** Only Lending changes a loan; only Deposits changes an account balance. A domain asks
  the owner through its interface; it never writes another domain's data.
- **ARC-7 Order of steps for a payout from a loan.** Lending decides and records the change, then asks Deposits to pay
  out with an idempotency key derived from its own record; if the payout fails, Lending keeps the change pending and
  retries with the same key. (The demo calls Deposits synchronously; production would use the integration layer.)

## Security and operations

- **ARC-8 Identity.** The customer is identified from the authenticated session, never from a field the client can
  choose. (The demo has no sign-in and uses a fixed demo customer; this is a known simplification.)
- **ARC-9 No personal data in logs.** Log identifiers (loan, offer, payout ids), never names, amounts with account
  numbers, or tokens.
- **ARC-10 Timeouts.** A synchronous call between services has a timeout (5 s) and a clear error to its caller.

## Design documents

- **ARC-11 Where designs live.** Impact analysis: `impact/<REQ>.md`. High-level design: `designs/<REQ>-hld.md` with a
  sequence diagram (Mermaid). Low-level design: in each domain repository, `design/LLD-<REQ>.md`.
- **ARC-12 Unknowns are written down.** Anything the inventory, the catalogue or the code cannot confirm is listed as an
  open question with the owner who must answer it. Nobody fills a gap with an assumption without saying so.
