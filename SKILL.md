---
name: market-research
description: 市场调研专业工作流 Skill。当用户需要进行竞品分析、市场规模评估（TAM/SAM/SOM）、用户研究、行业趋势分析、市场进入策略分析、用户画像构建、定价研究、价格策略分析、客户旅程分析、客户细分分析时触发。支持从客户需求接收到报告交付的完整闭环，内置六角色协作机制（项目经理、需求分析师、数据研究员、市场情报员、首席分析顾问、质量审核员）。适用于 B2B/B2C 市场调研，支持单代理顺序执行和多代理并行模式。
---

# Market Research Skill

将市场调研团队的协作模式内置为可复用的 AI Skill，架构灵感来自 [oh-my-opencode](https://github.com/oh-my-opencode) 的多代理协作系统。

## 角色体系

本 Skill 将调研工作分解为六个专业角色，每个角色有清晰的职责边界：

| 角色 | 核心职责 | 对应原型 |
|------|----------|----------|
| **项目经理** (PM) | 需求接收、任务编排、交付管理 | Sisyphus |
| **需求分析师** (RA) | 需求澄清、范围定义、隐性目标识别 | Metis |
| **数据研究员** (DR) | 内部数据检索、历史报告整理 | explore |
| **市场情报员** (MI) | 外部数据收集、竞品信息、行业数据 | librarian |
| **首席分析顾问** (SA) | 数据综合、洞察提炼、战略建议 | Oracle |
| **质量审核员** (QR) | 报告把关、逻辑验证、数据核查 | Momus |

角色详细指令见 `references/roles/` 目录。

## 执行模式选择

**单代理模式（简单任务 / 快速交付）**：
由你作为项目经理，按顺序扮演各角色完成工作。适合需求清晰、时效紧的场景。

**多代理模式（复杂任务 / 高质量交付）**：
使用 `task()` 委托专业子代理并行执行。数据研究员和市场情报员可同时工作。

## 工作流概览

所有调研项目遵循此核心流程：

```
[客户需求]
    ↓
[第1步] 需求分析师 → 需求澄清文档
    ↓
[第2步] 数据研究员 + 市场情报员 → 并行数据收集（background=true）
    ↓（等待两者完成）
[第3步] 首席分析顾问 → 综合分析与洞察
    ↓
[第4步] 项目经理 → 起草报告
    ↓
[第5步] 质量审核员 → 审核 + 反馈循环
    ↓
[交付客户]
```

## 调研类型路由

根据需求类型，加载对应工作流：

| 调研类型 | 工作流文件 |
|----------|-----------|
| 竞品分析 | `references/workflows/competitor-analysis-workflow.md` |
| 市场规模评估 | `references/workflows/market-sizing-workflow.md` |
| 用户研究 | `references/workflows/user-research-workflow.md` |
| 行业趋势报告 | `references/workflows/industry-trend-workflow.md` |

## 核心工作流步骤

### 第 1 步：需求接收与澄清

**执行角色**：需求分析师（详见 `references/roles/requirements-analyst.md`）

必须明确以下四项后才能进入下一步：
1. **调研目标**：客户最终要做什么决策？
2. **竞品范围**：分析哪些公司/产品？
3. **交付物**：报告格式、详细程度、截止时间
4. **数据质量要求**：是否接受估算？需要哪些数据来源？

⚠️ **在此步骤提问，不要在调研过程中中途打断。**

输出：需求澄清文档（使用 `assets/report-templates/requirements-doc.md` 模板）

---

### 第 2 步：并行数据收集

**执行角色**：数据研究员 + 市场情报员（同时启动）

**数据研究员任务**（`references/roles/data-researcher.md`）：
- 检索内部已有的行业报告、历史调研
- 整理客户现有数据资产
- 汇总相关数据指标

**市场情报员任务**（`references/roles/market-intelligence.md`）：
- 收集目标市场公开数据（数据源见 `references/methods/data-sources.md`）
- 整理竞品公开信息（官网、招聘、新闻、融资等）
- 提取行业报告关键数据

**多代理模式实现**：
```
task(role="数据研究员", prompt="...", run_in_background=true)   → task_id_A
task(role="市场情报员", prompt="...", run_in_background=true)   → task_id_B
# 继续其他工作
# 等待两者完成后合并结果
background_output(task_id_A) + background_output(task_id_B)
```

---

### 第 3 步：综合分析与洞察

**执行角色**：首席分析顾问（详见 `references/roles/senior-analyst.md`）

输入：第 2 步的所有数据收集结果

分析优先级：
1. 交叉验证关键数据（多源印证）
2. 识别核心趋势与模式
3. 量化机会与风险
4. 推导战略含义
5. 排序洞察（按决策价值）

---

### 第 4 步：报告起草

**执行角色**：项目经理

根据调研类型选择报告模板（`assets/report-templates/`）：
- 执行摘要（单页版，≤500字）→ `executive-summary.md`
- 竞品分析专项报告 → `competitor-analysis-report.md`
- 市场规模评估报告 → `market-sizing-report.md`
- 用户研究报告 → `user-research-report.md`
- 完整通用报告 → `standard-report.md`
- 投资人 BP 市场章节 → `investor-bp-market.md`

**报告必须包含**：
- [ ] 明确的执行摘要（≤300字）
- [ ] 3-5 条关键发现（数据支撑）
- [ ] 具体可执行建议（非泛泛而谈）
- [ ] 数据来源注释
- [ ] 置信度说明（高/中/低）

---

### 第 5 步：质量审核

**执行角色**：质量审核员（详见 `references/roles/quality-reviewer.md`）

**必检清单**（所有项目通过才能交付）：
- [ ] 是否完整回答了需求澄清文档中的客户问题？
- [ ] 关键数据是否标注来源且来源可信？
- [ ] 每个结论是否有明确的数据/逻辑支撑？
- [ ] 建议是否具体、可执行、有优先级？
- [ ] 报告格式是否符合约定的交付标准？
- [ ] 是否有未经验证的假设未被标注？

**未通过**：返回第 4 步修改，注明具体问题。
**通过**：可交付客户。

## 方法论工具箱

按需加载对应方法论文件：

- **竞品分析框架**（SWOT、波特五力、竞品矩阵）→ `references/methods/competitor-analysis.md`
- **市场规模估算**（TAM/SAM/SOM、自上而下/自下而上）→ `references/methods/market-sizing.md`
- **用户研究方法**（访谈、问卷、焦点小组）→ `references/methods/user-research.md`
- **公开数据源清单** → `references/methods/data-sources.md`
- **定价研究**（Van Westendorp PSM、Gabor-Granger、价值定价）→ `references/methods/pricing-research.md`
- **Jobs-to-be-Done**（JTBD框架、切换访谈、Job Map）→ `references/methods/jobs-to-be-done.md`
- **客户旅程地图**（5阶段旅程、情绪曲线、接触点分析）→ `references/methods/customer-journey.md`
- **客户细分**（RFM模型、B2B价值矩阵、行为聚类）→ `references/methods/segmentation.md`

## 角色协作协议

详见 `references/roles/collaboration-protocol.md`，核心原则：

1. **结构化交接**：每个角色输出标准格式的「交接文档」再传递给下一角色
2. **并行优先**：数据研究员与市场情报员默认并行执行
3. **单一提问原则**：需求澄清阶段集中提问，执行阶段不中断
4. **质量门禁**：质量审核员有权打回报告，最多循环 2 次
