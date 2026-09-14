# Mapping Rules Reference

## Metadata layers

Keep these layers distinct:

| Layer | Meaning |
|---|---|
| CRF question | User-facing prompt |
| Field label | Short display or export label |
| Rave Field OID | Sponsor-controlled implementation identifier |
| CDASH variable | Standardized collection metadata where defined |
| SDTM domain variable | Submission dataset column |
| TESTCD / TEST | Vertical findings concept |
| ORRES / STRESC / STRESN | Original and standardized results |
| Controlled term | Permitted submission value from a codelist |

A familiar-looking label does not prove that all layers share the same name.

## Direct-variable pattern

Use a direct domain variable when the collection concept corresponds to an SDTM column, subject to CDASH guidance and sponsor standards.

Example:

```text
Adverse event verbatim term
Field OID candidate: AETERM
SDTM variable: AETERM
```

Even here, verify the sponsor library rather than assuming.

## Vertical findings pattern

Many findings concepts become rows instead of columns.

Example:

```text
Collected field: Number of Lesions
Domain: TR
TRTESTCD = LESNUM
TRTEST   = Number of Lesions
TRORRES  = source value
TRSTRESN = standardized number
```

The Field OID can be test-specific, such as `TRLESNUM`, while the submission dataset stores the value in result variables.

## Indicator versus count

Treat these as separate concepts:

| Concept | Example answer |
|---|---|
| Occurrence/presence indicator | Yes / No |
| Count | 0, 1, 2, 3 |
| Identifier | NEW1 |
| Classification | NEW |
| Measurement | 18 mm |

Do not select an OID for one concept because its wording resembles another.

## OID construction when no sponsor standard is supplied

1. Start with the established domain/form context.
2. Reuse an official variable or TESTCD token when it accurately names the concept.
3. Add a concise discriminator only when needed.
4. Avoid abbreviations that can mean both number and indicator.
5. Keep the OID stable if the displayed question changes.
6. State that the OID is recommended sponsor-defined metadata unless an official source explicitly defines it.

## Dates

Identify what the date represents before selecting `--DTC` or a more specific timing variable:

- Assessment performed
- Specimen collection
- Event start or end
- Treatment administration
- Record entry

Do not map a date based only on the nearest form title.

## Controlled terminology

For every coded field, report:

- Codelist short name and NCI code when available
- CDISC Submission Value
- Extensible status
- Study CT version
- Any sponsor extension and justification

A synonym displayed on the CRF may differ from the required submission value.

## Collected versus derived

Mark a value as derived when it can be deterministically calculated from more granular collected data and the protocol does not require independent confirmation.

When both a summary and granular records are collected, specify a reconciliation rule and identify which source is authoritative.

## Confidence labels

Use:

- **High**: official term and sufficient study/form context
- **Moderate**: official target mapping but sponsor OID convention is unavailable
- **Low**: material study context or standards version is missing

Explain what would raise the confidence.
