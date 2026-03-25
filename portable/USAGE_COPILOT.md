# Usage (GitHub Copilot)

Copilot 没有统一的“Skill 插件标准”，不同工作流通常通过以下方式实现持续约束：
- 在编辑器/项目中注入固定指令（类似 Rules/Instructions）
- 使用自定义扩展（如果你已有环境）
- 在每次对话中使用固定模板提示词（prompt template）

通用接入建议：
1. 把 `portable/PLAYBOOK.md` 的 Bootstrapping + Maintainer 关键段落作为项目指令的一部分。
2. 要求 Copilot 在每次实现/修复任务时输出 vertical slice 日志字段：
   - touched zones
   - Evidence（测试/命令/链接）
   - Risks & invariants check
3. 通过脚本/约定让你或 CI 检查治理资产是否被更新（例如：slice log 必须存在、index 必须更新）。

如果你告诉我你具体使用 Copilot 的哪种入口（VS Code 插件？Copilot Workspace？还是某个扩展），我可以把上面的建议进一步落成“可复制的提示词模板”。

