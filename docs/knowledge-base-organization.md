# 模块6：知识库文档组织与索引结构

## 1. 目标定位

本模块核心解决「多Agent协同下的文档可追溯、可检索、无歧义」问题，通过统一的目录结构、命名规范、版本映射机制，确保所有产出物有明确的存放位置和索引入口，支撑全链路产出物的归档、追溯与版本管理。主要解决以下问题：

- 文档随意存放，Agent无法快速定位目标文件；
- 命名不规范，导致版本混乱、新旧文件难以区分；
- 缺少统一索引，新成员/新Agent接入时无法快速了解项目全貌；
- 版本映射关系不清晰，无法追溯"某次Commit对应哪个需求版本"。

## 2. 各文档组织方式

### 2.1 项目入口文档：AGENTS.md

每个项目必须在根目录创建 `AGENTS.md` 文件，作为项目简介与各主要文档的索引入口，所有Agent和人类用户均可通过该文件快速定位项目核心信息。

**AGENTS.md 核心内容结构**：

```markdown
# Agent Harness Engineering

## 项目简介
（简要描述项目定位、核心功能、技术栈，供Agent快速理解项目背景）

## 核心文档索引
| 文档名称 | 文档路径 | 内容概述 |
|----------|----------|----------|
| 全局架构总览 | docs/overview.md | 核心思想、全局架构图、多Agent角色分工、前置条件 |
| 需求标准化转化 | docs/requirements-standardization.md | 模块1：需求校准流程、指南规范 |
| ...           | ...                                   | ...                              |

## 目录结构速览
（列出项目核心目录的职责说明，方便快速定位）

## 快速开始
（环境搭建、常用命令、注意事项）
```

**AGENTS.md 更新规则**：

- **触发时机**：新增/删除/重命名核心文档时，必须同步更新AGENTS.md索引；
- **责任主体**：执行变更的Agent负责同步更新，Agent-5（巡检Agent）负责校验一致性；
- **校验规则**：AGENTS.md中的文档路径必须真实存在，无悬空链接，否则判定为一般问题。

### 2.2 docs 目录完整结构

所有项目文档统一存放于项目根目录下的 `docs/` 目录，按职能划分子目录：

```
docs/
├── guide/                    # 指南文档目录（Agent执行依据）
│   ├── solution-design-guide.md             # 《方案设计文档编写指南》
│   ├── acceptance-criteria-guide.md         # 《验收标准文档编写指南》
│   ├── acceptance-report-guide.md           # 《验收报告编写指南》
│   ├── design-review-guide.md               # 《设计方案评审文档编写指南》
│   ├── design-alignment-guide.md            # 《设计方案对齐确认文档编写指南》
│   ├── design-alignment-evaluation-guide.md # 《方案对齐评估指南》
│   ├── requirement-calibration-guide.md     # 《需求校准指南》
│   └── doc-entropy-check-guide.md           # 《文档熵收敛巡检检查指南》
│
├── demand/                   # 需求产物目录（按需求ID组织）
│   ├── REQ-2026-001/
│   │   ├── PRD.md                           # 标准化需求文档
│   │   ├── design/
│   │   │   ├── architecture-V1.0.0.md       # 架构设计方案（初始版本）
│   │   │   └── architecture-V1.0.1.md       # 架构设计方案（迭代版本）
│   │   ├── review/
│   │   │   ├── peer-review-V1.0.0.md        # 平级评审文档
│   │   │   ├── security-review-V1.0.0.md    # 安全评审报告
│   │   │   ├── performance-review-V1.0.0.md # 性能评审报告
│   │   │   └── compliance-review-V1.0.0.md  # 合规评审报告
│   │   ├── acceptance/
│   │   │   ├── criteria-V1.0.0.md           # 验收标准文档
│   │   │   └── report-V1.0.0.md             # 验收报告
│   │   └── alignment/
│   │       └── alignment-V1.0.0.md          # 设计方案对齐确认文档
│   │
│   └── REQ-2026-002/
│       └── ...（结构同上）
│
├── overview.md               # 全局架构总览（核心思想、角色分工）
├── requirements-standardization.md  # 模块1：需求标准化转化
├── architecture-design-review.md    # 模块2：架构设计与多维度评审
├── development-alignment.md         # 模块3：开发对齐与代码实现
├── automated-acceptance.md          # 模块4：自动化验收与全流程闭环
├── doc-entropy-governance.md        # 模块5：文档熵收敛治理
├── knowledge-base-organization.md   # 模块6：知识库文档组织（本文件）
├── guide-self-iteration.md          # 模块6：指南与规约文档的自我迭代
├── unified-interaction-entry.md     # 模块7：统一交互入口集成方案
│
├── ARCHITECTURE.md           # 系统架构文档（项目级）
├── CONVENTIONS.md            # 编码规范文档（项目级）
├── TESTING.md                # 测试规范文档（项目级）
├── ENVIRONMENT.md            # 环境搭建文档（项目级）
│
├── slim/                     # 规约提案管理目录（减重+增重）
│   ├── proposals/
│   │   ├── SLIM-2026-001.md  # 减重提案（示例）
│   │   └── ENRICH-2026-001.md # 增重提案（示例）
│   ├── archived-rules/       # 已减重规则归档区
│   │   └── SLIM-2026-001-rule.md
│   ├── enacted-rules/        # 已增重规则快照区
│   │   └── ENRICH-2026-001-rule.md
│   └── slim-index.md         # 规约提案统一索引
│
└── reference/                # 参考文档目录
    ├── glossary.md            # 术语表
    ├── dependencies.md        # 依赖清单
    └── changelog.md           # 变更日志
```

