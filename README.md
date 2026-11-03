# Clinical EHR Data Engineering and Validation Pipeline

## Project Overview
This clinical data engineering pipeline automates the extraction, triage, normalization, and validation of messy Electronic Health Records (EHR) from inpatient datasets. 

The project focus centers on parsing unstandardized administrative schemas, fixing cross-dataset identifier mismatches, and programmatically auditing complex temporal chronological sequence errors within hospital information systems.

## Methodology & Structural Features

* **Advanced Structural Array Swapping:** Implements an index-independent matrix transformation targeting chronologically inverted records (discharge events logged prior to admission). By converting cross-sections to pure NumPy layouts using `.values`, it executes a simultaneous data-swap across temporal boundaries without risking column contamination.
* **Optical Character Recognition (OCR) Error Triage:** Programmatically traps and converts machine-transcription text artifacts within chronological or integer fields (e.g., character `I` mutating to numerical `1`, and character `O` mutating to numerical `0`) ahead of type coercion down to `datetime64[ns]`.
* **Entity Resolution & ID Unification:** Resolves duplicated structural data records and identifier conflicts using a vectorized mapping key network paired with downstream `.fillna()` recovery fallbacks to preserve dataset integrity.
* **String Standardization & Normalization:** Filters out dirty categorical features (whitespace padding, casing variants, and direct misspellings like *diabtes* or *malariaa*) into clean clinical diagnostic classifications via hash-map translation dictionaries.
* **Defensive Pipeline Engineering:** Uses a dynamic `pd.Series` lookup mapping architecture to isolate valid target features, filter operational artifact columns, and execute signed UTF-8 exports containing unified, snake_case schemas.

## Data Schema & Dictionary Matrix

The data pipeline processes dirty source layers and isolates the following standardized attributes for downstream spatial modeling and epidemiological analytics:

| Target Schema Header | Source Field Derivation | Data Type | Cleansing Protocol Applied |
| :--- | :--- | :--- | :--- |
| `patient_id` | `patientid` / `patientname` | `object` (String) | Cross-checked duplications; mapped structural updates |
| `patient_name` | `patientname` | `object` (String) | Stripped out padding; validated case matches |
| `age` | `age` | `int64` (Integer) | Cleaned character mutations (`I` -> `1`); dropped blanks |
| `medical_condition` | `medicalcondition` | `object` (Categorical) | Corrected multi-variant spelling anomalies and casing |
| `has_insurance_verify`| `hasinsurance_verify` | `category` (Boolean) | Standardized 16-way mixed numeric/text states to Yes/No |
| `admission_date` | `admissiondate` | `datetime64[ns]` | Replaced corrupted literals (`O` -> `0`, `I` -> `1`); parsed via `%d/%m/%Y` |
| `discharge_date` | `dischargedate` | `datetime64[ns]` | Replaced corrupted literals; evaluated mixed timeline vectors |

## Technologies & Libraries Used
* **Language:** Python 3.x
* **Core Data Engineering:** pandas
* **Encoding Format:** UTF-8-BOM (`utf-8-sig`)

## Related Public Health Projects
* **[Malaria Trends Spatiotemporal Pipeline]([https://github.com](https://github.com/kennedyberitah/malaria-trends-kitui-subcounties)** - An automated geospatial epidemiology workflow tracking multi-year malaria incidence dynamics across Kitui County sub-counties using GeoPandas.
