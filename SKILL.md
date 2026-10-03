---
name: context-handoff
version: 0.1
status: validated
description: 将一个或多个 AI / 智能体已经发生的复杂工作历史，整理为可验证、可追溯、可安全继续接手的标准化上下文交接包。
---

# context-handoff Skill V0.1

## 1. Purpose

本 Skill 用于：

**把复杂、多轮、跨会话、跨智能体的工作历史，转换为可以被下一位 AI 快速理解，同时能够反向回查原始证据的 context-handoff 包。**

适用场景包括：

- ChatGPT → ChatGPT
- ChatGPT → Codex
- Codex → ChatGPT
- Codex → Codex
- 其他具有可导出历史或结构化日志的 AI / Agent 之间的交接

本 Skill 的目标不是：

- 替用户做业务裁决；
- 替代原始证据；
- 自动认定 CLOSED / REOPEN；
- 自动修改项目文件；
- 自动施工；
- 用摘要覆盖真实历史。

---

## 2. Core Principle

必须始终遵守：

**Evidence first. Summary second.**

即：

**原始证据 > 硬锚点 > 时间轴 > 阶段解释 > README > 精炼 handoff**

同时必须区分：

### 快速接手阅读顺序

用于新 AI 快速恢复工作基线：

`handoff → README → 时间轴 → 硬锚点 → 原始证据`

### 严格证据回查顺序

用于具体事实核验、施工前确认和争议裁决：

`原始证据 → 硬锚点 → 时间轴 → 阶段解释 → README / handoff`

两者不得混为一谈。

---

## 3. Trigger

当用户出现以下需求之一时，可考虑使用本 Skill：

- “帮我把这段历史交给下一位 AI”
- “整理跨会话上下文”
- “我换新聊天了，不想重新解释”
- “把 ChatGPT 和 Codex 的工作串起来”
- “做一个 handoff”
- “生成剪贴板交接包”
- “整理记忆锚点”
- “让另一个 AI 快速接手”
- “把历史沉淀成可验证上下文”
- “建立跨智能体工作时间轴”

如果用户只是要求普通聊天摘要，不应自动升级为完整 context-handoff 流程。
---

## 4. Inputs

可接受输入包括：

### ChatGPT

优先：

- 官方数据导出
- conversations JSON
- 会话附件
- 用户明确提供的聊天文本

### Codex

优先：

- rollout JSONL
- session JSONL
- 用户明确提供的 Codex 记录
- 审计 / 完成报告

### 其他 Agent

要求至少能够获得：

- 原始文本
- 时间
- role / author
- source identity

### 用户业务证据

例如：

- Markdown
- CSV
- Excel
- 日志
- 完成报告
- 裁定书
- SHA256
- 项目治理文件

不得把用户未提供、也无法验证的历史自动补齐。

---

## 5. Read-Only Rule

默认工作方式：

**只读。**

未经用户明确授权，不应：

- 修改原始聊天导出；
- 修改 Codex 原始 session；
- 修改业务资产；
- 删除源文件；
- 重命名源文件；
- 覆盖旧版本 handoff；
- 修改已经通过测试并形成 SHA256 的历史版本。

所有派生文件应写入：

**独立工作目录。**

---

## 6. Workflow

标准流程分为 9 个阶段。

### Stage 0｜Identify Sources

识别所有相关来源。

记录：

- Source type
- File name
- Path
- Size
- Timestamp
- Hash（如适用）

禁止直接开始摘要。

### Stage 1｜Extract Visible Evidence

从原始来源中提取真正参与用户与 AI 沟通的正文。

#### ChatGPT

若存在树状 `mapping`：

优先：

`current_node → parent`

恢复当前有效分支。

用户正文优先保留：

- text
- multimodal_text

assistant 正文优先保留：

- text

不得把以下内容自动作为正常可见聊天：

- thoughts
- reasoning_recap
- 隐藏内部节点
- 工具内部状态

#### Codex

保留至少：

- source file
- line number
- timestamp
- role
- Message ID（如存在）
- text / preview
### Stage 2｜Build Index

建立正式索引。

至少包含：

- Conversation / Session title
- Conversation ID
- Message ID
- role
- timestamp
- first message
- last message
- message count
- source pointer

同名标题必须依赖唯一 ID 区分。

---

### Stage 3｜Find Candidate Boundaries

寻找可能改变后续接手结论的阶段边界。

