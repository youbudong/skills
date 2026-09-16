---
name: council
description: Multi-agent expert council for independent analysis, adversarial debate, evidence verification, technical review, and structured solution synthesis.
version: 1.3.1
---

# Council

## 1. Purpose

Council is a structured multi-agent reasoning framework for problems where independent analysis, disagreement detection, evidence verification, adversarial review, and decision traceability materially improve answer quality.

Council is not a majority-voting system.

Its purpose is to:

1. Define the problem precisely.
2. Audit assumptions.
3. Produce independent analyses.
4. Separate facts, assumptions, models, interpretations, and tradeoffs.
5. Detect disagreements and dependency relationships.
6. Verify important claims against appropriate evidence.
7. Design measurements or experiments when physical verification is more appropriate than speculation.
8. Track constraints and invalidate conclusions when constraints change.
9. Challenge conclusions adversarially.
10. Produce a traceable synthesis.
11. Preserve uncertainty when evidence is insufficient.
12. State what evidence could falsify important conclusions.

Core principle:

> **Independent analysis → assumption audit → evidence → dispute → verification → adversarial review → synthesis**

A correct answer is not necessarily a confident answer.

> **Insufficient Evidence is a valid and valuable conclusion.**

---

# 2. Core Philosophy

Council follows six principles.

## 2.1 Independence Before Consensus

Agents should initially reason independently.

Agreement between agents is not itself evidence.

If multiple agents rely on the same source, assumption, or reasoning chain, their agreement must not be counted as independent confirmation.

---

## 2.2 Evidence Over Rhetoric

Claims must be evaluated according to evidence quality rather than the confidence or authority of the agent presenting them.

A technically persuasive explanation is not equivalent to verified evidence.

---

## 2.3 Assumptions Are First-Class Objects

User statements, agent premises, inferred operating conditions, and unstated environmental assumptions can all affect conclusions.

Important assumptions must therefore be explicitly identified and tracked.

---

## 2.4 Constraints Are Versioned

A conclusion is valid only under the constraints on which it depends.

If a material constraint changes, dependent decisions must be reassessed or invalidated.

---

## 2.5 Measurement Beats Speculation When Appropriate

If a question concerns a physical state that can be cheaply and safely measured, measurement should normally take precedence over additional speculation.

However:

* specification questions → Evidence-First
* physical-state questions → Measurement-First
* expensive/risky measurements → compare Information Gain against Cost/Risk first

---

## 2.6 User Agency

Council must distinguish technical conclusions from value judgments.

Council must not silently assume that:

* lower cost is always better;
* higher performance is always better;
* smaller size is always better;
* simplicity is always better;
* reliability is always more important than development speed.

When tradeoffs depend on user preferences, Council should expose the tradeoff rather than silently choose the preference.

---

# 3. Scope

## 3.1 In Scope

Council is appropriate for:

* engineering design review
* electronics
* embedded firmware
* software architecture
* debugging
* hardware selection
* RF systems
* PCB review
* mechanical systems
* experiments
* technical research
* architecture decisions
* competing technical hypotheses
* complex factual questions
* problems involving conflicting evidence
* problems requiring external verification

---

## 3.2 Out of Scope

Council should not automatically expand into:

* unrelated research
* unnecessary historical background
* speculative future scenarios
* irrelevant optimization
* excessive agent discussion
* repeated restatement of already established facts

Scope expansion requires an identified reason.

This prevents **Debate Drift**.

---

# 4. Roles

Council uses a unified role model.

## 4.1 Moderator

Responsible for:

* problem definition
* scope
* stage transitions
* dependency management
* constraint versions
* stopping conditions
* preventing debate drift
* state lifecycle

Moderator must not determine technical truth merely by authority.

Moderator manages the process, not the technical conclusion.

---

## 4.2 Expert

Produces domain-specific analysis.

An Expert must state:

* conclusion
* reasoning
* assumptions
* evidence
* uncertainty
* dependencies
* falsifiers

---

## 4.3 Devil's Advocate

Attempts to disprove or weaken important conclusions.

The Devil's Advocate should search for:

* hidden assumptions
* counterexamples
* boundary conditions
* failure modes
* alternative explanations
* overlooked constraints

---

## 4.4 Evidence Checker

Evaluates claims against available evidence.

The Evidence Checker must distinguish:

* evidence actually accessed during the current execution;
* previously known information;
* model memory;
* user-provided information;
* inference.

The Evidence Checker must never upgrade evidence merely because a claim sounds authoritative.

---

## 4.5 Reviewer

Performs final consistency and quality checks.

Reviewer checks:

* evidence alignment
* assumption consistency
* constraint validity
* dependency validity
* unresolved disputes
* unsupported claims
* newly introduced facts
* falsifiability
* output correctness

---

## 4.6 Synthesizer

Combines validated results into the final answer.

The Synthesizer:

* does not invent evidence;
* does not silently resolve unresolved disputes;
* does not introduce unsupported technical facts;
* preserves important uncertainty;
* distinguishes fact from inference.

---

# 5. Evidence Model

Council uses evidence levels E0–E4.

## E0 — No Evidence

Pure assertion, intuition, or speculation.

Examples:

* "I think this should work."
* "Probably caused by the clock."

---

## E1 — Model Knowledge

Information available from model knowledge without current external verification.

This includes remembered:

* datasheet specifications
* protocol behavior
* component characteristics
* common engineering practice

E1 must not be presented as freshly verified information.

---

## E2 — User-Provided Evidence

Information explicitly supplied by the user.

Examples:

* measurements
* screenshots
* logs
* uploaded datasheets
* oscilloscope captures
* PCB files
* test results

E2 is evidence even when Council itself cannot independently verify it.

---

## E3 — Externally Accessed Source

A source that Council actually accessed during the current execution.

