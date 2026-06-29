# SDD 工作流程规范

> 本文件由 SDD 脚手架自动生成，AI 工具会自动加载并遵守

---

## 核心原则

1. **SPEC 是唯一真相源** - 所有需求必须来自 `SPEC.md`
2. **先 SPEC，后代码** - 任何修改前先确认 SPEC 中是否已定义
3. **验收标准驱动** - 实现必须满足 SPEC 中的验收标准
4. **变更必须更新 SPEC** - 代码变更前先更新 SPEC

---

## 开始工作前必做

1. 读取 `docs/01-使用说明.md` 了解 SDD 核心规范
2. 读取 `SPEC.md` 理解当前项目的功能需求
3. 读取 `docs/todo/master.md` 了解当前开发任务
4. 按照 `docs/03-工作流程.md` 执行开发

---

## Git Commit 规范

- **不要频繁 commit** - 完成一个完整功能后再 commit
- **Commit message 要清晰** - 描述做了什么，为什么做
- **测试通过后再 commit** - 确保功能正常，不破坏已有功能

---

## 开发流程

1. **需求收集** → 输出：`requirements.md`（如需要）
2. **SPEC 编写** → 输出：`SPEC.md`
3. **开发实现** → 按照 SPEC 开发，频繁验证
4. **自测验证** → 对照 SPEC 验收标准测试
5. **验收确认** → 确认满足 SPEC，commit

---

## 项目特定规范

- 文件行数上限 500 行（超过需拆分）
- 新增功能先更新 SPEC.md，再动手
- 训练相关逻辑在 `services/session-service.js`
- 统计在 `services/stats-service.js`
- 图表复用 `utils/line-chart.js`（Canvas 2D 手绘）

---

*由 SDD 脚手架自动生成 - 请勿手动修改*
