# DSH 技能说明

> 最后更新：2026-11-10
> 更新方式：核对 `E:\AI\SkillManager\skills\` 与 `~/.dsh/skills/` 的文件哈希（两侧一致），并参照本会话最新的技能目录。插件启用状态本次未重新检查。

## 一、用户自建技能（本仓库收录）

| 技能 | 触发方式 | 说明 |
|---|---|---|
| `grill-me` | 仅手动（`/grill-me`） | 动手前逐项追问，直到达成共识。 |
| `handoff` | 仅手动 | 将当前对话压缩为交接文档，保存到系统临时目录。 |
| `reverse-engineer-anything` | 仅手动 | 逆向分析已发布的二进制或包，附带 `references/` 参考文档。原为第三方插件 `rea-bundle` 提供，本地副本已改为手动触发。 |
| `writing-for-agents` | 仅手动（`/writing-for-agents`） | 编写或修改 skill、AGENTS.md、CLAUDE.md。 |

手动触发的技能（设置了 `disable-model-invocation: true`）不会出现在模型的技能目录中，只能由用户输入名称调用。当前所有用户自建技能都已设为手动触发。

## 二、本会话可用的其他技能

| 技能 | 来源 | 说明 |
|---|---|---|
| `office`、`office-docx`、`office-pptx`、`office-xlsx`、`office-export-pdf`、`office-recalculate`、`office-render` | 内置（推断） | Office 文档、表格、演示文稿和 PDF 相关工作流。`office` 对应的插件 `@deepseek-ai/dsh-skill-office` 未在磁盘上找到。 |
| `diagnose-windows-sandbox-acl` | 内置（推断） | Windows 上 DSH 沙箱意外拒绝访问时，检查路径的 ACL 并修复。磁盘上未找到文件。 |
| `genui` | 第三方：`@changfenhuang/dsh-genui` 0.15.0 | 通过 dsh-ui 围栏输出结构化界面。 |

## 三、磁盘上存在但本会话未启用的技能

| 技能 | 来源 | 说明 |
|---|---|---|
| `archify` | 第三方：`@tt-a1i/archify-dsh` 1.0.0 | 插件已禁用。 |
| `openviking-memory`、`openviking-skills`、`ov-experience-memory` | 第三方：`@openviking/dsh-memory-plugin` 0.5.26 | 插件已禁用。 |
| `answer-me-with-html` | 第三方（位于 `~/.dsh/profiles/web/node_modules`） | 文件存在，所属插件未出现在插件列表中。 |

## 四、待确认事项

- 内置技能的来源标注为"推断"。磁盘搜索未进入 DSH 安装目录的 `app.asar` 包内部，需进一步检查才能确认。
- `answer-me-with-html` 的所属插件未在插件列表中出现，其启用状态需单独确认。

## 五、变更记录

| 日期 | 变更 |
|---|---|
| 2026-11-09 | 首次整理。 |
| 2026-11-10 | 移除已合并的 `grilling` 记录；`grill-me`、`handoff`、`reverse-engineer-anything` 改为用户自建并标注手动触发；`reverse-engineer-anything` 改为本地副本；`writing-for-agents` 改为仅手动触发。 |
