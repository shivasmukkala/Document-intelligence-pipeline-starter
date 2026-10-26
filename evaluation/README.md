# Evaluation

Evaluation measures how closely pipeline outputs match reference answers.

## Principles
- Keep reference answers separate from predictions.
- Record source IDs and annotation references for traceability.
- Define missing, ambiguous, and not-applicable cases.
- Document normalisation and matching rules.
- Report results per field and explain limitations.
- Do not invent metrics or report results before running the evaluation.

## Planned artefacts
- `gold_answers.jsonl`: reviewed reference answers, if permitted to store.
- `predictions.jsonl`: outputs produced by a documented run.
- `comparison.csv`: per-example comparisons.
- `reports/field-metrics.csv`: field-level metrics.
- `reports/error-analysis.md`: failure categories and representative examples.

Check dataset terms before publishing any source text or annotations.