Examples:

* datasheet
* official reference manual
* application note
* standards document
* manufacturer documentation
* authoritative technical source

E3 requires actual source access.

A remembered datasheet is E1, not E3.

---

## E4 — Direct Verification

Evidence obtained through direct execution or measurement.

Examples:

* oscilloscope measurement
* multimeter measurement
* logic analyzer capture
* firmware test
* controlled experiment
* reproducible benchmark
* actual hardware test

E4 is normally stronger for physical-state questions than purely documentary evidence.

---

# 6. Evidence Tool Boundary

This is a hard rule.

## 6.1 External Access Required for E3

An agent may declare E3 only when it has actually accessed the external source during the current execution.

The following are not sufficient:

* "According to the datasheet..."
* model memory
* a remembered URL
* a source name without access
* another agent claiming that it checked the source
* generated citations without source retrieval

---

## 6.2 No External Tool

If Council has no external retrieval capability:

```text
E3 = Forbidden
E4 = Allowed only if direct execution/measurement capability exists
E1 = Maximum documentary evidence level
```

Claims that require E3 verification must instead be marked:

* Unverified
* Open Question
* Verification Required

---

## 6.3 E3 Source Locator

Every E3 claim must contain a source locator whenever technically possible.

Recommended structure:

```yaml
source_locator:
  type: official_datasheet
  url: "..."
  document: "..."
  revision: "..."
  page: 42
  section: "Electrical Characteristics"
  accessed_at: "..."
```

If page or section information is unavailable, the locator must still identify the exact source sufficiently for independent retrieval.

---

## 6.4 E3 Runtime Audit

E3 is not considered fully auditable merely because an Agent provides a URL.

The runtime should retain:

```yaml
source_id:
source_locator:
retrieval_timestamp:
retrieval_status:
content_hash_or_reference:
auditability:
```

Recommended auditability states:

```text
full
limited
unavailable
```

If runtime source tracking is unavailable, the Evidence Checker may record E3 only with:

* a source locator;
* retrieval information when available;
* `auditability: limited`.

If content hashing is not feasible, it is not required.

The minimum audit trail is:

```text
source_locator
+
retrieval_timestamp when available
+
retrieval_status
```

A Reviewer should flag E3 evidence whose access cannot be independently reconstructed.

---

## 6.5 Evidence Upgrade Rule

Evidence can only be upgraded when a higher-quality verification event actually occurs.

Example:

```text
E1 → external source accessed → E3
E3 → direct measurement → E4
```

Agreement between agents does not upgrade evidence.

---

# 7. Evidence Dimensions

Evidence quality should be evaluated along four dimensions.

### Authority

Who produced the source?

### Applicability

Does it apply to the exact:

* component
* revision
* operating condition
* hardware configuration
* software version

?

### Recency

Is the source current enough for the question?

### Reproducibility

Can another party independently verify the evidence?

A high-authority source can still be inapplicable.

An official document can still contain:

* revision mistakes
* errata
* conditional specifications
* obsolete information

Authority therefore does not automatically imply correctness.

---

# 8. Evidence Independence

Council must distinguish **Agent Agreement** from **Independent Evidence**.

Example:

```text
Expert A → Datasheet X
Expert B → Datasheet X
Expert C → Datasheet X
```

This represents:

```text
Agent Agreement = 3
Independent Sources = 1
```

not three independent confirmations.

---

## 8.1 Independent Group

Each evidence source should have:

```yaml
source_id:
independent_group:
source_type:
origin:
```

Sources should normally share an `independent_group` when they derive from:

* the same underlying dataset;
* the same unpublished measurement;
* the same original experiment;
* the same internal analysis;
* substantially the same source material.

Examples:

```text
Manufacturer Datasheet
Manufacturer Application Note
Manufacturer Product Brief
```

may belong to the same independence group if they clearly derive from the same underlying technical information.

---

## 8.2 Conservative Independence Rule

If independence cannot be established, Council should assume the sources are **not independently confirmed**.

Therefore:

> **Uncertain independence must not be counted as independent evidence.**

The burden of establishing independence is on the party claiming that two sources are independent.

---

## 8.3 Independent Measurements

Measurements from physically separate experiments may constitute separate evidence groups if:

* they were independently performed;
* they do not merely reproduce the same dataset;
* their error sources are meaningfully distinct.

Repeated measurements from the same setup are not automatically independent evidence.

---

# 9. Claim Model

Important conclusions should be represented as Claims.

Example:

```yaml
claim_id: C-03
statement: "The SI4732 requires an external crystal."
status: supported
confidence: high
depends_on:
  - A-02
evidence:
  - E-04
falsifier:
  - "Official documentation explicitly supports an internal oscillator configuration."
```

---

# 10. Claim Status

Allowed states:

```text
supported
conditionally_supported
unsupported
contradicted
unresolved
invalidated
```

### Supported

Available evidence supports the claim under the current constraints.

### Conditionally Supported

The claim is supported only if one or more explicit conditions hold.

### Unsupported

Insufficient evidence currently supports the claim.

### Contradicted

Reliable evidence conflicts with the claim.

### Unresolved

Relevant evidence or reasoning remains materially divided.

### Invalidated

The claim may previously have been valid, but a dependency or constraint changed.

---

# 11. Falsifiability

Important Claims should state what evidence could change or invalidate them.

Example:

```yaml
falsifier:
  - "Measured oscillator waveform is absent while power/reset conditions are verified."
  - "Official revision documentation specifies a different clock requirement."
```

A final conclusion should answer:

> **What evidence would make this conclusion wrong?**

This is especially important for:

* debugging hypotheses
* architecture decisions
* hardware diagnosis
* experimental interpretation
* ambiguous documentation behavior

A claim without a plausible falsifier should be treated cautiously unless it is a direct definitional or mathematical fact.

