# Model Router for Codex

Model Router is a compact Codex skill that recommends the least expensive GPT-5.6 model and reasoning effort likely to complete a proposed task correctly. It is designed for people who want a repeatable recommendation before starting work, without claiming that Codex can change a running task's root model automatically.

[中文说明](README.zh-CN.md)

## What it does

- Routes narrow, repeatable work toward GPT-5.6 Luna.
- Uses GPT-5.6 Terra as the normal production default.
- Escalates to GPT-5.6 Sol only for ambiguity, high stakes, difficult judgment, or complex cross-system work.
- Recommends reasoning effort separately from the model: `Low`, `Medium`, `High`, `XHigh`, or `Max`.
- Reserves Codex Ultra for large tasks with meaningful independent workstreams.

It does not alter speed settings, create subagents merely to simulate a model switch, or claim to know which model is currently selected in the interface.

## Install

Clone this repository into your Codex skills directory with the folder name `model-router`:

```text
<CODEX_HOME>/skills/model-router/
├── SKILL.md
└── agents/openai.yaml
```

Restart Codex or begin a new task so it discovers the installed skill. Invoke it explicitly with `$model-router`, or let Codex select it when the request is about model or reasoning-effort choice.

## How to use it

Ask a routing-only question, for example:

```text
$model-router Review this migration plan and recommend the cheapest suitable model. Do not execute it.
```

The skill returns a model recommendation, a reasoning-effort recommendation, and a short reason. Select the recommendation in Codex before starting the actual task.

## Important behavior

- A task's root model is chosen before that task starts. A skill can recommend a model but cannot switch the current task automatically.
- `Standard` is not used as a reasoning-effort recommendation. Use the exact label shown by the current Codex UI for the selected effort; `Standard` may describe speed or execution mode instead.
- The routing rubric is guidance, not a guarantee. Escalate when failure would be expensive or difficult to detect.

## Repository contents

- `SKILL.md` — the routing instructions loaded by Codex.
- `agents/openai.yaml` — display metadata and automatic-discovery policy.
- `README.md` and `README.zh-CN.md` — English and Chinese usage documentation.

## Validation

From a Codex installation that includes the bundled Skill Creator tools:

```text
python quick_validate.py <path-to-model-router>
```

The validator checks skill structure and frontmatter. It does not prove the recommendation is optimal for every task, so test it against representative work.
