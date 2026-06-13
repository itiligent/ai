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
Run ISM-Mapper-Prompt.txt as a prompt.

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
1. ISM-Mapper-Prompt.txt
2. Run2-ISM-Mapping-Output.xlsx
3. Run3-ISM-Mapping-Test.xlsx
```

### Paste the below message 

Then run the below message with:
```text
run ISM Mapping Repeatability and Drift Validation Prompt as a prompt
```

```text
# ISM Mapping Repeatability and Drift Validation Prompt

You are reviewing repeatability and drift between two ISM mapping workbooks produced from the same frozen `Section Eligibility Ledger` using `ISM-Mapper-Prompt.txt`.

## Operating context


Inputs supplied in this run:

1. Baseline mapping workbook:
   `Run2-ISM-Mapping-Output.xlsx`

2. Repeat mapping workbook:
   `Run3-ISM-Mapping-Test.xlsx`

## Task

Compare the two supplied workbooks to determine whether Run 3 repeated Run 2 without mapping drift.

This is a validation-only task.

Do **not** re-run the ISM mapping task.

Do **not** inspect, parse, OCR, read, search, or re-extract the original Doco.

Do **not** use external ISM knowledge.

Do **not** use web search.

Do **not** use memory, prior outputs, assumptions, or any files other than the two supplied workbooks.

Use only the two supplied workbooks as evidence.

---

## Repeatability comparison scope

Treat the following worksheets as **stable mapping artefact worksheets**:

* `Section Eligibility Ledger`
* `Evidence Cluster Ledger`
* `Candidate Assessment Ledger`
* `ISM Action Register`
* `Optional Review Candidates`
* `Malformed References`
* `Reader Guide`, if present

For stable mapping artefact worksheets, any non-formatting cell-value difference is potential drift and must be reported.

Treat the following worksheets as **contextual validation / baseline-assessment worksheets**:

* `Validation Summary`
* `Repeatability Audit`

For `Validation Summary` and `Repeatability Audit`, baseline-context rows may differ between the baseline run and the repeat run because Run 2 may not have had a supplied baseline workbook, while Run 3 may have used Run 2 as its supplied baseline workbook.

Do **not** treat baseline-context wording differences as mapping drift where the stable mapping artefact worksheets are identical and these reported hashes match:

* Section inventory hash
* Eligible section inventory hash
* Evidence cluster inventory hash
* Candidate pool hash
* Action decision hash
* Mapping equivalence hash

Baseline-context rows may describe:

* whether a mapping baseline workbook was supplied;
* whether cross-run checks were assessed;
* whether Run 3 matched the supplied baseline workbook;
* whether RAC-038 to RAC-045 were assessed;
* whether cross-run repeatability checks passed.

These rows are contextual metadata unless they contradict the stable artefact worksheets or critical hashes.

---

## Comparison method

### 1. Inspect workbook structure

Confirm both workbooks contain the same relevant worksheets.

Identify:

* missing worksheets;
* extra worksheets;
* renamed worksheets;
* worksheets present in one workbook but not the other.

Classify missing required stable artefact worksheets as `Critical repeatability failure`.

Classify missing optional worksheets according to their role and impact.

---

### 2. Inspect workbook package health

Inspect workbook structure and OpenXML package health where tool-supported.

Check whether:

* each worksheet has valid used-range dimension metadata;
* worksheet dimension refs match the final populated range;
* worksheet dimension refs are not missing, stale, empty, header-only, or incorrect;
* workbook relationships use valid OpenXML relationship targets;
* workbook relationship targets do not contain unsafe leading-slash targets where relative targets are required;
* worksheet tables, if present, have valid refs matching populated ranges;
* AutoFilter refs, if present, match populated ranges;
* table refs and AutoFilter refs do not extend beyond populated rows;
* table refs and AutoFilter refs do not exclude populated rows;
* workbook package structure appears valid and openable.

Classify workbook package issues as `Workbook package hygiene issue` unless they alter cell values, row counts, hashes, worksheet presence, or stable artefact content.

If package issues prevent reliable workbook comparison, classify as `Critical repeatability failure`.

---

### 3. Compare worksheets row-by-row and column-by-column

Compare the following worksheets:

