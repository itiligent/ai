## ISM Control Mapping Workflow

Use `ISM-Control-Discovery-Prompt.txt` to map ISM controls from a DOCX or PDF document using a controlled two-step process.

The prompt should normally be used in two stages:

```text
Step 1: Create a frozen 'Section Eligibility Ledger' and 'Evidence Cluster Ledger'.
Step 2: Map ISM controls from that frozen ledger.
```

## Why this approach is needed

ISM control mapping can vary significantly if the LLM repeatedly re-extracts and reinterprets the document. Each pass may identify sections, evidence, or control relevance slightly differently.

This workflow avoids that variability by freezing the document structure and evidence ledger before mapping begins.

Once the ledger is frozen, the mapping step uses a fixed evidence source rather than re-reading and reinterpreting the document. This makes mapping output more stable.


## When to create a new frozen ledger

Create a new frozen ledger when:

```text
Your document is being scanned for the first time; or
Your document has changed since the last scan.
```

> [!WARNING] Do not reuse an old frozen ledger for a changed document.

A frozen ledger belongs to one exact version of one document. If the document content, headings, tables, ISM references or security evidence changes, create a new frozen ledger.

## Step 1 — Create the frozen ledger

Start a new chat.

Attach only:

```text
1. ISM-Control-Discovery-Prompt.txt
2. The Doco DOCX/PDF
3. The ASD OSCAL ISM catalog JSON/YAML
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt.txt as a prompt.

Use the attached Doco as `Doco`.

Set:
Operating mode = `Section ledger only`

Create the Excel workbook output. Stop after creating and freezing the Section Eligibility Ledger and Evidence Cluster Ledger. Do not perform mapping, suggested controls, suggested alternate controls or optional review candidates.
```

Download the output and save it as:

```text
Run1_Frozen_Section_Ledger.xlsx
```

Check that the workbook includes:

```text
Section Eligibility Ledger
Evidence Cluster Ledger
Validation Summary
Repeatability Audit
```

## Step 2 — Map ISM controls from the frozen ledger

Start a new chat.

Attach only:

```text
1. ISM-Control-Discovery-Prompt.txt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Step 1
3. Run1_Frozen_Section_Ledger.xlsx
```

Do not attach the original Doco unless only filename confirmation is needed.

Use this message:

```text
Run ISM-Control-Discovery-Prompt.txt as a prompt.

Set:
Operating mode = `Mapping from supplied frozen ledger`

Use `Run1_Frozen_Section_Ledger.xlsx` as the authoritative frozen Section Eligibility Ledger and Evidence Cluster Ledger.

Do not re-extract Doco. Do not rebuild the section ledger. Do not create new Evidence Cluster IDs. Perform the mapping pass only and create the Excel workbook output.
```

Download the output and save it as:

```text
ISM_Mapping_Output.xlsx
```

## Rule for changed documents

If the Doco changes:

```text
Create a new frozen ledger.
Map from the new frozen ledger.
Do not reuse or repair the old frozen ledger.
```

## Difference from ad hoc Full run mode

`Full run` performs extraction, ledger creation and ISM mapping in one pass.

Use `Full run` only for quick, ad hoc analysis where stability and reviewability are less important.

The two-step ledger workflow is better for controlled mapping because it separates document extraction from control mapping. This makes it easier to see exactly which sections were considered eligible, what evidence was used, and why the mapping output was produced.

In simple terms:

```text
Full run = quick one-pass scan and map.

Two-step workflow = controlled scan, frozen evidence base, then mapping from that evidence base.
```

## Quick decision guide

```text
New document = Section ledger only first, then Mapping from supplied frozen ledger.

Changed document = Create a new frozen ledger first.

Same exact document version = Reuse the existing frozen ledger for mapping.

Quick informal scan = Full run.
```
