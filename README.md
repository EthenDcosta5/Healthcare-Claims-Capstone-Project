# Healthcare Claims Capstone Project

## Overview
This capstone project simulates real-world scenarios in the healthcare domain, involving data extraction, visualization, and predictive analytics. The focus is on healthcare claims data, encompassing inpatient and outpatient claims, beneficiary demographics, and provider insights. 

**Objectives**:
- Extract actionable insights using SQL queries.
- Build interactive dashboards using Power BI/Tableau.

---

## Phase 1: SQL Analysis – Healthcare Claims Data

**Objective**: Extract key insights from healthcare claims data for better decision-making.

**Dataset Link**: [Download Raw Dataset](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Raw-Dataset.rar)

### SQL Tasks and Queries
1. **Claims Analysis**:  
   Retrieve the total amount reimbursed for inpatient claims grouped by the provider.
   ```sql
   SELECT 
       Provider,
       SUM(InscClaimAmtReimbursed) AS TotalAmountReimbursed
   FROM
       in_patient_data
   GROUP BY Provider
   ORDER BY TotalAmountReimbursed DESC;
   ```

2. **Provider Insights**:  
   Identify the top 5 providers with the highest number of outpatient claims.
   ```sql
   SELECT 
       Provider, COUNT(*) AS TotalClaims
   FROM
       out_patient_data
   GROUP BY Provider
   ORDER BY TotalClaims DESC
   LIMIT 5;
   ```

3. **Chronic Conditions**:  
   Count beneficiaries with claims associated with chronic conditions like diabetes.
   ```sql
   SELECT 
       COUNT(*) AS TotalCount
   FROM
       beneficiary_data
   WHERE
       ChronicCond_Diabetes = 1;
   ```

4. **Gender-Based Analysis**:  
   Calculate the average inpatient claim amount reimbursed for each gender.
   ```sql
   SELECT 
       b.gender,
       AVG(i.InscClaimAmtReimbursed) AS AverageReimbursed
   FROM
       beneficiary_data b
       LEFT JOIN in_patient_data i ON b.BeneID = i.BeneID
   GROUP BY b.gender;
   ```

5. **Beneficiary History**:  
   Retrieve all claims for a specific BeneID to analyze an individual beneficiary's history.
   ```sql
   SELECT 
       BeneID, ClaimID, ClaimStartDt, ClaimEndDt, Provider, InscClaimAmtReimbursed 
   FROM
       in_patient_data
   WHERE
       BeneID = 'BENE21203' 
   UNION 
   SELECT 
       BeneID, ClaimID, ClaimStartDt, ClaimEndDt, Provider, InscClaimAmtReimbursed
   FROM
       out_patient_data
   WHERE
       BeneID = 'BENE21203';
   ```

6. **High-Value Claims**:  
   Identify providers with claims where the admission date is in 2009, and the reimbursed amount exceeds $10,000.
   ```sql
   SELECT 
       Provider, AdmissionDt, InscClaimAmtReimbursed
   FROM
       in_patient_data
   WHERE
       YEAR(AdmissionDt) = 2009
           AND InscClaimAmtReimbursed > 10000;
   ```

7. **Demographic Analysis**:  
   Calculate the average deductible amount for beneficiaries aged 65+.
   ```sql
   SELECT 
       AVG(IPAnnualDeductibleAmt) AS Avg_Deductible
   FROM
       in_patient_data i
       JOIN beneficiary_data b ON i.BeneID = b.BeneID
   WHERE
       TIMESTAMPDIFF(YEAR, b.DOB, CURDATE()) >= 65;
   ```

8. **Physician Involvement**:  
   List all claims involving more than one physician.
   ```sql
   SELECT 
       BeneID, ClaimID, ClaimStartDt, ClaimEndDt, Provider, InscClaimAmtReimbursed,
       AttendingPhysician, OperatingPhysician, OtherPhysician
   FROM
       in_patient_data
   WHERE
       (AttendingPhysician IS NOT NULL AND OperatingPhysician IS NOT NULL)
       OR (AttendingPhysician IS NOT NULL AND OtherPhysician IS NOT NULL)
       OR (OperatingPhysician IS NOT NULL AND OtherPhysician IS NOT NULL);
   ```

**SQL Resources**:
- [SQL Queries File](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/SQL-Project-File.sql)
- [SQL Insights Report](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Insights-Report/SQL-Analysis-Insights-Report.docx)

---

## Phase 2: Dashboard Creation – Power BI

**Objective**: Create interactive dashboards for claims analysis.  
**Power BI Project File**: [Healthcare Claims Power BI File](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/HealthCare-Claims-Project-PowerBI.pbix)

### Dashboard Pages
**Home Page**
![Home Page](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Dashboard-Images/Home%20Page.png)
1. **Claims Overview**  
   - **Visuals**: Total Reimbursement, Claims Amount Distribution.  
   - **KPIs**: Total Claims, Average Claim Amount.

   ![Claims Overview](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Dashboard-Images/Page%201%20-%20Claims%20Overview.png)

2. **Provider Analysis**  
   - **Visuals**: Top 5 Providers, High-Value Claims.  
   - **KPIs**: Number of Providers, Total Reimbursement.  

   ![Provider Analysis](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Dashboard-Images/Page%202%20-%20%20Provider%20Analysis.png)

3. **Demographic Insights**  
   - **Visuals**: Gender and Race Breakdown, Chronic Condition Prevalence.  
   - **KPIs**: Number of Beneficiaries, Average Deductible.  

   ![Demographic Insights](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Dashboard-Images/Page%203%20-%20Demographic%20Insights.png)

4. **Trends Over Time**  
   - **Visuals**: Monthly/Quarterly Trends.  
   - **KPIs**: Average Claim Amount (Monthly).  

   ![Trends Over Time](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Dashboard-Images/Page%204%20-%20%20Trends%20Over%20Time.png)

**Dashboard Resources**:
- [Dashboard Insights Report](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Insights-Report/Dashboard-Insights-Report.docx)
- [Data Cleaning Process](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Data-Cleaning-Process.docx)

---

## Final Deliverables
- **Presentation PDF**: [Healthcare Claims Project PDF](https://github.com/EthenDcosta5/Healthcare-Claims-Capstone-Project/blob/main/Healthcare-Claims-Project-PDF.pdf)
