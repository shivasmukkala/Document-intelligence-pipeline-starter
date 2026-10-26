# Document Intelligence Pipeline - Implementation Plan

Main goal is to build, test, evaluate, and explain a working document extraction pipeline.

## Working rules

- Complete sessions in order, but spend extra sessions when a concept or error needs more time.
- Do not install all dependencies in advance. Install only what the current stage requires.
- Save actual outputs and test results; never fabricate evidence.
- Use a small, permitted sample before scaling up.
- Keep raw/reference answers separate from model predictions.
- Do not upload secrets or confidential documents.
- Record decisions and limitations, not just successful steps.

At 5–7 hours per week, treat each “day” as a work session. The plan may take 8–12 weeks or longer.

---
README.md — the project description and intended outcome.
PROJECT_PLAN.md — the step-by-step engineering plan.
.gitignore — excludes local configuration, virtual environments, and other files that shouldn't be committed.
.env.example — a safe template for future configuration.
requirements.txt — ready for dependencies to be added as needed.
docs/project-scope.md — initial scope and acceptance criteria.
docs/dataset-notes.md — a template for investigating CUAD.
docs/templates/daily-log-template.md — the daily reporting template.
docs/evidence-index.md — an index of proof to collect.
evaluation/README.md — the evaluation approach and planned reports.

## Stage 1 — Define the problem and check the environment

### Day 01 — Verify your computer and create the workspace
**Goal:** Know which tools are available and have a clean project folder.

**Check/install**
- VS Code is already available.
- In the VS Code PowerShell terminal, run:
  ```powershell
  python --version
  py --version
  git --version
  ```
- If Python checks fail, record the exact error before installing Python.
- If Git fails, record the error before installing Git.
- Create a workspace folder:
  ```powershell
  New-Item -ItemType Directory -Force -Path "$HOME\\Documents\\Projects\\document-intelligence-pipeline"
  Set-Location "$HOME\\Documents\\Projects\\document-intelligence-pipeline"
  Get-Location
  ```

**Understand**
- Python runs the application code.
- Git tracks changes and allows you to recover/review project history.
- VS Code is the editor; its terminal runs commands.

**Record**
- `docs/daily-log/day-01.md`: versions, exact outputs/errors, operating system, folder path, and fixes.
- Screenshot of terminal results if useful; redact personal paths if you prefer not to publish them.

**Done when:** Python and Git availability is known and the project folder exists.

### Day 02 — Write the problem statement and acceptance criteria
**Goal:** Define what success means before coding.

**Tasks**
1. Read `README.md`.
2. Confirm the initial target fields: `governing_law`, `termination_notice`, `renewal_terms`.
3. Define input types for version 1; start with text or extracted text before supporting complex formats.
4. Write success criteria: supported input, schema-valid output, error handling, evaluation, reproducible setup.
5. List out-of-scope items: legal advice, guaranteed correctness, production-grade handling of confidential legal documents.

**Record**
- `docs/project-scope.md`
- Open questions and assumptions.

**Done when:** A reader can tell what the first version will and will not do.

### Day 03 — Create the starter files and Git baseline
**Goal:** Start a clean, traceable codebase.

**Tasks**
1. Create the folders shown in the README.
2. Add `.gitignore` and `.env.example`.
3. Initialise Git, check status, and make a starter commit.
4. Create a GitHub repository only when ready to publish the initial scaffold.

**Commands**
```powershell
git init
git status
git add README.md PROJECT_PLAN.md .gitignore .env.example docs
git commit -m "docs: add project plan and starter documentation"
```

If Git identity is not configured, set your intended author name/email after reading Git's prompt. Never put API keys in `.env.example`.

**Record**
- `docs/daily-log/day-03.md`
- Commit ID and any issues.

**Done when:** The initial project structure is tracked by Git.

---

## Stage 2 — Understand the data before designing extraction

### Day 04 — Inspect CUAD and source terms
**Goal:** Understand whether the candidate dataset suits the chosen fields.

