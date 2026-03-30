# 模块5：文档全链路版本联动与熵收敛治理

## 1. 目标定位

本模块核心解决「文档与代码脱节、全链路信息偏差」问题，通过双机制保障（Git Hook强卡点 + 专属巡检Agent兜底）实现文档版本与代码版本的持续联动同步，收敛长期累积的熵增。主要解决以下问题：

- 文档修改后未同步更新关联文档，导致信息不一致；
- 版本号未及时递进，新功能基于旧文档开发；
- 产出物无锚点绑定，无法追溯变更根因；
- 文档与代码偏差静默扩散，发现时整改成本极高。

## 2. 涉及的 Agent

| Agent | 角色 | 职责 |
|-------|------|------|
| Agent-5（文档熵收敛巡检Agent） | 执行主体 | 执行全量/增量文档巡检，生成巡检报告，预警和处置偏差 |
| Git Hook（校验引擎） | 自动门禁 | 在pre-commit/pre-push/合并阶段自动触发校验，阻断不合规提交 |
| 人类（技术负责人） | 规则制定 + 例外审批 | 制定校验规则，审批例外申请，复核致命/严重问题 |

## 3. 触发前置条件

本模块是长效保障机制，贯穿全流程，触发时机包括：

- **pre-commit触发**：单文件（文档/代码）变更提交时，触发锚点一致性基础校验；
- **pre-push触发**：文件推送至分支时，触发全链路关联校验；
- **主干合并触发**：文件合并至主干时，触发增量联动校验；
- **定时触发**：每日22:00，Agent-5触发全量基线巡检。

## 4. 输入与输出

### 输入

| 输入项 | 来源 | 说明 |
|--------|------|------|
| 全链路产出物 | `docs/demand/`目录下所有文档 | PRD、设计方案、评审报告、验收标准、验收报告等 |
| 代码仓库 | Git仓库 | 所有代码文件及Commit记录 |
| 《文档熵收敛巡检检查指南》 | `docs/guide/doc-entropy-check-guide.md` | Agent-5的核心执行依据 |
| 版本映射表 | `版本映射表.xlsx`（项目根目录） | 需求ID→架构版本→文档路径→Commit Hash映射 |

### 输出

| 输出物 | 格式 | 归档路径 |
|--------|------|----------|
| 《文档熵收敛巡检报告》 | Markdown | `docs/demand/inspection/报告-YYYYMMDD-全量/增量.md` |
| 致命/严重问题预警 | 系统弹窗 + 通知 | 推送至责任人 |

### 与其他模块的标准化对接

- **跨模块**：本模块不依赖单一上游，监控所有模块产出物的一致性；
- **输出使用方**：所有Agent和人类均可查看巡检报告，对应Agent负责整改。

## 5. 核心落地机制

### 5.1 机制1：Git Hook（刚性拦截）

**触发时机**：任何Agent/人类修改文档（PRD、设计方案、验收标准）或代码时，提交/推送瞬间触发。

**校验内容**：当前修改文件绑定的需求ID、架构版本、Commit信息，是否与全链路关联文档一致。

**处置逻辑**：若发现关联文档未同步、版本错位、ID不一致 → 直接阻断提交，弹窗提示"需同步修改关联文档（例：修改PRD后，需同步更新架构设计方案V1.0.1）"，强制完成对齐后，方可提交。

### 5.2 机制2：Agent-5兜底巡检

**触发方式**：
- 每日固定时间（如22:00）执行全量扫描；
- 任何文档/代码合并至主干时，自动触发增量扫描。

**校验内容**：严格遵循《文档熵收敛巡检检查指南》，校验全链路产出物的锚点一致性、版本递进、变更联动。

**处置逻辑**：生成《文档熵偏离巡检报告》，标红差异项、关联责任人（对应Agent）、给出具体修改建议；致命/严重项自动预警，进入人工复核池；一般/优化项仅预警，纳入下次迭代整改。

## 6. 巡检核心校验标准

### 6.1 刚性校验基线（必须满足，否则触发预警/拦截）

