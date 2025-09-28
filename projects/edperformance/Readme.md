# 🎓 ED Performance Analysis (Power BI)

A complete, portfolio-ready **Emergency Department (ED) performance** analytics project built with **Power BI**. It transforms raw visit-level data into a clean **star schema**, adds **DAX measures** for operational KPIs, and presents **interactive dashboards** for clinicians, operations, and leadership.

> Data sources in this project:
>
> - `Performance Analyst Interview Task.xlsx` (raw Excel dataset)
> - `ED_Performance_Final.pbix` (final Power BI model & report)

---

## 📊 Executive Summary (Key Insights)

From **20,457** ED presentations (Jan–Dec **2021**):

- **Admission rate:** **47.0%**
- **28-day representation rate:** **6.44%**
- **Avg ED length of stay (LOS):** **230.2 minutes** (~3.84 hours)
- **4-Hour Target compliance:** **63.15%** of presentations ≤ 240 minutes  
- **P90 / P95 LOS:** **333.6 / 358.2 minutes**
- **Arrival pattern:** Peaks 10:00–13:00; highest volumes on **weekends**

<small>All stats computed from the uploaded Excel dataset.</small>

---

## 🧱 Data Model (Star Schema)

**Facts**
- **FactEDVisits**: visit-level records  
  Columns: PatientID, Sex, Admitted, ArrivalTime, DepartureTime, TimeInED (mins), Represented28d (0/1), DateKey…

**Dimensions**
- **DimDate** (marked as Date table): Date, Year, Month, YearMonth, Weekday…
- **DimPatient** (derived): PatientID, DOB, AgeAtArrival, AgeBand, Sex
- *(Optional)* **DimOutcome**: Outcome/Disposition groups (Admitted / Not admitted)
- *(Optional)* **DimTime**: Hour of arrival, Peak/Off-peak buckets

**Relationships**
- DimDate[Date] → FactEDVisits[ArrivalDate]
- DimPatient[PatientID] → FactEDVisits[PatientID]

---

## 🛠️ ETL & Transformations (Power Query)

**Source:** `Performance Analyst Interview Task.xlsx` → *Performance Analyst dataset* sheet

Steps you can replicate in **Power Query**:
1. **Type cleanup**: Convert Arrival/Departure/DOB to `datetime`; TimeInED to `decimal`.
2. **Visit date & hour**:
   - `ArrivalDate = Date.From([arrival_time])`
   - `ArrivalHour = Time.Hour(Time.From([arrival_time]))`
3. **Age at arrival**:
   - `AgeAtArrival = Duration.Days([arrival_time]-[date_of_birth]) / 365.25`
   - **Data quality guardrails:** set `AgeAtArrival = null` if `< 0` or `> 110`.
4. **Age bands** (Conditional Column): 0–14, 15–34, 35–64, 65–79, 80+.
5. **Outcome standardisation**: map `admitted` values to `Admitted` / `Not admitted`.
6. **Date table**: generate full calendar (2019–2025) with Year, Month, MonthName, Quarter, Weekday; mark as **Date table**.
7. **Anomaly checks** (recommended):
   - `DepartureTime < ArrivalTime` → flag as data error.
   - Negative or extreme `TimeInED` → review.
8. **Load**: Close & Apply; create relationships in Model view.

---

![ED Performance Dashboard](assets/images/dashboard1.jpg)

## 📐 DAX Measures (Core KPIs)

```DAX
-- Counts
Total Visits = COUNTROWS ( FactEDVisits )
Admissions   = CALCULATE ( [Total Visits], FactEDVisits[Admitted] = "Admitted" )
Not Admitted = CALCULATE ( [Total Visits], FactEDVisits[Admitted] = "Not admitted" )

-- Rates
Admission Rate % =
DIVIDE ( [Admissions], [Total Visits], 0 )

Rep 28d % =
AVERAGE ( FactEDVisits[Represented_28d] )   -- column is 0/1

-- Length of Stay (minutes)
Avg LOS (min) =
AVERAGE ( FactEDVisits[TimeInED] )

P90 LOS (min) =
PERCENTILEX.INC ( FactEDVisits, FactEDVisits[TimeInED], 0.9 )

P95 LOS (min) =
PERCENTILEX.INC ( FactEDVisits, FactEDVisits[TimeInED], 0.95 )

-- 4-hour performance
4hr Compliance % =
DIVIDE (
    CALCULATE ( [Total Visits], FactEDVisits[TimeInED] <= 240 ),
    [Total Visits],
    0
)

-- Peaks
Visits by Hour =
VAR HourTable =
    SUMMARIZE ( FactEDVisits, FactEDVisits[ArrivalHour], "Visits", [Total Visits] )
RETURN SUMX ( HourTable, [Visits] )
