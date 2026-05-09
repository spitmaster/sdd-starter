# Agents 目录

> 存放预定义的 Agent 定义

---

## 目录说明

本目录存放各种预定义的 Agent，每个 Agent 是一个专业角色。

---

## 添加新的 Agent

创建新 agent 文件，命名规范：`{agent-name}.md`

```markdown
# {Agent 名称}

## 角色
描述这个 agent 是什么角色

## 专业领域

## 触发词
- `召唤 {agent}`
- `使用 {agent}模式`
- `{关键词}`

## 工作流程

## 系统提示

## 工具权限

## 输出格式
```

---

## 现有 Agents

| Agent | 说明 |
|-------|------|
| SDD-Architect | 架构师，分析需求生成 SPEC |
| SDD-Developer | 开发者，按 SPEC 开发 |
| SDD-Reviewer | 审查员，代码审查 |
| SDD-Validator | 验证师，检查完整性 |
| SDD-Diagnoser | 诊断师，项目诊断 |

详情见 `docs/ai-config/for-codebuddy.md`

---

## Agent 模板

```markdown
# {Agent 名称}

## 角色

## 专业领域

## 触发词

## 工作流程

## 系统提示

## 输出格式
```

复制模板创建新 agent：`docs/agents/{你的agent名称}.md`
