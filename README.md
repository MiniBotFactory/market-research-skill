# market-research-skill

> 🔬 一个面向 AI 代理的市场调研专业工作流 Skill，适用于任何支持 Skill 格式的 AI 编程工具（Claude Code、Opencode、oh-my-opencode 等）。

---

## 这是什么？

`market-research-skill` 将一个专业市场调研团队的**分工协作模式**内置为可复用的 AI Skill。

加载这个 Skill 后，AI 代理（Claude 等）能够：

- 接收外部客户需求，进行结构化需求澄清
- 并行执行内外部数据收集
- 综合数据产出深度洞察
- 交付符合专业标准的市场研究报告

---

## 架构设计

本 Skill 将市场调研团队映射为六个 AI 代理角色：

```
外部客户需求
      ↓
【需求分析师】澄清需求，定义范围
      ↓
【项目经理】编排任务
   ├──→ 【数据研究员】内部数据检索  ─┐
   └──→ 【市场情报员】外部数据采集  ─┤（并行）
                                      ↓
                          【首席分析顾问】深度分析
                                      ↓
                          【质量审核员】把关交付
                                      ↓
                                  [交付客户]
```

| AI 角色 | 职责 | 对应原型 |
|---------|------|----------|
| 项目经理 | 需求接收、任务协调、交付管理 | Sisyphus |
| 需求分析师 | 需求澄清、隐性目标识别 | Metis |
| 数据研究员 | 内部数据检索整理 | explore |
| 市场情报员 | 外部数据采集、竞品信息 | librarian |
| 首席分析顾问 | 深度洞察、战略建议 | Oracle |
| 质量审核员 | 报告质量把关 | Momus |

---

## 功能覆盖

| 调研类型 | 支持 |
|----------|------|
| 竞品分析（SWOT、波特五力、竞品矩阵） | ✅ |
| 市场规模评估（TAM/SAM/SOM） | ✅ |
| 用户研究（访谈指南、问卷设计、用户画像） | ✅ |
| 行业趋势分析（PESTEL、趋势优先级） | ✅ |
| 定价研究（Van Westendorp、Gabor-Granger） | ✅ |
| Jobs-to-be-Done 分析 | ✅ |
| 客户旅程地图 | ✅ |
| 客户细分（RFM、B2B价值矩阵） | ✅ |

---

## 安装方式

### Claude Code / Opencode / oh-my-opencode（推荐）

```bash
# 克隆仓库
git clone https://github.com/MiniBotFactory/market-research-skill.git

# 复制到 skills 目录（~/.claude/skills/ 或平台对应目录）
cp -r market-research-skill ~/.claude/skills/market-research
```

> **平台对应目录**：
> - **Claude Code**：`~/.claude/skills/`
> - **Opencode**：`~/.opencode/skills/`
> - **oh-my-opencode**：`~/.claude/skills/`

### 手动加载（任意平台）

直接将 `SKILL.md` 的内容粘贴到你的 System Prompt 或角色定义中，按需引用 `references/` 目录下的文件即可使用。

---

## 使用方法

安装后，在与 Claude 的对话中，触发词包括：

- "帮我做一个竞品分析"
- "评估一下这个市场的规模"
- "我需要做用户研究"
- "分析一下行业趋势"
- "做市场调研报告"
- "研究一下定价策略"
- "画一张客户旅程地图"
- "做客户细分分析"

### 快速示例

```
用户：帮我分析一下国内企业级项目管理软件市场，
      我们想了解主要竞品的定价策略和目标客群差异。

Claude（加载 market-research-skill 后）：
  → 自动触发需求分析师角色
  → 澄清：竞品范围是否包括国际产品？报告用于什么决策？
  → 并行启动数据收集
  → 输出结构化竞品分析报告
```

---

## 文件结构

