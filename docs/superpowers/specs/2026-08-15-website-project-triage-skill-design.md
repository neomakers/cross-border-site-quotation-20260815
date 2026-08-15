# Website Project Triage Skill Design

Date: 2026-08-15

## Purpose

Create a reusable skill that helps non-technical sales staff classify website opportunities before they promise scope, prepare an order, or select contract attachments. The skill returns categories and uncertainty, never a price.

The skill is a decision-support tool. A delivery lead must approve the classification and contract package before quotation or signature.

## Language and model portability

- Use an ASCII skill name and a bilingual Chinese/English trigger description so Chinese sales language and English technical terms both match.
- Write operational guidance in concise Chinese, retain stable English terms such as WebGL, CMS, API, Shader, and output field names, and respond in the user's language.
- Do not duplicate the complete skill in two languages; full duplication increases context size without improving the A–F decision rules.
- Keep the core package in plain YAML and Markdown. Treat `agents/openai.yaml` as Codex-specific UI metadata that other orchestrators may ignore.
- A non-Codex model does not discover the skill folder automatically. Its host application must match the skill metadata, inject `SKILL.md`, and load required references.

## Core model

Use one primary frontend/experience class from A through E. Add class F only when the project includes backend or business-system work. Apply `-`, unmodified, or `+` to show difficulty within each class.

Examples:

- `A-`: very small fixed-template site.
- `A+`: template site at the upper limit, close to light customization.
- `B-`: smallest light-custom project.
- `C+/F-`: advanced visual frontend plus a small standard backend requirement.

When two adjacent grades are plausible, classify upward. For example, an `A+`/`B-` boundary becomes `B-`. This protects delivery scope while preserving a clear escalation path.

## Category matrix

### A — Standard template site

- Select one existing whole-site template.
- Replace brand, text, images, contact information, and limited product cards.
- Keep navigation, component structure, layout, mobile logic, and form logic unchanged.
- Deliver a static frontend without a customer login or CMS.

Modifiers:

- `A-`: 1–3 simple pages, few sections, no unusual assets.
- `A`: standard package boundary.
- `A+`: at the page/content/material limit, minor component rearrangement, or higher-than-normal content preparation.

### B — Light custom brand site

- Recombine approved standard components rather than use one complete site template.
- Apply one brand direction and a customized information structure.
- Use static product, news, and case pages; news is not real-time.
- Exclude customer-managed backend unless class F is added.

Modifiers:

- `B-`: mostly standard components with limited rearrangement.
- `B`: multiple page types and a clear custom brand composition.
- `B+`: near the upper page/content limit, more custom components, filtering/search, or significant responsive refinement.

### C — Advanced visual and motion site

- Use original visual direction, custom interaction design, motion sequences, transitions, custom cursor, or video integration.
- Do not treat WebGL/3D scenes as automatically included.
- Exclude customer-managed backend unless class F is added.

Modifiers:

- `C-`: advanced visual design with limited motion.
- `C`: several coordinated motion systems and custom page transitions.
- `C+`: complex scroll narratives, extensive animation states, demanding mobile degradation, or performance work; no major WebGL scene.

### D — Immersive WebGL interactive site

- Include at least one WebGL/Three.js/WebGPU scene or equivalent real-time graphics system.
- Include defined 3D assets, particles, shaders, camera paths, or scene-linked scroll/mouse interaction.
- Require explicit device coverage, performance budget, degradation strategy, and scene-based acceptance criteria.

Modifiers:

- `D-`: one simple WebGL hero or contained interactive scene.
- `D`: one standard immersive scene plus coordinated transitions and mobile degradation.
- `D+`: multiple scenes, advanced shaders/particles, heavy original assets, or stringent device/performance targets.

### E — Flagship immersive digital experience

- Use multiple real-time scenes or a site-wide interactive runtime.
- May include advanced particles, shaders, physics, AI navigation, multi-user interaction, or custom creative tooling.
- Require creative direction, UI, motion design, 3D, creative frontend, graphics engineering, QA, performance engineering, and project management.

Modifiers:

- `E-`: flagship presentation with limited system complexity.
- `E`: full multi-scene experience.
- `E+`: real-time multi-user, AI navigation, complex physics, custom engine/tooling, or unusually broad device/SLA requirements.

### F — Backend and business-system track

F is orthogonal to A–E and may be added to any primary class.

- `F-`: fixed-schema content management, form storage, simple file upload, or one simple third-party integration.
- `F`: login, member area, database workflows, standard payment, multiple APIs, roles, or structured content management.
- `F+`: ERP/CRM integration, complex permissions, approval workflows, high-risk payment/data flows, multi-tenant systems, real-time synchronization, or custom business platforms.

If no backend is required, omit F rather than output `F-`.

## Classification inputs

Collect or infer the following before a confident result:

1. Reference URLs and what specifically should be copied: visual style, layout, motion, 3D, or functionality.
2. Page count, page types, navigation, product/news/case volume, and whether news is static.
3. Language-pack count and who supplies approved translations.
4. Whether existing templates/components may be used and what may change.
5. Motion, transitions, video, Canvas, WebGL, 3D, particles, shaders, AI, or multi-user requirements.
6. Customer login, CMS, batch editing, database, search/filter, payment, API, ERP/CRM, file upload, or scheduled tasks.
7. Domain, hosting region, ICP filing, expected traffic, storage, bandwidth, video, and large-file requirements.
8. Security, monitoring, WAF/DDoS, audit, compliance, source-code transfer, and deployment ownership.
9. Material readiness, decision-maker, feedback process, deadline, and required acceptance devices.

