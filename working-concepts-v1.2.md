# OpenStrengths Technical Notes (Working Concepts) v1.1

_Last updated: December 12, 2025_

This document is a **technical companion to the OpenStrengths white paper (v3.1)**. It captures working concepts for the AI and measurement architecture that are **not yet finalized** and that we intend to refine with subject-matter experts (SMEs) before promoting them into the white paper. By keeping these concepts in a separate working document, we can iterate with SMEs on technical design questions—item generation, calibration, adaptive delivery, fairness monitoring—without revising the white paper with every change. Once a concept is validated and stable, it may be promoted into the white paper as canonical guidance.

Concepts here:

- May be partially implemented in code, partially on paper.
- Require **review and validation from psychometric / measurement experts**.
- Can be promoted into the white paper once validated, or revised / deprecated.

---

## Status Legend

Each concept has a `Status` field using one of:

- `Draft` – internal idea, not yet shown to any SME.
- `In SME Review` – has been shared with at least one SME, and feedback is being incorporated.
- `SME Feedback Incorporated` – updated based on SME comments; next changes will depend on data or future phases.

Status values are expected to change over time as experts review these concepts and as pilots / studies are conducted.

---

## Concept Index

| ID   | Area                        | Concept                                                                 | Status            | Related White Paper Sections                                      |
|------|-----------------------------|-------------------------------------------------------------------------|-------------------|--------------------------------------------------------------------|
| C-00 | Platform / Engine           | Domain-agnostic construct-seeded generation engine                      | In SME Review (Cho) | Part 2 – STEM Adapter (implicit), overall architecture             |
| C-01 | Item Generation             | Construct-seeded AI item-generation pipeline (reasoning chain)          | In SME Review (Cho) | Part 2 – STEM Adapter; "The Safety Question"                       |
| C-02 | Item Generation             | Anchors + embeddings as a semantic neighborhood for each construct      | In SME Review (Cho) | Part 2 – "Safety Question" (implied)                               |
| C-03 | Consistency & Linking       | Consistency framework (semantic → classical/IRT → operational) + FRIs   | In SME Review (Cho) | Part 2 – "Safety Question"; Roadmap – Phase 1 & 2                  |
| C-04 | Adaptation                  | Staged adaptive design (fixed → routed → CAT, with exposure control)    | In SME Review (Cho) | Part 2 – STEM Adapter (adaptive claims); Roadmap – Phase 1         |
| C-05 | Personalization vs Construct| Profile flags as eligibility / wording constraints (not part of trait)  | In SME Review (Cho) | Part 2 – User Classification System; STEM Adapter                  |
| C-06 | Validation Design           | Semantic-governance study (LLM gates vs SMEs on content validity)       | In SME Review (Cho) | Roadmap – Expert Validation; "The Safety Question"                 |
| C-07 | Validation Design           | Predictive & consequential validity strategy in means-tested populations| Draft             | Executive Summary; Roadmap – later phases                          |
| C-08 | Personalization, Fairness, Calibration | Item Models and Variants (Equivalence vs Distinct Items)     | Draft             | Part 2 – STEM Adapter; "The Safety Question"                       |
| C-09 | Validity Argument, Consequences, Design | Score Uses, Stakes, and Decision Structures                | Draft             | Executive Summary; Roadmap; "The Safety Question"                  |
| C-10 | Validity, Response Process, Accessibility | Response Process & Accessibility for Target Population      | Draft             | Executive Summary; Roadmap; User Classification System             |

---

## C-00 – Domain-Agnostic Construct-Seeded Generation Engine

**Area:** Platform / Engine Architecture
**Status:** In SME Review (Cho)
**Origin:**
- Internal concept drafted prior to external SME review; documents existing TypeScript architecture and intended design.
- Under review with Jake Cho as part of broader discussion of how AI-generated items align with construct specifications.

**Related White Paper Sections:** Part 2 – STEM Adapter (implicitly as "STEM-like engine"), overall architecture

### 1. Concept Summary

The generation engine is intended to be **construct-seeded and domain-agnostic**.

Instead of being hard-coded only to our six domains and 36 canonical strengths facets, the engine is built around a general pattern:

1. A user (or system) defines a **construct specification**, which includes:
   - A construct name (e.g., "Curiosity", "Job Engagement", or a custom facet).
   - A short, canonical description of what the construct _is_ (and, where relevant, what it is not).
   - Optional high-pole and low-pole descriptions.
   - Optional example anchor statements that sound like realistic behaviors.

2. The engine uses this specification to create a **semantic space** for the construct:
   - Embed the construct description (and anchors, if provided).
   - Treat these vectors as the core of a semantic neighborhood representing "on-construct" content.

3. The engine prompts an LLM to generate **candidate items** conditioned on the construct specification:
   - First-person, present-tense, self-referential.
   - Single clear idea (no double-barreled content).
   - At a specified reading level and tone.
   - Optionally targeting different endorsement intensities (more typical vs more extreme behaviors).

4. Each candidate item can be **screened against the semantic space** (see C-02 and C-03) before being considered for any pilot.

Our current strengths assessment (six domains, 36 facets) is one concrete ontology we are using to **validate and refine this engine**, but the architectural goal is broader:

> **If you can write a clear, bounded description of a construct, the engine should be able to:**
> - generate candidate items,
> - place them in a semantic space around that description,
> - and screen them for basic construct alignment,
> before empirical calibration.

### 2. Current Implementation Reality

- Implemented:
  - Data model and TypeScript implementation that store construct/facet specifications.
  - Generation of stems from facet descriptions via LLM calls.
  - Embedding of construct descriptions and anchors in Supabase.
- Not yet implemented:
  - Generic "bring your own ontology" API surface for external users.
  - A full semantic screening pipeline applied to every generated item (C-02 / C-03 describe the intended design).

### 3. Questions / Validation Needed from Experts

We want to focus expert input on questions that materially affect whether this engine is **safe and defensible to generalize** beyond our own ontology:

- Given our current construct-spec schema (name, canonical description, optional poles, optional anchors), are there **specific additional fields** you would consider essential for downstream validity (e.g., intended population, hypothesized correlates/opposites, exclusions)?
- In your view, what are the **boundary conditions** where a domain-agnostic, construct-seeded engine like this _should not_ be used without a bespoke, construct-by-construct build (e.g., certain clinical or high-stakes contexts)?

### 4. Promotion Criteria

