## ISM Control Mapping Workflow

Using [ISM-Control-Discovery-Prompt.txt](https://raw.githubusercontent.com/itiligent/ai/refs/heads/main/ISM-Control-Discovery-Prompt.txt) to map ISM controls from a DOCX or PDF document is a controlled two-step process.

For controlled and repeatable mapping, the prompt files's `Operating mode` is set differenty for each step:

```text
Step 1: Operating mode = `Section ledger only`

Step 2: Operating mode = `Mapping from supplied frozen ledger`

(A 3rd `Full run` ad-hoc operating mode exists. See bottom.)
```

---

## Why A 2 Step Approach Is Needed

ISM control mapping can vary significantly if the LLM is allowed to re-extract and reinterpret the document while it works. This can cause sections, evidence and control relevance to shift during a single mapping run, reducing consistency.

This 2 step workflow avoids that variability by freezing the document structure and evidence ledger before mapping begins. This makes the mapping output more stable and reviewable.

---


## Step 1: Create A Frozen Section Ledger Index

- Frozen ledger = fixed document map + fixed evidence index
- Freezes the document structure
- Stops the model from re-reading the document differently each time.
- Freezes the eligible document sections
- Locks in which sections are considered relevant for ISM mapping in the second step.

> [!NOTE]
> Do not reuse an old frozen ledger for a changed document, You must create this for each scan

---

Before running the prompt, in `ISM-Control-Discovery-Prompt.txt` set:

```text
Operating mode = `Section ledger only`
```

Start a new chat.

Attach only:

```text
1. ISM-Control-Discovery-Prompt.txt
2. The document you want to scan (DOCX/PDF)
3. The ASD OSCAL ISM catalog JSON/YAML
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt.txt as a prompt.

Use the attached Doco as `Doco`.

Set:
Operating mode = `Section ledger only`

Create the Excel workbook output and save it as:

`Run1_Frozen_Section_Ledger.xlsx`

Stop after creating and freezing the Section Eligibility Ledger and Evidence Cluster Ledger. Do not perform mapping, suggested controls, suggested alternate controls or optional review candidates.
```

Download the output and save it as:

```text
Run1_Frozen_Section_Ledger.xlsx
```

---

## Step 2: Baseline ISM Controls From The Frozen Section Ledger Index

Before running the prompt, in `ISM-Control-Discovery-Prompt.txt` set:

```text
Operating mode = `Mapping from supplied frozen ledger`
```

Start a new chat.

Attach only:

```text
1. ISM-Control-Discovery-Prompt.txt
2. The same ASD OSCAL ISM catalog JSON/YAML used in Step 1
3. Run1_Frozen_Section_Ledger.xlsx
```

Use this message:

```text
Run ISM-Control-Discovery-Prompt.txt as a prompt.

Set:
Operating mode = `Mapping from supplied frozen ledger`

Use `Run1_Frozen_Section_Ledger.xlsx` as the authoritative frozen Section Eligibility Ledger and Evidence Cluster Ledger.

Do not re-extract Doco. Do not rebuild the section ledger. Do not create new Evidence Cluster IDs.

Perform the mapping pass only and create the Excel workbook output.

Save the workbook as:

`Run2_Mapping_Baseline.xlsx`
```

Download the ISM mapping output and save it as:

```text
Run2_Mapping_Baseline.xlsx
```

---

### Ad-Hoc Use

`Operating mode = Full run` performs extraction, ledger creation and ISM mapping in one pass. Use this where stability and reviewability are less important.
