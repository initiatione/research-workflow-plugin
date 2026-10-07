# Research Workflow

一个统一管理四个独立 skill 的轻量插件，适用于机器学习与仿真科研实验。
每个 skill 按任务需要触发，不要求每次实验走完全部流程。

| Skill | 负责的决策 |
| --- | --- |
| [experiment-planning](skills/experiment-planning/SKILL.md) | 验证什么、先跑多少、何时扩大实验。探索先单 seed，方案稳定后补多 seed。 |
| [run-management](skills/run-management/SKILL.md) | 异步启动、资源允许时并行、进程恢复及产物归属。 |
| [fair-evaluation](skills/fair-evaluation/SKILL.md) | 比较条件、指标含义、统计单位及结论是否成立。 |
| [research-code-simplicity](skills/research-code-simplicity/SKILL.md) | 科研代码中的必要边界检查、重复 SHA、验证与维护成本。 |

## 使用

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
- 描述短而准确，参考文档按需读取，不把历史个案扩成所有任务的强制流程。

`plugin.json` 是可移植插件入口，`.codex-plugin/plugin.json` 提供 Codex
兼容清单；修改插件名称或版本时两者保持一致。技能文本在 `skills/` 中维护，
无需分别安装四份副本。仓库附带 marketplace 目录以统一发现和管理插件。

## 来源与验证

设计参考 OpenAI 官方的
[Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
以及 [codex-skills-library](https://github.com/sidiangongyuan/codex-skills-library)。
具体取舍见 [NOTICE.md](NOTICE.md)。

格式验证可使用 Codex 自带 skill-creator 的 `quick_validate.py` 对四个 skill
分别检查。发布前另做独立场景评审；格式通过不代表行为正确，也不代表已完成
某个客户端中的安装测试。插件本身不附加新的实验审计或校验框架。

MIT License.
