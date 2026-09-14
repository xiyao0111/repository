# RECIST Oncology Mapping Reference

Use this reference for tumor and lesion data collected under RECIST 1.1 or related oncology response criteria.

## Separate the three domains

| Domain | Purpose | Typical grain |
|---|---|---|
| TU | Identify a tumor, lesion, or disease site | One identification record per lesion/site occurrence |
| TR | Record a property, measurement, or assessment result | One test result per lesion or defined assessment unit and time point |
| RS | Record disease-response classifications | One response classification per category/evaluator/time point |

Do not choose among TU, TR, and RS from the word "lesion" alone. Use the form, question intent, record grain, and downstream mapping.

## New lesions

### Individual identification

When individual new lesions are recorded, represent their identity in TU. Published CDISC examples use identifiers such as:

```text
TUTESTCD = TUMIDENT
TUORRES  = NEW
TULNKID  = NEW1, NEW2, ...
```

Preserve the linkage needed to connect the identified lesion to corresponding TR observations.

### Presence of new lesions

A Yes/No response about whether new lesions are present is an indicator or response assessment, not a count. Do not reuse its OID or test code for "How many new lesions are present?"

Depending on the CRF and response design, a new-lesion assessment may be represented in RS with a response test such as New Lesions. Confirm the applicable RECIST supplement and the study's controlled terminology version before fixing `RSTESTCD` or response values.

### Number of new lesions on a TR form

For this known pattern:

```text
Form OID: TRNLS
Question: How many new lesions are present?
```

recommend:

| Attribute | Mapping |
|---|---|
| Rave Field OID | `TRLESNUM` |
| SDTM domain | `TR` |
| `TRTESTCD` | `LESNUM` |
| `TRTEST` | `Number of Lesions` |
| Original result | `TRORRES` |
| Standard numeric result | `TRSTRESN` |
| Result type | Non-negative integer; apply sponsor rules for whether zero is permitted |

`LESNUM` is the CDISC Submission Value for "Number of Lesions" in the Tumor or Lesion Properties Test Code terminology. "New" is supplied by the form/record context and related classification or linkage; it is not part of the generic test name.

`TRLESNUM` is an implementation recommendation constructed from the domain plus the standard test code. Verify it against the sponsor metadata repository before adoption.

## Data-design checks

For a count plus lesion log:

- Reconcile the count with the number of active new-lesion log records.
- Define whether zero is allowed or the field is blank when the parent indicator is No.
- Clear or inactivate dependent count and lesion records when the parent answer changes to No, according to sponsor data-preservation policy.
- Define whether equivocal findings are entered as new lesions and how later confirmation is handled.
- Keep evaluator/source distinctions, such as investigator versus independent central review.
- Confirm that assessment date, method, anatomical location, and linkage are captured at the correct grain.

## Evidence links

- NCI EVS CDISC SDTM Controlled Terminology archive: https://evs.nci.nih.gov/ftp1/CDISC/SDTM/Archive/
- CDISC Tumor/Lesion Identification and Results eCRF page: https://www.cdisc.org/kb/ecrf/tumorlesion-identification-results
- CDISC Oncology Disease Response Supplements: https://www.cdisc.org/kb/articles/cdisc-published/oncology-disease-response-rs-supplements
- RECIST Working Group: https://recist.eortc.org/recist-1-1/

Always verify the study's frozen standard and controlled-terminology versions.
