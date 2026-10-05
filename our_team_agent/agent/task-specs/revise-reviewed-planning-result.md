# Revise Reviewed Planning Result Task Specification

## Basic Information

- **Task ID:** T7
- **Task name:** Revise reviewed planning result
- **Task type:** Act
- **Task owner:** Organization budget lead (accountable for the released planning result)

## 1. Task Description

After the human reviewer in T6 approves the draft with revisions, this task applies only the review-approved corrections to the draft budget planning options. It keeps every unresolved item and assumption visible in the result, and it labels estimates as estimates. It does not add new options, change allocations the reviewer did not approve, or make purchases, transfers, guarantees, or priority decisions on the requester's behalf. The model-supported revision follows a fixed procedure: apply each approved correction, preserve each unresolved item, then assemble the reviewed planning result for release and save it so the requester can access it.

## 2. Inputs

### Input 1

- **Input name:** Draft set of budget planning options
- **Contents and format:** Two or more options, each with allocations across categories and events, assumptions (estimates labelled), and trade-offs. Document or file.
- **Source:** T5 · Draft budget planning options

### Input 2

- **Input name:** Review outcome and notes
- **Contents and format:** The recorded outcome "revisions approved," plus review notes listing each approved correction, each unresolved decision or item, and the reviewer's name and review date. Short structured record or document.
- **Source:** T6 · Review planning result

### Input 3

- **Input name:** Planning-input set
- **Contents and format:** The organized record of the requester's request, available funds, planned spending, past spending, event needs, categories, and priorities, with each item traceable to the original request. Structured record or document.
- **Source:** T1 · Register supplied budget inputs

- **If a required input is missing or invalid:** Do not revise. Record the status "T7 blocked" with the name of the missing or invalid input (for example, a review record that is not marked "revisions approved"). Send the case to the organization budget lead through the "Send unresolved-input packet" handoff.

## 3. Outputs

### Output 1

- **Output name:** Reviewed budget planning result
- **Contents and format:** The revised options with allocations, assumptions (estimates labelled), and trade-offs. It includes a list of the corrections applied, a list of unresolved items and identified gaps carried forward, and the review sign-off (reviewer name and date). Document or file.
- **Next task or recipient:** The requester, through the saved reviewed result (workflow END · Save reviewed planning result).
- **Complete when:** Every review-approved correction is shown as applied, every unresolved item and assumption from the review notes still appears in the result, each option's total stays within the supplied available funds, and the file is saved in the agreed location and available to the requester.

## 4. Planned Tools

### Tool 1

- **Tool name:** revise_planning_result
- **Input:** Draft set of budget planning options; Review outcome and notes; Planning-input set
- **Output:** Reviewed budget planning result
- **Implementation Route:** Functions/scripts and file operations. A script reads the draft, the review notes, and the planning-input set from files. A model-generation function applies each approved correction to the draft using a fixed prompt template and the correction list. A script then checks that every approved correction and unresolved item appears in the result and that each option's total does not exceed the supplied funds. The result is written to a versioned file in the results location.
- **Integration approach:** Direct integration. All work uses files and functions inside the workflow, so no external system or MCP server is needed.
- **Role in this task:** Applies the approved corrections to the draft, preserves unresolved items and assumptions, and saves the reviewed planning result for the requester. It returns the saved file and a short change list. It does not add options, change unapproved content, or release anything outside the workflow's save location.
- **Task timeout:** 10 minutes total elapsed time per run.
- **Maximum retries:** 2
- **Retry only when:** The generation call fails or times out, the output file is malformed, or the completeness or arithmetic check fails. Wait 30 seconds between attempts. Each attempt overwrites the same output file for this request, so retries cannot create duplicate results, and nothing is sent externally by this task. Hand off if it is uncertain whether the file was saved.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the status "T7 failed" with the error message, the attempt count, and any partial result marked as incomplete. Send the draft, review notes, and failure notes to the organization budget lead through the "Send unresolved-input packet" handoff. Do not save or release the result as if it had succeeded.
