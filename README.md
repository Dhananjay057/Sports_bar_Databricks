# Sports_bar_Databricks

This README file provides a comprehensive overview of the End-to-End Data Engineering Project designed to consolidate data from a parent company (Atlon) and its newly acquired subsidiary (Sports Bar) within the FMCG domain.
FMCG Data Engineering Project: Atlon & Sports Bar Merger
1. Project Overview
The primary objective of this project is to resolve the "data chaos" resulting from the merger of Atlon, a sports equipment manufacturer, and Sports Bar, an energy bar startup. While Atlon has a mature data infrastructure, Sports Bar's data is fragmented across spreadsheets, cloud drives, and various exports. This project builds a reliable, scalable, and unified data layer to provide leadership with consolidated analytics via a single dashboard.
2. Technical Stack
• Platform: Databricks Free Edition (Community Edition).
• Storage: AWS S3 (used as a data lake).
• Languages: Python (PySpark) and SQL.
• Architecture: Medallion Architecture (Bronze, Silver, and Gold layers).
• AI & Analytics: Databricks Dashboard and Genie (AI Assistant).
3. Data Model
The project utilizes a Star Schema to organize information efficiently for analytical processing.
• Fact Table: fact_orders (contains transaction dates, product IDs, customer IDs, and quantities).
• Dimension Tables:
    ◦ dim_customers: Customer names, markets, platforms, and channels.
    ◦ dim_products: Product IDs, categories, and divisions.
    ◦ dim_gross_price: Pricing information per product/year.
    ◦ dim_date: A scripted dimension table for time-based analytics (quarters, months).
4. Medallion Pipeline Architecture
Bronze Layer (Raw Data)
• Parent Data: Manually imported from CSV files directly into the Gold layer (simulating an existing established pipeline).
• Child Data: Ingested from AWS S3 buckets into Databricks.
• Metadata: Added columns for read_timestamp, file_name, and file_size for audit tracking and lineage.
Silver Layer (Cleaned Data)
This layer performs several critical transformations:
• Deduplication: Dropping duplicate records based on unique IDs.
• Standardization: Trimming leading spaces, fixing typos (e.g., "Bangalore" vs. "Bangaluru"), and using init_cap for consistent naming.
• Schema Alignment: Converting IDs to strings (categorical data) and renaming columns to match the parent company's schema.
• Derived Keys: Generating unique surrogate keys using SHA hashing for products where IDs were unreliable.
• Quality Checks: Handling null values in city fields and converting negative prices to positive.
Gold Layer (Business Ready)
• Consolidation: Merging cleaned child data with the parent tables using Upsert (Merge) operations to prevent duplicate entries.
• Aggregations: Converting daily-level child transactions into monthly-level records to align with Atlon’s historical reporting.
• Unified View: Creating a large denormalized view that joins all dimension and fact tables into a single "flat" table for high-speed dashboarding.
5. Incremental Loading & Orchestration
• Staging Strategy: To handle daily updates (e.g., December transactions), a staging table is used to process only the new incremental files without re-processing historical data.
• File Management: Once processed, files are moved from the "landing" folder in S3 to a "processed" folder.
• Jobs & Pipelines: Tasks are orchestrated in a specific sequence (Customers -> Products -> Prices -> Fact Orders) using Databricks Jobs to run automatically.
6. Analytics & Insights
• Databricks Genie: Allows users to ask natural language questions (e.g., "What are the top 5 products by revenue?") which the AI converts into SQL queries.
• Business Dashboard: Provides KPIs such as Total Revenue, Total Quantity, and Revenue Share by Channel, with filters for Year, Quarter, and Market.
7. Success Criteria Met
1. Consolidated Analytics: A single dashboard now shows aggregated data for both Atlon and Sports Bar.
2. Ease of Use: The toolset is designed for quick adoption by data professionals.
3. Scalability: The Medallion architecture and Databricks platform provide a long-term, scalable solution for future company acquisitions