- Demonstrated use of the engine on at least one ontology beyond the OpenStrengths model (e.g., a small external construct set).
- Expert agreement that a construct-seeded, domain-agnostic architecture is acceptable in principle, given appropriate safeguards and clearly defined boundary conditions.

### 5. Questions & Status

| ID | Question | Source | Status | Answer / Reference |
|----|----------|--------|--------|-------------------|
| C-00.Q1 | Can a construct-seeded, domain-agnostic engine generalize safely beyond the OpenStrengths ontology, or are there boundary conditions where it should not be used? | SME (Cho) + Internal | In progress | See §3: seeking expert input on boundary conditions for clinical or high-stakes contexts. |
| C-00.Q2 | What additional fields (beyond name, description, poles, anchors) would be essential in the construct specification schema for downstream validity? | SME (Cho) | Pending | To be discussed with SMEs; may include intended population, hypothesized correlates/opposites, explicit exclusions. |
| C-00.Q3 | Is the current TypeScript implementation of construct-seeded generation adequately flexible to support external ontologies? | Internal (Zach) | Answered | Yes, data model supports custom construct specs; "bring your own ontology" API not yet exposed but architecturally feasible. See §2. |

---

## C-01 – Construct-Seeded AI Item-Generation Pipeline (Reasoning Chain)

**Area:** Item Generation
**Status:** In SME Review (Cho)
**Origin:**
- Internal concept drafted prior to external SME review; documents reasoning chain for AI-assisted item generation.
- Under review with Jake Cho as part of validating the division of labor between human-defined constructs and AI-generated items.

**Related White Paper Sections:** Part 2 – STEM Adapter; "The Safety Question: How Do We Know AI Doesn't Change What's Being Measured?"

### 1. Concept Summary

This concept describes the **reasoning chain** we expect the item-generation pipeline to approximate.

At a high level:

1. **Constructs are defined through a human-led, research-based process.**
   - We surveyed major strengths and personality models and the underlying literature.
   - We used large language models as **research assistants** to:
     - surface and summarize studies,
     - help compare frameworks,
     - draft candidate formulations.
   - We then converged manually on a six-domain, 36-facet ontology and wrote canonical facet descriptions.
   - Regardless of tooling, these canonical definitions are treated as the human defined reference point for each construct, subject to revision as SME review and empirical validation come in.

2. **The AI is asked to work _within_ those definitions.**
   - The LLM receives the construct description and explicit rules (first-person, present tense, one behavior per item, etc.).
   - The LLM's job is to propose possible wordings of behaviors that exemplify the construct.
   - It does **not** decide which constructs exist or what they mean; it suggests phrasing for constructs that have already been defined.

3. **We screen what the AI proposes.**
   - **NLI validates construct fidelity:** positive stems should be entailed by the facet description; reverse stems should contradict it while staying on-construct.
   - Cross-construct checks: "Is it more aligned with some other construct instead?"
   - Embedding checks: item vectors should be near the construct's semantic neighborhood (see C-02).
   - Clear failure cases (off-topic, cross-loading, unclear wording) are rejected before pilot.

4. **Only then do we move to empirical evaluation.**
   - Items that pass semantic checks become **candidate items** in a pilot.
   - Classical and/or IRT analyses (see C-03) then determine which items are actually useful for measurement.

The core design principle:

> Ontologies and construct boundaries are set through human-led research and review.
> Within those boundaries, AI accelerates item authoring.
> Statistical evaluation then decides which AI-authored items are actually worth keeping.

### 2. Current Implementation Reality

- Implemented:
  - The six-domain, thirty-six-facet strengths model with canonical descriptions.
  - LLM generation of stems from facet descriptions in the TypeScript app.
  - NLI-based construct-fidelity checks at generation time (facet description as premise, stem as hypothesis).
  - Embeddings for facet descriptions and anchors stored in Supabase.
  - Extensive pipeline logging at the task level (CIDs, network calls, task state tracking).

- Not yet implemented:
  - Embedding-based screening and cross-construct checks as part of a standard semantic gate for every generated item.
  - A reviewer-facing audit view that surfaces the full semantic gate (NLI + embeddings) and associated logs for human inspection.

### 3. Questions / Validation Needed from Experts

We are specifically interested in whether this division of labor is **psychometrically acceptable**, not in broad philosophical questions:

- Given this "constructs first, AI second" arrangement, are there **specific failure modes** you would expect (e.g., narrow content coverage, subtle construct drift) that we should explicitly test for in early pilots?
- When we run our first pilot with AI-generated items, what **minimum checks** beyond standard content review would you strongly recommend (e.g., additional SME panels, structured cognitive interviews)?

### 4. Promotion Criteria

- At least one measurement expert confirms that this reasoning chain is acceptable as a basis for item authoring, given proper semantic and psychometric checks.
- We have run at least one small pilot showing that construct-seeded, AI-written items behave roughly as expected at the factor structure level.

### 5. Questions & Status

| ID | Question | Source | Status | Answer / Reference |
|----|----------|--------|--------|-------------------|
| C-01.Q1 | Is the "constructs first, AI second" division of labor psychometrically acceptable for a low-stakes strengths assessment? | SME (Cho) | In progress | See §1 and §3; awaiting SME confirmation that reasoning chain is sound. |
| C-01.Q2 | What specific failure modes (narrow coverage, construct drift) should be tested in early pilots? | SME (Cho) | Pending | To be discussed with SMEs before first pilot. |
| C-01.Q3 | What minimum checks beyond standard content review are needed when running first pilot with AI-generated items? | SME (Cho) | Pending | May include additional SME panels or structured cognitive interviews; see C-10. |
| C-01.Q4 | Is the current NLI-based construct-fidelity gate implemented and working? | Internal (Zach) | Answered | Yes, implemented in TypeScript app; checks entailment for positive stems and contradiction for reverse stems. See §2. |

---

## C-02 – Anchors + Embeddings as Semantic Neighborhoods

**Area:** Item Generation
**Status:** In SME Review (Cho)
**Origin:**
- Internal concept drafted prior to external SME review; extends semantic validation beyond NLI to include embedding-based neighborhood checks.
- Designed to support both content screening and anchor selection workflow.

**Related White Paper Sections:** Part 2 – "The Safety Question" (implicitly); future technical notes

### 1. Concept Summary

The long-term goal is to represent each construct as a **semantic neighborhood** defined by:

