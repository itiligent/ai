# 🛡️ ISM Control Discovery and Evidence Mapping

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

---

### 🎚️ Control Output Limits

Caps can be applied per section or evidence cluster.

* Lower volume settings apply stricter selection pressure for fewer, stronger matches.
* Higher volume produces broader coverage, but increases review effort.
* Enable/Disable Optional Control Candidates  

### ⚖️ Output Limit Trade-Off

> **Stricter settings improve precision and repeatability.**
> **Looser settings improve coverage, but create more optional review items.**

---

