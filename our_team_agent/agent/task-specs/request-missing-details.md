# Request Missing Details Task Specification

## Basic Information

- **Task ID:** T3
- **Task name:** Request missing details
- **Task type:** Act
- **Task owner:** Organization budget lead (accountable for what is sent to the requester)

## 1. Task Description

When T2 finds that the supplied information is not sufficient, this task prepares focused clarification questions for each missing, conflicting, or unclear budget input and records the requester's response when it is supplied. The model-supported operation drafts the questions from the omissions and ambiguities T2 identified, following a fixed procedure: one question per gap, written in plain language, referring only to what the requester supplied. It does not suggest or fill in values the requester did not supply. The workflow needs this task so gaps are resolved by the requester instead of being guessed, and so that the case can be handed off if the details never arrive.

## 2. Inputs

### Input 1

- **Input name:** Completeness findings
- **Contents and format:** A list of each omission, conflict, or ambiguity that T2 found, each tied to the planning-input item it concerns. Structured list or table.
- **Source:** T2 · Check input completeness

### Input 2

- **Input name:** Planning-input set
- **Contents and format:** The organized record of the requester's request, available funds, planned spending, past spending, event needs, categories, and priorities, with each item traceable to the original request. Structured record or document.
- **Source:** T1 · Register supplied budget inputs

### Input 3

- **Input name:** Requester response
- **Contents and format:** The requester's answers to the clarification questions, in free text or a document. It may arrive late or not at all.
- **Source:** The requester

- **If a required input is missing or invalid:** If the completeness findings or the planning-input set is missing or unreadable, no questions are sent. Record the status "T3 blocked" with the name of the missing input and send the case to the organization budget lead through the "Send unresolved-input packet" handoff. A missing Requester response is handled by the "Clarification received?" routing: details unavailable goes to the "Send unresolved-input packet" handoff.

## 3. Outputs

### Output 1

- **Output name:** Clarification questions
- **Contents and format:** A short list of focused questions, one for each missing, conflicting, or unclear input, each stating which input it concerns and why it is needed. It makes no assumptions and suggests no values. Message or document sent to the requester.
- **Next task or recipient:** The requester; the routing then continues at "Clarification received?" (T3 → D2).
- **Complete when:** Every item in the completeness findings has a matching question, no question suggests or assumes a value, and the questions are sent to the requester with the send time recorded.

### Output 2

- **Output name:** Recorded clarification response
- **Contents and format:** The requester's answers, each matched to the question it answers, with the date received. Questions that remain unanswered are marked as unanswered. Structured record or document.
- **Next task or recipient:** T2 · Check input completeness (if details are received); otherwise the "Send unresolved-input packet" handoff to the organization budget lead (if details are unavailable).
- **Complete when:** Each question is marked answered or unanswered, each answer is saved next to its question with the date received, and the record is saved in the request's file.

## 4. Planned Tools

### Tool 1

- **Tool name:** draft_clarification_questions
- **Input:** Completeness findings; Planning-input set
- **Output:** Clarification questions
- **Implementation Route:** Functions/scripts and file operations. A script reads the findings and the planning-input set from files. A model-generation function drafts one focused question per gap from a fixed prompt template. A script then checks that every finding has a question and writes the questions to a file.
- **Integration approach:** Direct integration
- **Role in this task:** Turns each omission, conflict, or ambiguity into a focused question, using only information the requester supplied. It returns the question file and changes no records.
- **Task timeout:** 5 minutes total elapsed time per run.
- **Maximum retries:** 2
- **Retry only when:** The generation call fails or times out, or the check finds a finding without a question. Wait 30 seconds between attempts. Each attempt overwrites the same question file, so retries create no duplicates, and nothing is sent to the requester by this tool.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the status "T3 question drafting failed" with the error and attempt count. Send the request, supplied records, and completeness findings to the organization budget lead through the "Send unresolved-input packet" handoff. Do not send partial or unchecked questions to the requester.

### Tool 2

- **Tool name:** send_clarification_request
- **Input:** Clarification questions
- **Output:** Clarification questions (sent, with the send time recorded)
- **Implementation Route:** Function/script and file operations. The approved question file is sent to the requester through the organization's existing message channel (direct send function). The send time and message ID are saved to the request's file.
- **Integration approach:** Direct integration
- **Role in this task:** Delivers the prepared questions to the requester and records that they were sent.
- **Task timeout:** 2 minutes total elapsed time per run.
- **Maximum retries:** 1
- **Retry only when:** The send function returns a clear failure with no message ID, such as a connection error. Wait 60 seconds before the retry. Before retrying, check the request's file for a saved message ID, and do not send again if one exists, so the requester never gets duplicate messages. If it is uncertain whether the message was delivered, do not retry and hand off.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the status "T3 send failed" with the error and whether delivery is uncertain. Send the questions and the case to the organization budget lead through the "Send unresolved-input packet" handoff so they can be sent manually. Do not continue to "Clarification received?" as if the questions had been sent.

### Tool 3

- **Tool name:** record_clarification_response
- **Input:** Requester response
- **Output:** Recorded clarification response
- **Implementation Route:** Functions/scripts and file operations. A model-generation function matches each part of the requester's reply to the question it answers, and a script saves the matched answers, the date received, and any unanswered questions to the request's file.
- **Integration approach:** Direct integration
- **Role in this task:** Records the requester's answers next to the questions and marks unanswered questions, so T2 can re-check completeness with the new information. It does not infer answers the requester did not give.
- **Task timeout:** 5 minutes total elapsed time per run once a response arrives. The wait for the requester's reply is handled by the "Clarification received?" routing, not by this timeout.
- **Maximum retries:** 2
- **Retry only when:** The matching function fails or times out, or the saved record is malformed. Wait 30 seconds between attempts. Each attempt overwrites the same response record, so retries create no duplicates.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the status "T3 response not recorded" with the error and the raw requester reply attached. Send the case to the organization budget lead through the "Send unresolved-input packet" handoff. Do not pass an unmatched or partial record to T2 as if it were complete.