* `Section Eligibility Ledger`
* `Evidence Cluster Ledger`
* `Candidate Assessment Ledger`
* `ISM Action Register`
* `Optional Review Candidates`
* `Malformed References`
* `Validation Summary`
* `Repeatability Audit`
* `Reader Guide`, if present

For each worksheet:

* compare headers;
* compare row counts;
* compare column counts;
* compare cell values;
* compare row order;
* compare stable row identifiers where present;
* identify added rows;
* identify removed rows;
* identify changed rows;
* identify changed columns.

Ignore pure formatting differences unless the task specifically requires presentation comparison.

Do not ignore content differences merely because hashes match. If a cell-value difference exists, report it and classify it.

---

## Sheet-specific row identifiers

Use sheet-specific row identifiers where available:

* `Section Eligibility Ledger`: `Ledger Row ID`
* `Evidence Cluster Ledger`: `Evidence Cluster ID`
* `Candidate Assessment Ledger`: `Candidate Row ID`
* `ISM Action Register`: `Register Row ID`
* `Optional Review Candidates`: `Optional Candidate Row ID`, `Candidate Row ID`, or equivalent stable row ID
* `Malformed References`: stable row ID if present; otherwise row number
* `Validation Summary`: check name, metric name, field name, or row label
* `Repeatability Audit`: `RAC ID`
* `Reader Guide`: section heading or row number

If a stable row ID exists, compare by stable row ID first and row number second.

Report row-order differences separately from cell-value differences.

A row-order difference is not mapping drift if all stable row IDs, cell values, hashes, and row counts match and the workbook does not define row order as part of the deterministic output.

---

## Critical repeatability artefacts

Treat these as critical repeatability artefacts:

* Section inventory hash
* Eligible section inventory hash
* Evidence cluster inventory hash
* Candidate pool hash
* Action decision hash
* Mapping equivalence hash
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
* Suggested-control audit completeness result
* Cross-run ledger hash matched
* Cross-run action decision hash matched
* Cross-run repeatability checks passed

Where the value is present in both workbooks, compare it directly.

Where Run 2 states that a cross-run baseline was not supplied and Run 3 states that a baseline was supplied and matched, classify the difference as `Expected baseline metadata difference`, provided the stable artefact worksheets and critical stable hashes match.

---

## Difference classifications

Classify each difference as one of:

* `Expected baseline metadata difference`
* `Formatting-only difference`
* `Workbook package hygiene issue`
* `Validation wording / metadata drift`
* `Ledger drift`
* `Evidence cluster drift`
* `Candidate pool drift`
* `Action decision drift`
* `Optional candidate drift`
* `Suggested alternate-control audit drift`
* `Critical repeatability failure`

Use the following classification rules.

### Expected baseline metadata difference

Use this where the difference exists only because Run 3 was supplied with Run 2 as a baseline workbook and Run 2 was not itself supplied with a baseline workbook.

Examples include differences in wording about:

* baseline workbook supplied;
* cross-run checks assessed;
* cross-run ledger hash matched;
* cross-run action decision hash matched;
* mapping equivalence hash matched;
* cross-run repeatability checks passed.

This classification is valid only where stable mapping artefact worksheets and critical stable hashes match.

### Formatting-only difference

Use this for differences that do not affect cell values, row counts, column counts, hashes, worksheet names, or workbook structure.

Examples include:

* styles;
* fills;
* borders;
* fonts;
* column widths;
* row heights;
* number formats that do not alter stored values.

### Workbook package hygiene issue

Use this for workbook-generation or OpenXML packaging issues that do not alter the mapped content.

Examples include:

* missing worksheet dimension refs;
* stale worksheet dimension refs;
* incorrect used-range metadata;
* unsafe leading-slash relationship targets;
* table refs that may cause Excel repair;
* AutoFilter refs that do not match populated ranges;
* workbook XML issues that are repairable without changing cell values.

If the package issue causes data loss, unreadable worksheets, broken rows, missing sheets, or unreliable comparison, classify as `Critical repeatability failure`.

### Validation wording / metadata drift

Use this for wording differences in validation rows that are not purely expected baseline-context differences, and do not alter the stable mapping artefacts.

### Ledger drift

Use this for differences in the `Section Eligibility Ledger`, including:

