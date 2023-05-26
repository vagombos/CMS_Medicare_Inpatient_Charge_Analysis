# CMS Medicare Inpatient Charge Analysis  
### Case Study Using CMS Open Source Inpatient Data from 2017-2021 (Python)
##### - [Background and Research Questions](#background-and-research-questions)
##### - [Statement of Business Task](#statement-of-business-task)
##### - [Description of Data Sources](#description-of-data-sources)
##### - [Data ETL and Analyses](#data-etl-and-analyses)
##### - [Conclusions and Insights](#conclusions-and-insights)
##### - [References](#references)
---
### Background and Research Questions:
A critical part of the healthcare sector is the link between the services rendered by providers and hospitals, and how those costs are passed down to insurance companies and the government (in the case of Medicare and Medicaid as examples). This is a particularly important area of research because Medicare costs have been rising and public opinion leans in the direction of more coverage, though how most understand where costs come from is complex and sometimes conflicting.[^1]
- The Research Questions are: 
    - **What factors captured in the data may account for higher payments?**
    - **What factors may account for more (or less) Medicare coverage for payments?**
    - **What payments/coverage can we predict for the next year (i.e., 2022, as the data's timeframe is 2017-2021)?**

### Statement of Business Task:
- The business task is to find factors associated with payments, medicare coverage, and predict the next year's payments. This exploratory analysis will be an investigation into what providers and diagnosis-related groupings are associated with payments and coverage. Further, using what is found about the payments, those factors can be used to predict what payments/coverage might be expected in 2022 based on the prior five years' data. Although CMS tracks costs across a variety of stages and areas of care, this case study focuses on just inpatient costs.
- Key factors have been established in the literature on medical costs that point to especially good prospects in the dataset to investigate, thus giving clearer direction on what to focus on:
1. The **type of diagnosis/condition of patients** is a driver for medical costs--especially chronic ones.[^2]
2. The **number of discharges or volume of services** by providers is related to medical costs. [^3]
3. The **types of providers/hospitals** have a relationship to Medicare costs.[^4],[^5]  

### Description of Data Sources:
- The open source [CMS (Centers for Medicare and Medicaid Services) Inpatient data](https://data.cms.gov/provider-summary-by-type-of-service/medicare-inpatient-hospitals/medicare-inpatient-hospitals-by-provider) provides information on hospital discharges from Original Medicare (fee-for-service) Part A (Hospital Insurance) beneficiaries by Inpatient Prospective Payment System (IPPS) hospitals. It includes information on the use, payment, and hospital charges for more than 3,000 U.S. hospitals that received IPPS payments. The Medicare Inpatient Hospitals - by Provider and Service datasets, from 2017-2021, were individually downloaded for each year.
- The CMS provides a [data dictionary](Data/MUP_INP_RY23_20230426_DD_PrvSrvc_508.pdf) of the 15 fields that are tracked in this Medicare Inpatient Hospitals - by Provider and Service dataset. Following the aforementioned factors to focus on, there are the following fields that were selected for the current analysis:  
    - *Rndrng_Prvdr_Org_Name*: The name of the provider.  
    - *DRG_Desc*: Description of the classification system (DRG) that groups similar clinical conditions (diagnoses) and the procedures furnished by the                  hospital during the stay.  
     - *Tot_Dschrgs*: The number of discharges billed by all providers for inpatient hospital services.  
     - *Avg_Tot_Pymt_Amt*: The average total payments to all providers for the DRG including the MS-DRG amount, teaching, disproportionate share, capital, and            outlier payments for all cases. Also included in average total payments are co-payment and deductible amounts that the patient is responsible for and              any additional payments by third parties for coordination of benefits.  
     - *Avg_Mdcr_Pymt_Amt*: The average amount that Medicare pays to the provider for Medicare's share of the MS-DRG. Medicare payment amounts include the MS-            DRG amount, teaching, disproportionate share, capital, and outlier payments for all cases. Medicare payments DO NOT include beneficiary co- payments              and deductible amounts nor any additional payments from third parties for coordination of benefits.  

Additionally, a simple metric called *"Prct_Mdcr_Covered" (Percent of Medicare Covered)* was calculated to determine what proportion of the average total payment was payed out by Medicare to the provider (i.e., "Prct_Mdcr_Covered" = "Avg_Mdcr_Pymt_Amt"] / ["Avg_Tot_Pymt_Amt"]) * 100).

### Data ETL and Analyses
Each year's data were locally downloaded and extracted into their component .csv files. This first step is a merge using the pandas library in Python. This step also includes some transformations of fields into workable formats, and a simple summary of the merged dataset (which gets saved locally as "merged_data.csv").

