# DSH 技能说明

> 最后更新：2026-11-11
> 更新方式：核对 `E:\AI\SkillManager\skills\` 与 `~/.dsh/skills/` 的文件哈希（两侧一致），并参照本会话最新的技能目录。插件启用状态本次未重新检查。

## 一、用户自建技能（本仓库收录）

| 技能 | 触发方式 | 说明 |
|---|---|---|
| `code-review`（`mattpocock/`） | 仅手动 | 审查某个起点以来的改动，并行对照编码规范和 spec，输出两份报告。 |
| `diagnosing-bugs`（`mattpocock/`） | 仅手动 | 排查难复现的 bug 或性能退化：先建立检查命令，再缩小范围、验证假设、修复。 |
| `research`（`mattpocock/`） | 仅手动 | 用一手资料调研问题，结论连同来源写入仓库的 Markdown 文件。 |
| `retro`（`mattpocock/`） | 模型可自动调用 | 复盘一次编码会话，提出改进导航、自动检查、编码规范、AGENTS.md 和工具调用成本的建议。 |
| `grill-me`（`mattpocock/`） | 仅手动（`/grill-me`） | 动手前逐项追问，直到达成共识。 |
| `handoff`（`mattpocock/`） | 仅手动 | 将当前对话压缩为交接文档，保存到系统临时目录。 |
| `reverse-engineer-anything` | 仅手动 | 逆向分析已发布的二进制或包，附带 `references/` 参考文档。原为第三方插件 `rea-bundle` 提供，本地副本已改为手动触发。 |
| `writing-for-agents`（`mattpocock/`） | 仅手动（`/writing-for-agents`） | 编写或修改 skill、AGENTS.md、CLAUDE.md。 |

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
| 2026-11-11 | 从 obra/superpowers 复制 4 个技能到 `skills/superpowers/`，设为手动触发，去掉正文中的 `superpowers:` 前缀，并新增 `SUPERPOWERS-NOTICE.md`。 |
| 2026-11-11 | 六个 mattpocock 相关技能（含 `grill-me`、`handoff`、`writing-for-agents`）移入 `skills/mattpocock/`。 |
| 2026-11-10 | 新增 `code-review`、`diagnosing-bugs`、`research`、`retro`；移除已合并的 `grilling` 记录；`grill-me`、`handoff`、`reverse-engineer-anything` 改为用户自建并标注手动触发；`reverse-engineer-anything` 改为本地副本；`writing-for-agents` 改为仅手动触发。 |
| 2026-11-11 | 删除 `skills/superpowers/`（4 个 obra/superpowers 技能、`SUPERPOWERS-NOTICE.md` 与 `README.md`）。superpowers 相关工作改为直接使用 `E:\AI\superpowers-dsh`（LayneChai/superpowers-dsh，基于上游 v7.0.2）。 |
