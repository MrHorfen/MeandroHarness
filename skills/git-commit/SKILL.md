---
name: "git-commit"
description: "根据 Conventional Commits 规范生成提交信息，并在分析代码变更的基础上自动完成变更拆分、文件暂存与安全提交。"
---

# Git Commit 技能

基于 Conventional Commits 规范的代码提交辅助工具。分析代码变更，自动完成文件分组与暂存，并生成结构化的提交信息。

## 何时触发

- 用户请求提交代码时
- 用户请求生成 commit message 时
- 用户请求分析 git diff 时
- 用户请求暂存文件时

## 功能说明

### 生成规范的提交信息

分析暂存区或工作区的代码差异（diff），识别变更类型（如 feat、fix、docs）及作用范围（scope），生成符合 Conventional Commits 规范的提交信息。

提交类型：
- `feat`: 新功能
- `fix`: Bug 修复
- `docs`: 文档变更
- `style`: 代码格式调整
- `refactor`: 代码重构
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建/工具变更

### 按逻辑拆分与暂存变更

当工作区中存在多项变更时，支持通过文件路径、通配符或交互式方式（git add -p）来暂存特定文件，确保单次提交仅包含一个变更。

### 安全执行 Git 提交流程

严格遵循 Git 安全协议（参考 `agent_constitution.md` §Git 纪律）：
- 提交前确认 `git root`、`git status --short`、ignored 文件和 staged 文件列表
- **禁止 `git add .`**：只 stage 与本任务相关的文件
- **禁止 force-add ignored 文件**，除非用户明确点名路径并要求
- dirty worktree 中不得回滚、覆盖或吸入用户未授权的改动
- 不修改 Git 配置
- 不执行破坏性命令（如 `push --force`、`reset --hard`、`checkout --`、`branch -D`），除非用户明确要求并确认风险
- 默认不跳过代码检查钩子（`--no-verify`）
- 钩子校验失败时，需修复后重新提交，不使用 `git commit --amend` 覆盖历史提交
- 若存在嵌套仓库，父仓和子仓分别审计、分别验收、分别 staged、分别提交

## 执行步骤

1. 执行 `git status` 查看当前变更状态
2. 执行 `git diff` 分析代码差异
3. 识别变更类型和作用范围
4. 生成符合规范的提交信息
5. 按逻辑分组暂存文件
6. 执行 `git commit` 提交变更
7. 执行 `git log` 确认提交结果

## 注意事项

- 不会将 .env、凭证或私钥等机密文件加入提交列表
- 提交描述必须使用现在时祈使句
- 提交描述长度不超过 72 字符
