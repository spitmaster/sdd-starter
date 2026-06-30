---
description: 检查当前目录的 SDD 脚手架状态
---

# sdd-starter:check - 检查 SDD 脚手架状态

> 检查当前目录的 SDD 脚手架状态，显示版本和配置信息

---

## 功能说明

此命令会检查当前目录的 SDD 脚手架状态，包括：

1. 脚手架是否已初始化
2. 各组件的状态和版本
3. 配置文件是否存在
4. 是否有可用更新

---

## 执行步骤

### 1. 检查基本结构

```bash
echo "=== SDD 脚手架状态检查 ==="
echo ""

# 检查 docs/ 目录
if [ -d "docs/" ]; then
  echo "✅ docs/ 目录存在"
  echo "   路径：$(realpath docs/)"
  echo "   文件数：$(find docs/ -type f | wc -l)"
else
  echo "❌ docs/ 目录不存在"
fi
echo ""

# 检查 .codebuddy/ 目录
if [ -d ".codebuddy/" ]; then
  echo "✅ .codebuddy/ 目录存在"
  echo "   路径：$(realpath .codebuddy/)"
  if [ -f ".codebuddy/rules/sdd-workflow.md" ]; then
    echo "   ✅ sdd-workflow.md 存在"
  else
    echo "   ❌ sdd-workflow.md 不存在"
  fi
else
  echo "❌ .codebuddy/ 目录不存在"
fi
echo ""

# 检查 CODEBUDDY.md
if [ -f "CODEBUDDY.md" ]; then
  echo "✅ CODEBUDDY.md 存在"
  echo "   路径：$(realpath CODEBUDDY.md)"
else
  echo "⚠️  CODEBUDDY.md 不存在（可选）"
fi
echo ""

# 检查 AGENTS.md
if [ -f "AGENTS.md" ]; then
  echo "✅ AGENTS.md 存在"
  echo "   路径：$(realpath AGENTS.md)"
else
  echo "⚠️  AGENTS.md 不存在（可选）"
fi
echo ""
```

### 2. 检查核心文档

```bash
echo "=== 核心文档检查 ==="
echo ""

DOCS=(
  "docs/01-使用说明.md:快速开始"
  "docs/02-SDD方法论.md:SDD 核心概念"
  "docs/03-工作流程.md:开发流程详解"
  "docs/04-AI工具指南.md:AI 工具使用"
  "docs/05-最佳实践.md:常见问题"
  "docs/06-场景指南.md:三种开发场景"
  "docs/07-小程序指南.md:小程序专项"
  "docs/08-项目诊断器.md:自动诊断"
)

for doc_info in "${DOCS[@]}"; do
  doc="${doc_info%%:*}"
  name="${doc_info##*:}"
  if [ -f "$doc" ]; then
    echo "✅ $name"
    echo "   $doc"
  else
    echo "❌ $name 不存在"
    echo "   $doc"
  fi
done
echo ""
```

### 3. 检查版本信息

```bash
echo "=== 版本信息 ==="
echo ""

# 检查插件版本
if [ -f ".codebuddy-plugin/plugin.json" ]; then
  echo "插件版本：$(jq -r '.version' .codebuddy-plugin/plugin.json 2>/dev/null || echo '未知')"
else
  echo "插件版本：未安装（可能是手动复制的脚手架）"
fi

# 检查模板文件
if [ -f "CODEBUDDY.md.template" ]; then
  echo "✅ CODEBUDDY.md.template 存在"
else
  echo "⚠️  CODEBUDDY.md.template 不存在"
fi
echo ""
```

### 4. 检查 Git 状态

```bash
echo "=== Git 状态 ==="
echo ""

if git rev-parse --git-dir > /dev/null 2>&1; then
  echo "✅ Git 仓库"
  echo "   分支：$(git branch --show-current)"
  echo "   状态：$(git status --short | wc -l) 个文件有改动"
else
  echo "⚠️  不是 Git 仓库（建议初始化）"
fi
echo ""
```

### 5. 检查更新

```bash
echo "=== 更新检查 ==="
echo ""

# TODO: 实现远程版本检查
echo "⚠️  更新检查功能待实现"
echo "   建议定期运行 /sdd-starter:update 更新脚手架"
echo ""
```

### 6. 生成报告

```bash
echo "=== 总结 ==="
echo ""

# 计算得分
score=0
total=10

[ -d "docs/" ] && ((score++))
[ -f ".codebuddy/rules/sdd-workflow.md" ] && ((score++))
[ -f "CODEBUDDY.md" ] && ((score++))
[ -f "AGENTS.md" ] && ((score++))
# ... 检查其他文件

echo "脚手架完整度：$score/$total"
echo ""

if [ $score -eq $total ]; then
  echo "✅ SDD 脚手架配置完整！"
elif [ $score -ge $((total / 2)) ]; then
  echo "⚠️  SDD 脚手架配置不完整，建议更新"
else
  echo "❌ SDD 脚手架配置缺失严重，建议重新初始化"
fi
echo ""

echo "建议操作："
if [ ! -d "docs/" ]; then
  echo "  1. 运行 /sdd-starter:init 初始化脚手架"
elif [ $score -lt $total ]; then
  echo "  1. 运行 /sdd-starter:update 更新脚手架"
else
  echo "  1. 开始使用 SDD 工作流程开发"
fi
echo ""
```

---

## 使用示例

```bash
# 检查当前项目的 SDD 脚手架状态
/sdd-starter:check

# 输出示例：
# === SDD 脚手架状态检查 ===
# ✅ docs/ 目录存在
# ✅ .codebuddy/ 目录存在
# ...
# 脚手架完整度：8/10
# ⚠️  SDD 脚手架配置不完整，建议更新
```

---

## 退出代码

- `0` - 检查完成
- `1` - 不是 SDD 项目

---

## 注意事项

1. **非侵入式** - 此命令只检查，不修改任何文件
2. **快速执行** - 检查过程通常不超过 5 秒
3. **详细输出** - 显示每个组件的状态，方便排查问题

---

## 扩展功能（TODO）

- [ ] 检查远程版本，提示更新
- [ ] 自动修复缺失的文件
- [ ] 验证配置文件格式
- [ ] 生成诊断报告（JSON 格式）
