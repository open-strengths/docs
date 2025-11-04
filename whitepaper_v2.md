# OpenStrengths  
### *An Open‑Source Framework for Mapping Human Potential*  
**White Paper · Version 2.0 (November 2025)**


---

## Front Matter

**OpenStrengths in a sentence**

An open, research‑grounded strengths system **in development**, with a planned free core and short, adaptive assessment, designed for trust and portability: results will travel through APIs (for technical integration) and a user‑facing Integration Marketplace (for direct use) so both developers and end-users can apply strengths in jobs, tasks, HR, education, and services.


**Vision**

Make strengths science open, trusted, and portable so anyone can discover, develop, and apply their strengths across learning, work, and relationships.


**Mission**

Design and validate a transparent, research-grounded strengths system that: (1) balances depth with speed through adaptive assessment technology, (2) openly shows you how it was built and why it works, and (3) makes your strengths profile truly yours so you can apply it wherever it matters through APIs and our Integration Marketplace.


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

An **open, AI-enhanced assessment platform** providing portable results (APIs + Integration Marketplace), safe AI use (facet-seeded generation with psychometric safeguards), personalization (trauma-informed, reading-level-appropriate items), and transparent methods (all algorithms and validation data publicly documented).

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
| **CliftonStrengths® 34** | Corporate coaching | 34 themes in 4 buckets | Large normative dataset; engaging language; robust coaching ecosystem | Proprietary content & scoring; trademarked theme names; limited independent peer-reviewed validation; contested factor structure (vendor tech reports vs. external replications). |
| **VIA Character Strengths** | Education & well-being | 24 character traits | Public items; positive-psychology heritage; intervention-friendly; many translations | High inter-item redundancy; some traits conflate virtue labels with behaviors; scoring algorithm only partially documented; factor stability varies across samples [McGrath 2021]. |
| **MBTI®** | Career counseling | 16 types from 4 dichotomies | Easy narrative appeal; ubiquitous in career workshops | Poor test-retest reliability; dichotomies unsupported by modern trait theory [Costa & McCrae 2004]. |
| **DISC** | Team workshops | Dominance, Influence, Steadiness, Conscientiousness | Simple quadrant model; widely used training archetype | Low predictive validity; vendor-specific scoring; scarce peer-reviewed evidence [Hughes 2019]. |
| **Big Five (IPIP‑NEO)** | Academic gold standard | O, C, E, A, N (30 facets) | Open items; robust validity | 30 facets can overwhelm lay users; no explicit focus on “strengths” framing. |
| **HEXACO** | Research & risk / ethics studies | Adds Honesty-Humility factor | Improved coverage of ethical behavior and counterproductive tendencies; cross-cultural evidence accumulating | Lower adoption in applied strengths settings; some translations proprietary; added complexity vs. Big Five; mapping to strengths framing is indirect. |

 *Layman’s takeaway:* Most popular models are either **proprietary and hard to audit** or **descriptive rather than strengths-based**. We’re aiming for an **open, strengths-first** approach that is **short**, **transparent**, and **validated**, and that maps results to **clear, actionable behaviors** (including creativity and safety/stability) instead of just labels.

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

| Domain        | Plain-Language Role                 | Empirical Factor (approx. mapping)                                 | Key Sources (examples)                          |
|---------------|-------------------------------------|---------------------------------------------------------------------|-------------------------------------------------|
| **Insight**   | Strategic analysis & sense-making   | Openness–Ideas / need-for-cognition; sense-making/system thinking  | Weick (1995); Klein (1998); DeYoung (2014)      |
| **Creativity**| Divergent ideation & synthesis      | Creative fluency/originality; associative/generative cognition      | Benedek et al. (2019); Beaty et al. (2021)      |
| **Drive**     | Goal pursuit & execution            | Industriousness / achievement striving; grit                        | Payne et al. (2007); Duckworth et al. (2007)    |
| **Stability** | Reliability & safety vigilance      | Conscientiousness (order/self-control); emotion regulation          | Christian et al. (2009); Baumeister et al. (2007)|
| **Connection**| Empathy & affiliation               | Agreeableness–compassion/prosociality; psychological safety context | Davis (1983); Edmondson (1999/2019)             |
| **Influence** | Persuasion & mobilization           | Extraversion–assertiveness/activation; leadership emergence          | Judge et al. (2002); Cialdini (2001); Bass (1985)|

> **Why six?** A four‑factor model merges Creativity into Insight and Stability into Drive—reducing predictive validity for innovation and high‑risk jobs by 7–12 %. A seven‑factor model (adding Honesty‑Humility) adds < 2 % variance and cross‑loads heavily [8].

---

### 4.2  Facet-by-Facet Justification

#### Design Principle: Inner Orientation → Outer Response

When we curated the 36 facets, we aimed for each domain to include facets that reflect an **inner orientation** (how a person tends to think/feel) and complementary **outer responses** (what they tend to do in the world). This is a communication aid, not a modeling constraint: we do **not** force these pairs in factor analysis or scoring. It simply explains the everyday logic behind the facet set while helping prevent cross-loading in our definitions.

**Examples by domain (non-exhaustive):**

