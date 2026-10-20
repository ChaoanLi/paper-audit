# paper-audit

A Claude Code skill for final-pass auditing of academic papers with computational experiments.

## What it does

Runs three sequential passes on a complete paper draft:

1. **Figure-text consistency** — reads each figure as an image and checks every textual claim against the visual evidence. Catches directions, shapes, and orderings stated incorrectly.

2. **Code-paper consistency** — checks that every formula, numerical method, parameter value, and protocol description in the paper matches the actual implementation. Catches misnamed algorithms, wrong sample sizes, and protocol mismatches.

3. **Redundancy and negative-value check** — reads the full paper looking for repeated key claims, meta-commentary sentences, uninformative qualifiers, and result paragraphs that restate the paper's thesis. Assigns Delete / Trim / Rewrite verdicts.

Each pass produces a structured list of flagged items with locations, quoted text, and recommended actions.

## Installation

Clone the repository, then copy `SKILL.md` into your Claude Code skills directory:

```sh
git clone https://github.com/ChaoanLi/paper-audit.git
cd paper-audit
mkdir -p ~/.claude/skills/paper-audit
cp SKILL.md ~/.claude/skills/paper-audit/SKILL.md
```

Then invoke with:

```
/paper-audit
```

## Example audit

The following is a **simulated example**, not a report from a real manuscript.
Suppose a draft contains these inputs:

- `paper.tex`, Section 4: "Each scenario uses 1,000 Monte Carlo replications."
- `experiments/config.py`, line 12: `N_REPLICATIONS = 200`.
- `paper.tex`, Results: "The following paragraph describes the results."

An illustrative report would contain:

```text
[Section 4] Paper: "1,000 Monte Carlo replications."
Code: experiments/config.py:12 sets N_REPLICATIONS = 200.
Action: verify the manifest/log for the reported run. A current config
alone does not establish which setting generated the published results.

[Results] Text: "The following paragraph describes the results."
Verdict: Delete — the sentence adds no result or interpretation.
```

For the figure pass, supply the current figure images together with the
manuscript. If the figure or source data is missing, record that check as
unverified. Review proposed changes before accepting them; a flagged
discrepancy is not by itself evidence that the paper or code is wrong.

## Scope

Covers: factual accuracy of figure descriptions, code-paper alignment, prose redundancy.

Does not cover: mathematical correctness of proofs, literature completeness, journal formatting, writing style beyond redundancy and factual accuracy.

## License

[MIT](LICENSE).

## Related skills

- [academic-code-release](https://github.com/ChaoanLi/academic-code-release):
  prepare and document a Python research code supplement in Cursor.
- [conversational-language-coaching](https://github.com/ChaoanLi/conversational-language-coaching):
  optional language tips during ordinary Claude Code conversations.
