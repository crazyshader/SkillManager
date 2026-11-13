# i-have-adhd 使用说明(DSH 适配版)

> 来源:https://github.com/ayghri/i-have-adhd ,commit `1ecea1993b7fce257ffbd686c070948b5e07eda6`,MIT 许可证。
> 本地位置:`E:\AI\SkillManager\skills\ayghri\i-have-adhd\SKILL.md`(来源见 `skills\ayghri\NOTICE.md`,许可证见 `skills\ayghri\LICENSE`)。SKILL.md 与上游完全一致,无本地改动。
> **当前状态:已安装到项目工作区,仅手动触发。未复制到 `~/.dsh/skills/`。**

## 一、它是什么

一份纯提示词的输出风格技能,没有脚本,不依赖网络和外部工具。它让智能体的回答"先给动作、步骤编号、结尾只留一个下一步",减少铺垫和绕弯。名字里的 ADHD 只是设计依据,使用不需要诊断。

核心判断:读者的工作记忆小、"知道答案"不等于"去做了"、开始最难、时间感模糊、需要看得见的进展。10 条规则都围绕这五点。

## 二、10 条规则

| 编号 | 规则 | 示例 |
|---|---|---|
| 1 | 第一行是可立即执行的动作 | "Edit `src/auth.ts:42` ..." |
| 2 | 多步任务用编号,每步一个动作,步骤尽量少 | 1. 打开文件 2. 替换函数 3. 跑测试 |
| 3 | 结尾给一个两分钟内能做的下一步 | "Next: 运行 `npm test`,贴出第一条失败信息" |
| 4 | 不扯题外话,第二个问题单独提 | "另外有个过期依赖,要处理吗?" |
| 5 | 每轮重述进度 | "5 步中的第 3 步完成" |
| 6 | 时间估计用具体单位 | "约 15 分钟" 而不是 "一会儿" |
| 7 | 把完成的成果说具体 | "现在可以用魔法链接登录" |
| 8 | 报错只陈述位置、原因、修复 | 不用 "糟糕" |
| 9 | 最终回答里每组列表不超过 5 项 | 只限制展示,不限制分析与检索 |
| 10 | 无开场白、无回顾、无客套结尾 | 禁用 "Great question"、"Hope this helps" |

## 三、持续性与关闭

- 调用后,规则对本会话之后的**每一轮**都生效,换话题也不失效。
- 关闭:对智能体说 `stop adhd mode` 或 `normal mode`,它会用一行确认并恢复默认风格。
- 想在新会话里恢复默认,不再调用即可,它不会跨会话保留。

## 四、规则的例外

| 情形 | 行为 |
|---|---|
| 你要求"解释"或"带我过一遍" | 完整解释,加标题便于回看,仍无开场白和客套结尾 |
| 即将执行破坏性操作(`rm -rf`、强推、迁移、删表) | 先确认,安全优先 |
| 连续三轮"还是不行" | 停止改代码,指出可疑假设,问一个诊断问题 |
| 需求真有歧义 | 问一个简短问题 |
| 规则与任务冲突(例如"有哪些选项") | 给 2 到 4 个排序选项,推荐在前 |
| 规则与运行环境冲突 | 环境的系统提示优先 |

## 五、手动触发的用法

`SKILL.md` 带有 `disable-model-invocation: true`,模型不会自行调用,必须由你点名。

### 方式一:点名技能(启用后)

```
使用 i-have-adhd 技能。
```

之后正常提问即可。

### 方式二:不安装,直接读项目文件(当前即可用)

```
请读取 E:\AI\SkillManager\skills\ayghri\i-have-adhd\SKILL.md,本会话按其规则回答。
```

### 关闭

```
stop adhd mode
```

README 里的 `/i-have-adhd` 是 Claude Code、Qwen Code、Grok 的斜杠命令写法,`$i-have-adhd` 是 Codex 的写法。DSH 是否支持同样的触发,我没有验证,以上面两种写法为准。

## 六、安装与停用

项目工作区已有该技能,不需要再做操作即可用方式二。项目级 `skills/` 目录是否会被 DSH 自动加载我没有验证,与本仓库其他技能的做法一致,以全局目录为准。

### 启用到 DSH 全局技能目录(可选)

```powershell
Copy-Item -Recurse "E:\AI\SkillManager\skills\ayghri\i-have-adhd" "$HOME\.dsh\skills\i-have-adhd" -Force
```

新开会话后生效。

### 停用

```powershell
Remove-Item -Recurse "$HOME\.dsh\skills\i-have-adhd"
```

## 七、注意事项

1. **规则以英文示例为主。** 机制(先动作、编号、限制列表长度)对中文同样有效;示例里的英文禁用语(如 "Great question")对中文不直接适用,效果请自行核对。
2. **与全局规则的关系。** 你的 `AGENTS.md` 已要求中文回复、简洁、不套话,与规则 10 重叠。叠加后主要新增的是:结尾给下一步(3)、每轮重述进度(5)、具体时间估计(6)、列表上限 5 项(9)。
3. **与 GenUI 的关系。** 规则 2 要求编号步骤,在 DSH 里可能与 `steps` 组件并用,不冲突。
4. **会增加每轮 token。** 每轮回答多出"进度"和"下一步"两行。
5. **规则 4 与"主动汇报"可能冲突。** 开源借鉴规则要求在特定情况下停下来询问,规则 4 的"题外话单独提"不会覆盖它,以你的 `AGENTS.md` 为准。
6. **只改输出风格。** 它不改变智能体的推理、工具调用或代码质量。

## 八、更新

上游更新后先看最新提交,再替换:

```powershell
Invoke-WebRequest https://raw.githubusercontent.com/ayghri/i-have-adhd/main/skills/i-have-adhd/SKILL.md -OutFile "E:\AI\SkillManager\skills\ayghri\i-have-adhd\SKILL.md"
```

替换后检查 frontmatter 仍有 `disable-model-invocation: true`,并更新 `NOTICE.md` 和本文件顶部的 commit。若已启用,还要重新复制到 `~/.dsh/skills/i-have-adhd`。

## 九、自定义

要调整规则(例如改成中文示例、把列表上限改成 7),直接编辑 `SKILL.md` 的规则段。改动后在 `NOTICE.md` 的"本地改动"里记录。
