# Detection Documentation Automation Prompt

## Overview

This repository contains a reusable prompt pattern for turning any detection query into SOC-ready documentation and tracking artifacts.

The prompt accepts a rule name, title, detection query, sample Confluence page, sample Jira ticket, and destination parent page. It asks the agent to analyze the query, generate SOC playbook content, and use that output to populate matching Confluence and Jira artifacts in a consistent format.

## What The Prompt Does

The prompt instructs the agent to use the `soc-playbook` skill to convert the supplied detection logic into operational SOC content. That output is expected to include:

- Detection requirements
- MITRE ATT&CK mapping
- A concise detection description
- Blind spots
- Known false positives
- Analyst investigation steps
- Threat actor or technique references, where applicable

The generated content is then mapped into two destinations:

1. A Confluence runbook page under the provided parent page.
2. A Jira ticket modeled after an existing sample ticket and assigned to the current sprint.

## Input Pattern

The prompt is designed to work with any detection query as long as the required context is provided:

- Rule name
- Human-readable title
- Confluence parent page
- Sample Confluence page to replicate
- Sample Jira ticket to replicate
- Detection query or rule logic
- Required Jira metadata, such as component, story points, epic link, and sprint assignment

The detection query can come from Chronicle/YARA-L, SIEM search logic, ESQL, Sigma-style logic, or another detection format, as long as the behavior, data source, filters, and matched fields are clear enough to analyze.

## Detection Analysis

For any supplied query, the prompt expects the agent to identify:

- What telemetry source the rule uses
- What activity the rule detects
- Which users, hosts, accounts, groups, IPs, files, or other entities are extracted
- What filters, exclusions, thresholds, or reference lists shape the detection
- What security risk the behavior represents
- Which MITRE ATT&CK technique best matches the detected behavior
- What the rule cannot prove on its own
- What benign activity could produce similar matches
- What an analyst should check during triage

## Confluence Output

For each input query, the prompt asks the agent to create a Confluence page titled with the supplied detection title.

The page should be created under the provided Confluence parent page and should replicate the formatting, structure, color grading, and look and feel of the provided sample Confluence page.

The prompt maps SOC playbook output into Confluence sections as follows:

| SOC Playbook Output | Confluence Section |
| --- | --- |
| Description | Detection Details: Description and SOC Details: General Tuning |
| MITRE ATT&CK Mapping | Detection Details: MITRE Technique IDs |
| Threat Actor References | Detection Details: References |
| Requirements | Detection Details: Requirements |
| Blind Spots | SOC Details: Blindspots |
| Rule Name | SOC Details: Dashboard Name |
| Known False Positives | SOC Details: False Positives |
| Investigation Steps | SOC Details: Investigation Steps |

## Jira Output

For each input query, the prompt also asks the agent to create a Jira ticket that replicates the provided sample Jira ticket.

The new ticket should use the supplied title and configured Jira fields, such as:

- Component
- Story points
- Epic link
- Current sprint

The Jira ticket description should follow a strict section mapping:

| SOC Playbook Output | Jira Description Header |
| --- | --- |
| Description | Description |
| MITRE ATT&CK Mapping | MITRE Techniques |
| Requirements | Requirements |
| Rule Name | Dashboard Name |

## Expected Final Output

After the Confluence page and Jira ticket are created, the prompt expects clickable links in this format:

```text
Confluence page: <link>
Jira ticket: <link>
```

## Purpose

The overall purpose of this prompt is to standardize detection engineering documentation for any detection query. It turns raw detection logic into repeatable SOC runbook content and Jira implementation tracking, ensuring that detection context, MITRE mapping, operational requirements, false positives, blind spots, and investigation guidance are consistently captured across both Confluence and Jira.
