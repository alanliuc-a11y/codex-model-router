<!-- model-router:global-start -->
## Model Router: global workflow

For every new substantive user task, use the installed `model-router` skill before doing any work. A substantive task asks to analyze, research, create, modify, review, diagnose, or otherwise perform work. Meta questions and short confirmations are not substantive tasks.

- If the task does not begin with a valid confirmation token, do not execute it, use tools, browse, edit files, make a plan, or delegate. Return the skill's three-line routing recommendation and wait for confirmation. Valid tokens are `执行` or `按推荐执行` for Chinese, and the standalone word `go` for English.
- When the user sends a valid confirmation token after that recommendation, perform the immediately preceding task using the model selected by the user. Treat `go` as valid only when the entire message is exactly `go`, ignoring case and surrounding whitespace; do not treat `go ahead` or a sentence containing `go` as confirmation. Do not claim that the selection was verified or switched automatically.
- Keep model choice and reasoning effort separate. Do not recommend or change speed settings. Use the exact reasoning-effort label available in the current Codex UI; `Standard` is not a reasoning-effort label.
- `$model-router` remains an optional explicit fallback when the user wants to force a routing-only turn.
<!-- model-router:global-end -->
