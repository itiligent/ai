## Deterministic ISM Control Mapping | Prompt Repeatability Audit & Testing
- **ISM-Control-Discovery-Prompt.txt** is a repeatability-hardened prompt for ASD ISM control mapping.
- Scans and extracts document sections into a frozen Section Eligibility Ledger.
- Creates stable Evidence Cluster IDs for security-relevant document content.
- Maps existing ISM references already noted in a document.
- Suggests additional ISM controls only where the reviewed document contains high-confidence evidence supporting their applicability.
- Reviews weak existing controls and records possible alternate-control decisions.
- Separates extraction tasks from mapping tasls to reduce inconsistent LLM results.
   - Uses a three-run test pattern:
   - Run 1: freeze the section/evidence ledger
   - Run 2: generate baseline mapping
   - Run 3: repeat mapping and compare results
- Provides an audit trail for suggested controls, rejected candidates, optional review candidates, and repeatability checks.

## Before you start..

#### [Download the ISM-Control-Discovery-Prompt.txt](https://raw.githubusercontent.com/itiligent/ai/refs/heads/main/ISM-Control-Discovery-Prompt.txt)

Use a **new chat for each run**. Attach only the files needed for that run.

Use the **same ISM catalog source every time**. Best option: upload the same ASD OSCAL ISM JSON/YAML catalog file each run. Do not let one run use an uploaded catalog and another use a URL or fallback.

## Run 1 — create the frozen ledger only

Purpose: create the stable section/evidence baseline. This run must **not** produce suggested controls, alternates or optional candidates.

In the ISM-Control-Discovery-Prompt, change the setting near the top to:

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
1. The ISM-Control-Discovery-Prompt.txt
2. The Doco DOCX/PDF
3. The same ASD OSCAL ISM catalog JSON/YAML, unless using the exact same pinned YAML source
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt.txt as a prompt.

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

In the ISM-Control-Discovery-Prompt.txt, change:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Attach only:

```text
1. The ISM-Control-Discovery-Prompt.txt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Run 1
3. Run1_Frozen_Section_Ledger.xlsx
```

Do **not** attach the original Doco unless you only need filename confirmation. The mapping pass should not re-read or re-extract the Doco.

Use this message:

```text
Run ISM-Control-Discovery-Prompt as a prompt.

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

## Run 3 — repeat mapping from the same frozen ledger

Purpose: test whether suggested controls, alternate controls and optional candidates repeat when the ledger is fixed.

Start another **new chat**.

In the ISM-Control-Discovery-Prompt.txt, keep:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Attach only:

```text
1. The ISM-Control-Discovery-Prompt.txt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Runs 1 and 2
3. Run1_Frozen_Section_Ledger.xlsx
4. Run2_Mapping_Baseline.xlsx
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt.txt as a prompt.

This is Run 3 of the repeatability test.

Set:
Operating mode = `Mapping from supplied frozen ledger`

Use `Run1_Frozen_Section_Ledger.xlsx` as the authoritative frozen Section Eligibility Ledger and Evidence Cluster Ledger.

Use `Run2_Mapping_Baseline.xlsx` as the cross-run repeatability baseline workbook.

Do not re-extract Doco. Do not rebuild the section ledger. Do not create new Evidence Cluster IDs. Perform the mapping pass only and create the Excel workbook output.

Save the workbook as:

`Run3_ISM_Mapping_Check.xlsx`

```

Download the output and rename it:

```text
Run3_ISM_Mapping_Check.xlsx
```

## How to judge pass/fail

Purpose: Comparing the output between Run 2 and Run 3. 

Attach only:

```text
1. The ISM-Control-Discovery-Prompt.txt
2. Run2_Mapping_Baseline.xlsx
3. Run3_ISM_Mapping_Check.xlsx
```

Use this message:

```text
You are reviewing repeatability and drift between two ISM mapping workbooks produced from the same frozen Section Eligibility Ledger using `ISM-Control-Discovery-Prompt.txt`.

