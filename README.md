# Global Layoffs Data Cleaning & EDA (MySQL)

This project analyzes **global layoffs data for 2022 and 2023** using MySQL.  
The dataset includes information on companies, locations, industries, funding, and layoffs, providing an opportunity to explore **economic trends and industry impacts**.

The workflow follows a **two-phase process**:
1. **Data Cleaning** – Transform raw data into a structured, standardized format suitable for analysis.
2. **Exploratory Data Analysis (EDA)** – Use SQL to extract meaningful insights, trends, and patterns.

## Project Structure**
- layoffs.csv # Raw dataset
- data_cleaning.sql # Data cleaning queries
- EDA.sql # Exploratory data analysis queries

## Dataset Overview
**Source:** layoffs.csv  
**Rows:** 2,000+ (after cleaning)  
**Timeframe:** 2022–2023  
**Columns:**
- `company` – Company name
- `location` – Office location
- `industry` – Industry sector
- `total_laid_off` – Number of employees laid off
- `percentage_laid_off` – % of workforce laid off
- `date` – Layoff date
- `stage` – Company funding stage
- `country` – Country name
- `funds_raised_millions` – Total funds raised (in millions)

## Data Cleaning Process 
**Source:** data_cleaning.sql
The raw dataset contained duplicates, inconsistent formats, and missing values.  

Steps taken:

### **1. Duplicate Removal**
- Created a **staging table** to work on data without modifying the original.
- Used `ROW_NUMBER() OVER (PARTITION BY ...)` to identify exact duplicate rows based on:
company, location, industry, total_laid_off, percentage_laid_off, date, stage, country, funds_raised_millions
- Deleted rows where `row_num > 1`.

### **2. Standardizing Text Fields**
- **Company names** – Trimmed extra spaces using `TRIM(company)`.
- **Country names** – Removed trailing punctuation (e.g., `"United States." → "United States"`).
- **Industry names** – Checked for inconsistencies and unified variations.


### **3. Converting Dates**
- Original `date` column stored as text in `MM/DD/YYYY` format.
- Converted to proper `DATE` type using:
```sql
STR_TO_DATE(date, '%m/%d/%Y')
```
- This enabled proper date sorting and time-based analysis.

### **4. Handling Missing Values**

- For missing industries:
   - Joined the table with itself on company and location to fill blanks from matching records.
- Removed rows where both total_laid_off and percentage_laid_off were NULL — indicating no useful data.

### **5. Finalizing Clean Dataset**
Dropped helper column row_num.
The cleaned table (layoffs_staging2) became the basis for EDA.

## Exploratory Data Analysis (EDA.sql)
After cleaning, we explored the dataset with SQL to uncover trends.
**1. Layoff Scale Analysis**
Max total layoffs and max percentage layoffs across all companies.
Identified companies with 100% layoffs and sorted them by the number of employees affected.

**2. Industry Impact**
- Aggregated total layoffs per industry.
- Found which industries were hit hardest (e.g., tech, finance, consumer goods).

**3. Geographic Trends**
- Summed layoffs by country to see where job cuts were most severe.
- Compared layoffs in North America, Europe, and Asia.

**4. Time-Based Trends**
- Earliest and latest layoffs in the dataset to establish the active time range.
- Yearly breakdown (2022 vs. 2023) to see how layoffs evolved.
- Monthly trends:
   - Grouped by SUBSTRING(date, 1, 7) (YYYY-MM) for a timeline view.
   - Created a rolling total to visualize cumulative layoffs over time.
 
**5. Company-Level Insights**
- Ranked companies by total layoffs per year using DENSE_RANK().
- Extracted the top 5 companies with the highest layoffs in each year.

## Key Insights from EDA
- Tech sector saw the largest layoffs both in volume and percentage.
- Layoffs peaked in mid-to-late 2022 and early 2023.
- Certain companies executed full workforce layoffs (100%).
- The United States had the highest layoff numbers, followed by countries with large tech hubs.
- Rolling totals show a steep rise during certain months, possibly linked to market downturns or funding issues.

## Tools Used
MySQL – Data cleaning and analysis.
MySQL Workbench – Running queries and data visualization.
CSV Data Import – For loading raw data into MySQL.

## **How to Run**
- Clone the repository:
```bash
git clone https://github.com/rachanap0/global-layoffs-sql-analysis.git
```
- Import layoffs.csv into MySQL.
- Run data_cleaning.sql to prepare the cleaned dataset.
- Run EDA.sql to reproduce the insights.




