# 来源说明

- 来源:https://github.com/addyosmani/agent-skills,commit `301b02b`(浅克隆,检出日期 2026-11-12),许可证 MIT(见同目录 LICENSE)
- 范围:`skills/` 下全部 25 个技能(含附属的 `references/`、`scripts/`、`examples.md` 等),以及仓库根目录的 `references/`(7 份共享清单)
- 本地改动:25 个 SKILL.md 的 frontmatter 开头均加入 `disable-model-invocation: true`,其余原样复制。
- 目录布局:各技能中的 `../../references/...` 相对路径在本目录下仍然有效(`addyosmani/references/` 与各技能目录同级),因此共享清单可以正常读取。
- 未搬运:`agents/` 角色文件、`.claude/commands`、`.gemini`、`.codex-plugin` 等各宿主适配层、hooks、evals、docs、CI。上游的 `/spec`、`/plan`、`/build` 等斜杠命令在 DSH 中不存在,请直接用技能名调用。
- 安全提示:`constraint-driven-development/scripts/quality-gate.mjs` 与 `idea-refine/scripts/idea-refine.sh` 是随技能附带的脚本,运行前先阅读。
