# sdd-init - 初始化 SDD 脚手架

> 在当前目录初始化 SDD (Specification-Driven Development) 脚手架

---

## 功能说明

此命令会在当前目录创建完整的 SDD 脚手架结构，包括：

1. `docs/` 目录 - 完整的 SDD 文档体系
2. `.codebuddy/rules/` - CodeBuddy 自动加载的工作规范
3. `CODEBUDDY.md.template` - 项目配置文件模板
4. `AGENTS.md.template` - CodeBuddy Agents 配置模板

---

## 执行步骤

### 1. 检查当前目录状态

```bash
# 检查是否已经是 Git 仓库
git rev-parse --git-dir 2>/dev/null

# 检查是否已有 SDD 结构
[ -d "docs/" ] && echo "检测到已有 docs/ 目录"
[ -f "CODEBUDDY.md" ] && echo "检测到已有 CODEBUDDY.md"
```

**处理逻辑**：
- 如果已有 SDD 结构 → 提示用户使用 `/sdd-update` 命令更新
- 如果是 Git 仓库 → 正常初始化，文件会加入版本控制
- 如果不是 Git 仓库 → 询问用户是否要初始化 Git

### 2. 复制脚手架文件

从插件目录复制以下文件到当前目录：

```bash
# 获取插件目录路径（CodeBuddy 会自动提供）
PLUGIN_DIR="<插件安装目录>"

# 复制核心目录
cp -r "$PLUGIN_DIR/docs" .
cp -r "$PLUGIN_DIR/.codebuddy" .

# 复制模板文件
cp "$PLUGIN_DIR/docs/ai-config/CODEBUDDY.md.template" ./
cp "$PLUGIN_DIR/docs/ai-config/for-codebuddy.md" ./AGENTS.md.template

# 复制 README
cp "$PLUGIN_DIR/README.md" ./
```

### 3. 自定义配置

```bash
# 提示用户编辑 CODEBUDDY.md
echo "请根据您的项目编辑以下文件："
echo "  1. CODEBUDDY.md.template → 重命名为 CODEBUDDY.md 并填入项目信息"
echo "  2. AGENTS.md.template → 重命名为 AGENTS.md（可选）"
echo "  3. docs/01-使用说明.md → 根据项目调整"
```

### 4. 初始化 Git（可选）

```bash
if [ ! -d ".git" ]; then
  read -p "是否初始化 Git 仓库？(y/n) " -n 1 -r
  echo
  if [[ $REPLY =~ ^[Yy]$ ]]; then
    git init
    git add .
    git commit -m "feat: 初始化 SDD 脚手架"
  fi
fi
```

### 5. 完成提示

```bash
echo "✅ SDD 脚手架初始化完成！"
echo ""
echo "下一步："
echo "  1. 将 CODEBUDDY.md.template 重命名为 CODEBUDDY.md"
echo "  2. 编辑 CODEBUDDY.md，填入您的项目信息"
echo "  3. 开始使用 SDD 工作流程开发"
echo ""
echo "参考文档："
echo "  - docs/01-使用说明.md - 快速开始"
echo "  - docs/03-工作流程.md - 开发流程"
echo "  - docs/04-AI工具指南.md - AI 工具使用"
```

---

## 使用示例

```bash
# 在新项目中初始化 SDD 脚手架
cd my-new-project
/plugin run sdd-starter/sdd-init

# 或者在已有项目中初始化
cd existing-project
/plugin run sdd-starter/sdd-init
```

---

## 注意事项

1. **备份重要文件** - 初始化前请备份您的项目文件
2. **冲突处理** - 如果已有 `docs/` 目录，会提示冲突
3. **Git 历史** - 初始化会创建新的 Git 提交（如果初始化 Git）
4. **模板文件** - 记得将 `.template` 文件重命名并自定义

---

## 下一步

初始化完成后，建议执行：

```bash
# 1. 重命名模板文件
mv CODEBUDDY.md.template CODEBUDDY.md

# 2. 编辑项目配置
# 编辑 CODEBUDDY.md，填入项目信息

# 3. 开始第一个功能开发
# 按照 docs/03-工作流程.md 执行
```
