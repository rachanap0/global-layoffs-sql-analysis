# Global Layoffs Data Cleaning & EDA (MySQL)

This project analyzes **global layoffs data for 2022 and 2023** using MySQL.  
The dataset includes information on companies, locations, industries, funding, and layoffs, providing an opportunity to explore **economic trends and industry impacts**.

The workflow follows a **two-phase process**:
1. **Data Cleaning** – Transform raw data into a structured, standardized format suitable for analysis.
2. **Exploratory Data Analysis (EDA)** – Use SQL to extract meaningful insights, trends, and patterns.

## Project Structure**
├── layoffs.csv # Raw dataset
├── data_cleaning.sql # Data cleaning queries
├── EDA.sql # Exploratory data analysis queries

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
- This enabled proper date sorting and time-based analysis.

### **4. Handling Missing Values**

- For missing industries:
   - Joined the table with itself on company and location to fill blanks from matching records.
- Removed rows where both total_laid_off and percentage_laid_off were NULL — indicating no useful data.

### **5. Finalizing Clean Dataset**
Dropped helper column row_num.
The cleaned table (layoffs_staging2) became the basis for EDA.


