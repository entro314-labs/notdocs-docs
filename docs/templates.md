---
id: 01M4WNEPZJ53VQXB2H19GFR4S1
type: page
title: Templates guide
description: "Every built-in template, from problem statement to postmortem, mapped to the stage of a product where it is written."
order: 20
updated: 2026-09-16
---

## One template per document a product team writes

A template is a type plus a starting body. Several templates can start documents of one type: `prd` and `rfc` are both specs, `adr` and `deprecation` are both decisions. `notdocs templates` lists every one with its type and a line on what it is for; the new-document dialog shows the same list. A repository adds or replaces templates with files in `docs/_templates/`, see [Documents](documents.md#templates-and-custom-types).

```sh
notdocs docs new docs/specs/search.md --title "Search" --template prd
notdocs docs new docs/postmortems/2026-09-16-api.md --title "API outage" --template postmortem
```

The templates below are grouped by the stage of a product where each is written. Early on, `prd`, `rfc`, `launch` and `runbook` cover most of the value; the rest earn their place as the team grows.

## Discovery

| Template | Type | What it is |
|---|---|---|
| `problem` | `brief` | A problem statement or opportunity brief: the pain, who has it, the evidence |
| `competitive` | `report` | Market and competitive analysis, exportable to Word and PDF |

## Definition

| Template | Type | What it is |
|---|---|---|
| `prd` | `spec` | Product requirements: users, stories, scope, out of scope, success metrics |
| `persona` | `persona` | Who a user is: goals, frustrations, how they work today |
| `journey` | `page` | A journey map: stages, actions, thoughts, pain points |
| `pricing` | `spec` | Pricing and packaging: tiers, limits and the reasoning |

A roadmap is the `roadmap.md` page `notdocs init` writes, a [query block](query-blocks.md) over the tasks.

## Design

| Template | Type | What it is |
|---|---|---|
| `research` | `report` | UX research findings: method, participants, findings, recommendations |
| `accessibility` | `spec` | Accessibility requirements against WCAG |

## Engineering

| Template | Type | What it is |
|---|---|---|
| `rfc` | `spec` | A technical design: context, goals, design, alternatives, rollout |
| `adr` | `decision` | An architecture decision record with the options that were weighed |
| `decision` | `decision` | The shorter record: context, decision, consequences |
| `threat-model` | `spec` | Security threat model and data-handling review |
| `infrastructure` | `spec` | Environments, pipeline, infrastructure as code, access |
| `standards` | `page` | Engineering standards and the definition of done |

## Launch

| Template | Type | What it is |
|---|---|---|
| `launch` | `launch` | The launch checklist an agent or a person works through |
| `release` | `release` | Release notes for one version |
| `battlecard` | `playbook` | Sales positioning, objections and answers |
| `brief` | `brief` | A marketing brief: objective, audience, positioning, messages |
| `playbook` | `playbook` | A support playbook: questions, answers, when to escalate |
| `terms`, `privacy`, `nda`, `dpa`, `sla` | the same | The legal set, as [business documents](business-documents.md) that export to Word and PDF |

## Operations

| Template | Type | What it is |
|---|---|---|
| `runbook` | `runbook` | How to operate a service and fix its common failures |
| `oncall` | `page` | The on-call rotation, severity levels and escalation |
| `slo` | `slo` | A service level objective, its indicators and alerts |
| `disaster-recovery` | `runbook` | Backups, recovery objectives and the recovery steps |
| `postmortem` | `postmortem` | An incident report: timeline, impact, root cause, follow-ups |
| `user-guide` | `page` | A customer-facing guide or onboarding page |

## Growth

| Template | Type | What it is |
|---|---|---|
| `metrics` | `page` | Metric definitions: north star, activation, retention, churn |
| `experiment` | `experiment` | An experiment brief: hypothesis, variants, metric, result |
| `feedback` | `page` | A customer feedback log and its themes |
| `quarterly-review` | `meeting` | A quarterly product review or retrospective |
| `deprecation` | `decision` | Retiring a feature: who is affected, timeline, migration |

:::tip[Types that carry structure]
`runbook`, `postmortem`, `release`, `experiment`, `persona`, `brief`, `slo` and `playbook` are types of their own, with fields a board or a query block can group by: a postmortem's `severity`, an experiment's `status`, an SLO's `service`. The rest start ordinary pages, specs, decisions and meetings. See [Documents](documents.md) for every field.
:::