- **Insight**
  - *Inner:* Self-Reflection (metacognitive awareness), Curiosity (exploration drive)
  - *Outer:* Perspective (applied sense-making), Foresight (planning/anticipation)

- **Creativity**
  - *Inner:* Imagination (mental simulation), Flexibility (cognitive set-shifting)
  - *Outer:* Innovation (implementation/adoption), Expression (externalizing ideas)

- **Drive**
  - *Inner:* Motivation (goal-directed energy), Self-Discipline (self-control/routines)
  - *Outer:* Achievement (pursuit of high standards), Perseverance (sustained effort)

- **Stability**
  - *Inner:* Composure (calm under stress), Self-Regulation (impulse/emotion control)
  - *Outer:* Resilience (bounce-back/growth), Adaptability (behavioral adjustment)

- **Connection**
  - *Inner:* Empathy (understanding/feeling with others), Gratitude (felt appreciation)
  - *Outer:* Kindness/Caring (prosocial acts, support), Justice (fairness in action)

- **Influence**
  - *Inner:* Self-Confidence (self-efficacy/approach), Initiative (self-starting intent)
  - *Outer:* Assertiveness (respectful advocacy), Leadership/Communication (mobilizing others)

> **Why include this?** It clarifies why two facets that seem “close” aren’t redundant: e.g., *Imagination* (inner ideation) vs *Innovation* (outer adoption), or *Composure* (calm during) vs *Resilience* (recovery after). We keep this as narrative guidance only; empirical distinctiveness is still judged by loadings, fit, and criterion prediction.


We retain the facet **definitions** exactly as written. “**Why this facet?**” explains (a) how the facet is **distinct** from its nearest neighbors (to reduce cross-loading), and (b) the **behavioral/criterion signals** we expect it to predict in applied settings.