* changed `Ledger Row ID`;
* changed section number;
* changed canonical section key;
* changed eligibility classification;
* changed eligibility reason;
* changed section evidence hash;
* changed Evidence Cluster Inventory;
* added or removed section rows.

### Evidence cluster drift

Use this for differences in the `Evidence Cluster Ledger`, including:

* changed `Evidence Cluster ID`;
* changed exact evidence text;
* changed evidence text hash;
* changed source offset;
* changed section linkage;
* added or removed evidence cluster rows.

### Candidate pool drift

Use this for differences in the `Candidate Assessment Ledger`, including:

* changed candidate rows;
* changed candidate IDs;
* changed control IDs;
* changed evidence score;
* changed acceptance or rejection decision;
* changed gate result;
* changed candidate discovery method;
* added or removed candidates;
* changed suggested-control audit fields;
* changed suggested-control evidence basis;
* changed suggested-control cap outcome;
* changed suggested-control output destination.

### Action decision drift

Use this for differences in the `ISM Action Register`, including:

* changed register rows;
* changed control IDs;
* changed row type;
* changed action;
* changed finding;
* changed evidence linkage;
* changed existing-control linkage;
* added or removed action rows;
* changed suggested-control rows;
* changed suggested-control trigger phrases;
* changed suggested-control action wording.

### Optional candidate drift

Use this for differences in `Optional Review Candidates`, including:

* changed optional candidate rows;
* changed candidate order;
* changed rank;
* changed control ID;
* changed evidence linkage;
* changed reason;
* changed optional candidate cap behaviour;
* a suggested control in one run becoming an optional review candidate in the other run;
* an optional review candidate in one run becoming a suggested control in the other run.

### Suggested alternate-control audit drift

Use this for differences in alternate-control review fields, including:

* alternate candidate discovery method;
* alternate candidates considered count;
* best rejected alternate control ID;
* best rejected alternate failure reason;
* suggested alternate promoted;
* suggested alternate rejected;
* existing-control linkage;
* same-evidence alternate review completeness.

### Critical repeatability failure

Use this where the difference invalidates repeatability.

Examples include:

* missing required stable artefact worksheet;
* mismatched critical stable hash;
* mismatched critical row count;
* changed stable mapping artefact cell values;
* changed ledger rows;
* changed evidence clusters;
* changed candidate decisions;
* changed action register decisions;
* changed optional candidates;
* changed suggested controls;
* unreliable workbook comparison due to corruption or unreadable package structure.

---

## Difference reporting requirements

For each difference, report:

* workbook sheet;
* row identifier or row number;
* column name;
* Run 2 value;
* Run 3 value;
* classification;
* likely cause;
* whether it affects repeatability;
* recommended prompt or process fix.

Where many repeated differences have the same cause, group them together and provide representative examples, but still state the total number of affected rows or cells.

---

## Suggested-control repeatability checks

Compare suggested controls between the two workbooks.

Confirm:

* same number of suggested controls;
* same suggested control IDs;
* same section references;
* same `Ledger Row ID` values;
* same `Evidence Cluster ID` values;
* same candidate row IDs where applicable;
* same trigger phrases;
* same action wording;
* same evidence scores;
* same candidate source pass values;
* same statement-level precision results;
* same gate results;
* same accepted/rejected decisions;
* same output destinations;
* same section-cap outcomes;
* same evidence-cluster-cap outcomes;
* same final `ISM Action Register` suggested-control rows;
* same supporting accepted candidate rows in the `Candidate Assessment Ledger`.

For every `Suggested control` row in the `ISM Action Register`, verify that there is a matching accepted candidate row in the `Candidate Assessment Ledger` with:

* same control ID;
* same `Ledger Row ID`;
* same `Evidence Cluster ID`;
* same section reference;
* `Evidence Score = 3`;
* accepted decision or equivalent final inclusion status;
* action-register output destination or equivalent output flag.

For every accepted suggested-control candidate in the `Candidate Assessment Ledger`, verify that there is a matching `Suggested control` row in the `ISM Action Register`.

If a candidate appears as `Suggested control` in one run but is rejected, omitted, downgraded to optional review, or mapped as a different row type in the other run, classify the difference according to where it appears:

