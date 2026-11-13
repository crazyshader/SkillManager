# 来源说明

- 来源:https://github.com/nextlevelbuilder/ui-ux-pro-max-skill,commit 6ae86f61f520d6ca229d2f40e070410638c908c5,许可证 MIT(见同目录 LICENSE)
- 范围:仓库 .claude/skills/ui-ux-pro-max(SKILL.md、data、references、scripts)
- 本地改动:SKILL.md 中的脚本路径 ${'$'}{CLAUDE_PLUGIN_ROOT}/.claude/skills/ui-ux-pro-max/scripts/search.py 改为 <技能目录>/scripts/search.py,并加一句让模型用用户语言解释结果;frontmatter 增加 disable-model-invocation: true(仅手动触发);其余原样复制。
- 未搬运:cli/、.claude-plugin/、同仓库的 banner-design、brand、design、design-system、slides、ui-styling 等其他技能。
- DSH 适配注意:脚本只用 Python 标准库;--persist 会在 --output-dir 下写 design-system/ 目录。