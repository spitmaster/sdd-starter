# sdd-update - 更新 SDD 脚手架

> 更新当前目录的 SDD 脚手架到最新版本

---

## 功能说明

此命令会更新当前目录的 SDD 脚手架文件到最新版本，包括：

1. 更新 `docs/` 目录中的文档
2. 更新 `.codebuddy/rules/` 中的工作规范
3. 更新模板文件
4. 保留您的自定义配置

---

## 执行步骤

### 1. 检查当前目录状态

```bash
# 检查是否是 SDD 项目
[ -f "CODEBUDDY.md" ] || [ -f "AGENTS.md" ] || [ -d "docs/" ]
if [ $? -ne 0 ]; then
  echo "错误：当前目录不是 SDD 项目"
  echo "请先运行 /sdd-init 初始化脚手架"
  exit 1
fi

# 检查 Git 状态
git status --porcelain
if [ -n "$(git status --porcelain)" ]; then
  echo "警告：当前有未提交的改动"
  echo "建议先提交或暂存改动，避免冲突"
  read -p "是否继续？(y/n) " -n 1 -r
  echo
  if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    exit 0
  fi
fi
```

### 2. 备份自定义配置

```bash
# 备份用户的自定义文件
BACKUP_DIR=".sdd-backup-$(date +%Y%m%d-%H%M%S)"
mkdir -p "$BACKUP_DIR"

# 备份可能已自定义的文件
[ -f "CODEBUDDY.md" ] && cp CODEBUDDY.md "$BACKUP_DIR/"
[ -f "AGENTS.md" ] && cp AGENTS.md "$BACKUP_DIR/"
[ -f "docs/01-使用说明.md" ] && cp "docs/01-使用说明.md" "$BACKUP_DIR/"

echo "✅ 配置已备份到 $BACKUP_DIR"
```

### 3. 更新脚手架文件

```bash
# 获取插件目录路径
PLUGIN_DIR="<插件安装目录>"

# 更新 docs/ 目录（保留自定义文档）
rsync -av --exclude="SPEC.md" \
  --exclude="todo/" \
  --exclude="milestones/" \
  "$PLUGIN_DIR/docs/" "./docs/"

# 更新 .codebuddy/ 目录
rsync -av "$PLUGIN_DIR/.codebuddy/" "./.codebuddy/"

# 更新模板文件
cp "$PLUGIN_DIR/docs/ai-config/CODEBUDDY.md.template" ./
cp "$PLUGIN_DIR/docs/ai-config/for-codebuddy.md" ./AGENTS.md.template

# 更新 README
cp "$PLUGIN_DIR/README.md" ./
```

### 4. 恢复自定义配置

```bash
# 提示用户检查并更新配置文件
echo ""
echo "⚠️  请手动检查以下文件，根据需要合并更新："
echo "  1. CODEBUDDY.md - 项目配置文件"
echo "  2. AGENTS.md - Agents 配置（如有）"
echo "  3. docs/01-使用说明.md - 使用说明（如有自定义）"
echo ""
echo "备份文件位于：$BACKUP_DIR"
echo ""

# 显示差异
if [ -f "$BACKUP_DIR/CODEBUDDY.md" ] && [ -f "CODEBUDDY.md" ]; then
  echo "CODEBUDDY.md 差异："
  diff -u "$BACKUP_DIR/CODEBUDDY.md" "CODEBUDDY.md" || true
fi
```

### 5. 更新日志

```bash
# 记录更新日志
cat >> .sdd-update.log << EOF

=== SDD 脚手架更新日志 ===
更新时间：$(date "+%Y-%m-%d %H:%M:%S")
更新版本：<插件版本>
备份位置：$BACKUP_DIR

更新的文件：
  - docs/ (文档体系)
  - .codebuddy/rules/ (工作规范)
  - CODEBUDDY.md.template (模板)
  - AGENTS.md.template (模板)
  - README.md (说明文档)

EOF

echo "✅ 更新日志已保存到 .sdd-update.log"
```

### 6. 完成提示

```bash
echo ""
echo "✅ SDD 脚手架更新完成！"
echo ""
echo "更新内容："
echo "  - docs/ 目录已更新"
echo "  - .codebuddy/rules/ 已更新"
echo "  - 模板文件已更新"
echo ""
echo "下一步："
echo "  1. 检查并更新 CODEBUDDY.md（如有自定义）"
echo "  2. 查看 .sdd-update.log 了解更新详情"
echo "  3. 测试 SDD 工作流程是否正常"
echo ""
echo "回滚方法（如需）："
echo "  git checkout -- ."
echo "  或"
echo "  cp $BACKUP_DIR/* ./"
```

---

## 使用示例

```bash
# 更新当前项目的 SDD 脚手架
/sdd-update

# 或者指定版本更新（TODO）
/sdd-update --version 1.1.0
```

---

## 更新策略

### 保留的文件（不覆盖）

- `CODEBUDDY.md` - 用户的项目配置（会提示合并）
- `AGENTS.md` - 用户的 Agents 配置（会提示合并）
- `SPEC.md` - 项目的规格文档
- `docs/todo/` - 任务清单
- `docs/milestones/` - 里程碑文档
- `.git/` - Git 历史

### 强制更新的文件

- `docs/01-使用说明.md` - 使用说明（除非用户自定义）
- `docs/02-SDD方法论.md` - SDD 方法论
- `docs/03-工作流程.md` - 工作流程
- `docs/04-AI工具指南.md` - AI 工具指南
- `.codebuddy/rules/sdd-workflow.md` - 工作规范
- `README.md` - 脚手架说明

---

## 注意事项

1. **备份优先** - 更新前会自动备份自定义配置
2. **Git 推荐** - 建议在 Git 仓库中使用，方便回滚
3. **手动合并** - 配置文件更新后需要手动合并自定义内容
4. **测试验证** - 更新后建议测试 SDD 工作流程

---

## 故障排除

### 更新失败

```bash
# 查看更新日志
cat .sdd-update.log

# 回滚到备份
cp .sdd-backup-*/CODEBUDDY.md ./
```

### 冲突解决

```bash
# 使用 Git 解决冲突（如果使用了 Git）
git status
git diff CODEBUDDY.md
```