* `Candidate pool drift` where the difference is in candidate assessment, evidence score, gate result, rejection reason, accepted/rejected status, or output destination;
* `Action decision drift` where the difference is in the final `ISM Action Register`;
* `Optional candidate drift` where a suggested control became an optional review candidate, or vice versa;
* `Critical repeatability failure` where the difference changes the final mapping result.

---

## Suggested alternate-control audit checks

Pay special attention to alternate-control audit fields.

For every weak existing control, check whether the `Candidate Assessment Ledger` consistently records:

* alternate candidate discovery method;
* alternate candidates considered count;
* best rejected alternate control ID, where applicable;
* best rejected alternate failure reason, where applicable;
* whether a suggested alternate was promoted or defensibly rejected.

Treat the following existing-control findings as weak or alternate-review-relevant where present:

* `Minor relevance concern`
* `Potentially incorrect reference`
* `Insufficient evidence`
* equivalent weak-existing-control wording

For every weak existing-control row, verify that the alternate-control review is traceable to:

* the same `Ledger Row ID`;
* the same `Evidence Cluster ID`;
* the same existing-control row or existing-control reference;
* the same section;
* the same evidence basis.

Report whether weak existing-control alternate coverage is complete in both workbooks.

If Run 2 and Run 3 both produce zero suggested alternates, confirm whether the rejection audit is identical and sufficient.

If suggested alternates appear, confirm that the same controls, same sections, same Evidence Cluster IDs, same existing-control linkages, same findings, and same actions appear in both runs.

---

## Suggested alternate-control repeatability checks

Compare suggested alternate controls between the two workbooks.

Confirm:

* same number of suggested alternate controls;
* same suggested alternate control IDs;
* same section references;
* same `Ledger Row ID` values;
* same `Evidence Cluster ID` values;
* same existing-control linkages;
* same trigger phrases;
* same action wording;
* same quality findings;
* same candidate assessment decisions.

Classify any difference as `Suggested alternate-control audit drift` or `Action decision drift`, depending on whether the difference is in audit evidence or the final action register.

---

## Optional Review Candidate drift and cap checks

Check Optional Review Candidate repeatability and cap behaviour.

Compare:

* `Optional Review Candidates` worksheet row count;
* optional candidates before global cap;
* optional candidates omitted due to cap;
* optional candidate global cap;
* optional candidate stable row IDs, where present;
* optional candidate rank/order;
* optional candidate control IDs;
* optional candidate section links;
* optional candidate `Ledger Row ID` values;
* optional candidate `Evidence Cluster ID` values;
* optional candidate reasons;
* optional candidate action-register gate not met;
* optional candidate output destination or equivalent flag;
* matching source rows in the `Candidate Assessment Ledger`, where available;
* whether the matching source candidate has `Evidence Score = 2`, where that field is present;
* whether any candidate moved between `Suggested control` and `Optional Review Candidate`; and
* whether any candidate moved between `Suggested alternate control` and `Optional Review Candidate`.

Confirm whether optional candidate ordering is deterministic.

If optional candidates are omitted due to cap, verify that the same candidates are retained and omitted in both runs.

Classify differences as `Optional candidate drift` unless they are purely baseline metadata.

If an Optional Review Candidate in one run becomes an ISM Action Register row in the other run, classify the impact according to where the change appears:

* `Candidate pool drift` where the candidate assessment, evidence score, gate result, rejection reason, accepted/rejected status, or output destination changed;
* `Action decision drift` where the final `ISM Action Register` changed;
* `Optional candidate drift` where the optional candidate worksheet changed; and
* `Critical repeatability failure` where the difference changes the final mapping result.

---

## Hash and count consistency checks

Where hashes and counts are present, compare the workbook-reported values.

Also independently confirm counts from worksheet content where practical.

Check consistency between:

* reported Candidate Assessment Ledger row count and actual Candidate Assessment Ledger rows;
* reported ISM Action Register row count and actual ISM Action Register rows;
* reported Optional Review Candidate row count and actual Optional Review Candidate rows;
* reported suggested control count and actual `Suggested control` rows;
* reported suggested alternate count and actual suggested alternate rows;
* reported no-control count and actual `No ISM control identified` rows;
* reported existing-control count and actual existing-control rows;
* reported suggested-control audit completeness result and actual suggested-control candidate/register reconciliation.

If a reported count differs from the actual worksheet count, report it as validation metadata drift or critical repeatability failure depending on impact.

