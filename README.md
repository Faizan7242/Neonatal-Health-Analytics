# Neonatal-Health-Analytics
## Project Overview

<img width="959" alt="SNCU_Dashboard_pg1" src="https://github.com/user-attachments/assets/7f8da90c-4085-4ce7-b247-b0c057ef9722" />

Bihar, one of the largest states in India with a population of approximately 130 million, has 35 District Hospitals providing neonatal healthcare services. This project aims to analyze and improve the performance of Special Newborn Care Units (SNCUs) across these hospitals by leveraging data analytics and visualization tools in Microsoft Power BI.
 
By analyzing critical healthcare indicators such as admission and discharge rates, bed occupancy, mortality trends, and   referral patterns, this dashboard provides actionable insights for policymakers and healthcare administrators.

## Key Terminologies

SNCU (Special Newborn Care Unit): A dedicated neonatal care unit in district hospitals for critical newborns.

Inborn: Infants born within the same facility where they receive treatment.

Outborn: Infants referred from outside facilities such as PHCs, CHCs, or private hospitals.

PHC (Primary Health Center): A basic health facility for primary medical services at the village level.

CHC (Community Health Center): A higher-level facility serving as a referral unit for multiple PHCs.

RH (Referral Hospital): A hospital equipped to handle complex medical cases referred from smaller facilities.

SDH (Sub-Divisional Hospital): A secondary care hospital serving a larger population.

APHC (Additional Primary Health Center): A supplementary health facility providing primary care services.


## Objective

To enhance the neonatal healthcare system in Bihar by analyzing hospital performance and identifying areas for improvement. The dashboard helps:

Track admission trends and identify districts with low healthcare accessibility.

Analyze bed occupancy rates to optimize resource allocation.

Identify high-mortality districts and the primary causes of neonatal deaths.

Evaluate referral trends to determine the capacity of district hospitals.

Rank hospitals based on key performance indicators.

## Data Source

### Government of Bihar SNCU Portal (SNCU Portal)

Secure government database requiring authorized access.

Contains real-time and historical neonatal healthcare data.

## Tools Used

Microsoft Excel – Data cleaning, pre-processing, and transformation.

Microsoft Power BI – Data visualization and dashboard creation.

## Methodology

#### Step 1: Data Extraction

Downloaded raw data from the SNCU portal covering neonatal admissions, discharges, deaths, referrals, and bed occupancy.

#### Step 2: Data Cleaning & Preprocessing

Checked for missing values and categorized them as 'Not Available' or 'Not Applicable.'

Standardized district names and healthcare facility codes for consistency.

Ensured all numerical values were correctly formatted for analysis.

#### Step 3: Data Verification

Cross-checked dataset with nurses and data entry operators from all 35 district hospitals.

Validated discrepancies in reported admission, discharge, and death counts.

#### Step 4: Data Visualization in Power BI

Imported cleaned data into Power BI.

Designed interactive dashboards for real-time analysis.

## Dashboard Features & Metrics

### 1. Admission Profile

Metric: Admission rate calculated based on live birth.

### Formula: 
  
  Admission Rate = 
  
	DIVIDE(SUM(Master_Sheet[Num_of_Admission]), SUM('Live Birth'[Live Birth])) * 100
  
### 2. Discharge Rate Analysis

Metric: Percentage of admitted neonates who were successfully discharged.

### Formula:

  Discharge Percentage = 
 
    (SUM(Master_Sheet[Num_of_Discharge])/SUM(Master_Sheet[Num_of_Admission]) * 100)

### 3. Bed Occupancy Rate

Metric: Measures bed utilization across districts over time.

### Formula:

  Total Bed Occupancy Rate = 
   
    DIVIDE(SUM(Master_Sheet[Num_of_Beds_Occupied]), 
    SUM(Beds[Beds]) * COUNTROWS(SUMMARIZE(Master_Sheet, Master_Sheet[Date])), 0) * 100

### 4. Death Analysis

Metric: Total death percentage across all admissions.

### Formula:

  Death Percentage = 
    
    DIVIDE(SUM(Master_Sheet[Num_of_Death]), SUM(Master_Sheet[Num_of_Admission]), 0) * 100

Breakdown: Deaths are bifurcated into inborn and outborn cases.

### 5. Outborn Death Analysis

<img width="262" alt="outborn_death_distribution" src="https://github.com/user-attachments/assets/faeb518e-c1a4-4d26-b879-109a4de5aece" />

Categorized outborn deaths by:

Government facilities (PHC, CHC, RH, SDH, APHC)

Private hospitals

Home deliveries

### 6. Referral Rate Analysis

<img width="381" alt="Referral_trends" src="https://github.com/user-attachments/assets/7f950425-7f00-4bfc-898f-92cb200846f2" />