**Tasks**
1. Read the official CUAD website and repository.
2. Inspect the dataset format and annotations.
3. Record source terms/licence and intended use requirements.
4. Map each target field to an actual source annotation if possible.
5. If a target does not map cleanly, revise the field definition or document a defensible transformation.

**Sources**
- https://www.atticusprojectai.org/cuad/
- https://github.com/TheAtticusProject/cuad
- https://huggingface.co/datasets/theatticusproject/cuad

**Record**
- `docs/dataset-notes.md`
- `docs/field-mapping.md`: target field, source label, transformation, ambiguity, and limitations.

**Done when:** You can justify the dataset and explain the mapping without guessing.

### Day 05 — Inspect one document end-to-end
**Goal:** See the real shape of a source document and its annotations.

**Tasks**
1. Obtain one permitted sample using the documented dataset method.
2. Inspect its text and annotation format.
3. Locate evidence for each target field.
4. Record missing fields and ambiguous examples.
5. Keep large source data outside Git unless its terms clearly allow redistribution and it is necessary.

**Record**
- `docs/daily-log/day-05.md`
- Redacted excerpt or source ID, field locations, and observations.

**Done when:** One sample is understood from raw text through reference annotation.

### Day 06 — Design the reference-answer format
**Goal:** Create trusted expected answers for evaluation.

**Tasks**
1. Define how each field's reference answer is stored.
2. Include document ID and source evidence/annotation reference.
3. Define missing, not-applicable, and ambiguous cases.
4. Make a small pilot set and review it manually.
5. Keep reference answers separate from model predictions.

**Record**
- `evaluation/README.md`
- `evaluation/gold_answers.jsonl` or a documented equivalent.
- Rules for normalising answers.

**Done when:** Each reference answer is traceable to its source and the annotation rules are clear.

### Day 07 — Define the output schema
**Goal:** Make the desired output precise.

**Tasks**
1. Specify field names and types.
2. Decide how missing or uncertain values are represented.
3. Decide whether to include evidence snippets, source locations, confidence, or status.
4. Write valid and invalid example outputs.
5. Avoid treating model confidence as proof of correctness.

**Record**
- `docs/output-schema.md`
- Example JSON files under `data/samples/` using non-sensitive data.

**Done when:** The same input should produce output with a predictable structure.

---

## Stage 3 — Build a deterministic foundation

### Day 08 — Set up a Python virtual environment
**Goal:** Isolate dependencies.

**Commands (Windows PowerShell)**
```powershell
py -m venv .venv
.venv\\Scripts\\Activate.ps1
python -m pip install --upgrade pip
```
If `py` is unavailable, use the working Python command discovered on Day 01. If PowerShell blocks activation, capture the error and use the documented alternative for your environment rather than changing security settings blindly.

**Tasks**
- Create `requirements.txt`.
- Add only the packages currently needed.
- Confirm the terminal uses the virtual environment.

**Record**
- `docs/environment-setup.md`
- Python and pip versions, installation output.

**Done when:** A clean virtual environment is active and usable.

### Day 09 — Create a minimal runnable Python package
**Goal:** Confirm imports and basic execution.

**Tasks**
1. Create `src/document_intelligence/__init__.py`.
2. Create a small module with one simple function.
3. Run a small test script.
4. Choose a consistent execution method and document it.

**Record**
- Source file and output.
- `docs/daily-log/day-09.md`.

**Done when:** Your code runs from the project environment with a repeatable command.

### Day 10 — Implement the first document loader
**Goal:** Read a small text file into Python.

**Tasks**
1. Implement `parser.py` for UTF-8 text.
2. Check file existence and read errors.
3. Check for empty content.
4. Write tests for normal, missing, and empty files.
5. Only add PDF parsing once the text-based flow works and input requirements justify it.

**Record**
- `src/document_intelligence/parser.py`
- Tests and sample output.
- `docs/decisions/input-format.md`.

**Done when:** A valid sample is loaded and expected failures return clear errors.

### Day 11 — Add minimal text preparation
**Goal:** Prepare text without losing meaning.

