---
layout: default
---

# Implementation Roadmap — AI Output Readability

> **TL;DR:** 5 sprints, 4 pillars. Sprint 1 creates the style guide + glossary (foundation). Sprint 2-3 updates all 25 agents. Sprint 3-4 builds the doc quality reviewer + gate. Sprint 5 measures and iterates.

---

## Roadmap at a Glance

```mermaid
gantt
    title 5-Sprint Implementation Plan
    dateFormat  YYYY-MM-DD

    section Sprint 1 — Foundation
    ghl-doc-style-guide skill        :crit, s1a, 2026-03-17, 5d
    Domain glossary (50+ terms)      :s1b, 2026-03-17, 3d
    Mermaid templates per doc type   :s1c, 2026-03-20, 3d
    Before/after examples            :s1d, 2026-03-21, 2d

    section Sprint 2 — Agent Updates Batch 1
    5 review agents (Squad 5)        :s2a, 2026-03-24, 5d
    3 frontend reviewers (Squad 9)   :s2b, 2026-03-24, 3d
    2 developer agents (Squad 3)     :s2c, 2026-03-27, 3d
    Review consolidation update      :s2d, 2026-03-27, 3d

    section Sprint 3 — Agent Updates Batch 2
    Product agents (Squad 1)         :s3a, 2026-03-31, 3d
    QA agents (Squad 4)              :s3b, 2026-03-31, 3d
    Platform experts (Squad 7)       :s3c, 2026-04-01, 3d
    Doc quality reviewer agent       :crit, s3d, 2026-04-01, 4d

    section Sprint 4 — Quality Gate
    check-doc-readability.sh         :crit, s4a, 2026-04-07, 3d
    Integrate into 11-phase workflow :s4b, 2026-04-10, 3d
    Update CLAUDE.md + REGISTRY.json :s4c, 2026-04-10, 2d

    section Sprint 5 — Measure + Iterate
    Baseline measurement             :s5a, 2026-04-14, 2d
    Team feedback collection         :s5b, 2026-04-16, 3d
    Iterate rules based on feedback  :s5c, 2026-04-17, 3d
    Retrospective + document wins    :s5d, 2026-04-21, 2d
```

---

## Sprint 1: Foundation (Week of Mar 17)

**Goal:** Create the rules that all agents will follow.

```mermaid
graph LR
    A[Sprint 1] --> B["Deliverable 1: ghl-doc-style-guide skill"]
    A --> C["Deliverable 2: Domain glossary"]
    A --> D["Deliverable 3: Mermaid templates"]
    A --> E["Deliverable 4: Before/after examples"]
```

### Deliverable 1: `ghl-doc-style-guide` Skill

**File:** `.agentic-workspace/skills/ghl-doc-style-guide/SKILL.md`

```
Rule Categories:
├── Structure Rules
│   ├── Every doc starts with TL;DR (≤3 lines)
│   ├── Sections: TL;DR → Diagram → Key Points → Details
│   ├── Omit empty sections entirely
│   └── Headers use action verbs
│
├── Brevity Rules
│   ├── No paragraph > 4 lines
│   ├── No section > 15 lines without a visual
│   ├── Tables for comparisons (>2 items)
│   ├── Start with conclusion, then context
│   └── No filler phrases
│
├── Visual Rules
│   ├── Mermaid diagram FIRST for flows/relationships
│   ├── Min 1 diagram per 300 words
│   ├── Tables for structured data
│   └── Inline code annotations (not separate explanation)
│
├── Terminology Rules
│   ├── Domain glossary terms only
│   └── Never mix terms for same concept
│
└── Tone Rules
    ├── Write for the reader, not the author
    ├── Lead with what to DO
    └── Be direct — no hedging
```

### Deliverable 2: Domain Glossary

**File:** `.agentic-workspace/glossary.md`

| Canonical Term | Never Use | Definition |
|---------------|-----------|-----------|
| location | sub-account, client account, business | Multi-tenant scope unit under an agency |
| locationId | subAccountId, clientId, businessId | Primary tenant identifier (string) |
| agency | parent account, company, organization | Top-level account that owns locations |
| agencyId | parentId, companyId | Agency identifier (string) |
| pipeline | funnel, sales process, deal flow | Deal tracking workflow with stages |
| pipeline stage | deal stage, funnel step | Single step within a pipeline |
| Smart List | segment, filter group, saved search | Saved contact filter with dynamic membership |
| contact | lead, prospect, customer | Person record within a location |
| opportunity | deal, sale | Revenue-tracked item in a pipeline |
| workflow | automation, sequence, drip | Automated action sequence triggered by events |
| ... | ... | ≥50 terms total |

### Deliverable 3: Mermaid Templates

**Directory:** `.agentic-workspace/templates/mermaid/`

