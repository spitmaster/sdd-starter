# SDD 工作流程规范

> 本文件由 SDD 脚手架自动生成，CodeBuddy 会自动加载并遵守
> 本规范基于全局规则 `~/.codebuddy/rules/project-guide.md` v1.2

---

## 核心原则（[MUST]）

1. **SPEC 是唯一真相源** - 所有需求必须来自 `SPEC.md`，代码注释、README.md、git log 都可能滞后
2. **先 SPEC，后代码** - 任何修改前先确认 SPEC 中是否已定义，不确定时先读文档
3. **验收标准驱动** - 实现必须满足 SPEC 中的验收标准，未完成验收标准不 commit
4. **变更必须更新 SPEC** - 代码变更前先更新 SPEC，保持文档与代码同步
5. **接手 30 秒原则** - 任何 session 开始时必须能回答"现在到哪一步"、"下一步做什么"

---

## 开始工作前必做（[MUST]）

1. **读取项目规范** - 读取 `CODEBUDDY.md`（项目根目录）了解项目特定规范
2. **读取 SPEC** - 读取 `SPEC.md` 理解当前项目的功能需求和验收标准
3. **读取当前任务** - 读取 `docs/todo/<branch>.md` 了解当前开发任务（如有）
4. **按照工作流程执行** - 严格按照 `docs/03-工作流程.md` 执行开发

---

## 三档规划文档体系（[SHOULD]）

根据项目规模使用不同的文档体系：

### 微项目（< 500 行）
- 跳过 `docs/`，只在源文件顶部写 `# 设计意图` 注释块
- 不需要 `CODEBUDDY.md`

### 小项目（500-5000 行）
- 使用两档：`roadmap.md` + `todo.md`
- 不分 product/program，功能设计写在一份 `design.md`

### 中大型项目（5000+ 行）
- 完整三档：`roadmap.md` + `milestones.md` + `todo.md`
- 按功能分 product/program 文档
- 必须有 `CODEBUDDY.md` + `docs/README.md`

---

## Git Commit 规范（[MUST]）

**重要：不要频繁 commit！**

- **完成完整功能后再 commit** - 不要一有改动就 commit，所有相关改动完成后一起 commit
- **Commit message 要清晰** - 使用常规格式：`feat:`, `fix:`, `docs:`, `refactor:`, `test:`
- **测试通过后再 commit** - 确保功能正常，不破坏已有功能
- **不 commit 中间过程** - 开发过程中的试错、调试代码不要 commit

---

## 开发流程（[MUST]）

1. **需求收集** → 输出：`requirements.md`（如需要）
2. **SPEC 编写** → 输出：`SPEC.md`（包含功能需求 + 验收标准）
3. **制定计划** → 输出：`docs/todo/<branch>.md`（具体任务清单）
4. **开发实现** → 按照 SPEC 开发，频繁验证
5. **自测验证** → 对照 SPEC 验收标准测试
6. **验收确认** → 确认满足 SPEC，一次性 commit

---

## 文档维护规范（[MUST]）

### 文档是真相源
- 代码注释、README.md、git log **都可能滞后**
- AI 接手后做任何架构判断之前，必须以设计文档为准
- 文档与代码不一致时，要么改代码、要么改文档，**不允许**让两者继续矛盾

### 文档更新触发
- 功能变更 → 先更新 SPEC.md
- 架构调整 → 先更新 design.md
- 完成里程碑 → 更新 milestones.md + todo.md
- 完成功能 → 更新 roadmap.md + 归档 milestones

---

## 项目特定规范

**注意**：以下规范为模板示例，实际项目应在 `CODEBUDDY.md` 中定义

- 文件行数上限 500 行（超过需拆分）
- 训练相关逻辑在 `services/session-service.js`
- 统计在 `services/stats-service.js`
- 图表复用 `utils/line-chart.js`（Canvas 2D 手绘）

---

## 反模式（[MUST] 避免）

| 反模式 | 为什么不好 | 正确做法 |
|--------|-----------|---------|
| 把"路线图 + 技术债 + 历史版本"塞一份 `roadmap.md` | 三类信息更新频率不同 | 拆开或明确分小节 |
| 用 commit log 当 milestones | 接手者必须 `git log` 才能知道进度 | milestones.md 主动维护 |
| 文档间用绝对路径 | 换机器立刻全断 | 全用相对路径 |
| 每个新功能起一份独立文档但不更新索引 | 索引滞后等于文档不存在 | 加文档时同步更新索引 |
| AI 修改设计文档时不告诉用户 | 用户失去对真相源的所有权 | 修改前明确告知用户 |

---

*由 SDD 脚手架自动生成 - 基于 ~/.codebuddy/rules/project-guide.md v1.2*
