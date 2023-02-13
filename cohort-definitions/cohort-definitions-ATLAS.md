---
title: "Phenotype Phebruary 2023"
author: "Mirza S. Khan"
date: 2023-02-11
header-includes:
  - \usepackage{enumitem}
  - \setlistdepth{20}
  - \renewlist{itemize}{itemize}{20}
  - \renewlist{enumerate}{enumerate}{20}
  - \setlist[itemize]{label=$\cdot$}
  - \setlist[itemize,1]{label=\textbullet}
  - \setlist[itemize,2]{label=--}
  - \setlist[itemize,3]{label=*}
---

# Definition 1: STEMI Diagnosis Codes

## Option 1: `01a-STEMI-Dx-codes.json`

- ATLAS Demo ID: 1781797

- Cohort Entry Events
  
  - Condition Occurrence of: STEMI Diagnosis Codes

- Inclusion Criteria:
  
  - age > 18 years old
  - First diagnosis of STEMI

## Option 2: `01b-ECG-Trop-Dx-code.json`

- ATLAS Demo ID: 1781764

- Cohort Entry
  
  - Troponin measured
  - ECG obtained

- Inclusion Criteria
  
  - Age >= 18 yo
  - First time they got a STEMI ICD Code
    - Stipulated 1 day before or 1 day after index event date

# Definition 2: UDMI Definition: `02-UDMI-definition.json`

> The 4th Universal Definition of Myocardial Infarction (UDMI) defines acute ST-elevation myocardial infarction (STEMI) as requiring a rise and/or fall of cardiac troponin (cTn) values (with at least 1 value > 99th percentile) and clinical evidence of ischemia (ie, symptoms, ECG changes, supportive ECG or other imaging findings, or evidence of coronary thrombus).



- :warning: Limitations of the existing definition
  
  - Was unable to add the appropriate unit concept (ng/L or ng/mL) or gender concept for sex-specific cutoff values due to ATLAS demo issues, so this will need to be included.

- Cohort Entry Events
  
  - I'm opting to limit to ED visits
  
  - ST Elevation on ECG
  
  - A troponin lab test was ordered/measured

- Inclusion Criteria
  
  - age > 18 years old
  
  - Troponin value elevation (further description based on lab value detailed in Definition 3)

# Definition 3: Starting with an elevated Troponin (+ ICU Stay): `03-elev-Trop-ICU.json`

- :warning: Limitations of the existing definition
  
  - Was unable to add the appropriate unit concept (ng/L or ng/mL) or gender concept for sex-specific cutoff values due to ATLAS demo issues, so this will need to be included.
  
  - Still need to add ICU stay between 1-2 days 

