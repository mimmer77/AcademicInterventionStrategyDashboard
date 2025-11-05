# 📈 Academic Intervention Strategy: Closing the Socioeconomic Achievement Gap

## Data Transformations (Feature Engineering)

The initial dataset was highly clean, so the primary focus was on **Feature Engineering** to create more analytically powerful binary variables for the dashboard and statistical modeling.

| Original Column | New Column | Transformation | Notes |
| :--- | :--- | :--- | :--- |
| `test preparation course` | **`Test_Prep_Completed`** | Replaced text values (`completed`, `none`) with **Whole Numbers** | **1 = Course Completed; 0 = No Course Taken.** Used to measure the efficacy and lift of the intervention. |
| `lunch` | **`Lunch_Standard_Indicator`** | Replaced text values (`standard`, `free/reduced`) with **Whole Numbers** | **1 = Standard Lunch (Higher SES). 0 = Free/Reduced Lunch (Lower SES).** *The final report visuals use the text labels "Higher SES" and "Lower SES" for clarity.* |
| `gender` | **`Gender_Is_Female`** | Replaced text values (`female`, `male`) with **Whole Numbers** | **1 = Female; 0 = Male.** Used to directly quantify gender-based performance gaps. |
| `parental level of education` | **`Parent_Post_Secondary_Education`** | Grouped original text values into a binary indicator. | **1 = Parent has a degree (Associate's or higher); 0 = Parent has not completed a degree.** Simplifies the impact analysis of parental education. |
| Math, Reading, Writing Scores | **`Average_Score`** | Calculated mean of the three subjects and rounded to 2 decimals. | Serves as the **central numerical metric** for overall student performance. |
| `Average_Score` | **`Performance_Segment`** | Conditional logic applied in Power Query based on grade cutoffs. | **Categorizes students into High ($\geq 80$), Medium ($60-79$), or Needs Intervention ($< 60$).** Key dimension for intervention targeting. |
| `race/ethnicity` | **(Direct Use as Category)** | N/A | Used directly as a **Categorical Dimension** to analyze **academic performance disparities** between the five anonymized groups (A-E). |

---

## 🛠️ Key DAX Measures & Gap Analysis

The following calculated measures were engineered in DAX to serve as the core metrics and gap analyses presented on the dashboard. They quantify the problem and the efficacy of the proposed solution.

| DAX Measure | Purpose | Formula / Rationale |
| :--- | :--- | :--- |
| **`_Total Students`** | Total count of unique student records. | `COUNTROWS('StudentsPerformance')` |
| **`_Overall Avg Score`** | The average score of all students. | `AVERAGE('StudentsPerformance'[Average_Score])` |
| **`% of Total Students`** | Calculates the percentage of students in a given segment (e.g., Needs Intervention). | `DIVIDE([_Total Students], CALCULATE([_Total Students], ALL('StudentsPerformance')))` |
| **`_Soc.Eco.Gap`** | Measures the score difference between High SES and Low SES segments. **(The 8.64 point difference)** | `ABS( CALCULATE( [Average_Score], 'Demographics'[Lunch_Standard_Indicator] = 1 ) - CALCULATE( [Average_Score], 'Demographics'[Lunch_Standard_Indicator] = 0 ) )` |
| **`_TestPrepLift`** | Quantifies the score improvement attributed to the Test Prep course in the target group. **(The 7.63 point lift)** | `CALCULATE( [Average_Score], 'Intervention'[Test_Prep_Completed] = 1, 'Demographics'[Lunch_Standard_Indicator] = 0 ) - CALCULATE( [Average_Score], 'Intervention'[Test_Prep_Completed] = 0, 'Demographics'[Lunch_Standard_Indicator] = 0 )` |
| **`_GenderGap`** | Measures the score difference between Female and Male students. | `ABS([Avg Score Female] - [Avg Score Male])` |

***

### Project Status
**COMPLETED:** Ready for portfolio presentation.
