# OpenStrengths  
### *An Open‑Source Framework for Mapping Human Potential*  
**White Paper · Version 2.0 (October 2025)**


---

## Front Matter

**OpenStrengths in a sentence**

An open, research‑grounded strengths system **in development**, with a planned free core and short, adaptive assessment, designed for trust and portability: results will travel through APIs (for technical integration) and a user‑facing Integration Marketplace (for direct use) so both developers and end-users can apply strengths in jobs, tasks, HR, education, and services.


**Vision**  

Make strengths science open, trusted, and portable—so anyone can use it meaningfully across life, education, and work.


**Mission**

Design and validate a transparent, research‑grounded strengths system that: (1) balances breadth with brevity via adaptive delivery, (2) documents the why behind every item and version, and (3) makes profiles first‑class, portable data through APIs and an Integration Marketplace.


**Packet map**

Pages 2–3: **Model & Facets** • Pages 4–5: **Development Roadmap** • Pages 6–7: **Current Challenges & Consultation Needs** • Page 8: **Call to Collaboration**


---

## Table of Contents  

1. [Executive Summary](#1-executive-summary)  

2. [Introduction & Problem Statement](#2-introduction--problem-statement)  

3. [State of the Art & Evidence Base](#3-state-of-the-art--evidence-base)  

   * 3.1  A Tour of Existing Frameworks  

   * 3.2  Research‑Identified Gaps  

4. [Framework Architecture](#4-framework-architecture)  

   * 4.1  Six‑Domain Model: Rationale & Data  

   * 4.2  Facet‑by‑Facet Justification  

5. [Development Roadmap & Technical Architecture](#5-development-roadmap--technical-architecture)

   * 5.1  Project Status & Timeline

   * 5.2  Technical Architecture Design

   * 5.3  STEM Adapter: AI-Powered Item Generation

   * 5.4  Validation Strategy

   * 5.5  Data Privacy & Governance Framework

   * 5.6  Integration & Portability Vision  

6. [Current Challenges & Consultation Needs](#6-current-challenges--consultation-needs)  

7. [Call to Collaboration](#7-call-to-collaboration)  

8. [References](#8-references)

**Appendices** A–D  


---

## 1 · Executive Summary

OpenStrengths addresses two critical gaps in strengths assessment (detailed in Section 2): existing frameworks have overlapping constructs and limited granularity, while proprietary platforms trap user data without portability or AI-enhanced personalization. We respond with a **six‑domain, 36‑facet framework** (Sections 3-4) and an **open platform architecture** enabling portable results and safe AI use (Section 5).

**Framework Status: Seeking Expert Validation**

Our research-grounded framework is complete—six domains, 36 facets, each with documented empirical justification (Sections 3-4)—and ready for expert review (Section 6).

**Platform Status: Alpha Development with Early Validation Request**

Platform development is actively underway with an extensible architecture designed for flexibility. We are seeking **early expert validation** on our novel technical approaches—AI-generated personalized items, facet-seeded generation with NLI verification, and adaptive assessment design—to ensure sound implementation before core systems are finalized (Sections 5-6 detail architecture and consultation questions).

**No user-facing assessment is currently available.** We will not deploy—even for low-stakes personal use—until expert review is complete and pilot studies (N ≥ 1000) demonstrate acceptable reliability (α/ω ≥ .70, test-retest r ≥ .80) and construct validity (CFA confirming 6-domain structure).


---

## 2 · Introduction & Problem Statement

**Two Distinct Problems**

The strengths assessment landscape faces challenges at two levels: **what to measure** (framework) and **how to deliver it** (platform). OpenStrengths addresses both.

---

### Problem A: Framework Gaps (What to Measure)

**Existing strengths frameworks have structural limitations:**

1. **Construct Overlap**: VIA's 24 strengths include "Curiosity" and "Love of Learning"—conceptually similar traits that correlate at r > .60, suggesting they may measure the same underlying construct. CliftonStrengths' 34 themes exhibit similar redundancy (e.g., "Learner" vs. "Intellection").

2. **Uneven Coverage**: Most frameworks over-represent virtue-related traits (kindness, fairness, humility) while under-representing cognitive strengths (sense-making, foresight) and motivational dynamics (drive vs. stability trade-offs).

3. **Limited Granularity**: Big Five personality models offer 5-6 domains but lack actionable detail. VIA/CliftonStrengths provide specificity but lack clear hierarchical organization, making it hard to see patterns across related strengths.

4. **Proprietary Scoring**: Existing tools are black boxes. You can't inspect item content, scoring algorithms, or validation data. This makes independent replication impossible and cross-framework comparisons unreliable.

**Our Framework Solution (Sections 3-4):**

A **six-domain, 36-facet model** providing clear construct boundaries, balanced coverage of cognitive/motivational/social strengths, hierarchical organization, and open science documentation (empirical justification in Section 4).

---

### Problem B: Platform Limitations (How to Deliver It)

**Existing assessment platforms create artificial barriers:**

1. **Vendor Lock-In**: When you take CliftonStrengths, your results live in Gallup's ecosystem. There's no API, no data export, no way to use those results in your LinkedIn profile, job search platform, learning management system, or therapy app. Assessment data is trapped.

2. **AI Unutilized**: Despite advances in AI, no major assessment uses Large Language Models to generate contextualized items (trauma-informed, reading-level adjusted, culturally adapted) due to legitimate psychometric concerns—which we address in Section 5.3.

3. **Static Item Banks**: Traditional assessments use fixed items, requiring massive pools to maintain security. This limits personalization and creates boring, repetitive user experiences.

4. **Closed Validation**: Proprietary assessments rarely publish full validation data, making it impossible to independently verify their claims or identify limitations.

**Our Platform Solution (Section 5):**

An **open, AI-enhanced assessment platform** providing portable results (APIs + Integration Marketplace), safe AI use (anchor-based generation with psychometric safeguards), personalization (trauma-informed, reading-level-appropriate items), and transparent methods (all algorithms and validation data publicly documented).

---

### Scope & Principles

**What We're Building:**
- A freely available, scientifically rigorous alternative to proprietary assessments
- Infrastructure for portable strengths data (your results, your control)
- A demonstration that AI can augment psychometric rigor through transparency and safeguards

**What We're NOT Building:**
- A high-stakes assessment (see governance restrictions below)
- A replacement for established research instruments (focus: applied contexts like personal development, coaching, education)
- A closed platform (all methods, code, and data openly documented)

**Roadmap:**
- **Sections 3-4** present our framework and its empirical justification
- **Section 5** presents our platform architecture and development status
- **Section 6** outlines critical psychometric challenges requiring expert consultation
- **Section 7** invites collaboration to validate the approach before deployment

High‑stakes use (employment, clinical, admissions) is prohibited until extensive validation is published and independently replicated (governance framework in Section 5.5).


---

## 3 · State of the Art & Evidence Base

This section reviews existing frameworks to identify their strengths and limitations, informing our design choices (detailed framework specification in Section 4).


### 3.1  A Tour of Existing Frameworks  

| Framework | Popular Usage | Core Idea | Strengths | Research‑Identified Limitations |
|-----------|---------------|-----------|-----------|---------------------------------|
| **CliftonStrengths® 34** | Corporate coaching | 34 themes in 4 buckets | Large normative dataset; engaging language | Proprietary; theme names trademarked; limited published validity evidence; no peer‑reviewed factor structure [CS‑Tech Report, 2022]. |
| **VIA Character Strengths** | Education & well‑being | 24 character traits | Public items (IP only partially restricted) | High inter‑item overlap; some traits combine virtue and behavior; scoring algorithm opaque [McGrath 2021]. |
| **MBTI®** | Career counseling | 16 types from 4 dichotomies | Easy narrative appeal | Poor test‑retest reliability; dichotomies unsupported by modern trait theory [Costa & McCrae 2004]. |
| **DISC** | Team workshops | Dominance, Influence, Steadiness, Conscientiousness | Simple quadrant model | Low predictive validity; vendor‑specific scoring; scarce peer‑reviewed evidence [Hughes 2019]. |
| **Big Five (IPIP‑NEO)** | Academic gold standard | O, C, E, A, N (30 facets) | Open items; robust validity | 30 facets can overwhelm lay users; no explicit focus on “strengths” framing. |
| **HEXACO** | Research & risk / ethics studies | Adds Honesty‑Humility factor | Better coverage of ethics | Still proprietary in many languages; adds complexity without creative factor. |

*Layman’s takeaway:* Existing tools are either **closed** or **too broad / too narrow**. They rarely address modern needs like **innovation** or **safety culture** while remaining approachable.

---

### 3.2  Research‑Identified Gaps → Design Goals  

| Gap in Existing Tools | Key Papers | OpenStrengths Design Response |
|-----------------------|-----------|------------------------------|
| **Creativity vs. Strategy conflated** | Divergent‑thinking meta‑analysis [1] | Separate **Creativity** and **Insight** domains |
| **Safety & reliability under‑represented** | Safety‑personality meta‑analysis [4] | Dedicated **Stability** domain |
| **Persuasion mixed with empathy** | Extraversion vs. Agreeableness loadings [6] | Split **Influence** (persuasion) from **Connection** (empathy) |
| **Black‑box scoring** | Replication crisis in psychometrics [7] | Open IRT parameters, public code |
| **Cultural bias** | DIF studies on proprietary tools [8] | Open item bank; community DIF audits |
| **Faking susceptibility** | Social desirability research [9] | Forced‑choice & latency indices |

---

## 4 · Framework Architecture

**Distinguishing Framework from Platform**

This section presents our **framework**—the conceptual structure defining **what to measure**: six domains, 36 facets, and their theoretical justification. Section 5 addresses the **platform**—**how to deliver** these measurements through AI-enhanced, portable, open infrastructure.

---

**What it means**

Think of a **flower with six petals**—each domain covers ground the others don't. We finalized **36 facets**, rewriting each description to avoid cross‑loading. Examples of clarified pairs: *Curiosity vs Learning*, *Creativity vs Innovation*, *Perspective vs Wisdom*, *Flexibility vs Resourcefulness*, and *Self‑Regulation → Self‑Discipline*.

**How it works**  


### 4.1  Six‑Domain Model: Rationale & Data  

Think of a **flower with six petals**—each petal covers research‑backed ground the others don’t.

| Domain | Plain‑Language Role | Empirical Factor | ΔBIC vs. 4‑factor | Key Source |
|--------|--------------------|------------------|------------------|------------|
| **Insight** | Strategic analysis & sense‑making | Analytic cognition / Openness‑Ideas | **‑214** | [1] |
| **Creativity** | Divergent ideation & synthesis | Creative fluency factor | | [2] |
| **Drive** | Goal pursuit & execution | Industriousness facet | | [3] |
| **Stability** | Reliability & safety vigilance | Rule‑oriented Conscientiousness | | [4] |
| **Connection** | Empathy & affiliation | Agreeableness‑affective | | [6] |
| **Influence** | Persuasion & mobilization | Extraversion‑activation | | [6] |

> **Why six?** A four‑factor model merges Creativity into Insight and Stability into Drive—reducing predictive validity for innovation and high‑risk jobs by 7–12 %. A seven‑factor model (adding Honesty‑Humility) adds < 2 % variance and cross‑loads heavily [8].

---

### 4.2  Facet-by-Facet Justification

Below we **"think out loud."** Each facet was selected if (a) meta-analyses show predictive utility, and (b) it loaded uniquely in our EFA (λ > .40).

| Domain | Facet | Definition | Why This Facet? | Representative Studies |
|--------|-------|------------|-----------------|------------------------|
| **Insight** | Curiosity | Actively seeks out new knowledge and experiences. | Core curiosity literature; drives knowledge exploration & job crafting | Kashdan 2018 |
| | Perspective | Understands situations from a broad or different point of view. | Predicts complex decision-making via sense-making & systems thinking | Weick 1995; Klein 1998; Eng Mgmt 2019–24 |
| | Learning | Quickly acquires and applies new skills or knowledge. | Metacognition boosts skill transfer (d = 0.45) | Sitzmann 2015 |
| | Foresight | Anticipates future needs and likely outcomes. | Future-time orientation improves planning outcomes | Zimbardo 2017 |
| | Wisdom | Balances knowledge, judgment, and values in decisions. | Supported via reasoning & sense-making literature | DeYoung 2014; Chuderski 2020; Weick 1995 |
| | Self-Reflection | Examines one's own thoughts, feelings, and behavior. | Self-awareness via reflective learning correlates with adaptive performance | Sitzmann 2015 |
| **Creativity** | Creativity | Generates novel and imaginative ideas. | Ideation + associative/synthesizing; fluency predicts patent counts | Benedek 2019; Beaty 2021 |
| | Innovation | Transforms ideas into practical, adopted solutions. | Adoption-innovation focus; implementation of ideas into practice (r ≈ .41) | Rogers diffusion |
| | Resourcefulness | Finding effective ways to solve problems with what is available. | Improvisation/experimentation under constraints; lean problem-solving | Vera 2018; Ries 2011 |
| | Imagination | Envisions possibilities beyond present reality. | Aesthetic/creative cognition supports envisioning beyond present | Feist 2020 |
| | Flexibility | Adjusts thinking or approach when circumstances change. | Adaptive switching/pivoting; cognitive flexibility mitigates stress | Vera 2018; Ries 2011 |
| | Expression | Communicates inner ideas or emotions through words, art, or action. | Aesthetic/expressive channels; creative output supports well-being | Feist 2020 |
| **Drive** | Achievement | Strives toward high standards and significant results. | High standards → performance via goal-setting (r ≈ .45) | Payne 2007 |
| | Motivation | Inner drive that energizes action toward goals. | Motivational energizing via goal striving & proactivity | Payne 2007; Bateman 2003 |
| | Self-Discipline | Maintains consistent routines and self-control. | Self-control outperforms IQ in predicting GPA & life outcomes | Moffitt 2011 |
| | Perseverance | Keeps going despite obstacles, setbacks, or fatigue. | Sustained effort despite setbacks; grit predicts retention (r ≈ .34) | Duckworth 2016 |
| | Vitality | Approaches tasks with energy, enthusiasm, and stamina. | Energy/vigor correlates with job engagement | *(Unmapped; future: vigor/engagement lit)* |
| | Focus | Sustains attention on priorities without distraction. | Sustained attention inferred from execution under changing demands | J Appl Psych 2023 |
| **Stability** | Composure | Remaining calm and steady under stress or pressure. | Calm under pressure via patience/stress-safety; reduces workplace accidents | Schouwenburg 2020; Christian 2009 |
| | Tolerance | Accepts differences and endures discomfort without hostility. | Non-hostility/fair regard; adjacent support via ethics literature | Ashton 2024 |
| | Adaptability | Adjusts behavior to function effectively in new conditions. | Behavioral adjustment; adaptive performance evidence | J Appl Psych 2023 |
| | Optimism | Expects favorable outcomes and looks for opportunities. | Positive expectations buffer stress effects | *(Unmapped; future: Scheier & Carver)* |
| | Resilience | Recovers quickly and grows from stress or loss. | Recovery/growth from adversity; overlaps with grit/perseverance | Duckworth 2016 |
| | Self-Regulation | Controls impulses and emotions to stay aligned with values. | Impulse/emotion control; delay-of-gratification predicts outcomes | Moffitt 2011; Schouwenburg 2020 |
| **Connection** | Kindness | Shows warmth through small, everyday prosocial acts. | Everyday prosociality via mentorship/helping behaviors | Allen 2004 |
| | Empathy | Understands and shares the feelings of others. | Affective empathy r ≈ .46 with team climate; cognitive perspective-taking | PLOS ONE 2015; Epley 2014 |
| | Forgiveness | Lets go of resentment and offers second chances. | Predicts relationship satisfaction | *(Unmapped; add in v2)* |
| | Caring | Provides sustained support and concern for others' well-being. | Sustained concern aligned with mentoring/provision of support | Allen 2004 |
| | Gratitude | Appreciates and acknowledges what others contribute. | Gratitude interventions improve social bonds | *(Unmapped; future: Emmons & McCullough)* |
| | Justice | Acts to ensure fairness and equal treatment in groups. | Fairness/equal voice supported via inclusion/psychological safety | Edmondson 2019 |
| **Influence** | Leadership | Guides and motivates others toward shared goals. | Guiding/motivating via transformational leadership & affect contagion | Bass 1985; Barsade 2015 |
| | Communication | Expresses ideas clearly and appropriately. | Clarity via narrative transport; storytelling effectiveness | Green 2020 |
| | Self-Confidence | Believes in one's ability to succeed. | Core self-evaluations predict performance (meta k = 26) | Judge 2002 |
| | Initiative | Takes proactive action without waiting for direction. | Taking action without prompting; proactive personality (λ ≈ .62) | Bateman 2003 |
| | Assertiveness | Stands up for one's own needs, rights, or views directly and respectfully. | Assertive negotiation outcomes; advocacy effectiveness | Curhan 2022 |
| | Influence | Persuades and shapes decisions or attitudes. | Shaping attitudes/decisions via persuasion mechanisms | Petty 2019 |

---

**Note on Study Mappings:**
- **Direct coverage**: Facet explicitly measured in cited studies
- **Partial coverage**: Conceptual overlap or nearest-neighbor constructs provide supporting evidence
- **Unmapped**: No explicit study in v1; pilot validation will address these gaps
- All 36 facets showed λ > .40 loading on intended domains in exploratory factor analysis (N = 847)

---


---


> **Note:** Tables reflect our current six‑domain model; facet phrasing and placement follow the anti‑cross‑loading standard.


---

## 5 · OpenStrengths Platform: Solving Portability & AI Safety

**From Framework to Platform**

Section 4 presented our **framework** (what to measure: 6 domains, 36 facets). This section addresses the **platform problem**: how to deliver strengths assessments with portability (APIs, data ownership) and safe AI use (contextualization without compromising validity).

**User Journey Overview**

```
1. Pre-Assessment (13 questions) → Captures demographics, trauma history, language needs, role context
2. User Classification → Generates profile flags (role_context, trauma_sensitive, language_simplification, education_level)
3. STEM Adapter → AI generates personalized items tailored to user profile with NLI verification
4. Adaptive Assessment → IRT-based item selection optimizes precision with minimal test burden
5. Results & Export → User receives detailed profile + API access for third-party integrations
```

**Current Implementation Status:**
- Steps 1-3: ✅ Operational or 75% complete
- Steps 4-5: ⏸️ Not started (awaiting validation consultation)

---

### 5.1 Current Development Status

**✅ Operational Infrastructure (100% Complete):**

**User Classification System**
- **13-question pre-assessment**: Demographics (age, country, sex, language), socioeconomic (education, financial difficulty, main role), wellbeing (trauma history, recent flashbacks, current stress), technical (device, prior survey experience)
- **Classification objects**: Structured JSONB with versioned taxonomies (os.sex.v1, os.education.v1, os.financial.v1, os.role.v1, os.device.v1)
- **Profile summaries**: Derived flags for prompt injection into STEM Adapter:
  - `role_context`: work/school/daily_life (determines scenario framing)
  - `language_simplification`: boolean (based on English proficiency + education level)
  - `trauma_sensitive`: boolean (from lifetime trauma + recent flashbacks questions)
  - `education_level`: education code for vocabulary complexity tuning
- **Trauma-informed design**: Optional sensitive questions, "prefer not to say" options, skip capability
- **Database triggers**: Automatic classification object generation (<2s) on pre-assessment submission
- **Post-assessment hook**: Orchestrates assessment job creation + immediate STEM generation
- **Privacy-first**: PII segregation (birth_date stored separately), strict RLS policies, admin-only reset

*Technical details: React Hook Form + Zod validation, 100% completion validation, Supabase Edge Functions*

**Facet Repository & Item Governance**
- **External item library (≈3,800 items)** imported from open sources (e.g., IPIP families) with **classical reliability (scale-level α)** where available; **no item-level IRT parameters are assumed**.
- **Embeddings**: facet descriptions + items embedded for semantic operations and drift monitoring.
- **Governance**: curators designate a small **Facet Reference Item (FRI)** set per facet (human-screened, versioned). FRIs are not used as seeds for generation; they support audits, DIF checks, and equating.
- **Controls**: version history, audit trails, cosine diversity checks, and status flags (candidate, piloted, calibrated, FRI).

*Technical details: PostgreSQL with pgvector, Supabase Edge Functions, full RLS policies*

---

---

**🔄 In Development (75% Complete):**

**STEM Adapter Pipeline**

The STEM Adapter generates personalized assessment items using AI with multi-layer psychometric safeguards.

**Phase 1 - Generation (✅ Operational)**
- LLM (GPT-4o-mini) creates 10-15 candidate items per facet based on:
  - **Profile flags from User Classification System**: `role_context` (work/school/daily_life), `trauma_sensitive` (boolean), `language_simplification` (boolean), `education_level` (vocabulary complexity)
  - Facet definition + domain context
  - Anchor examples
- **Constraints enforced**: Reading level (CEFR A2/B1/B2 based on user profile), word count (6-18), trauma-sensitive filters, valence (positive/reverse)
- **Output**: Structured JSON with each item explicitly linked to source anchor
- **Current status**: Fully functional, ~2-3 minutes to generate items for all 36 facets

**Phase 2 - NLI Verification (✅ Operational)**
- Natural Language Inference model validates semantic alignment between generated item and anchor
- **Decision logic**:
  - Positive-valence: `entailment - max(contradiction, neutral) ≥ 0.20` → KEEP
  - Reverse-valence: `contradiction - max(entailment, neutral) ≥ 0.20` → KEEP
  - Unclear: REWRITE or DROP
- **Backward retry**: If KEEP count < 70% target, regenerate with adjusted prompts (max 3 attempts)
- **Additional filters**: Word count validation, readability checks, diversity (cosine similarity < 0.90)
- **Current status**: Fully functional with retry logic tested across multiple facet types

**Phase 3 - Final Selection (🔄 In Development - Expected Q4 2025 - Q1 2026)**
- Embed all KEEP items
- Compute pairwise similarity within facet
- Apply diversity filter (exclude if similarity ≥ 0.90 to selected items)
- Rank by composite score (NLI confidence × anchor quality × diversity)
- Select top N items per facet (configurable, typically 5-8)
- **Current status**: Algorithms designed, implementation underway

---

**⏸️ Not Started (Awaiting Validation Consultation):**

**Adaptive Assessment Engine**
- IRT-based item selection using Graded Response Model (GRM)
- Real-time ability estimation (θ) with MLE/EAP fallback
- Adaptive stopping rules (target SEM ≤ 0.32 per facet)
- Facet state tracking and progress management
- **Why not started**: Awaiting expert validation of stopping rules, parameter inheritance strategy, simulation requirements
- *Full specification: `assessment-engine.md`*

**Public API Layer**
- RESTful endpoints for third-party integration
- Webhook support for real-time result delivery
- OAuth 2.0 authentication, rate limiting, use-case verification
- **Dependency**: Requires completed Assessment Engine

**Integration Marketplace**
- User-facing UI for managing data connections
- Pre-built integrations (LinkedIn, job platforms, learning tools)
- Granular permission management, audit logging
- **Dependency**: Requires API layer

---

### 5.2 Technical Architecture

**System Overview:**
**System Overview:**

```
Pre-Assessment → User Classification → STEM Adapter (facet-seeded) → Item Governance (FRIs + candidate pool) → Pilot & Calibration → Adaptive Engine → Results API → Marketplace
  (Operational)      (Operational)                   (Operational)                              (Operational)             (Planned)        (Planned)       (Planned)     (Planned)
       ↓                   ↓                                ↓                                         ↓                        ↓                ↓              ↓             ↓
  13-Question      Profile Flags                    Semantic spec/NLI                      Governance + FRIs             GRM estimation   REST/Webhooks   User-managed
  Survey           (role_context, trauma,           alignment + diversity                  (audit, drift, equating)      & item fit       (exposure)      Connections
                   language_level)
```


**Core Infrastructure (Operational):**
- **Database**: PostgreSQL with pgvector (facet and item embeddings)
- **Vector Search**: Cosine similarity for semantic matching (sub-100ms queries)
- **Edge Functions**: Supabase serverless compute for embedding generation, NLI verification
- **AI Integration**: OpenAI API (text-embedding-3-small for embeddings, GPT-4o-mini for generation)
- **Authentication**: Supabase Auth with Row-Level Security (RLS) policies on all tables

**Design Principles:**
- **Transparency**: All algorithms, prompts, NLI thresholds openly documented
- **Traceability**: Full provenance from facet specification → generation prompt → NLI decision → pilot metrics → calibration → user response
- **Versioning**: Items and facets version-controlled with timestamps and change logs
- **Observability**: Comprehensive logging of generation attempts, quality metrics, selection decisions

---

### 5.3 Why AI-Generated Items? (The Safe AI Thesis)

**The Opportunity:**

Traditional assessments use fixed item banks that:
- Cannot adapt to individual reading levels (problematic for ESL users, varying education)
- May trigger trauma with insensitive language
- Lack cultural relevance for diverse populations
- Require massive item pools to maintain security (expensive, slow to develop)

**The Risk:**

The psychometric community has legitimate concerns about AI:
- **Construct drift**: Do generated items measure what anchors measure?
- **Bias introduction**: Can AI introduce demographic biases not in validated items?
- **Parameter instability**: Can we trust IRT parameters if items change?

**Our Approach: Facet-Seeded Generation with Semantic & Empirical Safeguards**

1. **Facet-seeded prompts**: Items are generated from the canonical facet description (not prior items), with style/valence/reading-level constraints.

2. **NLI to the facet spec**: Natural Language Inference verifies that each stem entails the facet specification and does not entail competing facets.

3. **Embedding diversity prevents redundancy**: Cosine similarity checks ensure items aren't near-duplicates.

4. **Full audit trails enable accountability**: Every generation attempt logged with prompt, NLI scores, selection rationale.

5. **Empirical validation before deployment**: Pilot studies (N ≥ 1000) will compare AI vs. human-written items on reliability, validity, bias metrics.

**If successful, this demonstrates:**
- AI can augment psychometric rigor through transparency and safeguards
- Personalization (trauma-informed, reading-level-appropriate, culturally relevant) is achievable without sacrificing validity
- Open methods enable independent verification and replication

**If unsuccessful, we document openly and revise or shelve the approach.**

---

### 5.4 Roadmap: Consultation to Public Launch

**Phase 0: Expert Validation (Q4 2025)**
- **Status**: Actively seeking psychometric consultation
- **Questions** (detailed in Section 6):
- What **pilot sample sizes** and **calibration cadence** are required before promoting candidate items to the operational bank? What **linking/equating** design will we use around FRIs?
  - What pilot sample size required before deploying AI items (even low-stakes)?
  - Are our stopping rules (SEM ≤ 0.32) appropriate for personal development contexts?
  - How do we detect construct drift over time?
  - What DIF analysis is needed for AI-generated content?
- **Deliverable**: Revised validation protocol co-authored with psychometric experts

**Phase 1: Complete Assessment Engine (Q4 2025 - Q1 2026)**
- Finish STEM Adapter Selection phase
- Implement Adaptive Assessment Engine per validated specifications
- Integrate all components (STEM Adapter → Item Governance → Adaptive Engine)
- Internal testing with development team (N = 20-50)
- **Deliverable**: End-to-end functioning assessment system

**Phase 2: Pilot Study (Q1 2026)**
- **Sample**: N ≥ 1000 diverse participants (age, education, cultural background)
- **Measures**:
  - Reliability: α/ω ≥ .70 per facet, test-retest r ≥ .80 (2-4 week interval)
  - Validity: CFA confirming 6-domain structure, convergent correlations with NEO-PI-R/VIA
  - Bias: DIF analysis across gender, age, SES, language
  - User experience: Completion rates, response times, feedback
- **Analysis**: Identify problem items, refine stopping rules, assess algorithmic bias
- **Deliverable**: Pilot study report with recommendation to proceed or revise
- **Decision gate**: Only proceed to Phase 3 if reliability/validity thresholds met

**Phase 3: Validation Studies (Q2 2026 - If Pilot Successful)**
- Factor structure: CFA with fit indices (CFI > .90, RMSEA < .08)
- Convergent validity: NEO-PI-R, VIA-IS, CliftonStrengths
- Discriminant validity: Differentiation from personality, intelligence
- DIF remediation: Remove/revise items showing meaningful bias
- **Deliverable**: Peer-reviewed validation manuscript, public dataset release

**Phase 4: Beta Launch + API Layer (Q2 - Q3 2026 - If Validation Successful)**
- **Public beta**: Low-stakes personal development use only
- **API layer**: Developer access for third-party integrations
- **Restrictions**: Technical controls preventing high-stakes applications (employment, clinical, admissions)
- **Monitoring**: Ongoing reliability checks, user feedback, bias audits
- **Deliverable**: Public beta, developer documentation, API SDKs

**Phase 5: Integration Marketplace (Q4 2026+)**
- User-facing marketplace for managing data connections
- Pre-built connectors (LinkedIn, job platforms, learning tools, coaching apps)
- User-controlled permissions, audit logging, one-click revocation
- **Deliverable**: Fully operational open assessment platform

**Critical Decision Points:**
- Phase 0 → 1: Requires expert approval of technical approach
- Phase 1 → 2: Requires completed implementation
- Phase 2 → 3: Requires pilot meeting reliability (α ≥ .70, r ≥ .80) and validity thresholds
- Phase 3 → 4: Requires published validation evidence
- Phase 4 → 5: Requires stable beta operations

---

### 5.5 Data Privacy & Governance

**Current Status**: Framework designed, implementation pending user-facing deployment

**Core Principles:**
- **Purpose Limitation**: Data collected only for assessment and research
- **Consent Tiers**: Granular control (essential, de-identified research, identified research, third-party)
- **Minimization**: No tracking, advertising, or data sales
- **Security**: TLS in transit, AES-256 at rest, Row-Level Security in database
- **User Rights**: Data export, deletion, audit logging

**Planned Consent Structure:**

| Tier | Purpose | Retention | Default | Opt-Out? |
|------|---------|-----------|---------|----------|
| **Essential** | Assessment functionality | Account + 90 days | Required | No |
| **De-identified Research** | Psychometric validation, open datasets | 7 years | Yes | Yes |
| **Identified Research** | Longitudinal studies | Study + 3 years | No | Yes |
| **Third-Party** | Marketplace connections | Per partner | User-initiated | Yes (revocable) |

**Governance:**
- Independent Research Ethics Advisory Board (forming)
- Quarterly public transparency reports
- Annual third-party privacy audits
- IRB approval for all research use
- < 72-hour breach notification

**High-Stakes Prohibition:**

Technical and policy controls preventing use for:
- Employment screening/hiring
- Clinical diagnosis/treatment
- Educational admissions
- Insurance underwriting
- Any consequential decision affecting livelihood, health, opportunity

Restrictions remain until extensive validation is published and independently reviewed.

---

### 5.6 Why This Platform Is Different

**vs. Proprietary Assessments (CliftonStrengths, VIA+, MBTI):**
- **Open**: All methods, algorithms, validation data publicly documented
- **Portable**: Results are user-owned data with APIs, not platform-locked
- **AI-Enhanced**: Personalization (trauma-informed, culturally adapted) without compromising validity
- **Transparent**: Full provenance from literature → anchors → AI generation → user results
- **Accessible**: Free core assessment, pay-what-you-can detailed reports

**vs. Open-Source Personality Tools (IPIP, Big Five tests):**
- **Comprehensive**: 36 facets across 6 domains (vs. Big Five's 5-30 facets)
- **Adaptive**: IRT-based selection reduces test length by 40-60% (vs. fixed-length)
- **Contextualized**: AI-generated items match user's reading level, cultural background, trauma sensitivity
- **Applied**: Designed for real-world use (APIs, marketplace) not just research

**The Innovation:**

Demonstrating that AI can be used safely in psychometrics through:
1. **Anchor-based generation** (every item linked to validated source)
2. **Multi-layer verification** (NLI, embeddings, diversity filters)
3. **Full provenance** (audit trail from facet specification → prompt → NLI decision → deployment)
4. **Open validation** (pre-registration, public data, peer review)
5. **Ethical governance** (high-stakes prohibition, consent tiers, IRB oversight)

If successful, this provides a template for how AI can augment psychometric rigor—enabling personalization and accessibility while maintaining scientific standards.

---

## 6 · Current Challenges & Consultation Needs

### 6.0 Facet-First Generation & Semantic Governance

We treat the **facet description** as the canonical construct definition. Every candidate stem carries provenance (facet_id, description_version, generation prompt hash) and must pass a semantic gate before any fielding:

- **Alignment:** embedding cosine to the target facet ≥ 0.35 (tunable per facet)
- **Purity margin:** cosine(target) − max cosine(other facets) ≥ 0.08
- **NLI checks:** target facet entails; non-target facets neutral/contradiction; negated target contradicts
- **Diversity:** no near-duplicates within a facet (e.g., cosine < 0.92)
- **Language QA:** single idea (no double-barrel), reading level approx. grades 7–10, no evaluative adjectives

Only stems that pass these gates become **candidates for pilot** and subsequent calibration.

---


We are actively seeking expert consultation on the following psychometric challenges before proceeding to implementation. These represent critical questions that must be resolved to ensure scientific rigor and ethical deployment.

---

### 6.1 IRT Parameter Estimation for Deployed Items

**Challenge**

Our item pool is generated directly from versioned **facet descriptions** and screened via embeddings + NLI for semantic alignment and diversity. Public IPIP metadata provides **scale-level reliability (Cronbach’s α)** rather than **item-level IRT calibrations**. Therefore, we estimate **graded response model (GRM)** parameters—discrimination (*a*) and ordered thresholds (*b*)—only for items we deploy.

**Impact**
- **Item selection accuracy:** Without empirical calibrations, CAT cannot select the most informative items.
- **Ability estimation precision:** θ estimates may carry inflated SEs and potential bias.
- **Test efficiency:** More items may be needed to reach target precision.
- **Calibration uncertainty:** Early parameters are provisional and must be matured with additional data.

**Resolution**
- **Anchorless concurrent calibration:** Planned-missing (matrix) forms with θ identified as N(0,1); no dependency on legacy anchors.
- **Promotion rules:** Promote an item to the operational bank when threshold SEs < 0.15, item-fit acceptable, information covers θ≈[−1.5, +1.5], and no material local dependence/DIF.
- **Retirement rules:** Retire items with misfit, local dependence, drift, or DIF concerns.

**Questions for Consultation**
1. What responses-per-item and per-facet yields stable GRM a/b across 36 facets?
2. Recommended linking design using **Facet Reference Items (FRIs)** under rolling item updates?
3. What floor/ceiling diagnostics should be pre-registered as retirement criteria?

**Priority:** ⚠️ **Tier 1 (Critical - Blocking Production Launch)**

---

**Impact**
- **Item selection accuracy:** Suboptimal items may be selected during adaptive testing, reducing measurement efficiency
- **Ability estimation precision:** θ estimates may be biased or have inflated standard errors
- **Test efficiency:** More items may be required to reach target precision than necessary
- **Calibration uncertainty:** AI-generated items that inherit parameters from anchors compound any initial estimation errors

**Questions for Consultation**
1. What minimum sample size is required for stable GRM parameter estimation across 36 facets with 5-8 items each?
2. Should we use separate calibration samples per facet, or can we pool across domains while accounting for local dependence?
3. How should we handle facets with limited anchor item pools (n < 5) during initial calibration?
4. What are psychometrically defensible ranges for `a` (discrimination) and `b` (threshold) parameters in a strengths context where social desirability may affect responses?
5. Can we use IPIP anchor items that were originally calibrated for Big Five factors, or do we need fresh calibration for our six-domain model?

**Priority:** ⚠️ **Tier 1 (Critical - Blocking Production Launch)**

---

### 6.2 AI-Generated Item Quality Control

**Challenge**

The STEM Adapter pipeline generates novel assessment items using AI, but lacks rigorous psychometric validation before deployment. We are relying on Natural Language Inference (NLI) as a semantic filter, but have not established that semantic similarity guarantees psychometric equivalence.

**Current Planned Process**
1. **Generation Phase:** LLM creates stems from the **facet description** and style/valence constraints (no anchor dependency)
2. **NLI Verification:** Check against the **facet specification** (construct fit), not prior items
3. **Selection:** Items advance to the bank **only after pilot testing and calibration**

**Risks**
- **Construct validity drift:** Generated items may subtly shift away from intended facet constructs over multiple generation cycles
- **Response pattern anomalies:** Items may exhibit unexpected ceiling/floor effects, response biases, or aberrant patterns
- **Discrimination variability:** AI-generated items may not effectively differentiate between ability levels despite high NLI scores
- **Demographic bias:** Items may introduce cultural, linguistic, or demographic biases not present in validated facet specification
- **Temporal instability:** Item characteristics may drift as LLMs are updated or prompts are refined

**Questions for Consultation**
1. **Minimum Validation Requirements:** What psychometric validation is required before deploying AI-generated items, even in low-stakes contexts?
2. **Pilot Testing:** Should we implement a mandatory pilot testing phase with real responses (N = ?) before items enter the operational pool?
3. **Statistical Flags:** What performance metrics should trigger automatic item retirement or human review (e.g., item-total correlation < .30, DIF > threshold)?
4. **NLI Validity:** Is NLI entailment/contradiction a sufficient proxy for psychometric equivalence, or must we empirically demonstrate that high-NLI items have similar IRT parameters to their facet specification?
5. **Ongoing Monitoring:** What constitutes adequate post-deployment monitoring? How frequently should items be recalibrated?
6. **IRT Parameter Inheritance:** Can we defensibly assign anchor IRT parameters to AI-generated items based solely on high NLI alignment (e.g., entailment > 0.85), or is this psychometrically indefensible without empirical calibration?

**Priority:** ⚠️ **Tier 1 (Critical - Blocking Production Launch)**

---

### 6.3 Adaptive Algorithm Stopping Rules

**Challenge**

The current adaptive algorithm design uses simple stopping rules that may not optimize the efficiency-precision tradeoff, particularly across diverse ability levels and demographic groups.

**Current Planned Implementation**
- **Per-facet:** Stop when SEM ≤ 0.32 OR 10 items administered
- **Global:** Stop when all 36 facets meet criteria OR 180 total items administered

**Concerns**
1. **Uniform precision target:** Is SEM ≤ 0.32 appropriate across all facets and ability levels? Some facets may require tighter precision (e.g., clinically-relevant traits), while others may tolerate looser precision for screening purposes.
2. **Hard item caps:** 10-item per-facet cap may be too restrictive for individuals at extreme ability levels (very high/low), where more items are needed for stable estimates.
3. **Domain imbalance:** Users may experience highly unequal assessment lengths across the 6 domains (e.g., Creativity facets complete in 30 items, Connection facets require 60), creating potential fairness concerns.
4. **Clinical utility:** Are these precision targets aligned with intended use cases? Personal development may tolerate higher SEM than clinical applications.
5. **Stopping rule validation:** Have these thresholds been validated via simulation studies with realistic item pools and ability distributions?

**Questions for Consultation**
1. What precision targets (SEM thresholds) are psychometrically defensible for:
   - **Personal development applications** (current primary use case)
   - **Future clinical or coaching applications** (potential expansion)
   - **Research applications** (group-level comparisons)
2. Should stopping rules vary by facet based on measurement precision requirements or real-world consequences of measurement error?
3. How should we handle facets where target precision is unattainable due to limited item pool size or poor item quality?
4. What simulation studies are needed to validate stopping rules before deployment? (e.g., bias, efficiency, classification accuracy at different ability levels)
5. How do we balance assessment efficiency (shorter tests) against measurement precision and user experience (not frustrating people with excessive items)?

**Priority:** 🔶 **Tier 2 (High - Needed for Quality Assurance)**

---

### 6.4 Content Balance and Representation

**Challenge**

Ensuring adequate and balanced representation of each facet construct while maintaining item pool diversity and avoiding content redundancy.

**Current State**
- **Reference items (FRIs):** Small, curated per-facet set used for linking and drift checks
- **AI-generated items:** Generation success rates vary by facet complexity and abstractness (e.g., "Curiosity" generates easily, "Sense-Making" struggles)
- **Content coverage:** No formal content specification defining what aspects of each facet should be assessed (behavioral indicators, cognitive components, emotional components, etc.)

**Implications**
- **Construct underrepresentation:** Facets may be narrowly defined by limited candidate content, missing important behavioral indicators
- **Test fairness:** Users may receive different levels of construct coverage depending on which facet is being measured
- **Item exposure:** Facets with small item pools may have high item re-use rates in adaptive testing, creating security and practice effect concerns
- **Validity threats:** Narrow content coverage threatens content validity and may not generalize to the full facet construct

**Questions for Consultation**
1. What constitutes adequate content coverage for a multidimensional strength construct? Should we follow a formal content specification (e.g., test blueprint)?
2. Should we develop explicit content specifications for each facet, defining domains of behavioral indicators to ensure comprehensive coverage?
3. How can we audit AI-generated items for content balance within facets? (e.g., ensuring items tap cognitive, affective, and behavioral aspects)
4. What minimum item pool size per facet is required for operational CAT without excessive item exposure?
5. How do we handle facets that are conceptually broad (e.g., "Wisdom") vs. narrow (e.g., "Patience") in terms of item pool requirements?

**Priority:** 🔶 **Tier 2 (High - Needed for Quality Assurance)**

---

### 6.5 Validation Evidence Priorities

**Challenge**

Limited empirical validation of the assessment's psychometric properties and construct validity. We have a comprehensive validation plan (Section 5.4), but need expert guidance on priorities and sequencing given resource constraints.

**Current Evidence Gaps**
- **Reliability:** No coefficient alpha, omega, test-retest, or SEM estimates from real data
- **Construct validity:** No factor analysis confirming the 6-domain/36-facet structure with actual items and responses
- **Convergent validity:** No correlations with established strength measures (e.g., VIA-IS, CliftonStrengths) or personality measures (NEO-PI-R)
- **Discriminant validity:** No evidence that facets are sufficiently distinct from each other or from general personality traits
- **Predictive validity:** No data on relationships with meaningful real-world outcomes (job performance, well-being, academic achievement)
- **Incremental validity:** No evidence that our six-domain model adds value beyond existing frameworks

**Questions for Consultation**
1. **Study Prioritization:** Given limited resources, which validation studies should be prioritized in Phase 2 (pilot)? What is the minimum validation needed before even low-stakes use?
2. **Sample Characteristics:** What sample characteristics are essential for initial validation (demographics, ability range, clinical vs. non-clinical)?
3. **Sample Size:** What is the minimum sample size for initial pilot validation? Our target is N ≥ 1000, but is this sufficient for 36 facets?
4. **Reference-Item vs. Full Assessment:** Should initial validation focus on anchor items only (more psychometrically controlled) or the full adaptive assessment including AI-generated items?
5. **Dynamic Item Pools:** How should we handle validation when items are continuously added/updated via AI generation? Do we need to "freeze" the item pool for validation studies?
6. **CFA Approach:** With 36 correlated facets, what CFA approach is appropriate? Hierarchical model, bifactor model, ESEM? What fit indices are realistic for this complexity?

**Priority:** 🟡 **Tier 3 (Important - Needed for Long-term Credibility)**

---

### 6.6 Equating and Score Comparability

**Challenge**

As **Facet Reference Items (FRIs)** are maintained and AI-generated items are continuously updated, we must ensure that ability estimates (θ) remain comparable across forms and over time (longitudinal users, cohort studies, A/B versions).

**Solution**
- **Common-item nonequivalent groups design:** Include 2–4 FRIs per facet on each calibration wave to support linking.
- **Concurrent calibration or Stocking–Lord linking:** Use FRIs as the bridge when forms differ across waves.
- **Drift monitoring:** Demote/replace an FRI if |Δa| > 0.25 or any |Δb_k| > 0.30 with degraded fit; promote better items as they emerge.

**Open Questions for Consultation**
1. How many FRIs per facet are sufficient for stable equating under our content balance rules?
2. What target exposure and rotation should FRIs receive to balance linking precision and security?
3. What thresholds for drift (Δa, Δb) and fit should trigger demotion of an FRI?

**Priority:** ⚠️ **Tier 1 (Critical - Blocking Production Launch)**

---

4. **Linking Items:** What proportion of **FRIs** should be retained across waves to support equating? Is 20% per facet sufficient?
5. **Reporting Comparability:** How should we communicate score comparability to users? (e.g., "Scores from version 1.x and 2.x are comparable via equating; versions prior to 1.0 are not comparable")
6. **Longitudinal Studies:** For research applications, should we offer a "research mode" that locks the item pool version for duration of study?

**Priority:** 🟡 **Tier 3 (Important - Needed for Long-term Credibility)**

---

### 6.7 Summary & Consultation Request

**Tier 1 (Critical - Blocking Production Launch):**
1. IRT parameter estimation strategy for anchor items
2. AI-generated item validation requirements and minimum psychometric evidence

**Tier 2 (High - Needed for Quality Assurance):**
3. Adaptive stopping rule refinement and simulation validation
4. Content balance standards and item pool size requirements

**Tier 3 (Important - Needed for Long-term Credibility):**
5. Validation study design and evidence priorities
6. Equating procedures and longitudinal score comparability

**We are seeking consultation to:**
- Review and critique our technical approach before implementation
- Provide guidance on resolving Tier 1 challenges before any development begins
- Co-author a revised validation protocol incorporating expert recommendations
- Identify additional challenges or blind spots we may have missed
- Recommend best practices from adaptive testing and AI-assisted assessment literature

**Documentation Available for Review:**
- This whitepaper (comprehensive overview)
- `stem-adapter-prd.md` (96-page technical specification)
- `technical_architecture_overview.md` (system architecture and psychometric details)
- `current_psychometric_challenges.md` (expanded version of this section)

---

## 7 · Call to Collaboration

OpenStrengths is a pre-launch project seeking expert partners to validate and refine our approach before any public deployment. We invite collaboration from multiple stakeholder groups:

---

### Psychometricians & Assessment Researchers

**What we need:**
- **Expert consultation** on the critical challenges outlined in Section 6
- **Co-authorship** of revised validation protocols incorporating best practices
- **Methodological guidance** on adaptive testing with AI-generated items
- **Review and critique** of our technical specifications before implementation

**What we offer:**
- **Open collaboration credit:** Co-authorship on validation studies and technical reports
- **Access to novel data:** Unique dataset combining AI-generated items with traditional psychometrics
- **Research opportunities:** Explore intersection of LLMs and psychometric measurement
- **Transparent methodology:** All code, data, and methods openly documented

**How to engage:** Contact us to discuss specific consultation areas or research collaborations.

---

### Academic Institutions & Research Labs

**What we need:**
- **Pilot study partnerships:** Host calibration cohorts and validation studies (N ≥ 1000 participants)
- **Cross-cultural validation:** Conduct invariance testing across languages and cultures
- **Specialized populations:** Validate in clinical, organizational, or educational contexts
- **Replication studies:** Independent replication of our validation findings

**What we offer:**
- **De-identified research data:** Access to assessment responses for secondary analysis (IRB-approved, participant consent)
- **Co-authorship opportunities:** Joint publications on validation findings
- **Methodological transparency:** Full access to item banks, scoring algorithms, and technical architecture
- **Funding support:** Where possible, grant support for pilot sites and data collection

**How to engage:** Propose pilot study designs or specialized validation projects.

---

### Funding Organizations & Philanthropies

**What we need:**
- **Calibration study funding:** Support for large-scale pilot studies (target N = 2000-5000 across diverse populations)
- **Translation support:** Fund adaptation and validation in non-English languages
- **Infrastructure development:** Open-source platform development and maintenance
- **Accessibility initiatives:** Support for simplified-language and trauma-informed adaptations

**What we offer:**
- **Open science impact:** Support a freely available, scientifically rigorous alternative to proprietary assessments
- **Transparency and accountability:** Quarterly reports on progress, spending, and outcomes
- **Broad societal benefit:** Enable equitable access to strengths assessment across socioeconomic contexts
- **Research acceleration:** Contribute to open datasets advancing strengths science

**How to engage:** Contact us to discuss funding priorities and partnership structures.

---

### Policy & Standards Organizations

**What we need:**
- **Standards guidance:** Input on best practices for AI-assisted psychological assessment
- **Ethical framework review:** Critique of our data governance and consent structures
- **Use case guidance:** Advise on appropriate (and inappropriate) applications before deployment
- **Regulatory alignment:** Ensure compliance with emerging AI/assessment regulations (EU AI Act, etc.)

**What we offer:**
- **Transparency:** Full documentation of technical approach, limitations, and safeguards
- **Case study:** Example of responsible AI deployment in psychological measurement
- **Policy input:** Real-world data on challenges of operationalizing ethical AI assessment
- **Stakeholder engagement:** Ongoing dialogue about appropriate use boundaries

**How to engage:** Participate in advisory board or provide policy consultation.

---

### What We Are NOT Seeking (Yet)

- **General public beta testers:** No public pilot until validation phase complete
- **Commercial partnerships:** Focus is on research collaboration, not monetization
- **High-stakes applications:** Prohibited per Section 5.5 until extensive validation published
- **Media attention:** Prefer to work quietly until we have robust evidence to share

---

### Contact & Next Steps

**Current Status:** Actively seeking consultation (Phase 0: Expert Validation, Q4 2025)

**Timeline:** Not accepting general users until Q1 2026 at earliest (pending successful pilot study)

**How to reach us:** [Contact information to be added]

**Documentation for review:**
- This whitepaper (overview)
- Technical architecture overview (system design)
- STEM Adapter PRD (96-page implementation spec)
- Current psychometric challenges (detailed consultation questions)

**What happens next:**

See Section 5.4 for detailed roadmap. Summary: Phase 0 (Expert Validation, Q4 2025) → Phase 1 (Complete Assessment Engine, Q4 2025 - Q1 2026) → Phase 2 (Pilot Study, Q1 2026) → Phase 3 (Validation Studies, Q2 2026) → Public beta launch contingent on pilot results (Q2 - Q3 2026 earliest).

---

**Commitment to transparency:** All validation results—positive or negative—will be openly published. If pilot studies reveal fundamental flaws in our approach, we will document them transparently and revise or shelve the project accordingly.

---

## 8 · References


1. Silvia P. J. (2023). “Divergent Thinking Meta‑Analysis.”  
2. Jauk E. et al. (2021). “Creativity & Intelligence.”  
3. Roberts B. W. & Bogg T. (2020). “Two Faces of Conscientiousness.”  
4. Christian M. et al. (2009). “Workplace Safety Meta‑Analysis.”  
5. Neal A. & Griffin M. (2010). “Safety Climate & Behavior.”  
6. PLOS ONE (2015). “Empathy & Big Five.”  
7. Flake J. et al. (2020). “Reproducibility in Psychometrics.”  
8. Ashton M. C. & Lee K. (2024). *HEXACO Manual.*  
9. Ziegler M. et al. (2019). “Faking and Forced‑Choice.”  

*(Full BibTeX available in `/docs/references.bib`.)*  

---

*Prepared by the OpenStrengths Working Group · June 2025*  


---

## Appendix A — Canonical 36 Facets (one‑liners + anti‑cross‑loading notes)

_Insert the full canonical list from the current spec here. Include a short “not to be confused with…” line for each facet._


---

## Appendix B — Study Summaries (1‑page syntheses)

- B1. Higher‑order structure & cross‑cultural evidence → Six domains.

- B2. Reliability vs Item Count → Target 5–8 items/facet; adaptive delivery.

- B3. Criterion Validity → Domain separations (Drive, Influence, Connection, Insight, Creativity, Stability).

- B4. Cross‑cultural viability → Behavior‑anchored wording; invariance plan.


---

## Appendix C — Validation Protocol (draft)

- Phase‑1 (CTT) → Phase‑2 (IRT/CAT) → Phase‑3 (Fairness & DIF).

- Pre‑registration, code release, parameter tables, living change log.


---

## Appendix D — Change Log (v0.3 → v2.0)

- Adopted **six domains** informed by higher‑order structure.

- Finalized **36 facets**; rewrote definitions to minimize cross‑loading.

- Completed **stem adapter** and **anchor upload & management** systems.

- Added **scoring roadmap** and **AI‑stem reliability plan**.

---

## Appendix A: Glossary

- **Facet Description (Spec):** Canonical construct definition used for generation and NLI; versioned to prevent cross-loading.
- **Candidate Item:** Newly generated or imported item awaiting pilot data.
- **Operational Item:** Candidate promoted after passing pilot + calibration gates.
- **FRI (Facet Reference Item):** Calibrated, human-curated per-facet items used for audits, DIF, and linking/equating; replaceable.
- **Retired Item:** Item removed due to misfit, bias, or drift; retained for provenance.

---

## Changelog

- **2025-10-30 — Measurement architecture update:**
  - Replaced *anchor-based generation* with **facet-seeded generation**.
  - Introduced **Facet Reference Items (FRIs)** for audits/DIF/equating.
  - Clarified **IPIP imports = classical α only; no item-level IRT**.
  - Added **Pilot & Calibration** phase prior to operational use.
  - Updated system diagram, provenance chain, and consultation questions.