![](https://ars.els-cdn.com/content/image/1-s2.0-S0735109712042398-gr1.jpg)

- An elevated Troponin can be due to many things, i.e. it should be sensitive for acute STEMI, but not specific. By contrast, ST segment elevation (STE) on ECG can also be due to other causes, but there are also other STEMI-equivalent ECG findings. So I'm assuming that Troponin elevation is more sensitive than STE on ECG.

- "Elevated troponin is a sensitive and specific indication of cardiac myonecrosis, with troponin release from myocytes into the systemic circulation... In and of itself, elevated troponin does not indicate MI (myonecrosis due to ischemia); rather, it is nonspecific relative to the etiology of cardiac myonecrosis." [^trop-elev]

- Cohort Entry Events
  
  - Troponin measurement: `trop-meas.json`
  
  - Emergency Department presentation: `ED-visit.json`

- Inclusion Criteria
  
  - At least 1 troponin lab value elevated above threshold(s) **within 0-1 days of the index start date**
    
    - Troponin flagged as abnormal or elevated
    
    - Unit concept sets
      
      - 258804004 for ng/L
      
      - 258806002 for ng/mL
    
    - High sensitivity cardiac Troponin I [^lee-sexspectrop]
      
      - Unlike for hs-cTnT where all the assays are Roche, there are many different manufacturers making hs-cTnI assays
      
      - Men: hs-cTnI > 34 ng/L
      
      - Women: hs-TnI > 16 ng/L
    
    - High sensitivity cardiac Troponin T (most available assays are from Roche)
      
      - Men: hs-cTnT > 22 ng/L
      
      - Women: hs-cTnT > 14 ng/L
      
      - These cutoffs are those from Roche's 510(k) FDA approval and corroborated by computing the weighted mean for 99th URL for men and women from published studies [^jaha-sexspectrop]
    
    - Cardiac Troponin I (cTnI) [^trop-assays]
      
      - Concept set is in: `cTnI-meas.json`
      
      - >= 30 ng/L
        
        - or cTnI >= 0.03 ng/mL (pay attention to different units)
    
    - Cardiac Troponin T (cTnT) [^trop-assays]
      
      - Concept set is in: `cTnT-meas.json`
      
      - >= 16 ng/L
  
  - ~~ECG ordered → ST elevation or other ST equivalent finding~~
    
    - Include if data is present and deemed of good-ish quality
  
  - Medications: Aspirin + Antiplatelet
    
    - `aspirin-chewable-tabs.json`
    
    - `p2y12-inhibitors.json`
  
  - Cath lab for PCI +/- stent: `pci-w-or-wo-stent.json`
  
  - The above will likely include many NSTEMI cases too. How can we differentiate STEMI vs NSTEMI based on the above care processes?
    
    - STEMI patients commonly have a brief ICU stay.
      
      - "Of 19,507 patients with STEMI treated at 707 hospitals, 82.3% were treated in ICUs, with a median ICU stay of 1 day (interquartile range [IQR]: 1 to 2 days)." [^stemi-icu] This study used data from the Chest Pain-MI Registry.

```r
# hs-Cardiac Troponin T
# Source: Table 1 from https://www.ahajournals.org/doi/10.1161/JAHA.119.015272
pop_hscTnT <- c(479, 616, 104, 545, 524, 2955, 7575, 1374, 1301, 1540)
percfem_hscTnT <- c(.45, .50, .45, .53, .48, .54, .61, .64, .50, .52)
popfem_hscTnT <- pop_hscTnT * percfem_hscTnT
popmen_hscTnT <- pop_hscTnT * (1 - percfem_hscTnT)

# Vector of 99th percentile URL (ng/L) in each study
hscTnT_males <- c(18, 15, 13, 24, 20, 17, 26, 34, 22, 16)
hscTnT_females <- c(8, 10, 11, 14, 13, 11, 15, 24, 14, 12)

# Calculate weighted mean for men
# Weight by the population of men in each study
weighted.mean(hscTnT_males, popmen_hscTnT)
# [1] 22.37575

# Calculate weighted mean for women
# Weight by the population of women in each study
weighted.mean(hscTnT_females, popfem_hscTnT)
# [1] 14.4254
```

```r
# hs-Cardiac Troponin I
# Source: Table 2 from https://www.ahajournals.org/doi/10.1161/JAHA.119.015272
pop_hscTnI <- c(524, 854, 634, 1540, 1120, 524, 647, 500, 503, 565, 653, 524)
percfem_hscTnI <- c(.47, .5, .56, .52, .47, .47, .35, .5, .58, .54, .49, .47)
popfem_hscTnI <- pop_hscTnI * percfem_hscTnI
popmen_hscTnI <- pop_hscTnI * (1 - percfem_hscTnI)

# Vector of 99th percentile URL (ng/L) in each study
hscTnI_males <- c(36, 20, 13, 20, 33, 52, 21, 14, 81, 111, 43, 36)
hscTnI_females <- c(15, 19, 9, 11, 18, 23, 10, 6, 42, 51, 32, 30)

# Calculate weighted mean for men
# Weight by the population of men in each study
weighted.mean(hscTnI_males, popmen_hscTnI)
# [1] 35.33503

# Calculate weighted mean for women
# Weight by the population of women in each study
weighted.mean(hscTnI_females, popfem_hscTnI)
# [1] 20.78497
```

The Dimension Vista assays have _way higher_ URL cutoffs compared to the others… I'm not in favor of using the weighted mean cutoffs for hs-cTnI. Our objective is to be sensitive, so makes sense to use the lower cutoff anyway.

[^trop-elev]: Newby, L. K., Jesse, R. L., Babb, J. D., Christenson, R. H., De Fer, T. M., Diamond, G. A., Fesmire, F. M., Geraci, S. A., Gersh, B. J., Larsen, G. C., Kaul, S., McKay, C. R., Philippides, G. J., Weintraub, W. S., Harrington, R. A., Bhatt, D. L., Anderson, J. L., Bates, E. R., Bridges, C. R., … Wesley, D. J. (2012). ACCF 2012 Expert Consensus Document on Practical Clinical Considerations in the Interpretation of Troponin Elevations. Journal of the American College of Cardiology, 60(23), 2427–2463. https://doi.org/10.1016/j.jacc.2012.08.969

[^lee-sexspectrop]: Lee, K. K., Ferry, A. V., Anand, A., Strachan, F. E., Chapman, A. R., Kimenai, D. M., Meex, S. J. R., Berry, C., Findlay, I., Reid, A., Cruickshank, A., Gray, A., Collinson, P. O., Apple, F. S., McAllister, D. A., Maguire, D., Fox, K. A. A., Newby, D. E., Tuck, C., … Duncan, C. (2019). Sex-Specific Thresholds of High-Sensitivity Troponin in Patients With Suspected Acute Coronary Syndrome. Journal of the American College of Cardiology, 74(16), 2032–2043. https://doi.org/10.1016/j.jacc.2019.07.082

[^jaha-sexspectrop]: Bhatia, P. M., & Daniels, L. B. (2020). Highly Sensitive Cardiac Troponins: The Evidence Behind Sex‐Specific Cutoffs. Journal of the American Heart Association, 9(10). https://doi.org/10.1161/jaha.119.015272

[^trop-assays]: Ungerer, J. P. J., Tate, J. R., & Pretorius, C. J. (2016). Discordance with 3 Cardiac Troponin I and T Assays: Implications for the 99th Percentile Cutoff. Clinical Chemistry, 62(8), 1106–1114. https://doi.org/10.1373/clinchem.2016.255281

[^stemi-icu]: Shavadia, J. S., Chen, A. Y., Fanaroff, A. C., de Lemos, J. A., Kontos, M. C., & Wang, T. Y. (2019). Intensive Care Utilization in Stable Patients With ST-Segment Elevation Myocardial Infarction Treated With Rapid Reperfusion. JACC: Cardiovascular Interventions, 12(8), 709–717. https://doi.org/10.1016/j.jcin.2019.01.230

# Definition 4: Starting with an Elevated Trop (+ ST Elevation on ECG)

- Need to copy the above Cohort Definition, i.e. Definition 3, and:
  
  - Remove ICU Stay criteria
  
  - Add observation of ST elevation on ECG
    
    - Concept set is in `STE-on-ecg.json`

- Issue here is that we may miss some of the STEMI equivalents where there is no ST elevation noted on ECG.
  
  - **This actually would be an interesting finding if turns out to miss a number of true "STEMI" cases and would favor newer nomenclature of occlusion myocardial infarction (OMI), which may be STEMI(+) or STEMI(-).**

# Notes

- I chose not to use a Chest Pain/Angina or other Symptom as Inclusion Criteria because I'm operating under the assumption that I can infer that a patient is presenting with chest pain or angina-equivalent condition by them having an ECG and troponin ordered on presentation.
