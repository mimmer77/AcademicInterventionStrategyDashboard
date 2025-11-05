# 📈 Academic Intervention Strategy: Closing the Socioeconomic Achievement Gap

## Project Overview
This Power BI project provides a clear, data-driven strategy to maximize the Return on Investment (ROI) of an academic intervention program. The goal was to identify the root cause of a significant performance disparity and recommend a targeted resource allocation plan.

The final deliverable is an interactive dashboard that moves beyond simple reporting to deliver a single, clear strategic imperative for school administration.

## 📊 Key Findings & Strategic Imperative

The analysis established a clear causal link between socioeconomic status (SES) and performance, leading to an actionable recommendation:

* **The Problem (The Gap):** Students classified as **Lower SES** lag behind **Higher SES** students by an **8.64 point average score gap**. This gap represents the primary intervention target.
* **The Solution (The Lift):** The existing 'Test Preparation Course' intervention delivered a significant average performance **lift of 7.63 points** when completed.
* **Strategic Imperative:** The data model dictates that resources must be *aggressively reallocated* to maximize Test Prep completion within the **Lower SES** student segment to effectively close the achievement gap.

## 🛠️ Technical Components

This dashboard was built using **Power BI Desktop** and leverages the following techniques:

| Component | Technique | Purpose |
| :--- | :--- | :--- |
| **Data Transformation** | Power Query (M) | Data cleaning, data typing, and creating the `Lunch_Indicator` replacement logic (`0` to "Higher SES", `1` to "Lower SES") for clear visualization. |
| **Data Modeling** | Star Schema | Created a one-to-many relationship between the main fact table and a separate `Intervention_Lookup` dimension for clean DAX calculations. |
| **Visual Design** | Focused KPI Cards | Used conditional formatting and simplified visual design to clearly highlight the two core numbers (8.64 and 7.63) which drive the recommendation. |

## 📐 Key DAX Measures

The following custom DAX measures were critical to isolating the lift and quantifying the gap:

```dax
-- 1. Measures for Average Performance by Segment
[Average Score] = AVERAGEX( 'Scores', 'Scores'[Math Score] + 'Scores'[Reading Score] + 'Scores'[Writing Score] ) / 3

-- 2. The Core Gap Calculation
[Socio.Eco.Gap] = 
    ABS( 
        CALCULATE( [Average Score], 'Demographics'[Lunch Indicator] = "Higher SES" ) 
        - 
        CALCULATE( [Average Score], 'Demographics'[Lunch Indicator] = "Lower SES" ) 
    )

-- 3. The Intervention ROI (Test Prep Lift)
-- Calculates the lift provided by Test Prep for the target group (Lower SES)
[Test Prep Lift] = 
    CALCULATE( [Average Score], 'Intervention'[Test Prep Status] = "Completed", 'Demographics'[Lunch Indicator] = "Lower SES" ) 
    - 
    CALCULATE( [Average Score], 'Intervention'[Test Prep Status] = "None", 'Demographics'[Lunch Indicator] = "Lower SES" )
