# EchoMiner

**Rule-based extraction of structured data from semi-structured echocardiography PDF reports**

EchoMiner is a deterministic, rule-based Python pipeline that parses semi-structured echocardiography PDF reports and converts them into a structured, analysis-ready table, with one row per report and one column per field.


## What it extracts

EchoMiner extracts **45 fields per report**:

| Domain | Fields |
|---|---|
| Patient demographics | Identifiers, age, sex |
| M-mode measurements | Aortic root, left atrial dimension, ejection fraction, fractional shortening, and related dimensions |
| Doppler valve velocities | Mitral, tricuspid, aortic, and pulmonary valves |
| Categorical findings | 16 structured findings fields |
| Impressions | Up to 10 free-text impression statements per report |

## How it works

1. **Text extraction:** report text is read from PDF files using PyMuPDF.
2. **Report segmentation:** PDFs containing multiple reports are split into individual reports using the institutional footer markers.
3. **Pattern matching:** regular expressions identify and extract each field.
4. **Output:** results are returned as a pandas DataFrame and can be exported to Excel or CSV for statistical analysis.

The pipeline is deterministic: the same input always produces the same output.

## Scope and limitations

- EchoMiner was developed for the echocardiography report template of a single institution. Reports in other formats will need the segmentation markers and extraction patterns adapted.
- Extraction accuracy depends on report formatting. Outputs should be checked against a sample of source reports before use in research.

## Requirements

- Python 3
- [PyMuPDF](https://pymupdf.readthedocs.io/)
- [pandas](https://pandas.pydata.org/)

## Usage

```bash
python EchoMiner.py
```

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