| Template File | Doc Type | Diagram Type |
|--------------|----------|-------------|
| `architecture.md` | Architecture docs | C4 Context + Container |
| `api-flow.md` | API specs | Sequence diagram |
| `data-flow.md` | Data flow docs | Flowchart LR |
| `decision.md` | ADRs | Decision tree |
| `review-summary.md` | Review reports | Pie chart + finding table |
| `user-flow.md` | User stories | Flowchart TD |
| `timeline.md` | Plans/roadmaps | Gantt chart |

**Example — Architecture Template:**

```mermaid
graph TD
    subgraph "C4 Context"
        User["👤 User Persona"]
        System["🔷 Our System"]
        External["📦 External System"]
        User -->|"action"| System
        System -->|"integration"| External
    end
```

### Deliverable 4: Before/After Examples

Create 3 before/after comparisons showing the transformation:

| Example | Before (Current) | After (Target) |
|---------|-----------------|----------------|
| Spec excerpt | 500-word prose section | TL;DR + Mermaid flow + table |
| Review output | 5 separate verbose reports | 1 consolidated report with severity table |
| Architecture doc | ASCII text flows + paragraphs | C4 Mermaid diagram + brief context |

---

## Sprint 2: Agent Updates — Batch 1 (Week of Mar 24)

**Goal:** Update the most impactful agents first — review agents + developer agents.

```mermaid
graph TD
    A[Sprint 2] --> B["10 agents updated"]

    subgraph "Squad 5: Code Review (5 agents)"
        C[maintainability-reviewer]
        D[architecture-reviewer]
        E[performance-reviewer]
        F[reliability-reviewer]
        G[security-reviewer]
    end

    subgraph "Squad 9: Frontend Review (3 agents)"
        H[frontend-core-reviewer]
        I[frontend-i18n-reviewer]
        J[security-reviewer shared]
    end

    subgraph "Squad 3: Development (2 agents)"
        K[frontend-developer]
        L[backend-developer]
    end

    B --> C & D & E & F & G
    B --> H & I & J
    B --> K & L
```

### Changes Per Agent

**For each of the 10 agents, add to their `.md` definition:**

```yaml
# Add to skills list:
skills:
  - ghl-doc-style-guide  # NEW

# Add to rules:
rules:
  - Follow ghl-doc-style-guide for ALL markdown output
  - Start every report/doc with 3-line TL;DR
  - Include Mermaid diagram for any flow or relationship
  - Use domain glossary terms from glossary.md
  - Omit sections with no findings (don't say "No issues found")
  - Use severity table format for findings
```

**Review consolidation update to `ghl-code-review-pr` skill:**

```
Add consolidation step:
1. Collect all 5 reviewer outputs
2. Deduplicate: merge findings about same issue across reviewers
3. Normalize severity: map all reviewer scales to Critical/High/Medium/Low
4. Group by file (not by reviewer)
5. Output single report: Summary table → Grouped findings → Raw details (collapsed)
```

---

## Sprint 3: Agent Updates — Batch 2 + Doc Reviewer (Week of Mar 31)

**Goal:** Update remaining agents and build the doc quality reviewer.

```mermaid
graph TD
    A[Sprint 3] --> B["Remaining agents updated"]
    A --> C["Doc quality reviewer created"]

    subgraph "Squad 1: Product (3 agents)"
        D[product-manager]
        E[design-reviewer]
        F[design-auditor]
    end

    subgraph "Squad 4: QA (3 agents)"
        G[unit-tester]
        H[e2e-tester]
        I[mutation-tester]
    end

    subgraph "Squad 7: Platform Experts (7 agents)"
        J[platform-product-manager]
        K[platform-product-designer]
        L[platform-infra-engineer]
        M[platform-services-engineer]
        N[platform-database-engineer]
        O[platform-frontend-engineer]
        P[platform-sdet-engineer]
    end

    subgraph "New Agent"
        Q["ghl-doc-quality-reviewer"]
    end

    B --> D & E & F & G & H & I & J & K & L & M & N & O & P
    C --> Q
```

### `ghl-doc-quality-reviewer` Agent Spec

```yaml
name: ghl-doc-quality-reviewer
type: reviewer
squad: 5 (Code Review — extended to docs)
skills:
  - ghl-doc-style-guide

checks:
  - id: TLDR_PRESENT
    rule: "First section must be TL;DR, ≤3 lines"
    severity: BLOCK

  - id: DIAGRAM_RATIO
    rule: "≥1 Mermaid diagram per 300 words"
    severity: BLOCK

  - id: SECTION_LENGTH
    rule: "No section > 15 lines without visual element"
    severity: WARN

  - id: PARAGRAPH_LENGTH
    rule: "No paragraph > 4 lines"
    severity: WARN

  - id: EMPTY_SECTIONS
    rule: "Remove sections with no substantive content"
    severity: WARN (auto-suggest removal)

  - id: GLOSSARY_COMPLIANCE
    rule: "All domain terms match glossary.md"
    severity: WARN

  - id: ACTION_VERB_HEADERS
    rule: "Section headers use action verbs"
    severity: INFO

verdict_logic:
  APPROVED: Zero BLOCKs
  CHANGES_REQUESTED: Any BLOCK present
```

