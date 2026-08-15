---
name: website-project-triage
description: Use when a sales team needs 网站项目分级、网站需求梳理、参考站评估、报价前范围判断、合同附件建议 or 合同路由; triggers include quote, website scope, template, CMS, API, WebGL, Three.js, WebGPU, backend, ICP, hosting, source-code, and contract routing.
---

# Website Project Triage

在承诺范围、准备报价、下单或选择合同附件前，对网站机会作可复核的分级。它是决策支持，不是报价、合同批准或交付承诺；说明文字使用用户语言，保留输出字段和枚举值。

## 适用边界

必须用于网站/数字体验机会的售前分级、参考站拆解、范围确认和合同附件路由，尤其是出现 template、CMS、API、WebGL、支付、登录、hosting、ICP 或源代码时。

不得计算、输出、建议或推荐任何 selling price/销售价格、internal price/内部价格、cost/成本、price range/价格区间、discount/折扣/优惠、gross margin/毛利/毛利率、profit margin/利润率、margin/margin rate 或 quote recommendation/报价建议；不得批准报价、订单或合同，也不得替代技术、交付、法务或安全评审。纯内容润色、既有合同的法律解释、已签约后的实现排期不属于本 Skill。

## 先加载参考

每次分类前，先加载：

1. `references/classification-matrix.md`
2. `references/intake-checklist.md`

按条件加载：

| 条件 | 必须加载 |
| --- | --- |
| 提及 quote、订单、合同、hosting、ICP、安全、源代码、个人信息、支付或 API | `references/contract-routing.md` |
| 相邻等级难分、需要先例比较 | `references/anonymized-cases.md` |
| 集成到非 Codex host | `references/portable-integration.md` |

任何必需参考不可读取时，立即返回 `ESCALATE`，在 `missing_information` 写明缺失文件；不得临时编造规则或继续分类。非 Codex host 必须注入本文件并加载同一组必需参考。

## 决策顺序

1. 提取已明确事实：参考 URL 中**具体**要复刻的视觉/布局/动效/3D/功能；页面与内容量；语言；template 许可；动效与图形；数据与业务功能；hosting/ICP；安全与源代码；素材、决策人、工期、验收设备。参考站只是证据，不是完整范围。
2. 区分“分级阻断项”和“确认问题”。只有未知项仍可能改变 `primary_class`、F track、实时图形边界，或使 hosting 责任、素材/语言准备度与交付范围无法被明确排除或限定时，才是实质阻断项：返回 `NEEDS_INFO`，提出有针对性的问题；除已知硬性下限外，`primary_class` 用 `UNDETERMINED`。若已知事实已把主类、F track、图形边界、hosting 责任和素材准备风险充分限定到一个等级，且剩余事项可明确列为排除/另行范围而不会改变该等级，则返回 `CLASSIFIED`、使用适当的 `MEDIUM` confidence，并把剩余事项保留为确认问题；不得仅因还有语言、素材、hosting、设备或验收确认问题就自动降为 `NEEDS_INFO`/`LOW`。
3. 按 `classification-matrix.md` 选择一个前端/体验主类 `A`–`E`；按最难必需子系统，而非视觉相似度。WebGL、3D、particles、Shader、AI navigation、multi-user 都是明确升级信号。
4. 独立判断 backend/business track：登录、CMS/结构化数据、支付、数据库、API、ERP/CRM 等单列为 `F-`、`F` 或 `F+`；没有 backend 则为 `NONE`。不得把 F 隐藏在 A–E 中。
5. 依矩阵应用 `-`、无修饰或 `+`。两个相邻等级都合理时向上取级（如 `A+`/`B-` 取 `B-`）。相邻规则已确定唯一等级且剩余问题不会改变等级时，不得把这些确认问题当作分级阻断项。已知 WebGL 或业务功能只能确定下限时，可记录该下限并标 `LOW`，但不得称为最终分级。
6. 路由合同附件：始终列主 B2B 合同、项目订单、套餐/范围附件、验收/变更附件、重大条款确认；其余按 `contract-routing.md` 的事实触发。未确认事项不得写入 `included_scope`。只要任一方的 domain/hosting 持有、供应、provider、资源、配置或部署责任已经出现（包括已确认由客户负责并排除供应方工作），`contract_route` 就必须列域名/hosting 条款以记录责任边界；责任未确认但会实质影响交付时也必须列出，并标为“待责任确认的条件路由”。不得因 hosting 不属于供应方 included scope 或尚未确认而省略；backend、payment 或 data 项目同样适用。
7. 分配 `human_review`。无论等级或置信度，均须在报价、起草或创建订单、或签署前完成所列人工批准。

