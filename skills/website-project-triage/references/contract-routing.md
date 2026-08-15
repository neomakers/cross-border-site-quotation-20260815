# 合同附件路由

本文件只决定应选择或补充确认的附件类别，不提供法律解释、私有条款、商业金额或保证。Skill 不批准报价、订单或合同；最终附件组合须由 delivery lead 审核，触发特殊事项时再由 technical、legal 或 security reviewer 审核。

## 始终路由的基础文件

每个项目始终列出：

1. 主 B2B 合同；
2. 项目订单；
3. 套餐/范围附件；
4. 验收/变更附件；
5. 重大条款确认。

在范围事实未清楚或 confidence 为 `LOW` 时，只能把这些写为后续标准 route，不得据此起草订单或承诺签署。

## 事实触发矩阵

| 已确认条件 | 必须增加或强化的 route | 最低确认内容 | 人工门禁 |
| --- | --- | --- | --- |
| 原始请求/事实明确引入任一方的 domain/hosting ownership、provider、资源、配置、部署、self-hosting 或 migration 责任，或供应方提供/配置 hosting | 域名/hosting 条款；客户负责/供应方排除时仍记录责任边界，未确认时标为“待责任确认的条件路由”且不等于已纳入交付 | 持有方、provider、资源与配置责任、地区、部署 ownership、traffic/storage/bandwidth、备份与迁移边界 | delivery lead；技术责任不清时加 technical review |
| backend、payment、data、batch-import 或 integration 项目中，hosting 责任是该业务系统范围的实质必要条件 | 域名/hosting 条款；责任未确认时使用待责任确认的条件路由 | provider、资源/配置、部署与运行责任、地区、容量、备份和业务系统迁移边界 | backend technical + delivery lead；按数据/支付事实加 legal/security review |
| 中国大陆节点或 ICP filing assistance | ICP 条款 | 主体、协助边界、前置材料、节点与时间依赖 | delivery lead + legal review |
| form、backend、客户自部署或一般数据处理 | 安全条款（baseline security） | 部署边界、访问责任、常规保护、更新/备份/incident responsibility、验收边界 | backend/technical review；涉及数据时加 security review |
| 明确要求 WAF/DDoS、audit、monitoring、渗透测试、compliance、特殊 SLA 或类似能力 | 安全条款（advanced security）并在套餐/范围与验收/变更附件中单列 | 产品/服务边界、覆盖环境、指标、报告、第三方依赖、验收方法和持续运维责任 | security + legal + delivery lead review |
| source、repository、迁移、self-hosting、认证凭据或部署接管 | 源代码条款 | 移交对象、时间点、repository/dependency、部署资料、第三方许可、接管与验收责任 | technical + delivery lead；source transfer 加 legal review |
| form、analytics、cookies、logs、member data 或 cross-border data | 个人信息条款 | 数据类别、目的、保存/访问角色、hosting 地区、第三方与跨境、删除/退出责任 | legal/security review |
| `C+`、任何 `D` 或 `E`，或明确 WebGL/Three.js/WebGPU/实时 scene | WebGL/特殊视觉条款 | scene、3D assets/许可、device/browser、performance budget、降级、交互和 scene-based acceptance | creative-technical + delivery lead review |
| payment/invoice flow | 安全条款 + 个人信息条款；在项目订单、套餐/范围、验收/变更和重大条款确认中记录 payment-related confirmations | provider/地区、流程与异常路径、数据/责任边界、第三方依赖和验收；不默认包含未说明的财务系统 | backend technical + legal/security + delivery lead review |
| 一个或多个 API | 在套餐/范围与验收/变更附件中明确 integration；按数据和交付事实继续触发安全、个人信息或源代码条款 | 接口数量、方向、频率、文档、测试环境、认证责任、错误/重试、限流、第三方变更与验收 | backend technical review；个人数据或高风险流程加 legal/security review |

## Baseline security 与 advanced security

`baseline security` 是 form、backend、一般数据处理或客户自部署触发的安全责任确认。它不代表已包含 WAF/DDoS、持续 SOC/monitoring、audit、渗透测试、compliance 认证、特殊 SLA 或无限责任。

只有客户明确要求 advanced security 能力并提供覆盖环境、指标、运维与验收事实时，才将其列为单独范围，同时强化安全条款、套餐/范围附件和验收/变更附件。信息不完整时写入 `missing_information` 与 `excluded_or_separate_scope`，不得把 baseline security 扩写成 advanced security 承诺。

## Source-transfer obligations

只要谈到 source、repository、self-hosting、迁移、部署接管或认证凭据，就必须路由源代码条款。下列事项必须在签署前由人工确认：

- 交付的是构建产物、项目 source、repository history、部署资料还是以上组合；
- 第三方 assets、font、library、service 和 license 是否允许转移；
- 环境配置与敏感凭据如何由授权人员交接，Skill 不记录其值；
- migration、自部署、后续维护、dependency update 和 deployment ownership 的责任边界；
- source delivery、可构建性或部署接管的验收方法。

未出现 source-transfer 事实时不得默认承诺移交；已出现时也不得只写“包含源码”而跳过上述责任确认。

## 组合与冲突规则

1. 条件可叠加：例如 `D/F` 且含 payment、个人数据和客户自部署时，同时使用标准 route、WebGL/特殊视觉、安全、个人信息和源代码条款。
2. API 不是独立发明一个附件名；它先进入套餐/范围和验收/变更附件，再按数据、安全与 source 事实触发现有类别。
3. 未确认的 hosting、security、source、payment 或 integration 必须放入 `missing_information` 或 `excluded_or_separate_scope`，不能当作 included。域名/hosting route 只由事实触发矩阵的两个 hosting 谓词触发；触发后，客户负责或供应方排除时仍用条款记录边界，责任未决时标为条件路由。纯 A–E 项目仅由标准 intake 产生的 hosting 问题、风险或排除不触发 route；backend/payment/data/batch-import/integration 项目若业务系统实质需要 hosting 责任，则仍触发条件路由。
4. 每个 `human_review` 都必须保留指定 reviewer，并用用户语言明确表达“人工审批完成前，不得报价、起草/创建订单或签署合同”的完整含义，不得只列两个阶段。
5. 特殊许可、异常责任、附件冲突或必需 reference 缺失时，返回 `ESCALATE` 并点名 reviewer，不得自行解释或改写法律条款。
