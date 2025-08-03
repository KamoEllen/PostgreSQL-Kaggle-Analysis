# ERP Analytics Pipeline

# Data Cleaning Process

## Dataset Selection
Amazon Delivery Dataset: 43,632 logistics records with order details, agent performance, and delivery timestamps. Chosen because it contains operational data similar to what ERP systems handle - agent performance tracking, delivery scheduling, and SLA monitoring.

## Data Issues Identified
Initial assessment revealed several data quality problems:
- Missing values in key columns (Order_ID, timestamps, agent ratings)
- Alphanumeric Order_IDs not suitable for database primary keys
- Mixed data types requiring standardization
- Need for derived business metrics (on-time delivery tracking)

## Cleaning Approach

### Schema Design
Extracted four columns for ERP-style operational tracking:
```sql
CREATE TABLE amazon_deliveries (
    id BIGINT,           -- Converted from alphanumeric Order_ID
    order_date TEXT,     -- Original order date
    agent_rating TEXT,   -- Agent performance score
    ontime_flag TEXT     -- Derived delivery performance metric
);
```

### Key Transformations
**ID Generation**: Converted alphanumeric Order_IDs to numeric using hash function for database compatibility

**Data Cleaning**: Removed records with missing values in critical fields

**Business Logic**: Created `ontime_flag` by comparing Order_Time and Pickup_Time:
- "true" if pickup within 20 minutes of order
- "false" otherwise
- Handles parsing errors and edge cases

**Format Standardization**: Converted all non-ID fields to TEXT for PostgreSQL compatibility

## Output
- Cleaned CSV dataset with validated schema
- SQL export with CREATE TABLE and batched INSERT statements (1,000 records per batch)
- Comprehensive cleaning report with detailed quality metrics

The cleaned dataset is prepared for PostgreSQL import as part of a larger ERP demonstration pipeline.
