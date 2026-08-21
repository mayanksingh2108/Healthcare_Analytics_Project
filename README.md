# 🏥 Healthcare Data Analysis & Operations Dashboard 📊

An end-to-end data analytics project designed to uncover actionable insights into hospital operations, patient demographics, clinical performance, and revenue cycle management across a multi-hospital network.

## 🚀 Project Overview
This project simulates a real-world healthcare analytics workflow. It involves processing **20,000 raw transactional records**, modeling the data in a relational database, performing complex SQL analysis, and visualizing the findings in an interactive, enterprise-grade Power BI dashboard.

### 🛠️ Tech Stack
* **Python (Pandas, NumPy):** Data Cleaning & Transformation
* **MySQL (SQLAlchemy):** Relational Database & Data Ingestion
* **SQL:** Advanced Data Analysis (CTEs, Window Functions)
* **Power BI:** Data Modeling, DAX, & Interactive Dashboard Design

---

## 📂 Dataset Summary
The dataset consists of 5 interconnected tables containing approximately 30 columns:
1. **Patients:** Demographics (`Age`, `Gender`, `BloodType`)
2. **Admissions:** Clinical records (`AdmissionDate`, `DischargeDate`, `Diagnosis`, `Department`)
3. **Doctors:** Physician details (`Specialty`, `YearsOfExperience`)
4. **Billing:** Financials (`TotalCost`, `InsuranceCovered`, `PaymentStatus`)
5. **Hospitals:** Facility locations (`City`, `Region`)

---

## 🧠 Step-by-Step Implementation

### Step 1: Data Cleaning Using Python 🐍
Raw hospital data is rarely ready for immediate analysis. The initial ETL phase was handled in a Jupyter Notebook using **Pandas**.
* **Standardization:** Column names were converted to lowercase and spaces replaced with underscores for SQL compatibility.
* **Imputation:** Missing demographic data (Age, Gender, Blood Type) were imputed using medians and proportional random sampling. Missing billing costs were calculated using the median cost grouped by specific `diagnosis` and `admissiondays`.
* **Feature Engineering:** Created derived columns like `age_group`, `stay_category` (Short, Medium, Long Treatment), and dynamically derived `paymentstatus` based on insurance coverage ratios.

<p align="center">
  <img src="images/Screenshot%202026-07-24%20123401.png" alt="Patient Data Cleaning" width="80%">
</p>

### Step 2: Loading Data Directly into MySQL 🗄️
Instead of manually importing flat files, the cleaned DataFrames were pushed directly to a local MySQL server.
* Utilized **SQLAlchemy** `create_engine` to establish a secure database connection.
* Used the `.to_sql()` function with `if_exists='replace'` to automatically generate the database schema and load the 20,000+ records seamlessly.

<p align="center">
  <img src="images/Screenshot%202026-07-24%20123625.png" alt="Loading Data to MySQL" width="80%">
</p>

### Step 3: SQL Analysis 🔍
With the data securely in MySQL, 13 complex queries and stored procedures were developed to extract business logic. 
* **Window Functions:** Utilized `LAG()` to calculate Month-over-Month (MoM) revenue growth and `DENSE_RANK()` to identify "Star Doctors" by region.
* **Aggregations:** Mapped regional revenue, identified high-value patients, and tracked average admission days.
* **Self-Joins:** Tracked 30-day patient readmissions.

**Example: Calculating Regional Gross Revenue**
<p align="center">
  <img src="images/Screenshot%202026-07-24%20123808.png" alt="SQL Query" width="80%">
  <br>
  <img src="images/Screenshot%202026-07-24%20123833.png" alt="SQL Result" width="40%">
</p>

### Step 4: Load Data from SQL Database to Power BI 🔌
To create a dynamic and automated reporting layer, **Power BI** was connected directly to the **MySQL Database**. 
* Extracted the `admissions`, `billing`, `patients`, `doctors`, and `hospitals` tables via the MySQL database connector.
* Established a **Star Schema** data model, linking the central fact tables (`admissions` and `billing`) to their respective dimension tables.
* Generated a dynamic `CalendarTable` using DAX to enable time-intelligence tracking (MoM, YoY).

### Step 5: Power BI Dashboard Creation 📈
A 4-page interactive dashboard was designed to provide tailored insights for different hospital stakeholders (C-Suite, CMO, CFO).

#### 1. Executive Overview Dashboard
Provides a high-level pulse on network volume, total gross revenue, and regional performance.
<p align="center">
  <img src="images/Screenshot%202026-07-24%20124457.png" alt="Executive Dashboard" width="90%">
</p>

#### 2. Patient Information Dashboard
Analyzes patient demographics, blood type inventory needs, and the distribution of diagnoses across age groups.
<p align="center">
  <img src="images/Screenshot%202026-07-24%20124544.png" alt="Patient Dashboard" width="90%">
</p>

#### 3. Clinical Performance Dashboard
Tracks operational efficiency, highlighting 30-day readmission rates, specialty admission volumes, and the impact of doctor experience on patient length of stay.
<p align="center">
  <img src="images/Screenshot%202026-07-24%20124602.png" alt="Clinical Dashboard" width="90%">
</p>

#### 4. Billing Operations Dashboard
A granular financial deep-dive identifying outstanding debt, insurance coverage gaps, and the revenue cycle funnel.
<p align="center">
  <img src="images/Screenshot%202026-07-24%20124626.png" alt="Billing Dashboard" width="90%">
</p>

---

## 💡 Key Business Insights

1. **Revenue Concentration:** The **South Region** generates the highest gross revenue (~₹891M), followed by the North Region. Operational expansion efforts should prioritize these areas.
2. **Resource Bottlenecks:** **Pediatrics** and **Orthopedics** handle the highest volume of admissions. Staffing models and bed capacity must be adjusted to accommodate these specific departments.
3. **Collection Risks:** A staggering **₹695M** is tied up in outstanding debt. A significant portion of bills are in 'Defaulted' status due to low insurance coverage ratios (<50%), indicating a severe need for stricter upfront insurance verification.
4. **Supply Chain:** `O-` and `AB+` are the most common blood types treated; blood bank procurement and cold-chain storage should align closely with this distribution to prevent shortages.

---
*Created as a comprehensive portfolio project demonstrating end-to-end data analytics capabilities.*