---

## Sprint 4: Quality Gate + Workflow Integration (Week of Apr 7)

**Goal:** Automate enforcement so readability can't regress.

```mermaid
graph LR
    A["check-doc-readability.sh"] --> B{Checks}
    B --> C["TL;DR present?"]
    B --> D["Diagram count vs word count"]
    B --> E["Section length ≤ 15 lines"]
    B --> F["Glossary term compliance"]

    C -->|Missing| G["EXIT 1 (BLOCK)"]
    D -->|Ratio < 1:300| G
    E -->|Exceeded| H["WARN (non-blocking)"]
    F -->|Violations| H

    G --> I["Pipeline Fails"]
    H --> J["Pipeline Passes with Warnings"]

    style G fill:#ff6b6b,color:#fff
    style J fill:#ffd93d,color:#333
```

### Integration Points

| Integration | Where | How |
|------------|-------|-----|
| 11-phase workflow | Phase 4 (Implementation) | Run gate after each doc-producing step |
| Code review workflow | After `ghl-code-review-pr` | Run on consolidated review output |
| Spec validation | Phase 1 (Intake) | Extend `validate-spec.sh` with readability checks |
| CLAUDE.md | Agent roster table | Add `ghl-doc-quality-reviewer` to Squad 5 |
| REGISTRY.json | Agent registry | Auto-register new agent + skill |

---

## Sprint 5: Measure + Iterate (Week of Apr 14)

**Goal:** Prove it works, adjust what doesn't.

```mermaid
graph TD
    A[Sprint 5] --> B["Baseline Measurement"]
    A --> C["Team Feedback"]
    A --> D["Rule Iteration"]
    A --> E["Retrospective"]

    B --> B1["Measure: word count, diagram ratio, review time"]
    C --> C1["Survey: Is output easier to read? (1-5)"]
    D --> D1["Adjust thresholds based on false positives"]
    E --> E1["Document wins + share with team"]
```

### Measurement Plan

| Metric | How to Measure | Baseline (Sprint 5) | Target (Sprint 8) |
|--------|---------------|:--------------------:|:------------------:|
| Avg doc word count | `wc -w` on agent output | Establish | **-50%** |
| Docs with diagrams | Count Mermaid blocks | ~4% (1/25 agents) | **100%** |
| PR review cycle time | Git analytics | Establish | **-30%** |
| "What does this mean?" comments | PR comment search | Establish | **-40%** |
| Developer satisfaction | Survey (1-5 scale) | Establish | **≥4.0** |
| Glossary compliance | Automated check | Establish | **>95%** |

---

## Risk Mitigation

```mermaid
graph TD
    R1["Risk: Agents ignore style guide"] --> M1["Mitigation: Quality gate blocks non-compliant output"]
    R2["Risk: Over-constraining loses useful detail"] --> M2["Mitigation: Most rules are WARN, only TL;DR and diagrams BLOCK"]
    R3["Risk: Mermaid renders incorrectly"] --> M3["Mitigation: Tested templates + syntax validation in gate"]
    R4["Risk: Team finds new format unfamiliar"] --> M4["Mitigation: Before/after examples + training session"]
    R5["Risk: 25 agent updates introduce regressions"] --> M5["Mitigation: Batch updates (Sprint 2 + 3), test each batch"]
```

---

## Dependencies

```mermaid
graph LR
    A["Sprint 1: Style Guide + Glossary"] --> B["Sprint 2: Agent Updates Batch 1"]
    A --> C["Sprint 3: Agent Updates Batch 2"]
    A --> D["Sprint 3: Doc Quality Reviewer"]
    D --> E["Sprint 4: Quality Gate"]
    B & C & E --> F["Sprint 5: Measure + Iterate"]

    style A fill:#4ecdc4,color:#fff
    style F fill:#4ecdc4,color:#fff
```

**Critical path:** Sprint 1 (Foundation) blocks everything. Start here.

---

## How to Start

```
# Option 1: Plan the implementation
/ghl:plan agentic-workspace-docs/research/2026-03-14-ai-output-readability/SPEC.md

# Option 2: Go full autonomous
/ghl:lfg

# Option 3: Start Sprint 1 manually
# Create the style guide skill first
```

---

*This roadmap follows the proposed style: Gantt for timeline, flowcharts for dependencies, tables for data, no section > 15 lines.*