---

# 12. Problem Modes

Council supports five modes.

## Mode 0 — Direct

Use when:

* answer is simple;
* no material disagreement exists;
* no multi-domain reasoning is required;
* external verification is unnecessary.

Output should be concise.

### Mode 0 Override Rule

If the user requests multiple agents for a simple factual question, Council should not automatically launch a full council.

Use:

```text
Single Agent
+
optional Evidence Check
```

unless the problem genuinely benefits from independent analysis.

---

## Mode A — Quick Review

Use when:

* a second opinion is useful;
* there are limited uncertainties;
* no complex dependency graph is needed.

Typical flow:

```text
Problem Definition
→ Independent Review
→ Evidence Check
→ Final Review
```

---

## Mode B — Standard Council

Use when:

* multiple plausible explanations exist;
* meaningful disagreement is expected;
* several technical domains interact;
* evidence quality matters.

Flow:

```text
Problem Definition
→ Assumption Audit
→ Independent Analysis
→ Dispute Extraction
→ Evidence Resolution
→ Adversarial Review
→ Final Review
→ Synthesis
```

---

## Mode C — Engineering Council

Use for:

* hardware design
* firmware
* PCB
* RF
* embedded systems
* mechanical systems
* debugging
* component selection

Adds:

* measurement planning
* failure analysis
* dependency tracking
* constraint validation
* experiment design

---

## Mode D — Deep Debate

Use when:

* the problem is high consequence;
* evidence conflicts;
* multiple technical models remain viable;
* the user explicitly requests deep debate;
* several rounds of adversarial analysis are useful.

Mode D should normally use bounded rounds.

Recommended default:

```text
3–6 rounds
```

Never debate indefinitely.

---

# 13. Complexity Router

The Moderator should select the minimum sufficient mode.

Increase complexity when multiple conditions apply:

* multiple valid technical paths;
* cross-domain interaction;
* conflicting constraints;
* external verification required;
* high error cost;
* significant expert disagreement;
* difficult-to-reverse decisions.

Decrease complexity when:

* the question has one straightforward answer;
* evidence is already supplied;
* no material disagreement exists;
* further analysis would not change the decision.

---

# 14. Problem Definition

Before substantive analysis, Council should define:

```yaml
problem:
objective:
scope:
known_facts:
unknowns:
constraints:
success_criteria:
decision_required:
```

---

## 14.1 Missing Information

Council should identify information that could materially change the answer.

Ask the user only when necessary.

Default maximum:

```text
3 questions
```

If missing information is not decision-critical, proceed with explicit assumptions rather than blocking unnecessarily.

---

# 15. Assumption Audit

Before independent reasoning, extract material assumptions.

Each assumption should contain:

```yaml
assumption_id:
source:
statement:
status:
impact:
evidence:
```

Allowed states:

```text
Verified
Plausible
Unverified
Contradicted
Invalidated
```

---

## 15.1 Source

Possible sources:

```text
User
Expert
Documentation
Measurement
Inference
Environment
```

---

## 15.2 Shared Wrong Assumption

A critical Council failure occurs when every Expert independently accepts the same incorrect premise.

Therefore:

> **Agreement does not eliminate the need for Assumption Audit.**

Example:

```text
User premise:
"The crystal starts because both pins have different DC voltages."

Council must first test:
"Is differential DC voltage actually the relevant startup mechanism?"
```

The premise itself becomes an Assumption object.

---

# 16. Stage vs Round

These terms must not be confused.

### Stage

A logical phase of the Council process.

Examples:

```text
Problem Definition
Assumption Audit
Independent Analysis
Dispute Extraction
Evidence Verification
Adversarial Review
Final Review
Synthesis
```

### Round

A repeated iteration inside a stage or across stages.

Example:

```text
Round 1 → independent analysis
Round 2 → dispute challenge
Round 3 → revised analysis
```

A Stage answers:

> What kind of work are we doing?

A Round answers:

> Which iteration are we performing?

---

# 17. Independent Analysis

Each Expert should initially produce analysis independently.

Minimum output:

```yaml
conclusion:
reasoning:
assumptions:
evidence:
uncertainties:
dependencies:
falsifiers:
```

Experts should not see other Expert conclusions during the initial independent phase unless the problem explicitly requires shared information.

---

# 18. Dispute Extraction

After independent analysis, Moderator identifies material disagreements.

Each Dispute should contain:

```yaml
dispute_id:
claims:
category:
impact:
evidence:
status:
dependencies:
```

---

# 19. Dispute Categories

Council uses the following categories:

```text
FACT
ASSUMPTION
MODEL
INTERPRETATION
TRADEOFF
SCOPE
CONSTRAINT
EVIDENCE
CAUSALITY
```

---

# 20. Dispute Handling Strategy

Different disputes require different treatment.

| Category       | Default Treatment                                           |
| -------------- | ----------------------------------------------------------- |
| FACT           | Verify against authoritative evidence                       |
| ASSUMPTION     | Audit source and test validity                              |
| MODEL          | Compare predictions and boundary conditions                 |
| INTERPRETATION | Clarify definitions/context                                 |
| TRADEOFF       | Expose alternatives; do not silently choose user preference |
| SCOPE          | Return to scope definition                                  |
| CONSTRAINT     | Verify constraint and version                               |
| EVIDENCE       | Inspect source quality and independence                     |
| CAUSALITY      | Seek discriminating evidence or experiment                  |

---

## 20.1 Impact

Impact should be classified:

```text
Critical
High
Medium
Low
```

High-impact disputes receive verification priority.

---

# 21. Dependency Mapping

Important decisions should be represented as a dependency graph.

Example:

```text
D1: Crystal is oscillating
        ↓
D2: SI4732 clock is valid
        ↓
D3: I2C communication can be trusted
        ↓
D4: Firmware initialization is valid
```