**Tasks**
1. Normalise excess whitespace carefully.
2. Preserve paragraphs, punctuation, dates, and section identifiers where relevant.
3. Compare original and prepared text.
4. Test edge cases.

**Record**
- `src/document_intelligence/text_cleaner.py`
- Before/after example and tests.
- `docs/decisions/text-cleaning.md`.

**Done when:** Cleaning is deterministic, tested, and documented.

### Day 12 — Build a baseline extractor
**Goal:** Create a reference point before using an AI model.

**Tasks**
1. Implement a simple rule-based extractor for a narrow, explicit pattern.
2. Avoid pretending rules can handle all legal language.
3. Test on the pilot set.
4. Record successes and failures.

**Record**
- `src/document_intelligence/baseline_extractor.py`
- `reports/baseline-results.md`
- Examples of false positives and missed fields.

**Done when:** There is a baseline that can be compared with later methods.

---

## Stage 4 — Add structured extraction

### Day 13 — Choose the AI extraction approach
**Goal:** Make an informed model decision.

**Tasks**
1. Compare a hosted model API with a suitable local model if practical.
2. Check current pricing, rate limits, data handling, structured-output support, and regional availability.
3. Choose a small test before processing many documents.
4. Document the choice and alternatives.

**Record**
- `docs/decisions/model-choice.md`
- Estimated cost assumptions and privacy considerations.

**Done when:** The choice is justified and its trade-offs are recorded.

### Day 14 — Configure credentials safely
**Goal:** Keep secrets out of source control.

**Tasks**
1. Store the key in a local `.env` file or environment variable, as appropriate.
2. Ensure `.env` is ignored by Git.
3. Put only placeholder variable names in `.env.example`.
4. Verify `git status` does not show the secret file.
5. Do not paste API keys into chat or screenshots.

**Record**
- `docs/security-and-privacy.md`
- Redacted configuration example.

**Done when:** The application can read configuration without hardcoding credentials.

### Day 15 — Make the first model request
**Goal:** Prove that the selected model can extract a small example.

**Tasks**
1. Install the chosen SDK only now.
2. Make one small request using permitted text.
3. Request only the target fields.
4. Save a redacted response and record latency/cost where available.
5. Handle authentication, quota, and network errors.

**Record**
- `src/document_intelligence/extractor.py`
- Redacted request/response example.
- `docs/daily-log/day-15.md`.

**Done when:** A model request succeeds or the blocker is clearly documented.

### Day 16 — Define schema validation with Pydantic
**Goal:** Reject malformed results.

**Tasks**
1. Install Pydantic if not already installed.
2. Implement `schemas.py`.
3. Validate field types and required keys.
4. Define how missing/unknown values are represented.
5. Test valid and invalid payloads.

**Record**
- `src/document_intelligence/schemas.py`
- `tests/test_schemas.py`
- `docs/output-schema.md`.

**Done when:** Invalid model output can be detected before use.

### Day 17 — Add structured output and evidence
**Goal:** Improve output consistency and traceability.

**Tasks**
1. Use structured output or schema-constrained generation if the chosen model supports it.
2. Validate the result locally as well.
3. If possible, store a short supporting excerpt or source location for each extracted value.
4. Ensure the evidence actually appears in the source.
5. Handle missing and ambiguous fields explicitly.

**Record**
- Structured-output example.
- Evidence traceability tests.
- `docs/decisions/structured-output.md`.

**Done when:** Output is schema-validated and its claimed evidence can be checked.

### Day 18 — Connect the end-to-end pipeline
**Goal:** Join loading, cleaning, extraction, and validation.

**Tasks**
1. Implement `pipeline.py`.
2. Pass a document through each stage.
3. Return structured output and controlled error states.
4. Test a successful sample and at least two failure cases.

**Record**
- Pipeline code.
- Architecture diagram.
- End-to-end output and test evidence.

**Done when:** One function or command runs the whole core workflow.

---

## Stage 5 — Test and measure quality

### Day 19 — Establish the test suite
**Goal:** Protect core behaviour against regressions.

