# Troponin

- Elevated Troponin Measurements
    - These include LOINC codes and other observable lab measurement data.
    - These include Troponin I and Troponin T measurements. By the UDMI definition, a Troponin is considered elevated if it is > 99th percentile of the upper reference limit (URL). Fortunately, there are LOINC codes for these >99th percentile URL values, e.g. Concept Code 89581-3. Although it remains to be seen if/how common this information is available.
- Elevated Troponin Observations
    - These are concept codes flagging if a patient has a positive/elevated Trop
- Using this information, we can then exclude other causes of elevated Trop

# PCI with or without stent

- Excluded the Cardiac CT and Cardiac MRI stuff
- Includes stents (drug-eluting stents and bare metal coronary artery stents).
    - There are other flavors of bare metal stents for other places, e.g. esophagus, that I was mindful to exclude.

# Fibrinolysis

- Recall, this is the alternative treatment that acute STEMI patients will receive if the facility does not offer PCI and transferring the patient will involve a delay of PCI > 120 mins.
- TODO: assemble this concept set
- Might make for a good sanity check as the number of patients who get fibrinolysis for acute STEMI will be a small fraction compared to those who get PCI.

# DAPT

- Aspirin - chewable, immediate-release forms
    - Aspirin Chewable Tablet Concept Code: 40009842 
- P2Y12 inhibitors (used RxNorm)
