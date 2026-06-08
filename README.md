# Clinical Data Quality & Analytics Report: COVID-19 vs. Respiratory Pathologies

## 📌 Project Overview
This project serves as a comprehensive **Analytics Case Study** focused on healthcare intelligence. Using a public chest X-ray and clinical metadata dataset from GitHub, this exercise was intentionally designed to simulate a real-world scenario where raw data is chaotic, incomplete, and contains system errors. 

The core objective was to apply data cleaning best practices, design an optimized data model, and transform messy healthcare records into a reliable, interactive dashboard capable of delivering strategic, life-saving insights for hospital management.

* **Tech Stack:** Microsoft Power BI and Power Query.
* **Data Source:** [IEEE8023 COVID Chest X-Ray Dataset](https://raw.githubusercontent.com/ieee8023/covid-chestxray-dataset/master/metadata.csv)

---

## 🛠️ Data Quality & Cleansing Challenges (The Process)
Data from medical environments often suffers from human entry mistakes or extraction bugs. Below are the key data quality issues identified and resolved using **Power Query**:

1. **The PCR Column Trap (`RT_PCR_positive`):** This column was highly incomplete, containing only partial positive records and completely missing clear "Negative" results. Using it for filtering would have led to corrupted conclusions. 
   * *Solution:* Discarded the column for filtering and leveraged the validated `Findings` attribute instead (segmented into Pneumonia/Viral/COVID-19).
2. **Clinical Outlier Detection:** The raw data contained physically impossible metrics due to system errors.
   * *Solution:* Applied data guardrails. Any body temperature above 40°C or O_2 saturation above 100% was automatically converted into `null` (blank) values to prevent them from skewing the chart averages.
3. **Handling Missing Demographic Data:** The lines for body temperature and oxygen saturation for female patients in general respiratory sicknesses were broken and incomplete due to gaps in the raw dataset.
   * *Solution:* Maintained these as blanks to preserve data integrity and prevent the invention of false medical trends.

---

## 📐 Data Architecture & Modeling (Star Schema)
To ensure the dashboard is lightweight, efficient, and scalable, the data structure was decoupled from a single flat spreadsheet into a formal **Star Schema Relationship Model**:

* **Patient Dimension Table (`Dim_Patient`):** Contains unique patient demographics (`patientid`, `sex`, `age`).
* **Exams Fact Table (`Fact_Exams`):** Contains multi-event data captured per X-ray instance (`filename`, `temperature`, `pO2_saturation`, `offset`, `went_icu`, `intubated`, `survival`).

### Why this architecture?
A single patient can undergo multiple diagnostic images (X-rays/CT scans) on different days to monitor sickness progression. By using a Star Schema, we can track each specific exam flawlessly over time without duplicating personal data, keeping the database fast and filters functionally accurate.

*Optimization:* Highly technical or unsupported laboratory metrics (e.g., WBC, neutrophil, and lymphocyte counts) were removed to prioritize hospital management KPIs and user readability.I didn’t use the atributte extubated because of the lack of data and the other attributes not described here because I didn’t need them for my analysis.

---

## 📊 Strategic Insights & Clinical Meaning

### 1. The COVID-19 Timeline (`offset`)
The `offset` represents the exact number of days between the first symptoms and when the diagnostic image was taken. It functions as our clinical timeline. 

### 2. The Temperature "Rollercoaster" (Cytokine Storm)
* **Observation:** In COVID-19 patients, temperatures start high, drop significantly to 36/37°C around **Day 6**, and then shoot up aggressively, stabilizing near 39°C after **Day 10**.
* **Clinical Meaning:** This reflects the two-phase progression of COVID-19. The drop around Day 6 often creates a false sense of recovery. However, after Day 10, the patient's immune system can trigger a severe hyper-inflammatory response, which requires immediate medical intervention.

### 3. Immediate Ventilation Needs
* **Observation:** The intubated patients metric peaks drastically on **Day 0 (close to 54 patients)** for COVID-19 compared to standard pathologies.
* **Clinical Meaning:** Patients with normal respiratory issues seek early hospital care. COVID-19 patients tended to isolate at home during the first week, arriving at the emergency room only when their lungs were already highly compromised, requiring immediate, first-day intubation.

### 4. Advanced Age vs. Critical Care (Triage Evaluation)
* **Observation:** Patients over 61 years old faced a high mortality rate. However, cross-referencing this data reveals that **100% of these elderly patients who died were already admitted to the ICU**.
* **Clinical Meaning:** This proves the hospital's triage on this data and screening protocols worked. No elderly patient was left neglected in a regular ward; they were correctly prioritized for critical care. The high mortality represents a biological limitation of severe COVID-19 in advanced age, indicating that management should focus on **early prophylactic treatments** before ICU admission becomes necessary.

---

## 💡 Key Takeaway & Conclusion
This project demonstrates that raw data can easily lead to catastrophic management or clinical mistakes if not properly treated (e.g., automated scaling misalignments or misinterpreting artificial 100% mortality spikes caused by small sample sizes on specific days). 

By building a resilient Star Schema, implementing strict data cleansing guardrails, locking chart axes for perfect visual symmetry, and cross-referencing demographics with ICU tracking, a messy spreadsheet was turned into a strategic tool for healthcare decision-making.


---

## 📂 Project Materials & Deliverables

All documentation, presentation slides, and dashboard files are organized inside the `files` directory. 

👉 **[Click here to access the files folder](./files/)** to view:
* The native Power BI workbook (`.pbix`)
* The static PDF dashboard preview
* The executive presentation slides
* The complete written technical report

*Note: A dedicated README is available inside the folder to guide you through each asset.*