- **ID唯一绑定规则**：所有产出物（文档/代码/测试用例）必须绑定1个有效需求ID，禁止无ID、多ID、过期ID、伪造ID；
- **版本递进规则**：架构版本升级（如V1.0.0→V1.0.1），所有关联的设计文档、验收标准、测试用例必须同步升级版本号，旧版本自动标灰归档，禁止基于旧版本产出新内容；
- **Commit锚点规则**：设计文档标注的基准Commit，必须与主干当前生效Commit一致；代码内注释、接口契约，必须能回溯到对应架构文档段落；
- **变更联动规则**：修改以下任意一项，全链路关联文档必须同步复核修改：
  - 业务边界增减；
  - 非功能指标调整；
  - 核心数据结构/接口字段变更；
  - 验收标准阈值修改。

### 6.2 问题分级标准

| 问题级别 | 判定标准 | 整改时限 | 责任主体 |
|----------|----------|----------|----------|
| 致命 | 需求ID错位、核心变更未同步、Commit锚点篡改 | 24小时内 | 对应Agent + 人类复核 |
| 严重 | 版本号不递进、关联文档漏更新 | 48小时内 | 对应执行Agent |
| 一般 | 备注/说明类文案不同步 | 下次迭代前 | 对应执行Agent |
| 优化 | 格式、索引、引用排版不规范 | 无强制时限 | 对应执行Agent |

## 7. 《文档熵收敛巡检检查指南》（完整可落地版）

本指南为Agent-5与Git Hook校验引擎的核心约束手册，纳入项目规则库，全文可直接复制使用。

### 7.1 总则与适用范围

1.1 **目的**：管控需求、设计、文档、代码的信息偏差，收敛全流程熵增，确保全链路产出物一致、可追溯，支撑多Agent协同落地；

1.2 **适用范围**：本文档适用于本项目所有Agent产出的文档（PRD、架构设计、验收标准等）、代码、单元测试用例、评审报告，以及人类修改的所有相关产出物；

1.3 **责任主体**：Agent-5（文档熵收敛巡检Agent）负责执行巡检，人类负责制定规则、复核严重问题、审批例外情况。

### 7.2 基础绑定规范（强制执行）

**2.1 文档绑定要求**：所有文档头部必须包含以下必填字段（不可遗漏、不可篡改）：

```markdown
---
需求ID: REQ-2026-001
关联架构版本: V1.0.0
基准Commit: a1b2c3d4e5f6...（完整64位）
生效时间: YYYY-MM-DD
上版迭代号: 无（初始版本填"无"，迭代版本填上一版本号）
---
```

**2.2 代码绑定要求**：代码仓库根目录必须存放"版本映射表.xlsx"，记录需求ID→架构版本→文档路径→Commit Hash的关联关系，每次变更后自动更新；

**2.3 归档规范**：所有产出物按"需求ID/文档类型"分类归档（例：`REQ-2026-001/PRD`、`REQ-2026-001/架构设计`），禁止乱堆乱放。

### 7.3 触发校验时机规范

| 触发时机 | 校验范围 | 校验类型 |
|----------|----------|----------|
| pre-commit | 当前变更文件 | 锚点一致性基础校验（需求ID、版本号） |
| pre-push | 当前文件及所有关联文档/代码 | 全链路关联校验 |
| 主干合并 | 变更部分及关联文档 | 增量联动校验 |
| 每日22:00 | 所有产出物 | 全量基线巡检 |

### 7.4 逐条校验规则明细（Agent可直接解析）

| 规则编号 | 规则内容 | 问题分级 |
|----------|----------|----------|
| 规则001 | 所有产出物必须绑定唯一有效需求ID，无ID、多ID、过期ID、伪造ID，均判定为致命问题 | 致命 |
| 规则002 | 架构版本升级后，关联的设计文档、验收标准、测试用例必须在24小时内同步升级版本号 | 严重 |
| 规则003 | 设计文档标注的基准Commit，与主干当前生效Commit不一致 | 严重 |
| 规则004 | 修改业务边界、非功能指标、核心数据结构、验收标准后，未同步修改关联文档/代码 | 致命 |
| 规则005 | 代码内注释、接口契约，无法回溯到对应架构文档段落 | 一般 |
| 规则006 | 文档格式、索引、引用排版不规范（如字段缺失、排版混乱） | 优化 |
| 规则007 | 旧版本文档未标灰归档，仍被用于新增功能开发 | 严重 |
| 规则008 | 版本映射表未及时更新（新增需求产物后未同步） | 一般 |