**Tasks**
1. Install pytest.
2. Test parser, cleaner, schema, and pipeline.
3. Mock the model call in unit tests so most tests do not incur API costs.
4. Keep a separate small integration test for a real model if useful.

**Record**
- `tests/`
- Test command and actual result.
- `docs/testing.md`.

**Done when:** Tests run repeatably and do not require a live model for every check.

### Day 20 — Run evaluation over the pilot set
**Goal:** Compare predictions with reference answers.

**Tasks**
1. Run the pipeline over the fixed pilot set.
2. Save predictions separately from reference answers.
3. Compare by document and field.
4. Record failures and exceptions.
5. Do not tune on the final holdout set if you later create one.

**Record**
- `evaluation/predictions.jsonl`
- `evaluation/comparison.csv`
- Run configuration and date.

**Done when:** Each result can be traced to a test example and reference answer.

### Day 21 — Calculate field-level metrics
**Goal:** Quantify quality, not just show examples.

**Tasks**
1. Define the unit of evaluation and what counts as correct.
2. Calculate field-level precision/recall where the labels and task formulation support those metrics.
3. Include exact match or another clearly defined matching rule.
4. Document normalisation and treatment of missing values.
5. Add manual review for ambiguous matches.

**Record**
- `src/document_intelligence/evaluator.py`
- `reports/field-metrics.csv`
- `docs/evaluation-methodology.md`.

**Done when:** A reader can reproduce and interpret the metrics.

### Day 22 — Perform error analysis
**Goal:** Identify why the pipeline fails.

**Tasks**
1. Review incorrect and missing predictions.
2. Categorise causes: source parsing, ambiguous language, extraction, schema, field mapping, or reference answer.
3. Select representative examples.
4. Propose one targeted improvement.

**Record**
- `reports/error-analysis.md`
- Failure categories and examples.

**Done when:** The next improvement is based on observed evidence.

### Day 23 — Improve and compare
**Goal:** Verify whether an improvement helps.

**Tasks**
1. Change one meaningful factor, such as prompt wording, chunking, or a parsing rule.
2. Re-run the same evaluation set.
3. Compare metrics and error categories.
4. Keep the change only if the evidence justifies it.
5. Note possible overfitting to the pilot set.

**Record**
- Before/after report.
- Decision note under `docs/decisions/`.
- Commit identifying the change.

**Done when:** You can explain what improved, what did not, and what remains uncertain.

### Day 24 — Test edge cases and reliability
**Goal:** Make failure behaviour predictable.

**Tasks**
- Test empty text, unreadable files, missing fields, long input, malformed model output, API timeout, rate limit, and invalid credentials without exposing secrets.
- Decide whether to retry, return an error, or mark a field unknown.
- Ensure logs do not expose sensitive document text.

**Record**
- `tests/test_edge_cases.py`
- `docs/troubleshooting.md`
- Error-handling table.

**Done when:** Common failure modes have defined and tested behaviour.

### Day 25 — Review security, privacy, and cost
**Goal:** Make responsible choices before exposing the project publicly.

**Tasks**
1. Check dataset terms and redistribution rules.
2. Check model-provider data handling and pricing.
3. Check `.gitignore` and scan tracked files for secrets.
4. Avoid sending confidential documents to an external API.
5. Record per-document cost estimates if measurable.

**Record**
- `docs/security-and-privacy.md`
- `reports/cost-and-latency.md` where data is available.

**Done when:** Risks and cost assumptions are documented, not hidden.

---

## Stage 6 — Make the pipeline usable

### Day 26 — Add a command-line interface
**Goal:** Run the pipeline without editing source code.

**Tasks**
1. Accept an input file path.
2. Run the pipeline and output JSON.
3. Support a destination path for the result.
4. Return helpful errors for invalid input and configuration.

**Record**
- `cli.py`
- Example command and output.
- `docs/usage.md`.

**Done when:** A new user can run a documented command on a sample.

### Day 27 — Add result metadata and repeatability
**Goal:** Make outputs auditable.

**Tasks**
1. Include document ID, schema version, and run metadata where useful.
2. Record model/configuration identifiers where available.
3. Avoid storing secrets or unnecessary raw text.
4. Define output naming and overwrite behaviour.

