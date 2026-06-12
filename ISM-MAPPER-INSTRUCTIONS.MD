## 🧭 ISM Mapper Workflow

> Create a frozen evidence ledger first, then map ISM controls from that fixed ledger.

[![Workflow](https://img.shields.io/badge/workflow-frozen%20ledger%20%2B%20mapping-blue)](#-ism-mapper-workflow)
[![Output](https://img.shields.io/badge/output-XLSX-success)](#-workflow-outputs)
[![Source](https://img.shields.io/badge/source-ASD%20OSCAL%20ISM-orange)](https://github.com/AustralianCyberSecurityCentre/ism-oscal/releases)

---

## 📘 Before You Start

Read the [README first](https://github.com/itiligent/ISM-Mapper/blob/main/Readme.md).

Download the required files:

| File                                     | Source                                                                                                             |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `ISM-Mapper-Prompt.txt`                  | [Download prompt](https://raw.githubusercontent.com/itiligent/ai/refs/heads/main/ISM-Control-Discovery-Prompt.txt) |
| `ISM_catalog.json` or `ISM_catalog.yaml` | [ASD OSCAL ISM releases](https://github.com/AustralianCyberSecurityCentre/ism-oscal/releases)                      |
| Source document                          | The DOCX or PDF you want to assess                                                                                 |



---

## 🧊 Step 1: Create a Frozen Document Ledger

This step reads the source document once and creates a fixed evidence base for later control mapping.

### 1. Set the operating mode

Edit `ISM-Mapper-Prompt.txt` and set:

```text
Operating mode = `Section ledger only`
```

### 2. Start a new chat

Attach only:

```text
1. ISM-Mapper-Prompt.txt
2. The document you want to scan, such as DOCX or PDF
3. The ASD OSCAL ISM catalog JSON or YAML
```

### 3. Paste this message

```text
Run ISM-Mapper-Prompt.txt as a prompt.

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

### 4. Save the output

Download the workbook and save it as:

```text
Run1-Frozen-Section-Ledger.xlsx
```

---

## 🗺️ Step 2: Map ISM Controls from the Frozen Ledger

This step maps ISM controls using only the frozen ledger created in Step 1.

The original document must not be re-attached or re-read.

### 1. Set the operating mode

Edit `ISM-Mapper-Prompt.txt` and set:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

### 2. Start a new chat

Attach only:

```text
1. ISM-Mapper-Prompt.txt
2. The same ASD OSCAL ISM catalog JSON or YAML used in Step 1
3. Run1-Frozen-Section-Ledger.xlsx
```

> ⚠️ Do not attach the original source document in Step 2.

### 3. Paste this message

```text
Run ISM-Mapper-Prompt.txt as a prompt.

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

### 4. Save the output

Download the ISM mapping workbook and save it as:

```text
Run2-ISM-Mapping-Output.xlsx
```

---

## 📦 Workflow Outputs

| Step   | Output Workbook                   | Purpose                                                       |
| ------ | --------------------------------- | ------------------------------------------------------------- |
| Step 1 | `Run1-Frozen-Section-Ledger.xlsx` | Frozen Section Eligibility Ledger and Evidence Cluster Ledger |
| Step 2 | `Run2-ISM-Mapping-Output.xlsx`    | ISM control recommendations mapped from the frozen ledger     |

---

## ✅ Important Rules

* Use the same ISM catalog file in both steps.
* Do not attach the original document during Step 2.
* Do not rebuild or reinterpret the frozen ledger during mapping.
* Keep all `SEL-*` Ledger Row IDs unchanged.
* Keep all `EC-*` Evidence Cluster IDs unchanged.
* Treat `Run1-Frozen-Section-Ledger.xlsx` as the authoritative evidence base for Step 2.