Metric: Percentage of neonates referred to higher facilities.

### Formula:

   Referral Percentage = 
    
    DIVIDE(SUM(Master_Sheet[Num_of_Referrals]), SUM(Master_Sheet[Num_of_Admission]), 0) * 100

### 7. LAMA (Left Against Medical Advice) Cases

Highlights districts with the highest LAMA rates and trends over time.

### 8. Hospital Ranking System

#### Indicators:

Admission Rate (17%)

Discharge Rate (16%)

Death Rate (17%) (lower is better)

Referral Rate (17%) (lower is better)

LAMA Rate (16%) (lower is better)

Bed Occupancy Rate (17%)

### Formula:

  District_Ranking_Score = 
  		
    VAR AdmissionScore = DIVIDE(SUM(Master_Sheet[Num_of_Admission]), SUM(Population[Population]), 0) * 100
  		VAR DeathScore = DIVIDE(SUM(Master_Sheet[Num_of_Death]), SUM(Master_Sheet[Num_of_Admission]), 0) * 100
  		VAR ReferralScore = DIVIDE(SUM(Master_Sheet[Num_of_Referrals]), SUM(Master_Sheet[Num_of_Admission]), 0) * 100
  		VAR BedOccupancyScore =DIVIDE(
  		    SUM(Master_Sheet[Num_of_Beds_Occupied]),
  		    SUM(Beds[Beds]) * COUNTROWS(SUMMARIZE(Master_Sheet, Master_Sheet[Date])),
    		  0
	  	) * 100
      VAR LAMAScore = DIVIDE(SUM(Master_Sheet[LAMA]), SUM(Master_Sheet[Num_of_Admission]), 0) * 100
  		VAR DischargeScore = SUM(Master_Sheet[Num_of_Discharge])/SUM(Master_Sheet[Num_of_Admission]) * 100
    RETURN
    ( AdmissionScore * 0.17 ) +  // Positive impact
	  ( DischargeScore * 0.16 )  +  // Positive impact
	  ( (1 - (DeathScore / 100)) * 0.17 ) + // Negative impact (lower death rate is better)
	  ( (1 - (ReferralScore / 100)) * 0.17 ) + // Negative impact (lower referral rate is better)
	  ( (1 - (LAMAScore / 100)) * 0.16 ) + // Negative impact (lower LAMA rate is better)
	  ( BedOccupancyScore * 0.17 )  // Positive impact

### Ranking

District_Rank = 
    
    RANKX(ALL(Master_Sheet[Districts]), [District_Ranking_Score], , DESC, DENSE)

<img width="710" alt="SNCU_Dashboard_pg11" src="https://github.com/user-attachments/assets/35647a1b-9a7b-4592-9b66-3bb17b69c488" />


## Key Insights (Jan 2024 - March 2025)

**Admission Rate**: The admission rate i.e. (the percentage of admission on live birth) of neonates in the SNCU across Bihar 15.55%. 

<img width="719" alt="image" src="https://github.com/user-attachments/assets/6f1e81a0-e769-47d7-a2ac-d91f5e4248ba" />

**High Discharge Percentage**: Purbi Champaran, Sitamarhi

<img width="667" alt="Discharge percentage based on admission" src="https://github.com/user-attachments/assets/7afbe5a5-da55-464b-a260-d3903f7b4ab8" />

**Death Analysis**

**High Death Percentage**: Begusarai (critical concern)

Outborn Deaths: 67.34% vs. Inborn Deaths: 32.66%

<img width="584" alt="image" src="https://github.com/user-attachments/assets/190b2bcf-d3bf-417a-867f-94a3529959d3" />

**Death Percentage**

<img width="221" alt="image" src="https://github.com/user-attachments/assets/f008de98-69b8-4e8a-b309-0d79bf04c05c" />


The overall neonatal death percentage across Bihar has reduced from *4.07%* in the first quarter of 2024 (Jan-March) to *2.84%* in first quarter of 2025(Jan-March). 

**Major Referral Sources**: PHCs have the highest outborn referrals

**Highest Referral Rate**: Aurangabad (14.2%)

**Highest LAMA Cases**: Rohtas, followed by Munger

**Top 3 SNCUs**: Sitamarhi, Siwan, Bhojpur

**Bottom 3 SNCU**s: Munger, Vaishali, Patna

## Conclusion & Recommendations

**Resource Redistribution**: Optimize bed allocation across districts.

<img width="654" alt="Bed Occupancy rate by district" src="https://github.com/user-attachments/assets/32680ac6-907e-4a23-8ec8-bab8192bcf4e" />

**Early Intervention**: Improve neonatal care at PHCs to reduce outborn referrals.

**Mortality Reduction Strategies**: Enhance training and equipment in high-mortality districts.
