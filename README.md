# Save Codex tokens with better model choices

**English** | [简体中文](README.zh-CN.md)

**Model Router** is a small Codex skill that helps you choose a capable model and reasoning effort *before* you start a task. Its goal is simple: avoid paying for more model capability or reasoning than the task needs, while keeping enough quality for the job.

If you want to save Codex tokens, reduce unnecessary Codex token usage, or make your Codex workflow more efficient, this skill gives you one practical decision point before work begins.

## One-time global mode (recommended)

After installation, Codex can discover this skill automatically when a request is about choosing a model or reasoning effort. Automatic discovery is helpful, but it is not a promise that every ordinary task will be routed first.

To make routing the default for **every substantive task**, enable the included global workflow once. After this one-time setup, users write their task normally; `$model-router` is only an explicit fallback when they want to force a routing-only turn.

Review `GLOBAL-ROUTING.md`, then run one command from the installed `model-router` folder:

**Windows PowerShell**

```powershell
.\scripts\global-routing.ps1
```

**macOS / Linux**

```sh
./scripts/global-routing.sh
```

The scripts add or update only a marked Model Router section in the user-level `AGENTS.md`; they preserve existing instructions and create a backup before changing that file. Preview without changing anything with `-Preview` (PowerShell) or `--preview` (macOS/Linux). Remove the managed section with `-Disable` or `--disable`.

## Why use it?

It is easy to leave a powerful model and a high reasoning setting on for every task. That is often reasonable for difficult work, but wasteful for routine work such as a focused edit, a predictable check, or a repeatable transformation.

Model Router recommends the lowest suitable starting point:

- **GPT-5.6 Luna** for narrow, repeatable, and easy-to-check work.
- **GPT-5.6 Terra** for ordinary production work and well-scoped multi-step tasks.
- **GPT-5.6 Sol** when the work is ambiguous, high-risk, difficult to verify, or needs deeper judgment.

It recommends the reasoning effort separately, so you can avoid treating every task as a highest-effort task.

## What it does not do

- It does **not** automatically switch the model of a task that is already running.
- It does **not** change your speed setting.
- It does **not** claim to know which option is currently selected in your Codex interface.
- It does **not** promise a fixed saving such as “60% fewer tokens.”

A fixed percentage would be misleading: your saving depends on the work you do, the model and effort you used before, the length of the conversation, and the quality bar you need to meet.

## How efficiency is measured

The skill's efficiency claim is limited and testable: it helps you avoid using a larger model or higher reasoning effort when a smaller setting still meets the task's quality bar.

To measure your own result, pick a representative set of tasks and compare your normal setting with the recommended setting. Track:

1. Whether the task succeeds and the output is complete.
2. Total token usage and cost.
3. Time to a usable result.
4. Rework or failures that force escalation.

Count a lower-token route as an improvement only when the work still passes your required quality checks. OpenAI's model guidance likewise recommends comparing representative tasks and testing one lower reasoning setting rather than assuming the highest setting is always the best trade-off. [OpenAI model guidance](https://developers.openai.com/api/docs/guides/latest-model)

## Install

Clone this repository into your Codex skills directory, using `model-router` as the folder name:

```text
<CODEX_HOME>/skills/model-router/
├── SKILL.md
└── agents/openai.yaml
```

Restart Codex or start a new task so the skill can be discovered.

## Use it

With global mode enabled, write the task normally. Codex should recommend the model and reasoning effort before it starts work.

Use `$model-router` only when you want to force a routing-only turn, for example:

```text
$model-router Review this database migration plan. Recommend the lowest suitable model and reasoning effort. Do not execute the review.
```

The skill returns a model, a reasoning-effort recommendation, and a short reason. Choose that combination in Codex, then start the actual task.

## Important details

- The root model for a task is chosen before the task begins. A skill can recommend a choice; it cannot change the root model mid-task.
- `Standard` is not a reasoning-effort recommendation. Use the exact reasoning label available in your current Codex model picker; `Standard` may describe speed or execution mode instead.
- The router is decision support, not a guarantee. Use a stronger model or higher effort when an error would be costly or hard to detect.

## Search keywords

Codex token saving · save Codex tokens · reduce Codex token usage · token-efficient Codex workflow · improve Codex efficiency · Codex model selection · Codex reasoning effort · GPT-5.6 Luna · GPT-5.6 Terra · GPT-5.6 Sol

## Repository contents

- `SKILL.md` — routing instructions loaded by Codex.
- `agents/openai.yaml` — skill display metadata and automatic-discovery policy.
- `GLOBAL-ROUTING.md` — the small, managed user-level instruction block for global mode.
- `scripts/global-routing.ps1` and `scripts/global-routing.sh` — one-time enable, preview, update, and disable commands.
- `README.md` and `README.zh-CN.md` — English-first, bilingual documentation.

## Validate the skill

From an environment with the bundled Skill Creator tools:

```text
python quick_validate.py <path-to-model-router>
```

This checks the skill structure and frontmatter. It does not prove that every recommendation is optimal; validate the routing with representative tasks before relying on it for important work.
