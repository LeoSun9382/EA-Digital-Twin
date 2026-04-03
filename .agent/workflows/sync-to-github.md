---
description: Unified Release Workflow: Iteration Log, README Sync, Squash Merge & Push
---
// turbo-all

# Sync & Release (Unified Workflow)

该工作流是引擎的唯一发版入口。它会自动检测版本状态，并在推送 GitHub 前确保日志、README 和版本标签同步。

## 核心流程

### Step 0: 智能化版本审计与记账 (Auto-Log Check)
- **获取当前环境**：读取 `SKILL.md` 的 `version` (如 `v5.0.0`)。
- **检测总账状态**：读取根目录的 `ITERATIONS.md`。
- **分支感知**：执行 `git branch --show-current`。
- **智能决策**：
  1. 如果当前版本号**未记录**在 `ITERATIONS.md` 中：
     - **自动起草**：基于 `_docs/_history/v[当前]/` 下的 `plan.md` 和 `task.md` 起草日志。
     - **用户确认**：展示日志草案并询问“是否一键更新总账？”。
     - **双重归档**：确认后执行 `generate-iteration-log` 逻辑（存入历史文件夹并追加至总账顶部）。
  2. 如果版本已记录或仅为常规修复：直接进入下一步。

### Step 1: README & 资产校验 (Compliance Sync)
- **版本图标同步**：检查 `README.md` 顶部的版本 Badge。
- **[强制动作]**：确保 Badge 内容（如 `version-v5.0.0-blue`）与 `SKILL.md` 完全一致。如果不符，执行 `replace_file_content`。
- **资产暂存**：在当前分支执行 `git add .`。

### Step 2: 压缩合并入展柜 (The Squash)
- **触发条件**：仅当从 `dev-*` 分支发起且检测到正式发版需求时。
- **合并逻辑**：
```bash
cd /Users/leosun/Desktop/EA-Digital-Twin && \
git checkout main && \
git merge --squash [dev-分支名] && \
git commit -m "feat: EA Digital Twin Engine [版本号] — Release" && \
git tag [版本号]
```

### Step 3: GitHub 远程推送 (The Push)
- **推送动作**：
```bash
git push origin main --tags
```

### Step 4: 清理与汇报 (Finalize)
- **分支清理**：询问是否删除已结项的 `dev` 分支。
- **成功汇报**：展示最新 GitHub 公开链接及 Commit Hash。
