🛒 Walmart Analytics Data Pipeline 


(Airflow + dbt + Medallion Architecture)


An end-to-end modern data engineering pipeline that ingests Walmart retail transaction records, stages and models them using dbt with Medallion Architecture (Bronze ➔ Silver ➔ Gold), maintains historical tracking via dbt Snapshots (SCD Type 2), and orchestrates workflows with Apache Airflow via Docker Compose.


[ Raw CSV Files ]

(customers, stores, products, orders, order_items, employees)

       │
       ▼  (Ingestion via load_data.py / walmart_schema.sql)
       
[ Source Layer ] (Database raw schemas & tables)

       │
       ▼  (dbt snapshots: SCD Type-2 tracking)
       
[ Dimensions & Snapshots ] (dim_customers, dim_products, dim_stores, dim_employees, dim_orders)

       │
       ▼  (Silver Layer Transformations)
       
[ Silver Tier ]

   ├── silver_t/  (Cleaned, typed staging models: customers_t, products_t, etc.)
   
   └── silver_b/  (One Big Table / Denormalized view: obt_b)
       │
       
       ▼  (Gold Tier Analytics Layer)
[ Gold Tier ]

   ├── ephemeral/ (Intermediate models: eph_customers, eph_orders, etc.)
   
   └── fact/      (Aggregated analytics star schema: fact_orders)
       │
       
       ▼
[ Airflow Orchestrator (orchestrate.py) ] ➔ 
Automated execution & test assertions (test_obt.sql)
