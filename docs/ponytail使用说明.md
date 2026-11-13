# Ponytail 使用说明（DSH 适配版）

> 来源：https://github.com/DietrichGebert/ponytail，commit `96608e078b2d5ea607c5c3b8b5a54d1c1fba6846`，MIT 许可证。
> 本地位置：`E:\AI\SkillManager\skills\ponytail\`（来源与改动见同目录 `NOTICE.md`）。

## 一、它是什么

Ponytail 是一组提示词技能，让智能体按"最少代码"的原则做事：动手前先读相关代码，能复用就不新写，能用标准库或平台功能就不引入依赖，最后才写最少的代码。信任边界上的输入校验、防数据丢失的错误处理、安全和无障碍访问不会被省略。高风险逻辑（分支、循环、解析器、涉及金钱或安全）会留下一个小测试。

它不管理上下文，也不会压缩历史。对长上下文编码的帮助是间接的：少写代码，上下文涨得慢；先复用，减少重复造轮子。

## 二、包含的技能

| 技能 | 用途 | 是否改代码 |
|---|---|---|
| `ponytail` | 编码时的精简实现模式。 | 是，写代码时生效 |
| `ponytail-review` | 审查一次改动（未提交、已暂存、分支、PR 或指定文件）。 | 否，只出报告 |
| `ponytail-audit` | 审计整个仓库或指定目录，按优先级排序。 | 否，只出报告 |
| `ponytail-debt` | 把代码里的 `shortcut:` 注释汇总成债务清单。 | 否，只出报告 |

四个技能都设置了 `disable-model-invocation: true`，模型不会自动调用，只能由你手动触发。

## 三、安装与启用

1. 本目录下的技能目前只在项目里。要在其他项目也能用，把四个子目录复制到 `~/.dsh/skills/`（和你现有技能的同步方式一致）：

```powershell
foreach($n in 'ponytail','ponytail-review','ponytail-audit','ponytail-debt'){
  Copy-Item "E:\AI\SkillManager\skills\ponytail\$n" "$HOME\.dsh\skills\$n" -Recurse -Force
}
```

2. 新开会话后生效。我没有验证过项目级 `skills/` 目录是否会被自动加载，所以全局目录是更稳妥的路径。
3. 手动触发的方式与你现有的 `grill-me`、`writing-for-agents` 相同：在消息里写技能名，例如 `/ponytail`。如果输入框没有补全，直接在消息里说"使用 ponytail 技能"。

## 四、ponytail：精简实现模式

### 调用

```
/ponytail
/ponytail lite
/ponytail ultra
```

也可以直接说："用 ponytail 实现这个功能"。

### 三个级别

| 级别 | 行为 | 适用场景 |
|---|---|---|
| `lite` | 按要求做，并用一行话指出更小的方案，由你选。 | 你想保留决定权。 |
| `full`（默认） | 先问这个东西需不需要存在，再查仓库、标准库、平台、已装依赖，最后才写最少的代码。 | 日常开发。 |
| `ultra` | 在 full 的基础上质疑需求，对不合理的部分先反驳再动手。 | 需求模糊或可能过度设计。 |

不指定级别时用 `full`。

### 工作顺序

1. 读任务和它涉及的代码。列出改动要影响的所有位置：调用方、测试、夹具、配置、导出。
2. 按以下顺序取第一个可行的方案：
   1. 这东西需要存在吗？
   2. 代码库里已经有吗？
   3. 标准库或平台功能能做吗？
   4. 已安装的依赖能做吗？
   5. 能写成一行清晰的代码吗？
   6. 否则写最少的可运行代码。
3. 修 bug 时，先 grep 所有调用方，在共享代码里一次性修根因。
4. 回复结尾用一两行说明：跳过了什么、没检查什么、有什么风险。

### 注意事项

- 在 DSH 里没有 hooks，所以它不会"整个会话自动生效"，级别也不会跨会话保存。每个新会话需要重新调用。
- 如果你要的是完整实现（例如完整的错误分类、可扩展接口），在消息里明说，或者不调用它。
- 想退出，说"stop ponytail"或"正常模式"。这依赖上下文，不是硬开关。

## 五、ponytail-review：审查改动

### 调用

```
/ponytail-review
/ponytail-review 已暂存的改动
/ponytail-review 当前分支相对 main 的改动
/ponytail-review https://github.com/OWNER/REPO/pull/123
/ponytail-review src/billing/close_day.py
```

不指定目标时，审查未提交的改动；没有未提交改动时审查最后一次提交。

### 它检查什么

按重要性排序：正确性、安全、真实负载下的表现、缺失的测试、速度、可删减的代码。它不只看 diff，还会读被改动代码的调用方和相关测试。

### 输出格式

1. `What this change does:` 两三句话概述。
2. 分三组列出发现：**Must fix**、**Should fix**、**Nice to have**。发现编号贯穿全部分组，你可以说"修 2 和 5"。
3. 每条发现包含四部分：这段代码做什么、问题、修复办法、不修的后果，并附文件和行号。
4. 结尾给出 `Verdict`、可删减的行数（`Lean`）和未检查的部分（`Not checked`）。

输出语言已改为跟随你的语言，措辞保持简短。

### 限制

- 只出报告，不改代码。要修复，在报告之后说"修 1 和 3"。
- 每条发现必须有具体的触发场景，没有就不列。它不提风格偏好。

## 六、ponytail-audit：审计仓库

### 调用

```
/ponytail-audit
/ponytail-audit src/api
```

不指定范围时审计整个仓库。

### 流程

1. 先读 README、部署和构建配置、依赖列表、入口点和测试，估计预期负载。
2. 追踪主要数据流，重点读用户输入、金额、认证、数据写入、后台任务、进程间共享状态。
3. 大仓库会把精力放在出错代价最高的部分，并说明哪些没读。

### 输出

和 review 相同的三组结构，最多 20 条，未列出的会说明数量。结尾给出 `Verdict`、可删减的行数和依赖数量，以及 `Not checked`。

### 用法建议

- 接手陌生仓库时先跑一次，得到优先级列表。
- 大仓库建议限定目录，一次审计一个模块，输出更可控。
- 审计会读很多文件，在已经很长的会话里调用会占用上下文。更合适的做法是新开会话运行，把结论保存到文件里。

## 七、ponytail-debt：捷径债务清单

### 调用

```
/ponytail-debt
/ponytail-debt TODO
```

### 行为

- 用下面的命令在仓库里查找 `shortcut:`（以及旧的 `ponytail:`）注释，跳过被 `.gitignore` 忽略的文件：

```
git grep --no-index --exclude-standard -nE '(#|//|/[*]) ?(shortcut|ponytail):'
```

- 带参数时改搜你指定的标记词，例如 `TODO`。
- 每条记录输出：文件和行号、简化了什么、上限、升级触发条件。没有写触发条件的会标记 `no-trigger`。
- 结尾给出标记总数和缺少触发条件的数量。
- 需要保存时，说"写入 `PONYTAIL-DEBT.md`"。默认不写文件。

### 前提

只有代码里存在 `shortcut: <上限>, <升级时机>` 格式的注释时才有内容。`ponytail` 技能在写入有已知上限的捷径时会加这种注释。

## 八、推荐工作流

1. **日常开发**：需要精简实现时调用 `/ponytail`，完成后用 `/ponytail-review` 审查本次改动。
2. **接手仓库**：新会话中运行 `/ponytail-audit`，把结果保存成文件，再逐条处理。
3. **定期清理**：运行 `/ponytail-debt`，检查是否有捷径已经到了该升级的时候。
4. **验证效果**：同一个任务分别在有和没有 ponytail 的情况下各做一次，对比代码行数、测试和你是否满意。作者的基准数据来自 Claude Code 加 Opus 5.5，不代表你的环境。

## 九、与你现有规则的关系

- 你的 `AGENTS.md` 里"简洁优先"和"精准修改"与 ponytail 的核心规则大半重叠。ponytail 的额外价值在于"先复用已有实现"的检查顺序，以及 review 和 audit 的报告结构。
- `AGENTS.md` 里的"开源借鉴规则"（实现新模块前先调研开源项目）与 ponytail 的"先问需不需要存在"可能同时触发。冲突时以 `AGENTS.md` 为准，这是更具体的项目规则。
- 如果长期觉得 ponytail 有用，可以把核心规则提炼进 `AGENTS.md`，替代手动调用。

## 十、未搬运的内容

| 内容 | 原因 |
|---|---|
| `ponytail-gain` | 只展示作者的基准数据，没有实际功能。 |
| `ponytail-help` | 内容是 Claude Code 的信息栏、`/plugin` 和配置文件说明，在 DSH 里不适用。 |
| hooks、信息栏、Board、`/ponytail band` | 依赖 Claude Code 2.1.290 或更高版本，DSH 没有对应机制。 |
| `PONYTAIL_DEFAULT_MODE` 和 `~/.config/ponytail/config.json` | 由 hooks 读取，没有 hooks 就不起作用。 |

## 十一、本地改动清单

1. 四个 `SKILL.md` 的 frontmatter 增加 `disable-model-invocation: true`。
2. `ponytail-review` 和 `ponytail-audit`：输出语言从 "Very simple English" 改为跟随用户语言。
3. `ponytail`：级别切换说明改为在消息或技能参数中指定。

其余内容与上游一致。上游更新后需要手动比对并重新同步。
