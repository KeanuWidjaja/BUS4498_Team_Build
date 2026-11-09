# Permissions and boundaries plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant builds it following `runtime/permission-rules.md`. Where this plan and our task specifications disagree, this plan wins.

- Agent: Reads a club’s budget request for an event or month, compares planned spending with past spending by category, and drafts two or more budget options for the treasurer to review. It never makes purchases or moves money. (`our_team_agent`)
- Owner, who answers for this agent: Keanu Widjaja
- Written on: November 8, 2026

## 1. May (on its own)

| # | Action |
|---|---|
| 1 | Read a signed-in officer's budget request and the club's past spending records |
| 2 | Organize the inputs into one record, with a source for every number |
| 3 | Check the inputs for missing or conflicting information |
| 4 | Compare planned and past spending by category, with every estimate labelled |
| 5 | Read the relevant lessons before it drafts |
| 6 | Draft two or three budget options with assumptions and trade-offs, for the treasurer to review |

## 2. May, with a limit

| # | What | The limit | Kind | Who can change it |
|---|---|---|---|---|
| 1 | Send clarification questions on its own | On. Off: questions wait for the treasurer to send | A switch a reviewer can flip in the app | A signed-in reviewer, in the app, with a reason |
| 2 | Clarification rounds per request | 2; after that, the request goes to the budget lead as unresolved | A number a reviewer can change in the app | A signed-in reviewer, in the app, with a reason |
| 3 | Requests per day | 30 in any 24 hours; then it refuses until tomorrow | A number a reviewer can change in the app | A signed-in reviewer, in the app, with a reason |
| 4 | Plan a budget on its own | Plan a budget on its own | A number a reviewer can change in the app | A signed-in reviewer, in the app, with a reason |
| 5 | Option totals | Each option’s total is at or below the available funds supplied | Fixed: only the builders change it | Only the builders, by changing the code |
| 6 | Options per draft | 2 to 3 | Fixed: only the builders change it | Only the builders, by changing the code |
| 7 | AI models | Claude Haiku 5.5; in test mode, Claude Sonnet 5.5 too, to compare | Fixed: only the builders change it | Only the builders, by changing the code |

## 3. Ask first (hold for a person)

| # | When | What the agent does instead |
|---|---|---|
| 1 | Available funds are missing | Holds it for the treasurer; the reason says available funds are missing, and it asks the officer for them |
| 2 | Planned and past spending disagree for the same category | Holds it for the treasurer; the reason names the category and both numbers |
| 3 | Planned spending is more than the available funds | Holds it for the treasurer; the reason shows both totals |
| 4 | Available funds are over the $5,000 limit | Holds it for the treasurer; the reason names the $5,000 limit |
| 5 | The request asks it to buy something, pay someone, or move money | Refuses that part and holds the request for the treasurer, flagged |
| 6 | The request contains instructions to the AI (for example, “ignore the limits”) | Holds it for the treasurer, flagged; nothing changes |

## 4. Never

| # | Never | Where it is enforced |
|---|---|---|
| 1 | Make a purchase, payment, or money transfer | Code and a database rule |
| 2 | Release a budget result the treasurer hasn’t approved | A database rule |
| 3 | Draft an option whose total is more than the available funds | Code and a database rule |
| 4 | Fill in an amount the officer didn’t give | Code |
| 5 | Present an estimate as a guaranteed outcome | Code |
| 6 | Send the same clarification questions twice | A database rule |
| 7 | Change its own limits or switches, or approve its own lessons | A database rule |
| 8 | Save or show bank account numbers, card numbers, or student IDs | Code |

## 5. Who signs off

| Decision | Who | How |
|---|---|---|
| Every budget draft (approve, require corrections, or leave unresolved) | The treasurer | Approve or Reject in the app, with a reason; recorded with who and when |
| A request held under Ask first | The treasurer | Release or reject in the app; recorded with who and when |
| A request still unresolved after 2 clarification rounds | The organization budget lead | Resolve or close in the app; recorded with who and when |
| Switching a lesson off | The treasurer | Toggle in the app; recorded with who and when |

## The off switch

Keanu Widjaja or the treasurer, signed in, turns on “Paused” in the app’s settings. While paused, BudgetPilot answers “paused” and calls no AI.

## How mistakes are reported

A “Report a problem” button on every result saves a report that only signed-in reviewers can read. Keanu reviews reports every week, and each fix becomes a switch change, a lesson, or a code change. No blame.

## Rules we keep

- Every permission starts off. The agent may do only what this plan lists under *May* and *May, with a limit*.
- Writing a permission down does not enforce it. Every limit and every *Never* is enforced in code or in a database rule, never in instructions alone.
- A limit a person may change is a setting with an owner and a reason, which the code reads on every run.
- Asking means holding, not guessing: the record waits with a plain reason that names what is missing.
- A sign-off is a click in the app, recorded with who and when. A chat message is not a sign-off.
- Test mode never acts: with `dry_run`, the agent decides but saves nothing.