- The canonical construct description (and, where applicable, a low-pole description).
- A small, curated set of **anchor statements** that sound like realistic behaviors or self-descriptions at different intensities and that have passed semantic and, eventually, psychometric checks.

The intended use is:

1. Embed the construct description and anchors into a vector space.
2. Treat this cluster as the **"on-construct" region** for the facet.
3. Embed candidate items and check:
   - Are they sufficiently close (above a threshold) to the construct's neighborhood?
   - Are they not closer to some other construct's neighborhood?
4. Use those signals as part of a first-line **content plausibility screen**, not as a substitute for SME review or empirical validation.

Anchors here are semantic exemplars: sentences you would be comfortable handing to a new item writer as "this is what high (or low) on this facet looks like."

### 2. Current Implementation Reality

- Implemented today:
  - Data structures for anchors exist in the TypeScript app.
  - Facet (construct) descriptions are embedded and stored in Supabase.
  - Anchors can be embedded and their cosine similarity to the facet description can be computed.
- Not implemented / not in use yet:
  - A principled workflow for **selecting and promoting** anchors from the general item pool.
  - Routine use of anchors as part of an "on-construct neighborhood" check for every generated item.
  - Any psychometric criteria (for example, item–total correlations, factor loadings) for promoting anchors to dual roles as Facet Reference Items (see C-03).

Right now, anchors are defined and can be embedded, but they are not actively used in the item-generation or screening workflow.

### 2.1 Planned Anchor Workflow (Design Target)

We expect the anchor selection and promotion process to look roughly like this:

1. **Candidate pool**
   Start from items that have already passed:
   - NLI-based construct-fidelity checks against the facet description (positive stems entailed, reverse stems contradicted), and
   - Basic embedding screens against the facet description.

2. **AI-assisted proposal**
   Use NLI scores and embeddings to propose a small set of items per facet that are:
   - Highly aligned with the facet description.
   - Distant from other facets (low cross-loading risk).
   - Non-redundant with existing anchors.

3. **SME/content review**
   Human reviewers select a few of these as **semantic anchors** using a clear rubric, for example:
   - Would I use this sentence to explain the facet to a new item writer?
   - Is it clearly on-construct, behaviorally concrete, and not overly context-bound?
   - Does the set of anchors capture the main ways the facet shows up, rather than repeating one narrow behavior?

   Approved items are promoted to semantic anchors and marked as such in the system.

4. **Optional empirical promotion**
   After pilots, some semantic anchors may also be promoted to **Facet Reference Items (FRIs)** if they show desirable psychometric properties (for example, strong loadings on the intended facet, low loadings on others, stable behavior across subsamples, acceptable DIF).

One explicit research question for the project is how far we can safely push AI in steps (2) and (part of) (3): for example, by comparing AI-proposed anchors to SME-selected anchors and evaluating whether AI-based criteria can reliably approximate SME judgements over time.

### 3. Questions / Validation Needed from Experts

We assume embeddings and anchors are only **supporting tools** for content work, not replacements for SMEs. We want help scoping their responsible use:

- From your experience, where have semantic or embedding-based tools been useful in supporting content validity work, and where do you see clear red flags (for example, overfitting to surface wording, instability across models, false confidence)?
- If we adopt this anchor-based semantic neighborhood approach, what would you want to see in a **documentation or audit trail** (for example, how anchors were selected, thresholds used, SME review logs) to be comfortable that it is not substituting for expert judgement?
- How many anchors per facet, and what diversity of behaviors, would you consider "enough" to define a stable semantic neighborhood for screening AI-generated items?

### 4. Promotion Criteria

- Expert consensus that anchors and embeddings are acceptable as a first-line screening and support tool, with clear caveats and governance.
- A documented anchor-selection workflow (including SME review and, where relevant, empirical checks) tested on at least one pilot dataset.
- Evidence that the semantic neighborhood defined by description plus anchors correlates sensibly with SME assessments of on- vs off-construct items across multiple facets.

### 5. Questions & Status

| ID | Question | Source | Status | Answer / Reference |
|----|----------|--------|--------|-------------------|
| C-02.Q1 | Where have semantic/embedding tools been useful for content validity work, and where are there red flags? | SME (Cho) | Pending | Seeking expert input on appropriate use cases and limitations. |
| C-02.Q2 | What documentation/audit trail would SMEs want to see to ensure embeddings don't substitute for expert judgment? | SME (Cho) | Pending | May include anchor selection logs, thresholds used, SME review records. |
| C-02.Q3 | How many anchors per facet are "enough" to define a stable semantic neighborhood? | SME (Cho) + Internal | Pending | Need expert guidance on anchor set size and behavioral diversity. |
| C-02.Q4 | Is the anchor workflow (§2.1) currently implemented? | Internal (Zach) | Answered | Partially: data structures exist, embeddings stored in Supabase, but principled selection workflow not yet operational. See §2. |

---

## C-03 – Consistency Framework (Semantic → Classical/IRT → Operational) + FRIs

**Area:** Consistency & Linking
**Status:** In SME Review (Cho)
**Origin:**
- Created in response to Jake Cho's central question: "How will you know your AI-generated items behave consistently across forms, domains, and facets?"
- Synthesizes three-layer approach (semantic → statistical → operational) with hybrid calibrated-bank stance.

**Related White Paper Sections:** Part 2 – "The Safety Question"; Roadmap – Phase 1 & 2

### 1. Concept Summary

Our advisors posed a central question: **"How will you know your AI-generated items behave consistently across forms, domains, and facets?"**

Our working answer is a three-layer framework that connects:

1. **Semantic consistency (layer 1)** – Does the wording of each item actually reflect the intended construct?
2. **Statistical consistency (layer 2)** – Do items behave as expected in data, within and across facets?
3. **Operational consistency (layer 3)** – As the bank evolves, can scores still be compared over time and across forms?

#### 1.1 Semantic Consistency (Layer 1)

Goal: Ensure items are **on-construct** before they ever enter a pilot.

Planned components:

- **NLI-based construct fidelity (already implemented for generation)**
  - Premise: the canonical facet description (what people high on the facet look like).
  - Hypothesis: the generated stem.
  - For **positive stems**:
    - We keep items where entailment is substantially stronger than contradiction. The intent is that, if the facet description is true of a person, the positive stem should logically follow.
  - For **reverse stems**:
    - We keep items where the NLI model assigns substantially higher probability to contradiction than to entailment (for example, pCon > pEnt with additional minimum thresholds as we refine the gate). The intent is that reverse stems clearly oppose the facet description while staying on the same underlying construct.