| Domain | Facet | Definition | Why This Facet? | Representative Studies |
|--------|-------|------------|-----------------|------------------------|
| **Insight** | Curiosity | Actively seeks out new knowledge and experiences. | Distinct from **Learning** (acquisition speed) by emphasizing **exploration drive** (question asking, information seeking). Predicts exploratory behavior, job crafting, and idea input that fuels Creativity. | Kashdan & Silvia (2009); Zhang et al. (2020) |
|  | Perspective | Understands situations from a broad or different point of view. | Distinct from **Empathy** (affective understanding) and **Wisdom** (value-balanced judgment). Captures **sense-making** across frames, reducing blind spots in complex decisions. | Weick (1995); Klein (1998/2008) |
|  | Learning | Quickly acquires and applies new skills or knowledge. | Distinct from **Curiosity** (explore) by focusing on **acquisition/transfer** (ramp time, skill generalization). Predicts training ROI and time-to-competence. | Sitzmann & Ely (2011); Keith et al. (2010) |
|  | Foresight | Anticipates future needs and likely outcomes. | Distinct from **Perspective** (current framing) by emphasizing **anticipation and temporal projection**. Predicts proactive planning, risk mitigation, and reduced rework. | Zimbardo & Boyd (1999); Steel & König (2006) |
|  | Wisdom | Balances knowledge, judgment, and values in decisions. | Distinct from **Foresight** (prediction) by emphasizing **value-aligned trade-offs** and judgment under ambiguity. Predicts ethical, high-stakes choices. | Grossmann (2017); DeYoung (2014) |
|  | Self-Reflection | Examines one's own thoughts, feelings, and behavior. | Distinct from **Learning** (external skill gain) via **metacognitive self-monitoring** (error noticing, after-action review). Predicts adaptive performance and growth. | Sitzmann (2015); Grant et al. (2017) |
| **Creativity** | Creativity | Generates novel and imaginative ideas. | Distinct from **Insight** (analysis) by targeting **novel idea generation** (fluency/originality). Predicts idea count/quality, patents/proposals. | Benedek et al. (2019); Beaty et al. (2021) |
|  | Innovation | Transforms ideas into practical, adopted solutions. | Distinct from **Influence** (persuasion) by focusing on **implementation/adoption** (from pilot to routine use). Predicts process change and diffusion. | Rogers (2003); Crossan & Apaydin (2010) |
|  | Resourcefulness | Finding effective ways to solve problems with what is available. | Distinct from **Innovation** (adoption) by targeting **constraint-based improvisation** and workaround design. Predicts continuity under constraints. | Vera & Crossan (2005); Stokes (2014) |
|  | Imagination | Envisions possibilities beyond present reality. | Distinct from **Creativity** (output) by emphasizing **mental simulation** and envisioning alternatives. Predicts concepting, prototyping, and strategy vision. | Feist (2019); Abraham (2016) |
|  | Flexibility | Adjusts thinking or approach when circumstances change. | Distinct from **Adaptability** (behavioral change) by focusing on **cognitive shifting/set-switching**. Predicts pivot speed and reduced switch costs under stress. | Monsell (2003); Martin & Rubin (1995) |
|  | Expression | Communicates inner ideas or emotions through words, art, or action. | Distinct from **Communication** (audience-fit clarity) by focusing on **creative expression** and externalization. Predicts creative output/portfolio and well-being. | Kaufman & Beghetto (2009); Forgeard (2013) |
| **Drive** | Achievement | Strives toward high standards and significant results. | Distinct from **Motivation** (energy) by emphasizing **standards and goal level**. Predicts stretch goals, quality thresholds, and performance awards. | Payne et al. (2007); Judge & Ilies (2002) |
|  | Motivation | Inner drive that energizes action toward goals. | Distinct from **Vitality** (stamina) by capturing **goal-directed energy** and initiation of effort. Predicts discretionary effort and task initiation. | Kanfer (1990); Grant (2008) |
|  | Self-Discipline | Maintains consistent routines and self-control. | Distinct from **Focus** (momentary attention) by emphasizing **habit/routine adherence** and delay of gratification. Predicts completion and rule adherence. | Moffitt et al. (2011); Duckworth & Gross (2014) |
|  | Perseverance | Keeps going despite obstacles, setbacks, or fatigue. | Distinct from **Resilience** (recovery) and **Self-Discipline** (routines) by targeting **sustained effort under adversity**. Predicts completion rates and retention. | Duckworth et al. (2007/2016) |
|  | Vitality | Approaches tasks with energy, enthusiasm, and stamina. | Distinct from **Motivation** (wanting) by emphasizing **energetic arousal/stamina**. Predicts engagement and sustained output under load. | Schaufeli et al. (2006); Sonnentag (2003) |
|  | Focus | Sustains attention on priorities without distraction. | Distinct from **Self-Discipline** (routine) by targeting **moment-to-moment attentional control**. Predicts error rate, deep-work time, and task switch discipline. | Unsworth & Robison (2017); Gazzaley & Rosen (2016) |
| **Stability** | Composure | Remaining calm and steady under stress or pressure. | Distinct from **Resilience** (post-event bounce-back) by focusing on **during-event calm**. Predicts incident rates and safety-critical performance. | Christian et al. (2009); Schouwenburg (2020) |
|  | Tolerance | Accepts differences and endures discomfort without hostility. | Distinct from **Justice** (fairness action) by emphasizing **non-hostility and forbearance**. Predicts lower conflict and inclusion climate. | Ashton & Lee (2007); Bohannon (2018) |
|  | Adaptability | Adjusts behavior to function effectively in new conditions. | Distinct from **Flexibility** (cognitive) by emphasizing **behavioral adjustment** (strategy change, role shift). Predicts transition success. | Pulakos et al. (2000); J Appl Psych (2023) |
|  | Optimism | Expects favorable outcomes and looks for opportunities. | Distinct from **Self-Confidence** (self-efficacy) by focusing on **general positive expectancy**. Predicts persistence and stress buffering. | Scheier & Carver (1985/1993); Carver & Scheier (2014) |
|  | Resilience | Recovers quickly and grows from stress or loss. | Distinct from **Perseverance** (keep going during) and **Composure** (calm during). Predicts **time-to-recovery** and return-to-baseline. | Bonanno (2004); Hobfoll et al. (2018) |
|  | Self-Regulation | Controls impulses and emotions to stay aligned with values. | Distinct from **Self-Discipline** (routine adherence) by emphasizing **in-the-moment control** of impulses/emotions. Predicts safety compliance and fewer counterproductive acts. | Baumeister et al. (2007); Tangney et al. (2004) |
| **Connection** | Kindness | Shows warmth through small, everyday prosocial acts. | Distinct from **Caring** (sustained support) by focusing on **everyday prosocial micro-behaviors**. Predicts helping frequency and team warmth. | Grant & Gino (2010); Allen (2004) |
|  | Empathy | Understands and shares the feelings of others. | Distinct from **Perspective** (cognitive frame) by targeting **affective/cognitive understanding of people**. Predicts team climate and customer ratings. | Davis (1983); Decety & Jackson (2004) |
|  | Forgiveness | Lets go of resentment and offers second chances. | Distinct from **Justice** (fair process) by focusing on **relationship repair** and forbearance. Predicts cooperation and retention after conflict. | McCullough et al. (1997/2000) |
|  | Caring | Provides sustained support and concern for others' well-being. | Distinct from **Kindness** (episodic) by emphasizing **enduring, resource-backed support**. Predicts mentoring time and support provision. | Allen (2004); Dutton et al. (2014) |
|  | Gratitude | Appreciates and acknowledges what others contribute. | Distinct from **Kindness** (giving) by focusing on **recognition/acknowledgment** that increases reciprocity and cohesion. Predicts prosocial reciprocity. | Emmons & McCullough (2003); Algoe (2012) |
|  | Justice | Acts to ensure fairness and equal treatment in groups. | Distinct from **Tolerance** (forbearance) by emphasizing **fairness norms and voice**. Predicts psychological safety and inclusion. | Colquitt (2001); Edmondson (1999/2019) |
| **Influence** | Leadership | Guides and motivates others toward shared goals. | Distinct from **Influence** (individual persuasion) and **Assertiveness** (self-advocacy) by focusing on **direction-setting and motivation of groups**. Predicts team outcomes and climate. | Bass (1985); Judge & Piccolo (2004) |
|  | Communication | Expresses ideas clearly and appropriately. | Distinct from **Expression** (creative externalization) by emphasizing **audience-fit clarity and structure**. Predicts comprehension and adoption decisions. | Green & Brock (2000); Heath & Heath (2007) |
|  | Self-Confidence | Believes in one's ability to succeed. | Distinct from **Optimism** (general expectancy) by targeting **self-efficacy and approach behavior**. Predicts stretch goals and initiative taking. | Judge et al. (2002); Bandura (1997) |
|  | Initiative | Takes proactive action without waiting for direction. | Distinct from **Perseverance** (sustaining effort) by emphasizing **self-starting and ownership**. Predicts improvement suggestions and issue ownership. | Bateman & Crant (1993); Parker et al. (2006) |
|  | Assertiveness | Stands up for one's own needs, rights, or views directly and respectfully. | Distinct from **Leadership** (group direction) by focusing on **self-advocacy and boundary setting**. Predicts negotiation outcomes and voice. | Ames & Flynn (2007); Curhan et al. (2014/2022) |
|  | Influence | Persuades and shapes decisions or attitudes. | Distinct from **Leadership** (guidance) and **Communication** (clarity) by targeting **conversion/mobilization**. Predicts sales/win rate and adoption. | Petty & Cacioppo (1986/2019); Cialdini (2001) |

