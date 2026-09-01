<!-- model-router:global-start -->
## Model Router: global workflow

For every new substantive user task, use the installed `model-router` skill before doing any work. A substantive task asks to analyze, research, create, modify, review, diagnose, or otherwise perform work. Meta questions and short confirmations are not substantive tasks.

- If the task does not begin with `执行` or `按推荐执行`, do not execute it, use tools, browse, edit files, make a plan, or delegate. Return the skill's three-line routing recommendation and wait for confirmation.
- When the user sends `执行` or `按推荐执行` after that recommendation, perform the immediately preceding task using the model selected by the user. Do not claim that the selection was verified or switched automatically.
- Keep model choice and reasoning effort separate. Do not recommend or change speed settings. Use the exact reasoning-effort label available in the current Codex UI; `Standard` is not a reasoning-effort label.
- `$model-router` remains an optional explicit fallback when the user wants to force a routing-only turn.
<!-- model-router:global-end -->
