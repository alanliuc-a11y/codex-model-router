# 用更合适的模型节省 Codex 令牌

[英文说明](README.md) | **中文说明**

**模型路由器**是一个小型 Codex 技能。它会在你开始任务**之前**，从 GPT-5.6 Luna、Terra、Sol 和 GPT-6 Astra 中建议一个足够胜任的模型，并单独推荐推理强度。目标很直接：在质量够用的前提下，避免为不需要的模型能力或推理消耗付费。

如果你想节省令牌、节省 Codex 令牌、降低 Codex 令牌消耗，或提高 Codex 的使用效率，这个技能会在执行前给出一个可操作的选择建议。

## 最方便的安装方式：直接让 Codex 安装

在 Codex 中复制并发送下面**整段话**。不要只发送一个裸 GitHub 链接。

```text
请使用技能安装器从 GitHub 仓库 alanliuc-a11y/codex-model-router 安装 Codex 技能；仓库内路径使用 .，技能名称使用 model-router。安装完成后，请运行当前操作系统对应的附带脚本，启用它的全局路由功能；保留我现有的 AGENTS.md 规则，并在准备好后告诉我。
```

这个仓库把技能放在根目录，因此安装器需要路径 `.`。全局路由启用完成后，以后的任务都可以像平时一样直接输入，不需要每次加 `$model-router`。

## 第一步：安装

把本仓库克隆到 Codex skills 目录中，文件夹名称使用 `model-router`：

```text
<CODEX_HOME>/skills/model-router/
├── SKILL.md
└── agents/openai.yaml
```

重启 Codex 或新建一个任务，让应用重新发现该 skill。然后在已安装的 `model-router` 目录中运行一次全局启用命令：

**Windows PowerShell**

```powershell
.\scripts\global-routing.ps1
```

**macOS / Linux**

```sh
./scripts/global-routing.sh
```

这一次命令就是全局设置。请注意：这**不是**在聊天里第一次加一次 `$model-router` 就永久生效；前缀本身不是开关。脚本只会添加或更新用户级 `AGENTS.md` 中带标记的 Model Router 区块：保留原有规则，并在修改前创建备份。PowerShell 使用 `-Preview`、macOS/Linux 使用 `--preview` 可先预览；分别用 `-Disable`、`--disable` 删除受管理的区块。

## 第二步：使用

完成第一步后，每次对话都像平时一样直接输入任务即可。**不需要**在每次对话前加 `$model-router`。Codex 应先给出模型和推理强度建议，再开始工作。

只有在希望强制进入“只推荐、不执行”时，才显式加 `$model-router` 作为兜底，例如：

```text
$model-router 审核这份数据库迁移方案，推荐足够且最节省的模型和推理强度；不要执行审核。
```

技能会返回模型、推理强度和简短原因。请先在 Codex 中选好对应组合，再开始实际任务。中文交流时，确认词为 `执行` 或 `按推荐执行`。

## 它为什么有用？

很多人会一直使用最强模型和最高推理强度。面对困难任务，这样做可能合理；但对一次明确的修改、规则固定的检查或可重复的转换来说，往往没有必要。

模型路由器会建议一个尽量低、但仍适合的起点：

- **GPT-5.6 Luna**：范围窄、可重复、结果容易验证的工作。
- **GPT-5.6 Terra**：日常生产工作和边界清楚的多步骤任务。
- **GPT-5.6 Sol**：歧义大、风险高、难以验证，或需要更深判断的任务。
- **GPT-6 Astra**：只用于最困难的端到端工作，例如同时涉及编程、浏览器操作、电脑操作、研究和专业文档，而且错误代价高或很难发现的任务。

它会把“模型”和“推理强度”分开建议，避免所有任务都默认使用最高推理强度。Astra 不会成为新的默认模型；只要 Luna、Terra 或 Sol 足以达到质量要求，路由器仍会优先选择成本更低的模型。

## 它不会做什么

- 不会自动切换一个已经开始运行的任务的模型。
- 不会修改你的速度设置。
- 不会声称知道你当前 Codex 界面里选中了什么。
- 不会承诺“节省 60% Token”这类固定比例。

固定百分比并不可靠：实际节省取决于任务类型、原先使用的模型与推理强度、对话长度，以及你要求达到的质量标准。

## “效率提高”如何验证？

这个 skill 所说的效率是可检验的：当更小的模型或更低的推理强度仍能达到质量要求时，避免使用更大的配置。

建议选择一组有代表性的任务，把平时的设置与推荐设置进行对比，记录：

1. 任务是否完成，输出是否完整。
2. 总 Token 消耗与成本。
3. 得到可用结果所需的时间。
4. 是否出现返工、失败或必须升级配置的情况。

只有当质量检查也通过时，Token 更低才算真正的效率提升。OpenAI 的模型指南同样建议使用有代表性的任务比较，并尝试降低一档推理强度，而不是假定最高设置总是最佳取舍。[OpenAI 模型指南](https://developers.openai.com/api/docs/guides/latest-model)

## 重要说明

- 一个任务的根模型会在任务开始前确定。skill 只能建议，不能在任务运行中自动切换根模型。
- `标准` 不是推理强度建议；它可能是速度或执行模式。
- 在 Codex 中，本路由器只管理 GPT-5.6 Luna、Terra、Sol 和 GPT-6 Astra。若你同时安装了通用的跨平台模型路由器，应将后者设为仅显式调用；否则它的“当前可用模型中最低够用”策略可能与这里的受管模型目录冲突。
- 如果模型选择器中没有任何受管模型，路由器应报告这一不一致，而不是静默改用 GPT-5.4 Mini 等其他选项。
- OpenAI 的接口文档列出的 GPT-6 Astra 推理强度为 `Low`、`Medium`、`High`、`XHigh` 和 `Max`。如果 Codex 界面提供 `Ultra`，本路由器把它视为 Codex 的多智能体执行模式，而不是通用的接口推理强度值。
- Astra 是否可用取决于账号和发布进度。如果当前模型选择器中没有 Astra，应选择当下可用的最强合适替代模型。
- 路由器是决策辅助工具，不是质量保证。若错误代价高或很难发现，应主动使用更强模型或更高推理强度。

官方资料：[GPT-6 Astra 模型页面](https://developers.openai.com/api/docs/models/gpt-6-astra)和[OpenAI 模型指南](https://developers.openai.com/api/docs/guides/latest-model)。

## 搜索关键词

节省 Token · 节省 Codex Token · 降低 Codex Token 消耗 · 提高 Codex 效率 · Codex 模型选择 · Codex 推理强度 · GPT-5.6 Luna · GPT-5.6 Terra · GPT-5.6 Sol · GPT-6 Astra · Astra 模型路由

## 仓库内容

- `SKILL.md`：由 Codex 加载的路由规则。
- `agents/openai.yaml`：skill 的展示信息与自动发现策略。
- `GLOBAL-ROUTING.md`：用于全局模式的小型、受管理用户级规则区块。
- `scripts/global-routing.ps1`、`scripts/global-routing.sh`：一次性启用、预览、更新和禁用命令。
- `README.md`、`README.zh-CN.md`：英文在前、中文配套的说明文档。

## 校验 skill

在安装了 Skill Creator 工具的环境中运行：

```text
python quick_validate.py <model-router 路径>
```

该校验会检查目录结构与 frontmatter；它不会证明每一次建议都最优。对重要工作，发布或采用前仍应使用有代表性的任务进行验证。