A downstream decision must not be treated as independently established when it depends on an unresolved upstream claim.

---

## 21.1 Dependency Metadata

```yaml
decision_id:
depends_on:
status:
```

---

## 21.2 Parallel Execution

Independent tasks may run in parallel.

Dependent tasks must wait for required upstream results.

Do not parallelize merely because multiple agents are available.

---

## 21.3 Circular Dependency

If the dependency graph contains:

```text
D1 → D2
D2 → D1
```

Council must:

1. identify the circular dependency;
2. determine whether one dependency is actually an assumption;
3. split the claims into independently testable propositions;
4. return to Problem Definition if necessary.

The Moderator must not allow circular dependencies to become an endless debate loop.

---

# 22. Constraint Model

Constraints must be explicitly tracked.

Example:

```yaml
constraint_id: C-01
statement: "Input voltage = 24–30 V"
version: 1
source: User
status: active
```

Possible states:

```text
active
changed
invalidated
superseded
```

---

# 23. Constraint Change Protocol

Constraints may change during a Council session.

Never silently replace an old constraint.

Example:

```text
C1:
Input = 24 V

→ user changes requirement

C2:
Input = 28 V
```

The new constraint must receive a new version.

---

## 23.1 Invalidation Check

When a constraint changes, identify all Claims and Decisions that depend on it.

Example:

```text
Constraint C1 invalidated
        ↓
Claim C7 depends on C1
        ↓
Claim C7 reassessment required
        ↓
Decision D4 depends on C7
        ↓
Decision D4 reassessment required
```

A decision whose required constraint has been invalidated must not remain silently marked valid.

---

# 24. Decision Validity

A Decision is valid only if:

```text
required constraints are valid
AND
required claims are valid
AND
critical evidence remains applicable
AND
no unresolved critical dependency invalidates it
```

Possible states:

```text
valid
conditionally_valid
invalidated
unresolved
```

---

# 25. Decision Dependency

Every important decision should record:

```yaml
decision_id:
depends_on_claims:
depends_on_constraints:
depends_on_evidence:
validity:
```

This enables automatic or manual reassessment when inputs change.

---

# 26. Unresolved Disputes

Council must not force resolution when evidence is insufficient.

An unresolved dispute should retain:

```yaml
status: unresolved
reason:
competing_claims:
missing_evidence:
impact:
next_verification:
```

---

## 26.1 Final Output Dual Placement

An unresolved dispute must appear in both:

### Disputes

Describe:

* what the disagreement is;
* which claims conflict;
* why it remains unresolved.

### Uncertainty

Describe:

* how that unresolved dispute affects the final conclusion;
* what remains uncertain;
* whether action can safely proceed.

An unresolved dispute must not disappear merely because synthesis has been completed.

---

# 27. Evidence Resolution

For each important dispute:

1. identify required evidence;
2. determine highest available evidence level;
3. check source applicability;
4. check source independence;
5. check contradictions;
6. determine whether verification is sufficient.

Council should not spend equal effort on every dispute.

Priority should follow qualitatively:

```text
Impact × Uncertainty × Decision Dependence
```

This is a prioritization heuristic, not a requirement to assign artificial numerical values.

Do not create false precision.

---

# 28. Measurement-First Principle

Use conditional Measurement-First reasoning.

### Specification Question

Prefer:

```text
Evidence → Datasheet → Reference Manual → Application Note
```

### Physical-State Question

Prefer:

```text
Measurement → Experiment → Evidence
```

Examples:

```text
"What voltage should this pin have?"
→ documentation first

"What voltage is actually present on my board?"
→ measurement first
```

---

## 28.1 Risk / Cost Awareness

Measurement priority must consider:

* safety;
* equipment availability;
* cost;
* hardware risk;
* reversibility;
* expected information gain.

Do not recommend an expensive or dangerous measurement when a safe, cheap test can discriminate the same hypotheses.

---

# 29. Experiment Design

Experiments should specify:

```yaml
objective:
hypotheses:
equipment:
procedure:
expected_results:
failure_interpretation:
safety:
```

A good experiment should distinguish competing explanations rather than merely generate more data.

---

# 30. Verification Priority

Verification actions should be prioritized by expected usefulness.

A practical heuristic is:

```text
Verification Priority ≈ Information Gain / Cost
```

where Cost may include:

* time;
* money;
* hardware risk;
* complexity;
* irreversibility.

Example:

```text
Test A:
Measure one DC voltage.

Test B:
Capture startup timing including RESET,
CLOCK, I2C, and interrupt behavior.
```

If Test B can simultaneously discriminate several hypotheses at acceptable cost, it may have higher verification priority.

This is a prioritization heuristic, not a requirement to assign artificial numerical values.

---

# 31. Council-Level Failure Modes

Council must actively guard against system-level failures.

## 31.1 Shared Wrong Assumption

All agents accept the same incorrect premise.

Mitigation:

```text
Assumption Audit
+
independent premise challenge
```

---

## 31.2 Evidence Checker Capture

Evidence Checker is persuaded by expert wording rather than evidence.

Mitigation:

```text
source-first verification
+
source locator
+
independent review
```

---

## 31.3 Premature Convergence

Council settles on an early explanation before alternatives are tested.

Mitigation:

```text
Devil's Advocate
+
alternative hypothesis requirement
```

---

## 31.4 Synthesizer Novel Fact Injection

Synthesizer introduces a fact not present in the verified state.

Mitigation:

Every substantive final claim must trace to:

```text
User Input
Evidence
Measurement
Validated Reasoning
```

---

## 31.5 Evidence Correlation

Several sources appear independent but derive from the same underlying information.

Mitigation:

```text
independent_group
+
conservative independence rule
```

---

## 31.6 Authority Bias

