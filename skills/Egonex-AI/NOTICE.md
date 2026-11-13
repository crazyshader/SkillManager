# 来源说明

- 来源:https://github.com/Egonex-AI/Understand-Anything,commit `532a2e97dc007104f9a1b8d11b6cc16ae7c8cd3a`(浅克隆检出日期 2026-11-12),许可证 MIT(见同目录 LICENSE)
- 范围:仓库中的 `understand-anything-plugin/` 目录,包含 9 个技能(understand、understand-chat、understand-dashboard、understand-diff、understand-domain、understand-explain、understand-figma、understand-knowledge、understand-onboard)、`agents/`、`packages/`、`hooks/`、`src/` 及根部构建配置。
- 本地改动:9 个 `SKILL.md` 的 frontmatter 首行均加入 `disable-model-invocation: true`,其余原样复制。
- 未收录:`node_modules`、`dist`、`.git`、仓库根部的 install.sh / install.ps1、READMEs、assets、各平台适配目录。
- 目录结构必须保持 `understand-anything-plugin/{skills,agents,packages}`,技能通过相对路径或 `DEVIN_PLUGIN_ROOT` / `CLAUDE_PLUGIN_ROOT` 定位插件根目录,详见 `docs/Understand-Anything使用说明.md`。
