# OpenStrengths Technical Notes (Working Concepts) v0.3

_Last updated: November 30, 2025_

This document captures **working technical concepts** for the OpenStrengths platform that are **not yet fully validated** or finalized. It is an internal / expert-facing companion to the public white paper (v3.1).

Concepts here:

- May be partially implemented in code, partially on paper.
- Require **review and validation from psychometric / measurement experts**.
- Can be promoted into the white paper once validated, or revised / deprecated.

---

## Status Legend

Each concept has a `Status` field using one of:

- `Draft` – initial idea, may be incomplete or inconsistent.
- `Needs SME Review` – stable enough for expert review; waiting on feedback.
- `Under Review` – currently being discussed / evaluated with experts.
- `Validated` – agreed in principle by experts; ready to be promoted into canonical docs.
- `In Implementation` – being actively built in the product.
- `Deprecated` – concept was rejected, replaced, or folded into another idea.

Status values are expected to change over time as experts review these concepts and as pilots / studies are conducted.

---

## Concept Index

| ID   | Area                        | Concept                                                                 | Status            | Related White Paper Sections                                      |
|------|-----------------------------|-------------------------------------------------------------------------|-------------------|--------------------------------------------------------------------|
| C-00 | Platform / Engine           | Domain-agnostic construct-seeded generation engine                      | In Implementation | Part 2 – STEM Adapter (implicit), overall architecture             |
| C-01 | Item Generation             | Construct-seeded AI item-generation pipeline (reasoning chain)          | Needs SME Review  | Part 2 – STEM Adapter; “The Safety Question”                       |
| C-02 | Item Generation             | Anchors + embeddings as a semantic neighborhood for each construct      | Draft             | Part 2 – “Safety Question” (implied)                               |
| C-03 | Consistency & Linking       | Consistency framework (semantic → classical/IRT → operational) + FRIs   | Draft             | Part 2 – “Safety Question”; Roadmap – Phase 1 & 2                  |
| C-04 | Adaptation                  | Staged adaptive design (fixed → routed → CAT, with exposure control)    | Draft             | Part 2 – STEM Adapter (adaptive claims); Roadmap – Phase 1         |
| C-05 | Personalization vs Construct| Profile flags as eligibility / wording constraints (not part of trait)  | Needs SME Review  | Part 2 – User Classification System; STEM Adapter                  |
| C-06 | Validation Design           | Semantic-governance study (LLM gates vs SMEs on content validity)       | Draft             | Roadmap – Expert Validation; “The Safety Question”                 |
| C-07 | Validation Design           | Predictive & consequential validity strategy in means-tested populations| Draft             | Executive Summary; Roadmap – later phases                          |

---

## C-00 – Domain-Agnostic Construct-Seeded Generation Engine

**Area:** Platform / Engine  
**Status:** In Implementation  
**Related White Paper Sections:** Part 2 – STEM Adapter (implicitly as “STEM-like engine”), overall architecture

### 1. Concept Summary

The generation engine is intended to be **construct-seeded and domain-agnostic**.

Instead of being hard-coded only to our six domains and 36 canonical strengths facets, the engine is built around a general pattern:

1. A user (or system) defines a **construct specification**, which includes:
   - A construct name (e.g., “Curiosity”, “Job Engagement”, or a custom facet).
   - A short, canonical description of what the construct _is_ (and, where relevant, what it is not).
   - Optional high-pole and low-pole descriptions.
   - Optional example anchor statements that sound like realistic behaviors.

2. The engine uses this specification to create a **semantic space** for the construct:
   - Embed the construct description (and anchors, if provided).
   - Treat these vectors as the core of a semantic neighborhood representing “on-construct” content.

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
  - Generic “bring your own ontology” API surface for external users.
  - A full semantic screening pipeline applied to every generated item (C-02 / C-03 describe the intended design).

### 3. Questions / Validation Needed from Experts

We want to focus expert input on questions that materially affect whether this engine is **safe and defensible to generalize** beyond our own ontology:

- Given our current construct-spec schema (name, canonical description, optional poles, optional anchors), are there **specific additional fields** you would consider essential for downstream validity (e.g., intended population, hypothesized correlates/opposites, exclusions)?
- In your view, what are the **boundary conditions** where a domain-agnostic, construct-seeded engine like this _should not_ be used without a bespoke, construct-by-construct build (e.g., certain clinical or high-stakes contexts)?

### 4. Promotion Criteria

- Demonstrated use of the engine on at least one ontology beyond the OpenStrengths model (e.g., a small external construct set).
- Expert agreement that a construct-seeded, domain-agnostic architecture is acceptable in principle, given appropriate safeguards and clearly defined boundary conditions.

---

## C-01 – Construct-Seeded AI Item-Generation Pipeline (Reasoning Chain)

**Area:** Item Generation  
**Status:** Needs SME Review  
**Related White Paper Sections:** Part 2 – STEM Adapter; “The Safety Question: How Do We Know AI Doesn't Change What's Being Measured?”

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
   - Regardless of tooling, these canonical definitions are now treated as **human-owned ground truth** for what each construct means.

