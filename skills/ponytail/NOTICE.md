# 来源说明

- 来源:https://github.com/DietrichGebert/ponytail,commit 96608e078b2d5ea607c5c3b8b5a54d1c1fba6846,许可证 MIT(见同目录 LICENSE)
- 范围:skills/ponytail、ponytail-review、ponytail-audit、ponytail-debt
- 本地改动:四个 SKILL.md 的 frontmatter 增加 disable-model-invocation: true,其余原样复制。
- 未搬运:ponytail-gain(Claude Code 基准测试展示卡)、ponytail-help(讲 Claude Code 信息栏、/plugin 和配置文件)、plugin.json、hooks 与信息栏(依赖 Claude Code)。
- DSH 适配注意:没有 hooks,不会每个会话自动启用,也没有 /ponytail lite|full|ultra 的级别持久化;需要时手动调用技能。
- DSH 适配改动:ponytail-review、ponytail-audit 把输出语言从 "Very simple English" 改为跟随用户语言(与 AGENTS.md 中文规则一致);ponytail 的级别切换说明改为在消息或技能参数中指定级别。
