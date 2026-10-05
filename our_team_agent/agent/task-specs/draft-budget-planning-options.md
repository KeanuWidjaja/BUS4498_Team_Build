# Draft budget planning options Task Specification

## Basic Information

- **Task ID:** T5
- **Task name:** Draft budget planning options
- **Task type:** Reason
- **Task owner:** Keanu Widjaja

## 1. Task Description

Create planning options that reflect supplied funds and priorities, show assumptions and trade-offs, and avoid making purchases, transfers, guarantees, or priority decisions on the requester’s behalf.

## 2. Inputs

### Input 1

- **Input name:** T4 Comparison
- **Contents and format:** Planned vs past spending by category, with the material gaps, changing-cost considerations, and labelled estimates. May be in document format.
- **Source:** T4

### Input 2

- **Input name:** Available Funds
- **Contents and format:** Keanu Widjaja's budget request that was inputted into T1
- **Source:** Keanu Widjaja

- **If a required input is missing or invalid:** Alert Keanu Widjaja

## 3. Outputs

### Output 1

- **Output name:** A draft set of budget planning options
- **Contents and format:** Two or more ways that the available funds could be allocated across categories and events.
- **Next task or recipient:** T6
- **Complete when:** The draft contains at least two distinct options, each allocating the available funds across the requester's categories and events.

### Output 2

- **Output name:** Trade-offs
- **Contents and format:** What is gained and what is left underfunded or unaddressed if the requester chooses it.
- **Next task or recipient:** T6
- **Complete when:** Every option lists at least one gain and one underfunded item.

## 4. Planned Tools

### Tool 1

- **Tool name:** Draft budget options
- **Input:** T4 Comparison
- **Output:** A draft set of budget planning options
- **Implementation Route:** Functions/scripts and file operations. A script reads the planning-input set and the T4 comparison from files. A model-generation function drafts the options, assumptions, and trade-offs from a fixed prompt template. A script then checks that each option's total does not exceed the supplied funds, and the draft is written to a versioned file.
- **Integration approach:** Direct integration. Everything runs on files and functions inside the workflow, so no external system or MCP server is needed.
- **Role in this task:** Takes the T4 comparison, available funds, priorities, and categories, and drafts two or more allocation options. Each option includes labelled assumptions and trade-offs. It returns the draft file for T6 review. It makes no purchases, transfers, guarantees, or priority decisions and does not recommend an option.
- **Task timeout:** 10 minutes total elapsed time per run.
- **Maximum retries:** 2
- **Retry only when:** The generation call fails or times out, the output is malformed (missing assumptions or trade-offs), or the arithmetic check finds an option over the supplied funds. Wait 30 seconds between attempts. Each attempt overwrites the same draft file for this request, so retries cannot create duplicate drafts, and the task sends nothing externally.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the status "T5 failed" with the error message, the attempt count, and the last draft (if any) marked as incomplete. Send the request, supplied records, and failure notes to the organization budget lead through the "Send unresolved-input packet" handoff. Do not pass the draft to T6 as if it succeeded.
