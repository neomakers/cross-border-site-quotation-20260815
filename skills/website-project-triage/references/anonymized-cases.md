# 匿名案例先例

`case_library_version: 1.0.0`

本文件只包含 synthetic 或 fully anonymized 的规范化案例，用于相邻等级难分时比较判定信号。案例不是自动决策规则；矩阵、当前 evidence 和向上取级规则优先。

## 使用与更新规则

- 只在相邻等级有歧义或需要 precedent comparison 时加载。
- 每条案例必须保留 initial grade、人工 approved grade、change reason、contract-routing outcome 和 reusable lesson。
- 更新是经人工审核的 curated、versioned knowledge，绝不是 automatic model training；模型输出不得自动回写本文件。
- 新案例先去除或泛化主体标识、真实域名、联系信息、个人数据、认证凭据、source、私有合同内容、商业金额和唯一识别细节，再由人工 reviewer 批准并提升 `case_library_version`。
- 案例与当前事实冲突时，不套用案例；回到 classification matrix，必要时 `ESCALATE`。

## CASE-001 — 整站 template 上限

- `case_type`: synthetic
- `normalized_features`: 一个既有 whole-site template；少量页面；只替换品牌、文字和图片；navigation、组件结构、移动端与 form logic 不变；无 backend。
- `initial_grade`: `A`
- `approved_grade`: `A-`
- `reason_for_change`: 已确认页面少、section 简单且没有特殊素材，满足 `A-` 的全部收窄条件。
- `contract_routing_outcome`: 标准基础文件；无特殊事实触发额外附件。
- `reusable_lesson`: 只有证据明确落在最小范围时才使用 `-`，不能仅凭销售描述“简单”降级。

## CASE-002 — `A+` / `B-` 组件重组边界

- `case_type`: synthetic
- `normalized_features`: 使用获准 component library；部分页面改变内容顺序；自定义品牌 palette；静态 case 页面；无登录、CMS、支付或 custom animation；hosting 责任与环境未确认。
- `initial_grade`: `A+`
- `approved_status`: `CLASSIFIED`
- `approved_grade`: `primary_class: B-`; `backend_class: NONE`; `confidence: MEDIUM`。
- `confirmation_questions`: hosting 由谁提供/配置、部署地区与 ICP 是否涉及；这些问题继续保留，但在 hosting 工作明确排除于当前范围时不会改变 `B-`。
- `reason_for_change`: 页面由标准组件重新组合并形成轻量自定义信息结构；按相邻等级向上规则得到唯一 `B-`。已明确无 backend 与 custom animation，hosting 未决作为排除项和交付风险处理，不是分级阻断项。
- `contract_routing_outcome`: 标准 route；域名/hosting 条款标为待责任确认的条件路由，hosting 工作不写入 included scope；ICP、source/security 继续按事实确认。
- `reusable_lesson`: “仍使用标准组件”不等于整站 template；发生有限组件重组时从 B 评估。若主类、F、图形和交付责任已被事实或排除项充分限定，普通确认问题应保留，但不得把确定的相邻等级降为 `NEEDS_INFO`/`LOW`。

## CASE-003 — Motion 与 WebGL 边界

- `case_type`: synthetic
- `normalized_features`: 首页有实时 3D 产品、particles、mouse interaction 和 scroll-linked camera；移动端需可用；scene 资产、performance budget 和 fallback 未确认。
- `initial_grade`: `C+`
- `approved_grade`: provisional `D-` with `NEEDS_INFO`
- `reason_for_change`: 实时 3D scene 和 camera/interaction 是 D 的硬性下限，不属于普通 motion；关键信息缺失，不能给最终 D 等级。
- `contract_routing_outcome`: 标准基础文件加 WebGL/特殊视觉条款；hosting/provider/部署资源责任未确认且影响实时体验交付，因此现在即列待责任确认的域名/hosting 条件路由；source 和 security 保持未确认/排除。
- `reusable_lesson`: 先识别 rendering technology 和 scene acceptance，再判断 motion 复杂度；不得把 WebGL 留在 C。

## CASE-004 — 静态外观隐藏业务系统

- `case_type`: synthetic
- `normalized_features`: public catalog 外观可用标准组件；用户登录、invoice payment、结构化产品数据和 staff batch management 为必需；provider、roles、hosting 与 data retention 未确认。
- `initial_grade`: `B/NONE`
- `approved_grade`: provisional `B/F` with `NEEDS_INFO`
- `reason_for_change`: 登录、支付、database workflow 和批量管理必须独立增加 F；资料不足，前端与 backend 都不能视为最终批准。
- `contract_routing_outcome`: 标准基础文件加安全、个人信息和 payment-related confirmations；hosting/provider/部署资源责任未确认且影响 backend、payment 与 data 交付，因此现在即列待责任确认的域名/hosting 条件路由。
- `reusable_lesson`: A–E 只描述前端/体验；业务功能永远单列 F。

## CASE-005 — 标准 backend 与复杂业务集成边界

- `case_type`: synthetic
- `normalized_features`: member area、多个 roles 和 structured CMS 已确认，后续又确认 ERP/CRM 双向 real-time sync、复杂 approval workflow 和多组织隔离；页面/体验范围没有足够 evidence 支持 A–E 主类。
- `initial_grade`: `primary_class: UNDETERMINED`; `backend_class: F`
- `approved_status`: `NEEDS_INFO`
- `approved_grade`: `primary_class: UNDETERMINED`; `backend_class: F+`; 不是 standalone project grade 或 final classification。
- `unresolved_information`: 页面数量/类型、template/组件许可、视觉/motion/graphics、素材和 hosting 尚不足以判定 A–E。
- `reason_for_change`: ERP/CRM、实时同步、复杂审批和多组织隔离使 backend 从 F 升到 `F+`；前端 evidence 不足，所以保留 `primary_class: UNDETERMINED` 并返回 `NEEDS_INFO`。
- `contract_routing_outcome`: 标准基础文件；integration 写入套餐/范围和验收/变更附件；hosting/provider/部署资源责任未确认且影响 F+ integration 交付，因此现在即列待责任确认的域名/hosting 条件路由，并按数据、security、source ownership 事实叠加相关条款。
- `reusable_lesson`: 多个标准功能不一定是 `F+`；复杂业务系统、权限/审批或高风险数据流才是升级核心。F 永远与一个 A–E `primary_class` 或 `UNDETERMINED` 并列，不能单独充当项目等级。
