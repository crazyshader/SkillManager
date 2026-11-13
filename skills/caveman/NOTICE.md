# 来源说明

- 来源:https://github.com/JuliusBrussee/caveman,commit `3dc497a7f858c774d6e15b25e8c6120ef3a0eda8`(浅克隆,检出日期 2026-11-12),许可证 Apache-2.0(见同目录 LICENSE)
- 范围:caveman、ultracave、megacave、caveman-commit、caveman-review、investigate-first、lean-build、surgical-patch、safe-refactor、migration、verify-and-stop 共 11 个技能的 SKILL.md
- 本地改动:11 个 SKILL.md 的 frontmatter 均设置 `disable-model-invocation: true`,其余原样复制。
- 未搬运:cavecrew(依赖 Claude Code 子代理定义)、caveman-compress(Python 脚本,会把文件内容发给模型提供方)、caveman-stats/help/setup/discover/learn/manage/optimize/explore/evidence-review(驱动 caveman CLI 和代理)、各技能的 README.md 与 agents/openai.yaml、generated/、native/、CLI(`@caveman-ai/cli`)、hooks、代理层(Auto、Read less、Blocks、Browse)。
- DSH 适配注意:没有 hooks,不会每个会话自动启用,`/caveman status` 只能报告 `(not tracked by this host)`;需要时手动调用,用 `stop caveman` 或 `normal mode` 退出。
- 上游根目录的 NOTICE 文件已原样复制为同目录 UPSTREAM-NOTICE。