### 7.5 问题分级与处置流程

| 问题分级 | 处置方式 | 整改时限 | 责任主体 |
|----------|----------|----------|----------|
| 致命 | 阻断提交/合并，自动预警，进入人工复核池，强制整改 | 24小时内 | 对应执行Agent + 人类复核 |
| 严重 | 预警提示，禁止基于旧文档新增功能，限期整改 | 48小时内 | 对应执行Agent |
| 一般 | 仅预警，不阻断，纳入下次迭代统一整改 | 下次迭代前 | 对应执行Agent |
| 优化 | 仅预警，不阻断，按需整改 | 无强制时限 | 对应执行Agent |

### 7.6 巡检报告输出规范

**6.1 报告模板（固定格式，不可修改）**：

- 标题：`《文档熵收敛巡检报告-巡检时间-需求ID（全量/增量）》`（例：`《文档熵收敛巡检报告-20260328-全量》`）；
- 核心内容：
  1. 巡检概况（巡检范围、巡检时长、异常项总数）；
  2. 差异清单（问题分级、问题描述、关联产出物路径、关联需求ID）；
  3. 整改建议（具体修改操作、责任Agent）；
  4. 整改时限。

**6.2 报告归档**：巡检报告绑定对应需求ID（全量报告绑定"全量"标识），上传至产出物仓库"巡检报告"目录，可追溯、可查询；

**6.3 预警方式**：致命/严重问题通过系统弹窗+邮件通知人类，一般/优化问题仅在报告中标注。

### 7.7 例外审批规则（仅人类可操作）

**7.1 例外场景**（仅以下场景可申请例外，其余场景不可豁免）：

- 极小文案优化（如修改文档错别字），不影响核心内容与关联关系；
- 非关联注释调整（如代码内注释优化，不涉及逻辑与接口）。

**7.2 审批流程**：人类提交例外申请（注明申请理由、关联产出物、修改内容），记录豁免记录，留痕审计，豁免有效期最长为7天，过期自动失效。

## 8. slim-index 校验规则（Git Hook实现）

以下规则用于校验 `docs/slim/slim-index.md` 的一致性，由Git Hook在pre-commit阶段自动执行：

| 规则编号 | 规则内容 | 问题分级 |
|----------|----------|----------|
| S-001 | 索引中每条记录的提案ID，必须在 `docs/slim/proposals/` 下存在对应文件 | 致命 |
| S-002 | 索引中状态为"已减重"或"已恢复"的记录，必须在 `docs/slim/archived-rules/` 下存在对应归档文件 | 严重 |
| S-003 | 索引"当前状态"字段仅允许合法值：减重提案（审批中/已减重/已恢复/已驳回），增重提案（审批中/已生效/已驳回） | 一般 |
| S-004 | 状态为"已减重"/"已生效"时生效时间不可为空；状态为"已恢复"时最近恢复时间不可为空 | 严重 |
| S-005 | `slim-index.md` 的历史记录行不可删除，只可追加或修改状态字段 | 致命 |
| S-006 | 索引"当前状态"须与对应提案文件中"审批结论"字段保持一致 | 严重 |

### 8.1 Git Hook 校验脚本

以下脚本实现 S-001 至 S-006 全部校验规则，放置于 `.git/hooks/pre-commit`，赋予可执行权限（`chmod +x .git/hooks/pre-commit`）后自动生效：