典型关键词：

- REOPEN
- CLOSED
- PASS
- FAIL
- HOLD
- BLOCKER
- takeover
- audit
- implementation
- final review
- completed
- root cause
- inheritance
- rollback

候选边界只是候选。

不得直接升格为最终阶段结论。

---

### Stage 4｜Select Hard Anchors

从候选边界中选择少量真正决定后续接手方向的事件。

硬锚点优先包括：

1. 状态变化
2. 角色变化
3. 施工责任转移
4. 审计责任变化
5. BLOCKER 出现 / 解除
6. REOPEN / CLOSED
7. 人工最终确认
8. 根因确认
9. 治理原则变化
10. 新工作模式起源

每个硬锚点必须能够回查：

- 原始文件
- 时间
- Message ID / line
- role

不能只保存人工总结。

---

### Stage 5｜Build Cross-Agent Timeline

若存在多个 Agent：

建立统一时间轴。

至少包含：

- Time
- Agent
- Role
- Phase
- Meaning
- Message ID
- Source file
- Source pointer

特别检查：

- 谁在实施；
- 谁在审计；
- 谁在独立终审；
- 谁只有人工确认权；
- 角色切换的精确时间。

禁止把：

**实施者 = 独立终审者**

作为默认假设。

---

### Stage 6｜Define Phases

根据硬锚点建立：

- P0
- P1
- P2
- ...

每个阶段至少说明：

- Start
- End
- Name
- Agent roles
- Key evidence
- Exit condition

注意：

阶段属于：

**解释层。**

不是原始事实层。
---

### Stage 7｜Generate Handoff Package

标准输出建议包括：

#### Evidence

- visible evidence
- pre-window anchors
- source index

#### Navigation

- conversation index
- first / last anchors

#### Hard Evidence

- ChatGPT hard anchors
- Codex hard anchors
- cross-agent timeline

#### Interpretation

- phase definition
- README

#### Delivery

- concise `context-handoff.md`

#### Verification

- SHA256 Manifest
- blind-test instructions
- blind-test result

---

### Stage 8｜Blind Test

至少做一次：

**陌生 AI 接手测试。**

推荐做两轮。

#### Test A

普通独立新聊天。

目标：

验证内容本身是否足以恢复正确工作基线。

#### Test B

推荐：

**Temporary Chat + Non-Personalized**

目标：

在 UI 明确确认的隔离环境下再次验证。

---

## 7. EVIDENCE INSUFFICIENT

这是强制规则。

如果证据不足以支持某项事实：

必须输出：

`EVIDENCE INSUFFICIENT`

而不是：

- 推测；
- 补齐；
- 猜代码；
- 猜路径；
- 猜状态；
- 猜 CLOSED；
- 猜 REOPEN；
- 猜谁批准了什么。

`EVIDENCE INSUFFICIENT`

不是错误。

它是本 Skill 的安全机制。
---

## 8. Human Authority

若项目存在：

- 馆长
- Owner
- Maintainer
- Human approver
- Final authority

则必须明确区分：

### Machine-derived fact

机器可以产生或验证的事实。

例如：

- 文件是否存在；
- SHA256；
- 文件数量；
- 时间戳；
- 程序运行结果；
- 明确可读取的公式或字段；
- 日志中的实际返回值。

### AI interpretation

AI 根据现有证据形成的解释。

例如：

- 某问题是否属于公共实现；
- 某阶段是否可能已经具备完成条件；
- 某事件是否适合作为硬锚点；
- 某个时间窗口是否构成阶段边界。

AI interpretation 必须能够说明：

- 依据什么证据；
- 哪部分属于事实；
- 哪部分属于解释。

### Human confirmation

只有人工明确发生后才成立的确认。

例如：

- 最终接受；
- 人工确认 CLOSED；
- 授权实施；
- 授权独立终审；
- 人工验收某个结果件；
- 正式宣布阶段完成。

AI 不得把：

“建议确认”

写成：

“已经人工确认”。

也不得把：

“机器通过”

自动升级为：

“人工已接受”。

---

## 9. Closed-State Protection

如果某阶段已经有充分证据形成：

- CLOSED
- PASS
- completed
- approved
- accepted

后续发现新问题时：

不能自动把整个历史状态推翻。

必须先判断：

