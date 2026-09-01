# git-workflow

[![Version](https://img.shields.io/badge/version-v1.0.0-6f42c1)](./SKILL.md)
[![Tools](https://img.shields.io/badge/opencode%20%7C%20Claude%20Code%20%7C%20Codex-workflow-0ea5e9)](./README.md)
[![Git Workflow](https://img.shields.io/badge/Git%20Workflow-branch%20%7C%20commit%20%7C%20merge-f97316)](./SKILL.md)
[![License](https://img.shields.io/badge/license-MIT-22c55e)](./LICENSE)

为任何 git 项目提供统一开发工作流规范的 agent skill：分支管理、MAA 风格 commit/merge 消息、推送策略与"暂定稳定"判定标准。基于跨工具通用的 Agent Skills 标准，兼容 opencode、Claude Code、OpenAI Codex、Cursor 等。

> 目标：提交历史清晰可回溯，`main` 始终保持在稳定状态。

## 特性

- 分支工作流：只在分支上开发，验证稳定后才合入 `main`
- MAA 风格消息：`<类型>(<作用域>): 中文主体`
- 分支命名、推送备份与稳定判定标准
- 冲突处理与回滚指南

## 安装

标准 Agent Skills 目录，将 `SKILL.md` 复制或 `git clone` 到任一工具对应目录即可：

| 工具 | 目录 |
| ---- | ---- |
| opencode | `~/.config/opencode/skills/git-workflow/` |
| Claude Code | `~/.claude/skills/git-workflow/` |
| OpenAI Codex | `~/.codex/skills/git-workflow/` |
| Codex（新标准） | `~/.agents/skills/git-workflow/` |

```bash
git clone https://github.com/MashedPotato817/git-workflow.git ~/.config/opencode/skills/git-workflow
```

仅用于单个项目：把 `SKILL.md` 复制到项目内的 `.opencode/skills/git-workflow/`（或其他工具对应目录）。

Windows 用户把路径中 `~/` 换成 `%USERPROFILE%\` 即可。安装后**重启对应工具**生效。

## 使用

安装后自动触发：涉及分支管理、commit 提交规范、merge 到 main、暂定稳定、push 备份等话题，或新建分支、准备提交/合并/推送时，会按规范指导操作。也可直接提问，如"帮我合并这个分支到 main"。

## 工作流速览

```bash
# 1. 从最新 main 创建功能分支
git checkout main && git pull
git checkout -b feat/export-pdf

# 2. 随时提交（可保存实验性改动）
git add .
git commit -m "feat: 新增导出功能，支持 PDF 格式"

# 3. 同步 main 到工作分支
git merge main -m "chore(branch): 同步main最新改动"

# 4. 验证稳定后合并到 main（普通合并）
git checkout main
git merge feat/export-pdf -m "feat: 新增导出功能，支持 PDF 格式"

# 5. 推送
git push origin main
```

## Commit 消息规范

格式：`<类型>(<可选作用域>): <中文主体>`

| 类型 | 含义 |
| ---- | ---- |
| `feat` | 新功能 |
| `fix` | 修复 |
| `docs` | 文档 |
| `chore` | 杂项 / 维护 |
| `style` | 样式 / 界面 |
| `refactor` | 重构 |
| `test` | 测试 |
| `perf` | 性能 |
| `ci` | 持续集成 / 构建脚本 |

## License

MIT