# 🧹 Tech Layoffs Data Analysis (2020–2023)

## 📌 Project Overview
Cleaned and analyzed a real-world dataset of tech industry layoffs 
using MySQL. The project covers full data cleaning pipeline and 
exploratory data analysis to uncover trends.

## 🛠️ Tools Used
- MySQL
- SQL (CTEs, Window Functions, Joins, Aggregations)

## 📂 Files
| File | Description |
|------|-------------|
| `layoffs.csv` | Raw dataset |
| `layoffs_analysis.sql` | Full SQL script (cleaning + EDA) |

## 🔧 Data Cleaning Steps
1. Removed duplicate records using `ROW_NUMBER()` window function
2. Trimmed whitespace from company names
3. Standardized inconsistent values (e.g., "Crypto", "United States")
4. Converted date column from text to DATE format
5. Filled missing industry values using self-join
6. Removed rows with no layoff data

## 📊 Key Insights (EDA)
- **Amazon, Google, Meta** had the highest total layoffs
- **Consumer & Retail** industries were hit hardest
- Layoffs peaked in **2022–2023**
- Used rolling monthly totals to track cumulative trends
- Ranked top 5 companies per year using `DENSE_RANK()`

## 📈 Sample Queries
```sql
-- Top 5 companies by layoffs per year
WITH company_year AS (
  SELECT company, YEAR(date), SUM(total_laid_off) AS total_layoffs
  FROM layoffs_staging2
  GROUP BY company, YEAR(date)
),
Company_Year_Rank AS (
  SELECT *, DENSE_RANK() OVER (PARTITION BY years 
  ORDER BY total_laid_off DESC) AS Ranking
  FROM company_year WHERE years IS NOT NULL
)
SELECT * FROM Company_Year_Rank WHERE Ranking <= 5;
```

## 🔗 Dataset Source
[Kaggle - Tech Layoffs Dataset](https://www.kaggle.com/datasets/swaptr/layoffs-2022)