## 合同与人工门禁

除基础附件外，按事实加入：

- 任一方持有、供应或配置 domain/hosting，或相关 provider/资源/部署责任尚未确认但会实质影响交付：域名/hosting 条款；客户负责/供应方排除时也用条款记录责任边界，责任未定时标为条件路由并继续提问。中国大陆节点或备案协助：ICP 条款。
- 表单、backend、敏感数据、高级安全或客户自部署：安全条款；表单、analytics、cookies、logs、会员或跨境数据：个人信息条款。
- source、repository、credentials、迁移或 self-hosting：源代码条款；`C+`、任何 `D`/`E`：WebGL/特殊视觉条款。

评审最低要求：`A-`–`B` 且 `HIGH` 由 delivery lead；`B+`–`C+` 加 frontend/design；任何 `D`/`E` 由 creative-technical lead 与 delivery lead；任何 `F` 加 backend technical review。支付、个人信息、ICP、特殊许可、源代码转移、高级安全或异常责任加 legal/security review。无论等级或置信度，必须在报价、起草或创建订单、或签署前取得 `human_review` 指定人员的人工批准；`LOW` 或证据冲突时，在问题答复前不得起草订单。

## 输出契约

按以下顺序返回全部字段；即使 `NEEDS_INFO` 或 `ESCALATE` 也不得省略字段。`confidence` 只能是 `HIGH`、`MEDIUM` 或 `LOW`。

```text
status: CLASSIFIED | NEEDS_INFO | ESCALATE
primary_class: A-..E+ | UNDETERMINED
backend_class: NONE | F- | F | F+
confidence: HIGH | MEDIUM | LOW
one_line_verdict:
evidence:
missing_information:
included_scope:
excluded_or_separate_scope:
risk_flags:
contract_route:
human_review:
```

`included_scope` 只写证据支持的项目；`excluded_or_separate_scope` 明确 backend、图形、hosting、安全、source 等未确认或另行范围。任何适用于当前请求但未明确 included/confirmed 的生产或移交责任，都必须从 `missing_information`/`risk_flags` 镜像到 `excluded_or_separate_scope`，不能只提问或只报风险：

| 可观察的适用上下文 | 未明确 included/confirmed 时，`excluded_or_separate_scope` 必须写明 |
| --- | --- |
| 请求 realtime 3D、WebGL、原创/定制视觉、video 或其他依赖专用素材的体验，且素材来源/制作责任未确认 | original/custom asset production（含适用的 3D/visual/video asset 制作、采购、许可或优化责任） |
| 文案、图片、产品资料等材料缺失，或其创建/整理责任未确认 | content creation/copywriting 与适用的 original/custom asset production |
| 已出现 source、repository、self-hosting、部署接管、batch import/upload、既有站点/系统/数据转移或 integration handover，且迁移责任未确认 | migration |
| 出现多语言/language packs，且翻译制作或批准责任未确认 | translation production/approval |
| 出现加速、固定、提前上线或不可移动日期，且加速交付可行性未单独确认 | expedited delivery |

只应用与当前请求存在上述可观察上下文的行，不得把无关排除项机械堆入每个输出。另按已出现的系统上下文完成相关范围束：

- realtime/custom-visual 上下文：若未确认，`excluded_or_separate_scope` 同时列 undefined additional scenes/site-wide runtime、original/custom asset production、hosting/deployment、security 和 source/repository transfer。
- backend/payment/data/batch-import/integration 上下文：若未确认，`excluded_or_separate_scope` 同时列 ERP/CRM、复杂 permissions/approval、migration、integrations、security operations、hosting/deployment operations 和 source/repository transfer。

