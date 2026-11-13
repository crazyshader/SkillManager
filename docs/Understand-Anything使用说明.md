# Understand-Anything 使用说明

来源:https://github.com/Egonex-AI/Understand-Anything(MIT,commit `532a2e9`)。本地副本位于 `skills/Egonex-AI/understand-anything-plugin/`,9 个技能均已设为**仅手动触发**(`disable-model-invocation: true`),模型不会自动调用。

## 一、它是什么

把任意代码库(或 Karpathy 式 LLM wiki、Figma 文件)分析成一张知识图谱,存为 JSON,并提供交互式网页仪表盘浏览。流程分两层:

- **确定性层**:tree-sitter 解析源码,提取 import、export、函数/类、调用关系,同样输入得到同样结果,并用指纹做增量更新。
- **语义层**:LLM 读取解析结果和源码,生成中文/英文摘要、标签、架构分层、业务领域映射、导览路线。

多智能体流水线(由 `/understand` 调度子智能体,定义在 `agents/`):

| 智能体 | 作用 |
|---|---|
| project-scanner | 发现文件,识别语言与框架 |
| file-analyzer | 抽取函数、类、依赖,产出节点和边(最多 5 路并行,每批 20–30 文件) |
| architecture-analyzer | 划分架构层 |
| tour-builder | 生成导览 |
| graph-reviewer | 校验完整性与引用一致性,默认内联,`--review` 才启用完整 LLM 审查 |
| domain-analyzer / article-analyzer | 分别用于 `/understand-domain`、`/understand-knowledge` |

## 二、包含的技能与手动触发命令

| 技能 | 触发 | 用途 |
|---|---|---|
| `understand` | `/understand [path] [--full] [--auto-update] [--review] [--language zh] [--exclude ...]` | 分析代码库,生成 `.ua/knowledge-graph.json` |
| `understand-dashboard` | `/understand-dashboard [project-path]` | 启动网页仪表盘 |
| `understand-chat` | `/understand-chat <问题>` | 基于图谱问答 |
| `understand-explain` | `/understand-explain <文件路径>` | 深入讲解某文件/函数/模块 |
| `understand-diff` | `/understand-diff` | 分析当前改动或 PR 的影响面与风险 |
| `understand-onboard` | `/understand-onboard` | 生成新人上手指南 |
| `understand-domain` | `/understand-domain [--full]` | 提取业务域、流程、步骤 |
| `understand-knowledge` | `/understand-knowledge <wiki目录>` | 分析 LLM wiki,生成带聚类的知识图 |
| `understand-figma` | `/understand-figma <Figma URL 或 key>` | 分析 Figma 文件,需要 Figma API 凭据 |

如果 DSH 没有识别斜杠命令,直接说:"使用 understand 技能分析这个项目"。

## 三、前置条件

- Node.js ≥ 22、pnpm ≥ 10(本机 Node v24.19.0、pnpm 11.22.0,满足)。
- Python 3(`understand-domain`、`understand-knowledge` 的合并/解析脚本使用)。
- `git`(`--auto-update` 与 `understand-diff` 使用)。
- 首次运行 `/understand` 时,技能会在插件目录执行 `pnpm install` 并构建 `@understand-anything/core`(约 6 MB 源码,需联网下载依赖)。`dist` 与 `node_modules` 未随仓库提交,首次构建后才会生成。

## 四、在 DSH 中启用

技能内部靠"插件根目录"找 `packages/`、`agents/`。根目录解析顺序:`CLAUDE_PLUGIN_ROOT`、`DEVIN_PLUGIN_ROOT`、技能目录向上两级、若干 `~/.xxx` 约定路径。复制技能到 DSH 后目录布局会变,所以**必须设置环境变量**,指向本仓库里的插件目录。

1. 设置用户级环境变量(只需一次,设置后重启 DSH):

```powershell
[Environment]::SetEnvironmentVariable("DEVIN_PLUGIN_ROOT", "E:\AI\SkillManager\skills\Egonex-AI\understand-anything-plugin", "User")
```

2. 把 9 个技能目录复制到 DSH 技能目录:

```powershell
Get-ChildItem "E:\AI\SkillManager\skills\Egonex-AI\understand-anything-plugin\skills" -Directory |
  ForEach-Object { Copy-Item -Recurse -Force $_.FullName "$HOME\.dsh\skills\" }
```

3. 新开 DSH 会话。技能不在模型的技能目录里出现,只能用名称手动调用。

注意:技能正文是 bash 写的(`realpath`、`find`、`head` 等),在纯 PowerShell 下可能不可用。推荐在带 Git Bash/WSL 的环境运行,或让 agent 按等价的 PowerShell 命令执行;若在 Windows 上遇到路径解析失败,优先检查 `DEVIN_PLUGIN_ROOT` 是否对当前进程可见。

## 五、典型工作流

```text
/understand --language zh        # 首次全量分析,中文输出,结果写入 <项目>/.ua/
/understand-dashboard            # 打开仪表盘
/understand-chat 支付流程如何走?  # 问答
/understand-explain src/auth/login.ts
/understand-diff                 # 提交前看影响面
/understand                      # 之后重跑为增量,只分析变更文件
```

- `--language` 支持 en(默认)、zh、zh-TW、ja、ko、ru、vi;选择会保存到 `.ua/config.json`。
- 大型 monorepo 用 `/understand src/frontend` 限定子目录。
- `.ua/` 目录(若项目已有 `.understand-anything/` 则沿用它)。

## 六、成本与风险

- **token 消耗**:首次 `/understand` 会让子智能体读取整个项目,大项目消耗显著;建议先用 `path` 限定范围,或使用本地模型。增量运行便宜得多。
- **子智能体**:流水线依赖 DSH 的子智能体能力来跑 `agents/*.md`。如果当前会话不能派发子智能体,`/understand` 无法完整执行。
- **数据位置**:图谱和中间文件写在被分析项目内的 `.ua/`。建议在项目 `.gitignore` 加入 `.ua/intermediate/` 和 `.ua/diff-overlay.json`;想与团队共享图谱就提交其余文件。
- **`--auto-update`**:会在被分析项目安装 post-commit 钩子,属于对该项目的持久改动,需要时再用。
- **仪表盘**:本地启动 Vite 开发服务,监听 127.0.0.1,带 token;不会上传数据。
- **Figma**:`understand-figma` 需要 Figma API token,不要把 token 贴进对话,按技能说明用环境变量提供。

## 七、更新与卸载

- 更新:重新克隆上游,覆盖 `skills/Egonex-AI/understand-anything-plugin/`,并重新给 9 个 `SKILL.md` 加 `disable-model-invocation: true`,再复制到 `~/.dsh/skills/`。
- 卸载:删除 `~/.dsh/skills/understand*` 与 `DEVIN_PLUGIN_ROOT` 环境变量;被分析项目里的 `.ua/` 可按需删除。
