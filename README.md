# paper-audit

A Claude Code skill for final-pass auditing of academic papers with computational experiments.

## What it does

Runs three sequential passes on a complete paper draft:

1. **Figure-text consistency** — reads each figure as an image and checks every textual claim against the visual evidence. Catches directions, shapes, and orderings stated incorrectly.

2. **Code-paper consistency** — checks that every formula, numerical method, parameter value, and protocol description in the paper matches the actual implementation. Catches misnamed algorithms, wrong sample sizes, and protocol mismatches.

3. **Redundancy and negative-value check** — reads the full paper looking for repeated key claims, meta-commentary sentences, uninformative qualifiers, and result paragraphs that restate the paper's thesis. Assigns Delete / Trim / Rewrite verdicts.

Each pass produces a structured list of flagged items with locations, quoted text, and recommended actions.

## Installation

From a local checkout of this repository, copy `SKILL.md` into your Claude Code skills directory:

```sh
mkdir -p ~/.claude/skills/paper-audit
cp SKILL.md ~/.claude/skills/paper-audit/SKILL.md
```

Then invoke with:

```
/paper-audit
```

## Scope

Covers: factual accuracy of figure descriptions, code-paper alignment, prose redundancy.

Does not cover: mathematical correctness of proofs, literature completeness, journal formatting, writing style beyond redundancy and factual accuracy.

## License

MIT
