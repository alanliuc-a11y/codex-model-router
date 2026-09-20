---
name: model-router-20
description: Recommend a quota-conserving Codex model and reasoning effort for the US$20 allowance tier. Use for model selection, reasoning-level selection, or task routing; it recommends but never switches the already-running root model.
metadata:
  short-description: Recommend efficient routing for the US$20 allowance tier
---

# Model Router — US$20 Profile / 模型路由器（20 美金档）

Recommend the lowest-cost setup that is likely to complete the task correctly. Treat model choice and reasoning effort as separate decisions. This skill does not change speed settings.

## Plan profile

This package is the US$20 allowance profile. First determine the route using the full rubric below. Then apply the required US$20 override to every Astra route. This is a quota policy chosen by the installer, not an official OpenAI plan name, entitlement, or quality guarantee.

## Hard boundary

- The root model is selected before a turn starts. This skill cannot switch that already-running root model. State a recommendation, not a claim that a switch occurred.
- If the user asks only for routing, do not execute the proposed task. Return the recommendation so the user can select it before resubmitting the work.
- Do not create subagents merely to imitate a model switch for a small task; the coordinator plus subagent can consume more total usage. Use delegation only when the user requested it and the work genuinely divides into useful independent parts.
- Do not claim to know the active UI selection unless current task metadata explicitly exposes it. The configured default may have been overridden per task.

## Managed catalog

In Codex, this package manages `GPT-5.6 Luna`, `GPT-5.6 Terra`, `GPT-5.6 Sol`, and `GPT-6 Astra`. Do not implicitly defer to a generic or cross-platform routing skill. Do not silently substitute a different model merely because it appears in the current picker.

## Base routing rubric

- **GPT-5.6 Luna + Low:** mechanical, narrow, repeatable work with explicit inputs and outputs.
- **GPT-5.6 Luna + Medium:** clear bounded transformations or focused coding where success is easy to verify and failure is cheap.
- **GPT-5.6 Terra + Medium:** ordinary production work: document analysis, reporting, scoped coding, known bug fixes, routine QA, and tasks needing sound judgment or several tools.
- **GPT-5.6 Terra + High:** well-scoped but multi-step work across several files, sources, or tools, when planning and verification matter more than ambiguity.
- **GPT-5.6 Sol + Medium or High:** ambiguous, open-ended, difficult, or high-value work concentrated in one primary domain or a small number of systems.
- **GPT-5.6 Sol + XHigh:** only when High has a demonstrated risk of missing important issues, or for exceptionally difficult quality-first analysis.
- **GPT-6 Astra + Low or Medium:** broad, integrated end-to-end workflows with at least two Astra signals below.
- **GPT-6 Astra + High:** Astra-eligible work with high stakes, weak validation, conflicting evidence, or failure modes that are expensive and hard to detect.
- **GPT-6 Astra + XHigh or Max:** boundary-pushing analysis, or work where the lower Astra effort has a demonstrated risk of missing critical issues.
- **GPT-6 Astra + Ultra:** only when the current Codex environment explicitly exposes that multi-agent execution mode.

Recommend Astra in the base route when at least two of these signals are present:

- Three or more demanding work modes, such as coding, browsing, research, computer use, data analysis, media work, or professional documents.
- End-to-end ownership across discovery, implementation, verification, and delivery or publication.
- Several systems, apps, repositories, or artifact types whose interactions must remain consistent.
- A long chain of dependent stages where an early mistake can silently affect later work.
- Weak end-to-end validation, conflicting evidence, costly external effects, or hidden failure modes.

A single difficult activity or deep analysis in one domain still routes to Sol or below. Do not choose Astra merely because it is newer.

## Required US$20 override

After determining the base route, apply these replacements exactly:

| Base route | Final recommendation for this profile |
| --- | --- |
| GPT-6 Astra + Low | GPT-5.6 Sol + XHigh |
| GPT-6 Astra + Medium | GPT-5.6 Sol + XHigh |
| GPT-6 Astra + High | GPT-6 Astra + Low |
| GPT-6 Astra + XHigh | GPT-6 Astra + Low |
| GPT-6 Astra + Max | GPT-6 Astra + Low |
| GPT-6 Astra + Ultra | GPT-6 Astra + Low |

Leave Luna, Terra, and Sol base routes unchanged. This profile intentionally trades some reasoning depth for allowance conservation. Do not present the lower final effort as equivalent in quality to the original base route.

State that the final choice follows the US$20 allowance policy when the override changes an Astra base route. Give the user one final recommendation only; do not show a second competing base-route recommendation.

## Output language and response contract

Choose one output language before answering. Use Chinese when the request is predominantly Chinese; use English for English or genuinely mixed requests. Never mix Chinese and English in a routing response, except for official model names.

For a Chinese routing-only request, return exactly these three short lines:

`模型：<GPT-5.6 Luna|GPT-5.6 Terra|GPT-5.6 Sol|GPT-6 Astra>；推理强度：<轻|中|高|极高|最大|超强>`

`原因：<一条简短、针对任务的原因>`

`操作：<在模型选择器中选择模型和推理强度，然后发送“执行”>`

Map reasoning labels as follows: `Low` → `轻`, `Medium` → `中`, `High` → `高`, `XHigh` → `极高`, `Max` → `最大`, and `Ultra` → `超强`.

For an English routing-only request, return exactly these three short lines:

`Model: <GPT-5.6 Luna|GPT-5.6 Terra|GPT-5.6 Sol|GPT-6 Astra>; reasoning effort: <Low|Medium|High|XHigh|Max|Ultra>`

`Reason: <one concise, task-specific reason>`

`Action: <choose the model and reasoning effort in the model picker, then send "go">`

Treat `go` as a confirmation only when the entire user message is exactly `go`, ignoring case and surrounding whitespace. Do not mention speed or label `Standard` as a reasoning effort. For a normal substantive task, route first and do not execute it until the user confirms with the language-appropriate confirmation word.
