<!-- model-router:global-start -->
## Model Router: global workflow

This is the US$20 allowance profile. It applies the quota-conserving override after the base routing decision. This is an allowance strategy, not an official OpenAI subscription label.

For every new substantive user task, use the installed `model-router-20` skill before doing any work. A substantive task asks to analyze, research, create, modify, review, diagnose, or otherwise perform work. Meta questions and short confirmations are not substantive tasks.

- If the task does not begin with a valid confirmation token, do not execute it, use tools, browse, edit files, make a plan, or delegate. Return the skill's three-line routing recommendation and wait for confirmation. Valid tokens are `执行` or `按推荐执行` for Chinese, and the standalone word `go` for English.
- When the user sends a valid confirmation token after that recommendation, perform the immediately preceding task using the model selected by the user. Do not claim that the selection was verified or switched automatically.
- Follow the installed skill's output-language contract exactly. Keep model choice and reasoning effort separate. Do not recommend or change speed settings. Never use `Standard` as a reasoning-effort label.
- `$model-router-20` remains an optional explicit fallback when the user wants to force a routing-only turn.

### Fallback routing contract

- Use the same base rubric as the higher-allowance profile: Luna for narrow repeatable work, Terra for ordinary production work, Sol for ambiguous or high-risk work concentrated in one domain or a small number of systems, and Astra when at least two Astra signals are present.
- Astra signals are: three or more demanding work modes; end-to-end ownership from discovery through delivery; several interacting systems or artifact types; a long dependency chain; or weak validation, costly external effects, conflicting evidence, or hidden failures.
- After choosing the base route, replace every `GPT-6 Astra + 轻` or `GPT-6 Astra + 中` route with `GPT-5.6 Sol + 极高`. Replace every `GPT-6 Astra + 高`、`极高`、`最大`、or `超强` route with `GPT-6 Astra + 轻`.
- Do not silently replace the managed catalog with a different picker option. If none of Luna, Terra, Sol, or Astra is available, report the picker mismatch and stop.
- The fallback output is exactly three lines. For Chinese, start the first line with `模型：`; for English, start the first line with `Model:`.
<!-- model-router:global-end -->