1. 新问题是否属于后续真实运行问题；
2. 是否只是局部问题；
3. 是否属于实例差异；
4. 是否属于公共实现；
5. 是否真正破坏此前完成条件；
6. 是否存在新的正式 REOPEN 证据；
7. 是否需要人工重新裁定。

禁止因为出现：

- 新 bug；
- 新显示问题；
- 新运行差异；
- 新修复任务

就自动得出：

“此前全部完成状态无效”。

如果证据不足：

`EVIDENCE INSUFFICIENT`

---

## 10. Versioning

任何已经：

- 通过测试；
- 生成 SHA256；
- 被人工确认；
- 被用于真实交接

的 handoff 文件，

原则上：

**禁止无痕覆盖。**

后续改进应形成新版本，例如：

- V0.2
- V0.3
- V1.0

同时保留：

- 原版本；
- 原 SHA256；
- 原测试结果；
- 修改原因；
- 新版本新增或修正内容。

必须能够回答：

> 哪一个版本通过了哪一次测试？

以及：

> 当前交接所依据的是哪一个版本？

---

## 11. Manifest

每个重要冻结点建议生成：

**SHA256 Manifest**

至少记录：

- FileName
- SizeBytes
- LastWriteTime
- SHA256

Manifest 的作用包括：

- 防止交接包身份漂移；
- GitHub 迁移后的核验；
- 新电脑迁移后的核验；
- 确认测试针对的是哪一版文件；
- 发现文件被意外修改；
- 为后续审计提供可重复验证的身份锚点。

Manifest 本身也应生成 SHA256。

若后续新增文件：

不应无痕改写历史 Manifest。

可生成新的：

- Manifest V2
- Final Manifest
- Release Manifest

保留旧 Manifest 作为阶段证据。

---

## 12. Minimum Handoff Contents

一个可以交给陌生 AI 的最小 handoff，至少应包含：

1. 当前总体状态
2. 时间窗口
3. 关键角色
4. 角色变化
5. 已闭合事实
6. 不得回退的口径
7. 当前未完成内容
8. 下一步安全方向
9. 证据等级
10. 原始证据入口
11. 快速接手阅读顺序
12. 严格证据回查顺序
13. EVIDENCE INSUFFICIENT 规则

如果存在多个 Agent，还应至少说明：

- 谁负责实施；
- 谁负责审计；
- 谁负责独立终审；
- 谁具有人工确认权；
- 角色切换发生在什么时间；
- 哪些职责已经结束；
- 哪些职责仍然有效。

最小 handoff 的目标不是：

“把所有历史重新讲一遍”。

而是让陌生 AI 能够回答：

> 我现在在哪里？

> 我为什么会来到这里？

> 哪些事情已经完成？

> 哪些事情不能重新争论？

> 哪些事情仍需要核查？

> 如果证据不足，我应该去哪里查？
---

## 13. Blind-Test Safety Gates

盲测不能只判断：

“回答写得像不像。”

必须设置明确安全门。

### Gate 1｜审计期不能误写为施工期

如果证据显示某 Agent 当时只是：

- audit
- review
- read-only review
- independent audit

不得因为后来该 Agent 参与实施，就反向改写早期职责。

### Gate 2｜施工期不能继续误写为审计期

如果存在明确 takeover / implementation 委托：

必须记录角色切换。

不得继续沿用：

“该 Agent 只是审计者”

这一过期口径。

### Gate 3｜实施者不能自动等同于独立终审者

如果项目要求独立终审：

必须明确：

- 实施者；
- 审计者；
- 独立终审者；

是否为同一主体。

不得默认：

**谁做完，谁就自动有独立终审资格。**

### Gate 4｜后续修复不能自动推翻既有完成里程碑

当已经存在：

- CLOSED
- PASS
- completed
- accepted
- human confirmed

后续出现 bug 或运行问题时：

必须先判断问题性质。

不得自动宣布：

“此前完成状态全部无效。”

### Gate 5｜handoff 不能成为最高证据

handoff 的作用是：

**导航与恢复基线。**

不是：

**替代底层证据。**

若 handoff 与底层证据冲突：

以底层证据为准。

### Gate 6｜证据不足必须停止推断

若材料不足：

必须能够主动输出：

`EVIDENCE INSUFFICIENT`

而不是为了完成任务而：

- 猜路径；
- 猜文件；
- 猜代码；
- 猜状态；
- 猜责任人；
- 猜是否 CLOSED；
- 猜是否已人工确认。

