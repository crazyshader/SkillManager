# Caveman 使用说明（DSH 适配版）

> 来源：https://github.com/JuliusBrussee/caveman，commit `3dc497a7f858c774d6e15b25e8c6120ef3a0eda8`，Apache-2.0 许可证。
> 本地位置：`E:\AI\SkillManager\skills\caveman\`（来源与改动见同目录 `NOTICE.md`，上游 NOTICE 见 `UPSTREAM-NOTICE`）。

## 一、它是什么

上游 Caveman 由三部分组成：

1. **技能（skill）**：一组提示词，让智能体回答更简短，并附带几个减少过度编码的工作流技能。
2. **CLI 与代理层**（`@caveman-ai/cli`）：压缩工具输出（Read less）、模型路由（Auto）、脚本复用（Blocks）、网页压缩（Browse）、遥测等。
3. **Caveman Cloud**：面向你自己开发的智能体的观测和评测服务。

**本次只安装了第 1 部分的 11 个技能。** 第 2、3 部分需要 Node.js CLI、hooks 和代理，DSH 没有对应机制，且 Auto 会把你的提问和上下文发给 Caveman Cloud，所以没有安装。技能本身只是 Markdown，在本机运行，不发送任何数据。

## 二、已安装的技能

全部设置了 `disable-model-invocation: true`，模型不会自动调用，只能由你手动触发。

### 输出风格类

| 技能 | 作用 | 触发词 |
|---|---|---|
| `caveman` | 标准简短风格：先给结论，去掉客套话，技术内容完整保留。 | `/caveman` |
| `ultracave` | 极限压缩：只用片段，能一个词就一个词，每个事实只说一次。 | `/ultracave` |
| `megacave` | 文言文风格，字数更少，技术术语保持原文。 | `/megacave` |
| `caveman-commit` | 生成 Conventional Commits 提交信息，只输出文本，不执行 `git commit`。 | `/caveman-commit` |
| `caveman-review` | 代码审查，每个发现一行，格式 `L42: 🔴 bug: ...`。只出评论，不改代码。 | `/caveman-review` |

### 工作流类

| 技能 | 作用 |
|---|---|
| `investigate-first` | 先取证再动手：区分症状与推断原因，按证据给假设排序，找到原因前不改代码。 |
| `lean-build` | 做新功能时控制范围：先定验收条件和非目标，复用现有结构，不加多余模式和配置。 |
| `surgical-patch` | 修 bug 时在最窄的责任层修改，先复现，只加相关回归测试。 |
| `safe-refactor` | 重构前先确定行为保持边界和验证手段，一次只移动一个所有权边界。 |
| `migration` | 迁移（表结构、数据、API、依赖）时定义前进和回滚路径，破坏性步骤单独授权。 |
| `verify-and-stop` | 只验证已有工作是否满足验收条件，通过后立即停止，不做额外清理。 |

## 三、启用

1. 技能目前只在本仓库的 `skills\caveman\` 下，DSH 不会自动加载。要在所有项目使用，复制到全局技能目录：

```powershell
foreach($n in 'caveman','ultracave','megacave','caveman-commit','caveman-review','investigate-first','lean-build','surgical-patch','safe-refactor','migration','verify-and-stop'){
  Copy-Item "E:\AI\SkillManager\skills\caveman\$n" "$HOME\.dsh\skills\$n" -Recurse -Force
}
```

2. 新开会话后生效。我没有验证过项目级 `skills/` 目录是否被自动加载，全局目录更稳妥。
3. 手动触发方式与 `grill-me` 等技能相同：在消息里写 `/caveman`，没有补全就说"使用 caveman 技能"。

## 四、caveman：标准简短风格

### 开关

```
/caveman              开启，持续到整个会话结束
/caveman ultra        等同 /ultracave
/caveman wenyan       等同 /megacave
/caveman status       报告当前模式，不改变状态
stop caveman          关闭
normal mode           关闭
```

### 规则要点

1. **先给答案**：格式为"东西 + 动作 + 原因。下一步。"，不加问候和复述。
2. **去掉客套**：不说 "Sure!"、"Let me"、"Hope this helps"，也不用 just/really/basically 这类填充词。
3. **用短词**：用 "fix" 不用 "implement a solution for"；不自造缩写。
4. **冠词可省，意思不能省**：`not`、`never`、`no`、`only`、`except` 和数字、单位永远保留。
5. **一句一个意思**：每句最多 20 词，主动语态，一个东西只用一个术语。简短和清楚冲突时，清楚优先。
6. **原样保留载荷**：代码块、命令、路径、API 名称不改；报错只引用最关键的一行；展示改动时只给变更行加 1–2 行上下文，不贴整个文件；已有注释不删不缩。
7. **工具调用期间**：例行调用之间不输出文字；多步任务开始时一句话，阶段变化时一句话，结束时一句话汇报结果。
8. **语言**：只压缩风格，不换语言。你的全局规则要求中文，所以回复仍是中文，只是更短。
9. **不表演**：不说"我现在是原始人模式"，不加前缀。

### 自动恢复完整句子的场景

出现以下情况时会暂时用完整句子，之后再恢复简短：

1. 安全警告。
2. 不可逆操作，先用完整句子确认。
3. 片段可能让步骤顺序产生歧义。
4. 你表示困惑或重复提问。
5. 写进聊天以外的内容：代码、注释、提交信息、文档、issue、PR、记忆文件。
6. 智能体需要向你提问或提供选项。

### 发送前自检

智能体会检查：第一句是不是在预告要做什么；最后一句是不是复述或提供帮助；否定词、代码、路径、数字是否原样保留；有没有句子存在两种读法。

### 在 DSH 里的限制

- 没有 hooks，所以不会整个会话自动生效，也不会跨会话保存。每个新会话需要重新调用。
- `/caveman status` 只能报告智能体自己记得的模式，会附上 `(not tracked by this host)`，不是真实状态。
- "stop caveman" 靠上下文理解，不是硬开关。

## 五、ultracave 与 megacave

**ultracave**

- 去掉冠词、系动词和连接词，能用一个词就用一个词，每个事实只说一次。
- 底线与 caveman 相同：代码、命令、路径、否定词、数字、你的语言不变。
- 出现安全警告、不可逆操作、两种读法或你困惑时，回到完整句子。
- 示例：`Inline object prop, new ref, re-render. useMemo.`

**megacave**

- 用文言文回答，主语能恢复就省略，用"之、乃、为、其、则、故、以"表示结构。
- 代码、命令、路径、报错保持原文；否定词（不、非、勿、未、毋）和阿拉伯数字不变；文言有歧义时改用白话。
- 上游注明 token 节省很小且不稳定，不要指望它省钱，主要是屏幕字数少。
- 在你的中文环境里，它和 caveman 差别不大，可以当作风格选项。

## 六、caveman-commit：生成提交信息

```
/caveman-commit
/caveman-commit 暂存区的改动
```

- 格式：`<type>(<scope>): <祈使句摘要>`。type 取 `feat`、`fix`、`refactor`、`perf`、`docs`、`test`、`chore`、`build`、`ci`、`style`、`revert`。
- 标题尽量不超过 50 字符，上限 72，不加句号。
- 只有存在不明显的原因、破坏性变更、迁移说明或关联 issue 时才写正文，正文每行不超过 72 字符，要点用 `-`。
- 破坏性变更、安全修复、数据迁移和回滚提交必须写正文。
- 不写 "This commit does X"，不加 AI 署名，除非你自己的规则要求 `Assisted-by` 之类的 trailer。
- **只输出文本**，不会暂存文件、执行提交或修改历史。
- **注意与你的全局规则的关系**：`AGENTS.md` 要求提交信息保持原样、不翻译，所以该技能输出的是英文提交信息。需要中文提交信息时，在消息里明说。

## 七、caveman-review：一行一个发现

```
/caveman-review
/caveman-review 当前分支相对 main 的改动
```

- 格式：`L<行号>: <问题>。<修复>。`，多文件时用 `<文件>:L<行号>: ...`。
- 严重程度前缀（混合时使用）：

| 前缀 | 含义 |
|---|---|
| `🔴 bug:` | 行为错误，会引发事故 |
| `🟡 risk:` | 能运行但脆弱（竞态、缺少空值检查、吞掉错误） |
| `🔵 nit:` | 风格、命名，作者可忽略 |
| `❓ q:` | 真正的疑问，不是建议 |

- 必须有准确的行号、反引号包起来的符号名和具体修复方法。
- 安全问题、架构分歧和新手作者的场景会改用完整段落。
- **只出评论**，不写修复代码，不批准或要求修改，不运行 linter。
- 与 `ponytail-review`、`code-review`（mattpocock）的区别：它最短，适合快速扫描一次改动；要对照 spec 或做深度审查，用 `code-review`。

## 八、工作流技能

这六个技能都很短（每个约十几行），可以直接读 `skills\caveman\<名称>\SKILL.md`。它们的共同点是：限定范围、先取证、以证据停止。

| 场景 | 用哪个 | 一句话用法 |
|---|---|---|
| 原因不明的故障、间歇性问题、性能退化 | `investigate-first` | "用 investigate-first 排查 X，不要改代码"。产出是原因和证据。 |
| 新功能，担心做过头 | `lean-build` | 先写出验收条件和非目标；跑通路径后停止。 |
| 已知 bug 或小行为修改 | `surgical-patch` | 先复现，再在最窄的层修，跑聚焦测试和最近的检查。 |
| 提取、合并、移动代码 | `safe-refactor` | 动手前先确定验证手段，改完用同样的验证。 |
| 数据库、API、依赖、配置迁移 | `migration` | 先给出回滚路径；每次只做请求的阶段，不隐式执行收尾的破坏性步骤。 |
| 只想确认工作是否完成 | `verify-and-stop` | 汇报命令、结果和剩余风险，通过后不再做任何额外修改。 |

注意：

- `lean-build` 的正文提到 "Native Core"，这是上游的一份内部规则文件（`skills/native/native-core.md`），本次没有搬运。该技能单独使用时仍然可用，只是缺少那份背景规则。
- 它们与你的 `AGENTS.md`（简洁优先、精准修改、先写成功标准再动手）高度重叠。已有 `diagnosing-bugs`、`ponytail` 的情况下，可以不全部启用，选用顺手的即可。

## 九、推荐用法

1. **日常对话**：觉得回复太啰嗦时，说 `/caveman`，用完说 `stop caveman`。
2. **长日志、长输出的场景**：技能只压缩智能体的输出，不压缩工具输出。读大日志时可以让它先 `Select-String` 或过滤关键行再读，这是技能解决不了的部分。
3. **提交信息**：改完后说 `/caveman-commit`，把结果自己提交。
4. **快速审查**：提交前用 `/caveman-review` 扫一遍。
5. **做大事时**：`investigate-first` → `surgical-patch` 或 `lean-build` → `verify-and-stop`，按任务类型串联。

## 十、效果预期

- 上游自己的数据：在简单的"请简洁回答"指令之上，`/caveman` 额外节省约 3%，`/ultracave` 额外节省约 35%（10 个开发问题，Opus 4.5 级别模型，基于旧版规则，未重新测试）。
- JetBrains 的 86 个编码任务测试：输出 token 少 8.5%，质量无可测量的下降。编码会话大部分是代码和工具调用，风格压缩对它们影响很小。
- 技能规则本身约占 1,160 token 输入，且仅在调用时才加载。如果按请求计费（例如 GitHub Copilot），节省为零。
- 所以对你主要的收益是**减少阅读负担**，而不是省钱。省钱要靠压缩工具输出，而那部分没有安装。

## 十一、未搬运的内容

| 内容 | 原因 |
|---|---|
| CLI（`@caveman-ai/cli`）、`install.sh`、`install.ps1` | 会修改本机所有智能体的配置并包含遥测，DSH 不需要。 |
| Read less、Auto、Blocks、Browse、Waste fixes | 依赖 CLI 和代理；Auto 会向 Caveman Cloud 发送提问和上下文。 |
| `cavecrew` | 依赖 Claude Code 的子代理定义。 |
| `caveman-compress` | 是 Python 脚本，会把待压缩文件发给模型提供方；也可能改写 `AGENTS.md`，需要时再单独评估。 |
| `caveman-stats`、`caveman-help`、`caveman-setup`、`caveman-discover`、`caveman-learn`、`caveman-manage`、`caveman-optimize`、`caveman-explore`、`caveman-evidence-review` | 驱动 caveman CLI 或读取 Claude Code 的会话记录。 |
| hooks、Claude Code 状态栏徽章 | DSH 没有对应机制。 |
| 各技能的 `README.md`、`agents/openai.yaml`、`generated/`、`native/` | 只用于其他智能体的打包，运行时不用。 |

## 十二、本地改动与维护

1. 11 个 `SKILL.md` 的 frontmatter 均设置 `disable-model-invocation: true`（`ultracave`、`megacave` 上游已有），正文与上游一致。
2. 复制了上游 `LICENSE` 和 `NOTICE`（后者命名为 `UPSTREAM-NOTICE`），并新增 `NOTICE.md` 记录来源。
3. 上游更新后需要手动比对并重新同步；上游技能由 `skills/compile.mjs` 从模板生成，同步时注意以 `skills/<名称>/SKILL.md` 为准。
4. 商标：上游声明 "Caveman" 和石头徽标是 Julius Brussee 的商标。本仓库只做个人使用，若对外分发，请参考上游的 `TRADEMARKS.md`。
