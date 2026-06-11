[Download the ISM-Control-Discovery-Prompt](sandbox:/mnt/data/ISM-Control-Discovery-Action-Register-Prompt-ISM-Control-Discovery-Promp-repeatability-hardened.txt)

## Before you start

Use a **new chat for each run**. Attach only the files needed for that run.

Use the **same ISM catalog source every time**. Best option: upload the same ASD OSCAL ISM JSON/YAML catalog file each run. Do not let one run use an uploaded catalog and another use a URL or fallback.

Use clear filenames, for example:

* `Doco.docx`
* `ISM_catalog.yaml` or `ISM_catalog.json`
* `Run1_Frozen_Section_Ledger.xlsx`
* `Run2_Mapping_Baseline.xlsx`
* `Run3_Mapping_Test.xlsx`

## Run 1 — create the frozen ledger only

Purpose: create the stable section/evidence baseline. This run must **not** produce suggested controls, alternates or optional candidates.

In the ISM-Control-Discovery-Promp prompt, change the setting near the top to:

```text
Operating mode = `Section ledger only`
```

Keep:

```text
Catalog repeatability mode = `Strict pinned catalog`
Output mode = `Full coverage`
Optional review candidates = `On`
```

Attach only:

```text
1. The ISM-Control-Discovery-Promp prompt
2. The Doco DOCX/PDF
3. The same ASD OSCAL ISM catalog JSON/YAML, unless using the exact same pinned YAML source
```

Use this message:

```text
Run ISM-Control-Discovery-Action-Register-Prompt-ISM-Control-Discovery-Promp-repeatability-hardened.txt as a prompt.

Use the attached Doco as `Doco`.

This is Run 1 of the repeatability test.

Set:
Operating mode = `Section ledger only`

Create the Excel workbook output. Stop after creating and freezing the Section Eligibility Ledger and Evidence Cluster Ledger. Do not perform mapping, suggested controls, suggested alternate controls or optional review candidates.
```

After the workbook is produced, download it and rename it:

```text
Run1_Frozen_Section_Ledger.xlsx
```

Check the workbook before continuing. The validation summary should show:

```text
Operating mode = Section ledger only
Candidate Assessment Ledger rows = 0
Candidate pool hash = Not applicable — Section ledger only mode
Action decision hash = Not applicable — Section ledger only mode
```

Also confirm that these worksheets exist:

```text
Section Eligibility Ledger
Evidence Cluster Ledger
Validation Summary / Repeatability Audit
```

If the ledger IDs are not `SEL-0001`, `SEL-0002`, etc., or if Evidence Cluster IDs are not stable `EC-SEL-...` style IDs, stop and fix the prompt/output before running mapping.

## Run 2 — map from the frozen ledger

Purpose: generate the first mapping from the exact frozen ledger.

Start a **new chat**.

In the ISM-Control-Discovery-Promp prompt, change:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Attach only:

```text
1. The ISM-Control-Discovery-Promp prompt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Run 1
3. Run1_Frozen_Section_Ledger.xlsx
```

Do **not** attach the original Doco unless you only need filename confirmation. The mapping pass should not re-read or re-extract the Doco.

Use this message:

```text
Run ISM-Control-Discovery-Action-Register-Prompt-ISM-Control-Discovery-Promp-repeatability-hardened.txt as a prompt.

This is Run 2 of the repeatability test.

Set:
Operating mode = `Mapping from supplied frozen ledger`

Use `Run1_Frozen_Section_Ledger.xlsx` as the authoritative frozen Section Eligibility Ledger and Evidence Cluster Ledger.

Do not re-extract Doco. Do not rebuild the section ledger. Do not create new Evidence Cluster IDs. Perform the mapping pass only and create the Excel workbook output.
```

Download the output and rename it:

```text
Run2_Mapping_Baseline.xlsx
```

Check the workbook. It should show:

```text
Operating mode = Mapping from supplied frozen ledger
Candidate Assessment Ledger rows > 0
Candidate ledger non-empty in mapping mode = Yes
```

Record these values from the validation summary:

```text
Section inventory hash
Eligible section inventory hash
Evidence cluster inventory hash
Candidate pool hash
Action decision hash
ISM Action Register row count
Suggested alternate control row count
Suggested control row count
Optional Review Candidate row count
```

Run 2 is your **mapping baseline**.

## Run 3 — repeat mapping from the same frozen ledger

Purpose: test whether suggested controls, alternate controls and optional candidates repeat when the ledger is fixed.

Start another **new chat**.

In the ISM-Control-Discovery-Promp prompt, keep:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Attach only:

```text
1. The ISM-Control-Discovery-Promp prompt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Runs 1 and 2
3. Run1_Frozen_Section_Ledger.xlsx
4. Run2_Mapping_Baseline.xlsx
```

Use this message:

```text
Run ISM-Control-Discovery-Action-Register-Prompt-ISM-Control-Discovery-Promp-repeatability-hardened.txt as a prompt.

This is Run 3 of the repeatability test.

Set:
Operating mode = `Mapping from supplied frozen ledger`

Use `Run1_Frozen_Section_Ledger.xlsx` as the authoritative frozen Section Eligibility Ledger and Evidence Cluster Ledger.

Use `Run2_Mapping_Baseline.xlsx` as the cross-run repeatability baseline workbook.

Do not re-extract Doco. Do not rebuild the section ledger. Do not create new Evidence Cluster IDs. Perform the mapping pass only and create the Excel workbook output.
```

Download the output and rename it:

```text
Run3_Mapping_Test.xlsx
```

## How to judge pass/fail

The test passes only if Run 3 matches Run 2 on the important hashes and counts.

Minimum pass criteria:

```text
Cross-run ledger hash matched = Yes
Cross-run action decision hash matched = Yes
Candidate pool hash = same as Run 2
Action decision hash = same as Run 2
Candidate Assessment Ledger rows = same as Run 2
ISM Action Register rows = same as Run 2
Suggested alternate control rows = same as Run 2
Suggested control rows = same as Run 2
Optional Review Candidate rows = same as Run 2
```

Then compare these worksheets between Run 2 and Run 3:

```text
Candidate Assessment Ledger
ISM Action Register
Optional Review Candidates
Malformed References
Validation Summary / Repeatability Audit
```

If the hashes match, minor workbook formatting differences do not matter. If the hashes differ, inspect the `Candidate Assessment Ledger` first. That worksheet should explain whether the drift came from candidate generation, evidence scoring, tie-breaking, cap application, alternate-control selection or optional-candidate promotion.

## What not to do

Do not do this:

```text
Run 1: Full run
Run 2: Full run
Compare outputs
```

Do not do this either:

```text
Run 1: Section ledger only
Run 2: Section ledger only
Compare suggested controls
```

Those patterns rebuild the ledger each time, so they test extraction variability, not mapping repeatability.

The controlled pattern is:

```text
Run 1: Section ledger only
Run 2: Mapping from supplied frozen ledger
Run 3: Mapping from same supplied frozen ledger + Run 2 baseline
```
