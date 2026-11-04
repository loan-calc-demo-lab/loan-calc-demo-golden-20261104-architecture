# Architecture repository (baseline)

Enterprise Architecture's repository for the agentic SDLC demo: the **inputs** that impact analysis and design rely
on, and the **outputs** of the cross-domain stages. Demo runs never change this baseline; each run gets a fresh copy
of all four repositories (see `scripts/new-demo-run.sh`).

| Folder | What | Who changes it |
|---|---|---|
| `inventory/applications.yaml` | Application inventory: domain, team, owner, technology, repository, dependencies, last review | Enterprise Architecture with the owning teams |
| `interfaces/catalogue.yaml` + `interfaces/*.yaml` | Interface catalogue and OpenAPI contracts | Architect agent drafts, an architect approves |
| `standards/` | Architecture standards (contract first, idempotency, ownership …) | Enterprise Architecture |
| `specs/` | Requirements: epic, user stories per domain, acceptance criteria | Requirements Analyst agent drafts, the product owner approves (gate B) |
| `impact/`, `designs/` | Impact analysis and high-level design | Architect agent drafts, an architect approves (gate D) |
| `tests/e2e/` | End-to-end test scenarios before production | Tester agent drafts, the test manager approves |

The domain repositories of the same run are `…-lending`, `…-deposits` and `…-digital`. Agents may write there only
after the requirements and the design are merged here.

All names, customers and figures are fictional.
