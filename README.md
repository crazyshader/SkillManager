# SkillManager

统一管理和分享 DeepSeek Harness（DSH）的用户自建技能。

## 目录结构

```
skills/
├── grill-me/                  # 动手前的追问式压力测试，仅手动触发（/grill-me）
├── handoff/                   # 将当前对话压缩为交接文档，仅手动触发
├── mattpocock/                # 参考 mattpocock/skills 思路编写的技能
│   ├── code-review/           # 审查一段改动，分别对照编码规范和 spec，仅手动触发
│   ├── diagnosing-bugs/       # 排查难复现的 bug 或性能退化，仅手动触发
│   ├── research/              # 用一手资料调研问题并写入仓库，仅手动触发
│   └── retro/                 # 复盘编码会话，提出改进 agent 工作环境的建议，模型可自动调用
├── reverse-engineer-anything/ # 逆向分析与 AgentEnv 重建，仅手动触发
│   └── references/            # 按需读取的参考文档
├── writing-for-agents/        # 编写或修改 skill、AGENTS.md、CLAUDE.md 的指南
└── superpowers/               # 来自 obra/superpowers（MIT）的 4 个技能与 SUPERPOWERS-NOTICE.md，仅手动触发
    ├── brainstorming/
    ├── writing-plans/
    ├── test-driven-development/
    ├── systematic-debugging/
    └── SUPERPOWERS-NOTICE.md
AGENTS.md                      # DSH 全局规则（语言、开源借鉴、代码修改原则、代码复用），对应 ~/.dsh/AGENTS.md
DSH技能说明.md                  # 本机 DSH 技能来源与启用状态的记录
docs/research/                # 调研笔记，每条结论附一手来源和查阅日期
```

每个技能目录内保留原始的 `SKILL.md` 及其附属文件（如 `references/`）。

## 安装

将需要的技能目录复制到 DSH 技能目录：

```powershell
Copy-Item -Recurse .\skills\<技能名> "$HOME\.dsh\skills\"
```

安装后需要在新会话中才会生效。

## 说明

- 本仓库收录用户自建技能。例外：`brainstorming`、`writing-plans`、`test-driven-development`、`systematic-debugging` 复制自 MIT 许可的 obra/superpowers，来源与许可证见 `skills/superpowers/SUPERPOWERS-NOTICE.md`。其正文只去掉了 `superpowers:` 前缀，frontmatter 增加了 `disable-model-invocation: true`。
- 这 4 个技能引用的其他技能（如 `using-git-worktrees`、`subagent-driven-development`、`executing-plans`、`verification-before-completion`、`writing-skills`）未收录，相关步骤在 DSH 中不可直接调用。
- 第三方技能的来源和启用状态见 `DSH技能说明.md`。
- `AGENTS.md` 是 DSH 的全局规则文件，定义所有回复使用中文、代码和命令保持原样、简洁优先与精准修改等约定。它与本机 `~/.dsh/AGENTS.md` 内容相同，复制到该位置即可生效。
