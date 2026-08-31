# Codex 模型路由器

模型路由器（Model Router）是一个简洁的 Codex skill：在任务开始前，根据任务范围、歧义、风险、重复度与验证难度，推荐“足以正确完成任务”的最低成本 GPT-5.6 模型与推理强度。

[English README](README.md)

## 它解决什么问题

- 将窄范围、重复、结果容易验证的工作优先路由到 GPT-5.6 Luna。
- 将 GPT-5.6 Terra 作为日常生产任务的默认建议。
- 仅在高歧义、高风险、困难判断或跨系统复杂工作时升级到 GPT-5.6 Sol。
- 将“模型”和“推理强度”分开推荐：`Low`、`Medium`、`High`、`XHigh`、`Max`。
- 仅把 Codex Ultra 用于确实能拆成多个独立工作流的大型任务。

它不会修改速度设置，不会为了模拟切换模型而无意义地创建子代理，也不会声称已知当前 UI 选择的模型。

## 安装

将仓库克隆到 Codex 的 skills 目录，并确保目录名为 `model-router`：

```text
<CODEX_HOME>/skills/model-router/
├── SKILL.md
└── agents/openai.yaml
```

重启 Codex 或新建任务，让应用重新发现 skill。可以显式输入 `$model-router`，也可以让 Codex 在“选择模型／推理强度”的请求中自动调用它。

## 使用示例

只请求路由、不执行实际任务：

```text
$model-router 审核这份数据库迁移方案，推荐足够且最节省的模型；不要执行审核。
```

skill 会返回模型、推理强度和简短原因。请在开始实际任务前，手动在 Codex 中选择该组合。

## 重要边界

- 一个任务的根模型会在任务开始前确定。skill 只能提出建议，不能自动切换正在运行的任务。
- 不把 `Standard` 当作推理强度输出。应以当前 Codex UI 显示的推理强度名称为准；`Standard` 可能属于速度或执行模式。
- 路由规则是决策辅助，不是质量保证。若失败代价高或难以发现，应主动上调模型或推理强度。

## 仓库内容

- `SKILL.md`：由 Codex 加载的路由规则。
- `agents/openai.yaml`：展示信息与自动发现策略。
- `README.md`、`README.zh-CN.md`：英文与中文说明书。

## 校验

在已安装 Codex 内置 Skill Creator 工具的环境中运行：

```text
python quick_validate.py <model-router 路径>
```

该校验检查 skill 的目录结构和 frontmatter，不会证明每一次模型推荐都最优；发布前仍应使用有代表性的任务进行试用。