If page/content scope, backend, motion/graphics, hosting, or materials are materially unknown, return `NEEDS_INFO` with targeted questions. Do not guess a final grade.

## Boundary and escalation rules

1. Classify by the hardest required subsystem, not by visual similarity alone.
2. Treat a reference site as evidence, not a complete requirement.
3. Upgrade adjacent ambiguity: `A+` versus `B-` becomes `B-`; `B+` versus `C-` becomes `C-`; and so on.
4. Add F whenever the customer must log in, manage data, process payment, store structured data, or connect business systems.
5. Do not hide F inside A–E.
6. Treat WebGL, 3D, particles, shaders, AI navigation, and multi-user interaction as explicit escalation signals.
7. Treat incomplete or disorganized materials as a workload/risk modifier, not as a reason to pretend the site is simpler.
8. Never output a price, margin, internal settlement amount, or customer selling price.
9. Never approve a quote or contract. Mark the required review gate.

## Output contract

Return these fields in order:

1. `status`: `CLASSIFIED`, `NEEDS_INFO`, or `ESCALATE`.
2. `primary_class`: one of `A-` through `E+`, or `UNDETERMINED`.
3. `backend_class`: `NONE`, `F-`, `F`, or `F+`.
4. `confidence`: `HIGH`, `MEDIUM`, or `LOW`.
5. `one_line_verdict`: concise classification statement.
6. `evidence`: facts supporting the class.
7. `missing_information`: questions still requiring answers.
8. `included_scope`: likely included items under the class.
9. `excluded_or_separate_scope`: backend, graphics, hosting, security, source, or other separate items.
10. `risk_flags`: scope, material, schedule, hosting, compliance, data, and source-code risks.
11. `contract_route`: required contract attachments and special confirmations.
12. `human_review`: delivery, technical, security, or legal reviewers required before quotation/signature.

## Contract routing

- Always use the main B2B contract, project order, package/scope attachment, acceptance/change attachment, and major-terms confirmation.
- Add domain/hosting terms when the supplier provides or configures domain/hosting.
- Add ICP terms when a mainland China node or filing assistance is requested.
- Add security terms when the site has forms, backend, sensitive data, advanced security, or customer-managed deployment.
- Add source-code terms whenever source, repository, credentials, migration, or self-hosting is discussed.
- Add personal-information terms whenever forms, analytics, cookies, logs, member data, or cross-border data are involved.
- Add WebGL/special-visual terms for C+, D, or E projects.

## Human approval gates

- `A-` through `B` with high confidence: delivery lead review.
- `B+` through `C+`: delivery lead plus frontend/design review.
- Any D or E: creative-technical lead and delivery lead review.
- Any F: backend technical review.
- Payment, personal data, ICP, special licenses, source transfer, advanced security, or unusual liability: legal/security review as applicable.
- `LOW` confidence or conflicting evidence: no order draft until questions are answered.

## Experience iteration and privacy

The skill does not train itself. Improve it by curating anonymized cases after human review.

Store only:

- normalized requirement features;
- initial classification;
- approved final classification;
- changed fields and correction reason;
- contract-routing outcome;
- reusable lesson.

Remove or generalize customer names, domains, contacts, credentials, source code, confidential documents, exact deal prices, personal data, and unique identifying details. A human reviewer must approve every new case before it becomes a reference.

## Packaging

Create a publishable skill named `website-project-triage` with:

- `SKILL.md`: concise workflow, gates, and output contract;
- `references/classification-matrix.md`: A–F rules and boundary tests;
- `references/intake-checklist.md`: required sales questions;
- `references/contract-routing.md`: attachment selection;
- `references/anonymized-cases.md`: curated, non-identifying cases;
- `references/portable-integration.md`: instructions for hosts that call DeepSeek or other models;
- `agents/openai.yaml`: UI metadata.

Do not include prices or private customer information in the public package. Keep any internal commercial policy outside the skill.

## Reference loading contract

Before every classification, require the agent or host to load:

1. `references/classification-matrix.md`;
2. `references/intake-checklist.md`.

Load `references/contract-routing.md` when the request mentions a quote, order, contract, hosting, ICP filing, security, source code, personal data, payment, or API. Load `references/anonymized-cases.md` only for adjacent-grade ambiguity or precedent comparison. Load `references/portable-integration.md` only when integrating the skill into a non-Codex host.

If a required reference cannot be loaded, return `ESCALATE` and identify the missing reference rather than improvising a classification.

## Validation strategy

Run identical sales scenarios without and with the skill. Include at least:

1. A salesperson under time pressure asks for a direct quote from one reference URL.
2. A customer calls a WebGL reference “just a few animations.”
3. A static site request quietly includes login, payment, and batch product management.
4. A customer has not prepared materials but demands a fixed launch date.
5. A borderline `A+`/`B-` project where under-classification is tempting.

Success requires consistent classification, no prices, explicit missing questions, correct F separation, correct contract routing, and an enforced human-review gate.
