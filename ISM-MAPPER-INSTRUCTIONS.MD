## ISM Mapper Workflow
Read the [readme first](https://github.com/itiligent/ISM-Mapper/blob/main/Readme.md)

Download [Promptname.txt](https://raw.githubusercontent.com/itiligent/ai/refs/heads/main/ISM-Control-Discovery-Prompt.txt) 

Download [your preffered ISM version (in JSON format):](https://github.com/AustralianCyberSecurityCentre/ism-oscal/releases)

For controlled and repeatable mapping, the prompt files's `Operating mode` is set differenty for each step:

```text
Step 1: Operating mode = `Section ledger only`

Step 2: Operating mode = `Mapping from supplied frozen ledger`

(A 3rd `Full run` ad-hoc operating mode exists, see bottom.)
```

---


## Step 1: Create A Frozen Document Ledger

Edit the downloaded prompt file setting:

```text
Operating mode = `Section ledger only`
```

Start a new chat.

Attach only:

```text
1. Promptname.txt
2. The document you want to scan (DOCX/PDF)
3. The ASD OSCAL ISM catalog JSON/YAML
```

Use this message:

```text
Run Promptname.txt as a prompt.

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

## Step 2: Map ISM Controls From The Frozen Document Ledger

Before running the prompt, in `ISM-Control-Discovery-Prompt.txt` set:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Start a new chat.

Attach only:

```text
1.  Promptname.txt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Step 1
3. Run1-Frozen-Section-Ledger.xlsx
* Do not attach the original Doco *
```

Use this message:

```text
Run  Promptname.txt as a prompt.

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



### Ad-Hoc Use

`Operating mode = Full run` performs extraction, ledger creation and ISM mapping in one pass. Use this where stability and reviewability are less important.
