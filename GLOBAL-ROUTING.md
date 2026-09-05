<!-- model-router:global-start -->
## Model Router: global workflow

For every new substantive user task, use the installed `model-router` skill before doing any work. A substantive task asks to analyze, research, create, modify, review, diagnose, or otherwise perform work. Meta questions and short confirmations are not substantive tasks.

- If the task does not begin with a valid confirmation token, do not execute it, use tools, browse, edit files, make a plan, or delegate. Return the skill's three-line routing recommendation and wait for confirmation. Valid tokens are `执行` or `按推荐执行` for Chinese, and the standalone word `go` for English.
- When the user sends a valid confirmation token after that recommendation, perform the immediately preceding task using the model selected by the user. Treat `go` as valid only when the entire message is exactly `go`, ignoring case and surrounding whitespace; do not treat `go ahead` or a sentence containing `go` as confirmation. Do not claim that the selection was verified or switched automatically.
- Follow the installed skill's output-language contract exactly: a Chinese routing response uses Chinese labels and `执行`; an English routing response uses English labels and `go`. Do not mix the two languages in one routing response, except for the official English model name.
- Keep model choice and reasoning effort separate. Do not recommend or change speed settings. For Chinese, use the skill's Chinese reasoning labels; for English, use its English reasoning labels. Never use `Standard` as a reasoning-effort label.
- `$model-router` remains an optional explicit fallback when the user wants to force a routing-only turn.

### Conflict prevention and fallback contract

- In Codex, this is the authoritative router. Do not implicitly use a generic or cross-platform router, including `agent-model-router`; that skill may be used only when the user explicitly writes `$agent-model-router`.
- Do not rely on automatic skill discovery alone. If the `model-router` instructions are unavailable in a routing turn, apply this same managed catalog and rubric before responding:
  - `GPT-5.6 Luna` for narrow, repeatable, easy-to-check work; use `轻` or `中`.
  - `GPT-5.6 Terra` as the normal production default; use `中` or `高`.
  - `GPT-5.6 Sol` for ambiguous, difficult, high-risk, or cross-system work; use `中`、`高` or `极高`.
  - `GPT-6 Astra` only for the hardest end-to-end work across demanding tools or systems, when a Sol route is materially risky; use `轻` through `最大`, and use `超强` only when the current Codex environment explicitly exposes Astra + Ultra.
- Never silently replace the managed catalog with a different picker option such as `GPT-5.4-mini`. If none of Luna, Terra, Sol, or Astra is available, report the picker mismatch and stop; do not issue a confirmation prompt for a substitute model.
- The fallback output is still exactly three lines. For Chinese, start the first line with `模型：`, use `轻` rather than `低`, and do not use the label `推荐模型：`. For English, start the first line with `Model:`.
<!-- model-router:global-end -->
