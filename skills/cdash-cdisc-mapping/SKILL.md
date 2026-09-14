---
name: cdash-cdisc-mapping
description: Map clinical-trial CRF questions, specifications, and Medidata Rave Form OIDs/Field OIDs to defensible CDASH and CDISC SDTM metadata. Use this skill whenever the user asks what a Field OID, variable name, SDTM domain, SDTM variable, TESTCD/TEST, controlled term, result variable, or RECIST oncology mapping should be—even if they only paste a question label or screenshot. Also use it to review CRF specifications and explain why a proposed mapping is or is not standard.
---

# CDASH + CDISC Mapping

Produce traceable mapping recommendations for clinical data collection and submission metadata. Treat an EDC Field OID and an SDTM variable as different layers: they may resemble each other, but one does not automatically determine the other.

## Source priority

Use evidence in this order:

1. The sponsor's approved standards library, naming convention, and existing study metadata.
2. Protocol, SAP, imaging charter, lab manual, and CRF completion guidelines.
3. The study's declared CDASHIG, SDTMIG, and controlled terminology versions.
4. Current official CDISC Library, CDISC Knowledge Base, NCI EVS terminology, and published CDISC examples.
5. A clearly labeled recommendation or inference when no standard answer exists.

Do not silently replace an established sponsor convention with a preferred convention. State the standard mapping and the sponsor-specific implementation separately when they differ.

## Required context

Before selecting a Field OID, extract or ask for the following when missing and material:

- Exact CRF question and field label
- Form OID and form name
- Intended SDTM domain
- Whether the value is collected, derived, or operational only
- Data type, unit, codelist, and cardinality
- Parent or leading question and display condition
- Whether the form is a single record or repeating log
- Relevant assessor, method, time point, category, and subcategory
- Sponsor naming rules and the applicable CDISC versions

A one-step answer is acceptable when the supplied context makes the result unambiguous. Otherwise, identify the missing discriminator instead of guessing.

## Mapping workflow

### 1. Establish the collection concept

Rewrite the question as a precise concept:

- What entity is observed?
- What property or event is collected?
- At what grain: subject, visit, assessment, specimen, lesion, treatment, or log record?
- Is the answer an indicator, count, date, text, category, measurement, or identifier?

Do not infer a count from an indicator or treat a displayed label as the stored concept.

### 2. Use the form context

Inspect the Form OID before constructing a Field OID. A domain-bearing Form OID is strong context.

Examples:

- A field on `TRNLS` belongs to a TR new-lesion collection context.
- A superficially similar question on an RS response form may represent a classification rather than a tumor property.
- A question on a TU log may identify individual lesions rather than report an aggregate result.

The form context can disambiguate the same wording across TU, TR, and RS.

### 3. Determine the SDTM representation

Classify the target as one of:

- Direct SDTM variable, such as `AETERM` or `TRDTC`
- Findings test represented vertically by `--TESTCD`, `--TEST`, and result variables
- Identifier or linkage variable
- Qualifier, category, method, evaluator, time point, or unit
- Sponsor-defined collection field with no direct standard variable
- Derived value that should not be collected or directly submitted

For vertical findings domains, do not invent a column named after the concept when the standard representation is a fixed test plus a result:

```text
TRTESTCD = LESNUM
TRTEST   = Number of Lesions
TRORRES  = collected value
TRSTRESN = standardized numeric value
```

### 4. Check controlled terminology

Verify whether a proposed TESTCD, test name, or response is:

- In the applicable NCI/CDISC codelist
- In an extensible or non-extensible codelist
- Current for the study's frozen terminology version
- A synonym rather than the CDISC Submission Value

Cite the official source when web or library access is available. State the version or access date. Do not present a plausible abbreviation as an official term without verification.

### 5. Construct or assess the Rave Field OID

First reuse the sponsor's established OID pattern. When no pattern is supplied, prefer an OID that is:

- Stable and concept-based rather than question-text-based
- Consistent with the form/domain context
- Unambiguous about indicator versus count, date, result, or identifier
- Within the sponsor's length and character restrictions
- Compatible with downstream extracts and reusable checks

For a standard vertical test, a practical Rave Field OID may combine the domain prefix and TESTCD. Label this as an implementation recommendation unless it is explicitly present in CDASH or the sponsor library.

Do not claim that CDISC mandates a Rave Field OID. CDISC standardizes submission concepts and metadata; the EDC implementation OID remains subject to sponsor standards.

### 6. Validate the end-to-end mapping

Check:

- Field OID matches Form OID/domain context
- Data type matches the submitted result variable
- Unit and codelist are appropriate
- Repeating-record design preserves the intended grain
- Parent/child visibility rules do not create contradictory data
- Linkage between TU, TR, RS, or other related domains is possible
- Collected and derived values are clearly separated
- No information is lost during the wide-to-long transformation

## Output format

Use this compact structure unless the user requests a specification table:

### Recommendation

State the recommended Field OID and confidence.

### Mapping

| Layer | Value |
|---|---|
| Form OID | ... |
| Field OID | ... |
| CDASH status | Standard / Recommended sponsor-defined / Derived |
| SDTM domain | ... |
| SDTM variables | ... |
| TESTCD / TEST | ... |
| Data type / unit / codelist | ... |

### Reasoning

Explain which context determined the answer, which parts are official standards, and which are implementation recommendations.

### Validation questions

List only unresolved questions that could materially change the mapping.

## RECIST oncology routing

Read [references/oncology-recist.md](references/oncology-recist.md) whenever the question involves RECIST, iRECIST, imaging response, target lesions, non-target lesions, new lesions, TU, TR, or RS.

## General mapping reference

Read [references/mapping-rules.md](references/mapping-rules.md) when assessing Field OID construction, collected-versus-derived status, findings-domain verticalization, terminology, or uncertainty.

## Guardrails

- Do not map solely from English wording when Form OID or domain context is available.
- Do not confuse a Yes/No new-lesion assessment with the number of lesions.
- Do not equate Form OID, Field OID, CDASH variable, SDTM variable, or TESTCD.
- Do not cite an unofficial blog as the sole authority for a CDISC term.
- Do not invent a definitive answer when sponsor standards or the study's frozen versions are required.
- Correct earlier recommendations explicitly when new context changes the mapping.

## Canonical example

Input:

```text
Form OID: TRNLS
Question: How many new lesions are present?
```

Output:

```text
Recommended Field OID: TRLESNUM
SDTM domain: TR
TRTESTCD: LESNUM
TRTEST: Number of Lesions
TRORRES: collected count
TRSTRESN: standardized numeric count
```

Why: the `TRNLS` form establishes the TR new-lesion context, and `LESNUM` is the controlled test code for Number of Lesions. `TRLESNUM` is a practical domain-prefixed Rave Field OID recommendation; it is not a universal CDISC-mandated EDC OID.