Official documentation is treated as automatically correct.

Mitigation:

Check:

* revision;
* applicability;
* errata;
* operating conditions;
* contradictions;
* direct measurement when relevant.

---

## 31.7 Complexity Inflation

A simple question receives unnecessary Council processing.

Mitigation:

```text
Complexity Router
+
minimum sufficient mode
```

---

## 31.8 Debate Drift

The discussion moves away from the original decision.

Mitigation:

Moderator checks every new branch against:

```text
Objective
Scope
Decision Required
```

Branches without material relevance should be discarded.

---

# 32. Claim Status vs Confidence

These are independent dimensions.

### Claim Status

Describes the **state of evidence and logical support**.

### Confidence

Describes the **overall credibility of the current assessment**.

Example:

```yaml
status: conditionally_supported
confidence: high
```

This is valid when the condition itself is strongly established.

Likewise:

```yaml
status: unresolved
confidence: low
```

may be appropriate when competing explanations remain poorly constrained.

Do not automatically map:

```text
Supported → High
Conditionally Supported → Medium
Unresolved → Low
```

Status and Confidence must remain separate.

---

# 33. Confidence Rules

Allowed values:

```text
High
Medium
Low
```

Confidence should reflect:

* evidence quality;
* evidence independence;
* applicability;
* reproducibility;
* assumption stability;
* unresolved disputes;
* dependency stability.

Confidence must not be expressed with artificial precision such as:

```text
97.3%
```

unless a legitimate quantitative statistical model supports such a number.

---

# 34. Final Review

Before synthesis, Reviewer checks:

### Problem

* Is the original question answered?
* Is scope respected?

### Assumptions

* Are critical assumptions explicit?
* Did any assumption become contradicted?

### Evidence

* Are important claims supported?
* Are E3 claims actually externally sourced?
* Are E3 source locators present?
* Are sources applicable?
* Are sources independent where claimed?

### Constraints

* Are current constraints valid?
* Did any constraint change?
* Were dependent decisions reassessed?

### Disputes

* Are material disagreements represented?
* Are unresolved disputes preserved?

### Dependencies

* Are circular dependencies absent?
* Are downstream conclusions based on valid upstream claims?

### Synthesis

* Did Synthesizer introduce new facts?
* Is uncertainty preserved?
* Does every major conclusion have a traceable basis?

### Falsifiability

* Is it clear what evidence could change the important conclusions?

---

# 35. Output Profiles

Council uses output profiles according to Mode.

## Mode 0

```text
Answer
```

Optionally:

```text
Evidence
```

---

## Mode A

```text
Answer
Key Reasoning
Evidence
Caveat
```

---

## Mode B

```text
Conclusion
Confirmed Findings
Key Reasoning
Disputes
Evidence
Risks
Uncertainty
What Would Falsify the Conclusion
Next Step
```

---

## Mode C

```text
Conclusion
System State
Confirmed Findings
Assumptions
Technical Reasoning
Disputes
Evidence
Failure Modes
Constraints
Measurements / Experiments
Risks
Uncertainty
Falsifiers
Next Step
```

---

## Mode D

Mode D may include deeper sections as needed:

```text
Executive Conclusion
Problem Definition
Scope
Constraint State
Assumption Audit
Independent Analyses
Claim Registry
Evidence Registry
Evidence Independence
Dispute Registry
Dependency Graph
Alternative Hypotheses
Verification Plan
Experiment Design
Adversarial Review
Failure Analysis
Decision Validity
Risks
Uncertainty
Falsifiers
Open Questions
State Changes
Final Synthesis
Next Actions
```

### Mode D Output Rule

These sections are conditional.

Empty sections should be omitted.

The following core sections should normally remain:

```text
Conclusion
Confirmed Findings
Disputes
Evidence
Uncertainty
Falsifiers
Next Step
```

Mode D should not become a 19-section report when the problem does not require it.

---

# 36. Final Output Template Relationship

The Output Profiles define the **minimum structure appropriate to each Mode**.

The Mode B/C structures are the default final-output templates.

Mode D expands them when necessary.

No later template may silently introduce new reasoning rules.

The final answer should not reproduce internal Council state unless it materially helps the user.

---

# 37. Final Synthesis Rules

The Synthesizer must classify final statements as appropriate:

```text
Confirmed
Conditionally Supported
Unresolved
Unsupported
Invalidated
```

Avoid collapsing all of them into a single "answer."

---

## 37.1 Evidence Traceability

Important final claims should be traceable to:

```text
Claim
→ Evidence
→ Source
→ Assumption
→ Constraint
```

where applicable.

---

## 37.2 No Hidden Resolution

If Experts disagree and evidence does not resolve the dispute, the Synthesizer must not choose one silently.

Instead:

```text
A says X because...
B says Y because...
Current evidence does not distinguish them.
The following test would distinguish them...
```

---

# 38. State Model

Council state should contain at minimum:

```yaml
council:
  session_id:
  version:
  mode:
  stage:
  round:

problem:
scope:
objective:
success_criteria:

constraints: []
assumptions: []
claims: []
evidence: []
disputes: []
decisions: []
experiments: []

dependency_graph:
verification_queue:

state_history:
invalidations:
open_questions:
```

The state structure defined here describes **what Council stores**.

The state lifecycle and persistence rules are defined in §40.

---

# 39. State Transition Rules

Council state transitions must be explicit.

Example:

```text
Problem Defined
→ Assumptions Audited
→ Independent Analysis
→ Disputes Extracted
→ Evidence Resolved
→ Adversarial Review
→ Final Review
→ Synthesis
```

A Stage may not be considered complete if a required artifact is missing.

---

## 39.1 Invalidation Transition

Example:

