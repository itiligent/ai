## 🔁 Repeatability Testing

Repeatability testing confirms that the same frozen evidence base produces the same, or materially consistent, ISM mapping result when the same inputs are used.

This test is designed to confirm that mapping outcomes are stable, explainable, and auditable.

A repeatable run should use the same:

| Input                                | Purpose                                       |
| ------------------------------------ | --------------------------------------------- |
| 🧊 Frozen Section Eligibility Ledger | Fixed section evidence base                   |
| 🧩 Evidence Cluster Ledger           | Fixed evidence clusters and stable `EC-*` IDs |
| 📚 ASD OSCAL ISM catalog             | Same control catalog and control metadata     |
| ⚙️ Prompt version                    | Same mapping rules and scoring logic          |
| 🎚️ Prompt settings                  | Same caps, thresholds, and output settings    |

If results change between runs, repeatability testing helps identify whether the change was caused by:

* a changed frozen ledger;
* an updated ISM catalog;
* a prompt change;
* a settings change;
* nondeterministic mapping drift.

---

## 🧪 Run 3 — Repeat Mapping from the Frozen Ledger

Run 3 repeats the mapping pass using the same frozen evidence base used in Run 2.

The original source document must **not** be supplied or re-read.

### 1. Set the operating mode

Edit `ISM-Mapper-Prompt.txt` and set:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

### 2. Start a new chat

Attach only:

```text
1. Promptname.txt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Runs 1 and 2
3. Run1-Frozen-Section-Ledger.xlsx
4. Run2-ISM-Mapping-Output.xlsx
```

### 3. Paste this message

```text
Run Promptname.txt as a prompt.

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

Compare stable mapping artefacts against `Run2-ISM-Mapping-Output.xlsx`.

Create the Excel workbook output and save it as:

`Run3-ISM-Mapping-Test.xlsx`
```

---

## 🔍 Run 4 — Compare Run 2 and Run 3 for Drift

Run 4 compares the baseline mapping workbook against the repeat mapping workbook.

This run does **not** perform ISM mapping. It only checks whether Run 3 reproduced Run 2 without meaningful drift.

### 1. Keep the operating mode setting:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

### 2. Start a new chat

Attach only:

```text
1. Promptname.txt
2. Run2-ISM-Mapping-Output.xlsx
3. Run3-ISM-Mapping-Test.xlsx
```

### Prompt message