- **Embedding-based checks (design target)**
  - Embed the facet description and (eventually) a curated set of semantic anchors (see C-02).
  - Embed candidate items.
  - Use cosine similarity to:
    - Check that items sit close to their own facet's semantic neighborhood.
    - Detect potential cross-loading items that are closer to some other facet.

Together, NLI and embeddings define a **semantic gate**: items that fail are not even allowed into the candidate pool for pilots.

#### 1.2 Statistical Consistency (Layer 2 – Classical First, IRT Later)

Goal: Ensure items that pass the semantic gate also behave sensibly in data.

Planned progression:

- **Step 1: Classical test theory (CTT) with fixed or planned-missing forms**
  - Item–total correlations within facet.
  - Internal consistency (e.g., omega/alpha) for each facet/domain.
  - Exploratory and confirmatory factor analyses to check:
    - Do items load primarily on their intended facet?
    - Do facets cluster into domains in a plausible way?

- **Step 2: Polytomous IRT models once data supports it**
  - Candidate models: graded response or partial credit, chosen with expert input.
  - Estimate item parameters and information curves.
  - Check:
    - Item fit and discrimination.
    - Stability across calibration waves and key subgroups.
    - Whether the added complexity is justified relative to CTT.

The idea is to move from **CTT → IRT** only when we have enough data and a clear reason, rather than starting in IRT by default.

#### 1.3 Operational Consistency (Layer 3 – FRIs and Linking)

Goal: Maintain comparability as items are added, retired, or assembled into different forms.

Key concept:

- **Facet Reference Items (FRIs)**
  - A small, stable subset of items per facet that:
    - Have strong semantic alignment (layer 1).
    - Show good psychometric performance (layer 2).
  - FRIs stay in use longer than typical items and serve as **anchors** for:
    - Linking new calibrations back to older ones.
    - Monitoring drift in the facet over time.

Operational rules (design target):

- Only items that pass layers 1 and 2 become eligible as FRIs.
- FRIs must be present in each new calibration wave or in carefully designed overlapping forms.
- The bank is monitored over time:
  - Track item and test information functions.
  - Watch for changes in FRI parameters that may signal drift in usage or population.

The overall idea:

> **Layer 1** (semantic) decides which items even enter the candidate pool.
> **Layer 2** (classical/IRT) decides which items are worth keeping as measurement tools.
> **Layer 3** (operational) decides which items keep the score scale stable as the bank and forms evolve.

In early phases, OpenStrengths will behave like a traditional calibrated bank: a finite set of reviewed items and item models will be field-tested and calibrated before use in operational scoring. On-the-fly personalized items and newly generated items will only be added to the operational bank after calibration and monitoring, not deployed as uncalibrated content. Over time, the generative pipeline is intended to maintain and extend this calibrated bank with reduced SME burden, rather than replace calibration entirely.

---

### 2. Current Implementation Reality

- **Semantic layer:**
  - Facet descriptions exist and are used as the premise in an NLI-based construct-fidelity gate for generated stems (positive and reverse). Items that fail this NLI gate do not enter the candidate pool.
  - Embeddings for facet descriptions and anchors are stored in Supabase.
  - Embedding-based cross-construct screening and anchor-based semantic neighborhoods (as described in C-02) are not yet implemented in the production pipeline.

- **Statistical layer:**
  - No large-scale pilot has been run yet.
  - No CTT or IRT analyses have been applied to real respondent data.
  - A measurement expert has already recommended starting with fixed or planned-missing forms and classical analyses before committing to a specific IRT model or full CAT, which this framework accommodates.

- **Operational layer:**
  - No FRIs have been selected.
  - No live operational item bank, linking design, or CAT engine is in production.

Right now, layer 1 is partially implemented (NLI construct-fidelity checks at generation time); layers 2 and 3 are design-level only.

---

### 3. Questions / Validation Needed from Experts

We want focused input on how to make this framework practical and "good enough" for our context (low-stakes strengths assessment with AI-generated items):

Our primary calibration sample will be adults in means-tested programs recruited via partner agencies (for example workforce development, housing supports). Generalizability to other populations or contexts will require explicit checks and, where necessary, separate calibration or DIF analyses.

1. **For the first AI-generated pilot:**
   - What minimal but sufficient set of analyses would you run to judge consistency across facets and forms?
   - Which CTT indices and factor models would you consider mandatory vs "nice to have"?

2. **Transition from CTT to IRT:**
   - In this setting, which polytomous models would you consider first, and why?
   - What criteria would you use to decide that IRT is adding enough value to justify the extra complexity (beyond what CTT and factor analysis give us)?

3. **Facet Reference Items (FRIs):**
   - How many FRIs per facet would you consider reasonable for linking and monitoring in a multi-facet strengths assessment like this?
   - What kind of ongoing monitoring of FRI behavior (e.g., fit, parameters, DIF) would you expect before you were comfortable saying "this scale is stable over time"?

---

### 4. Promotion Criteria

This concept would be considered ready to promote into the main white paper (and implemented in code and process) when:

- At least one measurement expert confirms that this layered framework is sound and realistic for our use case and volumes.
- We have run at least one pilot showing that:
  - The semantic gate (NLI and, eventually, embeddings/anchors) reduces obviously off-construct items.
  - Classical analyses and early IRT models produce coherent facet-level scales that align with the intended ontology.
  - A preliminary set of FRIs per facet can be identified and appears reasonably stable across subsamples or calibration waves.

At that point, this three-layer consistency framework would become part of the "canonical" description of how OpenStrengths maintains consistency across AI-generated items, forms, domains, and facets.

### 5. Questions & Status

| ID | Question | Source | Status | Answer / Reference |
|----|----------|--------|--------|-------------------|
| C-03.Q1 | What minimal but sufficient analyses should be run to judge consistency across facets and forms in the first AI-generated pilot? | SME (Cho) | In progress | See §3; seeking expert input on mandatory vs "nice to have" CTT indices and factor models. |
| C-03.Q2 | Which polytomous IRT models should be considered first, and what criteria justify moving from CTT to IRT? | SME (Cho) | Pending | Candidate models: graded response or partial credit; criteria to be defined with expert input. |
| C-03.Q3 | How many FRIs per facet are reasonable for linking and monitoring in a 36-facet assessment? | SME (Cho) | Pending | Need expert guidance on FRI set size and ongoing monitoring expectations. |
| C-03.Q4 | Is the NLI construct-fidelity gate currently operational? | Internal (Zach) | Answered | Yes, implemented for generation; items failing NLI gate do not enter candidate pool. See §2. |
| C-03.Q5 | Will the primary calibration sample be adults in means-tested programs? | Internal (Robert) | Answered | Yes, recruited via partner agencies; generalizability to other populations requires explicit checks. See §3. |