```text
Decision D4 = valid

Constraint C2 changes

↓
Claims depending on C2 are reassessed

↓
D4 reassessed

↓
D4 = valid / conditionally_valid / invalidated / unresolved
```

---

# 40. State Persistence

For multi-round or long-running Council sessions, state must persist across rounds.

At minimum, persist:

```text
Problem
Constraints
Assumptions
Claims
Evidence
Disputes
Decisions
Dependencies
Invalidations
Open Questions
State History
```

---

## 40.1 Session Isolation

Every Council instance should have a unique:

```text
session_id
```

State from separate Council sessions must not be silently merged.

Example:

```text
session_id: council-si4732-20260916-001
```

A new session should normally create a new state namespace unless the user explicitly requests continuation.

---

## 40.2 Round Start

At the beginning of every Round:

1. load the latest Council state for the current `session_id`;
2. verify state version;
3. verify active constraints;
4. verify unresolved disputes;
5. verify invalidations;
6. identify changed inputs;
7. confirm the state belongs to the current Council session.

Agents must reason from the current state, not an earlier snapshot.

---

## 40.3 Round Commit

At the end of every Round:

1. record new Claims;
2. record new Evidence;
3. update Assumption status;
4. update Disputes;
5. update Decisions;
6. apply invalidations;
7. update dependency graph;
8. append state history;
9. create a new state version.

Example:

```yaml
state_version: 17
previous_state_version: 16
round: 3
changes:
  - claim_updated: C-04
  - evidence_added: E-11
  - decision_invalidated: D-02
```

---

## 40.4 Cross-Session Resumption

When a user returns later and asks Council to continue a previous session:

1. identify the original `session_id`;
2. load the latest valid persisted state;
3. verify state integrity;
4. revalidate active constraints;
5. revalidate critical assumptions;
6. identify changes in user requirements, environment, tools, or evidence;
7. reassess affected Claims and Decisions;
8. only then resume the Council process.

Prior state must not be treated as permanently valid merely because it was valid in an earlier session.

If the current user explicitly changes a constraint, the Constraint Change Protocol takes precedence.

---

## 40.5 Persistence Failure

If state cannot be reliably persisted:

* do not pretend continuity exists;
* reconstruct the minimum required state from available records;
* mark continuity as limited;
* revalidate critical assumptions and constraints before proceeding.

If prior state cannot be recovered, do not invent it.

---

# 41. Verification Queue

Important unresolved items should enter a queue.

Example:

```yaml
verification_id:
target:
reason:
priority:
required_evidence:
estimated_cost:
risk:
status:
```

Possible status:

```text
pending
running
verified
failed
blocked
cancelled
```

---

# 42. Stopping Conditions

Council should stop when:

1. the user's question is answered;
2. remaining uncertainty cannot materially change the action;
3. evidence is sufficient for the required confidence;
4. remaining disputes are low impact;
5. additional rounds fail the Novel Information Test.

---

## 42.1 Novel Information Test

Before another round, ask:

> Is this round likely to produce information that can materially change a Claim, Decision, Constraint, or verification plan?

If not, stop.

---

# 43. Agent and Tool Failure

If an agent fails:

* preserve completed results;
* mark the missing role;
* continue if redundancy exists;
* downgrade confidence if the missing role was critical.

If an external tool fails:

* do not fabricate its result;
* downgrade E3-dependent claims;
* convert them to verification-required states.

---

# 44. Conflicting Sources

When sources conflict:

1. identify exact disagreement;
2. compare revisions;
3. compare operating conditions;
4. compare applicability;
5. inspect errata;
6. evaluate source authority;
7. check source independence;
8. seek direct measurement if appropriate.

Do not resolve conflicts by source authority alone.

---

# 45. Engineering Review Protocols

## 45.1 Hardware

Check:

* absolute maximum ratings;
* recommended operating conditions;
* voltage domains;
* current capability;
* thermal limits;
* startup behavior;
* sequencing;
* pull-ups/pull-downs;
* decoupling;
* layout;
* grounding;
* EMC;
* connector limits;
* protection.

---

## 45.2 Firmware

Check:

* initialization order;
* reset behavior;
* clock configuration;
* interrupt handling;
* timing;
* concurrency;
* state machines;
* error handling;
* watchdog behavior;
* persistent configuration;
* boundary conditions.

---

## 45.3 PCB

Check:

* net connectivity;
* clearances;
* creepage where applicable;
* return paths;
* power distribution;
* thermal paths;
* sensitive analog/RF routing;
* high-current paths;
* connector mechanical constraints;
* manufacturing constraints;
* DRC.

---

## 45.4 RF

Check:

* impedance;
* antenna matching;
* transmission-line geometry;
* grounding;
* crystal/clock requirements;
* RF layout;
* shielding;
* noise sources;
* supply noise;
* antenna environment.

---

## 45.5 Debugging

Prefer:

```text
Symptom
→ hypotheses
→ discriminating observation
→ measurement
→ elimination
→ root cause
→ fix
→ regression test
```

Do not jump directly from symptom to root cause.

---

# 46. Code Review Protocol

For code review:

1. establish intended behavior;
2. identify build/runtime environment;
3. inspect control flow;
4. inspect state transitions;
5. inspect resource ownership;
6. inspect concurrency;
7. inspect error handling;
8. inspect boundary conditions;
9. inspect hardware/API assumptions;
10. propose tests.

Compiler output and runtime logs should be treated as evidence, not merely commentary.

---

# 47. Language Policy

Council's documentation and internal schemas are written in English for consistency and execution precision.

However:

> **User-facing output must follow the user's language.**

Default rule:

```text
Documentation language:
English

Internal Council language:
English preferred

User-facing output language:
Follow the user's language
```

---

## 47.1 Language Selection

The final response language should be determined in this order:

1. Explicit language requested by the user.
2. Language used predominantly in the user's current request.
3. Language established by the current conversation.
4. English as the fallback language.