```text
You are reviewing repeatability and drift between two ISM mapping workbooks produced from the same frozen Section Eligibility Ledger using `ISM-Mapper-Prompt.txt`.

Set:

Operating mode = `Mapping from supplied frozen ledger`

Inputs supplied in this run:

1. Baseline mapping workbook:
   `Run2-ISM-Mapping-Output.xlsx`

2. Repeat mapping workbook:
   `Run3-ISM-Mapping-Test.xlsx`

Task:

Compare the two workbooks to determine whether Run 3 repeated Run 2 without drift.

Do not re-run the ISM mapping task.
Do not inspect or re-extract the original Doco.
Do not use external ISM knowledge, prior outputs, memory, web search or assumptions.
Use only the two supplied workbooks.

Comparison method:

1. Inspect workbook structure:

   - confirm both workbooks contain the same relevant worksheets;
   - identify missing, extra or renamed worksheets.

2. Compare these worksheets row-by-row and column-by-column:

   - `Section Eligibility Ledger`
   - `Evidence Cluster Ledger`
   - `Candidate Assessment Ledger`
   - `ISM Action Register`
   - `Optional Review Candidates`
   - `Malformed References`
   - `Validation Summary`
   - `Repeatability Audit`
   - `Reader Guide`, if present

3. Treat these as critical repeatability artefacts:

   - Section inventory hash
   - Eligible section inventory hash
   - Evidence cluster inventory hash
   - Candidate pool hash
   - Action decision hash
   - Candidate Assessment Ledger row count
   - ISM Action Register row count
   - Existing control row count
   - Suggested control row count
   - Suggested alternate control row count
   - No ISM control identified row count
   - Optional Review Candidate row count
   - Optional candidates before cap
   - Optional candidates omitted due to cap
   - Weak existing-control alternate coverage result
   - Cross-run ledger hash matched
   - Cross-run action decision hash matched
   - Cross-run repeatability checks passed

4. Classify differences as one of:

   - `Expected baseline metadata difference`
   - `Formatting-only difference`
   - `Validation wording / metadata drift`
   - `Ledger drift`
   - `Evidence cluster drift`
   - `Candidate pool drift`
   - `Action decision drift`
   - `Optional candidate drift`
   - `Suggested alternate-control audit drift`
   - `Critical repeatability failure`

5. For each difference, report:

   - workbook sheet;
   - row identifier or row number;
   - column name;
   - Run 2 value;
   - Run 3 value;
   - classification;
   - likely cause;
   - whether it affects repeatability;
   - recommended prompt or process fix.

6. Pay special attention to alternate-control audit fields.

   For every weak existing control, check whether the Candidate Assessment Ledger consistently records:

   - alternate candidate discovery method;
   - alternate candidates considered count;
   - best rejected alternate control ID, where applicable;
   - best rejected alternate failure reason;
   - whether a suggested alternate was promoted or defensibly rejected.

7. Check whether suggested alternate controls are repeatable:

   - if Run 2 and Run 3 both produce zero suggested alternates, confirm whether the rejection audit is identical and sufficient;
   - if suggested alternates appear, confirm the same controls, same sections, same Evidence Cluster IDs, same existing-control linkages and same actions appear in both runs.

8. Check optional candidate cap behaviour:

   - compare Optional Review Candidate rows;
   - compare optional candidates before cap;
   - compare optional candidates omitted due to cap;
   - confirm cap ordering is deterministic.

Output format:

Provide:

1. Executive verdict:

   - `Repeatability passed`
   - `Repeatability passed with metadata-only differences`
   - `Repeatability failed`

2. Summary comparison table with key counts and hashes from both runs.

3. Drift table listing all non-identical differences, grouped by classification.

4. Root-cause analysis of any meaningful drift.

5. Specific recommendations to improve `ISM-Mapper-Prompt.txt`, if needed.

6. Final statement confirming:

   - whether Run 3 can be accepted as repeatable against Run 2;
   - whether further prompt changes are required.
```

---

## ✅ Expected Outcome

A successful repeatability test should show that Run 3 reproduced Run 2 using the same frozen evidence base.

Acceptable differences are usually limited to:

| Difference type                     | Acceptable? | Notes                                 |
| ----------------------------------- | ----------: | ------------------------------------- |
| Formatting-only changes             |       ✅ Yes | Styling, widths, colours, wrapping    |
| Workbook creation timestamp         |       ✅ Yes | Expected metadata difference          |
| Run label or output filename        |       ✅ Yes | Expected baseline metadata difference |
| Validation wording changes          |   ⚠️ Review | Accept only if meaning is unchanged   |
| Ledger row changes                  |        ❌ No | Indicates frozen evidence drift       |
| Evidence Cluster ID changes         |        ❌ No | Breaks traceability                   |
| Candidate score changes             |        ❌ No | Indicates mapping drift               |
| Action decision changes             |        ❌ No | Indicates repeatability failure       |
| Optional candidate ordering changes |   ⚠️ Review | May indicate nondeterministic ranking |

---

## 🧭 Repeatability Verdict Guide

Use the following guide when interpreting the Run 4 result:

| Verdict                                                  | Meaning                                                                                                        |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| ✅ `Repeatability passed`                                 | No meaningful differences were found                                                                           |
| 🟡 `Repeatability passed with metadata-only differences` | Only expected metadata, filename, timestamp, or formatting differences were found                              |
| 🔴 `Repeatability failed`                                | Ledger, evidence cluster, candidate, action decision, optional candidate, or alternate-control drift was found |

---

## 🧊 Important Rule

Repeatability testing is only valid if the original Doco is **not** supplied in Runs 3 or 4.

The purpose of the test is to prove that mapping is driven by the frozen evidence base, not by re-reading or reinterpreting the source document.