---

## C-04 – Staged Adaptive Design (Fixed → Routed → CAT, with Exposure Control)

**Area:** Adaptation
**Status:** In SME Review (Cho)
**Related White Paper Sections:** Part 2 – STEM Adapter (adaptive claims); Roadmap – Phase 1

### 1. Concept Summary

Our advisors posed a central question: **"How is the examinee profile used to drive adaptive item selection and test assembly (including exposure controls)?"**

Our working plan is **staged**, not "jump straight to full CAT":

1. **Stage 0 – Fixed or planned-missing forms (no adaptation)**
   - Early pilots use fixed or planned-missing designs:
     - Good for factor structure checks and initial item statistics.
     - Simpler logistically and analytically.
   - This stage produces:
     - Basic item statistics (CTT),
     - Evidence about whether facets/domains behave as expected,
     - Data for selecting FRIs.

2. **Stage 1 – Simple routing within facets**
   - Introduce a small set of "routing items" per facet that cover moderate endorsement levels.
   - Based on responses, assign the examinee to a coarse region (e.g., lower / mid / higher endorsement) for that facet.
   - Select follow-up items from the corresponding difficulty band, while:
     - Respecting content coverage for that facet,
     - Ensuring a minimum number of items per facet.

3. **Stage 2 – Full CAT (longer term)**
   - Once we have solid calibrations:
     - Estimate θ for each facet as responses are collected.
     - Select items that provide the most information at current θ, with:
       - Content balancing constraints,
       - Simple exposure control (e.g., maximum usage per item, randomization within small sets).
   - Define stopping rules such as:
     - Target precision for θ,
     - Maximum items per facet or per test.

Throughout all stages:

- **Profile flags** (see C-05) affect _which items are eligible_ and _how they're worded_ (role context, reading level, trauma-sensitive exclusions).
- The **latent trait model** itself (what the facet scores mean) does **not** incorporate these flags as predictors.

### 2. Current Implementation Reality

- A scoring engine that can track provisional θ and SEM exists in the codebase, but it is not yet used to drive information-based item selection or exposure control. All current flows are effectively fixed-form or scripted rather than truly adaptive.
- Profile flags and pre-assessment logic exist conceptually and in parts of the prototype.
- The staged design (0 → 1 → 2) is a working plan, influenced by expert advice to start with simpler designs and analyses before full CAT.

### 3. Questions / Validation Needed from Experts

- In a multi-facet strengths assessment like ours, is a staged path (fixed → routed → CAT) a sensible trajectory, or would you recommend a different progression?
- When we reach Stage 1, what would you consider a reasonable number of routing items and follow-up items per facet for early adaptive prototypes?
- For Stage 2, what level of exposure control (e.g., simple caps on item usage, randomization) is "good enough" for low-stakes, personal-development use cases?

### 4. Promotion Criteria

- Expert agreement on the staged approach and on the key design parameters for each stage.
- Pilot or simulation results showing that routing and CAT do not materially distort the facet score distributions compared to fixed-form baselines.

---

## C-05 – Profile Flags as Eligibility / Wording Constraints (Not Part of Trait)

**Area:** Personalization vs Construct
**Status:** In SME Review (Cho)
**Related White Paper Sections:** Part 2 – User Classification System; Pre-assessment; STEM Adapter

### 1. Concept Summary

We use a pre-assessment to derive **profile flags**, for example:

- `role_context` (e.g., student, employee, caregiver),
- `language_simplification` or reading level,
- `trauma_sensitive` (yes/no),
- other accessibility- or context-related indicators.

Design rule:

> Profile flags affect **which items are eligible** and **how they are worded**,
> but they do **not** change what the construct means or how scores are interpreted.

Profile-based personalization (for example role context, reading level, trauma-sensitive wording) is implemented as part of the item-generation context, not as part of the latent trait model. The current design treats these as intended incidental features: they should change how items are framed for the test taker without changing what is being measured.

By default, variants generated from the same item model using different incidental feature values are intended to function as equivalent items for scoring and calibration. This is an empirical claim. We plan to test it using DIF and invariance analyses and by comparing item parameters across variants. Where equivalence does not hold, we will either treat those variants as distinct items (separate calibrations) or constrain their use.

### 2. Current Implementation Reality

- The pre-assessment concept and flag types are defined and partially implemented in the prototype.
- Flags are used in generation prompts to personalize wording in a STEM-like flow.
- Flags are **not yet** integrated into a live adaptive engine (no engine exists yet).
- No formal DIF / invariance analyses have yet been run to quantify the effect of personalization on item functioning.

### 3. Questions / Validation Needed from Experts

- Is this separation — using flags only as eligibility/wording constraints and not as predictors in the trait model — sufficient to preserve construct interpretability?
- Are there specific risks (e.g., differential item functioning by role context) we should expect when changing wording and examples, and how would you recommend we test for them?
- Under what conditions would you insist that certain constructs **not** be personalized in this way?

### 4. Promotion Criteria

- Expert agreement that this stance is acceptable given proper monitoring.
- At least one DIF / invariance analysis showing that personalization decisions do not introduce unacceptable bias across key groups.

---

## C-06 – Semantic-Governance Study (LLM Gates vs SMEs)

**Area:** Validation Design
**Status:** In SME Review (Cho)
**Related White Paper Sections:** Roadmap – Expert Validation; "The Safety Question"

### 1. Concept Summary

We want to evaluate the **semantic gates** (NLI + embedding thresholds) against human expert judgements.

High-level study design:

- Select a sample of AI-generated items across multiple constructs.
- Have SMEs rate each item on:
  - Construct relevance,
  - Cross-loading risk,
  - Clarity / readability.
- Compare:
  - SME ratings to the decisions from NLI + embedding gates,
  - Cases of agreement and disagreement.
- Use findings to:
  - Adjust thresholds and rules,
  - Decide where SME review is mandatory vs where AI gates are sufficient for first-line screening.