---

## 5 · OpenStrengths Platform — how we deliver safely

**Two-minute summary.** Section 4 defined *what* we measure (6 domains, 36 strengths). Section 5 shows *how* we deliver it safely from day one and keep improving. Think of two spaces:
- **The kitchen:** where questions are created and checked.
- **The dining room:** where assessments are served.

We run a short **cold start** to create a **reliable first menu** (our initial calibrated set). After that, new AI-generated questions enter gradually—**small trial → behind-the-scenes check → counts a little → counts fully**. Most scoring always comes from the calibrated set, and any new question can be **paused instantly** if a safety signal fires. Independent expert **opinion letters (L0–L3)** and **decision gates** keep us honest—without requiring a full re-pilot every time we add a question.

**Quick definitions.**
- **Strength (facet):** one specific quality we measure (e.g., Perseverance).  
- **Question (item/stem):** one short statement you respond to.  
- **Draft question:** a new, not-yet-tested question.  
- **Operational question:** a tested, calibrated question we use for scoring.  
- **Reference questions (FRIs):** a small, stable subset of operational questions we keep showing to keep scores consistent over time.  
- **Classification profile:** a simple profile from the pre-assessment (e.g., reading level, work/school/daily-life framing, trauma sensitivity) used only to adjust wording—not scoring.  
- **Adaptive test:** the computer chooses your next question based on your previous answers (fewer, smarter questions).  
- **Behind-the-scenes check:** we calculate what your score *would* be with a new question but don’t show or use it yet.

---

### 5.0 Cold start — from zero to a first reliable bank

**Analogy:** Opening night. Before daily specials, you set a **core menu** everyone can trust.

**Step 0 · Seed & screen (in the kitchen).**  
We generate 12–20 draft questions per strength from the strength’s description (we do **not** copy legacy tests). Automatic checks confirm meaning, reading level, and sensitive-language safety. *(The rules for these checks are pre-approved by experts; no human signs off on individual questions.)*

**Step 1 · First trial run (short versions, no official score yet).**  
- **What users see:** a normal-looking strengths assessment, just shorter, with a banner: *“Calibration mode — preview only.”* Different people see different mixes so we cover the whole pool.  
- **What counts:** **nothing yet**. New questions are tried but do **not** affect anyone’s official score.  
- **What the system does:** gathers enough responses to judge clarity, fit with the intended strength, and **fairness across groups and across classification profiles** (easier wording vs. standard, work vs. school framing, etc.). When targets are met, the platform runs a **promotion cycle**.

**Promotion cycle — moving from trial to bank.**  
1) **Basic hygiene:** clear wording, sensible use of answer choices, low “this is confusing/inappropriate” reports.  
2) **Right thing measured:** the question moves with the **intended strength**, not some other idea.  
3) **Fair for everyone—and across profiles:** two people with the **same underlying strength** should get the **same score**, even if one saw simpler wording or a different everyday example. Personalization is like a ramp instead of stairs—better access, not bonus points.  
4) **Coverage:** a small **varied set** per strength so we’re not asking the same thing three ways.  
5) **Pick the keepers:** select **4–8 questions** per strength, mark them **Operational**, and tag a few as **FRIs** to keep the scale steady over time.  
6) **Version & lock:** save as **the first bank** with a version number and changelog. Official scoring starts here.

> **Exit rule:** Move to Step 2 when **each strength** has **enough good questions** (usually 4–8). If a strength falls short, the system auto-rewrites a few questions and runs another tiny trial.

**Step 2 · Promote the first bank (create the first stable core).**  
The selected set becomes the **Operational** bank; FRIs are designated for ongoing stability.

**Step 3 · Turn on adaptive testing carefully.**  
The adaptive engine first runs **behind the scenes** to confirm it matches the short fixed forms, then goes live.

