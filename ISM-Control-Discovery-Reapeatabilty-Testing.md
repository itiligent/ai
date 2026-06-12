## Reapeatibilty pass/fail

Compare the output between Run 2 and Run 3. 

Attach only:

```text
1. The ISM-Control-Discovery-Prompt.txt
2. Run2_Mapping_Baseline.xlsx
3. Run3_ISM_Mapping_Output.xlsx
```

Use this message:

```text
You are reviewing repeatability and drift between two ISM mapping workbooks produced from the same frozen Section Eligibility Ledger using `ISM-Control-Discovery-Prompt.txt`.

Inputs supplied in this run:

1. Baseline mapping workbook:
   `Run2_Mapping_Baseline.xlsx`

2. Repeat mapping workbook:
   `Run3_ISM_Mapping_Output.xlsx`

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