### 2.3 目录职责说明

| 目录/文件 | 职责说明 | 维护主体 |
|-----------|----------|----------|
| `docs/guide/` | 存放所有Agent执行依据的指南文档，包括四大核心指南及配套指南 | 人类编写，Agent只读 |
| `docs/demand/` | 存放所有需求相关的中间产物文档，按需求ID划分子目录 | Agent产出，人类校验 |
| `docs/demand/{需求ID}/PRD.md` | 标准化需求文档，由Agent-0产出 | Agent-0 |
| `docs/demand/{需求ID}/design/` | 存放架构设计方案，支持多版本并存 | Agent-1 |
| `docs/demand/{需求ID}/review/` | 存放所有评审文档（平级评审、专项评审） | Agent-2 + 专家Agent |
| `docs/demand/{需求ID}/acceptance/` | 存放验收标准与验收报告 | Agent-4 |
| `docs/demand/{需求ID}/alignment/` | 存放设计方案对齐确认文档 | Agent-3 |
| `docs/slim/` | 规约提案管理目录，统一存放减重与增重两类提案及对应规则文件 | 人类主导，Agent辅助 |
| `docs/slim/proposals/` | 存放所有提案文件（SLIM-/ENRICH- 前缀区分），含审批记录 | 提案发起方填写，人类审批 |
| `docs/slim/archived-rules/` | 存放已减重规则归档文件，含规则原文、减重记录、恢复记录 | 人类操作，Agent只读 |
| `docs/slim/enacted-rules/` | 存放已增重规则快照文件，含规则正式文本、增重记录、废止记录 | 人类操作，Agent只读 |
| `docs/slim/slim-index.md` | 规约提案统一索引，同时追踪减重与增重提案状态 | 操作人更新，Agent-5 校验 |
| `docs/reference/` | 参考文档、术语表、变更日志等辅助资料 | 人类/Agent共同维护 |

### 2.4 文档命名规范

#### 需求产物文档命名规则

| 文档类型 | 命名格式 | 示例 |
|----------|----------|------|
| 标准化PRD | `PRD.md` | `PRD.md` |
| 架构设计方案 | `architecture-V{版本号}.md` | `architecture-V1.0.0.md` |
| 平级评审文档 | `peer-review-V{版本号}.md` | `peer-review-V1.0.0.md` |
| 安全评审报告 | `security-review-V{版本号}.md` | `security-review-V1.0.0.md` |
| 性能评审报告 | `performance-review-V{版本号}.md` | `performance-review-V1.0.0.md` |
| 合规评审报告 | `compliance-review-V{版本号}.md` | `compliance-review-V1.0.0.md` |
| 验收标准 | `criteria-V{版本号}.md` | `criteria-V1.0.0.md` |
| 验收报告 | `report-V{版本号}.md` | `report-V1.0.0.md` |
| 对齐确认文档 | `alignment-V{版本号}.md` | `alignment-V1.0.0.md` |

#### 版本号规则

- 格式：`V{主版本}.{次版本}.{修订号}`，例：`V1.0.0`、`V1.0.1`、`V1.1.0`
- 版本递进规则：
  - **主版本**：架构重大调整、需求边界变更；
  - **次版本**：功能新增、性能优化；
  - **修订号**：问题修复、文案完善。

### 2.5 文档索引与追溯机制

#### 版本映射表

项目根目录必须维护 `版本映射表.xlsx`，记录需求ID、架构版本、文档路径、Commit Hash的关联关系：

| 需求ID | 架构版本 | 文档类型 | 文档路径 | Commit Hash | 生效时间 |
|--------|----------|----------|----------|-------------|----------|
| REQ-2026-001 | V1.0.0 | 架构设计 | docs/demand/REQ-2026-001/design/architecture-V1.0.0.md | a1b2c3d4... | 2026-03-28 |
| REQ-2026-001 | V1.0.0 | 验收标准 | docs/demand/REQ-2026-001/acceptance/criteria-V1.0.0.md | a1b2c3d4... | 2026-03-28 |

#### 文档内部锚点要求

所有文档头部必须包含以下锚点信息（与版本映射表一致）：

```markdown
---
需求ID: REQ-2026-001
架构版本: V1.0.0
基准Commit: a1b2c3d4e5f6...
生效时间: 2026-03-28
上版迭代号: 无
---
```

### 2.6 异常处理与校验规则

| 异常场景 | 判定级别 | 处置方式 |
|----------|----------|----------|
| `docs/guide/` 或 `docs/demand/` 目录不存在 | 严重 | Agent-5自动预警，人类需在24小时内创建 |
| AGENTS.md索引路径与实际路径不一致 | 一般 | 限期整改 |
| 版本映射表未更新（新增需求产物后） | 一般 | 需在24小时内更新 |
| 未按需求ID创建子目录或目录命名不规范 | 严重 | 限期整改 |
