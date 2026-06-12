## ISM Control Mapping Workflow

Using [ISM-Control-Discovery-Prompt](https://raw.githubusercontent.com/itiligent/ai/refs/heads/main/ISM-Control-Discovery-Prompt.txt) to map ISM controls from a DOCX or PDF document is a controlled two-step process.

For controlled and repeatable mapping, the prompt files's `Operating mode` is set differenty for each step:

```text
Step 1: Operating mode = `Section ledger only`

Step 2: Operating mode = `Mapping from supplied frozen ledger`

(A 3rd `Full run` ad-hoc operating mode exists, see bottom.)
```

---

## Why A 2 Step Approach Is Needed

ISM control mapping can vary significantly if the LLM is allowed to re-extract and reinterpret the document while it works. This can cause sections, evidence and control relevance to shift during a single mapping run, reducing consistency.

This 2 step workflow avoids that variability by freezing the document structure and evidence ledger before mapping begins. This makes the mapping output more stable and reviewable.

---


## Step 1: Create A Frozen Document Ledger/Index

- Frozen ledger = fixed document map + fixed evidence index
- Freezes the document structure
- Stops the model from re-reading the document differently each time.
- Freezes the eligible document sections
- Locks in which sections are considered relevant for ISM mapping in the second step.

> [!NOTE]
> Do not reuse an old frozen ledger for a changed document, You must create this for each scan

---

Before running the prompt, in the downloaded prompt file set:

```text
Operating mode = `Section ledger only`
```

Start a new chat.

Attach only:

```text
1. ISM-Control-Discovery-Prompt
2. The document you want to scan (DOCX/PDF)
3. The ASD OSCAL ISM catalog JSON/YAML
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt as a prompt.

Use the attached Doco as `Doco`.

Set:

Operating mode = Section ledger only

Create the Excel workbook output and save it as:

Run1-Frozen-Section-Ledger.xlsx

Stop after creating and freezing the Section Eligibility Ledger and Evidence Cluster Ledger.

Do not perform mapping.

Do not perform existing-control quality review.

Do not perform suggested controls.

Do not perform suggested alternate controls.

Do not create optional review candidates.

The output workbook must contain the frozen Section Eligibility Ledger and Evidence Cluster Ledger required for later mapping-from-ledger runs.
```

Download the output and save it as:

```text
Run1-Frozen-Section-Ledger.xlsx
```

---

## Step 2: Map ISM Controls From The Frozen Section Ledger Index

Before running the prompt, in `ISM-Control-Discovery-Prompt.txt` set:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Start a new chat.

Attach only:

```text
1. ISM-Control-Discovery-Prompt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Step 1
3. Run1-Frozen-Section-Ledger.xlsx
* Do not attach the original Doco *
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt as a prompt.

Set:

Operating mode = `Mapping from supplied frozen ledger`

Use Run1-Frozen-Section-Ledger.xlsx as the authoritative frozen Section Eligibility Ledger and Evidence Cluster Ledger.

Do not attach, inspect, read, parse, OCR, search or re-extract the original Doco.

Do not rebuild the section ledger.

Do not reinterpret section headings.

Do not reclassify section eligibility.

Do not create, rename, replace or modify any SEL-* Ledger Row IDs.

Do not create, rename, replace or modify any EC-* Evidence Cluster IDs.

Copy the Section Eligibility Ledger and Evidence Cluster Ledger worksheets from Run1-Frozen-Section-Ledger.xlsx into the output workbook unchanged, except for workbook styling or formatting.

Perform the mapping pass only from the supplied frozen ledger and selected catalog.

Create the Excel workbook output and save it as:

Run2-ISM-Mapping-Output.xlsx
```

Download the ISM mapping output and save it as:

```text
Run2-ISM-Mapping-Output.xlsx
```

---


## Run 3 (Optional, Repeatabilty Testing Only)

Start a new chat.

Attach only:

```text
1. The ISM-Control-Discovery-Prompt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Runs 1 and 2
3. Run1-Frozen-Section-Ledger.xlsx
4. Run2-ISM-Mapping-Output.xlsx
```

```
Run `ISM-Control-Discovery-Prompt` as a prompt.

Set:

Operating mode = `Mapping from supplied frozen ledger`

Use `Run1-Frozen-Section-Ledger.xlsx` as the authoritative frozen `Section Eligibility Ledger` and `Evidence Cluster Ledger`.

Use `Run2-ISM-Mapping-Output.xlsx` as the cross-run repeatability baseline workbook.

Do not attach, inspect, read, parse, OCR, search or re-extract the original Doco.

Do not rebuild the section ledger.

Do not reinterpret section headings.

Do not reclassify section eligibility.

Do not create, rename, replace or modify any `SEL-*` Ledger Row IDs.

Do not create, rename, replace or modify any `EC-*` Evidence Cluster IDs.

Copy the `Section Eligibility Ledger` and `Evidence Cluster Ledger` worksheets from `Run1-Frozen-Section-Ledger.xlsx` into the output workbook unchanged, except for workbook styling or formatting.

Perform the mapping pass only from the supplied frozen ledger and selected catalog.

Compare stable mapping artefacts against `Run2-Mapping-Baseline.xlsx`.

Create the Excel workbook output and save it as:

`Run3-ISM-Mapping-Test.xlsx`
```

## Run 4 (Optional)

Start a new chat.

Attach only:

```text
The ISM-Control-Discovery-Prompt
Run2-ISM-Mapping-Output.xlsx
Run3-ISM-Mapping-Test.xlsx
```

Use this message:

```text
You are reviewing repeatability and drift between two ISM mapping workbooks produced from the same frozen Section Eligibility Ledger using `ISM-Control-Discovery-Prompt`.

Inputs supplied in this run:

1. Baseline mapping workbook:
   `Run2-ISM-Mapping-Output.xlsx`

2. Repeat mapping workbook:
   `Run3-ISM-Mapping-Repeat-Test.xlsx`

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

5. Specific recommendations to improve `ISM-Control-Discovery-Prompt`, if needed.

6. Final statement:

   * whether Run3 can be accepted as repeatable against Run2;
   * whether further prompt changes are required

```




### Ad-Hoc Use

`Operating mode = Full run` performs extraction, ledger creation and ISM mapping in one pass. Use this where stability and reviewability are less important.
