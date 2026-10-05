# Review Planning Result Task Specification

## Basic Information

- **Task ID:** T6
- **Task name:** Review planning result
- **Task type:** Verify
- **Task owner:** Human budget reviewer designated by the organization budget lead

## 1. Task Description

A human reviewer checks the draft budget planning options from T5 before anything is released to the requester. The reviewer compares the draft against the supplied planning inputs and the requester's stated priorities, and uses their own judgment to confirm input fidelity, spot arithmetic or interpretation concerns, confirm that assumptions are transparent and estimates are labelled, and confirm that the draft does not make purchases, transfers, guarantees, or priority decisions for the requester. The reviewer then records one review outcome: revisions approved, corrections required, or decision unresolved.

## 2. Inputs

### Input 1

- **Input name:** Draft set of budget planning options
- **Contents and format:** Two or more options, each with allocations across categories and events, assumptions (estimates labelled), and trade-offs. Delivered as a document or file.
- **Source:** T5 · Draft budget planning options

### Input 2

- **Input name:** Planning-input set
- **Contents and format:** The organized record of the requester's request, available funds, planned spending, past spending, event needs, categories, and priorities, with each item traceable to the original request. Structured record or document.
- **Source:** T1 · Register supplied budget inputs

### Input 3

- **Input name:** Spending comparison
- **Contents and format:** Planned vs. past spending by category or activity, with material gaps, changing-cost considerations, and labelled estimates. Table or document.
- **Source:** T4 · Compare planned and past spending

- **If a required input is missing or invalid:** Review does not start. The reviewer records "review not possible" with the name of the missing or unreadable input, and the case goes to the organization budget lead through the "Send unresolved-input packet" handoff.

## 3. Outputs

### Output 1

- **Output name:** Review outcome and notes
- **Contents and format:** One outcome (revisions approved, corrections required, or decision unresolved), plus review notes listing each required correction, each unresolved decision or item, and any concerns about input fidelity, arithmetic, interpretation, assumptions, or alignment with requester priorities. Short structured record or document.
- **Next task or recipient:** Routing at "Review outcome?": revisions approved goes to T7 · Revise reviewed planning result; corrections required goes to T5 · Draft budget planning options; decision unresolved goes to the "Send unresolved-input packet" handoff.
- **Complete when:** Exactly one outcome is recorded, every required correction and every unresolved item is listed in the notes, and the reviewer has signed the record with their name and the review date.

## 4. Planned Tools

### Tool 1

- **Tool name:** present_for_review
- **Input:** Draft set of budget planning options; Planning-input set; Spending comparison
- **Output:** Review outcome and notes
- **Implementation Route:** File operations. The three input files are retrieved and placed together in one review packet for the reviewer. The reviewer's completed outcome and notes are saved back to a review record file.
- **Integration approach:** Direct integration
- **Role in this task:** Retrieves the draft, the planning-input set, and the comparison and hands them to the human reviewer in one place. It accepts the reviewer's outcome and notes and saves them as the review record. The tool does not judge the draft.
- **Task timeout:** The reviewer responds within one business day after the packet is assigned.
- **Maximum retries:** Not applicable — manual task.
- **Retry only when:** Not applicable
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the status "review overdue" or the error, with the time assigned. Send the case to the organization budget lead for reassignment. Do not pass the draft to T7 or release it to the requester as if it had been reviewed.
