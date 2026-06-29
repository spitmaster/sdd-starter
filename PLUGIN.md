# SDD-Starter 插件使用指南

> 本文档说明如何安装和使用 sdd-starter 插件

---

## 插件功能

`sdd-starter` 插件提供以下功能：

1. **初始化脚手架** (`/sdd-init`) - 在当前目录初始化 SDD 脚手架
2. **更新脚手架** (`/sdd-update`) - 更新当前目录的 SDD 脚手架到最新版本
3. **检查状态** (`/sdd-check`) - 检查当前目录的 SDD 脚手架状态
4. **工作流程指导** (`sdd-workflow` skill) - 提供 SDD 开发流程的逐步指导

---

## 安装方法

### 方式 1: 从插件市场安装（推荐）

```bash
# 1. 添加插件市场（如果尚未添加）
/plugin marketplace add https://github.com/spitmaster/sdd-starter

# 2. 安装 sdd-starter 插件
/plugin install sdd-starter

# 3. 验证安装
/plugin list
```

### 方式 2: 本地开发安装

```bash
# 1. 克隆 sdd-starter 仓库
git clone https://github.com/spitmaster/sdd-starter.git

# 2. 在 CodeBuddy 中加载本地插件
/plugin install local /path/to/sdd-starter
```

---

## 使用方法

### 1. 初始化脚手架

在新项目中使用 SDD 脚手架：

```bash
# 进入你的项目目录
cd my-project

# 运行初始化命令
/plugin run sdd-starter/sdd-init

# 按照提示操作
# 1. 确认初始化
# 2. 编辑 CODEBUDDY.md（从模板复制）
# 3. 开始使用 SDD 工作流程
```

**初始化后的下一步**：

```bash
# 1. 复制模板文件
cp CODEBUDDY.md.template CODEBUDDY.md

# 2. 编辑 CODEBUDDY.md，填入项目信息
# 使用编辑器打开 CODEBUDDY.md，修改：
#   - 项目一句话定位
#   - 架构不变量
#   - 关键目录速查
#   - 开发规范

# 3. 可选：复制 AGENTS.md
cp AGENTS.md.template AGENTS.md

# 4. 开始开发
# 按照 docs/03-工作流程.md 执行
```

### 2. 更新脚手架

当 SDD 脚手架有新版本时，更新你的项目：

```bash
# 进入你的项目目录
cd my-project

# 运行更新命令
/plugin run sdd-starter/sdd-update

# 按照提示操作
# 1. 确认更新
# 2. 检查配置文件差异（如有自定义）
# 3. 合并自定义配置
```

**更新策略**：

- ✅ **保留**：`CODEBUDDY.md`、`AGENTS.md`、`SPEC.md`、`docs/todo/`、`docs/milestones/`
- 🔄 **更新**：`docs/01-使用说明.md` 等核心文档、`.codebuddy/rules/`、`README.md`
- � backup **自动备份**：更新前会自动备份自定义配置文件

### 3. 检查状态

检查当前目录的 SDD 脚手架状态：

```bash
# 进入你的项目目录
cd my-project

# 运行检查命令
/plugin run sdd-starter/sdd-check

# 输出示例：
# === SDD 脚手架状态检查 ===
# ✅ docs/ 目录存在
# ✅ .codebuddy/ 目录存在
# ✅ CODEBUDDY.md 存在
# ...
# 脚手架完整度：8/10
```

### 4. 使用工作流程 Skill

在开发过程中，使用 `sdd-workflow` skill 获取指导：

```bash
# 触发 skill
> 请按照 sdd-workflow skill 指导我完成 SDD 开发

# 或在具体步骤中参考
> 下一步应该做什么？（参考 sdd-workflow skill）
```

---

## 插件结构

```
sdd-starter/
├── .codebuddy-plugin/          # 插件配置目录
│   └── plugin.json             # 插件描述文件
├── commands/                   # 插件命令
│   ├── sdd-init.md            # 初始化命令
│   ├── sdd-update.md          # 更新命令
│   └── sdd-check.md          # 检查命令
├── skills/                     # 插件 skills
│   └── sdd-workflow.md       # 工作流程指导
├── docs/                       # SDD 文档体系
├── .codebuddy/                 # CodeBuddy 配置
└── README.md                   # 项目说明
```

---

## 开发插件

如果您想修改或增强此插件：

### 1. 修改命令

编辑 `commands/` 目录中的文件：

```bash
# 修改初始化命令
edit commands/sdd-init.md

# 修改更新命令
edit commands/sdd-update.md

# 修改检查命令
edit commands/sdd-check.md
```

### 2. 修改 Skill

编辑 `skills/` 目录中的文件：

```bash
# 修改工作流程指导
edit skills/sdd-workflow.md
```

### 3. 更新插件配置

编辑 `.codebuddy-plugin/plugin.json`：

```json
{
  "name": "sdd-starter",
  "version": "1.0.1",  // 更新版本号
  "description": "...",
  // ...
}
```

### 4. 本地测试

```bash
# 在 CodeBuddy 中重新加载插件
/plugin uninstall sdd-starter
/plugin install local /path/to/sdd-starter

# 测试命令
/plugin run sdd-starter/sdd-init
```

---

## 常见问题

### Q: 插件安装失败怎么办？

A: 检查以下几点：
1. 插件市场是否正确添加
2. 插件名称是否拼写正确
3. CodeBuddy 版本是否支持插件功能

### Q: 初始化时提示"已有 SDD 结构"怎么办？

A: 如果您的项目已经初始化过 SDD 脚手架，应该使用更新命令：
```bash
/plugin run sdd-starter/sdd-update
```

### Q: 更新时我的自定义配置会丢失吗？

A: 不会。更新前会自动备份配置文件（`CODEBUDDY.md`、`AGENTS.md` 等），更新后会提示您手动合并。

### Q: 如何卸载插件？

A:
```bash
/plugin uninstall sdd-starter
```

---

## 贡献

欢迎提交 PR 改进此插件！

贡献前请确保：

1. 插件功能符合 SDD 方法论
2. 命令文档清晰完整
3. 测试过安装和使用流程
4. 更新 `plugin.json` 中的版本号

---

## 许可证

MIT License

---

*本文档由 SDD 脚手架插件提供*
