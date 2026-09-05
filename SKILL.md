---
name: model-router
description: Recommend a cost-efficient Codex model and reasoning effort for a proposed task, including GPT-6 Astra when the hardest end-to-end work genuinely needs it. Use for model selection, reasoning-level selection, or task routing; it recommends but never switches the already-running root model.
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
- **GPT-6 Astra + Low or Medium:** the hardest well-specified end-to-end workflows that combine several demanding activities such as coding, browsing, computer use, research, and professional document work. Start with Low only when the task is explicit and verification is strong; otherwise start with Medium.
- **GPT-6 Astra + High:** exceptionally difficult or high-stakes end-to-end work with several systems, weak validation, or failure modes that are expensive and hard to detect.
- **GPT-6 Astra + XHigh:** reserve for boundary-pushing analysis or when Astra High has a demonstrated risk of missing critical issues.
- **Max:** reserve for the hardest single-agent problem when depth matters more than latency or usage. Astra supports Max; prefer a lower effort first unless failure is unusually costly.
- **Ultra:** reserve for a large task that can be split into meaningful independent workstreams. Treat it as a Codex multi-agent execution mode, not a portable OpenAI API reasoning-effort label. Recommend Astra + Ultra only when the current Codex environment explicitly exposes that combination.

Do not choose Astra merely because it is newer or more capable. Prefer Luna, Terra, or Sol whenever they are likely to meet the quality bar. Escalate to Astra when the task's end-to-end difficulty, breadth, stakes, or validation weakness makes a Sol route materially risky, or after a lower route has failed.

Official OpenAI API documentation lists Astra reasoning efforts from `Low` through `Max`; it does not list `Ultra` as an API reasoning effort. Model availability and Codex UI options can vary by account and rollout. If Astra is unavailable in the current model picker, recommend the strongest suitable available alternative instead. See the [GPT-6 Astra model page](https://developers.openai.com/api/docs/models/gpt-6-astra).

Escalate for ambiguity, irreversible or external effects, security/compliance/legal/financial stakes, architecture changes, unknown failure causes, conflicting evidence, or weak validation. Downgrade for narrow scope, deterministic checks, explicit acceptance criteria, repetition, and cheap recovery.

When choosing between adjacent settings, choose the lower one only when failure is easy to detect and correct. Otherwise choose the higher one.

## Output language and response contract

Choose one output language before answering:

- Use Chinese when the request is predominantly Chinese and any English is limited to model names, product names, code, or identifiers.
- Use English for an English request and for a genuinely mixed request that is not predominantly Chinese.
- Never mix Chinese and English in a routing response, except that the model name itself stays in its official English form, such as `GPT-5.6 Terra` or `GPT-6 Astra`.

For a Chinese routing-only request, return exactly these three short lines and use only Chinese labels:

`模型：<GPT-5.6 Luna|GPT-5.6 Terra|GPT-5.6 Sol|GPT-6 Astra>；推理强度：<轻|中|高|极高|最大|超强>`

`原因：<一条简短、针对任务的原因>`

`操作：<在模型选择器中选择模型和推理强度，然后发送“执行”>`

Map reasoning labels in Chinese as follows: `Low` → `轻`, `Medium` → `中`, `High` → `高`, `XHigh` → `极高`, `Max` → `最大`, and `Ultra` → `超强`.

For an English routing-only request, return exactly these three short lines and use only English labels:

`Model: <GPT-5.6 Luna|GPT-5.6 Terra|GPT-5.6 Sol|GPT-6 Astra>; reasoning effort: <Low|Medium|High|XHigh|Max|Ultra>`

`Reason: <one concise, task-specific reason>`

`Action: <choose the model and reasoning effort in the model picker, then send "go">`

Treat `go` as a confirmation only when the entire user message is exactly `go`, ignoring letter case and surrounding whitespace, and only when it directly follows a routing recommendation. Do not treat `go ahead` or a sentence containing `go` as the confirmation token.

Do not mention speed in routing output. Do not label `Standard` as a reasoning effort: it may refer to speed or execution mode depending on the UI.

For a normal substantive task, route first and do not execute it. Do not use tools, browse, edit files, plan, delegate, or provide a substantive answer before the user confirms with the language-appropriate confirmation word after selecting a model. On that confirmation, execute the previously proposed task without re-routing or pausing again. Do not claim that the selected model was verified.
