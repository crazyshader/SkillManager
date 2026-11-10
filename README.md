# SkillManager

统一管理和分享 DeepSeek Harness（DSH）的用户自建技能。

## 目录结构

```
skills/
├── grill-me/                  # 动手前的追问式压力测试，仅手动触发（/grill-me）
├── handoff/                   # 将当前对话压缩为交接文档，仅手动触发
├── reverse-engineer-anything/ # 逆向分析与 AgentEnv 重建，仅手动触发
│   └── references/            # 按需读取的参考文档
└── writing-for-agents/        # 编写或修改 skill、AGENTS.md、CLAUDE.md 的指南
AGENTS.md                      # DSH 全局规则（语言规则），对应 ~/.dsh/AGENTS.md
DSH技能说明.md                  # 本机 DSH 技能来源与启用状态的记录
```

每个技能目录内保留原始的 `SKILL.md` 及其附属文件（如 `references/`）。

## 安装

将需要的技能目录复制到 DSH 技能目录：

```powershell
Copy-Item -Recurse .\skills\<技能名> "$HOME\.dsh\skills\"
```

安装后需要在新会话中才会生效。

## 说明

- 本仓库只收录用户自建技能，不包含第三方插件提供的技能。
- 第三方技能的来源和启用状态见 `DSH技能说明.md`。
- `AGENTS.md` 是 DSH 的全局规则文件，定义所有回复使用中文、代码和命令保持原样等约定。它与本机 `~/.dsh/AGENTS.md` 内容相同，复制到该位置即可生效。