### Gate Failure Rule

若任一关键 Gate 失败：

即使：

- 语言流畅；
- 摘要漂亮；
- 看起来“很懂项目”；

也应优先判定：

**FAIL**

或：

**需要补证后重新测试**

安全边界高于文字表现。

---

## 14. Output Style

context-handoff 输出应优先：

- 简洁；
- 明确；
- 可执行；
- 可追溯；
- 时间顺序清楚；
- Agent 角色清楚；
- 事实与解释分开；
- 当前状态与历史状态分开。

### 推荐表达

优先写：

- “原始证据显示……”
- “当前证据支持……”
- “该结论属于解释层……”
- “尚未发现足够证据……”
- “EVIDENCE INSUFFICIENT”
- “需要回查 source pointer……”

### 避免

- 大段文学化总结；
- 重复叙事；
- 无来源断言；
- 把推测写成事实；
- 为了完整而补写未知内容；
- 把历史所有细节都塞进精炼 handoff；
- 把 handoff 写成新的长聊天存档。

Skill 的价值不是：

“重新复制历史。”

而是：

**压缩阅读量，同时保留证据回查能力。**

---

## 15. Recommended Repository Layout

最终 GitHub 仓库建议结构：

context-handoff/
├─ SKILL.md
├─ README.md
├─ scripts/
│  ├─ extract_chatgpt.ps1
│  ├─ extract_codex.ps1
│  ├─ build_index.ps1
│  ├─ build_anchors.ps1
│  ├─ build_timeline.ps1
│  ├─ build_handoff.ps1
│  └─ verify_handoff.ps1
├─ templates/
│  ├─ handoff_template.md
│  ├─ blind_test_A.md
│  └─ blind_test_B.md
├─ schema/
│  └─ context_handoff_schema.json
├─ tests/
└─ examples/
   └─ sample_001/

### SKILL.md

用于：

- Skill 执行规则；
- 证据原则；
- 工作流；
- 安全边界。

### README.md

用于：

- 人类阅读；
- 项目定位；
- 使用说明；
- 快速入门。

### scripts/

用于逐步把人工验证过的流程沉淀成自动化脚本。

自动化脚本不得绕过：

- Evidence first
- EVIDENCE INSUFFICIENT
- Human Authority
- Versioning

等核心治理原则。

### templates/

保存：

- handoff 模板；
- 盲测模板；
- 后续可复用交接结构。

### schema/

用于定义：

- anchor；
- timeline；
- source pointer；
- phase；
- evidence level

等结构化字段。

### examples/sample_001/

用于保存第一套真实验证样本。

真实业务中的敏感原始资产是否进入 GitHub：

必须由用户另行决定。

Skill 不应默认把所有原始聊天或业务资产上传到 GitHub。

---

## 16. Validation Before Release

本 Skill 的新版本在正式发布前：

`SKILL.md`

之前，至少验证以下项目。

### Test 1｜Evidence Layer

陌生 AI 是否理解：

- 原始证据是什么；
- 硬锚点是什么；
- 解释层是什么。

不得把三者混淆。

### Test 2｜Reading Order

是否能够正确区分：

**快速接手阅读顺序**

与：

**严格证据回查顺序**

### Test 3｜Evidence Insufficient

面对缺失信息时：

是否能够主动输出：

`EVIDENCE INSUFFICIENT`

而不是猜测。

### Test 4｜Role Transition

给出一个包含角色变化的新样本时：

是否能够识别：

- 原角色；
- 切换时间；
- 新角色；
- source pointer。

### Test 5｜Closed-State Protection

面对：

“完成以后又出现新问题”

的场景，

是否能够避免自动推翻此前完成状态。

### Test 6｜Human Authority

是否能够区分：

- machine-derived fact；
- AI interpretation；
- human confirmation。

不得把建议、推测或机器结果改写成人工最终确认。

### Test 7｜New Sample Transfer

最重要的一项：

给候选 Skill 一个：

**不同于 sample_001 的陌生小样本**

看它能否按照流程自行设计：

- source index；
- hard anchors；
- timeline；
- phase；
- handoff；
- blind test。

若只能复述原始真实样本 sample_001：

说明 Skill 尚未真正通用化。

### Promotion Rule

若上述任一关键测试失败：

候选 Skill：

**不得升格。**

应先：

1. 找到失败原因；
2. 修正候选版本；
3. 形成新版本；
4. 重新测试。