*Technical details (for experts):* GRM via planned-missing short forms; no parameter inheritance. Promotion requires acceptable fit (e.g., S-X² or equivalent), information coverage (target SEM ≤ .32 in −1 to +1 θ), LD/DIF within thresholds, and designated FRIs. Modest hierarchical priors may stabilize early a/b; full provenance retained.

---

### 5.1 Item development — the kitchen

New questions are drafted from each strength’s description, checked for meaning and clarity, tried in tiny doses, and only then added to the everyday set.

- **Draft →** AI writes short questions from the strength description—never by copying old tests.  
- **Automatic checks →** meaning match, readability, sensitive-language safety, de-duplication.  
- **Small trials →** a few people see the question; it **doesn’t** affect scores yet.  
- **Promote or pause →** the platform lets it **count a little**, then **count fully**—or **auto-pauses** it.

**Who approves questions?**  
**Experts approve the rules; the system approves the questions.** An independent psychometrician signs an opinion letter on our Validation Blueprint (what to check, how to check it, and the thresholds). After that, the platform admits, trials, promotes, or pauses questions **automatically**, with humans only on exception. Every decision is logged for audit.

*Technical details (for experts):* Facet-seeded prompts; NLI alignment to spec; cosine diversity thresholds. Bank states: **Draft → Piloted → Operational → (FRI) → Retired** with audit trails. Governance: exposure caps, content balance, per-strength coverage; demotion/retire rules on drift/misfit; end-to-end provenance.

---

### 5.2 Assessment delivery — the dining room

1) **Pre-assessment (13 questions):** sets reading level, chooses simple examples (work/school/daily life), avoids insensitive wording.  
2) **Adaptive assessment:** **once the first calibrated bank exists,** the computer picks your next question so you answer **fewer, smarter questions**. Only **tested, calibrated** questions affect your score.  
3) **Results & export:** you get a clear profile; developers can export via API (once available).

*Technical details (for experts):* CAT with GRM, content balance, exposure control; initially shadowed against fixed forms. Personalization guardrails: wording can change (reading level, light scenario), **construct meaning and scoring keys do not**. No per-user generation at runtime; delivery uses banked items.

---

### 5.2.1 Fair personalization — what changes vs. what never changes

We adjust wording so questions are easier to read and feel relevant, but we **never** change what the question means or how it's scored.

| Personalization knobs (allowed) | What never changes (locked) |
|---|---|
| Reading level (simpler words) | The underlying strength being measured |
| Light scenario framing (work / school / daily life) | The scoring key and the scale |
| Trauma-sensitive phrasing (avoid triggering language) | Difficulty and discrimination targets we validate |
| Contextual examples ("class project" vs "work project") | How results are interpreted and compared |

**Examples in practice**

Below are two strengths showing how wording adapts while the **construct and scoring stay identical**. Every variant scores the same way; personalization only changes accessibility and relevance.

**Innovation (Creativity domain)**
*Invariant intent: tests new approaches and adopts what works.*

- **Base (B2):** I try new ways to solve problems and keep what works. [score↑ = agree]
- **Simplified (A2/B1):** I test new ways and keep the ones that help. [same scoring]
- **Work framing:** On the job, I pilot new methods and adopt the ones that improve results. [same scoring]
- **School framing:** In class projects, I try new approaches and keep the best one. [same scoring]
- **Daily-life framing:** At home, I experiment with better ways to do chores and stick with what works. [same scoring]
- **Trauma-sensitive:** I make small, low-risk trials before changing to a new method. [same scoring]
- **Reverse-coded:** I use the same approach even when it isn't working well. [score↑ = disagree]

**Empathy (Connection domain)**
*Invariant intent: understands others' feelings before responding.*

- **Base (B2):** I try to understand how others feel before I respond. [score↑ = agree]
- **Simplified (A2/B1):** I try to see how someone feels before I answer. [same scoring]
- **Work framing:** With teammates or customers, I check how they're feeling before I reply. [same scoring]
- **School framing:** With classmates, I think about their feelings before I speak. [same scoring]
- **Daily-life framing:** With friends or family, I try to understand their feelings first. [same scoring]
- **Trauma-sensitive:** When someone shares big feelings, I listen first and keep my tone gentle. [same scoring]
- **Reverse-coded:** I respond quickly without thinking about how the other person feels. [score↑ = disagree]

> **The key principle:** Different people see different words, but two people with the **same Innovation or Empathy** get the **same score**—whether they saw simple wording, work examples, or trauma-sensitive phrasing. Personalization is about access, not advantage.

**How we check.**
- **Swap tests:** a small group sees the "out-of-profile" wording to confirm scores don't shift.
- **FRIs for linkage:** everyone sees a few reference questions regularly; they act like a ruler so measurements stay the same over time.

*Technical details (for experts):* Variants grouped into **families** and equated via FRI anchors. Multiple-group GRM/CFA for configural/metric thresholds (ΔCFI/ΔRMSEA within tolerance). DIF checks (IRT-LR, MH-DIF) across protected groups **and** classification strata. Low-rate counterfactual sampling to estimate include/exclude θ; require median |Δθ| ≤ .05. Classification never enters the scoring model; it only routes presentation.

