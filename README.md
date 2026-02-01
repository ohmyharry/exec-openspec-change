# execute-openspec-change

一个用于自动化执行 OpenSpec 提案/变更的 Claude Code skill。这个 skill 通过编排多个 Superpowers skills，提供从提案到合并代码的完整自动化执行工作流。

An automated execution workflow skill for OpenSpec proposals/changes. It orchestrates multiple Superpowers skills to provide a seamless execution experience from proposal to merged code.

## 功能特性 / Features

- **自动触发识别** - 支持命令行和自然语言触发
- **设计验证** - 自动检查并补充 design.md
- **智能规划** - 智能合并 tasks.md 与新生成的计划
- **工作树管理** - 使用 git worktree 创建隔离环境
- **子代理执行** - 通过 subagent 实现自动化实现
- **代码审查** - 自动触发代码审查流程
- **测试验证** - 自动化测试和验证
- **合并决策** - 提供详细的合并前报告

## 使用方法 / Usage

### 触发方式 / Trigger Patterns

```bash
# 命令方式 / Command style
/execute add-subagent-display
/exec fix-permission

# 自然语言 / Natural language
执行 add-subagent-display
帮我完成这个变更
开始实现 fix-permission
```

### 执行模式 / Execution Modes

```bash
/execute <change-id>              # 全自动模式 (默认)
/execute <change-id> --mode=guided  # 引导模式 - 每个检查点暂停
/execute <change-id> --mode=debug   # 调试模式 - 详细输出
```

### 状态查询 / Status Query

```bash
/status                           # 查看所有活跃工作树
/status <change-id>               # 查看特定变更进度
/status --all                     # 包含已完成的
```

## 工作流程 / Workflow

```
PENDING → DESIGNING → PLANNING → EXECUTING → REVIEWING → TESTING → READY → MERGED
```

1. **Design Validation** - 检查 design.md 完整性
2. **Planning** - 生成并合并 tasks.md
3. **Worktree Creation** - 创建 git worktree 隔离环境
4. **Execution** - 通过 subagent 执行任务
5. **Code Review** - 自动代码审查
6. **Testing** - 运行测试验证
7. **Merge** - 合并到主分支

## 状态持久化 / State Persistence

每个变更的执行状态保存在 `openspec/changes/<id>/.exec-state.json`:

```json
{
  "change_id": "add-subagent-display",
  "status": "EXECUTING",
  "mode": "auto",
  "worktree_path": "/worktrees/server-add-sub",
  "current_task": 15,
  "total_tasks": 20,
  "checkpoints": [...]
}
```

## 依赖管理 / Dependency Management

支持变更之间的依赖关系管理：

```markdown
## Dependencies
- depends_on: another-change-id
- blocks: multi-session-support
```

## 集成的 Superpowers Skills

| Phase | Skill | Purpose |
|-------|-------|---------|
| Design | superpowers:brainstorming | 生成/补充 design.md |
| Plan | superpowers:writing-plans | 生成执行计划 |
| Worktree | superpowers:using-git-worktrees | 创建隔离环境 |
| Execution | superpowers:subagent-driven-development | 执行实现任务 |
| Review | superpowers:requesting-code-review | 代码质量审查 |
| Acceptance | superpowers:verification-before-completion | 最终验证 |

## 安装 / Installation

1. Clone 此仓库到你的 Claude Code skills 目录：
   ```bash
   git clone https://github.com/ohmyharry/exec-openspec-change.git ~/.claude/skills/exec-openspec-change
   ```

2. 确保 SKILL.md 在正确的位置

3. 重启 Claude Code

## 贡献 / Contributing

欢迎提交 Issue 和 Pull Request！

## 许可证 / License

MIT License

## 作者 / Author

[@ohmyharry](https://github.com/ohmyharry)
