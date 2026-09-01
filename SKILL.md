---
name: model-router
description: Recommend a cost-efficient Codex model and reasoning effort for a proposed task. Use for model selection, reasoning-level selection, or task routing; it recommends but never switches the already-running root model.
metadata:
  short-description: Recommend an efficient Codex model and effort
---

# Model Router / 模型路由器

Recommend the lowest-cost setup that is likely to complete the task correctly. Treat model choice and reasoning effort as separate decisions. This skill does not change speed settings.

## Hard boundary

- The root model is selected before a turn starts. This skill cannot switch that already-running root model. State a recommendation, not a claim that a switch occurred.
- If the user asks only for routing, do not execute the proposed task. Return the recommendation so the user can select it before resubmitting the work.
- Do not create subagents merely to imitate a model switch for a small task; the coordinator plus subagent can consume more total usage. Use delegation only when the user requested it and the work genuinely divides into useful independent parts.
- Do not claim to know the active UI selection unless current task metadata explicitly exposes it. The configured default may have been overridden per task.

## Routing rubric

- **GPT-5.6 Luna + Low:** mechanical, narrow, repeatable work with explicit inputs and outputs: search, extraction, classification, renaming, formatting, short translation, or a tiny deterministic edit.
- **GPT-5.6 Luna + Medium:** clear bounded transformations or focused coding where success is easy to verify and failure is cheap.
- **GPT-5.6 Terra + Medium:** default for ordinary production work: document analysis, reporting, scoped coding, known bug fixes, routine QA, and tasks needing sound judgment or several tools.
- **GPT-5.6 Terra + High:** well-scoped but multi-step work across several files, sources, or tools, when planning and verification matter more than ambiguity.
- **GPT-5.6 Sol + Medium or High:** ambiguous, open-ended, difficult, high-value, or cross-system work; unknown-cause debugging; architecture; deep research; polished final deliverables; or decisions where weak judgment would be costly.
- **GPT-5.6 Sol + XHigh:** use only when High has a demonstrated risk of missing important issues, or for exceptionally difficult quality-first analysis. Prefer High as the initial attempt.
- **Max:** reserve for the hardest single-agent problem when depth matters more than latency or usage.
- **Ultra:** reserve for a large task that can be split into meaningful independent workstreams. It is multi-agent delegation, not merely a stronger reasoning setting.

Escalate for ambiguity, irreversible or external effects, security/compliance/legal/financial stakes, architecture changes, unknown failure causes, conflicting evidence, or weak validation. Downgrade for narrow scope, deterministic checks, explicit acceptance criteria, repetition, and cheap recovery.

When choosing between adjacent settings, choose the lower one only when failure is easy to detect and correct. Otherwise choose the higher one.

## Response contract

For routing-only requests, return exactly these three short lines:

`模型：GPT-5.6 <Luna|Terra|Sol>；推理强度：<Low|Medium|High|XHigh|Max>`

`原因：<one concise task-specific reason>`

`操作：<在模型选择器中选择模型与对应推理强度，然后发送确认词>`

Use the user's language for the confirmation word:

- For a Chinese request, say `执行` (or `按推荐执行`).
- For an English request, say `go`.

Treat `go` as a confirmation only when the entire user message is exactly `go`, ignoring letter case and surrounding whitespace, and only when it directly follows a routing recommendation. Do not treat `go ahead` or a sentence containing `go` as the confirmation token.

Do not mention speed in routing output. If the Codex UI uses localized labels, use the exact label shown for the selected reasoning effort. Do not label `Standard` as a reasoning effort: it may refer to speed or execution mode depending on the UI.

For a normal substantive task, route first and do not execute it. Do not use tools, browse, edit files, plan, delegate, or provide a substantive answer before the user confirms with the language-appropriate confirmation word after selecting a model. On that confirmation, execute the previously proposed task without re-routing or pausing again. Do not claim that the selected model was verified.