2. **The AI is asked to work _within_ those definitions.**  
   - The LLM receives the construct description and explicit rules (first-person, present tense, one behavior per item, etc.).
   - The LLM’s job is to propose possible wordings of behaviors that exemplify the construct.
   - It does **not** decide which constructs exist or what they mean; it suggests phrasing for constructs that have already been defined.

3. **We screen what the AI proposes.**  
   - NLI checks: “Does this item describe someone high (or low) on this construct?”  
   - Cross-construct checks: “Is it more aligned with some other construct instead?”  
   - Embedding checks: item vectors should be near the construct’s semantic neighborhood (see C-02).  
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
  - 6-domain, 36-facet strengths model with canonical descriptions.
  - LLM generation of stems from facet descriptions in the TypeScript app.
  - Embeddings for facet descriptions and anchors are stored in Supabase.
- Not yet implemented:
  - A full item-level semantic gate (NLI + embedding) that is applied to all generated stems.
  - A complete, logged pipeline from “construct spec → generation → semantic screening → candidate pool.”

### 3. Questions / Validation Needed from Experts

We are specifically interested in whether this division of labor is **psychometrically acceptable**, not in broad philosophical questions:

- Given this “constructs first, AI second” arrangement, are there **specific failure modes** you would expect (e.g., narrow content coverage, subtle construct drift) that we should explicitly test for in early pilots?
- When we run our first pilot with AI-generated items, what **minimum checks** beyond standard content review would you strongly recommend (e.g., additional SME panels, structured cognitive interviews)?

### 4. Promotion Criteria

- At least one measurement expert confirms that this reasoning chain is acceptable as a basis for item authoring, given proper semantic and psychometric checks.
- We have run at least one small pilot showing that construct-seeded, AI-written items behave roughly as expected at the factor structure level.

---

## C-02 – Anchors + Embeddings as Semantic Neighborhoods

**Area:** Item Generation  
**Status:** Draft  
**Related White Paper Sections:** Part 2 – “Safety Question” (implicitly); future technical notes

### 1. Concept Summary

Represent each construct as a **semantic neighborhood** defined by:

- The canonical construct description (and, where applicable, a low-pole description).
- A curated set of **anchor statements** that sound like realistic behaviors or self-descriptions at different intensities.

We then:

- Embed the construct descriptions and anchors into a vector space.
- Treat this cluster as the **“on-construct” region** for the construct.
- Embed candidate items and check:
  - Are they sufficiently close (above a threshold) to the construct’s neighborhood?
  - Are they not closer to some other construct’s neighborhood?

The goal is to use embeddings as an **early, automated content plausibility check**, not as the final arbiter of validity.

### 2. Current Implementation Reality

- Implemented:
  - Data structures for anchors in the TypeScript app.
  - Embeddings for construct descriptions and anchors stored in Supabase.
  - Ability to compute cosine similarity between an anchor and its construct description.
- Not yet implemented:
  - Embedding of all items (stems) with systematic thresholds for acceptance/rejection.
  - A multi-construct “semantic map” where overlap and distance across many constructs are monitored routinely.

### 3. Questions / Validation Needed from Experts

We assume embeddings are only a **screening tool**. We want help scoping their responsible use:

- Where have you seen semantic/embedding tools usefully support content validity work, and where do you see red flags (e.g., overfitting to surface wording, instability across models)?
- If we adopt embedding-based screening, what would you want to see in a **documentation or audit trail** to be comfortable that it is not substituting for SME judgement?

### 4. Promotion Criteria

- Expert consensus that embeddings are acceptable as a first-line screening tool, with clear caveats.
- A documented set of thresholds/rules, tested on at least one pilot dataset, showing how semantic screening correlates with SME judgements.

---

## C-03 – Consistency Framework (Semantic → Classical/IRT → Operational) + FRIs

**Area:** Consistency & Linking  
**Status:** Draft  
**Related White Paper Sections:** Part 2 – “Safety Question”; Roadmap – Phase 1 & 2

### 1. Concept Summary

A central question for our advisors is: **“How will you know your AI-generated items behave consistently across forms, domains, and facets?”**

Our working answer is a three-layer framework:

1. **Semantic Consistency (layer 1)**  
   - Use NLI and embeddings relative to construct descriptions and anchors to ensure that items:
     - Assert something consistent with the intended construct.
     - Do not align more strongly with another construct.
   - This is about **content alignment** _before_ any data is collected.

2. **Statistical Consistency (layer 2 – classical first, IRT later)**  
   - Start with **fixed or planned-missing designs** and **classical test theory (CTT)** analyses:
     - Item–total correlations,
     - Internal consistency (e.g., omega/alpha) per facet,
     - Exploratory and confirmatory factor analyses to see whether items load where the ontology says they should.
   - As the data matures, move to **polytomous IRT models** (e.g., graded response or partial credit), in line with expert guidance:
     - Estimate item parameters and information curves,
     - Check whether items behave similarly across calibration waves and subgroups.
   - This layer addresses consistency in **how items function** psychometrically.

