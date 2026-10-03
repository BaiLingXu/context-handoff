# context-handoff

**Validated V0.1**

**Repository privacy maintenance: V0.1.1 (2026-10-03)**

context-handoff 是一个跨 AI / 智能体的上下文交接 Skill。

它的目标不是简单总结聊天，而是把复杂、多轮、跨会话、跨智能体的工作历史，转换为：

**可验证、可追溯、可安全继续接手的标准化 context-handoff 包。**

---

## Repository Privacy Maintenance V0.1.1

本次 V0.1.1 是**仓库发布层的隐私维护**，不是 Skill 功能升级。

核心边界：

- `SKILL.md` 继续保持 **Validated V0.1** 的规则、语义与能力边界；
- 不把本次隐私维护冒充为新的功能验证；
- 新 Public 仓库使用全新仓库身份与干净 Git 历史；
- 旧 Public 仓库已转为 Private evidence 仓，用于保留历史证据，不作为当前公开发布源；
- 新公开提交不使用私人邮箱；
- MIT `LICENSE` 的公开版权署名改为隐私安全的 `context-handoff contributors`；
- 真实原始聊天、Codex session / rollout、业务资产及未授权个人信息仍不得默认进入 Public 仓库。

本次维护不修改 `SKILL.md` 的功能规则，因此 V0.1 的 Blind Test 与跨领域迁移验证结论仍只对应原已验证 Skill 内容。

当前公开仓库包的机器身份由：

- `PUBLIC_RELEASE_MANIFEST_V0.1.1.csv`
- `PUBLIC_RELEASE_MANIFEST_V0.1.1.sha256`

记录。

历史 V0.1 发布身份仍由：

- `PUBLIC_RELEASE_MANIFEST_V0.1.csv`
- `PUBLIC_RELEASE_MANIFEST_V0.1.sha256`

记录。历史 Manifest 中的哈希对应当时的 V0.1 原始公开载荷，不应被解释为当前 V0.1.1 仓库包的哈希。

---

## 当前正式版本

Skill Version:

**V0.1**

Skill Status:

**VALIDATED**

Repository Packaging Revision:

**V0.1.1 — privacy-only republish**

当前公开载荷至少包括：

- `SKILL.md`
- `README.md`
- `LICENSE`
- 历史 V0.1 Manifest 与 sidecar
- 当前 V0.1.1 Manifest 与 sidecar

README 不内嵌当前发布文件或 Manifest 的 SHA256 数值。

这样可以避免：

- README 中哈希过期；
- 人工或 AI 抄录哈希出错；
- README 与 Manifest 形成循环哈希依赖。

当前仓库包发布身份采用单向链：

`public payload → PUBLIC_RELEASE_MANIFEST_V0.1.1.csv → PUBLIC_RELEASE_MANIFEST_V0.1.1.sha256`

---

## 它解决什么问题

复杂 AI 协作经常遇到：

- 新聊天无法快速恢复历史；
- ChatGPT 与 Codex 职责变化容易丢失；
- CLOSED / REOPEN 等状态被错误回退；
- 摘要与原始证据混为一谈；
- 不同 Agent 的实施、审计、终审职责混淆；
- 上下文太长，接手成本越来越高；
- AI 为补齐空白而猜测不存在的事实。

context-handoff 的核心目标是：

**减少重新解释历史的成本，同时保留原始证据回查能力。**

---

## 核心原则

### Evidence first. Summary second.

证据等级：

原始证据 > 硬锚点 > 时间轴 > 阶段解释 > README > 精炼 handoff

同时必须区分：

### 快速接手阅读顺序

handoff → README → 时间轴 → 硬锚点 → 原始证据

用于：

**快速恢复工作基线。**

### 严格证据回查顺序

原始证据 → 硬锚点 → 时间轴 → 阶段解释 → README / handoff

用于：

**具体事实核验、施工前确认和争议裁决。**

---

## EVIDENCE INSUFFICIENT

当现有材料不足以证明某个事实时：

必须明确输出：

EVIDENCE INSUFFICIENT

禁止为了答案完整而：

- 猜代码；
- 猜路径；
- 猜状态；
- 猜 CLOSED / REOPEN；
- 猜责任人；
- 猜人工确认；
- 根据常识补齐历史。

证据不足不是失败。

停止猜测是本 Skill 的核心安全能力。

---

## 标准工作流

V0.1 的主要流程：

1. Identify Sources
2. Extract Visible Evidence
3. Build Index
4. Find Candidate Boundaries
5. Select Hard Anchors
6. Build Cross-Agent Timeline
7. Define Phases
8. Generate Handoff Package
9. Blind Test

核心链路可概括为：

**提取 → 去噪 → 定位 → 锚定 → 合并 → 交接 → 验证**

---

## 已完成验证

### 真实样本

real_validation_sample_001

来源于真实 ChatGPT + Codex 长周期协作历史。

完成：

- ChatGPT 有效分支恢复；
- 3658 条正式窗口可见消息提取；
- ChatGPT / Codex 硬锚点；
- 跨智能体统一时间轴；
- P0～P5 阶段定义；
- 精炼 handoff；
- SHA256 Manifest；
- 陌生 AI 接手测试。

### Blind Test A

**PASS｜97 / 100**

验证：

普通独立新聊天下可以恢复正确工作基线。

### Blind Test B

**PASS｜100 / 100**

