# addyosmani/agent-skills 使用说明

## 一、这是什么

[addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)(MIT)是 Addy Osmani 维护的一套工程工作流技能,把资深工程师的流程固化成 25 个技能,覆盖 定义 → 规划 → 构建 → 验证 → 评审 → 发布 全生命周期。

每个技能结构一致:Overview、When to Use、Process(分步流程)、Rationalizations(常见偷懒借口与反驳)、Red Flags、Verification(必须给出证据,如测试通过、构建输出)。核心理念是"流程而非散文"、"验证不可省略"。

## 二、安装位置与触发方式

- 位置:`skills/addyosmani/`(项目工作区),来源与改动见该目录 `NOTICE.md`。
- 触发:**仅手动**。25 个 SKILL.md 均设置 `disable-model-invocation: true`,模型不会自动调用,需要你在对话里写出技能名,例如"使用 spec-driven-development 技能,为……写规格"。
- 启用到 DSH:复制到 DSH 技能目录后,在新会话中生效。

```powershell
# 全部安装(保持 references 与技能目录同级)
Copy-Item -Recurse .\skills\addyosmani\* "$HOME\.dsh\skills\"
```

注意:`references/` 会作为同级目录被复制进 `~/.dsh/skills/references/`,各技能里的 `../../references/` 才能解析。若只装个别技能,请同时复制 `references/`。技能共 25 个,同时放入技能目录会增加目录体积,建议只装常用的。

## 三、技能一览

| 阶段 | 技能 | 何时手动调用 |
|---|---|---|
| 元 | `using-agent-skills` | 不确定该用哪个技能时,它给出选择决策树和通用操作规则 |
| 定义 | `interview-me` | 需求含糊,让 agent 一次一问地追问,直到约 95% 把握 |
| 定义 | `idea-refine` | 只有粗略想法,需要发散/收敛成方案 |
| 定义 | `spec-driven-development` | 新项目/新功能,先写 PRD(目标、命令、结构、风格、测试、边界) |
| 定义 | `constraint-driven-development` | 没有写下质量标准,生成 CONSTRAINTS.md 并防止 agent 静默跳过检查 |
| 规划 | `planning-and-task-breakdown` | 有规格后拆成带验收标准、按依赖排序的小任务 |
| 构建 | `incremental-implementation` | 改动涉及多个文件,按薄切片实现、测试、提交 |
| 构建 | `test-driven-development` | 实现逻辑或修 bug,红-绿-重构 |
| 构建 | `context-engineering` | 会话开始、切换任务或输出质量下降时整理上下文 |
| 构建 | `source-driven-development` | 需要依据官方文档写代码并标注来源 |
| 构建 | `doubt-driven-development` | 高风险或陌生代码,对每个决策做对抗式复核 |
| 构建 | `frontend-ui-engineering` | 组件架构、状态管理、响应式、WCAG 2.1 AA |
| 构建 | `api-and-interface-design` | 契约优先的 API/模块边界设计 |
| 验证 | `browser-testing-with-devtools` | 用 Chrome DevTools MCP 看 DOM、控制台、网络(需该 MCP) |
| 验证 | `debugging-and-error-recovery` | 复现、定位、缩小、修复、防回归 |
| 评审 | `code-review-and-quality` | 合并前五维评审,带严重度标签 |
| 评审 | `code-simplification` | 行为不变的前提下降低复杂度 |
| 评审 | `security-and-hardening` | 处理输入、认证、存储、外部集成 |
| 评审 | `performance-optimization` | 先测量再优化,Core Web Vitals |
| 发布 | `git-workflow-and-versioning` | 小提交、主干开发、版本管理 |
| 发布 | `ci-cd-and-automation` | 搭建或修改流水线与质量门 |
| 发布 | `deprecation-and-migration` | 下线旧系统、迁移用户 |
| 发布 | `documentation-and-adrs` | 记录架构决策(ADR)与 API 文档 |
| 发布 | `observability-and-instrumentation` | 结构化日志、指标、追踪、告警 |
| 发布 | `shipping-and-launch` | 上线前清单、灰度、回滚、监控 |

## 四、推荐的手动流程

上游用 `/spec → /plan → /build → /test → /review → /ship` 串起生命周期。DSH 没有这些斜杠命令,等价做法是按顺序手动点名技能:

1. 需求模糊:`interview-me`,然后 `spec-driven-development`
2. `planning-and-task-breakdown`
3. 每个任务:`incremental-implementation` 配合 `test-driven-development`
4. 出问题:`debugging-and-error-recovery`
5. 合并前:`code-review-and-quality`,按需加 `security-and-hardening`、`performance-optimization`
6. 发布:`shipping-and-launch`

## 五、调用示例

```text
使用 interview-me 技能,我想做一个技能管理工具,但还没想清楚。
使用 spec-driven-development 技能,为"技能启用状态检查"写规格,先别写代码。
使用 code-review-and-quality 技能评审当前未提交的改动。
使用 using-agent-skills 技能,告诉我修这个 flaky 测试该用哪个技能。
```

## 六、与本仓库其他技能的关系

- 与 `mattpocock/grill-me`、`interview-me` 功能相近(追问需求),二选一即可。
- 与 `caveman` 系列互补:caveman 控制输出风格和工作量,agent-skills 规定流程与验证。同时开启时,按需求选一个主导流程,避免两套规则互相冲突。
- 与全局 `AGENTS.md` 的"精准修改、验证优先"一致;上游强调的小提交、每任务提交在你要求提交时才执行,不会自行提交。

## 七、注意事项

- 技能内容为英文,按全局规则 agent 会用中文回复。
- 个别技能依赖外部能力:`browser-testing-with-devtools` 需要 Chrome DevTools MCP;`doubt-driven-development` 的跨模型升级需你授权。
- 附带脚本 `constraint-driven-development/scripts/quality-gate.mjs`、`idea-refine/scripts/idea-refine.sh` 运行前请先阅读。
- 上游已知问题:单独安装单个技能会丢失共享 `references/`(上游 issue #361),本仓库已保留该目录。
- 同步上游:重新克隆后覆盖,并对每个 SKILL.md 重新加上 `disable-model-invocation: true`。