---

## Verdict rules

Return `Repeatability passed` only where:

* all stable mapping artefact worksheets are identical at the cell-value level;
* all critical stable hashes match;
* all critical row counts match;
* no required worksheet is missing;
* no meaningful workbook package hygiene issue is found.

Return `Repeatability passed with metadata-only differences` where:

* all stable mapping artefact worksheets are identical at the cell-value level;
* all critical stable hashes match;
* all critical row counts match;
* differences are limited to expected baseline metadata, validation wording, formatting-only differences, or workbook package hygiene issues that do not affect content.

Return `Repeatability failed` where there is any:

* ledger drift;
* evidence cluster drift;
* candidate pool drift;
* action decision drift;
* optional candidate drift;
* suggested alternate-control audit drift;
* mismatched critical stable hash;
* mismatched critical row count;
* missing required stable worksheet;
* workbook corruption or package issue that prevents reliable comparison.

---

## Output format

Provide the output in the following structure.

### 1. Executive verdict

State one of:

* `Repeatability passed`
* `Repeatability passed with metadata-only differences`
* `Repeatability failed`

Briefly explain the verdict.

### 2. Workbook structure summary

Provide a table showing:

* worksheet name;
* present in Run 2;
* present in Run 3;
* status;
* notes.

### 3. Workbook package health summary

Provide a table showing:

* check performed;
* Run 2 result;
* Run 3 result;
* classification;
* impact;
* recommended fix.

Include this section even if no workbook package issues are found.

### 4. Summary comparison table

Provide a table comparing key counts and hashes from both runs, including:

* Section inventory hash;
* Eligible section inventory hash;
* Evidence cluster inventory hash;
* Candidate pool hash;
* Action decision hash;
* Mapping equivalence hash;
* Candidate Assessment Ledger row count;
* ISM Action Register row count;
* Existing control row count;
* Suggested control row count;
* Suggested alternate control row count;
* No ISM control identified row count;
* Optional Review Candidate row count;
* Optional candidates before cap;
* Optional candidates omitted due to cap;
* Weak existing-control alternate coverage result;
* Suggested-control audit completeness result;
* Optional Review Candidate drift result, where present;
* Cross-run ledger hash matched;
* Cross-run action decision hash matched;
* Cross-run repeatability checks passed.

### 5. Stable artefact comparison result

State whether each stable mapping artefact worksheet is identical at the cell-value level.

Use a table with:

* worksheet;
* row count Run 2;
* row count Run 3;
* column count Run 2;
* column count Run 3;
* cell-value comparison result;
* classification.

### 6. Drift table

List all non-identical differences, grouped by classification.

For each difference include:

* workbook sheet;
* row identifier or row number;
* column name;
* Run 2 value;
* Run 3 value;
* classification;
* likely cause;
* whether it affects repeatability;
* recommended prompt or process fix.

If no differences exist for a classification, state `None found`.

### 7A. Suggested controls audit result

Report:

* number of suggested-control rows in each run;
* whether the suggested-control row count matches the reported validation count in each workbook;
* whether every suggested-control row in the `ISM Action Register` has a matching accepted row in the `Candidate Assessment Ledger`;
* whether every accepted suggested-control candidate in the `Candidate Assessment Ledger` has a matching row in the `ISM Action Register`;
* whether every suggested-control row has `Evidence Score = 3`;
* whether suggested-control rows use the same `Ledger Row ID` values in both runs;
* whether suggested-control rows use the same `Evidence Cluster ID` values in both runs;
* whether suggested-control rows use the same ISM control IDs in both runs;
* whether suggested-control trigger phrases match in both runs;
* whether suggested-control action wording matches in both runs;
* whether suggested-control evidence linkages match in both runs;
* whether suggested-control candidate source pass values match in both runs;
* whether suggested-control statement-level precision results match in both runs;
* whether suggested-control gate results match in both runs;
* whether suggested-control section-cap outcomes match in both runs;
* whether suggested-control evidence-cluster-cap outcomes match in both runs;
* whether any suggested-control candidate was inconsistently accepted, rejected, downgraded to optional review, or omitted between runs;
* whether any suggested-control drift exists.

Classify suggested-control differences as:

* `Candidate pool drift` where the difference is in candidate assessment, evidence score, gate result, rejection reason, accepted/rejected status, or output destination;
* `Action decision drift` where the difference is in the final `ISM Action Register`;
* `Optional candidate drift` where a Run 2 suggested control became a Run 3 Optional Review Candidate, or vice versa;
* `Critical repeatability failure` where the difference changes the final mapping result.

### 7B. Suggested alternate-control audit result

Report:

* number of existing-control rows in each run;
* number of weak existing-control rows in each run;
* number of suggested alternate-control rows in each run;
* whether every weak existing control has complete alternate review audit fields;
* whether every promoted suggested alternate has a matching accepted row in the `Candidate Assessment Ledger`;
* whether every accepted suggested alternate candidate in the `Candidate Assessment Ledger` has a matching row in the `ISM Action Register`;
* whether suggested alternate rows use the same `Ledger Row ID` values in both runs;
* whether suggested alternate rows use the same `Evidence Cluster ID` values in both runs;
* whether suggested alternate rows use the same existing-control linkages in both runs;
* whether suggested alternate control IDs match in both runs;
* whether suggested alternate trigger phrases match in both runs;
* whether suggested alternate action wording matches in both runs;
* whether suggested alternate quality findings match in both runs;
* whether promoted alternates match;
* whether rejected alternates match;
* whether any alternate-control audit drift exists.

Classify suggested alternate-control differences as:

* `Suggested alternate-control audit drift` where the difference is in alternate review evidence, alternate candidate discovery method, alternate candidate count, rejected alternate details, existing-control linkage, or same-evidence alternate review completeness;
* `Candidate pool drift` where the difference is in candidate assessment, evidence score, gate result, accepted/rejected status, rejection reason, or output destination;
* `Action decision drift` where the difference is in the final `ISM Action Register`;
* `Optional candidate drift` where a suggested alternate became an Optional Review Candidate, or vice versa;
* `Critical repeatability failure` where the difference changes the final mapping result.

### 7C. Optional Review Candidate drift result

Report:

* number of Optional Review Candidate rows in each run;
* Optional Review Candidates before global cap in each run;
* Optional Review Candidates omitted by global cap in each run;
* optional candidate global cap in each run, where reported;
* whether the `Optional Review Candidates` worksheet is cell-value identical;
* whether optional candidate rank/order is identical;
* whether optional candidate stable row IDs match, where present;
* whether retained and omitted candidates match where a cap was applied;
* whether any candidate moved between `Suggested control` and `Optional Review Candidate`;
* whether any candidate moved between `Suggested alternate control` and `Optional Review Candidate`;
* whether every Optional Review Candidate maps back to a matching `Candidate Assessment Ledger` row, where available;
* whether every matching source candidate has `Evidence Score = 2`, where that field is present;
* whether optional candidate reasons match;
* whether optional candidate `Ledger Row ID` values match;
* whether optional candidate `Evidence Cluster ID` values match;
* whether optional candidate section links match;
* whether optional candidate action-register gate-not-met fields match;
* whether optional candidate cap behaviour is deterministic;
* whether any Optional Review Candidate drift exists.

Classify Optional Review Candidate differences as:

* `Optional candidate drift` where optional candidate rows, order, rank, control IDs, reasons, evidence links, section links, cap behaviour, or gate-not-met fields differ;
* `Candidate pool drift` where the source `Candidate Assessment Ledger` row differs;
* `Action decision drift` where a candidate changes the final `ISM Action Register` output;
* `Critical repeatability failure` where the difference changes the final mapping result.

### 8. Root-cause analysis

Explain the likely cause of any meaningful drift.

If differences are metadata-only, explain why they are expected and why they do not affect repeatability.

If workbook package hygiene issues are found, explain whether they are workbook-generation issues rather than mapping drift.

### 9. Recommendations

Provide specific recommendations to improve `ISM-Mapper-Prompt.txt` or the workbook generation process, if needed.

Separate recommendations into:

* mapping repeatability fixes;
* validation wording fixes;
* workbook package hygiene fixes;
* optional improvements.

If no prompt change is required, state that clearly.

### 10. Final acceptance statement

State:

* whether Run 3 can be accepted as repeatable against Run 2;
* whether further prompt changes are required;
* whether workbook-generation hardening is recommended;
* whether any issue found affects the ISM mapping result.
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