---

### 5.3 Why AI-generated questions

**Analogy:** Seasonal dishes keep the menu fresh and locally relevant—but only after the kitchen and a few tasters approve.

**What AI helps with**
- **Fit the reader:** match reading level and everyday context.  
- **Be sensitive:** avoid wording that could re-trigger or exclude.  
- **Stay relevant:** keep examples fresh without changing what’s measured.

**Guardrails at a glance**
- Only **calibrated** questions affect scores.  
- Generation lives in the **kitchen**, not per user at runtime.  
- **Instant freeze** if any safety signal fires.  
- FRIs anchor the scale and check drift.  
- Personalization changes **words**, not **scores** (variants must pass invariance/DIF checks).

*Technical details (for experts):* NLI alignment to the facet spec (not legacy items), cosine diversity, DIF checks, drift monitoring, thresholds per the Validation Blueprint (Appendix **B**). All AI features are feature-flagged.

---

### 5.3.1 Rolling out new questions — from trial to full use

**Analogy:** A rookie joins the team: practice → scrimmage → limited minutes → starter, only if they play well.

1) **Small trial** — shows up rarely; **never** affects results.  
2) **Behind-the-scenes check** — we compute scores with/without the question but **don’t use** them yet.  
3) **Counts a little** — begins to influence results a small amount while we keep watching.  
4) **Counts fully** — joins the everyday set; any problem later auto-pauses it.

*Technical details (for experts):* Promotion requires minimum unique N across strata (e.g., ≥300), acceptable GRM fit, SE(a) ≤ .20, TCC RMSD within bounds vs FRI-linked scale, DIF within thresholds, and median \|Δθ\| ≤ .05 include/exclude. Auto-pause: complaint rate, readability fails, misfit, DIF, drift.

---

### 5.4 Status & roadmap

**Status & Roadmap Tracker (as of Nov 2025)**  
*Gates L0–L3 are independent expert opinion letters at phase transitions.*

| Component | Status | Current Phase | Next Gate | Target Window |
|---|---|---|---|---|
| Pre-assessment & classification | ✅ Live | Phase 1 complete | — | Delivered |
| AI drafting (from strength descriptions) | ✅ Live | Phase 1 complete | — | Delivered |
| Meaning & diversity checks | ✅ Live | Phase 1 complete | — | Delivered |
| Audit trails & governance | ✅ Live | Phase 1 complete | — | Delivered |
| Final selection & ranking for new questions | 🔄 In progress | Phase 1 → 2 | **L2** (Pilot results) | Q1 2026 |
| Cold-start plan & first calibrated bank | 🔄 In progress | Phase 2 | **L2** (Pilot results) | Q1 2026 |
| Adaptive test (shadow mode) | 🔄 In progress | Phase 1 → 2 | **L1** (Build), then **L2** | Q4 2025–Q1 2026 |
| Public API & integration marketplace | ⏸️ Not started | Phases 4–5 | After **L3** (Validation) | Q2–Q4 2026 |

> **Legend:** **L0** Protocol readiness · **L1** Build conformance · **L2** Pilot results · **L3** Validation

**Phases & gates (compact timeline)**

| Phase | Window | Gate to next |
|---|---|---|
| **0** Protocol design & readiness | **Q4 2025** | **L0** issued |
| **1** Build & conformance | **Q4 2025–Q1 2026** | **L1** issued + shadow checks green |
| **2** Pilot & first bank | **Q1 2026** | Reliability/validity met + **L2** issued |
| **3** Validation & transparency | **Q2 2026** | Publication/peer confirm + **L3** issued |
| **4** Public beta & API | **Q2–Q3 2026** | Stability/fairness steady |
| **5** Integration marketplace | **Q4 2026+** | Ongoing audits & reports |

**Timeline:** P0 (Q4’25) → P1 (Q4’25–Q1’26) → P2 (Q1’26) → P3 (Q2’26) → P4 (Q2’26–Q3’26) → P5 (Q4’26+)  
**Gates:** L0 → L1 → L2 → L3



---

## 6 · Validation Priorities, Open Questions, and Study Design

This section prioritizes validation of our **novel approach** (facet-seeded generation, semantic governance, and replaceable Facet Reference Items) before routine measurement details. Each subsection includes: hypotheses, evidence we’ll gather, acceptance criteria, and any remaining expert questions. Priorities: **P1 (critical), P2 (important), P3 (nice-to-have)**.

---

### 6.0 Facet-First Generation & Semantic Governance (P1)

**Hypotheses**
- H1: Stems generated from the **facet description** (not legacy items) show strong semantic alignment and low cross-loading.
- H2: Our semantic gate (embeddings + NLI) aligns with SME judgments and improves downstream psychometrics (fit, information, DIF).

**Evidence to Gather**
- SME rating study (3–5 experts/facet): relevance, clarity, single-idea, and non-overlap with adjacent facets.
- Correlate SME scores with **alignment** (cosine to target), **purity** (cos target − max cos non-target), and NLI outcomes.
- Ablation: regress item fit/information on semantic metrics to confirm predictive value.

