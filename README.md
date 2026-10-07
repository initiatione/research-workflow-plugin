# Research Workflow

一个面向 Claude Code、Codex 及兼容 Agent Skills 客户端的轻量科研插件。
四个独立 skill 使用同一份内容，不依赖特定模型；每个 skill 按任务需要触发，
不要求每次实验走完全部流程。

| Skill | 负责的决策 |
| --- | --- |
| [experiment-planning](skills/experiment-planning/SKILL.md) | 验证什么、先跑多少、何时扩大实验。探索先单 seed，方案稳定后补多 seed。 |
| [run-management](skills/run-management/SKILL.md) | 异步启动、资源允许时并行、进程恢复及产物归属。 |
| [fair-evaluation](skills/fair-evaluation/SKILL.md) | 比较条件、指标含义、统计单位及结论是否成立。 |
| [research-code-simplicity](skills/research-code-simplicity/SKILL.md) | 科研代码中的必要边界检查、重复 SHA、验证与维护成本。 |

## 使用

### Claude Code

```bash
claude plugin marketplace add initiatione/research-workflow-plugin
claude plugin install research-workflow@initiatione-research
```

安装后可按命名空间调用，例如 `/research-workflow:experiment-planning`。
更新时运行 `claude plugin marketplace update initiatione-research`，再运行
`claude plugin update research-workflow@initiatione-research`，重启会话后使用。

### Codex

将仓库加入 Codex marketplace：

```bash
codex plugin marketplace add initiatione/research-workflow-plugin
codex plugin add research-workflow@initiatione-research
```

支持 `plugin add` 的 Codex CLI 可用上面的第二条命令安装。也可以在
支持本地 marketplace 的 ChatGPT 桌面客户端插件目录中，选择
**Initiatione Research**，安装 **Research Workflow**，在新对话中使用。
不同客户端的插件安装支持可能不同；加入 marketplace 不等于完成安装。
更新源后可运行 `codex plugin marketplace upgrade initiatione-research`，
再按客户端提示刷新或重新启动。以 `codex plugin marketplace list` 显示的
实际 marketplace 名称为准。

### 其他客户端

`plugin.json` 使用可移植 Agent Plugins 格式。支持该格式的客户端可按其
安装流程加载整个插件；仅支持 Agent Skills 的客户端需要按自身规则注册
`skills/` 下的技能，并保留根目录 `references/` 与技能之间的相对路径。
不要仅复制单个 `SKILL.md`，否则按需读取的参考文档会丢失。

模型本身不负责安装插件；能否安装取决于承载模型的客户端。
目前提供 Claude Code 与 Codex 原生入口，其他客户端尚未逐一验证。

示例请求：

- “用 experiment-planning 设计一次单 seed pilot，给出合理预算与扩大条件。”
- “用 run-management 启动已确定的实验，空闲资源允许时并行运行。”
- “用 fair-evaluation 检查这张表能否直接比较，并指出需要重评的单元。”
- “用 research-code-simplicity 精简这段科研代码中重复的校验。”

插件没有 MCP、hooks、运行时依赖或自动启动训练的脚本。
它复用目标项目已有的环境、启动器、结果路径和质量工具。
此前已授权的执行可继续；插件不会额外要求每一步重复确认。

## 设计原则

- 训练预算依据 pilot、收敛趋势和实际训练暴露确定，避免过高迭代余量。
- 单 seed 用于快速探索；后续固定方案再补多 seed，不用单次结果声称稳定优势。
- 同一比较采用一致评估条件，实验刻意改变的变量单独说明。
- 跑实验前参考相关论文和项目定义指标；指标紧扣核心任务且能区分有意义的
  行为差异。例如位置姿态保持要求连续达标，单次到达不能算持续保持成功。
- 只保留真正边界的必要校验；SHA 不作为日常验收仪式。
- 变更后按科学影响复用结果，避免无差别重训重评。
- 长训练优先支持安全续训：完整恢复适用训练状态与累计进度，验证中断恢复
  与不中断执行的一致性；仅加载权重不算完整续训，仿真限制如实说明。
- 描述短而准确，参考文档按需读取，不把历史个案扩成所有任务的强制流程。

`plugin.json` 是可移植插件入口；`.claude-plugin/plugin.json` 与
`.codex-plugin/plugin.json` 分别提供 Claude Code 和 Codex 入口。
修改插件名称或版本时三者保持一致。技能文本在 `skills/` 中维护，
两种客户端读取同一份内容。仓库分别附带对应 marketplace 清单以统一管理插件。

## 来源与验证

设计参考 OpenAI 官方的
[Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
以及 [codex-skills-library](https://github.com/sidiangongyuan/codex-skills-library)。
具体取舍见 [NOTICE.md](NOTICE.md)。

Claude Code 可分别验证两份清单：

```bash
claude plugin validate .claude-plugin/plugin.json --strict
claude plugin validate .claude-plugin/marketplace.json --strict
```

Codex 可用自带 skill-creator 的 `quick_validate.py` 检查技能格式。
清单和技能格式已验证，Codex marketplace 入口已通过 CLI 识别；尚未进行
两个客户端的完整安装与新会话调用测试。此前已完成独立场景评审；格式通过
不代表行为正确。插件本身不附加新的实验审计或校验框架。

MIT License.