`contract_route` 使用固定槽位：先列基础附件，再列事实触发的条款。若任一输出字段已把 domain/hosting/provider/资源/配置/部署责任识别为当前交付的相关未决项，当前 `contract_route` 必须包含这一槽位：`域名/hosting 条款（待责任确认的条件路由：确认持有方、provider、资源/配置与部署责任）`。该槽位是当前路由，后续确认只决定其责任内容。

输出前逐字段交叉检查：每个适用且未确认项必须真的出现在 `excluded_or_separate_scope`；只在 `missing_information` 提问或只在 `risk_flags` 报风险不算排除。`risk_flags` 至少检查范围、素材、排期、hosting、合规、数据、源代码。若任一输出字段已说明 domain/hosting 的持有、provider、资源、配置或部署责任，无论该事项来自原始需求还是按 intake 主动识别、由客户还是供应方负责，`contract_route` 必须镜像列域名/hosting 条款；责任未决时标为条件路由。不得只问、只报风险或以“供应方不负责/原始需求没提”为由省略 route。`human_review` 必须写明“报价、订单起草/创建或签署前”的评审人。不得输出价格或任何价格区间，不得代表任何人签字、批准或承诺。

## 快速判断

| 信号 | 动作 |
| --- | --- |
| 单一固定 template、静态内容 | 从 A 评估；确认未含 CMS/登录 |
| 重组标准组件与品牌信息结构 | 从 B 评估 |
| 原创视觉、复杂 motion/video | 从 C 评估；WebGL 不默认包含 |
| 一个或多个实时 WebGL/Three.js/WebGPU 场景 | 从 D 或 E 评估，要求设备、性能、降级和场景验收 |
| CMS、登录、支付、数据、API 或业务集成 | 另加 F，不取代 A–E |
| 仍可能改变等级/track/图形边界或无法限定交付的核心事实缺失 | `NEEDS_INFO`，不先承诺 |
| 唯一等级已由事实与向上规则确定，剩余问题可排除且不改变等级 | `CLASSIFIED` + `MEDIUM`，保留确认问题 |
| 必需参考缺失 | `ESCALATE`，点名缺失 reference |

## 红旗：停止承诺并回到门禁

| 错误说法 | 必须做法 |
| --- | --- |
| “只有一个参考 URL，今天先报固定价。” | 报 `NEEDS_INFO`，说明参考站不定义范围。 |
| “参考站有的功能都算包含。” | 仅把客户明确要的功能写入证据，其余单列未知/排除。 |
| “WebGL 只是几个动画。” | 升级图形风险，确认场景、资产、设备、性能和降级，并安排评审。 |
| “静态站带登录、支付、批量管理，算 C 就够。” | 分开 A–E 与 `F`，补齐数据、安全、支付和 API 事实。 |
| “素材没齐，先按快速模板上线。” | 将素材与排期列风险；没有明确授权不得用占位方案缩小范围。 |
| “还有少量确认问题，所以必须一律 `NEEDS_INFO`/`LOW`。” | 先判断问题是否会改变主类、F、图形或可限定的交付边界；不会改变等级时按事实 `CLASSIFIED`/`MEDIUM`，并保留问题和排除项。 |
| “已经在问题或风险里提到，所以不用写进 `excluded_or_separate_scope`。” | 问题/风险不等于排除；对每个适用且未确认项，在排除字段再次明确写出。 |
| “hosting 是按 intake 主动补问，不是原始需求，所以不用路由。” | 一旦 hosting/provider/部署责任被识别为 relevant 并写入任一字段，就必须镜像域名/hosting 条款；未决时标条件路由。 |

若发现上述说法、催促当天报价、或试图以“后面再确认”绕过实质分级阻断项，停止承诺，保持 `NEEDS_INFO`/`ESCALATE`，并等待指定人工评审。普通确认问题不得被反向用来拖延一个已由证据确定的 `CLASSIFIED` 结果。
