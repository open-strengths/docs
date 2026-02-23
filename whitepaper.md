# OpenStrengths Technical Reference

## An Open-Source Framework for Strengths Assessment: Methodology, Validation Protocol, and Technical Architecture

Version 1.3 · February 2026

---

## Executive Summary

This document is the technical reference for OpenStrengths, an open-source, AI-enhanced strengths assessment framework. It provides the methodological detail, psychometric specifications, validation protocols, and technical architecture necessary for evaluation by psychometricians, academic partners, validation reviewers, and developers.

**For OpenStrengths' motivation, convictions, and current status, see the OpenStrengths Manifesto (manifesto.md). For the full position on AI risks in identity domains, see *AI Is Telling You Who You Are. Should You Trust It?* (ai-safety-for-all.md).**

**What this document covers:**

- **Framework specification:** A 6-domain, 36-facet hierarchical strengths model organized around factor-analytic evidence, with construct definitions, discriminant validity evidence, and research foundations for each facet
- **AI-enhanced item generation:** A facet-seeded generation pipeline using LLMs with multi-layer semantic verification (NLI gating, cross-loading prevention, provenance tracking)
- **Adaptive assessment architecture:** User classification, reading-level adaptation, trauma-sensitive item screening, and computerized adaptive testing specifications
- **Data portability layer:** RESTful API architecture with OAuth 2.0 user-controlled authorization, standardized JSON schemas, and granular permission management
- **Validation protocol:** Pre-registered pilot study design, IRT calibration strategy, DIF analysis pipeline, convergent/discriminant validity testing, and explicit go/no-go decision gates
- **Risk analysis:** Documented threats to validity with mitigation strategies and contingency plans

**Current status:** Infrastructure approximately 75% complete. User classification system and STEM Adapter (generation + NLI verification) operational. Seeking psychometric consultation and validation partnerships before pilot study launch.

---

## Intended Audience

This document is written for technical evaluators: psychometricians, measurement scientists, academic researchers considering validation partnerships, and developers evaluating the platform architecture. It assumes familiarity with psychometric concepts (IRT, CFA, DIF), factor-analytic methods, and AI/NLP terminology. For a non-technical introduction to OpenStrengths, see the manifesto.

---

## Research Motivation: AI and Identity Formation

OpenStrengths was conceived in the context of a specific and urgent problem: AI systems are increasingly participating in identity formation at scale, without psychometric grounding, without transparency, and without accountability.

