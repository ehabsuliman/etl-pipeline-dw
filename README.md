# Skaila ETL Pipeline `Ehab Suliman - 12113027`

This project builds a data warehouse for the Sakila movie rental business. The source data comes from a MySQL OLTP database designed for daily transactions like rentals, payments, and customer management. Since transactional schemas are not ideal for analytics, the data is transformed into a star schema that supports reporting on revenue, rentals, customer activity, and store performance.

The pipeline uses Python, Pandas, and SQLAlchemy. The warehouse is stored in SQLite as `warehouse.db`.

The warehouse answers key business questions such as:
- Which films are rented the most
- Which films are returned late most often
- Which stores generate the highest revenue
- Which customers spend the most
- How revenue changes over time

Analysis showed that **Bucket Brotherhood** was the most rented film with 34 rentals, while **Ridgemont Submarine** had the highest number of late returns. Revenue between the two stores was nearly identical, with both generating around \$33k total revenue. Monthly revenue peaked in July 2005 before dropping sharply in February 2006. The highest-spending customer was **Karl Seal** with \$221.55 in total payments.

The warehouse follows a star schema design with two fact tables and five dimension tables.

- `fact_rental` stores rental transactions and tracks rental duration, return status, and late returns.
- `fact_payment` stores payment transactions and payment amounts.

The dimension tables provide descriptive business context:
- `dim_date` contains calendar attributes
- `dim_customer` stores customer and location details
- `dim_film` stores film, category, and language information
- `dim_store` stores store locations
- `dim_staff` stores employee information

The ETL pipeline extracts data from 15 Sakila source tables into Pandas DataFrames. During transformation, missing values were cleaned, tables were joined, surrogate keys were generated, and operational data was reshaped into analytical dimensions and facts. Examples include combining customer names into a `full_name` field, enriching films with category and language information, and calculating rental durations and late returns.

Dimensions are loaded first, followed by fact tables to maintain referential integrity. After loading, the warehouse contains:
- 90 date records
- 599 customers
- 1,000 films
- 2 stores
- 2 staff members
- 16,044 rental records
- 16,044 payment records

The final warehouse consolidates normalized transactional data into a clean analytical model that supports fast business reporting without querying the live operational database.