### 2. Current Implementation Reality

- No formal study has been designed or run yet.
- Only informal experiments exist (spot-checking AI gates vs human intuition).

### 3. Questions / Validation Needed from Experts

- What would you consider a meaningful level of agreement between SME ratings and AI-based semantic gates for this to be trustworthy?
- How many items, constructs, and SMEs are needed before such a study provides actionable evidence?
- Which governance decisions (e.g., thresholds, mandatory SME review for certain constructs) could reasonably be based on such a study?

### 4. Promotion Criteria

- Study design reviewed and endorsed by at least one measurement expert.
- At least one completed study with interpretable results guiding our semantic-gating policy.

---

## C-07 – Predictive & Consequential Validity Strategy in Means-Tested Populations

**Area:** Validation Design
**Status:** Draft
**Related White Paper Sections:** Executive Summary; Roadmap – later phases

### 1. Concept Summary

Plan a series of **predictive and consequential validity studies**, focusing especially on:

- People with lower economic mobility,
- Participants in government or means-tested programs,
- Educational and workforce development contexts.

Examples:

- Do certain strengths profiles predict:
  - Engagement with programs,
  - Persistence/completion,
  - Employment or earnings changes,
  - Self-reported wellbeing or reduced stress?
- Are there unintended negative consequences:
  - Misinterpretation of scores by agencies,
  - Labeling or exclusion based on strengths profiles?

### 2. Current Implementation Reality

- No predictive or consequential validity studies are underway.
- Some candidate field partners and contexts have been identified conceptually.

### 3. Questions / Validation Needed from Experts

- Which outcome domains (e.g., engagement, completion, employment, wellbeing) should we prioritize given our mission and populations of interest?
- What kinds of designs (longitudinal, quasi-experimental, natural experiments) are realistic yet still informative in real-world, resource-constrained settings?
- How should we predefine what counts as "acceptable" vs "unacceptable" consequences of using strengths profiles in these contexts?

### 4. Promotion Criteria

- A prioritized list of predictive / consequential validity studies agreed upon with advisors and implementation partners.
- At least one study designed and, where applicable, IRB/ethics-reviewed.

---

## C-08 – Item Models and Variants (Equivalence vs Distinct Items)

**Area:** Personalization, Fairness, Calibration
**Status:** Draft

### 1. Concept Summary

To support profile-based personalization and AI-assisted item generation, we introduce an explicit item model layer.

An **item model** is a structured template tied to a single facet. It defines:

- A core behavior or self-description the item is intended to capture.
- A bounded feature space for allowed variation (for example role context, reading level, pronoun set).
- Rules about which features can change and which must remain fixed.

From each item model, the system can generate **variants** by instantiating allowed feature values. For example, a gratitude item model might yield work-focused, school-focused, or community-focused variants at different reading levels.

Within each item model we distinguish:

- **Radical features** – changes that alter the underlying behavior, interpretation, or difficulty. These create a new item or a new item model.
- **Incidental features** – surface-level variations that are not intended to change the construct being measured (for example swapping "coworkers" for "classmates" when the facet is explicitly context-invariant, or simplifying syntax for reading level).

This distinction is both conceptual and operational. It is recorded in the model specification and reviewed by SMEs.

### 2. Intended Data and System Design

In the system we expect to represent item models explicitly, for example:

- `item_models`
  - `id`
  - `facet_id`
  - `model_code`
  - `core_behavior_description` (the invariant radical part)
  - `incidental_features` (JSON: which dimensions can vary and allowed values)
  - `pooling_policy` (for example `pool_all`, `pool_by_feature`, `separate`)
  - SME approval metadata

Generated stems are linked back to an item model and tagged with the incidental feature values that were applied at generation time, for example:

```json
{
  "incidental_features_applied": {
    "role_context": "employee",
    "reading_level": "B2",
    "trauma_sensitive": false
  }
}
```

This provides the data needed to study how variants behave across profiles and populations.

### 3. Equivalence and Pooling Policy

Our working assumption is:

Variants generated from the same item model, differing only on features declared incidental, are intended to be equivalent for scoring and calibration.

We treat that as a testable hypothesis, not a given. The validation plan includes:

- DIF and invariance analyses by profile flags and key demographic groups.
- Comparisons of estimated item parameters (difficulty, discrimination) across variants.

If variants from a given model show meaningful differences in parameters or fairness, we will:

- Either treat them as distinct items with separate calibrations, or
- Constrain or retire problematic variants or feature combinations.

This makes the pooling or splitting of variants an explicit decision backed by data.

### 4. SME Role

SMEs will review and approve item models rather than only individual items. For each model they will:

- Confirm that the facet definition and core behavior description are aligned.
- Classify proposed features as radical or incidental and review the allowed feature values.
- Review a sample of generated variants to check that the intended construct, difficulty band, and reading level are preserved.

Item-model documentation will record these decisions so they can be revisited as empirical data accumulates.

### 5. Questions for Advisors

- From a measurement perspective, what minimum set of analyses would you consider sufficient to justify pooling variants from a single item model for scoring and calibration?
- Are there feature types (for example context, timeframe, valence) that you would rarely or never treat as incidental in a strengths inventory like this?

---

## C-09 – Score Uses, Stakes, and Decision Structures

**Area:** Validity Argument, Consequences, Design
**Status:** Draft

### 1. Concept Summary

This concept clarifies how OpenStrengths scores are intended to be used in practice, and at what stakes, so that the psychometric design and validation work can be aligned with those uses.

At a high level, we distinguish between:

- **Descriptive profile uses** – ranking or grouping facets to support self-understanding, coaching, and guidance, without direct pass/fail decisions.
- **Decision and triage uses** – using scores (or bands of scores) to trigger program actions, recommendations, or eligibility pathways.
- **High-stakes classifications** – formal cut-score decisions with significant consequences (for example licensure, employment, exclusion).

OpenStrengths is initially positioned in the **descriptive profile + low- to moderate-stakes triage** space, not as a high-stakes licensing or accountability instrument.

### 2. Intended Uses of Scores: Risk-Adjusted Framework

We organize score uses into two allowed tiers plus a set of explicitly prohibited uses. Each allowed tier has a corresponding **evidence bar** and **governance commitment**.

#### Tier 1 – Self-Insight / Coaching (Allowed)

