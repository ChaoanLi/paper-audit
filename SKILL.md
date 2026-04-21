---
name: paper-audit
description: Final-pass audit for academic papers with computational experiments. Runs three sequential passes — figure-text consistency, code-paper consistency, and redundancy/negative-value — to catch factual contradictions, implementation mismatches, and prose that hurts more than it helps. Use when a draft is otherwise complete and ready for submission review.
origin: custom
---

# Paper Audit

A structured final review for computational papers. Three passes, each targeting a distinct failure mode. Run them in order.

## When to Activate

- Draft is content-complete and ready for submission review
- Paper describes numerical experiments with accompanying code
- Author wants a systematic check before sending to co-authors or a journal
- Author suspects redundancy or verbosity but cannot locate it

---

## Pass 1: Figure-Text Consistency

**Goal:** Every factual claim about a figure must be verifiable from that figure.

### Procedure

For each figure in the paper:

1. Read the figure as an image. Do not rely on memory or the caption alone.
2. Extract the visual facts: directions of curves, locations of minima/maxima, relative magnitudes, qualitative shapes, which method is highest/lowest.
3. Find every sentence in the surrounding text that makes a claim about this figure.
4. Check each claim against the visual facts. Flag any that:
   - Describe a direction or shape that is visually opposite or absent
   - Report a numerical range that does not match the visible scale
   - Use qualitative descriptors ("sharp", "flat", "smooth", "steep") that contradict what the figure shows
   - Assert a relative ordering between methods that the figure does not support

### Common failure modes

- Text written from memory of an earlier figure version, not the current one
- Qualitative adjectives derived from intuition rather than observation
- Numerical ranges copied from an outdated result file while the figure reflects a newer run
- Relative comparisons between methods stated in the wrong direction

### Output format

```
[Figure N, Section X] Claim: "..." 
Visual evidence: [what the figure actually shows]
Action: [rewrite / delete / verify source data]
```

Apply all fixes before proceeding to Pass 2.

---

## Pass 2: Code-Paper Consistency

**Goal:** Every formula, parameter, and method description in the paper must match the implementation exactly.

### Procedure

For each method or formula described in the experimental setup:

1. Locate the corresponding implementation in the codebase. Read the actual code — do not infer from function or variable names alone.
2. Check each of the following:

| Paper element | What to verify in code |
|---|---|
| Named numerical method | Is the actual algorithm what the paper names? (e.g., "Gaussian quadrature" vs. trapezoidal rule) |
| Formula | Does the code implement the exact expression? Check signs, factors, normalizations, and whether operations are at training points or a separate grid |
| Parameter values | Do n, σ², replications, kernel parameters in the paper match the config? |
| Protocol | Does the paper accurately describe what is fixed vs. re-randomized across replications? |
| Reported metrics | Does the paper report what the code actually computes (coverage, interval score, MSE, etc.)? |

3. For each result table, verify that column definitions match the code's output variable names.

### Common failure modes

- Paper names a sophisticated method but code uses a simpler one
- Sample sizes or noise levels differ between paper text and config file
- Number of replications stated in paper does not match the actual run
- A correction or adjustment described as universal is only applied to one method in code
- "Fixed design" described in paper but code re-randomizes inputs per replication

### Output format

```
[Section X / Eq. Y] Paper: "..."
Code: [file, line, actual behavior]
Action: [fix paper / fix code / add clarifying note]
```

Apply all fixes before proceeding to Pass 3.

---

## Pass 3: Redundancy and Negative-Value Check

**Goal:** Remove prose that repeats a point already made or actively reduces clarity.

### Procedure

Do one full sequential read of the paper. Track the following:

#### 3a. Repeated key claims

Note every occurrence of the paper's central contribution or finding. Flag occurrences beyond the second (Abstract and Introduction are expected; additional instances in Methods, Results, and Conclusion are suspect unless they add a new angle or new evidence).

#### 3b. Meta-commentary sentences

Flag any sentence whose only content is announcing what the next sentence or section does:
- "The following describes..."
- "We now present..."
- "As shown above..."
- "We apply X to this problem as follows."

These sentences can be deleted without information loss.

#### 3c. Uninformative qualifiers

Flag self-assessments that the reader cannot verify:
- "compact but challenging"
- "elegant", "natural", "straightforward", "simple"
- "directly demonstrating X" when the experiment tests only one instance of a claimed general phenomenon

#### 3d. Explanations that restate notation

Flag inline prose that paraphrases an equation immediately above it without adding interpretation. If removing the sentence loses no information, remove it.

#### 3e. Thesis-restating tail clauses

Flag sentence-final clauses of the form "..., thus demonstrating [the paper's main contribution]" appended to individual experiment results. The contribution belongs in the Introduction and Conclusion; repeating it after every result dilutes both.

### Verdict options

For each flagged item, assign one of:
- **Delete** — no information lost
- **Trim** — keep the core, cut the redundant tail  
- **Rewrite** — the point is worth making but the phrasing is padded or misleading

---

## Quality Gate

Before marking the audit complete:

- [ ] Every claim about a figure is verifiable from the current figure file
- [ ] Every named numerical method matches the actual implementation
- [ ] Every reported parameter matches the codebase config
- [ ] No key claim appears more than twice without adding new substance
- [ ] No meta-commentary sentences remain
- [ ] Result paragraphs do not end by reasserting the paper's main contribution

## Scope

This skill covers: factual accuracy of figure descriptions, code-paper alignment, and prose redundancy.

This skill does not cover: mathematical correctness of proofs, completeness of literature review, journal formatting, or writing style beyond redundancy and factual accuracy.
