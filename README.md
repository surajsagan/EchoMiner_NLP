# EchoMiner

**Rule-based extraction of structured data from semi-structured echocardiography PDF reports**

EchoMiner is a deterministic, rule-based Python pipeline that parses semi-structured echocardiography PDF reports and converts them into a structured, analysis-ready table, with one row per report and one column per field.


## What it extracts

EchoMiner returns **47 columns per report**: 2 patient identifiers (Name, Address) and 45 data fields.

| Domain | Columns |
|---|---|
| Patient details | Name, Address, Age / Gender (one combined field, e.g. `56 / M`) |
| M-mode measurements | AO, LA, RV, L VID d, L VID s, IVS d, IVS s, LVPW d, LVPW s, EDV, ESV, SV, EF, FS |
| Doppler | MV and TV (E and A velocities), AV and PV (Vmax) |
| Findings | 16 free-text sections: Left Ventricle, Left Atrium, Right Ventricle, Right Atrium, Aorta, Pulmonary Artery, IVS, IAS, Mitral Valve, Aortic Valve, Tricuspid Valve, Pulmonary Valve, Pericardium, Colour Doppler, Doppler Study, Others |
| Impressions | IMPRESSION1 to IMPRESSION10, one impression statement per column |

## How it works

1. **Text extraction:** the text of each PDF is read with PyMuPDF.
2. **Report segmentation:** a PDF containing several reports is split into individual reports at each "Echo Technologist" signature line, which closes each report.
3. **Pattern matching:** regular expressions extract each field from each report.
4. **Output:** one pandas DataFrame, with one row per report.

The pipeline is deterministic: the same input always produces the same output.

## Output format

- All values are returned as text. Numeric fields must be converted before analysis.
- Age / Gender, MV and TV are combined fields (for example `56 / M`, `E80, A60`) and must be split before analysis.
- A field not found in a report is left empty.

## Scope and limitations

- EchoMiner was written for the echocardiography report template of a single institution. Reports in other formats need the segmentation marker and extraction patterns adapted.
- Doppler values are assigned by their order within the Doppler section (first E/A pair to MV, second to TV; first Vmax to AV, second to PV). Reports listing valves in a different order will be misassigned.
- Extraction accuracy depends on report formatting. Check outputs against a sample of source reports before using them in research.

## Requirements

- Python 3
- [PyMuPDF](https://pymupdf.readthedocs.io/)
- [pandas](https://pandas.pydata.org/)
- [openpyxl](https://openpyxl.readthedocs.io/) (only for exporting to Excel)

## Usage

The script provides one function, `extract_echo_data()`, which takes the path to a single PDF and returns a DataFrame.

```python
from EchoMiner import extract_echo_data

df = extract_echo_data("reports.pdf")
df.to_csv("echo_extracted.csv", index=False)
# or: df.to_excel("echo_extracted.xlsx", index=False)
```

To process a folder of PDFs:

```python
from pathlib import Path
import pandas as pd
from EchoMiner import extract_echo_data

frames = [extract_echo_data(str(p)) for p in sorted(Path("reports").glob("*.pdf"))]
pd.concat(frames, ignore_index=True).to_csv("echo_extracted.csv", index=False)
```

## Data privacy

This repository contains source code only. It contains **no echocardiography reports, patient data, or extracted datasets**. EchoMiner's output includes patient names and addresses; handle it under the applicable ethics approval and data-protection rules.
## Data privacy

This repository contains source code only. It contains **no echocardiography reports, patient data, or extracted datasets**.

## Archived version and citation

An archived, citable version (v1) is available on Zenodo:
**https://doi.org/10.5281/zenodo.21281483**

If you refer to this software, please cite:

> B Manjunath, S. (2026). *EchoMiner: source code for rule-based NLP extraction from echocardiography PDF reports* [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.21281483

## Authors

**Suraj B Manjunath**, Senior Research Fellow, Department of Community Medicine, JSS Medical College, JSS Academy of Higher Education and Research (JSS AHER), Mysuru, India

**Project leader:** Dr. Madhu B, Department of Community Medicine, JSS Medical College, JSS AHER, Mysuru, India

Developed as part of doctoral research under the DBT-BUILDER Initiative.

## Copyright and terms of use

Copyright © 2026 Suraj B Manjunath and co-authors, JSS Medical College and Hospital, JSS Academy of Higher Education and Research, Mysuru, India. **All rights reserved.**

This code is made publicly visible for transparency and peer review only. No permission is granted to use, copy, modify, or distribute it, in whole or in part, without prior written permission from the authors.

## Contact

For permissions or questions: **surajbm@jssuni.edu.in**