**Use Cases:**
- Each person receives a **36-facet profile**, typically presented as a rank-ordered list (highest to lowest) within six domains.
- Reporting is **strengths-oriented**: lower scores indicate relatively less expressed strengths, not "deficits" or diagnostic labels.
- Primary contexts:
  - Self-reflection and personal development.
  - Conversations with coaches, mentors, or case managers.
  - Linking to qualitative narratives and skill-building suggestions (for example "how this facet might show up at work").

**Stakes:**
- Scores inform individuals and helpers.
- No direct decision consequences: scores do not, by themselves, deny services, impose sanctions, or trigger formal interventions.

**Evidence Bar (Tier 1):**
- Reliability at the facet level sufficient to support stable rank ordering and banding.
- Internal structure (EFA/CFA) showing that facets and domains behave as intended.
- Basic convergent and discriminant validity (for example correlations with related constructs).
- Initial fairness checks (group-level DIF and invariance analyses by key demographics).
- Qualitative feedback from test takers and practitioners on interpretability and respect.

**Current Status:**
- **This is where OpenStrengths v1 is confined at launch.**
- All current and near-term deployments operate exclusively in Tier 1.

---

#### Tier 2 – Support Targeting / Extra Help (Conditionally Allowed)

**Use Cases:**
- In contexts like housing authorities, workforce development, or other means-tested services, scores may inform:
  - **Recommendations** (for example suggesting programs, coaching modules, or supports that align with a person's strengths).
  - **Triage flags** (for example "may benefit from extra support with follow-through" as a prompt for a human worker, not an automated gate).
- Implementation uses:
  - **Score bands or quantiles** on facets or domains (for example "higher expressed," "typical," "lower expressed") rather than hard pass/fail thresholds.
  - **Composite indicators** where helpful (for example combinations of stability and drive facets for certain types of interventions).

**Stakes:**
- Scores guide extra help and tailored opportunities.
- **Supportive and additive only**: scores may suggest *more* support, outreach, or resources, never *less* access to existing services or benefits.
- Human workers retain final decision authority.

**Evidence Bar (Tier 2):**
- All Tier 1 evidence, plus:
- Predictive validity for relevant outcomes (for example program engagement, persistence, self-reported progress).
- Governance rules explicitly documented (for example what kinds of flags are permitted, who can override, audit trails).
- Ongoing bias monitoring by demographic and program context.
- Evidence that triage recommendations do not systematically disadvantage specific groups.

**Current Status:**
- **Not yet deployed.**
- Any move to Tier 2 requires completion of the Tier 2 evidence program and explicit partner agreement on documented governance rules.

---

#### Prohibited Uses (Explicit Red Line)

The following uses are **explicitly out of scope** for OpenStrengths and will be **prohibited by design and by policy**:

- Any use of scores as the sole or primary basis for **denying, reducing, or terminating** housing, income, food, health, or related benefits.
- Any use of scores as a **hiring/firing, licensure, or exclusion tool**.
- Any use where an adverse classification from OpenStrengths directly **triggers sanctions**.
- Any formal **pass/fail classifications** with legal or policy consequences.

**Why These Are Prohibited:**

OpenStrengths is **not designed or validated** as a high-stakes selection or sanctioning instrument, and we do not intend to support such uses. High-stakes uses would require a fundamentally different validity and fairness program, different content and score scales, and legal and policy review that are not part of the current instrument's scope.

---

#### Fit-for-Purpose, Risk-Adjusted Validation

The framework above reflects a fundamental design principle:

> **The level of psychometric scrutiny and governance is proportional to decision risk.**
>
> Early deployments are restricted to Tier 1 uses unless and until Tier 2 evidence and governance structures are co-designed with partners and reviewed by measurement experts. Prohibited uses remain categorically out of scope.

### 3. Implications for Validity and Design

Given these intended uses:

1. **Construct and content validity** (C-00, C-01, construct validation packet)
   - The six-domain, thirty-six-facet ontology must support meaningful interpretations of *relative* strengths across the profile.
   - Facet descriptions and items must be clearly interpretable for the target population and the professionals who will use the reports.

2. **Internal structure and reliability**
   - For profile uses, reliability at the **facet level** is important enough to support stable rankings and bands, even if domains are sometimes aggregated for communication.
   - The planned move from classical test theory to IRT is motivated by **precision and comparability** across forms and time, not by high-stakes decisions.

3. **Fairness and differential impact**
   - Even at low- to moderate-stakes, we need evidence that scores behave comparably across key groups (for example by demographic variables and by program context), especially when scores inform triage or recommendations.
   - DIF analyses and invariance checks become part of the routine evidence base, particularly as the item bank and populations expand.

4. **Consequential validity**
   - Because the target population includes individuals on means-tested services, we treat consequences as central:
     - Are profiles and recommendations experienced as helpful and respectful?
     - Do they support engagement, agency, and skill-building rather than stigma or fatalism?
   - This suggests mixed-method evaluation (for example qualitative feedback from test takers and practitioners) alongside psychometric evidence.

5. **Predictive and external criteria validity**
   - To support Tier 2 uses and to demonstrate that facet/domain scores have meaningful real-world correlates, we plan to examine relationships between scores and external outcomes (for example program engagement, persistence, completion, self-reported progress, or employment changes).
   - This work will build on the foundational Tier 1 evidence base and is critical for any move beyond purely descriptive profile uses.

### 4. Relationship to Other Concepts

- **Construct Model and Validation Packet**
  The construct validation work with university partners (for example domain/facet definitions and SME review) is the foundation for any meaningful score interpretation in this use framework.

- **C-03 – Three-Layer Consistency**
  The internal and operational consistency checks described in C-03 are tied to ensuring that:
  - Facet scores can be trusted for profile ranking and banding.
  - Longitudinal and cross-form comparisons remain defensible as the bank evolves.

- **C-04 / C-05 – Adaptation and Profile Flags**
  Adaptive delivery and personalization are constrained by the intended uses:
  - Profile flags may influence which items are seen and how they are worded, but they do not enter the latent trait model.
  - Any routing or CAT logic must preserve the comparability and interpretability of facet scores for their intended descriptive and triage roles.

### 5. Questions for Advisors

- Given these intended uses (profile + low/moderate-stakes triage, no current high-stakes pass/fail decisions), what would you consider the **minimal adequate validity program** in the early phases?
- Are there specific risks or unintended consequences you would flag for this population and use case that should be monitored from the outset (for example particular facets more sensitive to cultural or contextual interpretation)?
- If, in some settings, stakeholders request more decision-like uses (for example thresholds for program eligibility tiers), what conditions and additional evidence would you view as necessary before supporting those uses?

---

## C-10 – Response Process & Accessibility for Target Population

**Area:** Validity, Response Process, Accessibility
**Status:** Draft

### 1. Concept Summary

This concept describes the planned work to ensure that OpenStrengths items and score reports are **understandable, respectful, and culturally appropriate** for the target population: adults with lower educational attainment, many served by means-tested programs, often in rural or underserved contexts, and including individuals with trauma histories.

Response-process validity asks: **Do test takers interpret items in the way we intend?** This is critical for our population even at Tier 1 (low-stakes) uses, because misinterpretation undermines the meaning of scores and can erode trust or engagement.

Unlike purely psychometric analyses (factor structure, reliability, DIF), response-process work is **qualitative and user-centered**. It complements, but does not replace, statistical validation.

---

### 2. Planned Studies and Methods

#### 2.1 Cognitive Interviews / Think-Alouds

**Goal:** Understand how target users interpret item language, navigate response scales, and make sense of score reports.

**Sample:**
- Adults currently or recently enrolled in means-tested services (for example workforce development, housing supports, SNAP, TANF).
- Rural and underserved contexts.
- Mix of literacy levels and trauma exposure.
- Diverse demographic representation (race, ethnicity, age, gender, disability status).

**Protocol:**
- Present a subset of items (positive and reverse stems, across facets).
- Ask participants to:
  - Read the item aloud and explain what they think it means.
  - Identify any confusing, triggering, or culturally inappropriate language.
  - Choose a response and explain their reasoning.
- Present a sample score report and ask:
  - What does this profile tell you about yourself?
  - How would you use this information?
  - Does the language feel respectful and strengths-oriented, or stigmatizing?

**Analysis:**
- Identify items where intended meaning diverges from participant interpretation.
- Flag language that triggers distress, confusion, or cultural mismatch.
- Document participants' emotional and cognitive reactions to report framing.

---

#### 2.2 Reading Level and Language Simplification

**Goal:** Ensure items are accessible to adults with lower literacy levels without changing the underlying construct.

**Approach:**
- Use automated readability tools (for example Flesch-Kincaid, SMOG) as a first screen, targeting Grade 6–8 reading level for final items.
- Use LLMs to propose simplified phrasings of items that flag as too complex.
- Submit LLM-proposed simplifications to:
  - **SME review** (do they preserve construct alignment?).
  - **User feedback** (are they clearer for target readers?).
- Final decision on which simplifications to adopt rests with human reviewers informed by both SME and user input.

**Feedback Loop:**
- Findings feed back into **item models** (C-08), updating reading-level constraints and approved phrasing templates.
- Items that cannot be simplified without distorting the construct are flagged for possible retirement or restriction to higher-literacy subsamples.

---

#### 2.3 Trauma-Sensitive Item Design

**Goal:** Minimize triggering or re-traumatizing language while preserving construct coverage.

**Approach:**
- Review all items (especially those touching on stress, interpersonal conflict, or self-regulation) with a trauma-informed care specialist.
- Flag items that:
  - Use language implying blame or failure.
  - Reference specific trauma scenarios (abuse, violence, loss) unnecessarily.
  - Could be interpreted as intrusive or coercive.
- For flagged items:
  - Revise phrasing to focus on strengths and agency rather than deficits.
  - Offer context-neutral alternatives where possible (for example "in difficult situations" rather than "when facing violence").
  - Where revision is not feasible without losing construct meaning, mark the item with a `trauma_sensitive: true` flag so it can be excluded for individuals who opt into trauma-sensitive versions.

**Governance:**
- Trauma-sensitive exclusions are implemented via profile flags (C-05) at generation or selection time.
- Exclusions are documented and audited to ensure they do not systematically distort facet coverage or introduce new biases.

---

#### 2.4 Cultural Interpretation and Contextual Appropriateness

**Goal:** Ensure that facet descriptions and items reflect culturally diverse expressions of strengths, not just dominant-culture norms.

**Approach:**
- Engage community partners and focus groups representing the target population's diversity (race, ethnicity, region, immigrant status).
- Present facet definitions and sample items, and ask:
  - Do these descriptions resonate with how strengths show up in your community or context?
  - Are there ways of expressing this strength that are not captured by these items?
  - Are any items culturally biased or inappropriate?
- Use findings to:
  - Expand the diversity of anchor statements and item prompts within each facet's item models.
  - Identify facets where cultural interpretation varies significantly, flagging them for careful DIF analysis in pilots.

---

### 3. Relationship to Other Concepts

- **C-05 – Profile Flags**
  Accessibility features (reading level, trauma-sensitive wording) are implemented as profile flags that affect which items are eligible and how they are phrased, without changing the latent trait model.

- **C-08 – Item Models and Variants**
  Findings from response-process work inform the **incidental features** allowed within item models (for example reading level, trauma sensitivity) and help distinguish those from **radical features** that would change what is being measured.

- **C-03 – Consistency Framework (Semantic Layer)**
  Response-process feedback complements the NLI and embedding checks by validating that items are not only semantically aligned with constructs but also **interpretable by real users**.

- **C-09 – Score Uses and Stakes**
  Because the target population includes individuals in vulnerable contexts, response-process validity is treated as **essential even at Tier 1 (low-stakes) uses**, not as a luxury reserved for high-stakes testing.

---

### 4. Current Implementation Reality

- **Not yet implemented:**
  - No cognitive interviews or think-alouds have been conducted.
  - No formal trauma-informed review has been completed.
  - Readability screening and LLM-based simplification exist as prototype capabilities but are not yet part of the standard item-generation or approval workflow.

- **Design-level only:**
  - The workflows described here are intended plans that will be piloted before the first large-scale deployment.

---

### 5. Questions for Advisors

- In your experience, what is the **minimum sample size and diversity** for cognitive interviews or think-alouds to provide actionable evidence for response-process validity in a strengths assessment like this?
- Are there specific **red flags** or failure modes you would watch for when using AI to propose simplified or trauma-sensitive item phrasings (for example construct drift, unintended tone shifts)?
- Given our target population and Tier 1 uses, how would you prioritize response-process work relative to other early validation activities (for example pilot psychometrics, DIF analyses)?

---
