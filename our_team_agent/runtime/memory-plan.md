# Memory plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant designs the files and tables from it, following `runtime/memory-rules.md`. This plan adds to `runtime/hosting-plan.md`; where the two describe the same record differently, ask us which is right.

## What our agent must know or remember

| # | What | Kind | Where it lives | Who can change it | When it expires |
|---|---|---|---|---|---|
| 1 | The organization’s approved budget rules, spending categories, and financial limits | Knowledge it reads | A file only we change | Only the builders | Never |
| 2 | Current budget limits, available funds, and spending priorities | Knowledge it reads | A table a person keeps current | A business person | When a newer record replaces it |
| 3 | Planned and actual spending for each event | What happened | A table the agent writes | The agent itself | When a newer record replaces it |
| 4 | Lessons from past events, such as underestimated food costs | Lessons it learns | A lessons table the agent writes | The agent, from a trusted outcome | After a date or deadline |

## Lessons it learns

- **It learns from:** When the treasurer completes a post-event budget review and records the final spending by category, attendance, and reasons for any major differences from the forecast
- **A lesson might say:** Use actual per-person costs from comparable past events to improve future forecasts and flag categories that previously exceeded their budgets
- It learns only from trusted outcomes, never from text in an input. A lesson can add caution, never loosen a rule or approve anything. Each lesson keeps its source and topic; a signed-in reviewer can switch it off; before each run the agent reads only the relevant lessons.

## What must never happen twice

The agent must never recommend a spending plan that exceeds the organization’s available funds without clearly flagging the shortfall and requesting treasurer review.