3. **Operational Consistency (layer 3)**  
   - Only items that pass layers 1 and 2 become **operational items** available to any adaptive or fixed-form administration.
   - Introduce **Facet Reference Items (FRIs)**:
     - A small, stable subset of items per facet that remain in use over time.
     - FRIs anchor the scale so that new items and new forms can be linked back to previous calibrations.
   - Monitor item performance (e.g., fit statistics, information) and test-level information over time to detect drift.

The overall idea is:

> Semantic gates decide which items even _enter_ the candidate pool.  
> Classical/IRT analyses decide which items are worth keeping.  
> Operational rules (including FRIs) decide which items are trusted for live use and how we maintain comparability over time.

### 2. Current Implementation Reality

- Semantic layer:
  - Construct descriptions and anchors exist, with embeddings in Supabase.
  - The full NLI + embedding gate for every item is not yet implemented.
- Statistical layer:
  - No large-scale pilot has been run; classical analyses and IRT are not yet applied to real data.
  - A measurement expert has already suggested starting with **simpler, fixed-form designs and classical analyses** before committing to a specific IRT model or full CAT. This is consistent with moving from CTT → IRT → CAT over time.
- Operational layer:
  - No FRIs selected yet.
  - No live operational item bank or linking or CAT in production.

### 3. Questions / Validation Needed from Experts

We want targeted input on how to make this practical, given AI-generated items and our staged plan:

- For a first-wave pilot of AI-generated items, what **minimal but sufficient set of analyses** would you run to assess consistency across facets and forms (e.g., which CTT indices, which factor models)?
- When moving from CTT to IRT, which polytomous models would you consider good first candidates in this context, and what criteria would you use to decide whether they add enough value to justify the extra complexity?
- How many FRIs per facet, and what kinds of monitoring, would you consider “good enough” for maintaining linking in a low-stakes, evolving strengths assessment?

### 4. Promotion Criteria

- Agreement from measurement experts that this layered framework is sound and realistic for our scale.
- At least one pilot showing:
  - Semantic gates reduce obviously off-construct items.
  - Classical analyses and early IRT models produce coherent facet-level scales.
  - FRIs can be identified and appear stable across subsets.

---

## C-04 – Staged Adaptive Design (Fixed → Routed → CAT, with Exposure Control)

**Area:** Adaptation  
**Status:** Draft  
**Related White Paper Sections:** Part 2 – STEM Adapter (adaptive claims); Roadmap – Phase 1

### 1. Concept Summary

A central question for our advisors is: **“How is the examinee profile used to drive adaptive item selection and test assembly (including exposure controls)?”**

Our working plan is **staged**, not “jump straight to full CAT”:

1. **Stage 0 – Fixed or planned-missing forms (no adaptation)**  
   - Early pilots use fixed or planned-missing designs:
     - Good for factor structure checks and initial item statistics.
     - Simpler logistically and analytically.
   - This stage produces:
     - Basic item statistics (CTT),
     - Evidence about whether facets/domains behave as expected,
     - Data for selecting FRIs.

2. **Stage 1 – Simple routing within facets**  
   - Introduce a small set of “routing items” per facet that cover moderate endorsement levels.
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

- **Profile flags** (see C-05) affect _which items are eligible_ and _how they’re worded_ (role context, reading level, trauma-sensitive exclusions).
- The **latent trait model** itself (what the facet scores mean) does **not** incorporate these flags as predictors.

### 2. Current Implementation Reality

- No adaptive engine is implemented today.
- Profile flags and pre-assessment logic exist conceptually and in parts of the prototype.
- The staged design (0 → 1 → 2) is a working plan, influenced by expert advice to start with simpler designs and analyses before full CAT.

### 3. Questions / Validation Needed from Experts

- In a multi-facet strengths assessment like ours, is a staged path (fixed → routed → CAT) a sensible trajectory, or would you recommend a different progression?
- When we reach Stage 1, what would you consider a reasonable number of routing items and follow-up items per facet for early adaptive prototypes?
- For Stage 2, what level of exposure control (e.g., simple caps on item usage, randomization) is “good enough” for low-stakes, personal-development use cases?

### 4. Promotion Criteria

- Expert agreement on the staged approach and on the key design parameters for each stage.
- Pilot or simulation results showing that routing and CAT do not materially distort the facet score distributions compared to fixed-form baselines.

---

## C-05 – Profile Flags as Eligibility / Wording Constraints (Not Part of Trait)

**Area:** Personalization vs Construct  
**Status:** Needs SME Review  
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

Concretely:

- `role_context` influences the scenario framing of items (work, school, family), not the underlying trait.
- `language_simplification` governs reading complexity, not the latent trait.
- `trauma_sensitive` removes or rephrases potentially triggering items, but we aim to keep the underlying content coverage as parallel as possible.

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
**Status:** Draft  
**Related White Paper Sections:** Roadmap – Expert Validation; “The Safety Question”

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
- How should we predefine what counts as “acceptable” vs “unacceptable” consequences of using strengths profiles in these contexts?

### 4. Promotion Criteria

- A prioritized list of predictive / consequential validity studies agreed upon with advisors and implementation partners.
- At least one study designed and, where applicable, IRB/ethics-reviewed.

---
