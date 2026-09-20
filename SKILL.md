---
name: model-router
description: Recommend a cost-efficient Codex model and reasoning effort for higher-allowance plans, including GPT-6 Astra when broad, integrated end-to-end work benefits from its coordination strength. Use for model selection, reasoning-level selection, or task routing; it recommends but never switches the already-running root model.
metadata:
  short-description: Recommend an efficient Codex model and effort
---

# Model Router — Higher-Allowance Profile / 模型路由器（高额度档）

Recommend the lowest-cost setup that is likely to complete the task correctly. Treat model choice and reasoning effort as separate decisions. This skill does not change speed settings.

## Plan profile

This root package is the higher-allowance profile, intended for plans above the US$20 allowance tier, such as interfaces that expose 5× or 20× usage. Apply the routing rubric below unchanged. The label describes an allowance strategy, not an official OpenAI subscription name or entitlement.

## Hard boundary

- The root model is selected before a turn starts. This skill cannot switch that already-running root model. State a recommendation, not a claim that a switch occurred.
- If the user asks only for routing, do not execute the proposed task. Return the recommendation so the user can select it before resubmitting the work.
- Do not create subagents merely to imitate a model switch for a small task; the coordinator plus subagent can consume more total usage. Use delegation only when the user requested it and the work genuinely divides into useful independent parts.
- Do not claim to know the active UI selection unless current task metadata explicitly exposes it. The configured default may have been overridden per task.

## Managed catalog and router collisions

In Codex, this is the authoritative router. Its managed catalog is `GPT-5.6 Luna`, `GPT-5.6 Terra`, `GPT-5.6 Sol`, and `GPT-6 Astra`. Do not implicitly defer to a generic or cross-platform routing skill, including `agent-model-router`; that skill is an explicit-only fallback when the user names it.

Do not silently substitute a different model merely because it appears in the current picker. If the picker does not expose any model in the managed catalog, say that the catalog is unavailable and ask the user to resolve it; do not recommend a substitute model or provide an execution confirmation.

## Routing rubric

- **GPT-5.6 Luna + Low:** mechanical, narrow, repeatable work with explicit inputs and outputs: search, extraction, classification, renaming, formatting, short translation, or a tiny deterministic edit.
- **GPT-5.6 Luna + Medium:** clear bounded transformations or focused coding where success is easy to verify and failure is cheap.
- **GPT-5.6 Terra + Medium:** default for ordinary production work: document analysis, reporting, scoped coding, known bug fixes, routine QA, and tasks needing sound judgment or several tools.
- **GPT-5.6 Terra + High:** well-scoped but multi-step work across several files, sources, or tools, when planning and verification matter more than ambiguity.
- **GPT-5.6 Sol + Medium or High:** ambiguous, open-ended, difficult, or high-value work that is concentrated in one primary domain or a small number of systems: unknown-cause debugging, architecture, deep research, polished deliverables, or decisions where weak judgment would be costly.
- **GPT-5.6 Sol + XHigh:** use only when High has a demonstrated risk of missing important issues, or for exceptionally difficult quality-first analysis. Prefer High as the initial attempt.
- **GPT-6 Astra + Low or Medium:** broad, integrated end-to-end workflows that meet at least two Astra signals below. Start with Low only when the task is explicit and verification is strong; otherwise start with Medium.
- **GPT-6 Astra + High:** Astra-eligible work with high stakes, weak validation, conflicting evidence, or failure modes that are expensive and hard to detect.
- **GPT-6 Astra + XHigh:** reserve for boundary-pushing analysis or when Astra High has a demonstrated risk of missing critical issues.
- **Max:** reserve for the hardest single-agent problem when depth matters more than latency or usage. Astra supports Max; prefer a lower effort first unless failure is unusually costly.
- **Ultra:** reserve for a large task that can be split into meaningful independent workstreams. Treat it as a Codex multi-agent execution mode, not a portable OpenAI API reasoning-effort label. Recommend Astra + Ultra only when the current Codex environment explicitly exposes that combination.

### Astra selection gate

Recommend Astra as the first choice when the task has at least two of these signals:

- It combines three or more demanding work modes, such as coding, browsing, research, computer use, data analysis, media work, or professional documents.
- It requires end-to-end ownership across discovery, implementation, verification, and delivery or publication.
- It spans several systems, apps, repositories, or artifact types whose interactions must remain consistent.
- It has a long chain of dependent stages where an early mistake can silently propagate into later work.
- It has weak end-to-end validation, conflicting evidence, costly external effects, or hidden failure modes.

Do not require proof that Sol will fail before choosing Astra. If two Astra signals are present, Astra's integration and coordination advantage is enough to justify the recommendation. A single difficult activity, deep analysis in one domain, or ordinary work across one or two tools should still route to Sol or below. Do not choose Astra merely because it is newer.

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
