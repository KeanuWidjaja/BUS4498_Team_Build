# Hosting plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant turns it into a GitHub Pages website and a Supabase backend, following `runtime/hosting-rules.md`. Where this plan and our task specifications disagree, this plan wins.

## Our project

- GitHub repository: https://github.com/KeanuWidjaja/BUS4498_Team_Build
- Our agent’s folder: `our_team_agent` (our plans are in `our_team_agent/runtime/`)
- Supabase project: https://uaultozpzbnppqwpjvuo.supabase.co
- Owner of the repository and the Supabase project: Keanu Widjaja
- Teammate: Angel Losoya, a collaborator on GitHub and a Developer in the Supabase organization
- We say “I’m working on it” and “pushed” in: Our Discord DM/Chat. One of us works at a time.

## What our agent does

Reads a club’s budget request for an event or month, compares planned spending with past spending by category, and drafts two or more budget options for the treasurer to review. It never makes purchases or moves money.

## Our tasks

Use the tasks in our task specifications: `our_team_agent/agent/workflow-of-tasks.md` and `our_team_agent/agent/task-specs/`.

## Who can see or change what

Club officers see and submit only their own budget requests and results. Only the treasurer (the budget reviewer) sees every request, approves or rejects each draft, and can switch lessons off. Past spending and available funds are never shown on the public page.

## External tools and applications

| # | Tool or application | What the agent uses it for | What it does with it | Most sensitive data it touches | How it connects |
|---|---|---|---|---|---|
| 1 | Gmail | Sends the clarification questions to the officer who made the request, after code checks there is one question per missing item and no made-up amount | Sends with it | Internal | A sign-in (OAuth) |
| 2 | Google Sheets | Reads the Club's past-spending log so the officer doesn't have to retype past spending | Reads from it | Confidential | An API key (in Supabase Secrets) |

## Files people upload

None.
