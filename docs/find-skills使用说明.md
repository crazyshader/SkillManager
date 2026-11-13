# find-skills 使用说明(DSH 适配版)

> 来源:https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md,commit `773fb2c7bbf16781670a3520affc4abd0c6151ae`,MIT 许可证。
> 本地位置:`E:\AI\SkillManager\skills\vercel-labs\find-skills\`(来源见 `skills\vercel-labs\NOTICE.md`,许可证见 `skills\vercel-labs\LICENSE`)。SKILL.md 仅增加了 `disable-model-invocation: true`,其余原样。

## 一、它是什么

`find-skills` 是一份纯提示词技能,没有脚本。它教智能体:当你问"怎么做 X""有没有做 X 的技能"时,去开放技能生态(https://skills.sh/)里查找、核实并推荐可安装的技能,经你同意后再安装。

它依赖外部命令行工具 Skills CLI(`npx skills`),需要 Node.js。本机已有 Node v24.19.0、npx 11.17.0。

## 二、技能的执行流程

| 步骤 | 内容 |
|---|---|
| 1. 理解需求 | 确定领域(React、测试、部署等)和具体任务,判断是否常见到可能已有技能。 |
| 2. 先看排行榜 | 查 https://skills.sh/ 的安装量排行,看是否已有知名技能。 |
| 3. 搜索 | 运行 `npx skills find [关键词] [--owner <owner>]`。 |
| 4. 核实质量 | 安装量建议 1K 以上,低于 100 要谨慎;优先官方来源(`vercel-labs`、`anthropics`、`microsoft`);来源仓库 star 少于 100 要持怀疑。 |
| 5. 推荐 | 向你给出技能名、功能、安装量、来源、安装命令和 skills.sh 链接。 |
| 6. 安装 | 你同意后运行 `npx skills add <owner/repo@skill> -g -y`。 |
| 无结果时 | 说明没找到,改为直接帮你完成任务,并可建议 `npx skills init <名称>` 自建技能。 |

### Skills CLI 常用命令

| 命令 | 作用 |
|---|---|
| `npx skills find [query] [--owner <owner>]` | 交互式或按关键词搜索技能。 |
| `npx skills add <package>` | 从 GitHub 等来源安装技能。 |
| `npx skills update` | 更新已安装的技能。 |
| `npx skills init <名称>` | 生成新技能的模板。 |

`add` 的 `-g` 表示安装到用户级(全局),`-y` 表示跳过确认。

## 三、在本项目中安装到 DSH

项目内已收录,要让 DSH 加载,复制到全局技能目录:

```powershell
Copy-Item -Recurse "E:\AI\SkillManager\skills\vercel-labs\find-skills" "$HOME\.dsh\skills\find-skills" -Force
```

新开会话后生效。我没有验证过项目级 `skills/` 目录会被自动加载,全局目录更稳妥(与 ponytail 说明一致)。

## 四、使用方法

在会话里直接用自然语言触发,例如:

- "有没有帮我做 PR 审查的技能?"
- "find a skill for changelog"
- "我想让你更擅长 React 性能优化,能找个技能吗?"
- 或明确说"使用 find-skills 技能,帮我找 playwright 测试相关的技能"。

智能体会搜索、核实并给出推荐,等你确认后才安装。本地副本已在 frontmatter 加入 `disable-model-invocation: true`(与本仓库其他技能一致),模型不会自动调用,只能由你手动触发,需在消息里写明技能名,例如 `/find-skills` 或"使用 find-skills 技能"。

## 五、注意事项(适配 DSH 时的风险)

1. **安装位置不一定是 DSH 的目录。** `npx skills add` 面向 Claude Code、Cursor 等多种智能体,会把技能写入它检测到的智能体目录。我没有验证它是否支持 DSH。安装后请检查 `~/.dsh/skills/`;若技能没出现,手动把下载的技能目录复制进去。
2. **`-g -y` 会跳过确认并写入用户级目录。** 建议让智能体先给出推荐,你确认后再装,不要让它直接用 `-y` 批量安装。
3. **第三方技能是提示词,等同于给智能体加指令。** 安装前阅读 SKILL.md,确认没有可疑命令或网络调用。本技能自带的质量门槛(安装量、来源、star)只能降低风险,不能替代你的审查。
4. **依赖网络与外部服务。** 需要访问 skills.sh、GitHub 和 npm 源;离线时无法使用。
5. **`npx` 会下载并运行远程包。** 首次执行会拉取 `skills` 包,请确认处于可信网络环境。
6. 本仓库规则要求第三方技能记录来源与许可证。你从生态里装入本仓库的任何新技能,也请在对应目录的 `NOTICE.md` 登记。

## 六、更新

上游更新后,对比最新提交再替换:

```powershell
Invoke-WebRequest https://raw.githubusercontent.com/vercel-labs/skills/main/skills/find-skills/SKILL.md -OutFile "E:\AI\SkillManager\skills\vercel-labs\find-skills\SKILL.md"
```

替换后需重新加上 `disable-model-invocation: true`。同时更新 `NOTICE.md` 和本文件顶部的 commit。
