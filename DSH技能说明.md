# DSH 技能说明

> 最后更新：2026-11-09
> 更新方式：重新检查 `~/.dsh` 下的 SKILL.md、插件列表（`plugin_manager`）和会话技能目录后，修改下表。

## 一、本会话可用技能（共 11 个）

| 技能 | 来源 | 是否开启 | 说明 |
|---|---|---|---|
| `office` | 内置（推断） | 是 | 办公文档总入口。磁盘上未找到文件，对应的 `@deepseek-ai/dsh-skill-office` 插件未安装。 |
| `office-docx` | 内置（推断） | 是 | 创建、读取、编辑、校验 Word 文档（.docx）。 |
| `office-pptx` | 内置（推断） | 是 | 创建、读取、编辑、校验 PowerPoint 演示文稿（.pptx）。 |
| `office-xlsx` | 内置（推断） | 是 | 读取、创建、编辑 Excel 工作簿、数据和公式（.xlsx）。 |
| `office-export-pdf` | 内置（推断） | 是 | 用 LibreOffice Kit 把 Office 文档转为 PDF。 |
| `office-recalculate` | 内置（推断） | 是 | 刷新表格公式缓存，并验证计算结果。 |
| `office-render` | 内置（推断） | 是 | 用 LibreOffice Kit 渲染文档为图片，检查版面。 |
| `diagnose-windows-sandbox-acl` | 内置（推断） | 是 | Windows 上 DSH 沙箱意外拒绝访问时，检查路径及祖先目录的 ACL 并修复。磁盘上未找到文件。 |
| `genui` | 第三方：`@changfenhuang/dsh-genui` 0.15.0 | 是 | 通过 dsh-ui 围栏输出结构化界面。插件已启用。 |
| `reverse-engineer-anything` | 第三方：`rea-bundle`（REA） | 是 | 逆向分析原生、托管、Electron/JS、固件、浏览器等目标。文件位于 `~/.dsh/plugins/rea-bundle`。 |
| `grilling` | 用户自建 | 是 | 针对计划或决策进行追问。文件位于 `~/.dsh/skills/grilling`。 |
| `writing-for-agents` | 用户自建 | 是 | 编写或修改 skill、AGENTS.md、CLAUDE.md。文件位于 `~/.dsh/skills/writing-for-agents`。 |

## 二、磁盘上存在但本会话未启用的技能

| 技能 | 来源 | 是否开启 | 说明 |
|---|---|---|---|
| `grill-me` | 用户自建 | 否 | 文件位于 `~/.dsh/skills/grill-me`，不在本会话目录中。 |
| `handoff` | 用户自建 | 否 | 文件位于 `~/.dsh/skills/handoff`，不在本会话目录中。 |
| `archify` | 第三方：`@tt-a1i/archify-dsh` 1.0.0 | 否 | 插件已禁用。 |
| `openviking-memory`、`openviking-skills`、`ov-experience-memory` | 第三方：`@openviking/dsh-memory-plugin` 0.5.26 | 否 | 插件已禁用。 |
| `answer-me-with-html` | 第三方（位于 `~/.dsh/profiles/web/node_modules`） | 否 | 文件存在，不在本会话目录中；所属插件未出现在插件列表中。 |

## 三、待确认事项

- 内置技能的来源标注为"推断"。磁盘搜索未进入 DSH 安装目录的 `app.asar` 包内部，需进一步检查才能确认。
- `answer-me-with-html` 的所属插件未在插件列表中出现，其启用状态需单独确认。

## 四、变更记录

| 日期 | 变更 |
|---|---|
| 2026-11-09 | 首次整理，共记录 11 个会话可用技能和 5 组磁盘上未启用的技能。 |