只有通过验证后，才生成正式：

`SKILL.md`
---

## 17. sample_001 Status

第一套真实样本：

`real_validation_sample_001`

已经完成：

- ChatGPT 原始导出解析；
- ChatGPT 当前有效分支恢复；
- 3658 条正式窗口可见消息提取；
- 6 条前置锚点提取；
- 15 条目标会话正式索引；
- ChatGPT 用户硬锚点；
- Codex 用户硬锚点；
- 跨智能体统一时间轴；
- P0～P5 阶段定义；
- SHA256 Manifest；
- README；
- 精炼 context-handoff；
- Blind Test A；
- Blind Test B；
- Blind Test A / B 正式裁定。

### Blind Test A

结果：

**PASS**

分数：

**97 / 100**

能力判断：

**基本可接手，但需要补证**

隔离状态：

**NOT YET PROVEN**

主要发现：

快速接手阅读顺序与严格证据回查顺序需要明确区分。

### Blind Test B

结果：

**PASS**

分数：

**100 / 100**

能力判断：

**基本可接手，但需要补证**

测试环境：

**Temporary Chat + Non-Personalized**

隔离状态：

**UI-CONFIRMED**

安全门：

**5 / 5 PASS**

证据边界：

**PASS**

该测试进一步验证：

- 在隔离环境下仍能恢复正确工作基线；
- 能够正确区分快速阅读与严格证据回查；
- 能够在缺失具体施工证据时主动输出 `EVIDENCE INSUFFICIENT`；
- 不因后续修复推翻既有整体完成状态；
- 不混淆实施者与独立终审者。

### sample_001 的意义

该样本证明：

精炼 context-handoff 可以在显著减少阅读量的同时，让陌生 AI 恢复：

- 当前状态；
- 历史阶段；
- Agent 职责；
- 角色切换；
- 已闭合事实；
- 不得回退的治理边界；
- 证据等级；
- 下一步应如何回查。

同时也证明：

handoff 不能替代底层施工证据。

它的正确作用是：

**让 AI 不迷路，并知道什么时候必须回到底层证据。**

### sample_001 暴露的下一改进项

ChatGPT 侧已经形成：

- 可见原始派生证据层；
- 前置锚点层。

Codex 侧虽然已有：

- rollout 文件名；
- 行号；
- timestamp；
- Message ID；
- preview；

但未来应进一步形成：

**Codex 原始证据层索引 / 提取文件**

使两侧底层证据结构更加对称。

这应进入后续版本迭代，而不是无痕修改已经通过测试的历史 handoff。

---

## 18. Final Rule

本 Skill 的最终判断标准不是：

“摘要写得是否漂亮。”

也不是：

“文件做得是否很多。”

而是：

> 一个此前不了解历史的 AI，
> 能否在较少阅读量下恢复正确工作基线，
> 保住关键状态与角色边界，
> 知道哪些事实已经闭合，
> 知道哪些事情仍需补证，
> 知道什么时候必须回查原始证据，
> 并在证据不足时停止猜测。

如果能：

**handoff 成功。**

如果不能：

**返回证据层修正。**

不得通过继续补写摘要来掩盖证据不足。

---

## Release Status

当前文件：

`SKILL.md`

版本：

**V0.1**

状态：

**VALIDATED**

来源候选稿：

`17_SKILL_V0.1_候选.md`

来源候选稿 SHA256：

`91ED5C4E543D33F0BACCE6F61EDF0CBD388DB98828C6E41E9C5E4778CA6D76F9`

验证结果：

### sample_001 Blind Test A

**PASS｜97 / 100**

### sample_001 Blind Test B

**PASS｜100 / 100**

环境：

**Temporary Chat + Non-Personalized**

### Bluebird Cross-Domain Transfer Test

**PASS｜100 / 100**

Safety Gates：

**6 / 6 PASS**

Evidence Gaps：

**3 / 3 正确输出 EVIDENCE INSUFFICIENT**

迁移裁定：

**可迁移**

升格裁定：

**ELIGIBLE**

因此：

**context-handoff Skill V0.1 已完成真实样本验证、严格隔离交接验证以及陌生领域迁移验证，正式升格为 VALIDATED V0.1。**

未来修改不得无痕覆盖该版本身份。

后续改进应形成：

- V0.2
- V0.3
- V1.0

并重新执行相应验证。