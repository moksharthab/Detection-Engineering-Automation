---
name: soc-playbook
description: Create SOC playbook content for detections and alerting logic. Use when Codex is given a detection query, SIEM rule, analytic logic, or alert condition and needs to produce JIRA-ready content with MITRE ATT&CK mapping, a two-sentence detection description, blind spots, false positives, and analyst investigation steps.
---

# SOC Playbook

## Overview

Generate concise, analyst-friendly playbook text from a detection query or rule. Keep the output operational, defensible, and tightly tied to what the detection logic can actually observe.

## Workflow

1. Read the detection query or rule carefully and identify the behavior, telemetry source, filters, thresholds, exclusions, and target entities.
2. Infer the detection objective from the logic itself rather than from speculative attack stories.
3. Map the behavior to the most direct MITRE ATT&CK technique or sub-technique.
4. Draft the required sections in the output format below.
5. State assumptions briefly when the rule is ambiguous, especially when the log source or field semantics are unclear.

## Output Format

Return these sections in this order:

- `Requirements`
- `MITRE ATT&CK Mapping`
- `Description`
- `Blind Spots`
- `False Positives`
- `Investigation Steps`

Apply these rules:

- In `Requirements`, provide exactly 2 bullets:
  - `Source: <log source name>`
  - `Event: <event type(s) or event activity described by the rule>`
- Use the log source named or implied by the detection in `Source`.
- Use the event types, API actions, operation names, or event activity explicitly used in the search logic in `Event`.
- When the rule does not specify exact normalized event names, prefer accurate descriptive wording such as `exec activity in GKE audit events` instead of over-specifying inferred event identifiers.
- Keep `Description` to exactly 2 sentences.
- Provide 3 to 4 bullets for `Blind Spots`.
- Provide 3 to 4 bullets for `False Positives`.
- Make `Investigation Steps` detailed, sequential, and practical for SOC analysts.
- Base every section on what the detection can and cannot see.
- Avoid generic filler such as "investigate the alert further" without concrete actions.

## MITRE ATT&CK Mapping

Map to the ATT&CK technique or sub-technique that best matches the suspicious behavior being detected, not merely the data source. Prefer the most specific sub-technique when the rule clearly supports it; otherwise provide the parent technique. If more than one mapping is relevant, lead with the primary mapping and only mention secondary mappings when they materially improve analyst understanding.
Keep a reference link to the exact MITRE ATT&CK page for the given TTP ID.
Keep any other useful references highlighting threat actors using this technique.

## Description

Write 2 concise sentences that explain:

- What activity the rule is looking for.
- Why that activity matters from an attacker or risk perspective.

Do not mention implementation trivia unless it is important to the security meaning of the rule.

## Blind Spots

List limitations of the detection logic, such as:

- Missing telemetry sources or unenriched fields.
- Evasion paths created by exclusions, thresholds, or narrow filters.
- Activity outside the monitored platform, protocol, or time window.
- Inability to distinguish intent, success, or downstream impact from the current rule alone.

## False Positives

List plausible benign explanations that could satisfy the rule. Prefer environment-specific legitimate activity patterns such as administrative scripts, scanners, management tooling, scheduled jobs, service accounts, or bulk automation rather than abstract statements.

## Investigation Steps

Write a practical triage workflow for SOC analysts. Cover the following where applicable:

1. Validate the alert by reviewing the matched fields, entity identifiers, timing, thresholds, and rule exclusions.
2. Identify the actor and asset context, including user, host, IP, service account, process, and business owner.
3. Determine the scope by pivoting across nearby events from the same entities before and after the alert.
4. Review corroborating telemetry such as process, authentication, network, email, cloud, or directory events depending on the rule type.
5. Assess maliciousness by comparing behavior against known admin activity, maintenance windows, approved tools, and historical baselines.
6. Decide escalation, containment, or closure criteria based on the evidence gathered.

When helpful, tailor the investigation steps to the actual telemetry implied by the query.

## Quality Bar

- Do not invent data sources, fields, or facts not supported by the rule text.
- Do not over-map to a high-profile attack technique when the logic only supports a generic behavior.
- Do not repeat the same idea across blind spots and false positives.
- Prefer crisp analyst language suitable for direct use in a JIRA ticket.
