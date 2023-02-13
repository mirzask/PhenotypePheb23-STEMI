# Concept Sets

- Concept sets for acute STEMI based on ICD code

- Concept set for cardiac catheterization +/- stent (drug-eluting stent or bare metal stent)
  
  - all should occur within 0-1 days of encounter patient (technically within 60-90 mins of encounter start)
  
  - must include left heart cathterization; exclude right heart catheterization _only_
  
  - preceded by the following:
    
    - treated with Aspirin ≥ 162 mg dose
    
    - treated with either loading dose of a P2Y12 inhibitor:
      
      - Prasugrel 60 mg
      
      - Cangrelor 30 μg/kg bolus
      
      - Ticagrelor 180 mg
      
      - Clopidogrel 600 mg

- Concept set for ST elevation
  
  - These are observations of STE from ECG readings

- Concept set for Troponin elevation
  
  - Lab value > 99th URL
  
  - Actual observation of 'Troponin elevation' or 'Cardiac troponin positive'

# Cohort Definitions

- ICD Codes *only* for acute STEMI
  
  - Limiting to ICD 10 codes

- ICD Code + Keyword search
  
  - Discharge summaries: Case-insensitive `STEMI` or case-insensitive `ST*elevat` or case-insensitive `ST*segment`, where `*` is a wild-card character that can take on any value. (Somani et al., 2022)

- Keyword search _only_

- Chest pain/Angina + ST elevation on ECG + Troponin elevation + (above PCI +/- stent concept set)
  
  - Where encounter diagnosis is NOT pericarditis, myocarditis, Takotsubo cardiomyopathy, spontaneous coronary artery dissection, NSTEMI, prinzmetal's angina, pulmonary embolism
  
  - Recall, all of this should occur within 0-1 days of encounter start

- ST elevation on ECG + Troponin elevation + (above PCI +/- stent concept set)
  
  - Above minus the 'Chest pain/Angina' bit to see if we can catch folks with 'atypical angina' or 'silent MI' better. These often include women, diabetic patients, and post-op patients.

- (PCI +/- stent concept set) + Echocardiogram order + ICU stay + cardiac rehabilitiation referral order
  
  - Rationale: Some patients may have ECG performed elsewhere (by EMS or transferred from another hospital)
  
  - ICU: 'Intensive care unit' (Concept code: '309904001'; use child concepts); some hospitals may have a dedicated 'Cardiac intensive care unit' (Concept code: 309907008)
  
  - Cardiac rehab: unsure if referral order observation will be seen during the acute STEMI encounter. May need to see if cardiac rehabilitation visits done within few weeks of discharge date.
