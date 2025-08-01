# Customer Personality Analysis Project
**Dataset:** Kaggle Customer Personality Analysis  
**Stack:** PostgreSQL   
**Engineer:** Data cleaning in Kaggle Notebook

---

## 1. Data Cleaning - Why and How

### Raw Data Issues
- File parsing error (tab-separated format)
- 24 missing Income values (1.1%)
- 3 customers aged 104-131 years (unrealistic)
- Extreme income outlier: $666,666

### Cleaning Actions Taken

**File Format Fix:** Converted from tab-separated (TSV) to comma-separated (CSV) format - original used `\t` delimiters, output uses standard `,` delimiters for PostgreSQL compatibility

**Removed Invalid Ages:** Deleted 3 customers >100 years old (clear data entry errors)

**Fixed Missing Income:** Imputed 24 missing values with median ($51,382)

**Controlled Outliers:** Capped extreme income at 99th percentile ($154,216)

**Transformed Complain Field:** Changed from numeric (0,1) to boolean-like strings: 0 → 'f', 1 → 't' for PostgreSQL compatibility

**Feature Selection:** Kept only 4 columns needed for analysis:
- ID (customer identifier)
- Year_Birth (demographics) 
- Income (segmentation)
- Complain (behavior flag)

### Results
- **Clean Dataset:** 2,237 customers × 4 features
- **Data Quality:** 100% complete, realistic ranges
- **PostgreSQL Ready:** Proper data types and structure

```sql
-- Ready for PostgreSQL import
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    year_birth INTEGER,
    income DECIMAL(10,2), 
    complain VARCHAR(1) CHECK (complain IN ('f', 't'))
);
```

**Files Created:**
- `cleaned_customer_data_final.csv`



---
**Next:** PostgreSQL database setup and data import
