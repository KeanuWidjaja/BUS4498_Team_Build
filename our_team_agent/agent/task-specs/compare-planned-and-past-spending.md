# Compare planned and past spending Task Specification

```yaml
# BASIC INFORMATION
task_id: "T4"
task_name: "Compare planned and past spending"
task_owner: "Keanu Widjaja"

# Agent Inference Configuration
Provider: "Anthropic"
Model: "claude-sonnet-5-5"
Role: "Assess registered budget inputs against predefined completeness criteria, identify missing, conflicting, or unclear information, and produce the Budget input completeness assessment without inventing values."
Maximum inference requests per task run: "3, including the initial request and up to two retries."
On inference failure or exhausted limits: "Record unresolved status, preserve the inputs and failure details, and mark any partial assessment incomplete. Hand off to the organization’s treasurer or authorized event organizer for manual assessment."
```

## 1. Task Goal

- **Objective:** Analyze supplied planned and historical spending by relevant category or activity, identify material gaps or changing-cost considerations supported by the records, and keep estimates clearly labelled as estimates.

## 2. Inbound Inputs

### Input 1

- **Input name:** Registered budget planning inputs
- **What it contains:** A structured record containing available funds, currency, event or budgeting period, budget request, and any supplied needs, categories, priorities, planned spending, or past spending. Each entry identifies its source. Missing information and conflicting entries remain clearly marked.
- **Source:** T1 · Register supplied budget inputs.

## 3. Tool Permissions and Boundaries

### Task-Wide Limits

- **Total task timeout:** 200 Seconds, including calls, retries, and waiting
- **Maximum tool calls:** 3 Total, including retries

### Tool 1

- **Tool name:** check_input_completeness
- **Tool type:** Language-model call using Anthropic’s claude-sonnet-5-5.
- **Supports these permitted subtasks:** Assess input completeness; Identify missing, conflicting, or unclear details; Produce completeness assessment
- **Allowed use:** Read Registered budget planning inputs from T1 and predefined completeness criteria. Create the Budget input completeness assessment in the workflow record for routing to T3 or T4.
- **Prohibited use:** Invent missing values, alter original inputs, resolve conflicts without supporting evidence, change requester priorities, access unapproved sources, or make purchases or transfers.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 60 seconds
- **Maximum retries per call:** 2 additional attempts, subject to the task-wide limit of 3 calls.
- **Retry conditions and failure response:** Retry only after a temporary service interruption or response timeout. Wait 10 seconds between attempts. Reuse the same request identifier and input version and check for an existing completed assessment before saving another output. Missing budget details do not trigger retries. For a non-retriable error, exhausted retries, or a total task timeout, record the unresolved status and failure evidence, preserve inputs, and mark the partial output as incomplete. Hand off to the organization’s treasurer or authorized event organizer.

Tool-specific and task-wide limits both apply; stop at whichever is reached first. Naming a tool does not authorize uses outside its stated permissions.

## 4. How the Agent Should Reason

### Permitted Subtask 1

- **Subtask name:** Asset input completeness
- **Subtask description:** Examine registered budget inputs against planning requirements. Produce an assessment identifying which information is sufficient and which areas require further examination.
- **Subtask boundary:** Read supplied inputs and predefined criteria only. Do not invent values, change priorities, or treat optional historical spending as mandatory. No approval is required within this boundary
- **Retry limits:** : Up to 2 additional attempts for temporary failures, within the shared limit of 3 tool calls and 200 seconds.  Permitted Subtask 2

- **Decision guidance:** After each subtask, use its findings to select the permitted subtask most likely to resolve the most important remaining uncertainty. Do not follow a fixed sequence. If no permitted subtask can make useful progress, stop and hand the case to a person.

## 5. When to Stop or Hand Off to a Human

- **Stop successfully when:** The Budget input completeness assessment records findings for every applicable criterion, references supplied evidence, identifies any clarification needs, and names the next task. The assessment may conclude that inputs are insufficient.
- **Hand off early when:** : Required source evidence cannot be accessed or traced, further examination makes no progress, a non-retryable error occurs, or resolving a finding requires authority outside the task. Stop when either 3 tool calls or 200 seconds is reached. Ordinary missing budget details route to T3 for clarification.
- **Hand off to:** The organization’s treasurer or authorized event organizer. Preserve the inputs, findings, and failure evidence, record unresolved status, and take no further autonomous action while awaiting review.

Stop at the first applicable budget limit or handoff condition. While awaiting review, take no further autonomous action.

## 6. Outbound Deliverable

- **Status:** Completed or escalated to human.
- **Result or recommendation:** The completed result. If escalated before reaching a supported result, write undetermined.
- **Evidence summary:** The most important evidence supporting the result or explaining why no result could be reached.
- **Subtasks performed:** Permitted subtasks completed, including repeated attempts.
- **Unresolved issues:** Remaining uncertainties or questions; use none only if no unresolved issue remains.
- **Handoff note:** Reason for stopping, unresolved questions, and what the reviewer needs to decide; write Not applicable for a completed task.
- **Next task or recipient:** T3 · Request missing details if clarification is needed; T4 · Compare planned and past spending if inputs are sufficient; the organization’s treasurer or authorized event organizer if human review is required.
