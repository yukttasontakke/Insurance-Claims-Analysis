Insurance Claims Analysis – Power BI Project

1.	Project Overview
This project aims to analyze life insurance claims and beneficiary feedback to provide insights into claim approvals, claim processing performance, customer satisfaction, and operational improvements.
The goal is to assist insurance companies in identifying key areas for optimizing claim handling and improving client service.

2.	Data Sources
Life_Insurance_Claims.xlsx: Contains detailed information about life insurance claims, including claim ID, policyholder information, claim amount, approval status, claim date, etc.
Beneficiary_Feedback.xlsx: Contains feedback data from beneficiaries, including satisfaction ratings and feedback comments
             
3.	Data Preparation (Power Query)
•	Imported both datasets into Power BI.
•	Performed cleaning steps:
o	Removed duplicate records.
o	Handled missing values in key fields Recommendation column and Mode of Sending the Questionary column.
o	Standardized date formats.
•	Merged Life Insurance Claims with Beneficiary Feedback using a common identifier Policy ID.

4.	Data Modeling
•	Established a one-to-one relationship between:
o	Life_Insurance_Claims (Policy ID) ↔ Beneficiary_Feedback(Policy ID)
•	Created a date hierarchy for easier time-based reporting (Year > Quarter > Month) on Date of Death, Date of Receiving the Response, Date of Sending the Questionary, Date of Penalty

 
5.	Measures and Calculations (DAX)
Created several important DAX measures, for Beneficiary_Feedback:
•	Avg Response Time (Days) = 
AVERAGEX (
Beneficiary_Feedback,
 DATEDIFF (
        Beneficiary_Feedback [Date of Sending the Questionary],
        Beneficiary_Feedback [Date of Receiving the Response],
        DAY))
•	Avg. CSAT Score = AVERAGE (Beneficiary_feedback [CSAT Score])
•	Avg. NPS Score = AVERAGE (Beneficiary_feedback [NPS Score])
•	Avg. Response Time (Days) = AVERAGEX (Beneficiary_feedback, DATEDIFF (Beneficiary_Feedback [Date of Sending the Questionary], Beneficiary_feedback [Date of Receiving the Response], DAY))
•	Reachability Rate = DIVIDE(CALCULATE(COUNTROWS(Beneficiary_feedback), Beneficiary_feedback [Reachability] = "Reachable"), COUNTROWS(Beneficiary_feedback))
•	Recommendation Rate = DIVIDE(CALCULATE(COUNTROWS(Beneficiary_feedback), Beneficiary_feedback [Recommendation (Y/N)] = "Y"), COUNTROWS(Beneficiary_feedback))
      Created Calculated columns for Beneficiary_Feedback:
•	CSAT Category = 
SWITCH (
    TRUE (),
    Beneficiary_Feedback [CSAT Score] >= 1 && Beneficiary_Feedback [CSAT Score] <= 2, "Low (1–2)",
    Beneficiary_Feedback [CSAT Score] = 3, "Neutral (3)",
    Beneficiary_Feedback [CSAT Score] >= 4 && Beneficiary_Feedback [CSAT Score] <= 5, "High (4–5)",
    "Invalid or Missing"
)


•	NPS Category = 
SWITCH (
    TRUE (),
    Beneficiary_feedback [NPS Score] >= 0 && Beneficiary_feedback [NPS Score] <= 6, "Detractor (0–6)",
    Beneficiary_feedback [NPS Score] >= 7 && Beneficiary_feedback [NPS Score] <= 8, "Passive (7–8)",
    Beneficiary_feedback [NPS Score] >= 9 && Beneficiary_feedback [NPS Score] <= 10, "Promoter (9–10)",
    "Invalid"
)

Created several important DAX measures, for Life_Insurance_Penalty:
•	Average Penalty Claim = DIVIDE ([Total Penalty Amount], [Penalized Claims])
•	Avg Payment Duration by Department = AVERAGE (Penalty [Payment Duration (days)])
•	Penalized Claims = CALCULATE(COUNTROWS(Penalty), Penalty[Penalty Paid (Y/N)] = "Y")
•	Total Penalty Amount = SUM(Penalty[Penalty Amount])
•	Total Percentage = DIVIDE(Penalty[Penalized Claims], Penalty[Total policy])
•	Total policy = COUNT(Penalty[Policy ID])

6.	Visualizations
The report includes the following visualizations:
•	KPI Cards showing Total Policy, Penalized Claims, Total Percentage, Total Penalty amount, Average Penalty Claim
•	Bar Charts for:
o	Policy ID by NPS Category
o	Policy ID by CSAT Category
•	Line Chart showing Penalized claims over Years.
•	Pie Chart to show Delay in payments vs. department.
•	Donut chart to show Top Penalty Reasons
•	Ribbon Chart to show Date of sending Questionary by months
•	Line and stacked column chart to show Date of Death and Settlement Date by type of Contract
•	Slicers for dynamic filtering by:
o	Policy ID
o	Type of Policy

7.	Key Insights
•	Higher approval rates were observed in certain policy types.
•	Average claim amount varied significantly between approved and rejected claims.
•	Customer satisfaction tended to be higher for claims processed quickly.
•	Claims were concentrated more in certain months (seasonality observed)

8.	Conclusion
This dashboard enables stakeholders to monitor claims performance and identify key improvement areas in claims processing and customer service.
By analysing the approval rates, claim amounts, and customer feedback together, the company can take strategic actions to optimize operations.

9.	Tool Used
•	Microsoft Power BI
•	Power Query Editor
•	DAX (Data Analysis Expressions)
Power BI Report below
 










