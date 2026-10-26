# Document Intelligence Pipeline

> **Project status:** Planning / starter scaffold  
> **Purpose:** Build a reproducible Python pipeline that turns documents into validated, structured JSON and measures the quality of its extractions.

## 1. Project overview

Documents often contain important information in long passages, inconsistent layouts, or wording that is difficult to process automatically. This project explores how to extract a small set of fields from documents, validate the results, and measure how reliably the system works.

The first version will focus on commercial-contract text and a deliberately small set of fields:

- `governing_law`
- `termination_notice`
- `renewal_terms`

These fields are initial targets, not a claim that every dataset provides a directly matching label. During dataset investigation, we will document the mapping from source annotations to our output fields and revise the scope if necessary.

## 2. Project goals

By the end of the first release, the repository should contain:

1. A document loader for the chosen input format.
2. A defined output schema and validation rules.
3. A baseline extractor for comparison.
4. An AI-assisted extraction component, if an API or suitable local model is available.
5. Clear handling for missing, ambiguous, and invalid results.
6. An evaluation set with reference answers and traceable sources.
7. Field-level evaluation and error analysis.
8. Automated tests and reproducible setup instructions.
9. A command-line workflow; an HTTP API is an optional extension.
10. Documentation showing what was built, how it was tested, the evidence, limitations, and next steps.

## 3. Example input and output

**Illustrative input:** a permitted sample of contract text.

**Illustrative output:**

```json
{
  "document_id": "sample-001",
  "governing_law": "England and Wales",
  "termination_notice": "30 days",
  "renewal_terms": "Renews annually unless terminated"
}
```

This is only an example of the target shape. It is not a verified extraction from a particular contract. The actual pipeline must return values supported by the input and should represent missing or uncertain fields according to the documented schema.

## 4. Proposed architecture

```text
Document
   |
   v
Load / Parse
   |
   v
Text preparation
   |
   v
Field extraction (baseline first, AI-assisted later)
   |
   v
Schema validation
   |
   v
Structured JSON + run metadata
   |
   v
Evaluation against reference answers
   |
   v
Metrics, error analysis, improvements
```

The architecture is intentionally incremental. Start with a small, inspectable sample and a working baseline before adding model APIs, document parsers, databases, web APIs, containers, or cloud deployment.

## 5. Planned technology

| Area | Initial choice | When to introduce it |
|---|---|---|
| Language | Python | First implementation |
| Editor | VS Code | Development |
| Version control | Git and GitHub | From the start |
| Data and reference answers | JSON/JSONL and CSV as appropriate | Dataset stage |
| Validation | Pydantic | Once the output contract is defined |
| Tests | pytest | As components are implemented |
| AI extraction | A selected model SDK or suitable local model | After baseline extraction |
| Document parsing | Start with text; add a PDF parser such as Docling only if needed | After input requirements are confirmed |
| API | FastAPI (optional) | After the core pipeline works |
| Storage / analytics | DuckDB (optional) | If evaluation or query needs justify it |
| Packaging / deployment | Docker and a hosting platform (optional) | After reproducibility and tests |

Exact versions and model choices will be recorded when selected. Do not assume that an API subscription includes usage credits; check provider pricing before sending requests.

## 6. Dataset and responsible use

CUAD (Contract Understanding Atticus Dataset) is a candidate source for contract understanding work:

- Project website: https://www.atticusprojectai.org/cuad/
- Official repository: https://github.com/TheAtticusProject/cuad
- Hugging Face dataset page: https://huggingface.co/datasets/theatticusproject/cuad

Before using it, inspect its documentation, annotations, licence/usage conditions, file format, and field labels. Record the exact source and the mapping used for each target field. Do not assume our three target fields map one-to-one to the dataset's labels.

Use only documents that you are permitted to process and redistribute. Do not commit confidential contracts, private personal information, credentials, or API keys. Follow dataset terms and the chosen model provider's data policies.

## 7. How to work through the project

Follow `PROJECT_PLAN.md` in order. Each session should produce a tangible engineering result and a record of the work.

For every session:

1. Read the objective and prerequisites.
2. Check/install only the tools required for that task.
3. Complete the implementation or investigation steps.
4. Run the stated verification checks.
5. Record actual results, errors, and fixes.
6. Save proof (test output, sample output, metrics, or a redacted screenshot).
7. Update the relevant documentation and daily log.
8. Commit the verified changes.

Do not claim a test passed unless you ran it. If something fails, document the error and resolution instead of hiding it.

## 8. Project status

This repository is a **starter scaffold**, not a completed pipeline. The code and evaluation results will be added as the plan is completed.

| Component | Status |
|---|---|
| Project scope and starter documentation | Started |
| Environment verification | Not started |
| Dataset investigation | Not started |
| Document loader | Not started |
| Baseline extraction | Not started |
| AI-assisted extraction | Not started |
| Schema validation | Not started |
| Evaluation and error analysis | Not started |
| CLI / optional API | Not started |
| Final reproducibility review | Not started |

## 9. Definition of done for the first release

- A new user can follow the setup instructions on a clean environment.
- The pipeline can process a permitted sample document.
- The output follows a documented schema.
- Missing, malformed, and ambiguous fields are handled explicitly.
- Automated tests run successfully.
- Evaluation results can be reproduced from documented inputs and commands.
- The README reports actual results and known limitations.
- No secrets or unapproved private data are present in the public repository.

## 10. Current next step

Start with **Day 01 — verify the local environment and create the project workspace**. Use `PROJECT_PLAN.md` for the exact checks, commands, evidence, and documentation expected at each stage.
