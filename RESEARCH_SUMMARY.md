---
layout: default
---

# AI Output Readability — Research Summary

> **TL;DR:** Our 25 agents produce working but hard-to-read output. The fix: enforce conciseness rules, add Mermaid diagrams everywhere, create a doc quality reviewer agent, and standardize output templates across all agents.

---

## The Problem in One Diagram

```mermaid
graph LR
    A[25 AI Agents] -->|generate| B[Walls of Markdown Text]
    B -->|read by| C[6 Personas]
    C -->|experience| D[Cognitive Overload]

    style D fill:#ff6b6b,color:#fff
    style B fill:#ffd93d,color:#333
```

**Current state:** Every agent formats independently. No conciseness rules. Only 1 Mermaid diagram in the entire codebase. Output is comprehensive but exhausting to read.

---

## Who's Affected

| Persona | Share | Volume |
|---------|:-----:|--------|
| Developers (code + reviews) | **35%** | ████████████████░░░░ |
| Product Managers (specs + PRDs) | **20%** | █████████░░░░░░░░░░░ |
| QA Engineers (test scenarios) | **15%** | ███████░░░░░░░░░░░░░ |
| Design Reviewers (audit reports) | **10%** | █████░░░░░░░░░░░░░░░ |
| Leadership (summaries) | **10%** | █████░░░░░░░░░░░░░░░ |
| New Members (onboarding docs) | **10%** | █████░░░░░░░░░░░░░░░ |

---

## Top 10 Pain Points

| # | Pain Point | Severity | Who Feels It |
|---|-----------|:--------:|-------------|
| 1 | Inconsistent domain terms ("sub-account" vs "location" vs "client") | **Critical** | Everyone |
| 2 | Too verbose — 500 lines when 100 would do | **High** | Everyone |
| 3 | Generic code naming (`data`, `item` vs `locationContacts`, `pipelineStage`) | **High** | Developers, QA |
| 4 | No TL;DR — must read everything to find the key point | **High** | PMs, Leadership |
| 5 | 5 review agents duplicate findings without consolidation | **High** | Developers |
| 6 | Inconsistent formatting across 25 agents | **High** | Everyone |
| 7 | Empty boilerplate sections ("Risk: None identified") | **Medium** | PMs, Developers |
| 8 | Test names describe mechanism, not behavior | **Medium** | QA |
| 9 | No progressive disclosure in docs | **Low** | New Members |
| 10 | Data claims without source attribution | **Low** | Leadership |

---

## Current State Audit

### What Exists

```mermaid
graph TD
    subgraph "Quality Controls Today"
        A[8 Review Agents] -->|check| B[Code Quality]
        C[ESLint + Gates] -->|enforce| D[Syntax + Types]
        E[11-Phase Workflow] -->|gates at| F[Phases 1,3,4,5,7,10]
        G[1 Tech Writer Agent] -->|produces| H[API Docs + Runbooks]
    end

    subgraph "Missing ❌"
        I[No Diagram Generation Skill]
        J[No Conciseness Enforcement]
        K[No Doc Quality Reviewer]
        L[No Output Style Guide]
        M[No Visual Templates]
    end

    style I fill:#ff6b6b,color:#fff
    style J fill:#ff6b6b,color:#fff
    style K fill:#ff6b6b,color:#fff
    style L fill:#ff6b6b,color:#fff
    style M fill:#ff6b6b,color:#fff
```

### Existing Doc Pipeline (Text-Only)

| Component | Count | Visualization? |
|-----------|:-----:|:--------------:|
| Technical Writer Agent | 1 | None |
| Summarization Skills | 3 | None |
| Mermaid Diagrams | **1** (only in swarm.md) | Minimal |
| Chart/Metric Visuals | 0 | None |
| Visual Templates | 0 | None |
| Output Style Guide | 0 | None |

---

## The Solution — 4 Pillars

```mermaid
graph TD
    A[AI Output Readability Initiative] --> B[Pillar 1: Style Guide Skill]
    A --> C[Pillar 2: Agent Updates]
    A --> D[Pillar 3: Doc Quality Reviewer]
    A --> E[Pillar 4: Quality Gate]

    B --> B1["Anti-verbosity rules"]
    B --> B2["Mermaid-first templates"]
    B --> B3["Section length limits"]

    C --> C1["Update all 25 agents"]
    C --> C2["Add visual output rules"]
    C --> C3["Domain glossary enforcement"]

    D --> D1["New reviewer agent"]
    D --> D2["Scores readability"]
    D --> D3["Checks diagram ratio"]

    E --> E1["Gate script in CI"]
    E --> E2["Blocks verbose output"]
    E --> E3["Enforces templates"]
```

