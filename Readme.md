# 🛡️ Document-to-ISM Control Mapping

> A repeatable workflow for discovering evidence and mapping Australian Government Information Security Manual (ISM) controls from source documents.

[![Workflow](https://img.shields.io/badge/workflow-frozen%20ledger%20mapping-blue)](#-deterministic-mapping)
[![Evidence](https://img.shields.io/badge/evidence-traceable-success)](#-evidence-ledgers)
[![Output](https://img.shields.io/badge/output-XLSX-orange)](#-control-recommendation-types)
[![Method](https://img.shields.io/badge/method-repeatable%20%26%20auditable-purple)](#-main-trade-off)

---

## 🎯 Purpose

This project provides a repeatable method for identifying and mapping Australian Government Information Security Manual (ISM) controls from source documents.

The core principle is:

> **Interpret the source document once, freeze the evidence base, then map ISM controls only from that frozen evidence.**

This reduces mapping drift, improves repeatability, and ensures each control recommendation is traceable to a stable evidence record.

---

## 🧭 Deterministic Mapping

This project uses a deterministic mapping approach rather than a purely interpretive one.

In this context, **deterministic** means the same frozen evidence base should produce the same mapping result when the same prompt settings, ISM catalog, and input ledger are used.

To support this, a **frozen ledger** is created before control mapping. The source document is interpreted once, assessed, and then locked as the authoritative evidence base.

Later mapping runs do **not** re-read, re-section, or reinterpret the source document. Instead, they map controls only from the frozen evidence already recorded in the ledger.

This helps to:

* reduce mapping drift between runs;
* make results easier to repeat;
* make differences between runs easier to detect and explain;
* provide a stable audit trail for control recommendations.

The frozen ledger records:

* eligible document sections;
* evidence found in those sections;
* how evidence was grouped;
* stable evidence IDs used during mapping.

---

## 📒 Evidence Ledgers

The project separates **evidence discovery** from **control mapping**.

This separation prevents the control mapping stage from changing the evidence base while recommendations are being produced.

---

### 📄 Section Eligibility Ledger

The **Section Eligibility Ledger** identifies which document sections are suitable for ISM control mapping.

It answers:

> **Is this section suitable for control mapping?**

Eligible sections become part of the controlled evidence base.

---

### 🧩 Evidence Cluster Ledger

The **Evidence Cluster Ledger** records usable evidence found inside eligible sections.

It answers:

> **What evidence supports later control mapping?**

Discovered evidence is grouped into clusters and assigned stable `EC-*` IDs.

These IDs become the fixed references used for mapping, validation, and review.

---

## ✅ Eligibility Triad

Document sections are assessed using an eligibility triad.

A section is considered for mapping when it shows sufficient evidence across the following areas:

| Eligibility area                      | What it means                                                                                                                    |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Section structure**                 | The content belongs to a real document section.                                                                                  |
| **Control-relevant content**          | The content relates to security, governance, operations, risk, access, monitoring, protection, assurance, or related ISM topics. |
| **Actionable or obligation language** | The content includes requirements, responsibilities, commitments, procedures, controls, or design decisions.                     |

The eligibility triad helps prevent controls from being mapped against low-value or non-solution content, such as:

* table of contents entries;
* headers and footers;
* document control pages;
* glossary-only content;
* approval pages;
* comments or annotations;
* reviewer notes;
* isolated code snippets, scripts, YAML files, configuration dumps, or other implementation fragments with no surrounding control context.

---

## 🔎 Direct ISM References

Direct ISM references in the source document are treated as **explicit evidence**, not automatically as suggested mappings.

Where a numbered section contains valid bracketed ISM references, such as:

```text
[ISM-1695]
[ISM-1695, ISM-1696]
```

the section can be included in the frozen ledger even if the normal eligibility triad is incomplete.

This override only applies where no hard exclusion applies.

For example, references found in table of contents entries, document control pages, comments, annotations, or other excluded content do **not** qualify.

---

## 🗺️ How Mapping Works

Control mapping is performed from frozen evidence clusters.

The mapper uses the **Evidence Cluster Ledger** as the authority, not the original document. This means each recommendation can be traced back to a stable `EC-*` evidence ID.

```text
Document section
   ↓
Section Eligibility Ledger
   ↓
Evidence Cluster Ledger
   ↓
Frozen EC-* evidence ID
   ↓
Control recommendation
```

---

## 🧾 Control Recommendation Types

### ✅ Suggested Control

Used when the frozen evidence strongly supports a specific ISM control.

> **High confidence = Suggested control**

---

### 🔁 Suggested Alternate Control

Used when the document references or appears to contain one control, but the evidence better supports another control or an additional control.

> **Better fit than the existing or referenced control = Suggested alternate control**

---

### 🟡 Optional Control Candidate (For Manual Review)

Optional Review Candidates allow plausible but uncertain matches to be surfaced without overstating them as firm recommendations.

This is useful when the evidence suggests a possible ISM relationship, but the confidence level is not high enough for a suggested control.

> **Possible match requiring human review = Optional Review Candidate**

---

## 📊 Confidence Thresholds

| Confidence level                           | Output type                 |
| ------------------------------------------ | --------------------------- |
| High confidence                            | Suggested control           |
| Medium confidence / better-fit alternative | Suggested alternate control |
| Low-to-medium confidence                   | Optional Review Candidate   |
| Low confidence                             | No ISM control identified   |

This prevents weak or uncertain evidence from being presented as a firm recommendation.

---

## ⚙️ Key Tunable Settings

### 📚 ISM Catalog Source

Controls the source of truth for ISM controls.

* Uploaded `ISM_catalog.json` improves repeatability.
* Online ASD OSCAL catalog (YAML) sources.

### 🎚️ Control Output Limits and Output Modes

ISM Mapper applies control output limits after candidate controls have been scored and ranked.

These limits help keep the output focused, reviewable, and repeatable. They do **not** change the evidence score of a candidate control. Instead, they control how many scored candidates are allowed into the final workbook.

---

### 📌 Default Settings

| Setting                              |         Default |
| ------------------------------------ | --------------: |
| Output mode                          | `Full coverage` |
| Optional Review Candidates           |            `On` |
| Optional Review Candidate global cap |            `100` |

---

### 🧭 Output Modes

ISM Mapper supports three output modes.

| Mode                   | Behaviour                                                                                                                                                                                                 | Use case                      |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| `Full coverage strict` | Every eligible section is represented in the ISM Action Register. Suggested controls and alternates are included only where they pass high-confidence gates. Optional Review Candidates are not produced. | Assurance-focused review      |
| `Full coverage`        | Every eligible section is represented. Suggested controls, suggested alternates, and optional review candidates may be produced where supported by evidence.                                              | Default balanced review       |
| `Mapped controls only` | Only rows with existing controls, suggested controls, or suggested alternates are included. Sections with no clear mapping are omitted from the action register but still counted in validation.          | Concise mapped-control output |

`Full coverage` and `Full coverage strict` prevent eligible sections from silently disappearing by requiring every eligible section to have an explicit action-register outcome.

`Mapped controls only` produces a shorter workbook, but it is less useful for checking which eligible sections had no clear ISM mapping.

---

### 🧭 Full Coverage Mode

`Full coverage` is the default balanced output mode.

It requires every eligible solution section to be represented in the ISM Action Register, even where no clear ISM control can be mapped.

This prevents eligible sections from silently disappearing from the output and improves auditability because every eligible section has an explicit outcome.

In this mode:

* existing ISM references are preserved and reviewed;
* suggested controls are included where the evidence passes the required precision gates;
* suggested alternate controls are included where the evidence supports a better-fitting or additional control;
* optional review candidates may be produced outside the ISM Action Register;
* eligible sections with no clear mapping receive a `No ISM control identified` row.

> `Full coverage` does not mean “suggest as many controls as possible”.
> It means every eligible section must be accounted for.

---

### ✅ Suggested Control Limits

Suggested controls are used where the frozen evidence strongly supports a specific ISM control.

| Limit                                           | Default |
| ----------------------------------------------- | ------: |
| Maximum Suggested Controls per section          |     `2` |
| Maximum Suggested Controls per evidence cluster |     `1` |

A second suggested control may only be included where it is supported by a separate evidence cluster with a separate obligation, security activity, or control outcome.

This prevents multiple controls from being mapped to the same evidence unless there is a clear evidence basis.

---

### 🔁 Suggested Alternate Control Limits

Suggested alternate controls are used where the document references or appears to contain one control, but the evidence better supports another control or an additional control.

| Limit                                                       | Default |
| ----------------------------------------------------------- | ------: |
| Maximum Suggested Alternates per existing control reference |     `1` |
| Maximum Suggested Alternates per section                    |     `2` |
| Maximum Suggested Alternates per evidence cluster           |     `1` |

Alternates are capped so they remain targeted and defensible rather than becoming broad speculative mappings.

---

### 🟡 Optional Review Candidate Limits

Optional Review Candidates are used where there is plausible evidence, but not enough confidence for a firm recommendation.

| Limit                                                                 |        Default |
| --------------------------------------------------------------------- | -------------: |
| Optional Review Candidates per section                                | `1` by default |
| Optional Review Candidates per section where separate evidence exists |      Up to `2` |
| Global Optional Review Candidate cap                                  |           `20` |

A second Optional Review Candidate may only be included where it is supported by a clearly separate Evidence Cluster ID and a different implementation obligation or control outcome.

This allows useful discovery items to be surfaced without overstating uncertain matches as firm recommendations.

---

### 🧮 How Limits Are Applied

Control limits are applied **after scoring**, not during scoring.

```text
Frozen evidence
   ↓
Candidate controls identified
   ↓
Candidate controls scored
   ↓
Candidates ranked by evidence strength and fit
   ↓
Output limits applied
   ↓
Final workbook rows created
```

The mapper should not increase or reduce a candidate score to satisfy a limit.

If a section or evidence cluster reaches its configured limit, lower-ranked candidates are excluded from the output.

---

### 🏆 Default Priority Order

When output limits are reached, candidates are prioritised in this order:

1. **Suggested Controls** with strong evidence fit;
2. **Suggested Alternate Controls** where the alternate is a better or additional fit;
3. **Optional Review Candidates** with plausible but uncertain evidence;
4. weak, repetitive, or low-confidence candidates are excluded.

This keeps the output useful for assurance review while still allowing controlled discovery coverage.

---

### ⚖️ Output Limit Trade-Off

> **Stricter settings improve precision and repeatability.**
> **Looser settings improve coverage, but create more optional review items.**

For assurance-focused reviews, stricter settings are usually preferred.

For discovery-focused reviews, looser settings may be useful, provided optional candidates remain clearly separated from firm recommendations.