```bash
#!/usr/bin/env bash
# slim-index 校验脚本 —— 实现规则 S-001 至 S-006
# 仅在本次提交涉及 docs/slim/ 下的文件时触发

set -euo pipefail

SLIM_DIR="docs/slim"
PROPOSALS_DIR="${SLIM_DIR}/proposals"
ARCHIVED_DIR="${SLIM_DIR}/archived-rules"
INDEX_FILE="${SLIM_DIR}/slim-index.md"

# 检查本次提交是否涉及 slim 相关文件，无关则跳过
CHANGED=$(git diff --cached --name-only)
if ! echo "${CHANGED}" | grep -q "^docs/slim/"; then
  exit 0
fi

ERRORS=()    # 致命/严重问题，阻断提交
WARNINGS=()  # 一般问题，阻断提交（按规则 S-003）

# ── 工具函数 ──────────────────────────────────────────────

# 从 slim-index.md 提取数据行（跳过表头与分隔行，同时匹配 SLIM- 与 ENRICH-）
parse_index_rows() {
  grep -E "^\| (SLIM|ENRICH)-" "${INDEX_FILE}" 2>/dev/null || true
}

# 从提案文件中提取"审批结论"字段值
get_proposal_conclusion() {
  local proposal_file="$1"
  grep -E "^- 审批结论：" "${proposal_file}" 2>/dev/null \
    | sed 's/- 审批结论：//' \
    | sed 's/（如适用）//' \
    | tr -d ' ' \
    | head -1
}

# 将审批结论映射为索引状态（按提案类型分别映射）
map_conclusion_to_status() {
  local conclusion="$1"
  local proposal_id="$2"
  if [[ "${proposal_id}" == ENRICH-* ]]; then
    case "${conclusion}" in
      "通过")    echo "已生效" ;;
      "驳回"*)   echo "已驳回" ;;
      "延期"*)   echo "审批中" ;;
      "")        echo "审批中" ;;
      *)         echo "UNKNOWN" ;;
    esac
  else
    case "${conclusion}" in
      "通过")    echo "已减重" ;;
      "驳回"*)   echo "已驳回" ;;
      "延期"*)   echo "审批中" ;;
      "已恢复")  echo "已恢复" ;;
      "")        echo "审批中" ;;
      *)         echo "UNKNOWN" ;;
    esac
  fi
}

# ── S-005：禁止删行检测 ────────────────────────────────────
if git show HEAD:"${INDEX_FILE}" > /tmp/slim_index_old.md 2>/dev/null; then
  OLD_COUNT=$(grep -cE "^\| (SLIM|ENRICH)-" /tmp/slim_index_old.md || echo 0)
  NEW_COUNT=$(grep -cE "^\| (SLIM|ENRICH)-" "${INDEX_FILE}" || echo 0)
  if [ "${NEW_COUNT}" -lt "${OLD_COUNT}" ]; then
    ERRORS+=("[致命][S-005] slim-index.md 记录行数减少（原 ${OLD_COUNT} 行，现 ${NEW_COUNT} 行），禁止删除历史记录")
  fi
fi

# ── 逐行校验 S-001 至 S-004、S-006 ───────────────────────
while IFS='|' read -r _ proposal_id proposal_type target_rule proposal_time status effect_time restore_time _; do
  proposal_id=$(echo "${proposal_id}" | tr -d ' ')
  proposal_type=$(echo "${proposal_type}" | tr -d ' ')
  status=$(echo "${status}" | tr -d ' ')
  effect_time=$(echo "${effect_time}" | tr -d ' ')
  restore_time=$(echo "${restore_time}" | tr -d ' ')

  [ -z "${proposal_id}" ] && continue

  proposal_file="${PROPOSALS_DIR}/${proposal_id}.md"
  archived_file="${ARCHIVED_DIR}/${proposal_id}-rule.md"

  # S-001：提案文件存在性
  if [ ! -f "${proposal_file}" ]; then
    ERRORS+=("[致命][S-001] ${proposal_id}：提案文件不存在（期望路径：${proposal_file}）")
  fi

  # S-002：归档文件一致性（减重提案才需要归档规则文件）
  if [[ "${proposal_type}" == "减重" ]]; then
    if [[ "${status}" == "已减重" || "${status}" == "已恢复" ]]; then
      if [ ! -f "${archived_file}" ]; then
        ERRORS+=("[严重][S-002] ${proposal_id}：减重提案状态为\"${status}\"但归档规则文件不存在（期望路径：${archived_file}）")
      fi
    fi
  fi

  # S-003：状态合法性
  if [[ "${proposal_type}" == "减重" ]]; then
    case "${status}" in
      "审批中"|"已减重"|"已恢复"|"已驳回") ;;
      *) WARNINGS+=("[一般][S-003] ${proposal_id}：减重提案状态值\"${status}\"非法") ;;
    esac
  elif [[ "${proposal_type}" == "增重" ]]; then
    case "${status}" in
      "审批中"|"已生效"|"已驳回") ;;
      *) WARNINGS+=("[一般][S-003] ${proposal_id}：增重提案状态值\"${status}\"非法") ;;
    esac
  fi

  # S-004：时间字段完整性
  if [[ "${status}" == "已减重" || "${status}" == "已生效" ]]; then
    if [[ -z "${effect_time}" || "${effect_time}" == "-" ]]; then
      ERRORS+=("[严重][S-004] ${proposal_id}：状态为\"${status}\"但生效时间为空")
    fi
  fi
  if [[ "${status}" == "已恢复" && ( -z "${restore_time}" || "${restore_time}" == "-" ) ]]; then
    ERRORS+=("[严重][S-004] ${proposal_id}：状态为\"已恢复\"但最近恢复时间为空")
  fi

  # S-006：索引状态与提案文件审批结论一致性
  if [ -f "${proposal_file}" ]; then
    conclusion=$(get_proposal_conclusion "${proposal_file}")
    expected_status=$(map_conclusion_to_status "${conclusion}" "${proposal_id}")
    if [[ "${expected_status}" != "UNKNOWN" && "${expected_status}" != "${status}" ]]; then
      ERRORS+=("[严重][S-006] ${proposal_id}：索引状态\"${status}\"与提案文件审批结论\"${conclusion}\"（期望状态：${expected_status}）不一致")
    fi
  fi

done < <(parse_index_rows)

# ── 输出结果 ──────────────────────────────────────────────
if [ ${#WARNINGS[@]} -gt 0 ]; then
  echo ""
  echo "【slim-index 校验】发现以下问题（一般）："
  for w in "${WARNINGS[@]}"; do
    echo "  ⚠  ${w}"
  done
fi

if [ ${#ERRORS[@]} -gt 0 ]; then
  echo ""
  echo "【slim-index 校验】发现以下问题（致命/严重），提交已阻断："
  for e in "${ERRORS[@]}"; do
    echo "  ✗  ${e}"
  done
  echo ""
  echo "请修复上述问题后重新提交。"
  exit 1
fi

if [ ${#WARNINGS[@]} -gt 0 ]; then
  echo ""
  echo "上述一般问题不阻断本次提交，请在下次迭代前修复。"
  exit 1
fi

exit 0
```

