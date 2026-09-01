# git-workflow

一套可复用的 **agent skill**，为任何 git 项目提供统一的开发工作流规范：分支管理、MAA 风格 commit / merge 消息格式、推送策略与"暂定稳定"判定标准。基于跨工具通用的 `SKILL.md` 标准（Agent Skills 规范），兼容 opencode、Claude Code、OpenAI Codex、Cursor 等。

> 目标：提交历史清晰可回溯，`main` 始终保持在稳定状态。

## 特性

- **分支工作流**：所有开发在分支上进行，只有验证过的成果才合入 `main`
- **规范的提交消息**：MAA 风格 `<类型>(<作用域>): 中文主体`
- **规范的合并消息**：手动编写相当于 PR 标题的 merge 消息，通过普通合并保留分支提交历史
- **分支命名规范**：`feat/xxx`、`fix/xxx`、`docs/xxx`、`chore/xxx`
- **推送与稳定策略**：个人仓库备份式推送、明确的稳定判定标准
- **冲突与回滚指南**：处理合并冲突、`git revert` / `git reset` 的正确姿势

## 支持的 AI 工具

`SKILL.md` 采用跨工具通用的 Agent Skills 标准，同一份文件可直接用于以下工具：

| 工具 | 全局目录（所有项目生效） | 项目目录（当前仓库生效） |
| ---- | ---- | ---- |
| opencode | `~/.config/opencode/skills/git-workflow/` | `.opencode/skills/git-workflow/` |
| Claude Code | `~/.claude/skills/git-workflow/` | `.claude/skills/git-workflow/` |
| OpenAI Codex | `~/.codex/skills/git-workflow/` | `.codex/skills/git-workflow/` |
| Codex（新标准） | `~/.agents/skills/git-workflow/` | `$REPO_ROOT/.agents/skills/git-workflow/` |
| Cursor | — | `.cursor/skills/git-workflow/` |

各工具安装示例（以全局安装为例）：

```bash
# opencode
mkdir -p ~/.config/opencode/skills/git-workflow && cp SKILL.md ~/.config/opencode/skills/git-workflow/
# Claude Code
mkdir -p ~/.claude/skills/git-workflow && cp SKILL.md ~/.claude/skills/git-workflow/
# OpenAI Codex
mkdir -p ~/.codex/skills/git-workflow && cp SKILL.md ~/.codex/skills/git-workflow/
# Codex（新标准）
mkdir -p ~/.agents/skills/git-workflow && cp SKILL.md ~/.agents/skills/git-workflow/
```

Windows PowerShell：

```powershell
Copy-Item SKILL.md "$env:USERPROFILE\.config\opencode\skills\git-workflow\"
Copy-Item SKILL.md "$env:USERPROFILE\.claude\skills\git-workflow\"
Copy-Item SKILL.md "$env:USERPROFILE\.codex\skills\git-workflow\"
Copy-Item SKILL.md "$env:USERPROFILE\.agents\skills\git-workflow\"
```

> 也可直接把本仓库 `git clone` 到上述任意目录。安装后**重启对应工具**才会生效。

## 安装

> 前置：安装任一支持 Agent Skills 的工具（[opencode](https://opencode.ai)、[Claude Code](https://code.claude.com)、[OpenAI Codex](https://developers.openai.com/codex/) 等）。
>
> 下面以 opencode 为例，其他工具的目录见上方「支持的 AI 工具」表。

### 方式一：全局安装（推荐，所有项目生效）

把本仓库克隆到 opencode 的全局 skill 目录：

```bash
git clone <本仓库地址> ~/.config/opencode/skills/git-workflow
```

Windows PowerShell：

```powershell
git clone <本仓库地址> "$env:USERPROFILE\.config\opencode\skills\git-workflow"
```

### 方式二：单个项目安装

在目标项目下创建 skill 目录并复制 `SKILL.md`：

```bash
mkdir -p .opencode/skills/git-workflow
cp SKILL.md .opencode/skills/git-workflow/
```

### 方式三：通过 `skills.paths` 引用（不复制）

在项目的 `opencode.json` 中指向本仓库所在目录：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "skills": {
    "paths": ["C:/path/to/git-workflow"]
  }
}
```

> 无论哪种方式，安装后**重启 opencode** 才会生效。

## 使用

安装后，在对话中提到"分支管理 / commit 提交规范 / merge 到 main / 暂定稳定 / push 备份"等话题，或开始新建分支、准备提交、准备合并、准备推送时，skill 会自动触发，指导你按统一规范操作。

也可以主动提问，例如：

- "按工作流规范，我该怎么提交这次改动？"
- "帮我合并这个分支到 main"
- "这个功能完成了吗，可以 merge 吗？"

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

## 自定义

如需调整（例如改用英文主体、新增类型、改变分支命名规则），直接编辑 `SKILL.md` 中的对应章节即可；全局安装时修改 `~/.config/opencode/skills/git-workflow/SKILL.md`。

## GitHub Topics 推荐

本仓库建议设置的 topics（GitHub 仓库页 → About → 编辑 → Topics）：

```
skill, skills, agent-skills, opencode, claude-code, claude, codex, openai-codex, ai, ai-agent, git, git-workflow, version-control, workflow, developer-tools, automation, commit-message
```

## License

MIT