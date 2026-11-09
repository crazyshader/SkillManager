# SkillManager

统一管理和分享 DeepSeek Harness（DSH）的用户自建技能。

## 目录结构

```
skills/
├── grill-me/              # 技能入口，调用 grilling 技能
├── grilling/              # 设计树式追问，用于压力测试计划或决策
├── handoff/               # 会话交接
└── writing-for-agents/    # 编写或修改 skill、AGENTS.md、CLAUDE.md 的指南
DSH-AGENTS.md              # DSH 全局规则（语言规则），对应 ~/.dsh/AGENTS.md
DSH技能说明.md              # 本机 DSH 技能来源与启用状态的记录
```

每个技能目录内保留原始的 `SKILL.md` 及其附属文件（如 `agents/openai.yaml`）。

## 安装

将需要的技能目录复制到 DSH 技能目录：

```powershell
Copy-Item -Recurse .\skills\<技能名> "$HOME\.dsh\skills\"
```

## 说明

- 本仓库只收录用户自建技能，不包含第三方插件提供的技能。
- 第三方技能的来源和启用状态见 `DSH技能说明.md`。
- `DSH-AGENTS.md` 是 DSH 的全局规则文件，定义所有回复使用中文、代码和命令保持原样等约定。它与本机 `~/.dsh/AGENTS.md` 内容相同，复制到该位置即可生效。