```python
# Import the pandas library
import pandas as pd

# Define the path to the folder containing the CSV files
path = ".../Medicare_Inpatient_Hospital_by_Provider_and_Service_2017-2021"

# Create a list of the CSV files
csv_files = [
    "Medicare_Inpatient_Hospital_by_Provider_and_Service_2017.csv",
    "Medicare_Inpatient_Hospital_by_Provider_and_Service_2018.csv",
    "Medicare_Inpatient_Hospital_by_Provider_and_Service_2019.csv",
    "Medicare_Inpatient_Hospital_by_Provider_and_Service_2020.csv",
    "Medicare_Inpatient_Hospital_by_Provider_and_Service_2021.csv"
]

# Create a list of dataframes, one for each CSV file
dfs = []
for csv_file in csv_files:
    df = pd.read_csv(path + "/" + csv_file, encoding="cp1252")
    df["Year"] = csv_file.split("_")[-1].split(".csv")[0]
    dfs.append(df)
    
# Merge the dataframes into one dataframe
merged_df = pd.concat(dfs, ignore_index=True)

# Convert the strings to floats
merged_df["Avg_Tot_Pymt_Amt"] = merged_df["Avg_Tot_Pymt_Amt"].str.replace("$", "").str.replace(",", "").astype(float)
merged_df["Avg_Mdcr_Pymt_Amt"] = merged_df["Avg_Mdcr_Pymt_Amt"].str.replace("$", "").str.replace(",", "").astype(float)

# Convert the "Tot_Dschrgs" column to a number
merged_df["Tot_Dschrgs"] = merged_df["Tot_Dschrgs"].str.replace("$", "").str.replace(",", "").astype(float)

# Calculate the percentage of Medicare payments that are covered by Medicare
merged_df["Prct_Mdcr_Covered"] = (merged_df["Avg_Mdcr_Pymt_Amt"] / merged_df["Avg_Tot_Pymt_Amt"]) * 100

# Save the merged dataframe to a CSV file
merged_df.to_csv("merged_data.csv", index=False)

# Get a summary of the columns and data types
merged_df.info()
```
This returns the following:
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 886238 entries, 0 to 886237
Data columns (total 17 columns):
 #   Column                     Non-Null Count   Dtype  
---  ------                     --------------   -----  
 0   ï»¿Rndrng_Prvdr_CCN        886238 non-null  int64  
 1   Rndrng_Prvdr_Org_Name      886238 non-null  object 
 2   Rndrng_Prvdr_City          886238 non-null  object 
 3   Rndrng_Prvdr_St            886238 non-null  object 
 4   Rndrng_Prvdr_State_FIPS    886238 non-null  int64  
 5   Rndrng_Prvdr_Zip5          886238 non-null  int64  
 6   Rndrng_Prvdr_State_Abrvtn  886238 non-null  object 
 7   Rndrng_Prvdr_RUCA          886202 non-null  float64
 8   Rndrng_Prvdr_RUCA_Desc     886202 non-null  object 
 9   DRG_Cd                     886238 non-null  int64  
 10  DRG_Desc                   886238 non-null  object 
 11  Tot_Dschrgs                886238 non-null  float64
 12  Avg_Submtd_Cvrd_Chrg       886238 non-null  object 
 13  Avg_Tot_Pymt_Amt           886238 non-null  float64
 14  Avg_Mdcr_Pymt_Amt          886238 non-null  float64
 15  Year                       886238 non-null  object 
 16  Prct_Mdcr_Covered          886238 non-null  float64
dtypes: float64(5), int64(4), object(8)
memory usage: 114.9+ MB
```  
Although all 15 original fields remain in the dataset, the following analyses pertain only to the ones of interest mentioned above along with the Percent Medicare Covered metric.

- Initial Summary Year by Total Number of Discharges, Average Total Payment Amount, Average Medicare Payment Amount, and Percent Medicare Covered:

```python
# Group the dataframe by year
grouped_df = merged_df.groupby("Year")

# Calculate the summary statistics by Year, summing Total Discharges and getting averages for the other quantitative fields
summary_df = grouped_df.agg({
    "Tot_Dschrgs": "sum",
    "Avg_Tot_Pymt_Amt": "mean",
    "Avg_Mdcr_Pymt_Amt": "mean",
    "Prct_Mdcr_Covered": "mean"
})

# Format the "Tot_Dschrgs" column in millions
summary_df["Tot_Dschrgs"] = summary_df["Tot_Dschrgs"].astype(float).map("{:,}".format)

# Print the summary statistics
summary_df
```
Which results in the following table:  
![SummaryTable1](Images/SummaryTable1.PNG)  




---
### References 
[^1]: Blendon, R. J., Brodie, M., Benson, J. M., Altman, D. E., & Buhr, T. (2006). Americans' views of health care costs, access, and quality. The Milbank quarterly, 84(4), 623–657. https://doi.org/10.1111/j.1468-0009.2006.00463.x
[^2]: Deyo, D., Hemingway, J., & Hughes, D. (2015). Identifying Patients With Undiagnosed Chronic Conditions: An Examination of Patient Costs Before Chronic Disease Diagnosis.. Journal of the American College of Radiology : JACR.
[^3]: Hadley, J., Reschovsky, J., Corey, C., & Zuckerman, S. (2009). Medicare fees and the volume of physicians' services.. Inquiry : a journal of medical care organization, provision and financing.
[^4]: Thompson, J., & Mccue, M. (2004). Organizational and market factors associated with Medicare dependence in inpatient rehabilitation hospitals. Health Services Management Research.
[^5]: Courtney, P., Darrith, B., Bohl, D., Frisch, N., & Valle, C. (2017). Reconsidering the Affordable Care Act’s Restrictions on Physician-Owned Hospitals: Analysis of CMS Data on Total Hip and Knee Arthroplasty. The Journal of Bone and Joint Surgery.