环境：

**Temporary Chat + Non-Personalized**

验证：

严格隔离条件下仍能恢复正确工作基线，并正确使用：

EVIDENCE INSUFFICIENT

---

## 跨领域迁移验证

为了避免 Skill 只会复述第一套真实案例，另建立：

**Bluebird 网站迁移陌生样本**

该样本与原案例业务领域完全不同。

测试结果：

**PASS｜100 / 100**

Safety Gates：

**6 / 6 PASS**

Evidence Gaps：

**3 / 3 正确停止推断**

迁移裁定：

**可迁移**

因此 V0.1 已满足正式升格条件。

---

## V0.1 已验证的关键能力

- 正确识别 Agent 角色变化；
- 不把早期审计角色反向改写成后期实施角色；
- 不把实施者自动等同于独立终审者；
- 保护已经形成的 CLOSED / PASS / completed 状态；
- 不因后续局部修复自动推翻历史完成状态；
- 区分 machine-derived fact、AI interpretation、human confirmation；
- 在证据不足时停止推断；
- 保留 source pointer；
- 支持跨 Agent 时间轴；
- 支持陌生 AI 快速接手。

---

## Release 身份

历史 V0.1 Skill 发布身份由：

`PUBLIC_RELEASE_MANIFEST_V0.1.csv`

记录。

当前 V0.1.1 Public 仓库包发布身份由：

`PUBLIC_RELEASE_MANIFEST_V0.1.1.csv`

记录。

Manifest 中的 SHA256：

**由机器直接从公开仓库实际文件计算。**

不得依赖人或 AI 手工抄录 64 位哈希作为唯一发布门禁。

---

## GitHub 的角色

GitHub 可以作为：

- 版本管理；
- Skill 保存；
- 迁移载体；
- 发布与维护场所；
- 新电脑恢复来源。

但 GitHub 不是本 Skill 的业务目的。

本 Skill 的真正服务对象是：

**不同 AI / Agent 之间的可靠上下文交接。**

---

## Maintenance & Contributions

This project is shared for practical reuse and community improvement.

Issues and pull requests are welcome.

Maintenance is **best-effort**, and response times are not guaranteed.

这意味着：

- 欢迎提出 Issue；
- 欢迎提交 Pull Request；
- 不承诺固定响应时间；
- 不承担逐一答疑或持续客服义务；
- 有价值的问题和改进建议可以进入后续版本。

Validated V0.1 的 Skill 规则不做无痕覆盖。

仓库级隐私、许可证署名或发布元数据维护，应明确标识为维护修订，不得冒充新的功能验证。

功能性后续改进应通过：

`Issue → 分支 → 测试 → PR → Review → Merge → 新版本`

完成。

---

## License

本项目采用：

**MIT License**

详见：

`LICENSE`

MIT License 允许他人使用、修改、合并、发布和再分发本项目内容，但须保留相应版权及许可证声明。

本项目按许可证中的 **“AS IS”** 条款提供，不承诺适用于所有场景。

---
## 隐私与上传边界

不要默认把以下内容上传公共 GitHub：

- 完整 ChatGPT 原始导出；
- Codex 原始 session / rollout；
- 私人聊天；
- 业务原始资产；
- 包含个人信息的证据；
- 私人邮箱、真实姓名或其他不需要公开的 commit metadata；
- 密钥、账号、token；
- 未授权公开的内部材料。

正式仓库可以优先保存：

- SKILL.md
- README.md
- 通用 scripts
- templates
- schema
- 脱敏测试样本
- 测试规则

真实原始样本是否上传：

**必须由用户单独决定。**

公共发布前应同时检查：

- 文件正文；
- Git commit author / committer；
- commit email；
- LICENSE 署名；
- README 与 Manifest；
- 历史提交中是否仍保留不应公开的信息。

---

## 推荐仓库结构

context-handoff/
├─ SKILL.md
├─ README.md
├─ LICENSE
├─ PUBLIC_RELEASE_MANIFEST_V0.1.csv
├─ PUBLIC_RELEASE_MANIFEST_V0.1.sha256
├─ PUBLIC_RELEASE_MANIFEST_V0.1.1.csv
├─ PUBLIC_RELEASE_MANIFEST_V0.1.1.sha256
├─ scripts/
├─ templates/
├─ schema/
├─ tests/
└─ examples/
   └─ sample_001/

后续脚本应逐步实现：

- ChatGPT 提取；
- Codex 提取；
- index；
- anchors；
- timeline；
- handoff；
- verification。

但自动化不得绕过证据规则。

---

## 当前成熟度

V0.1 已完成：

- 真实样本验证；
- 普通陌生 AI 接手验证；
- 临时聊天 + 不个性化隔离验证；
- 跨领域迁移验证；
- SHA256 发布身份验证。

因此当前 Skill 状态：

**VALIDATED V0.1**

当前仓库维护状态：

**V0.1.1 privacy-only republish**

---

## 下一功能版本

V0.2 可以重点改进：

1. Codex 原始证据层标准化；
2. SHA256 全流程机器传递；
3. 通用 source schema；
4. 自动硬锚点候选提取；
5. handoff 模板标准化；
6. Release Manifest 自动生成；
7. 更多不同领域迁移测试。

已有 Validated V0.1 的 Skill 内容不做无痕覆盖。

后续功能改进形成新版本并重新验证。
