# Website Project Triage Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use `superpowers:subagent-driven-development` (recommended) or `superpowers:executing-plans` to implement this plan task-by-task. Steps use checkboxes and must be completed in order.

**Goal:** Build, test, install, and publish a bilingual-triggered `website-project-triage` Skill that classifies website opportunities as A–F without outputting prices or approving contracts.

**Architecture:** Keep the executable workflow concise in `SKILL.md`; keep detailed decision rules, intake questions, contract routing, anonymized precedents, and non-Codex integration instructions in required reference files. Install the working copy under the local Codex skills directory, then mirror a privacy-safe, price-free copy into this repository for version control and external distribution.

**Tech Stack:** Markdown, YAML, Codex Skill packaging, `skill-creator` scripts, PowerShell, Git, GitHub CLI/API.

## Global Constraints

- The Skill must never output a selling price, internal settlement price, margin, discount, or approval to sign.
- A–E classify the frontend/experience; F is an orthogonal backend/business-system track.
- Every class supports `-`, standard, and `+`; adjacent ambiguity is classified upward.
- Chinese sales language and English technical terms must both trigger the Skill.
- The response follows the user's language; operational instructions remain concise Chinese with stable English technical terms.
- `classification-matrix.md` and `intake-checklist.md` are mandatory for every classification.
- Missing mandatory references produces `ESCALATE`; the model must not improvise.
- Public files must not contain customer identity, domains, contacts, credentials, source code, private documents, or exact deal prices.
- A human approval gate is mandatory before quotation, order drafting, or signature.

## Task 1: Freeze the approved design and evaluation contract

**Files:**

- Modify: `docs/superpowers/specs/2026-08-15-website-project-triage-skill-design.md`
- Create: `skill-evals/website-project-triage/scenarios.md`
- Create: `skill-evals/website-project-triage/expected-signals.md`

- [ ] Verify the design includes bilingual triggers, user-language output, non-Codex host behavior, and mandatory reference loading.
- [ ] Repair any encoding corruption so all A–F punctuation and Chinese text render correctly as UTF-8.
- [ ] Write five fixed sales scenarios:
  1. one reference URL plus pressure to quote immediately;
  2. a WebGL site described as “just a few animations”;
  3. a static-looking site that includes login, payment, and batch management;
  4. missing customer materials plus a fixed launch date;
  5. an `A+`/`B-` boundary case.
- [ ] Define observable success signals per scenario: no price, correct A–F separation, targeted questions, exclusions, risk flags, contract route, confidence, and human review.
- [ ] Confirm no fixture contains a real customer identity or deal price.

**Verification:**

```powershell
rg -n "TBD|TODO|真实客户|报价金额" docs/superpowers/specs/2026-08-15-website-project-triage-skill-design.md skill-evals/website-project-triage
```

Expected: no unfinished markers or identifying data; scenario references to “price requests” are allowed but no numeric commercial prices appear.

**Commit:**

```powershell
git add -- docs/superpowers/specs/2026-08-15-website-project-triage-skill-design.md skill-evals/website-project-triage/scenarios.md skill-evals/website-project-triage/expected-signals.md
git commit -m "Add website triage skill evaluation contract"
```

## Task 2: Run RED baseline pressure tests without the Skill

**Files:**

- Create: `skill-evals/website-project-triage/baseline-results.md`

- [ ] Dispatch the five frozen scenarios to clean subagents without loading the new Skill.
- [ ] Require each subagent to answer as a general sales-assistance model and preserve its result.
- [ ] Record failures against the expected signals, especially invented prices, under-classification, hidden backend scope, missing questions, missing contract attachments, and absent human approval.
- [ ] Summarize recurring rationalizations such as “the customer called it simple” or “the reference URL is enough.”
- [ ] Do not weaken scenarios after observing failures.

**Verification:**

```powershell
rg -n "Scenario [1-5]|Failure|Rationalization|Baseline" skill-evals/website-project-triage/baseline-results.md
```

Expected: all five scenarios have evidence-backed baseline findings and at least one explicit pass/fail judgment.

**Commit:**

```powershell
git add -- skill-evals/website-project-triage/baseline-results.md
git commit -m "Record website triage skill baseline failures"
```

## Task 3: Initialize the local Skill skeleton

**Files:**

- Create: `C:/Users/Longer/.codex/skills/website-project-triage/SKILL.md`
- Create: `C:/Users/Longer/.codex/skills/website-project-triage/agents/openai.yaml`
- Create directory: `C:/Users/Longer/.codex/skills/website-project-triage/references/`