**Acceptance Criteria (initial)**
- SME interrater agreement: κ ≥ .60 (or ICC ≥ .70).
- Correlation of SME relevance with alignment ≥ .50; with purity ≥ .40.
- NLI: high entailment to target facet; neutral/contradiction to non-targets; ≤ 5% false-positives on non-targets.
- Gate thresholds (initial, tunable): alignment ≥ 0.35; purity ≥ 0.08; near-duplicate cosine < 0.92; reading level ≈ grades 7–10; no double-barrels.

**Expert Questions**
- Q1: Are our thresholds reasonable across all 36 facets, or should some be facet-specific?
- Q2: Preferred protocol for periodic gate re-tuning (change-point or drift tests)?

---

### 6.1 Facet Reference Items (FRIs) for Linking & Drift (P1)

**Hypotheses**
- H3: A small per-facet set of **FRIs** (2–4 items) enables stable score comparability across forms/waves without fixing item params forever.
- H4: Drift monitoring with replaceable FRIs preserves comparability while permitting content refresh.

**Evidence to Gather**
- Include FRIs on every calibration wave; run concurrent calibration or Stocking–Lord linking.
- Track **Δa**, **Δb** per FRI; monitor linking error (θ mean/SD shifts, TCC RMSD).

**Acceptance Criteria**
- Linking stability across waves: |Δμ_θ| ≤ 0.05; |Δσ_θ| ≤ 0.05; TCC RMSD ≤ 0.05.
- Drift thresholds to demote/replace an FRI: |Δa| > 0.25 **or** any |Δb_k| > 0.30 **with** fit degradation (flag).
- FRI coverage: each facet’s FRIs collectively informative on θ ≈ [−1.5, +1.5].

**Expert Questions**
- Q3: Is 2–4 FRIs/facet adequate under our content-balance rules? Target exposure/rotation recommendations?

---

### 6.2 Anchorless Calibration & Planned-Missing Design (P1)

**Hypotheses**
- H5: **Anchorless concurrent GRM** with θ identified as N(0,1) yields stable parameters when paired with planned-missing (matrix) forms.
- H6: Our promotion/retirement rules produce a bank that meets precision targets with short tests.

**Design**
- Forms: 6 booklets × ~30 items each (content-balanced); each person answers ~30 items.
- **Standard sample targets** (initial): **400–600 responses per item** (raise for sparse categories); total respondents for a 250–300 item pilot ≈ **4k–6k**.
- Identification: θ ~ N(0,1) (no fixed item parameters).

**Acceptance Criteria**
- Item parameter precision: all threshold SEs < 0.15; SE(a) reasonable (e.g., < 0.15–0.20).
- Item fit: acceptable S-X² (or equivalent), residual diagnostics acceptable; local dependence (Yen’s Q3) median ≤ .10, max ≤ .20.
- Information coverage per facet: high information on θ ∈ [−1.5, +1.5] (or facet-specific).
- Promotion rule: promote to **Operational** when precision + fit + coverage + no LD/DIF.
- Retirement rule: misfit, LD, drift, or bias flags.

**Expert Questions**
- Q4: Any facet-specific tweaks to form design to mitigate local dependence?
- Q5: Recommended fit indices and thresholds for polytomous items in our use case?

---

### 6.3 CAT Safety, Efficiency, and Content Balance (P1)

**Hypotheses**
- H7: A content-balanced CAT with exposure control achieves domain-level precision with short tests, without overexposing items.

**Evidence to Gather**
- Monte Carlo CAT simulations with our calibrated bank and constraints; shadow testing in early production.

**Acceptance Criteria**
- Average test length (per domain target): ≤ 8–12 items while achieving conditional SEM ≤ 0.35 on θ ∈ [−1.5, +1.5].
- Exposure control (e.g., Sympson–Hetter or KL-based): max exposure ≤ 0.20 (tunable) with stable score precision.
- Content balance: enforced per facet/domain; no facet starved of information.

**Expert Questions**
- Q6: Preferred exposure-control method and parameters in personality/strengths CAT contexts?

---

### 6.4 Fairness, Social Desirability, and Validity Checks (P1)

**Hypotheses**
- H8: Social-desirability influence is bounded; DIF across key groups is negligible or correctable.

**Design & Acceptance Criteria**
- Include a brief SD scale during pilots; aim for |r(facet, SD)| ≤ .30. If exceeded, evaluate wording/keys or model adjustments.
- DIF screens (e.g., IRT-LR, MH): group Ns ≥ 400–500; flag if |Δb| > 0.30 with fit change or ΔMH ≥ 1.0 (context-dependent).
- Attention checks + response-time flags to ensure data quality.

**Expert Questions**
- Q7: Recommended DIF method(s) for polytomous items with multiple covariates?

---

### 6.5 Construct Validity: Convergent, Discriminant, Known-Groups (P2)

**Evidence to Gather**
- Convergent correlations with related constructs (expect r ≈ .30–.60).
- Discriminant correlations with unrelated constructs (expect |r| ≤ .30).
- Known-groups contrasts with preregistered hypotheses (expect d ≥ 0.5 where theory predicts).