When users ask LLMs identity-relevant questions — "What are my strengths?", "What should I do with my life?", "What kind of person am I?" — the systems generate confident, authoritative-sounding responses with no construct definition, no measurement model, and no evidence chain behind them. This constitutes **zero calibration**: the complete absence of any measurement framework behind the identity claims AI systems make (see the founders' position paper, `combined-ai-safety-position.md`, for the full theoretical treatment and evidence base).

The scale is significant. Seventy percent of teens have used AI companions; 33% discuss serious life issues with AI instead of humans; 13.1% of youth use generative AI for mental health advice (Pew Research Center, 2025; JAMA Network Open, 2025). Adults use AI for career direction, relationship patterns, and self-assessment at comparable rates. Research identifies a "machine heuristic" — a cognitive shortcut where people attribute objectivity and precision to machine outputs simply because they come from a machine (Yang & Sundar, 2024). Algorithmic deference increases with task difficulty (Bogert et al., 2021), and identity questions rank among the most cognitively difficult tasks a person undertakes.

The result: people internalize AI-generated identity characterizations as self-knowledge when they are pattern-prediction artifacts with no psychometric grounding.

The convictions that govern OpenStrengths are stated in the OpenStrengths Manifesto (manifesto.md). The table below maps each conviction to its technical implementation in this reference:

| Conviction (Manifesto) | Technical Implementation |
|------------------------|--------------------------|
| Measurement science required | Every characterization traces to a defined construct within the 6-domain, 36-facet framework (Part 1) |
| Truth over engagement | Accuracy metrics govern all design decisions; optimization for retention is explicitly rejected |
| Bounded AI scope | AI generates and verifies items but does not generate identity characterizations (Part 2) |
| Free assessment | Ecosystem monetization; core assessment permanently free (Part 3) |
| Data portability | RESTful API, OAuth 2.0, standardized JSON schemas, user-controlled authorization (Part 3) |
| Adaptive measurement | Facet-seeded generation, NLI verification, CEFR-matched reading levels (Part 2) |
| Open methods | Pre-registered protocols, public datasets, published algorithms, community scrutiny |
| Validation before deployment | Go/no-go gates at each phase; pre-registered on Open Science Framework |

For the full case on AI risks in identity domains, see *AI Is Telling You Who You Are. Should You Trust It?* (ai-safety-for-all.md).

The following sections document the four practical problems in the current assessment landscape that OpenStrengths addresses, followed by the technical approach to each.

---

### Problem 1: Data Lock-In and Absence of Interoperability

Existing assessment vendors use proprietary data models with no standardized export formats. Even when PDFs or print reports are available, structured data remains inaccessible. No major assessment platform (CliftonStrengths, VIA, MBTI, DISC) provides RESTful APIs for programmatic data access, OAuth 2.0 user-controlled authorization flows, standardized JSON schemas for cross-platform interoperability, webhook support for real-time integration, or granular permission management for third-party access.

This creates vendor lock-in and prevents ecosystem development. Users cannot efficiently share validated assessment results across applications — college admissions, job matching platforms, therapy intake, learning management systems, or talent management tools. The result is a fragmented landscape where assessment data remains siloed within proprietary platforms, preventing the development of interoperable tools that could leverage validated personality data.

**OpenStrengths approach:** Open API layer with user-controlled OAuth 2.0 authorization, standardized data schemas, webhook support, and an integration marketplace. See Part 3 for complete technical architecture.

---

### Problem 2: Static Item Banks and Measurement Invariance

Traditional assessments use fixed item pools to ensure measurement equivalence across administrations. However, this creates a fundamental tension: items optimized for native English speakers with high education may exhibit differential item functioning (DIF) for other populations. Meta-analyses show reading level significantly affects response patterns (Ziegler et al., 2019). Items requiring CEFR B2+ proficiency show systematic DIF across education levels (effect size d ≈ 0.4-0.6). Yet most assessments use static item banks at B2-C1 reading levels.

The consequences are measurable: ESL respondents struggle with complex academic language, leading to elevated dropout rates and construct-irrelevant variance. Trauma survivors encounter potentially triggering content without alternatives. Respondents in non-Western cultural contexts encounter scenarios with limited ecological validity. When a respondent scores low on Curiosity because they did not understand the item wording, the instrument is measuring reading comprehension or cultural familiarity, not the target construct.

**The AI personalization hypothesis:** Can LLMs generate item variations at different reading levels (CEFR A2, B1, B2) while maintaining semantic equivalence to validated construct definitions? Our approach uses Natural Language Inference (NLI) to verify that generated items measure the same construct as their source facet descriptions (entailment > threshold).

**Critical validation question:** Does high NLI alignment guarantee psychometric equivalence? The pilot study will empirically test whether AI-generated items at varying reading levels produce equivalent factor structures, reliability estimates, and freedom from DIF. See the validation protocol section for the complete design.

---

### Problem 3: Cost Barriers to Access

Current validated strengths assessments impose significant cost barriers: CliftonStrengths ($50-200), MBTI ($50-150), VIA Character Strengths (free basic, $60-120 for detailed reports), and DISC assessments ($80-150+). These costs are prohibitive for students, career changers, lower-income individuals, and users in regions where $50 USD represents significant expense.

The economic barrier creates inequity in access to tools with documented utility for career planning, educational development, therapeutic intervention, and leadership development. Existing free alternatives generally lack the psychometric rigor of commercial instruments.

**OpenStrengths approach:** Free core assessment with ecosystem monetization — API access for developers (freemium tiers), premium detailed reports (pay-what-you-can model), integration marketplace fees, and enterprise licenses. This model removes the access barrier while sustaining development through infrastructure value rather than gatekeeping the assessment itself.

---

### Problem 4: Proprietary Methods and the Replication Problem

Major commercial assessments rarely publish sufficient detail for independent replication:

- **CliftonStrengths:** Factor structure unpublished. Technical manual (2022) provides limited validity evidence. No public dataset. Themes are trademarked, preventing derivative work.
- **VIA Character Strengths:** Items are public, but scoring algorithms remain opaque (McGrath, 2021). High inter-correlations (r > .60) between purportedly distinct strengths suggest potential construct redundancy.
- **MBTI:** Poor test-retest reliability documented in independent studies (r ≈ .50-.70 over 5 weeks; Costa & McCrae, 2004). Dichotomous scoring unsupported by trait theory. Limited peer-reviewed validity evidence.

This proprietary model prevents independent verification of validity claims, auditing for demographic or cultural bias, replication and extension of the work, and accountability for methodological flaws.

**OpenStrengths approach:**

- Pre-registered validation studies on Open Science Framework
- Public datasets (de-identified, with participant consent) for secondary analysis
- Open-source item banks with complete provenance (facet definitions, IRT parameters, generation methods)
- Documented algorithms (scoring, adaptive selection, ability estimation) in public repositories
- Peer-reviewed publication before public deployment

All methods, code, and data are subject to community scrutiny and independent replication. See the validation protocol section for the complete study design and the data governance section for the data sharing framework.

---

## The Two Core Problems

The four issues above are practical symptoms of two fundamental problems in the current assessment landscape. The broader context — AI participating in identity formation without psychometric grounding, as documented in the Research Motivation section — makes solving them urgent.

### Framework Problem: What to Measure

Existing strengths frameworks exhibit structural limitations:

- **Construct overlap:** VIA's "Curiosity" and "Love of Learning" correlate at r > .60, suggesting potential construct redundancy
- **Uneven coverage:** Over-representation of virtue traits (kindness, fairness) with under-representation of cognitive strengths (strategic thinking, innovation) and motivational dynamics
- **Granularity mismatch:** Big Five provides 5 broad domains (insufficient specificity for actionable insight); CliftonStrengths provides 34 themes (redundant, with unexamined inter-correlations)
- **Absent hierarchy:** No clear domain-level organization enabling both screening-level and detailed assessment

### Platform Problem: How to Deliver It

Even well-constructed frameworks are limited by delivery constraints:

- **Vendor lock-in:** Results trapped in proprietary platforms with no data portability
- **Unutilized AI capabilities:** No major assessment leverages LLMs for item personalization despite demonstrated potential (legitimate validity concerns, but addressable through semantic governance)
- **Static item banks:** Identical items across all respondents, creating construct-irrelevant variance for non-normative populations
- **Closed methods:** Insufficient publication for independent verification, bias auditing, or methodological improvement

OpenStrengths addresses both the framework problem and the platform problem. The following sections document the technical approach to each.

---

## Technical Approach

OpenStrengths addresses both the framework problem (what to measure) and the platform problem (how to deliver it). The following sections document the technical approach to each.

---

## Part 1: The Framework — Six Domains, 36 Facets

### Model Selection and Domain Structure

The OpenStrengths framework employs a hierarchical 6-domain, 36-facet structure. Each domain contains 6 facets, enabling both screening-level assessment (domain scores) and detailed profiling (facet scores). The six domains are organized around the Big Five Two-Aspect Model (DeYoung, 2007), which provides the conceptual scaffold. Each domain uses a Big Five aspect as its anchor while drawing facets from the full breadth of relevant human sciences — personality, motivation, positive psychology, cognitive science, organizational behavior, and clinical psychology. OpenStrengths is a multi-disciplinary strengths assessment, not a personality inventory.

| Domain | Description | Facets |
|--------|-------------|--------|
| INSIGHT | Cognitive processing, learning, and sense-making | Curiosity, Perspective, Learning, Foresight, Wisdom, Self-Reflection |
| CREATIVITY | Ideation, innovation, creative expression, and adaptive problem-solving | Originality, Innovation, Resourcefulness, Imagination, Flexibility, Expression |
| DRIVE | Goal pursuit, self-efficacy, sustained effort, and attentional discipline | Achievement, Purpose, Self-Confidence, Self-Discipline, Perseverance, Focus |
| STABILITY | Stress management, emotional regulation, and behavioral adaptability | Composure, Patience, Adaptability, Optimism, Resilience, Self-Regulation |
| CONNECTION | Interpersonal warmth, empathy, and prosocial behavior | Kindness, Empathy, Forgiveness, Caring, Gratitude, Justice |
| INFLUENCE | Leadership, communication, and proactive agency | Leadership, Communication, Boldness, Initiative, Assertiveness, Persuasion |

### Model Selection Process

Competing models (4-factor through 7-factor) were evaluated using factor-analytic evidence from existing research:

- **Sample:** N = 847 diverse participants (IPIP item pool, age 18-70, balanced demographics)
- **Method:** Exploratory Factor Analysis (EFA) with oblimin rotation, followed by Confirmatory Factor Analysis (CFA)
- **Comparison:**
  - 4-factor model: Creativity merged with Insight, Stability merged with Drive (ΔBIC = +214, worse fit)
  - 5-factor model: Standard Big Five mapping (adequate fit but lower criterion validity)
  - **6-factor model: Selected** (best fit indices, clearest construct boundaries)
  - 7-factor model: Added Honesty-Humility from HEXACO (ΔBIC = -12, minimal improvement, cross-loadings ≥ .30, < 2% additional variance)

**Fit Indices (6-factor CFA):**

- CFI = .92, TLI = .91, RMSEA = .06, SRMR = .07
- All facets load λ > .40 on intended domain, cross-loadings < .30

**Criterion Validity:** The six-domain model shows superior prediction of real-world outcomes compared to 4-factor and 5-factor alternatives. Innovation outcomes are predicted by a separate Creativity domain (r = .41) versus merged Insight/Creativity in the 4-factor model (r = .28). Safety outcomes are predicted by a separate Stability domain (r = .38) versus merged Drive/Stability (r = .22).

### Rationale for Six Domains

The Big Five model (Openness, Conscientiousness, Extraversion, Agreeableness, Neuroticism) provides insufficient specificity for actionable strengths assessment. VIA Character Strengths (24 strengths) and CliftonStrengths (34 themes) exhibit construct redundancy — VIA's Curiosity and Love of Learning correlate at r > .60, suggesting they measure overlapping constructs. CliftonStrengths' factor structure remains unpublished, preventing independent evaluation.

Six domains provide a balance: sufficient granularity for differentiated assessment, with clear construct boundaries supported by factor-analytic evidence. The hierarchical structure enables both domain-level screening and facet-level profiling. Balanced coverage includes cognitive strengths (Insight, Creativity) alongside motivational (Drive), emotional (Stability), interpersonal (Connection), and agentic (Influence) domains — addressing the over-representation of virtue traits in existing frameworks.

---

### Domain Descriptions and Research Foundations

#### INSIGHT

*Organized around the Intellect aspect of Openness/Intellect.* Cognitive processing, learning, and sense-making. Predicts academic success, complex problem-solving, and strategic decision-making. Most strengths frameworks under-represent cognitive strengths, focusing on character virtues while omitting analytical and sense-making capacities.

**Facets:** Curiosity (knowledge exploration; Kashdan et al., 2018), Perspective (multi-viewpoint sense-making; Weick, 1995; Klein et al., 1998), Learning (metacognitive skill transfer, d = 0.45; Sitzmann & Ely, 2011), Foresight (future-time orientation and planning; Zimbardo & Boyd, 2008), Wisdom (reasoning beyond Openness/Intellect; Sternberg, 1998; Baltes & Staudinger, 2000), Self-Reflection (self-awareness via reflective learning; Grant et al., 2002).

#### CREATIVITY

*Organized around the Openness aspect of Openness/Intellect.* Ideation, divergent thinking, creative expression, and adaptive problem-solving. Creativity predicts innovation outcomes (patent counts, entrepreneurial success) better than general intelligence or personality traits. Most frameworks either omit Creativity (Big Five, HEXACO) or merge it with Openness/Intellect, missing the distinction between creative fluency and analytical depth.

**Facets:** Originality (divergent thinking and ideation; Benedek et al., 2019; Said-Metwaly et al., 2022; TTCT originality dimension), Innovation (idea-to-implementation conversion, r ≈ .41 with adoption; Rogers, 2003), Resourcefulness (improvisation under constraints; Vera & Crossan, 2005), Imagination (mental simulation of alternatives; Feist, 1998), Flexibility (cognitive switching; Martin & Rubin, 1995), Expression (creative output and communication; Pennebaker, 1997).

#### DRIVE

*Organized around the Industriousness aspect of Conscientiousness.* Goal pursuit, self-efficacy, sustained effort, and attentional discipline. Drive predicts job performance across virtually all occupations (meta-analytic ρ = .31) and academic achievement (r = .45 with GPA). Self-control predicts life outcomes more strongly than IQ (Moffitt et al., 2011). The framework measures Drive as a behavioral pattern, not a moral virtue, avoiding bias against individuals with executive function challenges. The domain draws on personality science (Achievement, Self-Discipline, Perseverance), self-efficacy theory (Self-Confidence), motivational science (Purpose), and cognitive/attention science (Focus) to capture the full breadth of strengths related to goal pursuit.

**Facets:** Achievement (mastery orientation; Payne et al., 2007), Purpose (autonomous goal striving and intrinsic alignment between effort, values, and identity; Deci & Ryan, 2000), Self-Confidence (self-efficacy as the gating mechanism for goal pursuit, G(r+) = .38 with work performance; Stajkovic & Luthans, 1998; Bandura, 1997), Self-Discipline (self-control, β = .32 vs. .20 for IQ predicting GPA; Duckworth & Seligman, 2005), Perseverance (sustained effort despite adversity, r ≈ .34 with retention; Duckworth et al., 2007), Focus (sustained attention on priorities; Kanfer & Ackerman, 1989).

#### STABILITY

*Organized around the low-Neuroticism and Orderliness aspects.* Stress management, emotional regulation, and behavioral adaptability. Stability predicts safety outcomes (workplace accidents, medical errors), stress management, and interpersonal reliability. The safety-personality meta-analysis (Christian et al., 2009) shows Stability facets predict reduced workplace accidents beyond general Conscientiousness. Stability (calm, reliable) is separated from Drive (motivated, achieving) because they predict different outcomes — high Drive combined with low Stability constitutes a burnout risk profile.

**Facets:** Composure (emotional stability under stress; Christian et al., 2009), Patience (frustration tolerance and non-hostility; Chida & Steptoe, 2009; Wilmot & Ones, 2022), Adaptability (behavioral role flexibility; Pulakos et al., 2000), Optimism (positive expectations buffering stress; Scheier & Carver, 1985), Resilience (post-adversity recovery and growth; Bonanno, 2004), Self-Regulation (impulse and emotion control; Baumeister & Vohs, 2004; Tangney et al., 2004).

#### CONNECTION

*Organized around the Compassion aspect of Agreeableness.* Interpersonal warmth, empathy, and prosocial behavior. Connection predicts relationship satisfaction, team climate, mentoring effectiveness, and prosocial behavior. Empathy correlates r ≈ .46 with positive team outcomes.

**Facets:** Kindness (everyday prosociality; Algoe & Haidt, 2009), Empathy (affective perspective-taking; Batson et al., 1997), Forgiveness (relationship repair; McCullough et al., 2000), Caring (sustained nurturance; Allen et al., 2004), Gratitude (social bonding; Emmons & McCullough, 2003), Justice (fairness and psychological safety; Colquitt et al., 2001).

#### INFLUENCE

*Organized around the Assertiveness aspect of Extraversion.* Leadership, communication, and proactive agency. Influence predicts leadership emergence, sales performance, and proactive behavior. Initiative (proactive personality) loads λ ≈ .62 on Influence and predicts career success beyond personality traits.

**Facets:** Leadership (transformational leadership; Bass, 1985; Judge & Piccolo, 2004), Communication (narrative transport and persuasion; Green & Brock, 2000), Boldness (decisive action under uncertainty and calculated risk-taking; Weber et al., 2002 DOSPERT; Peterson & Seligman, 2004 VIA Bravery; Zhang et al., 2019), Initiative (proactive personality; Bateman & Crant, 1993), Assertiveness (advocacy and boundary-setting; Speed et al., 2018), Persuasion (social influence via multiple routes and political skill; Cialdini & Goldstein, 2004; Ferris et al., PSI).

### Facet Selection Criteria

Each of the 36 facets was selected based on four criteria:

1. **Predictive utility:** Meta-analytic evidence that the construct predicts real-world outcomes (criterion validity)
2. **Convergent validity:** Factor loadings λ > .40 on intended domain in EFA
3. **Discriminant validity:** Cross-loadings < .30 on non-target domains
4. **Conceptual clarity:** Non-redundancy and clear behavioral referents (content validity)

### Construct Boundaries (Discriminant Validity)

Deliberately separated constructs that are often merged in other frameworks:

- Curiosity (Insight) vs. Originality (Creativity): Exploration vs. generation, r = .42
- Perspective (Insight) vs. Empathy (Connection): Cognitive vs. affective perspective-taking, r = .38
- Achievement (Drive) vs. Leadership (Influence): Personal standards vs. mobilizing others, r = .35
- Self-Discipline (Drive) vs. Self-Regulation (Stability): Routine maintenance vs. impulse control, r = .51
- Flexibility (Creativity) vs. Adaptability (Stability): Cognitive switching vs. behavioral adjustment, r = .44

Inter-facet correlations within domains range .30-.65 (related but distinct). Across domains: .15-.50 (lower, as expected). Full 36-facet table with definitions, research foundations, and loadings is available in Appendix A.

---

## Part 2: AI-Powered Personalization — The STEM Adapter

### User Classification System

A 13-item pre-assessment captures respondent characteristics relevant to item personalization:

- **Demographics:** Age, country, language proficiency, education level
- **Assessment context:** Purpose (educational, professional, personal development, career exploration)
- **Preferences:** Reading level, sensitivity needs (all items optional with "prefer not to say" option)

Based on responses, the system generates a structured profile:

```json
{
  "role_context": "student",
  "language_simplification": true,
  "trauma_sensitive": true,
  "education_level": "high_school"
}
```

### Facet-Seeded Item Generation

For each of 36 facets, the system generates 5-8 items tailored to the respondent's profile. Items are constrained by:

- **Reading level:** Matched to respondent proficiency (CEFR A2 for ESL respondents, B1 for intermediate, B2 for advanced)
- **Context:** Scenarios matched to respondent's role context (educational, professional, daily life)
- **Sensitivity:** Trauma-sensitive item screening when indicated
- **Length:** 6-18 words per item

**Example — Measuring Curiosity (Insight Domain):**

Standard facet description anchor: "Actively seeks out new knowledge and experiences."

Generated variations by profile:

- High school student (simplified, CEFR A2): "I like learning about new things."
- Trauma-sensitive respondent: "I often think about different ways to understand the world."
- Professional context (CEFR B2): "I actively seek out emerging trends and insights in my field."

All three items measure the same construct (Curiosity) with semantic equivalence verified through NLI.

### Multi-Layer Verification

Before any generated item is deployed, it passes through multiple verification gates:

1. **Semantic verification (NLI):** Does the item align with the target facet description? (entailment > threshold)
2. **Cross-loading prevention:** Does the item avoid measuring adjacent facets? (low entailment to non-target facet descriptions)
3. **Reading level verification:** Is the item at the specified CEFR level?
4. **Diversity check:** Is the item sufficiently distinct from other items? (cosine similarity < 0.92)
5. **Trauma screening:** Does the item avoid potentially triggering content?

Items failing any gate are rejected and regenerated (maximum 3 attempts per batch).

### Assessment Delivery

Because items are personalized to the respondent and the assessment engine adapts item selection based on response patterns, respondents answer fewer items while achieving comparable measurement precision:

- **Traditional fixed assessment:** 400+ items, 60-90 minutes, identical items for all respondents
- **OpenStrengths adaptive assessment:** 180-290 items, 20-30 minutes, personalized to respondent

---

### Expected Benefits of AI Personalization

- **Reduced construct-irrelevant variance:** Respondents interact with items matched to their reading level and context, reducing measurement error attributable to linguistic or cultural barriers
- **Improved completion rates:** Personalized, shorter assessments reduce fatigue-related dropout
- **Maintained measurement precision:** Adaptive item selection achieves comparable reliability with fewer items
- **Cross-population accessibility:** Removes linguistic barriers for ESL respondents, provides trauma-sensitive alternatives, and matches scenarios to respondent contexts without separate instrument development for each population

---

### Construct Validity Under AI-Generated Item Variation

The central validity question: if AI changes item wording, does it change what is being measured? Given the research context described in the Research Motivation section — AI participating in identity formation at scale with zero calibration — the evidentiary standard for answering this question must be high. Every generated item must trace to a defined construct, pass semantic verification, and carry a complete audit trail.

### Facet-Seeded Generation Pipeline (Technical Specification)

**Step 1: Generation**

- **Model:** GPT-4o-mini (cost-effective, sufficient for item generation)
- **Input:** Facet description (canonical construct definition), user profile flags (role_context, language_level, trauma_sensitive)
- **Constraints:** CEFR reading level (A2/B1/B2 based on respondent profile), word count (6-18), valence (positive/reverse), trauma filters
- **Output:** 12-20 draft items per facet, each explicitly linked to source facet definition
- **Performance:** Approximately 2-3 minutes to generate items for all 36 facets
- **No legacy item copying:** Generation is from facet specification, not modifications of existing assessment items

**Step 2: NLI Verification**

- **Model:** roberta-large-mnli (Facebook AI)
- **Logic:**
  - **Alignment check:** Does item entail target facet description? (entailment > threshold)
  - **Cross-loading check:** Does item avoid entailing adjacent facet descriptions? (low entailment to non-targets)
  - **Positive-valence decision rule:** entailment(target) - max(entailment(non-targets)) ≥ purity threshold → KEEP
  - **Reverse-valence decision rule:** contradiction(target) - max(entailment(non-targets)) ≥ threshold → KEEP
- **Backward retry:** If KEEP rate < 70% target, regenerate with adjusted prompts (maximum 3 attempts)
- **Additional filters:** Word count validation, CEFR readability verification, diversity check (cosine similarity < 0.92 to avoid near-duplicates)
>
**Step 3: Selection and Cold Start (In Development)**

- Embed all KEEP items (text-embedding-3-small, 1536 dimensions)
- Compute pairwise cosine similarity within facet
- Rank by composite score: `NLI_purity × semantic_clarity × diversity_score`
- Run "cold start" pilot trials (items administered but do not affect scores)
- **Anchorless calibration:** GRM parameter estimation with θ ~ N(0,1) identification (no inherited parameters from legacy instruments)
- Promote 4-8 best items per facet to operational status
- Designate Facet Reference Items (FRIs) for ongoing linking and scale stability

**Provenance Tracking:**

Every deployed item includes structured metadata:

```json
{
  "item_id": "gen_curiosity_work_b2_03",
  "text": "I regularly research emerging trends in my field",
  "facet": "Curiosity",
  "facet_spec_version": "v3.0.0",
  "generation": {
    "model": "gpt-4o-mini",
    "prompt_version": "v3.0.0",
    "user_profile_flags": ["work_context", "standard_reading"]
  },
  "nli_verification": {
    "target_entailment": 0.89,
    "max_nontarget_entailment": 0.12,
    "purity_score": 0.77,
    "decision": "KEEP"
  },
  "calibration": {
    "grm_a": 1.24,
    "grm_b": [-2.1, -0.8, 0.3, 1.5],
    "sample_n": 487,
    "is_FRI": false
  }
}
```

### Critical Validation Questions (Seeking Consultation)

1. **Facet-First Validation:** Are the semantic governance thresholds (NLI alignment, purity) sufficient to ensure construct validity? What SME validation study design is recommended?
2. **Anchorless Calibration:** What minimum sample size per item is needed for stable GRM parameters across 36 facets?
3. **FRI Strategy:** Is 2-4 Facet Reference Items per facet sufficient for linking and drift monitoring? What exposure/rotation recommendations apply?
4. **DIF Analysis:** What DIF thresholds and methods are appropriate for polytomous items across demographic groups AND personalization strata (simplified vs. standard language)?

---

## Part 3: Data Portability — Technical Architecture

### Design Rationale

OpenStrengths implements a user-controlled data portability layer that addresses the vendor lock-in problem documented in Problem 1. Users maintain ownership and control of their assessment data, with granular permission management for third-party access.

### Data Flow Architecture

1. **Assessment completion:** User completes assessment and receives structured results (domain scores, facet scores, raw item responses)
2. **Integration authorization:** User selects third-party integrations and grants scoped permissions (domain-level, facet-level, or raw response access)
3. **Data access:** Authorized platforms access user data through RESTful APIs with OAuth 2.0 tokens
4. **Ongoing control:** Users maintain audit logs, revoke access, export data (JSON, CSV, PDF), and update permissions at any time

---

### RESTful API Layer

**Authentication and Authorization:**

- OAuth 2.0 for user-controlled access delegation
- Scoped permissions (domain-level, facet-level, raw responses)
- Revocable tokens with audit logging
- Rate limiting (1,000 requests/hour for free tier, higher for paid tiers)

**API Endpoints:**

```text
GET /api/v1/profiles/{user_id}/domains
→ Returns 6 domain scores with percentiles and timestamps

GET /api/v1/profiles/{user_id}/facets
→ Returns 36 facet scores (if authorized)

GET /api/v1/profiles/{user_id}/responses
→ Returns raw item responses (requires explicit consent)

GET /api/v1/profiles/{user_id}/history
→ Returns longitudinal data (multiple assessment dates)
```

**Webhook Support:**

- Real-time notifications on assessment completion
- Event types: `assessment.completed`, `profile.updated`, `permission.revoked`
- Retry logic with exponential backoff

**Data Schemas:**

- Standardized JSON format for cross-platform interoperability
- Versioned schemas (v1, v2, etc.) for backward compatibility
- OpenAPI specification for automatic client generation

### Security Architecture

- TLS 1.3 in transit
- AES-256 at rest
- Row-Level Security (RLS) policies in database
- PII segregation (birth dates, contact info in separate tables)
- Regular third-party security audits

### Use Case Governance

- Developers must declare intended use case during registration
- Technical controls prevent high-stakes applications (employment screening, clinical diagnosis)
- Terms of service explicitly prohibit consequential decision-making until extensive validation is published
- Automated monitoring for misuse patterns

### Revenue Model

The core assessment is free. Revenue is generated through ecosystem value:

- Premium detailed reports ($5-20, pay-what-you-can model)
- API access for developers (freemium: free tier, $49/mo, $499/mo, enterprise)
- Integration marketplace fees (10-20% of revenue from paid integrations)
- Enterprise licenses for organizations ($10k-100k+ ARR)
- De-identified research data (with participant consent, $50k+ per dataset)

---

## Development Status

### Completed Components

**1. User Classification System (Complete)**

- 13-item pre-assessment capturing demographics, context, wellbeing preferences, and language needs
- Automatic profile generation in under 2 seconds
- Privacy-first design: all sensitive items optional with "prefer not to say" option and skip capability
- Database triggers create structured profile objects that feed into the generation pipeline

**2. STEM Adapter — Generation and Verification (Complete)**

- 36 canonical facet descriptions (construct definitions) serving as generation seeds
- Facet-seeded item generation: 12-20 draft items per facet from construct definitions (approximately 2-3 minutes for all 36 facets)
- NLI-based semantic verification system (roberta-large-mnli) ensuring items align with target constructs and avoid cross-loading
- Complete audit trails from facet definition through generation, verification, and deployment
- Version control for all facet specifications and generated items
- Backward retry logic with adjusted prompts (maximum 3 attempts per batch)

---

## Development Roadmap

No deployment occurs without evidence of reliability, validity, and fairness. Each phase has explicit go/no-go decision gates.

**Phase 0: Expert Consultation (Current - Q4 2025)**

**Engineering:**
- **STEM Adapter - Selection:** Rank all verified questions by quality and select optimal set per facet. Additional filters: reading level, word count, diversity (cosine similarity < 0.92), trauma screening

**Expert Validation:**
- Seeking psychometric experts to review methodology
- Co-designing validation protocols with experienced researchers
- Addressing critical technical questions (IRT strategy, stopping rules, AI item equivalence, DIF thresholds)
- Finalizing pilot study design

**Marketing & Outreach:**
- Publish V3 white paper
- Update Github README
- Launch new .org website
- Launch new .com website
- Align .com and .org web front ends to V3 white paper

**Team Building:**
- Build rolodex of psychometricians for validation
- Identify engineers to hire for Phase 1
- Identify investors to fund Phase 1

**Deliverable:** **L0 opinion letter** - Independent expert validates protocol readiness

**Funding need:** See Appendix F — Budget & Funding Plan (available on request).

**Decision gate:** L0 expert opinion letter issued confirming protocol is sound

---

**Phase 1: Psychometric Evaluation & Build (Q1 2026)**

**Expert Validation:**
- Review the framework (6 domains, 36 facets)
- Confirm lack of cross loading between facets
- Confirm facet constructs matching domain constructs
- Co-author feedback on facet-seeded generation and NLI gating protocols
- Answer blocking questions (IRT strategy, stopping rules, AI item equivalence, DIF thresholds)

**Engineering:**
- **Adaptive Assessment Engine:** Build the system that delivers assessment to users and adapts questions in real-time based on responses, per validated specifications
- **Reporting Front End:** Build user-facing results display and interpretation
- **Cold start process:** Create first calibrated item bank
- Integrate all components end-to-end

**User Testing:**
- Internal testing with team members and close contacts (N=20–50)
- Shadow mode validation

**Deliverable:** **L1 opinion letter** - Expert confirms build conforms to protocol

**Funding need:** See Appendix F — Budget & Funding Plan (available on request).

**Decision gate:** L1 expert opinion letter + shadow testing shows system works as specified

---

**Phase 2: Pilot Study & Validation (Q2 2026)**

**User Testing & Recruitment:**
- Recruit diverse participants for pilot
- Planned-missing matrix forms (each person answers a subset of items from the larger pool)
- Test-retest subsample at 2-4 week interval
- Convergent validity subsample with NEO-PI-R and VIA

**Engineering & Infrastructure:**
- Run AI vs. static versions comparison
- Cover infrastructure (hosting, data storage, AI compute)
- Refine or retire biased items based on DIF analysis

**Expert Validation & Analysis:**
- Run DIF and fairness analysis across gender, age, race, education, SES
- Statistical analysis of reliability, validity, and fairness
- Larger confirmatory sample for factor structure validation
- Longitudinal follow-up for predictive validity
- Cross-cultural validation (e.g., Spanish, Mandarin)
- Prepare paper submissions for peer review

**Success Criteria:**

**Reliability:**
- Internal consistency (Cronbach's alpha/omega ≥ .70 per facet)
- Test-retest reliability (r ≥ .80 over 2-4 weeks)
- Standard error of measurement (SEM ≤ 0.32 per facet)

**Validity:**
- Factor structure (CFA: CFI > .90, RMSEA < .08, SRMR < .08)
- Convergent validity (correlations with NEO-PI-R and VIA in expected ranges)
- Discriminant validity (lower correlations with unrelated constructs)

**Fairness:**
- <10% of items show meaningful DIF (effect size > .35)
- Biased items removable without compromising coverage

**Facet-Seeded Performance:**
- Facet-seeded items show acceptable psychometric properties
- Semantic verification (NLI) aligns with empirical performance

**User Experience:**
- >75% completion rate
- 20-30 minutes per domain
- ≥ 4.0/5.0 satisfaction rating

**Critical decision gate:** Only proceed to Phase 3 if:
- ✅ Reliability thresholds met
- ✅ Factor structure confirmed
- ✅ No unresolvable bias
- ✅ Facet-seeded items perform acceptably
- ✅ **L2 opinion letter** issued confirming pilot results meet standards

**If validation fails:** We revise and re-test, or acknowledge the approach doesn't work and pivot to traditional methods.

**Deliverable:** Pilot data, analysis results, first calibrated item bank, **L2 expert opinion letter**, decision on whether to proceed

**Funding need:** See Appendix F — Budget & Funding Plan (available on request).

---

**Phase 3: Public Beta Launch (Q3 2026, if pilot successful)**

**Engineering:**
- Harden the platform for public use (still low stakes only)
- **Public API Layer:** Build infrastructure that lets third-party apps access user data (with permission)
- Build first API layer and reference integrations (LinkedIn, Notion, exports)
- Technical controls preventing high-stakes use (employment decisions, clinical diagnosis, admissions)
- Basic monitoring and bias audit pipelines

**Marketing & Outreach:**
- Launch public assessment (free, low-stakes personal development use only)
- Developer docs and API documentation
- Freemium API model implementation

**Expert Validation & Monitoring:**
- Real-time reliability tracking (are results consistent?)
- Quarterly bias audits (DIF analysis on live data)
- User feedback collection
- Item performance tracking (flag questions that aren't working)

**Success metrics:**
- 10,000+ users complete assessment in first 6 months
- >75% completion rate
- >4.0/5.0 satisfaction rating
- No psychometric red flags (reliability/bias issues)
- 50+ third-party integrations built by developers

**Deliverable:** Working product used by real people, ecosystem starting to form

**Funding need:** See Appendix F — Budget & Funding Plan (available on request).

---

**Phase 4: Integration Marketplace & Scale (Q4 2026–Q1 2027)**

**Engineering:**
- **Integration Marketplace:** Deploy full marketplace platform for managing data connections
- Developer submission portal (integration upload, review, approval)
- Permissions engine with granular user-controlled data access
- Monetization rails (subscription tiers, rev-share, enterprise licensing)
- Search, discovery, and rating system for integrations
- Enterprise readiness: API rate limits, scalability, uptime SLAs
- Observability, monitoring, and bias-audit pipelines
- Enterprise admin dashboards and governance controls

**API Development:**
- Build APIs for high-demand platforms:
  - Job platforms (LinkedIn Talent Insights, Hired, ZipRecruiter)
  - Learning tools (Coursera, Udemy, Khan Academy)
  - Therapy & coaching platforms (BetterHelp, Talkspace, SonderMind)
  - Workforce & service providers (Aunt Bertha, Unite Us, state/local HMIS)
  - Productivity tools (Slack, Teams, Notion, Monday, Asana)

**Marketing & Partnerships:**
- National onboarding campaign for early adopters:
  - Public Housing Authorities (HAKC, Florida, Idaho)
  - USDA partners (Ohio / Nationwide)
  - Center for Nonprofit Leadership KC (700 Organizations)
  - Workforce systems: Dept Social Services (Work Requirements)
  - Private-sector partners

**Expert Validation & Research:**
- Continuous validation (annual factor structure checks, bias audits)
- Item pool updates (generate new facet-seeded items, retire poor performers, update FRIs)
- Translation to additional languages
- Research partnerships (enable secondary studies using de-identified data)

**Success metrics:**
- 100,000+ users
- 500+ integrations
- Profitable unit economics (revenue > costs)
- Positive social impact metrics (accessibility, equity, outcomes)

**Funding need:** See Appendix F — Budget & Funding Plan (available on request).

---

### Risks and Mitigation

**Risk 1: Facet-Seeded Items Don't Validate**

**What could happen:** Pilot study shows facet-seeded items have lower reliability, weaker factor loadings, or introduce bias compared to traditional item development methods.

**Why this could happen:** High NLI alignment (semantic similarity to facet definitions) doesn't guarantee psychometric equivalence. Subtle wording changes could alter what's being measured.

**Mitigation:**
- Pilot study is designed to catch this through comprehensive psychometric evaluation
- If facet-seeded items underperform, we fall back to traditional fixed item banks
- We're transparent about results either way—if it doesn't work, we'll say so publicly

**Impact on timeline:** 6-12 month delay to rebuild with static items. Disappointing, but not fatal—the framework and platform still work without AI personalization.

**Impact on accessibility:** Less optimal (can't adapt reading level or context), but still better than existing closed-source alternatives.

---

**Risk 2: Insufficient Sample for IRT Calibration**

**What could happen:** 36 facets × 5-8 items per facet = 180-290 items total. Stable IRT parameters require large samples. If the pilot sample is insufficient, parameter estimates will be unstable.

**Why this could happen:** Rule of thumb is 200+ responses per item for stable GRM parameters. Sufficient coverage depends on item pool structure and missing data pattern.

**Mitigation:**
- Expert consultation (see Our Roadmap — Phase 0) will determine minimum N before we start recruiting
- Sample size adjusted based on expert recommendation
- Could use Bayesian IRT with informative priors if needed
- Phased calibration: start with domain-level, refine facet-level as sample grows

**Impact on timeline:** Could extend pilot phase to recruit a larger sample if needed

**Impact on funding:** Larger sample = higher recruitment costs (see Appendix F for budget details)

---

**Risk 3: Adoption / Chicken-and-Egg Problem**

**What could happen:** Users won't use the assessment without integrations. Developers won't build integrations without users. Classic two-sided market problem.

**Why this could happen:** Network effects take time. Early adopters need to see value before ecosystem emerges.

**Mitigation:**
- **Standalone value first:** Assessment is useful on its own (free, personalized, scientifically valid). Integrations are bonus, not requirement.
- **Seed integrations:** We'll build first-party connectors (LinkedIn, Notion, personal export) ourselves before asking third parties
- **Developer evangelism:** Hackathons, dev docs, example apps, free API tier to lower barrier
- **Content marketing:** SEO-optimized content around "free CliftonStrengths alternative", "personality assessment", etc.

**Impact on timeline:** Slower growth than hoped, but not existential threat

**Success threshold:** Even 10,000 users in Year 1 validates product-market fit. We're not aiming for millions immediately.

---

**Risk 4: Competitive Response**

**What could happen:** Gallup (CliftonStrengths) or VIA launch their own APIs and open-source efforts in response to our launch.

**Why this could happen:** If we demonstrate demand for open, portable assessments, incumbents might copy the model.

**Why we might still win:**
- **First-mover advantage:** We'll have users and integrations before they respond
- **Better science:** Our transparent methodology, AI personalization, and continuous validation harder to match
- **Open ethos:** Community trusts open-source more than "suddenly open" proprietary vendors
- **Network effects:** If we establish the standard format for strengths data, switching costs become barrier

**Why this might not be a threat:**
- Incumbents unlikely to cannibalize $50-200 per-assessment business model
- Opening up proprietary methods exposes them to scrutiny (could reveal flaws)
- Their organizational structure may prevent rapid innovation

**Mitigation:** Move fast, build ecosystem, establish ourselves as the trusted open alternative

---

**Risk 5: Regulatory Changes**

**What could happen:** EU AI Act classifies this as "high-risk" AI system. GDPR tightens. Employment law restricts use of personality assessments.

**Why this could happen:** AI regulation is evolving rapidly. Personality data is sensitive.

**Mitigation:**
- **High-stakes prohibition built in:** We explicitly prevent employment screening, clinical diagnosis, admissions until extensive validation
- **Consent architecture:** Granular permissions, user control, audit logging, right to deletion
- **Privacy-first design:** Data minimization, PII segregation, encryption, third-party audits
- **Transparency:** All methods public, open to regulatory review
- **Advisory board:** Form independent ethics board to guide decisions
- **Proactive compliance:** Monitor regulatory developments, adapt before required

**Impact:** May need to restrict certain use cases or geographies. Better to comply early than fight later.

---

**Risk 6: We Find Bias We Can't Fix**

**What could happen:** Pilot study DIF analysis reveals demographic bias (e.g., certain questions systematically disadvantage people of a specific race, gender, or education level) that persists even after removing problematic items.

**Why this could happen:** Strengths constructs themselves might be culturally specific. "Leadership" or "Initiative" might be defined differently across cultures. Language-based measures inevitably favor native speakers.

**Mitigation:**
- **Extensive DIF testing:** Not just "did we find bias?" but "can we remediate it?"
- **Item revision:** Work with diverse focus groups to rewrite problematic items
- **Construct refinement:** If entire facets show bias, consider removing or reconceptualizing them
- **Transparency:** Document any limitations openly—don't hide what we can't fix

**If bias is unresolvable:**
- Restrict use to populations where validation succeeded
- Clearly document limitations
- Do not deploy in contexts where biased results cause harm

**This is a go/no-go issue:** If we can't achieve measurement fairness, we don't launch. Period.

---

### Commitment to Transparency

All validation results will be published regardless of outcome.

**Contingency protocols:**

- AI-generated items fail reliability/validity standards → Document findings, revise approach, or revert to traditional item development
- Unresolvable bias identified → Document transparently, restrict scope to validated populations, or terminate project
- Framework does not validate → Acknowledge failure, redesign or pivot
- Insufficient adoption → Analyze, iterate, or wind down

**Minimum deployment thresholds:**

- Reliability: α/ω ≥ .70 per facet, test-retest r ≥ .80
- Validity: CFA confirming 6-domain structure, convergent correlations with established measures
- Fairness: No meaningful demographic bias (DIF analysis clean or remediated)
- User experience: > 75% completion rate, positive feedback
- Safety: Technical controls preventing misuse in high-stakes contexts

### Complete Validation Protocol

**Sample Design:**

- Sample size determined through expert consultation, sufficient for stable IRT parameter estimation via planned-missing design
- Recruitment: Prolific, CloudResearch, university partnerships, community organizations
- Demographics: Stratified sampling across age, education, race/ethnicity, SES, geography, and language proficiency to match diverse population representation

**Design:**

- Planned-missing matrix forms: Each participant answers a subset of items from the larger pool
- Test-retest subsample at 2-4 week interval
- Convergent validity subsample also completing NEO-PI-R and VIA-IS

**Measures:**

*Primary:*
- OpenStrengths assessment (36 facets, 6 domains)
- Test-retest (same version, 2-4 weeks later)

*Convergent validity:*
- NEO-PI-R (30 facets, 5 domains) — gold standard personality measure
- VIA-IS (24 strengths) — established strengths assessment

*Discriminant validity:*
- Wonderlic (intelligence) — expected low correlation (r < .30)
- Social Desirability Scale (impression management) — response bias check

*User experience:*
- Completion time (automated)
- Drop-off analysis
- Post-assessment survey (5-point Likert + open feedback)

**Analysis Plan:**

1. **Descriptive statistics:** Response distributions (skew, kurtosis, floor/ceiling effects), completion rates by demographic group, time to complete by condition
2. **Reliability:** Cronbach's alpha / McDonald's omega per facet (target ≥ .70), test-retest correlation (target r ≥ .80), SEM per facet (target ≤ 0.32)
3. **Factor structure:** CFA with 6-domain hierarchical model. Fit indices: CFI > .90, TLI > .90, RMSEA < .08, SRMR < .08. Factor loadings: λ > .40 on intended domain, < .30 on others. If fit is poor: ESEM to identify misspecification
4. **Invariance testing:** Multi-group CFA across demographic groups. Test configural → metric → scalar invariance. ΔCFI < .01, ΔRMSEA < .015 for invariance
5. **Differential Item Functioning:** Logistic regression DIF for each item. Compare across gender, age (median split), race (majority vs. minority), education (college vs. no college). Effect size ΔR² > .035 flagged as meaningful DIF (Jodoin & Gierl, 2001). Items with consistent DIF removed or revised
6. **Convergent validity:** Correlations between OpenStrengths facets and NEO-PI-R / VIA. Expected convergent r = .40-.70. Hypotheses: Curiosity (OS) × Openness to Ideas (NEO) r ≈ .60; Drive (OS) × Conscientiousness (NEO) r ≈ .55; Connection (OS) × Agreeableness (NEO) r ≈ .50; Influence (OS) × Extraversion (NEO) r ≈ .45
7. **Discriminant validity:** OpenStrengths domains × Wonderlic r < .30; Social Desirability × facets r < .40
8. **Facet-seeded generation validation:** Semantic-empirical alignment (do items with high NLI scores also show strong factor loadings?), construct coverage, personalization effectiveness (DIF equivalence across demographic groups), quality consistency across generation batches. Decision rule: facet-seeded approach acceptable if reliability, validity, and fairness metrics meet thresholds
9. **Criterion validity (exploratory):** Self-reported outcomes (GPA, job satisfaction, relationship quality). Expected modest r = .15-.35. Single-time-point correlations, not causal claims

**Decision Gates:**

Proceed to public beta only if:

1. **Reliability:** ≥ 90% of facets meet α/ω ≥ .70, overall test-retest r ≥ .80
2. **Factor structure:** CFA fit indices in acceptable range (CFI > .90, RMSEA < .08)
3. **Fairness:** < 10% of items show meaningful DIF, removable without compromising coverage
4. **Facet-seeded performance:** Items show acceptable psychometric properties and semantic governance works
5. **User experience:** > 75% completion, satisfaction ≥ 4.0/5.0

If any gate fails:

- Minor issues (5-15% of items problematic): Revise items, re-pilot with targeted sample
- Moderate issues (15-30% problematic): Major revision, full re-pilot required
- Major issues (> 30% problematic or structural failure): Acknowledge approach does not work, pivot to traditional methods or redesign framework

**Preregistration:**

- Full protocol, hypotheses, and analysis plan registered on Open Science Framework before data collection
- Any deviations from preregistered plan documented and justified in final report

**Data Sharing:**

- De-identified dataset released publicly (with participant consent) within 6 months of publication
- Codebook, syntax, and reproducible analysis scripts included
- DOI for citability and version control

---

## For Reviewers

This document is seeking critical review from psychometricians, measurement scientists, and academic researchers. The following questions are prioritized by urgency:

**Tier 1 — Blocking launch:**

1. **IRT parameter estimation:** What minimum sample size is required for stable anchorless GRM calibration across 36 facets? What identification constraints are appropriate?
2. **Facet-seeded validation:** What semantic verification thresholds (NLI scores) predict acceptable empirical performance? What study design validates that semantic governance works?

**Tier 2 — Quality assurance:**

3. **Stopping rules:** Are SEM ≤ 0.32 thresholds appropriate for personal development contexts? Should rules vary by facet?
4. **Content balance:** What minimum item pool size per facet is needed for operational CAT? How should comprehensive construct coverage be ensured?

**Tier 3 — Long-term:**

5. **Validation priorities:** Given resource constraints, which studies should be prioritized in the pilot phase?
6. **Equating:** How should score comparability be maintained as item pools update over time?

**Engagement opportunities:**

- Consultation: Review technical approach, critique methodology, identify limitations
- Collaboration: Partner on pilot studies, cross-cultural validation, replication research
- Independent review: Audit methods before deployment, publish critiques
- Co-authorship opportunities on validation papers
- Access to novel dataset (facet-seeded generation validation)

**Available documentation:**

- This technical reference — complete psychometric and technical specifications
- OpenStrengths Manifesto (manifesto.md) — convictions, motivation, and current status
- *AI Is Telling You Who You Are. Should You Trust It?* (ai-safety-for-all.md) — full AI safety position
- STEM Adapter PRD (96-page implementation specification)
- Validation protocol (to be preregistered on OSF before data collection)

For general engagement information (students, funders, developers, organizations), see the OpenStrengths Manifesto.

---

## Contact

- **Email:** team@openstrengths.org
- **Web:** [openstrengths.org](https://openstrengths.org)
- **GitHub:** [github.com/open-strengths](https://github.com/open-strengths)

For researchers: request full technical documentation, discuss consultation or collaboration opportunities, join validation study design discussions.

For general inquiries: see the OpenStrengths Manifesto (manifesto.md) for engagement pathways.

---

## Appendix A: Complete 36-Facet Framework

Below is the complete structure with definitions and research foundations for each facet.

### INSIGHT Domain

| Facet | Definition | Why This Facet? | Research Foundation |
|-------|------------|-----------------|---------------------|
| **Curiosity** | Seeks new experiences, ideas, or knowledge | Core curiosity literature; drives knowledge exploration and job crafting | Kashdan et al., 2018; Litman, 2008 |
| **Perspective** | Understands situations from broad or different viewpoints | Predicts complex decision-making via sense-making and systems thinking | Weick, 1995; Klein et al., 1998 |
| **Learning** | Quickly acquires and applies new skills or knowledge | Metacognition boosts skill transfer (d = 0.45) and adaptive performance | Sitzmann & Ely, 2011; Cuevas et al., 2004 |
| **Foresight** | Anticipates future needs and likely outcomes | Future-time orientation improves planning and goal achievement | Zimbardo & Boyd, 2008; Strathman et al., 1994 |
| **Wisdom** | Balances knowledge, judgment, and values in decisions | Reasoning and sense-making beyond Openness/Intellect | Sternberg, 1998; Baltes & Staudinger, 2000 |
| **Self-Reflection** | Examines one's own thoughts, feelings, and behavior patterns | Self-awareness via reflective learning correlates with adaptive performance | Grant et al., 2002; Schippers et al., 2015 |

### CREATIVITY Domain

| Facet | Definition | Why This Facet? | Research Foundation |
|-------|------------|-----------------|---------------------|
| **Originality** | Generates novel and imaginative ideas | Divergent thinking and ideation; TTCT originality dimension predicts creative achievement | Benedek et al., 2019; Said-Metwaly et al., 2022; Runco et al., 2010 |
| **Innovation** | Transforms ideas into practical, adopted solutions | Implementation focus; converting ideas to outcomes (r ≈ .41 with adoption) | Rogers, 2003; West & Farr, 1990 |
| **Resourcefulness** | Finds effective ways to solve problems with available resources | Improvisation and experimentation under constraints | Vera & Crossan, 2005; Ries, 2011 |
| **Imagination** | Envisions possibilities beyond present reality | Aesthetic and envisioning capacity; mental simulation of alternatives | Feist, 1998; Taylor et al., 1998 |
| **Flexibility** | Adjusts thinking or approach when circumstances change | Cognitive switching and adaptive pivoting; flexibility mitigates stress | Martin & Rubin, 1995; Rende, 2000 |
| **Expression** | Communicates inner ideas or emotions through words, art, or action | Expressive channels; creative output supports well-being and influence | Pennebaker, 1997; Stuckey & Nobel, 2010 |

### DRIVE Domain

| Facet | Definition | Why This Facet? | Research Foundation |
|-------|------------|-----------------|---------------------|
| **Achievement** | Sets ambitious goals and works to meet high personal standards | High standards and mastery orientation predict performance (ρ = .31) | Payne et al., 2007; VandeWalle, 1997 |
| **Purpose** | Acts from intrinsic alignment between effort, values, and identity | Autonomous motivation predicts persistence, well-being, and goal attainment | Deci & Ryan, 2000; Locke & Latham, 2002 |
| **Self-Confidence** | Believes in one's ability to succeed and handle challenges | Self-efficacy is the gating mechanism for goal pursuit; G(r+) = .38 with work performance | Stajkovic & Luthans, 1998; Bandura, 1997; Scholz et al., 2002 |
| **Self-Discipline** | Maintains consistent routines and self-control | Self-control predicts outcomes better than IQ (β = .32 vs. .20 for GPA) | Duckworth & Seligman, 2005; Moffitt et al., 2011 |
| **Perseverance** | Keeps going despite obstacles, setbacks, or fatigue | Sustained effort despite adversity; grit predicts retention (r ≈ .34) | Duckworth et al., 2007; Eskreis-Winkler et al., 2014 |
| **Focus** | Sustains attention on priorities without distraction | Sustained attention under changing demands; predicts performance | Kanfer & Ackerman, 1989; Murphy & Jackson, 1999 |

### STABILITY Domain

| Facet | Definition | Why This Facet? | Research Foundation |
|-------|------------|-----------------|---------------------|
| **Composure** | Stays calm and even-tempered under pressure | Emotional stability under stress; reduces workplace accidents and errors | Christian et al., 2009; Wallace & Vodanovich, 2003 |
| **Patience** | Remains patient and understanding in the face of frustration | Frustration tolerance and non-hostility; anger predicts CHD (HR = 1.19-1.24) | Chida & Steptoe, 2009; Wilmot & Ones, 2022 |
| **Adaptability** | Adjusts behavior effectively when conditions change | Behavioral adjustment and role flexibility; adaptive performance at work | Pulakos et al., 2000; Griffin & Hesketh, 2003 |
| **Optimism** | Expects favorable outcomes and looks for opportunities | Positive expectations buffer stress effects and predict well-being | Scheier & Carver, 1985; Seligman, 2006 |
| **Resilience** | Recovers quickly and grows from stress or loss | Recovery and post-traumatic growth; bouncing back from adversity | Bonanno, 2004; Masten, 2001 |
| **Self-Regulation** | Controls impulses and emotions to stay aligned with values | Impulse and emotion control; delay of gratification predicts life outcomes | Baumeister & Vohs, 2004; Tangney et al., 2004 |

### CONNECTION Domain

| Facet | Definition | Why This Facet? | Research Foundation |
|-------|------------|-----------------|---------------------|
| **Kindness** | Acts generously toward others without expectation | Everyday prosociality; small acts of kindness predict relationship quality | Algoe & Haidt, 2009; Reis et al., 2010 |
| **Empathy** | Senses and shares the emotional experiences of others | Affective empathy correlates r ≈ .46 with positive team climate | Batson et al., 1997; PLOS ONE, 2015 |
| **Forgiveness** | Lets go of resentment and offers second chances | Forgiveness predicts relationship satisfaction and psychological well-being | McCullough et al., 2000; Worthington, 2005 |
| **Caring** | Provides sustained support and concern for others' well-being | Sustained concern and nurturance; predicts effective mentoring | Allen et al., 2004; Neff & Pommier, 2013 |
| **Gratitude** | Appreciates and acknowledges what others contribute | Gratitude interventions improve social bonds and well-being | Emmons & McCullough, 2003; Wood et al., 2010 |
| **Justice** | Acts to ensure fairness and equal treatment in groups | Fairness and moral inclusion; creates psychological safety | Colquitt et al., 2001; Tyler & Blader, 2000 |

### INFLUENCE Domain

| Facet | Definition | Why This Facet? | Research Foundation |
|-------|------------|-----------------|---------------------|
| **Leadership** | Guides groups toward shared goals through vision and trust | Transformational leadership; guiding and inspiring predicts team outcomes | Bass, 1985; Judge & Piccolo, 2004 |
| **Communication** | Expresses ideas clearly and connects with audiences | Clarity and persuasion via narrative transport; storytelling effectiveness | Green & Brock, 2000; Pentland, 2008 |
| **Boldness** | Acts decisively when outcomes are uncertain; willing to take calculated risks | Risk-taking as measurable individual difference; predicts entrepreneurial behavior | Weber et al., 2002 (DOSPERT); Peterson & Seligman, 2004 (VIA Bravery); Zhang et al., 2019 |
| **Initiative** | Takes proactive action without waiting for direction | Proactive personality loads λ ≈ .62 on Influence and predicts career success | Bateman & Crant, 1993; Seibert et al., 2001 |
| **Assertiveness** | Stands up for own needs, rights, or views directly and respectfully | Assertiveness in negotiation; advocacy effectiveness and boundary-setting | Speed et al., 2018; Ames & Flynn, 2007 |
| **Persuasion** | Persuades and shapes decisions or attitudes in others | Social influence and political skill; shaping attitudes via multiple routes | Cialdini & Goldstein, 2004; Ferris et al., PSI; Munyon et al., 2015 |

### Facet Selection Methodology

Each of the 36 facets was selected based on four criteria:

1. **Predictive utility:** Meta-analytic evidence that the construct predicts real-world outcomes
2. **Convergent validity:** Factor loadings λ > .40 on intended domain in EFA
3. **Discriminant validity:** Cross-loadings < .30 on non-target domains
4. **Conceptual clarity:** Non-redundancy and clear behavioral referents

### Inter-Facet Correlations

- Within domains: r = .30–.65 (related but distinct constructs)
- Across domains: r = .15–.50 (lower, demonstrating discriminant validity)

### Construct Boundaries (Deliberately Separated)

| Pair | Distinction | r |
|------|-------------|---|
| Curiosity (Insight) vs. Originality (Creativity) | Exploration vs. generation | .42 |
| Perspective (Insight) vs. Empathy (Connection) | Cognitive vs. affective perspective-taking | .38 |
| Achievement (Drive) vs. Leadership (Influence) | Personal standards vs. mobilizing others | .35 |
| Self-Discipline (Drive) vs. Self-Regulation (Stability) | Routine maintenance vs. impulse control | .51 |
| Flexibility (Creativity) vs. Adaptability (Stability) | Cognitive switching vs. behavioral adjustment | .44 |

Full psychometric justification, factor loadings, and validation evidence available in technical supplement.

---

## Appendix B: References (Selected)

Full bibliography available in technical documentation.

### Personality & Trait Theory

- Ashton, M. C., & Lee, K. (2007). Empirical, theoretical, and practical advantages of the HEXACO model of personality structure. *Personality and Social Psychology Review, 11*(2), 150-166.
- Costa, P. T., & McCrae, R. R. (2004). A contemplated revision of the NEO Five-Factor Inventory. *Personality and Individual Differences, 36*(3), 587-596.
- DeYoung, C. G. (2014). Openness/Intellect: A dimension of personality reflecting cognitive exploration. In M. L. Cooper & R. J. Larsen (Eds.), *APA Handbook of Personality and Social Psychology, Vol. 4: Personality Processes and Individual Differences* (pp. 369-399). American Psychological Association.

### Strengths Assessment

- Clifton, D. O., & Harter, J. K. (2003). *Investing in Strengths. In K. S. Cameron, J. E. Dutton, & R. E. Quinn (Eds.), Positive Organizational Scholarship* (pp. 111-121). Berrett-Koehler.
- McGrath, R. E. (2021). Technical issues with the VIA Assessment. *Journal of Positive Psychology, 16*(1), 121-131.
- Peterson, C., & Seligman, M. E. P. (2004). *Character Strengths and Virtues: A Handbook and Classification*. Oxford University Press.

### Psychometrics & Measurement

- Embretson, S. E., & Reise, S. P. (2000). *Item Response Theory for Psychologists*. Lawrence Erlbaum Associates.
- Flake, J. K., & Fried, E. I. (2020). Measurement schmeasurement: Questionable measurement practices and how to avoid them. *Advances in Methods and Practices in Psychological Science, 3*(4), 456-465.
- Ziegler, M., Booth, T., & Bensch, D. (2019). Getting entangled in the nomological net: Thoughts on validity and conceptual overlap. *European Journal of Psychological Assessment, 35*(3), 157-161.

### Creativity & Innovation

- Beaty, R. E., Kenett, Y. N., Christensen, A. P., Rosenberg, M. D., Benedek, M., Chen, Q., ... & Silvia, P. J. (2018). Robust prediction of individual creative ability from brain functional connectivity. *Proceedings of the National Academy of Sciences, 115*(5), 1087-1092.
- Benedek, M., Jauk, E., Sommer, M., Arendasy, M., & Neubauer, A. C. (2014). Intelligence, creativity, and cognitive control: The common and differential involvement of executive functions in intelligence and creativity. *Intelligence, 46*, 73-83.
- Said-Metwaly, S., Fernández-Castilla, B., Kyndt, E., & Van den Noortgate, W. (2022). Testing conditions and creative performance: Meta-analyses of the effects of time limits and instructions. *Psychology of Aesthetics, Creativity, and the Arts, 16*(1), 171-189.

### Motivation & Conscientiousness

- Bandura, A. (1997). *Self-Efficacy: The Exercise of Control*. W. H. Freeman.
- Duckworth, A. L., Peterson, C., Matthews, M. D., & Kelly, D. R. (2007). Grit: Perseverance and passion for long-term goals. *Journal of Personality and Social Psychology, 92*(6), 1087-1101.
- Moffitt, T. E., Arseneault, L., Belsky, D., Dickson, N., Hancox, R. J., Harrington, H., ... & Caspi, A. (2011). A gradient of childhood self-control predicts health, wealth, and public safety. *Proceedings of the National Academy of Sciences, 108*(7), 2693-2698.
- Payne, S. C., Youngcourt, S. S., & Beaubien, J. M. (2007). A meta-analytic examination of the goal orientation nomological net. *Journal of Applied Psychology, 92*(1), 128-150.
- Stajkovic, A. D., & Luthans, F. (1998). Self-efficacy and work-related performance: A meta-analysis. *Psychological Bulletin, 124*(2), 240-261.

### Stability & Resilience

- Bonanno, G. A. (2004). Loss, trauma, and human resilience: Have we underestimated the human capacity to thrive after extremely aversive events? *American Psychologist, 59*(1), 20-28.
- Chida, Y., & Steptoe, A. (2009). The association of anger and hostility with future coronary heart disease: A meta-analytic review of prospective evidence. *Journal of the American College of Cardiology, 53*(11), 936-946.
- Christian, M. S., Bradley, J. C., Wallace, J. C., & Burke, M. J. (2009). Workplace safety: A meta-analysis of the roles of person and situation factors. *Journal of Applied Psychology, 94*(5), 1103-1127.
- Tangney, J. P., Baumeister, R. F., & Boone, A. L. (2004). High self-control predicts good adjustment, less pathology, better grades, and interpersonal success. *Journal of Personality, 72*(2), 271-324.

### Connection & Relationships

- Emmons, R. A., & McCullough, M. E. (2003). Counting blessings versus burdens: An experimental investigation of gratitude and subjective well-being in daily life. *Journal of Personality and Social Psychology, 84*(2), 377-389.
- McCullough, M. E., Rachal, K. C., Sandage, S. J., Worthington, E. L., Brown, S. W., & Hight, T. L. (2000). Forgiveness, forbearance, and time: The temporal unfolding of transgression-related interpersonal motivations. *Journal of Personality and Social Psychology, 79*(1), 107-114.

### Leadership & Influence

- Bass, B. M. (1985). *Leadership and Performance Beyond Expectations*. Free Press.
- Bateman, T. S., & Crant, J. M. (1993). The proactive component of organizational behavior: A measure and correlates. *Journal of Organizational Behavior, 14*(2), 103-118.
- Cialdini, R. B., & Goldstein, N. J. (2004). Social influence: Compliance and conformity. *Annual Review of Psychology, 55*, 591-621.
- Judge, T. A., Bono, J. E., Ilies, R., & Gerhardt, M. W. (2002). Personality and leadership: A qualitative and quantitative review. *Journal of Applied Psychology, 87*(4), 765-780.
- Weber, E. U., Blais, A.-R., & Betz, N. E. (2002). A domain-specific risk-attitude scale: Measuring risk perceptions and risk behaviors. *Journal of Behavioral Decision Making, 15*(4), 263-290.

### AI & NLP (Selected)

- Natural Language Inference models, semantic similarity research, and GPT-based item generation methods referenced throughout technical sections. Full bibliography available in technical supplement.

---

## Appendix F: Budget & Funding Plan

*Available on request to qualified partners and reviewers.*

---

## Document Information

**Prepared by:** The OpenStrengths Working Group

**Version:** 3.0

**Date:** February 2026

**Purpose:** This technical reference provides the complete methodological specification for the OpenStrengths framework, including psychometric architecture, AI-powered item generation pipeline, validation protocol, and technical infrastructure. For a general introduction to the project, see the OpenStrengths manifesto.

**Updates:** This is a living document. As development progresses and validation results emerge, updated versions will be released with transparent changelog.

**V3.0 Changelog (from V2.0):**
- Facet renames: Creativity → Originality, Influence → Persuasion, Tolerance → Patience, Motivation → Purpose
- DRIVE composition: Self-Confidence moved from INFLUENCE; Vitality removed
- INFLUENCE composition: Boldness added; Self-Confidence moved to DRIVE
- Domain taglines updated for CREATIVITY, DRIVE, STABILITY, CONNECTION, INFLUENCE
- Structural reframe: domains described as "organized around" Big Five aspects rather than "mapping to" them
- Added "organized around" language and multi-disciplinary sourcing description to framework introduction
- Updated all domain descriptions, facet tables, and construct boundary references
- Added new references: Bandura (1997), Stajkovic & Luthans (1998), Said-Metwaly et al. (2022), Chida & Steptoe (2009), Weber et al. (2002), Zhang et al. (2019)

**License:** Released under Creative Commons Attribution 4.0 International (CC BY 4.0). Free to share and adapt with attribution.

---

**Contact:** team@openstrengths.org | openstrengths.org | github.com/open-strengths
