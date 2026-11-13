# SkillManager

统一管理和分享 DeepSeek Harness（DSH）的用户自建技能。

## 目录结构

```
skills/
├── mattpocock/                # 参考 mattpocock/skills 思路编写或借鉴的技能
│   ├── code-review/           # 审查一段改动，分别对照编码规范和 spec，仅手动触发
│   ├── diagnosing-bugs/       # 排查难复现的 bug 或性能退化，仅手动触发
│   ├── grill-me/              # 动手前的追问式压力测试，仅手动触发（/grill-me）
│   ├── handoff/               # 将当前对话压缩为交接文档，仅手动触发
│   ├── research/              # 用一手资料调研问题并写入仓库，仅手动触发
│   ├── retro/                 # 复盘编码会话，提出改进 agent 工作环境的建议，模型可自动调用
│   └── writing-for-agents/    # 编写或修改 skill、AGENTS.md、CLAUDE.md 的指南，仅手动触发
├── caveman/                   # 来源 JuliusBrussee/caveman（Apache-2.0）,11 个简短输出与工作流技能，仅手动触发,见 NOTICE.md 与 docs/caveman使用说明.md
├── addyosmani/                 # 来源 addyosmani/agent-skills(MIT)25 个工程流程技能,仅手动触发,见 NOTICE.md 与 docs/addyosmani使用说明.md
├── ponytail/                  # 来源 DietrichGebert/ponytail(MIT),4 个"最小改动"风格与审查技能,仅手动触发,见 NOTICE.md 与 docs/ponytail使用说明.md
├── anthropics/                # 来源 anthropics/skills(Apache-2.0),mcp-builder 与 skill-creator,仅手动触发,见 NOTICE.md
├── vercel-labs/               # 来源 vercel-labs/skills(MIT),find-skills,仅手动触发,见 NOTICE.md 与 docs/find-skills使用说明.md
├── Egonex-AI/                 # 来源 Egonex-AI/Understand-Anything(MIT),9 个代码库理解技能及 agents/packages,仅手动触发,见 NOTICE.md 与 docs/Understand-Anything使用说明.md
├── nextlevelbuilder/          # 来源 nextlevelbuilder/ui-ux-pro-max-skill（MIT）,见 NOTICE.md
│   └── ui-ux-pro-max/         # UI/UX 设计知识库与设计系统生成器（Python 检索脚本）
├── blader/                    # 来源 blader/humanizer(MIT),去除 AI 写作腔,仅手动触发,仅存放在项目内、未安装到 DSH,见 NOTICE.md 与 docs/humanizer使用说明.md
│   └── humanizer/
├── ayghri/                    # 来源 ayghri/i-have-adhd(MIT),先给动作、编号步骤的输出风格,仅手动触发,仅存放在项目内、未安装到 DSH,见 NOTICE.md 与 docs/i-have-adhd使用说明.md
│   └── i-have-adhd/
├── reverse-engineer-anything/ # 逆向分析与 AgentEnv 重建，仅手动触发
│   └── references/            # 按需读取的参考文档
└── writing-for-agents/        # 编写或修改 skill、AGENTS.md、CLAUDE.md 的指南
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

- 本仓库只收录用户自建技能。obra/superpowers 相关工作不在本仓库，直接以 `E:\AI\superpowers-dsh`（LayneChai/superpowers-dsh，基于上游 v7.0.2）为基础，新需求在其上定制修改。
- 第三方技能的来源和启用状态见 `DSH技能说明.md`。
- `AGENTS.md` 是 DSH 的全局规则文件，定义所有回复使用中文、代码和命令保持原样、简洁优先与精准修改等约定。它与本机 `~/.dsh/AGENTS.md` 内容相同，复制到该位置即可生效。