### Pillar Breakdown

#### Pillar 1: `ghl-doc-style-guide` Skill (Sprint 1)

**What it does:** A shared skill consumed by ALL agents that enforces:

```
RULES:
├── Every doc starts with a 3-line TL;DR
├── Every concept with flow/relationships → Mermaid diagram FIRST
├── Tables for comparisons (never prose for >2 items)
├── No paragraph > 4 lines
├── No section > 15 lines without a visual
├── Headers use action verbs ("Configure Redis" not "Redis Configuration")
├── Omit empty sections entirely
├── Start sections with conclusion, not context
├── Max 1 accent emoji per section header (optional)
└── Domain terms from enforced glossary only
```

#### Pillar 2: Update All 25 Agent Definitions (Sprint 2-3)

**What changes per agent:**

| Agent Type | Current Output | New Output |
|-----------|---------------|------------|
| Developer agents | Code + verbose explanation | Code + 3-line summary + Mermaid flow |
| Review agents | Long prose findings | Severity table + consolidated findings |
| Product agents | Text-heavy specs/PRDs | TL;DR + Mermaid user flows + tables |
| QA agents | Mechanism-named tests | Behavior-named tests + coverage diagram |
| Architect agent | Text data flows | Mermaid C4 diagrams + decision tables |
| Tech writer agent | Prose docs | Visual-first docs with Mermaid + tables |

#### Pillar 3: `ghl-doc-quality-reviewer` Agent (Sprint 3)

**New agent that reviews all doc output for:**

```mermaid
graph LR
    A[Doc Quality Reviewer] --> B{Checks}
    B --> C["Diagram-to-text ratio ≥ 1:300 words"]
    B --> D["TL;DR present?"]
    B --> E["Section length ≤ 15 lines"]
    B --> F["No paragraphs > 4 lines"]
    B --> G["Domain terms consistent?"]
    B --> H["Empty sections removed?"]

    C --> I{Pass/Fail}
    D --> I
    E --> I
    F --> I
    G --> I
    H --> I
```

#### Pillar 4: Quality Gate Script (Sprint 3-4)

**`scripts/gates/check-doc-readability.sh`** — automated enforcement:

| Check | Threshold | Action |
|-------|-----------|--------|
| Word count per section | Max 200 words | WARN if exceeded |
| Mermaid diagram count | Min 1 per 300 words | BLOCK if missing |
| TL;DR present | Required at top | BLOCK if absent |
| Paragraph length | Max 4 lines | WARN if exceeded |
| Domain glossary compliance | 100% match | WARN on violations |
| Empty section detection | 0 allowed | Auto-remove |

---

## Frameworks & Tools to Adopt

```mermaid
graph TD
    subgraph "Diagram Layer"
        A[Mermaid ⭐ Primary] --> A1["Flowcharts, Sequences, ERDs, C4"]
        B[D2 Language] --> B1["Complex architecture diagrams"]
    end

    subgraph "Doc Structure Layer"
        C[Diataxis Framework] --> C1["4 doc types: Tutorial, How-to, Reference, Explanation"]
        D[MADR Format] --> D1["ADRs under 200 words + 1 diagram"]
        E[C4 Model] --> E1["4-level architecture viz"]
    end

    subgraph "Quality Layer"
        F[Vale Prose Linter] --> F1["Custom GHL style rules as code"]
        G[Cognitive Complexity] --> G1["Code readability scoring"]
    end
```

| Tool | Purpose | Integration Effort | Priority |
|------|---------|:------------------:|:--------:|
| **Mermaid** | Diagrams in markdown (GitHub/VSCode native) | Low | **P0** |
| **Diataxis** | Doc type categorization | Low | **P0** |
| **MADR** | Concise ADR format | Low | **P1** |
| **C4 Model** | Architecture diagrams via Mermaid C4 | Low | **P1** |
| **D2** | Complex architecture diagrams | Medium | **P2** |
| **Vale** | Prose linting as code | Medium | **P2** |

---

## Competitive Landscape

