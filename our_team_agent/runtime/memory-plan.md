# Memory plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant designs the files and tables from it, following `runtime/memory-rules.md`. This plan adds to `runtime/hosting-plan.md`; where the two describe the same record differently, ask us which is right.

## What our agent must know or remember

| # | What | Kind | Where it lives | Who can change it | When it expires |
|---|---|---|---|---|---|
| 1 | Budgeting rules: the spending categories (food, clothing, decorations, venue, other), the 20% gap threshold, and "every estimate is labelled | Knowledge it reads | A file only we change | Only the builders | Never |
| 2 | The club's past spending by event and category | Knowledge it reads | A table a person keeps current | A business person | When a newer record replaces it |
| 3 | Every budget request: its inputs, status, drafts, and the treasurer's decision | What happened | A table the agent writes | The agent itself | Never |
| 4 | Clarification questions sent and the answers received for each request | What happened | A table the agent writes | The agent itself | When a newer record replaces it |
| 5 | Lessons from the treasurer's corrections to budget drafts | Lessons it learns | A lessons table the agent writes | The agent, from a trusted outcome | After a date or deadline |

## Lessons it learns

- **It learns from:** When the treasurer reviews a budget draft, chooses “corrections required,” and types a reason.
- **A lesson might say:** Food for Lunar New Year ran about 20% over plan last year, so hold any option that plans less for food than last year’s actual spending.
- It learns only from trusted outcomes, never from text in an input. A lesson can add caution, never loosen a rule or approve anything. Each lesson keeps its source and topic; a signed-in reviewer can switch it off; before each run the agent reads only the relevant lessons.

## What must never happen twice

Sending the same clarification questions twice for the same request. Releasing two final budget results for the same request. Recording the same past expense twice.