**Acceptance Criteria**
- Pattern of correlations matches theory across domains/facets.
- Known-groups results in predicted direction and magnitude.

---

### 6.6 Reliability & Precision Targets (P2)

**Acceptance Criteria**
- Marginal reliability (EAP) per **domain** ≥ .85; per **facet** ≥ .75 (initial).
- Test–retest (≈ 2-week interval): ICC(2,1) domains ≥ .80; facets ≥ .70.
- Score SEM curves published for θ ∈ [−2.0, +2.0].

---

### 6.7 Documentation, Transparency, and Governance (P3)

**Requirements**
- Pre-register gate thresholds, promotion/retirement rules, and FRI drift thresholds.
- Publish calibration run sheets: N per item, SEs, fit, linking diagnostics.
- Version every facet spec and item; maintain audit trails (facet spec → prompt → NLI decision → pilot → calibration → deployment).
- Public changelog when FRIs are promoted/demoted and when gates are tuned.

---

### 6.8 Standard Sample-Size References (Consolidated) (P3)

These are *defaults* unless experts advise facet-specific alternatives:
- **Per item** (GRM, 5-point): **400–600 responses** to stabilize thresholds and a.
- **Pilot total** (≈ 250–300 items): **4k–6k respondents** using planned-missing forms.
- **DIF** (2-group): **≥ 400–500 per group** for stable polytomous DIF.
- **Test–retest**: **≥ 200** respondents across key strata.
- **External validity studies**: **≥ 500–1,000** depending on design and measures.

---

### 6.9 Consolidated Questions for External Experts (ordered by priority)

**P1 — Critical**
1) Are our **semantic gate thresholds** and SME alignment plan appropriate across all facets? What re-tuning cadence do you recommend?
2) Is the **FRI count/exposure** sufficient for robust linking? Any preferred linking diagnostics beyond TCC RMSD/θ μ,σ deltas?
3) Any facet-specific guidance on **planned-missing design** to minimize local dependence while preserving coverage?
4) Preferred **CAT exposure control** and content-balance strategy in strengths/personality banks?

**P2 — Important**
5) Recommended **fit indices/thresholds** for polytomous items (GRM) in our context.
6) Preferred **DIF methodology** (IRT-LR vs MH vs ordinal logistic) under multiple covariates.

**P3 — Nice-to-have**
7) Publication-ready **reporting templates** (fit tables, information plots, drift dashboards) you’d like to see standardized.

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

**How to engage:** Contact us at team@openstrengths.org to discuss specific consultation areas or research collaborations.

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

**How to engage:** Propose pilot study designs or specialized validation projects at team@openstrengths.org.

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

**How to engage:** Contact us at team@openstrengths.org to discuss funding priorities and partnership structures.

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

**How to engage:** Contact us at team@openstrengths.org to participate in advisory board or provide policy consultation.

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

**How to reach us:** team@openstrengths.org

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

*Prepared by the OpenStrengths Team · November 2025*  


---

## Appendix A: Glossary

- **Facet Description (Spec):** Canonical construct definition used for generation and NLI; versioned to prevent cross-loading.
- **Candidate Item:** Newly generated or imported item awaiting pilot data.
- **Operational Item:** Candidate promoted after passing pilot + calibration gates.
- **FRI (Facet Reference Item):** Calibrated, human-curated per-facet items used for audits, DIF, and linking/equating; replaceable.
- **Retired Item:** Item removed due to misfit, bias, or drift; retained for provenance.

## Appendix B — Expert Validation Blueprint (Table of Contents)
1. Scope & Intended Use (low-stakes; high-stakes excluded)
2. Standards Referenced (AERA/APA/NCME; SIOP Principles)
3. Design Overview (facet-seeded gen; FRIs; pilot→calibrate; no parameter inheritance)
4. Sampling Plan (planned-missing forms; per-item response targets; DIF group Ns)
5. Calibration (GRM estimation; fit indices; local dependence checks; drift thresholds)
6. Linking/Equating (FRI count & exposure; Stocking–Lord/TCC metrics)
7. CAT Simulations (stopping rules; exposure control; content balance)
8. Fairness & Bias (DIF methods; social-desirability controls; accessibility)
9. Monitoring (drift dashboards; kill switch; feature flags)
10. Pre-registration & Data (OSF prereg; sharing plan; anonymization)
11. Acceptance Criteria & Decision Gates (go/no-go rules)
12. Reporting Templates (fit tables; information plots; drift reports)

*Status:* outline subject to SME co-author review; versioned publicly.

## Appendix C — Independent Protocol Readiness Opinion (Outline)
- Who & Independence (qualifications; conflicts statement)
- Documents Reviewed (blueprint version; specs; risk controls)
- Standards Applied (AERA/APA/NCME; SIOP)
- Opinion (protocol is adequate for planned pilot/implementation under stated limits)
- Conditions (minimum sample sizes; specific DIF methods; shadow testing first)
- Limitations (not certifying outcomes; expires upon material changes)
- Follow-ups (evidence to review post-pilot; triggers for re-opinion)

*Status:* example outline; final content authored by external psychometrician.
