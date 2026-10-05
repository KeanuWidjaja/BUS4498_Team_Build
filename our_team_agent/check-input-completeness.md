# Check input completeness Task Specification

## Basic Information

- **Task ID:** T2
- **Task name:** Check input completeness
- **Task type:** Reason
- **Task owner:** Keanu Widjaja

## 1. Task Description

Check whether the supplied information is sufficient to distinguish available funds, intended spending, prior spending where provided, and requester priorities; identify omissions or ambiguities without filling them with unsupported values.

## 2. Inputs

### Input 1

- **Input name:** Registered Budget Planning inputs
- **Contents and format:** A structured record containing available funds, currency, event or budgeting period, budget request, and any supplied needs, categories, priorities, planned spending, or past spending. Each entry identifies its source. Missing fields and conflicting entries remain explicitly marked.
- **Source:** T1 Register supplied budget inputs.

- **If a required input is missing or invalid:** f the record is absent or unreadable, return it to T1 for correction. If the record is readable but contains missing, conflicting, or unclear budget details, identify those gaps and route them to T3 · Request missing details.

## 3. Outputs

### Output 1

- **Output name:** Budget input completeness assessment
- **Contents and format:** A structured assessment stating whether the inputs are sufficient for planning. List each missing, conflicting, or unclear detail, its source, and the clarification needed. Distinguish required information from optional information, including past spending where available.
- **Next task or recipient:** T3 · Request missing details when clarification is needed; otherwise, T4 · Compare planned and past spending.
- **Complete when:** Each completeness criterion has a recorded finding, identified gaps reference the relevant inputs, and the assessment states whether clarification is needed before proceeding.

## 4. Planned Tools

### Tool 1

- **Tool name:** check_input_completeness
- **Input:** Registered budget planning inputs
- **Output:** Budget input completeness assessment
- **Implementation Route:** Proposed function/script using AI to assess the inputs against predefined completeness criteria
- **Integration approach:** Direct intergration
- **Role in this task:** Identify missing, conflicting, or unclear budget details and specify needed clarification without inventing values
- **Task timeout:** 60 seconds per attempt
- **Maximum retries:** 2
- **Retry only when:** A temporary service interruption or response timeout prevents completion. Wait 10 seconds between attempts. Reuse the same request identifier and input version, checking for an existing completed assessment to prevent duplicates. Missing budget details are assessment findings, not reasons to retry.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the failure status, request identifier, attempt count, and error details. Preserve the inputs and mark any partial assessment incomplete. Route the exception to the requester, such as the organization’s treasurer or authorized event organizer, for review.
