# Permissions and boundaries plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant builds it following `runtime/permission-rules.md`. Where this plan and our task specifications disagree, this plan wins.

- Agent: For individuals and organizations managing recurring expenses, reduce average monthly overspending across food, clothing, events, and similar categories by 25%, measured by the difference between planned and actual spending, moving from a baseline established during the user’s first month to the target within 90 days, without making purchases or financial transfers, overriding user priorities, or presenting estimates as guaranteed outcomes. The agent will read receipts, have access to refund requests, and have access to fundraiser information. (`our_team_agent`)
- Owner, who answers for this agent: Keanu Widjaja
- Written on: November 8, 2026

## 1. May (on its own)

| # | Action |
|---|---|
| 1 | Read the organization’s budget, event details, and spending history. |
| 2 | Calculate projected costs and remaining funds. |
| 3 | Compare different budget scenarios. |
| 4 | Identify categories that may exceed their limits. |
| 5 | - Record forecast results and post-event spending data. |

## 2. May, with a limit

| # | What | The limit | Kind | Who can change it |
|---|---|---|---|---|
| 1 | Create spending recommendations | Recommendations must stay within available funds and preserve at least a 10% emergency reserve | A number a reviewer can change in the app | A signed-in reviewer, in the app, with a reason |

## 3. Ask first (hold for a person)

| # | When | What the agent does instead |
|---|---|---|
| 1 | The available funds, event costs, or spending limits are missing or contradictory | The recommendation would exceed available funds. |
| 2 | The agent would need to delete or overwrite an existing financial record. | It waits for a person |

## 4. Never

| # | Never | Where it is enforced |
|---|---|---|
| 1 | Never make purchases, transfers, or financial approvals. | Code |

## 5. Who signs off

| Decision | Who | How |
|---|---|---|
| Any recommendation that exceeds available funds, changes an approved budget, or involves uncertain or conflicting financial data. | Treasurer (Keanu Widjaja) | Keanu approves or rejects the recommendation in the app, with the decision, reason, timestamp, and related event recorded. |

## The off switch

Keanu Widjaja can pause BudgetPilot in the app and must provide a reason. While paused, the agent must not make recommendations, write records, or learn from the new outcomes. Resuming the agent also requires Keanu's approval.

## How mistakes are reported

Provide a "report a problem" option besides each recommendation. Reports go to Keanu Widjaja for review, with the original recommendation, input data, decision, and timestamp preserved. Corrections and new lessons are added only after review.

## Rules we keep

- Every permission starts off. The agent may do only what this plan lists under *May* and *May, with a limit*.
- Writing a permission down does not enforce it. Every limit and every *Never* is enforced in code or in a database rule, never in instructions alone.
- A limit a person may change is a setting with an owner and a reason, which the code reads on every run.
- Asking means holding, not guessing: the record waits with a plain reason that names what is missing.
- A sign-off is a click in the app, recorded with who and when. A chat message is not a sign-off.
- Test mode never acts: with `dry_run`, the agent decides but saves nothing.
