# CMS Medicare Inpatient Charge Analysis  
### Case Study Using CMS Open Source Inpatient Data from 2017-2021 (Python)
##### - [Background and Research Questions](#background-and-research-questions)
##### - [Statement of Business Task](#statement-of-business-task)
##### - [Description of Data Sources](#description-of-data-sources)

#### - [References](#references)
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
- The open source [CMS (Centers for Medicare and Medicaid Services) Inpatient data](https://data.cms.gov/provider-summary-by-type-of-service/medicare-inpatient-hospitals/medicare-inpatient-hospitals-by-provider) provides information on hospital discharges from Original Medicare (fee-for-service) Part A (Hospital Insurance) beneficiaries by Inpatient Prospective Payment System (IPPS) hospitals. It includes information on the use, payment, and hospital charges for more than 3,000 U.S. hospitals that received IPPS payments.
- The CMS provides a [data dictionary](Data 
---
### References 
[^1]: Blendon, R. J., Brodie, M., Benson, J. M., Altman, D. E., & Buhr, T. (2006). Americans' views of health care costs, access, and quality. The Milbank quarterly, 84(4), 623–657. https://doi.org/10.1111/j.1468-0009.2006.00463.x
[^2]: Deyo, D., Hemingway, J., & Hughes, D. (2015). Identifying Patients With Undiagnosed Chronic Conditions: An Examination of Patient Costs Before Chronic Disease Diagnosis.. Journal of the American College of Radiology : JACR.
[^3]: Hadley, J., Reschovsky, J., Corey, C., & Zuckerman, S. (2009). Medicare fees and the volume of physicians' services.. Inquiry : a journal of medical care organization, provision and financing.
[^4]: Thompson, J., & Mccue, M. (2004). Organizational and market factors associated with Medicare dependence in inpatient rehabilitation hospitals. Health Services Management Research.
[^5]: Courtney, P., Darrith, B., Bohl, D., Frisch, N., & Valle, C. (2017). Reconsidering the Affordable Care Act’s Restrictions on Physician-Owned Hospitals: Analysis of CMS Data on Total Hip and Knee Arthroplasty. The Journal of Bone and Joint Surgery.
