# Clinical Description

## Overview

The 4th Universal Definition of Myocardial Infarction (UDMI) defines acute ST-elevation myocardial infarction (STEMI) as requiring a rise and/or fall of **cardiac troponin (cTn)** values (with at least 1 value > 99th percentile) and **clinical evidence of ischemia** (ie, symptoms, ECG changes, supportive ECG or other imaging findings, or evidence of coronary thrombus). The underlying etiology is plaque disruption with coronary atherothrombosis. [^udmi] [^lancet] 

![](https://www.ahajournals.org/cms/asset/1572f4bd-1cb9-4f37-bff5-51a7856046de/e618fig03.jpg)

In some camps, they prefer the term acute coronary occlusion myocardial infarction (OMI). [^omi] Those who prefer OMI point out that some patients with acute coronary artery occlusion and require urgent reperfusion therapy fail to meet traditional STEMI electrocardiogram (ECG) criteria ("STEMI-negative OMI").

[^omi]: Pendell Meyers H, Bracey A, Lee D, et al. Accuracy of OMI ECG findings versus STEMI criteria for diagnosis of acute coronary occlusion myocardial infarction. IJC Heart & Vasculature. 2021;33:100767. doi:10.1016/j.ijcha.2021.100767
[^lancet]: Bergmark, B. A., Mathenge, N., Merlini, P. A., Lawrence-Wright, M. B., & Giugliano, R. P. (2022). Acute coronary syndromes. _The Lancet_, _399_(10332), 1347–1358. https://doi.org/10.1016/S0140-6736(21)02391-6
[^udmi]: Thygesen K, Alpert JS, Jaffe AS, et al. Fourth Universal Definition of Myocardial Infarction (2018). Circulation. 2018;138(20). doi:10.1161/cir.0000000000000617

## Assessment

- History and physical, including family history of early heart disease

- cardiac troponin
  
  - considered positive if > 99th percentile upper reference limit

- ECG
  
  - STEMI criteria: new ST-elevation at the J-point in 2 contiguous leads with the cut-point: ≥1 mm in all leads other than leads V2–V3 where the following cut-points apply: ≥2 mm in men ≥40 years; ≥2.5 mm in men <40 years, or ≥1.5 mm in women regardless of age

As above, acute STEMI is defined by an elevated troponin lab value and evidence of clinical ischemia. Given the acuity of this condition, patients are urgently evaluated and treated. Prompt evaluation often entails work-up with a **troponin** lab value with a cutoff of > 99th percentile of the upper reference limit considered positive. An ECG often will show ST-elevation, but other STEMI equivalent ECG findings may also be seen. [^stemi-equiv] Classic symptoms concerning for an acute STEMI include angina (chest pain/pressure) +/- radiation to the arms/shoulders and/or neck/jaw. Symptoms may also include fatigue, dyspnea, lightheadedness, diaphoresis and nausea. Importantly, some patients may present with "silent MIs," which is more commonly seen among women, diabetic, and post-op patients.

[^stemi-equiv]: Asatryan B, Vaisnora L, Manavifar N. Electrocardiographic Diagnosis of Life-Threatening STEMI Equivalents. JACC: Case Reports. 2019;1(4):666-668. doi:10.1016/j.jaccas.2019.10.030

## Plan

- Cardiac catheterization, often involving stent placement

- Dual antiplatelet therapy (DAPT)
  
  - Aspirin + either one of the following: Clopidogrel, Prasugrel, Ticagrelor
    
    - Some patients may also receive IV cangrelor
  
  - loading dose, followed by maintenance dose
  
  - recommended for 12 months following acute coronary syndrome in most patients

- High-intensity statin

- Beta-blocker
  
  - often initiated within 24 hrs absent any contraindications

- +/- ACE inhibitor/ARB

- +/- aldosterone antagonist

- Risk factor stratification and modification, e.g. smoking cessation, diabetes control, etc.

- Echocardiogram

- Referral to cardiac rehabilitation program

The priority with patients presenting with an acute STEMI is revascularization. When there is enough degree of suspicion for STEMI, patients are immediately treated with aspirin (325 mg or 162 mg are common doses used) as the evaluation (labs, ECG) are underway. They will also be treated with a loading dose of a P2Y12 inhbitor. The cardiac catheterization lab is activated and the patient is taken for urgent revascularization, ideally by percutaneous coronary intervention  (PCI).

Post-PCI, patients are often cared for in the intensive care unit. Some patients may require vasopressor or inotropic medications or mechanical circulatory support devices. Patients will be prescribed a high-intensity statin and DAPT (often continued for at least 12 months). A beta-blocker is also often started within 24 hours. They will also undergo further evaluation with echocardiogram and risk factor stratification. Patients should be referred to cardiac rehabilitation prior to discharge.

# Computable STEMI Phenotyping

- Thoughts on possible phenotyping pportunities using structured data not previously addressed or with limited use
  
  - **Timing** from encounter start to aspirin, ECG, loading dose of P2Y12 inhibitor, cath lab, stent (everything should happen very quickly with a STEMI)
  
  - Intervention (PCI, stent): procedure codes and device codes
    
    - Related procedure codes
      
      - includes left heart catheterization; exclude if _only_ right heart catheterization
    
    - Related stent device codes: Child concepts of 'Coronary artery stent' should do the trick
  
  - ICU stay after cath could also tip the probability in favor of a STEMI as well

In a systematic review of computable phenotypes for patients with Acute MI (includes STEMI, NSTEMI), Rubbo et al. find that the vast majority rely on structured data, in particular ICD codes. [^rubbo]

![](https://ars.els-cdn.com/content/image/1-s2.0-S0167527315003137-gr1.jpg)

- STEMI Diagnosis codes [^somani] [^coloma] [^patel]
  
  - ICD-9 and ICD-10 codes: 410, 410.21, 410.31, 410.41, 410.01, 410.11, 410.51, 410.61, 410.81, 410.91
  
  - ICD-10 codes: I21, I21.11, I21.19, I21.21, I21.01, I21.02, I21.09, I21.29, I21.3

- (If available) Keyword search: clinical notes for 'STEMI' and related keywords [^somani]
  
  - Looked exclusively at discharge summaries and used the following "regular expression criteria: case-insensitive `STEMI` or case-insensitive `ST*elevat` or case-insensitive `ST*segment`, where `*` is a wild-card character that can take on any value."

- (If available) Keyword search: ECG readings [^somani]
  
  - Keywords (case-insensitive): 'STEMI', 'ACUTE MI'

[^rubbo]:  Rubbo B, Fitzpatrick NK, Denaxas S, et al. Use of electronic health records to ascertain, validate and phenotype acute myocardial infarction: A systematic review and recommendations. International Journal of Cardiology. 2015;187:705-711. doi:10.1016/j.ijcard.2015.03.075

[^somani]: Somani S, Yoffie S, Teng S, et al. Development and validation of techniques for phenotyping ST-elevation myocardial infarction encounters from electronic health records. JAMIA Open. 2021;4(3). doi:10.1093/jamiaopen/ooab068
[^coloma]: Coloma PM, Valkhoff VE, Mazzaglia G, et al. Identification of acute myocardial infarction from electronic healthcare records using different disease coding systems: a validation study in three European countries. BMJ Open. 2013;3(6):e002862. doi:10.1136/bmjopen-2013-002862
[^patel]: Patel AB, Quan H, Welsh RC, et al. Validity and utility of ICD-10 administrative health data for identifying ST- and non-ST-elevation myocardial infarction based on physician chart review. CMAJ Open. 2015;3(4):E413-E418. doi:10.9778/cmajo.20150060