```
market-research-skill/
├── SKILL.md                              # 核心：触发条件 + 工作流概览
├── README.md                             # 本文件
├── LICENSE                               # MIT 许可证
│
├── references/
│   ├── roles/                            # 六个角色的详细指令
│   │   ├── collaboration-protocol.md     # 角色间协作协议
│   │   ├── requirements-analyst.md       # 需求分析师
│   │   ├── data-researcher.md            # 数据研究员
│   │   ├── market-intelligence.md        # 市场情报员
│   │   ├── senior-analyst.md             # 首席分析顾问
│   │   └── quality-reviewer.md           # 质量审核员
│   │
│   ├── workflows/                        # 按调研类型的工作流
│   │   ├── competitor-analysis-workflow.md
│   │   ├── market-sizing-workflow.md
│   │   ├── user-research-workflow.md
│   │   └── industry-trend-workflow.md
│   │
│   └── methods/                          # 方法论工具箱（8个）
│       ├── competitor-analysis.md        # SWOT、波特五力、竞品矩阵
│       ├── market-sizing.md              # TAM/SAM/SOM 计算方法
│       ├── user-research.md              # 访谈、问卷、用户画像
│       ├── data-sources.md               # 公开数据源清单（中国+全球）
│       ├── pricing-research.md           # 定价研究（PSM、Gabor-Granger）
│       ├── jobs-to-be-done.md            # JTBD框架、切换访谈、Job Map
│       ├── customer-journey.md           # 5阶段旅程、情绪曲线、接触点
│       └── segmentation.md              # RFM、B2B价值矩阵、行为聚类
│
└── assets/
    └── report-templates/                 # 报告模板（7个）
        ├── executive-summary.md          # 执行摘要（单页版，≤500字）
        ├── standard-report.md            # 标准市场调研报告
        ├── requirements-doc.md           # 需求澄清文档
        ├── competitor-analysis-report.md # 竞品分析专项报告
        ├── market-sizing-report.md       # 市场规模评估报告
        ├── user-research-report.md       # 用户研究报告
        └── investor-bp-market.md         # 投资人BP市场章节
```

---

## 设计理念

### 为什么要这样设计？

传统市场调研的痛点：

1. **需求不清晰**：执行过程中反复返工，浪费资源
2. **数据收集串行**：内部数据和外部数据一前一后找，效率低
3. **分析和数据混用**：数据整理和洞察分析边界模糊
4. **交付质量不稳定**：没有标准化的质量检验机制

本 Skill 的解决方案：

1. **强制需求澄清**：执行前必须产出需求澄清文档
2. **并行数据收集**：内外部数据同时启动，节省 40-60% 时间
3. **角色分离**：数据收集→分析→审核有清晰边界
4. **质量门禁**：质量审核员有权打回，最多 2 次循环

### Skill 结构说明

本 Skill 遵循标准的三层 Progressive Disclosure 设计：

- `SKILL.md` — 触发条件 + 工作流总览（AI 加载 Skill 时首先读取）
- `references/roles/*.md` — 每个角色的详细 Prompt（按需加载，可直接传入子代理）
- `references/methods/*.md` — 方法论知识库（执行对应调研类型时按需加载）

这种设计使 Skill 在任何支持文件引用的 AI 平台上都能有效工作。

---

## 贡献指南

欢迎以下类型的贡献：

- 🔧 **新工作流**：添加新的调研类型（如竞争情报监测）
- 📚 **数据源更新**：更新或添加新的公开数据源
- 🌍 **多语言支持**：英文版、其他语言版
- 🐛 **问题反馈**：在 Issues 中报告实际使用中发现的问题

### 如何贡献

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feature/add-new-workflow`
3. 提交变更：`git commit -m 'Add: new workflow'`
4. 推送分支：`git push origin feature/add-new-workflow`
5. 创建 Pull Request

---

## 许可证

MIT License — 详见 [LICENSE](LICENSE) 文件。

---

## 致谢

- [oh-my-opencode](https://github.com/oh-my-opencode) — 多代理 Skill 架构的灵感来源
- [Opencode](https://github.com/opencode) — AI 编程工具
- [Claude Code](https://claude.ai/code) — Anthropic 官方 CLI

---

*Made with ❤️ for market researchers who want to leverage AI collaboration*