An explicit user instruction always takes precedence.

Example:

```text
User:
"请用中文解释这个问题。"

→ Final output: Chinese
```

```text
User:
"Please answer in English."

→ Final output: English
```

---

## 47.2 Internal vs User-Facing Language

Internal Council artifacts may remain in English:

```yaml
claim:
evidence:
assumption:
dispute:
decision:
dependency:
constraint:
```

Agent prompts, state schemas, evidence metadata, and control structures should normally remain in English.

This does not require the final answer to be English.

The language used for internal reasoning must not determine the language of the user-facing response.

---

## 47.3 Technical Terminology

Technical identifiers should normally remain unchanged.

Examples:

```text
SI4732-A10-GSR
CH32V307RCT6
I2C
SPI
RESET
SDA
SCL
GPIO
PAM8403
```

Do not translate identifiers merely to make the response linguistically uniform.

Technical terminology may be presented bilingually when useful:

```text
Assumption Audit（假设审计）
Evidence Checker（证据检查）
Dependency Graph（依赖图）
```

After a term has been established, either form may be used consistently.

---

## 47.4 Mixed-Language Input

If the user mixes languages, Council should normally use the predominant language of the current request.

Example:

```text
User:
"这个 SI4732 的 crystal startup 是怎么工作的？"
```

Recommended output:

```text
Chinese explanation
+
English technical terminology where appropriate
```

Do not automatically switch the entire answer to English because technical terms are English.

---

## 47.5 Explicit Output Requirements

If the user explicitly requests:

* Chinese
* English
* Japanese
* bilingual output
* translated terminology
* original terminology

Council must follow that requirement for the final output.

If bilingual output is requested, avoid duplicating the entire answer unnecessarily.

Prefer:

```text
中文说明
English technical term
```

rather than producing two complete copies unless the user explicitly requests both.

---

## 47.6 Evidence and Source Language

Evidence should retain the original source language when exact wording matters.

For example:

```text
Official documentation:
"RESET input pin..."
```

The final response may explain the meaning in the user's language.

Do not translate a quoted technical statement in a way that changes its technical meaning.

When exact wording is important:

```text
Original:
"..."

Explanation:
...
```

---

## 47.7 Code and Structured Data

Code, API names, variable names, protocol fields, YAML keys, JSON keys, and other machine-readable identifiers should not be translated.

For example:

```yaml
status: conditionally_supported
confidence: high
```

should remain unchanged even when the surrounding explanation is Chinese.

---

## 47.8 Final Language Check

Before producing the final answer, Council should perform:

```text
[ ] Explicit user language requirement followed
[ ] User-facing language matches current conversation
[ ] Technical identifiers preserved
[ ] Code and structured data not translated
[ ] Source quotations preserved where necessary
[ ] No accidental language switch caused by internal Agent language
```

The final response should feel native to the user's conversation rather than reflecting the language of the internal Council process.

---

# 48. Prompt Templates

## Expert Prompt

```text
Analyze the problem independently.

Return:

1. Conclusion
2. Reasoning
3. Assumptions
4. Evidence
5. Uncertainty
6. Dependencies
7. What would falsify your conclusion
8. What additional information would materially change your conclusion

Do not treat another agent's agreement as evidence.
Do not claim E3 without actual external source access.
```

---

## Devil's Advocate Prompt

```text
Attempt to disprove the current leading conclusions.

Identify:

1. Hidden assumptions
2. Alternative explanations
3. Boundary conditions
4. Contradictory evidence
5. Failure modes
6. Missing measurements
7. Falsifiers

Do not manufacture disagreement where evidence is strong.
```

---

## Evidence Checker Prompt

```text
Check the important claims against available evidence.

For every important source:

- identify source
- identify revision/version
- identify applicability
- identify independence group
- identify evidence level
- identify contradictions
- provide source locator when E3

Never claim external verification without actual source access.
```

---

## Reviewer Prompt

```text
Audit the Council state.

Check:

- problem definition
- assumptions
- evidence
- evidence independence
- claim status
- confidence
- constraints
- dependencies
- invalidations
- unresolved disputes
- falsifiers
- unsupported facts
- novel synthesis facts
- output consistency

Do not introduce new unsupported technical claims.
```

---

# 49. Example State

```yaml
council:
  session_id: council-si4732-20260916-001
  version: 1.3.1
  mode: C
  stage: evidence_verification
  round: 2

claim:
  id: C-07
  statement: "The communication failure is caused by clock startup."
  status: conditionally_supported
  confidence: medium

assumptions:
  - id: A-03
    statement: "Power and reset timing are valid."
    status: unverified

evidence:
  - id: E-12
    level: E4
    type: logic_analyzer
    independent_group: measurement_group_1

decision:
  id: D-04
  statement: "Verify oscillator startup before changing firmware."
  validity: conditionally_valid
  depends_on:
    - C-07
    - A-03
```

---

# 50. Example: SI4732 Debugging

Suppose a user reports:

> "SI4732 does not respond over I2C."

Council should not immediately conclude:

```text
I2C address is wrong.
```

Instead:

### Problem

```text
Objective:
Determine why SI4732 does not respond to I2C commands.
```

### Assumptions

```text
A1:
SI4732 is powered correctly.

A2:
RESET timing is valid.

A3:
Crystal/clock is operational.

A4:
I2C wiring is correct.

A5:
Address interpretation is correct.
```

### Competing hypotheses

```text
H1: power problem
H2: reset problem
H3: clock problem
H4: I2C electrical problem
H5: incorrect address
H6: firmware initialization problem
```

### Verification

Prefer a measurement that can distinguish several hypotheses.

For example:

```text
Capture:
VCC
RESET
clock
SDA
SCL
```

during startup.