- [ ] Confirm that `C:/Users/Longer/.codex/skills/website-project-triage` does not already contain user work; if it exists, inspect and preserve it rather than overwriting.
- [ ] Run the official scaffold command:

```powershell
python C:/Users/Longer/.codex/skills/.system/skill-creator/scripts/init_skill.py website-project-triage `
  --path C:/Users/Longer/.codex/skills `
  --resources references `
  --interface 'display_name=网站项目分级助手 / Website Project Triage' `
  --interface 'short_description=将独立站需求分为 A–F 并路由合同附件；不输出价格。' `
  --interface 'default_prompt=使用 $website-project-triage 判断以下网站需求的 A–F 分类、缺失信息、风险和合同附件，不要报价。'
```

- [ ] Inspect generated metadata and confirm all YAML strings are quoted.
- [ ] Confirm `default_prompt` explicitly mentions `$website-project-triage`.

**Verification:**

```powershell
Get-ChildItem -Recurse C:/Users/Longer/.codex/skills/website-project-triage
```

Expected: `SKILL.md`, `agents/openai.yaml`, and `references/` exist.

## Task 4: Implement the core Skill workflow

**Files:**

- Modify: `C:/Users/Longer/.codex/skills/website-project-triage/SKILL.md`

- [ ] Write bilingual trigger metadata covering Chinese sales phrases and English terms such as quote, website scope, template, CMS, API, WebGL, and contract routing.
- [ ] State when the Skill must and must not be used.
- [ ] Require loading `classification-matrix.md` and `intake-checklist.md` before every classification.
- [ ] Add conditional loading rules for contract, precedent, and portable-integration references.
- [ ] Implement the decision order: extract evidence → identify missing facts → classify A–E → classify F independently → apply modifier → route contract attachments → assign human reviewers.
- [ ] Enforce `NEEDS_INFO` when page/content, backend, graphics, hosting, or materials are materially unknown.
- [ ] Enforce upward classification at adjacent boundaries.
- [ ] Enforce no-price and no-signature gates.
- [ ] Require the fixed output fields in the approved order.
- [ ] Add a short quick-reference section and a red-flag/rationalization table derived from RED failures.

**Verification:**

```powershell
rg -n "classification-matrix|intake-checklist|NEEDS_INFO|ESCALATE|human_review|不得.*价格|A.*E|F" C:/Users/Longer/.codex/skills/website-project-triage/SKILL.md
```

Expected: all gates and required references are explicitly present.

## Task 5: Implement the required reference library

**Files:**

- Create: `C:/Users/Longer/.codex/skills/website-project-triage/references/classification-matrix.md`
- Create: `C:/Users/Longer/.codex/skills/website-project-triage/references/intake-checklist.md`
- Create: `C:/Users/Longer/.codex/skills/website-project-triage/references/contract-routing.md`
- Create: `C:/Users/Longer/.codex/skills/website-project-triage/references/anonymized-cases.md`
- Create: `C:/Users/Longer/.codex/skills/website-project-triage/references/portable-integration.md`

- [ ] Put the complete A–F definitions, modifiers, boundary tests, and escalation signals in `classification-matrix.md`.
- [ ] Put compact sales questions and minimum evidence requirements in `intake-checklist.md`.
- [ ] Map domain/hosting, ICP, security, source-code, personal-information, WebGL, payment, and API conditions to contract attachments in `contract-routing.md`.
- [ ] Add only synthetic or fully anonymized examples to `anonymized-cases.md`; include initial grade, approved grade, reason for change, and reusable lesson.
- [ ] Explain non-Codex integration in `portable-integration.md`: metadata matching, `SKILL.md` injection, required reference loading, user-language output, schema enforcement, and failure behavior.
- [ ] State explicitly that raw DeepSeek or another raw model API will not auto-discover the directory.
- [ ] State explicitly that anonymized case updates are curated versioned knowledge, not automatic model training.

**Verification:**

```powershell
rg -n "A-|A\+|B-|C-|D-|E-|F-|F\+" C:/Users/Longer/.codex/skills/website-project-triage/references/classification-matrix.md
rg -n "DeepSeek|auto-discover|自动.*发现|reference|ESCALATE" C:/Users/Longer/.codex/skills/website-project-triage/references/portable-integration.md
rg -n "客户名称|客户域名|联系电话|成交价格|API key|password" C:/Users/Longer/.codex/skills/website-project-triage/references
```

Expected: classifications and portability rules are present; privacy scan returns no real sensitive data.

## Task 6: Run GREEN pressure tests with the Skill

**Files:**

- Create: `skill-evals/website-project-triage/skill-results-v1.md`

- [ ] Give the same five frozen scenarios to clean subagents with the complete Skill and required references.
- [ ] Do not add hints beyond the Skill package.
- [ ] Compare every result against `expected-signals.md`.
- [ ] Fail a scenario if it outputs any price, hides F inside A–E, ignores a mandatory question, omits a required attachment, or lacks human review.
- [ ] Record verbatim evidence for every failure and classify the loophole.

**Verification:**

```powershell
rg -n "Scenario [1-5]|PASS|FAIL|Price gate|F track|Human review" skill-evals/website-project-triage/skill-results-v1.md
```

Expected: five explicit verdicts. Do not proceed while a high-risk scenario fails.

## Task 7: Refactor against observed loopholes and rerun

**Files:**

- Modify: `C:/Users/Longer/.codex/skills/website-project-triage/SKILL.md`
- Modify as needed: `C:/Users/Longer/.codex/skills/website-project-triage/references/*.md`
- Create: `skill-evals/website-project-triage/skill-results-final.md`

- [ ] Patch only the general rule that caused each failure; do not write scenario-specific answers into the Skill.
- [ ] Add or strengthen rationalization counters where the model evaded a rule.
- [ ] Rerun all five frozen scenarios using clean subagents.
- [ ] Confirm each scenario passes the no-price, classification, missing-info, routing, and review gates.
- [ ] Run at least two microtests: a clearly minimal `A-` case and a clearly complex `E+/F+` case.

**Verification:**

```powershell
rg -n "FAIL" skill-evals/website-project-triage/skill-results-final.md
```

Expected: no unresolved `FAIL` entries.

## Task 8: Validate package structure and metadata

**Files:**

- Validate: `C:/Users/Longer/.codex/skills/website-project-triage/`

- [ ] Run the official validator:

```powershell
python C:/Users/Longer/.codex/skills/.system/skill-creator/scripts/quick_validate.py C:/Users/Longer/.codex/skills/website-project-triage
```

- [ ] Scan for forbidden prices and private identifiers.
- [ ] Check all relative reference links resolve.
- [ ] Open `agents/openai.yaml` and verify bilingual UI metadata.
- [ ] Confirm the Skill returns in the user's language while keeping stable output keys.

**Verification:**

```powershell
rg -n "¥|人民币|内部结算|销售价|利润|折扣" C:/Users/Longer/.codex/skills/website-project-triage
```

Expected: validator succeeds and forbidden-price scan returns no matches except a rule explicitly prohibiting prices, if retained.

## Task 9: Publish the privacy-safe Skill package

**Files:**

- Create: `skills/website-project-triage/SKILL.md`
- Create: `skills/website-project-triage/agents/openai.yaml`
- Create: `skills/website-project-triage/references/classification-matrix.md`
- Create: `skills/website-project-triage/references/intake-checklist.md`
- Create: `skills/website-project-triage/references/contract-routing.md`
- Create: `skills/website-project-triage/references/anonymized-cases.md`
- Create: `skills/website-project-triage/references/portable-integration.md`

- [ ] Mirror the validated local package into the repository without adding private/internal commercial material.
- [ ] Compare local and repository copies byte-for-byte.
- [ ] Validate the repository copy with `quick_validate.py`.
- [ ] Commit only the Skill, evaluation records, and plan files.
- [ ] Push/upload the committed files to the GitHub `main` branch using the established repository workflow.
- [ ] Open the GitHub file URLs and verify Markdown renders correctly.

**Verification:**

```powershell
python C:/Users/Longer/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/website-project-triage
git diff --no-index -- C:/Users/Longer/.codex/skills/website-project-triage skills/website-project-triage
git status --short
```

Expected: validation succeeds, directory comparison has no content differences, and only intentional files are committed.

**Commit:**

```powershell
git add -- docs/superpowers/plans/2026-08-15-website-project-triage-skill.md skills/website-project-triage skill-evals/website-project-triage
git commit -m "Add website project triage skill v1"
```

## Task 10: Final operational acceptance

**Files:**

- Review: `skills/website-project-triage/`
- Review: `skill-evals/website-project-triage/skill-results-final.md`

- [ ] Test one natural Chinese sales message and one English technical brief.
- [ ] Confirm both trigger the same classification logic and return in the request language.
- [ ] Confirm a direct “give me a price” instruction still produces classification only.
- [ ] Confirm a missing mandatory reference produces `ESCALATE` rather than a guessed answer.
- [ ] Confirm any F result requests backend technical review.
- [ ] Confirm D/E and sensitive-data cases request the correct technical/security/legal review.
- [ ] Record the installed path, public GitHub path, version date, and known limitations in the final handoff.

