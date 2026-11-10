# 调研：karpathy-guidelines skill 是否适合采用

- 查阅日期：2026-11-10
- 来源 1（一手）：https://raw.githubusercontent.com/multica-ai/andrej-karpathy-skills/main/skills/karpathy-guidelines/SKILL.md
  - frontmatter 声明 `license: MIT`。
  - 四条原则：Think Before Coding、Simplicity First、Surgical Changes、Goal-Driven Execution。
- 来源 2（一手，仅作尝试）：https://github.com/multica-ai/andrej-karpathy-skills 页面可以抓取，但 HTML 中未能解析出仓库许可证、星标或提交时间。
- 未能验证：GitHub API 返回 403（速率限制，抓取时间 2026-11-10），无法读取最近提交时间、issue 活跃度和仓库根目录 LICENSE 文件。许可证结论只基于 skill 文件 frontmatter 的 MIT 声明，仓库级许可证需要人工确认。
  AGENTS.md 开源借鉴规则要求的"12 个月内有提交""6 个月内有人回复 issue"条件无法验证，需要人工在仓库页面确认。
- 原文引用的 Karpathy 观察链接：https://x.com/karpathy/status/2015883857489522876（未访问）。

## 结论

- 许可证：skill 声明 MIT，可以直接安装使用，并保留原许可声明。
- 与现有规则重合度高：DSH 系统提示词和 ~/.dsh/AGENTS.md 已经有"先澄清假设""只改必要的部分""验证后再报告"等同类约束。安装后大部分内容是重复的。
- 独有价值：第 3 条"Surgical Changes"的"每一行改动都要能追溯到需求"，以及第 4 条"先写可验证的成功标准再循环到通过"。这两点现有规则里表述较弱。
- 代价：偏保守，会增加提问和计划步骤。对小改动会显得啰嗦，文件内也写明"简单任务凭判断"。

## 建议

适合在需要频繁改现有代码、又常遇到过度改动的场景使用。若只想要其中两条独有原则，可以把它们摘进 AGENTS.md，而不是整份安装。整份照搬属于 AGENTS.md 中需要先询问用户的情形，本次未照搬。