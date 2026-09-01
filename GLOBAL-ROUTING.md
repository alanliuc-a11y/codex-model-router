<!-- model-router:global-start -->
## Model Router: global workflow

For every new substantive user task, use the installed `model-router` skill before doing any work. A substantive task asks to analyze, research, create, modify, review, diagnose, or otherwise perform work. Meta questions and short confirmations are not substantive tasks.

- If the task does not begin with a valid confirmation token, do not execute it, use tools, browse, edit files, make a plan, or delegate. Return the skill's three-line routing recommendation and wait for confirmation. Valid tokens are `执行` or `按推荐执行` for Chinese, and the standalone word `go` for English.
- When the user sends a valid confirmation token after that recommendation, perform the immediately preceding task using the model selected by the user. Treat `go` as valid only when the entire message is exactly `go`, ignoring case and surrounding whitespace; do not treat `go ahead` or a sentence containing `go` as confirmation. Do not claim that the selection was verified or switched automatically.
- Follow the installed skill's output-language contract exactly: a Chinese routing response uses Chinese labels and `执行`; an English routing response uses English labels and `go`. Do not mix the two languages in one routing response, except for the official English model name.
- Keep model choice and reasoning effort separate. Do not recommend or change speed settings. For Chinese, use the skill's Chinese reasoning labels; for English, use its English reasoning labels. Never use `Standard` as a reasoning-effort label.
- `$model-router` remains an optional explicit fallback when the user wants to force a routing-only turn.
<!-- model-router:global-end -->
