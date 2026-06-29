# sdd-starter:upgrade - 升级 sdd-starter 插件

> 拉取 sdd-starter 最新代码并更新插件到最新版本

---

## 功能说明

此命令会执行以下操作：

1. 从 GitHub 拉取 sdd-starter 最新代码
2. 更新插件文件到最新版本
3. 重新加载插件配置
4. 更新当前项目的脚手架文件（可选）

---

## 执行步骤

### 1. 检查插件安装状态

```bash
# 检查插件是否已安装
/plugin list | grep sdd-starter

if [ $? -ne 0 ]; then
  echo "错误：sdd-starter 插件未安装"
  echo "请先安装插件：/plugin install sdd-starter"
  exit 1
fi

# 获取插件安装路径
PLUGIN_PATH=$(/plugin list --paths | grep sdd-starter | awk '{print $2}')
echo "插件安装路径：$PLUGIN_PATH"
```

### 2. 拉取最新代码

```bash
# 进入插件目录
cd "$PLUGIN_PATH"

# 检查是否是 Git 仓库
if [ -d ".git" ]; then
  echo "正在拉取最新代码..."
  
  # 保存本地修改（如有）
  if [ -n "$(git status --porcelain)" ]; then
    echo "检测到本地修改，正在 stash..."
    git stash
  fi
  
  # 拉取最新代码
  git pull origin main
  
  if [ $? -eq 0 ]; then
    echo "✅ 代码拉取成功"
  else
    echo "❌ 代码拉取失败"
    exit 1
  fi
  
  # 恢复本地修改（如有）
  if [ -n "$(git stash list)" ]; then
    echo "正在恢复本地修改..."
    git stash pop
  fi
else
  echo "⚠️  插件目录不是 Git 仓库"
  echo "建议重新安装插件："
  echo "  1. /plugin uninstall sdd-starter"
  echo "  2. /plugin install sdd-starter"
  exit 1
fi
```

### 3. 更新插件配置

```bash
# 重新加载插件配置
/plugin reload sdd-starter

# 检查插件版本
NEW_VERSION=$(jq -r '.version' "$PLUGIN_PATH/.codebuddy-plugin/plugin.json")
echo "✅ 插件已更新到版本：$NEW_VERSION"
```

### 4. 更新当前项目的脚手架（可选）

```bash
# 询问用户是否更新当前项目
read -p "是否更新当前项目的 SDD 脚手架？(y/n) " -n 1 -r
echo

if [[ $REPLY =~ ^[Yy]$ ]]; then
  echo "正在更新当前项目的脚手架..."
  
  # 备份当前配置
  BACKUP_DIR=".sdd-backup-$(date +%Y%m%d-%H%M%S)"
  mkdir -p "$BACKUP_DIR"
  
  [ -f "CODEBUDDY.md" ] && cp CODEBUDDY.md "$BACKUP_DIR/"
  [ -f "AGENTS.md" ] && cp AGENTS.md "$BACKUP_DIR/"
  
  # 更新脚手架文件
  rsync -av --exclude="SPEC.md" \
    --exclude="todo/" \
    --exclude="milestones/" \
    "$PLUGIN_PATH/docs/" "./docs/"
  
  rsync -av "$PLUGIN_PATH/.codebuddy/" "./.codebuddy/"
  
  cp "$PLUGIN_PATH/docs/ai-config/CODEBUDDY.md.template" ./
  cp "$PLUGIN_PATH/docs/ai-config/for-codebuddy.md" ./AGENTS.md.template
  
  echo "✅ 脚手架已更新"
  echo "备份文件位于：$BACKUP_DIR"
  echo ""
  echo "⚠️  请手动检查并更新以下文件（如有自定义）："
  echo "  1. CODEBUDDY.md"
  echo "  2. AGENTS.md"
fi
```

### 5. 显示更新日志

```bash
# 显示最新 commit 日志
echo ""
echo "=== 最近更新 ==="
cd "$PLUGIN_PATH"
git log --oneline -10

echo ""
echo "=== 更新完成 ==="
echo "插件版本：$(jq -r '.version' .codebuddy-plugin/plugin.json)"
echo "更新时间：$(date '+%Y-%m-%d %H:%M:%S')"
```

---

## 使用示例

```bash
# 升级 sdd-starter 插件到最新版本
/sdd-starter:upgrade

# 输出示例：
# 正在拉取最新代码...
# ✅ 代码拉取成功
# ✅ 插件已更新到版本：1.1.0
# 
# 是否更新当前项目的 SDD 脚手架？(y/n) y
# ✅ 脚手架已更新
# 
# === 最近更新 ===
# 48df673 feat: 添加 sdd-starter CodeBuddy 插件
# 5c3c15d feat: 优化 CodeBuddy 自动加载配置
# ...
```

---

## 故障排除

### 拉取代码失败

```bash
# 检查网络连接
ping github.com

# 检查 Git 配置
git config --list

# 手动拉取
cd ~/.codebuddy/plugins/sdd-starter
git pull origin main
```

### 插件重新加载失败

```bash
# 手动重新安装插件
/plugin uninstall sdd-starter
/plugin install sdd-starter
```

### 更新脚手架时冲突

```bash
# 使用 Git 解决冲突
git status
git diff CODEBUDDY.md

# 或恢复备份
cp .sdd-backup-*/CODEBUDDY.md ./
```

---

## 注意事项

1. **备份重要** - 更新前会自动备份配置文件
2. **网络连接** - 需要访问 GitHub 拉取代码
3. **Git 仓库** - 插件必须是 Git 仓库才能自动更新
4. **手动更新** - 如果自动更新失败，可以手动重新安装插件

---

## 后续步骤

更新完成后，建议：

```bash
# 1. 检查插件状态
/plugin list

# 2. 检查脚手架状态（如果在项目中）
/sdd-starter:check

# 3. 查看更新日志
cat .sdd-update.log

# 4. 测试新功能
/sdd-starter:init --help
```

---

*本命令由 sdd-starter 插件提供*