The Council should not assume that a user's explanation of why the crystal should oscillate is itself correct.

The exact SI4732 variant, crystal configuration, board wiring, and documentation revision must be verified before making a device-specific claim.

---

# 51. Anti-Patterns

Council must reject the following behaviors.

## 51.1 Majority Vote

```text
3 agents say X
1 agent says Y
Therefore X is correct.
```

Invalid.

---

## 51.2 Consensus Bias

```text
Everyone agrees.
Therefore the claim is proven.
```

Invalid.

---

## 51.3 Fake Verification

```text
"I checked the datasheet..."
```

when the source was not actually accessed.

Invalid.

---

## 51.4 Evidence Inflation

```text
Four agents cited the same datasheet.
Therefore four independent sources confirm it.
```

Invalid.

---

## 51.5 Endless Debate

Continuing rounds after no material information gain.

Invalid.

---

## 51.6 False Precision

Giving unsupported numerical confidence such as:

```text
97.3% certain
```

Invalid unless justified by an actual quantitative model.

---

## 51.7 Hidden Constraint Change

Changing:

```text
24 V
```

to:

```text
30 V
```

without versioning the constraint and reassessing dependent conclusions.

Invalid.

---

# 52. Final Answer Philosophy

§37 defines **how** final synthesis should be constructed.

This section defines **what the final answer should optimize for**.

Council should optimize for:

```text
Correctness
Traceability
Useful uncertainty
Efficient verification
User agency
```

not:

```text
maximum confidence
maximum length
maximum agent count
maximum consensus
```

The final answer should tell the user:

1. what is established;
2. what is conditionally supported;
3. what remains uncertain;
4. why;
5. what evidence would resolve it;
6. what action is justified now.

---

# 53. Default Council Configuration

Recommended default:

```yaml
mode: auto
max_rounds: 4
max_user_questions: 3

evidence:
  require_locator_for_e3: true
  allow_e3_without_external_tools: false
  conservative_independence: true
  runtime_audit: true
  content_hash_optional: true

assumptions:
  audit_before_independent_analysis: true

constraints:
  versioned: true
  propagate_invalidation: true

dependencies:
  detect_cycles: true

output:
  omit_empty_sections: true
  preserve_unresolved_disputes: true
  include_falsifiers: true
  follow_user_language: true

state:
  persist_between_rounds: true
  persist_between_sessions: true
  commit_each_round: true
  session_isolation: true
```

---

# 54. Minimal Execution Algorithm

This section is the executable expansion of the principles defined in §2.

```text
1. Receive problem.

2. Determine user-facing language.

3. Select minimum sufficient Mode.

4. Establish or load session_id.

5. Define:
   objective
   scope
   constraints
   success criteria

6. Audit assumptions.

7. Identify missing information.

8. Run independent Expert analyses.

9. Register Claims.

10. Register Evidence.

11. Check Evidence Independence.

12. Extract Disputes.

13. Build Dependency Graph.

14. Prioritize verification.

15. Perform Evidence Verification and/or Measurement.

16. Update Claims.

17. Apply Constraint Change Protocol if necessary.

18. Run Adversarial Review.

19. Reassess Decisions.

20. Run Final Review.

21. Preserve unresolved disputes.

22. Synthesize final output in the user's language.

23. Commit Council state.

24. Stop when additional rounds fail the Novel Information Test.
```

---

# 55. Design Principles Summary

Council is built around these rules:

```text
Do not confuse agreement with evidence.

Do not confuse authority with applicability.

Do not confuse model memory with external verification.

Do not confuse repeated citations with independent evidence.

Do not confuse confidence with correctness.

Do not hide assumptions.

Do not silently change constraints.

Do not allow invalidated decisions to survive unchanged.

Do not force unresolved disputes into false consensus.

Do not replace a cheap discriminating measurement with endless speculation.

Do not let the Synthesizer invent facts.

Do not continue debate without expected information gain.

Do not substitute Council preference for user preference.

Do not hide uncertainty merely to produce a cleaner answer.

Do not let internal Council language determine user-facing language.

Do not merge independent Council sessions without explicit continuation.
```

The fundamental rule is:

> **Council exists to make reasoning more reliable, not merely to make answers more elaborate.**

---

# 56. Quality Gate

Before final output, the Council must pass:

```text
[ ] Problem defined
[ ] Scope controlled
[ ] User-facing language determined
[ ] Critical assumptions audited
[ ] Constraints versioned
[ ] Important claims registered
[ ] Evidence levels valid
[ ] E3 claims auditable
[ ] E3 source locators present where applicable
[ ] Evidence independence checked
[ ] Critical disputes extracted
[ ] Dependencies valid
[ ] No unresolved circular dependency
[ ] Constraint changes propagated
[ ] Decisions validated
[ ] Unresolved disputes preserved
[ ] Falsifiers identified
[ ] No unsupported synthesis facts
[ ] Confidence justified independently of Claim Status
[ ] Correct session_id used
[ ] State committed when persistence is required
[ ] Final answer matches requested mode
[ ] Final answer follows user's language
```

Failure of a critical item should prevent a "fully confirmed" conclusion.

---

# 57. Final Principle

Council is not designed to guarantee certainty.

It is designed to make the path from:

```text
Question
→ Assumption
→ Claim
→ Evidence
→ Dispute
→ Verification
→ Decision
```

visible, testable, revisable, and traceable.

When evidence is insufficient, Council should say so.

When a conclusion is conditional, Council should state the condition.

When a decision depends on an assumption, Council should expose the dependency.

When a constraint changes, Council should reassess the affected state.

When evidence conflicts, Council should preserve the conflict until it is resolved or explicitly accepted as unresolved.

When further debate cannot produce useful information, Council should stop.

> **The goal is not consensus. The goal is reliable reasoning.**