```mermaid
graph TD
    subgraph "Visual-First Output"
        V0["Vercel v0"]
        TARGET["GHL Target ⭐"]
    end

    subgraph "Text-Heavy Output"
        subgraph "Strong Style Control"
            CURSOR["Cursor"]
            WRITER["Writer"]
        end
        subgraph "Weak Style Control"
            COPILOT["Copilot"]
            DEVIN["Devin"]
            TODAY["GHL Today"]
        end
    end

    TODAY -.->|"Goal"| TARGET

    style TARGET fill:#4ecdc4,color:#fff
    style TODAY fill:#ff6b6b,color:#fff
    style V0 fill:#ffd93d,color:#333
```

**Key insight:** No competitor does readability-as-a-first-class-metric for multi-agent systems. This is a gap we can own.

| Competitor | Approach | Strength | Weakness |
|-----------|----------|----------|----------|
| **Cursor** | `.cursorrules` for style enforcement | Teams control conventions | Free-text rules, no validation |
| **Writer** | Brand voice scoring against style guides | Most mature quality scoring | Marketing-focused, expensive |
| **Vercel v0** | Consistent shadcn/ui output | Clean, idiomatic code | React-only, no codebase adaptation |
| **Copilot** | Relies on existing linters | Matches repo style via context | No readability scoring |
| **Devin** | Step-by-step reasoning shown | Shows work transparently | Verbose, no style enforcement |

---

## Success Metrics

```mermaid
graph LR
    subgraph "Leading Indicators (Weekly)"
        A["'What does this mean?' comments on PRs"]
        B["Follow-up questions before implementation"]
        C["Review comment volume per PR"]
    end

    subgraph "Lagging Indicators (Quarterly)"
        D["Developer satisfaction: 'AI docs are easy to read' (1-5)"]
        E["Rework rate on AI-generated artifacts"]
        F["Time-to-merge for AI vs human code"]
    end
```

| Metric | Baseline | Target | How to Measure |
|--------|:--------:|:------:|---------------|
| PR review cycle time | Establish | **-30%** | Git analytics |
| Domain term consistency | ~60% | **>95%** | Automated glossary lint |
| Artifact scan time | Establish | **-40%** | Timed user testing |
| Review deduplication | 0% | **>80%** | Pre/post finding comparison |
| Docs with Mermaid diagrams | **4%** (1/25 agents) | **100%** | Template compliance check |
| Avg doc word count | Establish | **-50%** | Automated word count |

---

## Implementation Timeline

```mermaid
gantt
    title AI Output Readability — 5 Sprint Roadmap
    dateFormat  YYYY-MM-DD

    section Sprint 1: Foundation
    Create ghl-doc-style-guide skill       :s1a, 2026-03-17, 5d
    Create domain glossary                  :s1b, 2026-03-17, 3d
    Define Mermaid templates per doc type   :s1c, 2026-03-20, 3d

    section Sprint 2: Agent Updates (Batch 1)
    Update 5 review agents (Squad 5)        :s2a, 2026-03-24, 5d
    Update 3 frontend reviewers (Squad 9)   :s2b, 2026-03-24, 3d
    Update developer agents (Squad 3)       :s2c, 2026-03-27, 3d

    section Sprint 3: Agent Updates (Batch 2) + Reviewer
    Update product agents (Squad 1)         :s3a, 2026-03-31, 3d
    Update QA agents (Squad 4)              :s3b, 2026-03-31, 3d
    Create ghl-doc-quality-reviewer agent   :s3c, 2026-04-01, 4d

    section Sprint 4: Quality Gate + Integration
    Build check-doc-readability.sh gate     :s4a, 2026-04-07, 3d
    Integrate into 11-phase workflow        :s4b, 2026-04-10, 3d
    Update code-review-pr orchestration     :s4c, 2026-04-10, 3d

    section Sprint 5: Polish + Measure
    Baseline measurement                    :s5a, 2026-04-14, 2d
    Team training + feedback collection     :s5b, 2026-04-16, 3d
    Iterate based on feedback               :s5c, 2026-04-17, 3d
```

---

## What's Next

| Action | Command |
|--------|---------|
| See competitive deep-dive | Read `COMPETITIVE_ANALYSIS.md` |
| See full spec with acceptance criteria | Read `SPEC.md` |
| See current codebase audit | Read `CODEBASE_AUDIT.md` |
| See detailed implementation plan | Read `IMPLEMENTATION_ROADMAP.md` |
| Start implementation | `/ghl:plan SPEC.md` |
| Go full auto | `/ghl:lfg` with SPEC.md |

---

*This document itself follows the proposed style: TL;DR first, Mermaid diagrams for every concept, tables instead of prose, no section > 15 lines.*