脚本部署说明：

- **路径**：`.git/hooks/pre-commit`
- **权限**：`chmod +x .git/hooks/pre-commit`
- **依赖**：仅依赖 bash / grep / sed，无需额外安装工具

### 8.2 pre-commit 框架集成配置示例

若项目已引入 [pre-commit](https://pre-commit.com) 框架，推荐将校验脚本封装为 `local` 类型 hook：

```yaml
# .pre-commit-config.yaml

repos:
  - repo: local
    hooks:
      - id: slim-index-check
        name: "slim-index 校验（S-001 至 S-006）"
        language: script
        entry: scripts/hooks/slim-check.sh
        files: ^docs/slim/
        pass_filenames: false
        stages: [pre-commit]
        verbose: true
```

## 9. 异常处理

| 异常场景 | 处置方式 |
|----------|----------|
| 巡检发现致命问题 | Agent-5自动阻断相关提交/合并，预警人类，对应Agent在24小时内整改，人类复核通过后解除阻断 |
| 关联文档同步不及时 | Agent-5自动提醒对应Agent，逾期未整改的升级为严重问题，触发人类介入 |
| 例外申请审批 | 人类需在24小时内完成审批，审批结果同步至Agent-5，纳入巡检记录 |

## 10. 模块输出验收标准

本模块持续运行的验收标准：

- [ ] Git Hook已配置并生效（pre-commit、pre-push均可自动触发）；
- [ ] Agent-5已配置定时触发（每日22:00），全量巡检正常运行；
- [ ] 巡检报告按规范格式归档，无悬空链接；
- [ ] 致命/严重问题均在规定时限内完成整改；
- [ ] slim-index.md 通过 S-001 至 S-006 全部校验规则。
