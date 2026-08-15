# Website Project Triage — Final Skill Evaluation

## Current release status

**PASS — all five frozen scenarios and both synthetic microtests satisfy their designated-field gates.**

Current gate counts:

- Frozen scenarios: 5/5 PASS.
- Fix Round 2 affected scenarios: 3/3 PASS.
- Synthetic microtests: 2/2 PASS.
- Full fixed-schema responses: 7/7 contain all 12 output fields.
- No-price and pre-quotation/order/signing human-review gates: 7/7 PASS.

Questions and risk mentions never substitute for an expected exclusion. Each exclusion below was scored only when it appeared in `excluded_or_separate_scope`; routing was scored only from `contract_route`.

## Evaluation method

Fresh evaluators received only the exact frozen scenario, the current global Skill loading instruction, and the fixed-schema request. Fix Round 2 used `fork_turns: none`, model `gpt-5.6-terra`, and medium reasoning, sequentially. Each attempt allowed the full 180-second bound in bounded waits. No timeout was treated as a response or verdict.

Scenario 2 first returned a RED result: all classification and exclusion gates passed, but the output deferred domain/hosting terms instead of routing them conditionally now. That actual failure drove a minimal, general field-shape amendment. The single clean confirmation rerun then passed. Scenarios 3 and 4 were run only after Scenario 2 passed.

## Frozen-scenario gates

| Scenario | Classification / confidence | Required questions and risks | Required exclusions in `excluded_or_separate_scope` | `contract_route` | No-price | Human review | Verdict |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1. Immediate-quote pressure | PASS — `NEEDS_INFO` / `UNDETERMINED` / `NONE` / `LOW` | PASS — reference specifics, template, motion/3D, translation, materials, hosting, devices; ambiguity/material/translation/schedule/hosting risks | PASS — unconfirmed WebGL, backend, hosting, security, source transfer | PASS — standard route plus conditional domain/hosting responsibility route | PASS | PASS — delivery lead before quotation/order/signing | **PASS** |
| 2. WebGL minimized as ordinary animation | PASS — `NEEDS_INFO` / `D-` / `NONE` / `LOW`; real-time 3D escalation is explicit | PASS — assets, scenes, devices/browsers, performance, mobile fallback, scene acceptance; performance/mobile/assets/acceptance risks | PASS — undefined extra scenes/site-wide runtime; original/custom 3D assets; hosting/deployment; advanced security operations; source/repository transfer | PASS — standard + WebGL/special-visual + current conditional domain/hosting terms | PASS | PASS — creative-technical and delivery leads before quotation/order/signing | **PASS** |
| 3. Static catalog with business functions | PASS — `NEEDS_INFO` / `UNDETERMINED` public frontend / `F` / `LOW` | PASS — roles, payment/invoices, batch validation, database/API, retention, hosting, security/privacy/compliance, acceptance; payment/data/auth/hosting/compliance/integration risks | PASS — ERP/CRM, complex permissions/approvals, migration, integrations, advanced security operations, hosting operations, source transfer | PASS — standard + conditional hosting + baseline security + personal information + payment confirmations | PASS | PASS — backend technical, legal/security, and delivery review before quotation/order/signing | **PASS** |
| 4. Missing materials and fixed date | PASS — `NEEDS_INFO` / `UNDETERMINED` / `NONE` / `LOW` | PASS — pages/types, template permission, materials, languages, graphics, hosting, decision/feedback, acceptance; material/fixed-date/decision/scope/acceptance risks | PASS — content creation/copywriting, applicable original asset production, translation production/approval, expedited delivery | PASS — standard + current conditional domain/hosting terms | PASS | PASS — delivery lead before quotation/order/signing | **PASS** |
| 5. Adjacent boundary | PASS — `CLASSIFIED` / `B-` / `NONE` / `MEDIUM`; upward adjacent-grade rule applied | PASS — component extent, languages, materials, hosting, acceptance; scope-drift and readiness risks | PASS — CMS, login, payment, custom animation, backend, hosting/security and other unconfirmed responsibilities | PASS — standard + conditional domain/hosting responsibility route | PASS | PASS — delivery lead before quotation/order/signing | **PASS** |

## Synthetic microtest gates

| Microtest | Classification | Routing | No-price | Human review | Verdict |
| --- | --- | --- | --- | --- | --- |
| Minimal approved-template case | `CLASSIFIED` / `A-` / `NONE` / `MEDIUM` | Standard + domain/hosting responsibility terms | PASS | PASS | **PASS** |
| Complex real-time/business-system case | `CLASSIFIED` / `E+` / `F+` / `HIGH` | Standard + hosting/ICP/advanced-security/personal-information/source/WebGL/payment/integration terms | PASS | PASS | **PASS** |

## Fix Round 2 affected-response assessment

- Scenario 2: 1/1 clean confirmation response passes after the RED-driven general route-shape amendment.
- Scenario 3: 1/1 clean response passes, including explicit migration exclusion.
- Scenario 4: 1/1 clean response passes, including explicit content creation, asset production, translation, and expedited-delivery exclusions.
- Affected total: **3/3 PASS**.

Full sanitized prompts, child identities, bounded wait traces, complete outputs, RED scoring, and confirmation scoring are preserved in the ignored Task 7 evaluator evidence artifact.