**Record**
- Output format examples.
- `docs/reproducibility.md`.

**Done when:** The result and the conditions that produced it are clear.

### Day 28 — Add FastAPI only if useful
**Goal:** Provide an HTTP interface after the core workflow works.

**Tasks**
1. Create a health endpoint.
2. Create an extraction endpoint.
3. Validate request inputs and return structured errors.
4. Test locally using the interactive API docs or an HTTP client.

**Record**
- API source, example request/response, tests.
- `docs/api-usage.md`.

**Done when:** A local client can call the API. If time is short, defer this stage and polish the CLI.

### Day 29 — Add logging and operational information
**Goal:** Make behaviour easier to investigate.

**Tasks**
1. Log stage names, elapsed time, and error category.
2. Redact secrets and avoid logging full sensitive document content.
3. Add clear messages for configuration failures.
4. Test the logging behaviour.

**Record**
- `docs/operations.md`
- Redacted sample logs.

**Done when:** A developer can understand a failed run without exposing confidential information.

### Day 30 — Reproduce from a clean setup
**Goal:** Verify the instructions, not just the developer's existing machine.

**Tasks**
1. Follow the README in a fresh virtual environment.
2. Install declared dependencies.
3. Run tests.
4. Run a sample through the pipeline.
5. Fix undocumented manual steps.

**Record**
- Exact commands and results in `docs/reproducibility.md`.

**Done when:** The core flow can be recreated from the documented steps.

---

## Stage 7 — Publish a credible portfolio

### Day 31 — Finalise the architecture and design decisions
**Goal:** Explain why the implementation looks the way it does.

**Tasks**
- Draw the actual architecture, not the intended future architecture.
- Document why each dependency is used.
- Separate implemented features from planned features.
- Record alternatives rejected and reasons.

**Record**
- `docs/architecture.md`
- `docs/decisions/`.

**Done when:** The design documentation matches the code.

### Day 32 — Finish the README
**Goal:** Let a first-time visitor understand and run the project.

**Tasks**
- Verify overview, prerequisites, installation, usage, tests, sample output, evaluation, limitations, privacy, and roadmap.
- Include actual measured results only.
- Link detailed documents and evidence.
- Remove claims about features that are not implemented.

**Record**
- Final README and a review checklist.

**Done when:** A reader can understand the purpose and reproduce the core workflow.

### Day 33 — Organise proof and daily logs
**Goal:** Make the development process inspectable.

**Tasks**
- Complete daily logs.
- Save redacted terminal/test output and useful screenshots.
- Link proof to the relevant stage and code.
- Avoid publishing API keys, personal data, or unauthorised document contents.

**Record**
- `evidence/`
- `docs/daily-log/`
- `docs/evidence-index.md`.

**Done when:** Each major milestone has meaningful proof.

### Day 34 — Final quality review
**Goal:** Check that the repository is safe and honest.

**Tasks**
- Run tests and inspect `git status`.
- Review tracked files and secret handling.
- Confirm dataset terms and licence notes.
- Check all README links and commands.
- Verify evaluation results are reproducible.
- Mark each feature as complete, in progress, or deferred.

**Record**
- `docs/final-review.md`
- Final test report.

**Done when:** The repository accurately reflects what was built.

### Day 35 — Publish version 1
**Goal:** Release the first reproducible portfolio version.

**Tasks**
1. Push the final files to your own public GitHub repository.
2. Verify the public README and evidence links.
3. Create a release/tag only if the project is genuinely ready.
4. Follow the instructions as a new user.
5. Write a retrospective covering achievements, errors, lessons, limitations, and next steps.

**Record**
- Repository URL and release/commit reference.
- `docs/project-retrospective.md`.

**Done when:** Another person can understand, inspect, and run the documented core workflow.

---

## Daily record template

For each session, create `docs/daily-log/day-NN.md` using `docs/templates/daily-log-template.md`.

Do not fill in successful outcomes in advance. Record the real command, output, error, fix, and verification after performing the work.