Inputs supplied in this run:

1. Baseline mapping workbook:
   `Run2_v8_Mapping_Baseline.xlsx`

2. Repeat mapping workbook:
   `Run3_v8_Mapping_Test.xlsx`

Task:

Compare the two workbooks to determine whether Run 3 repeated Run 2 without drift.

Do not re-run the ISM mapping task.
Do not inspect or re-extract the original Doco.
Do not use external ISM knowledge, prior outputs, memory, web search or assumptions.
Use only the two supplied workbooks.

Comparison method:

1. Inspect workbook structure:

   * confirm both workbooks contain the same relevant worksheets;
   * identify missing, extra or renamed worksheets.

2. Compare these worksheets row-by-row and column-by-column:

   * `Section Eligibility Ledger`
   * `Evidence Cluster Ledger`
   * `Candidate Assessment Ledger`
   * `ISM Action Register`
   * `Optional Review Candidates`
   * `Malformed References`
   * `Validation Summary`
   * `Repeatability Audit`
   * `Reader Guide`, if present

3. Treat these as critical repeatability artefacts:

   * Section inventory hash
   * Eligible section inventory hash
   * Evidence cluster inventory hash
   * Candidate pool hash
   * Action decision hash
   * Candidate Assessment Ledger row count
   * ISM Action Register row count
   * Existing control row count
   * Suggested control row count
   * Suggested alternate control row count
   * No ISM control identified row count
   * Optional Review Candidate row count
   * Optional candidates before cap
   * Optional candidates omitted due to cap
   * Weak existing-control alternate coverage result
   * Cross-run ledger hash matched
   * Cross-run action decision hash matched
   * Cross-run repeatability checks passed

4. Classify differences as:

   * `Expected baseline metadata difference`
   * `Formatting-only difference`
   * `Validation wording / metadata drift`
   * `Ledger drift`
   * `Evidence cluster drift`
   * `Candidate pool drift`
   * `Action decision drift`
   * `Optional candidate drift`
   * `Suggested alternate-control audit drift`
   * `Critical repeatability failure`

5. For any difference, report:

   * workbook sheet;
   * row identifier or row number;
   * column name;
   * Run 2 value;
   * Run 3 value;
   * classification;
   * likely cause;
   * whether it affects repeatability;
   * recommended prompt or process fix.

6. Pay special attention to v8-specific alternate-control audit fields. For every weak existing control, check whether the Candidate Assessment Ledger consistently records:

   * alternate candidate discovery method;
   * alternate candidates considered count;
   * best rejected alternate control ID, where applicable;
   * best rejected alternate failure reason;
   * whether a suggested alternate was promoted or defensibly rejected.

7. Check whether suggested alternate controls are repeatable:

   * if Run 2 and Run 3 both produce zero suggested alternates, confirm whether the rejection audit is identical and sufficient;
   * if suggested alternates appear, confirm the same controls, same sections, same Evidence Cluster IDs, same existing-control linkages and same actions appear in both runs.

8. Check optional candidate cap behaviour:

   * compare Optional Review Candidate rows;
   * compare optional candidates before cap;
   * compare optional candidates omitted due to cap;
   * confirm cap ordering is deterministic.

Output format:

Provide:

1. Executive verdict:

   * `Repeatability passed`
   * `Repeatability passed with metadata-only differences`
   * `Repeatability failed`

2. Summary comparison table with key counts and hashes from both runs.

3. Drift table listing all non-identical differences, grouped by classification.

4. Root-cause analysis of any meaningful drift.

5. Specific recommendations to improve `ISM-Control-Discovery-Action-Register-Prompt-v8-repeatability-hardened.txt`, if needed.

6. Final statement:

   * whether Run 3 can be accepted as repeatable against Run 2;
   * whether further prompt changes are required;
   * whether the ISM-Control-Discovery-Prompt.txt alternate-control audit is adequate.

```


Malformed References
Validation Summary / Repeatability Audit
```

