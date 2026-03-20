# 📊 **ETL Excel to PostgreSQL using Docker**
This project aims to do data cleaning using pandas and numpy and storing it in PostgreSQL database using Docker

## 📍 **The main processes in this project include:**
1. **Downloading the Dataset**, Fetching data from reliable sources
2. **Data Transformation**, Using VS Code or Jupyter Notebook to converting time formats and adjusting the data schema to fit PostgreSQL
3. **Running PostgreSQL with Docker**, Setting up the database environtment using containers
4. **Connecting to PostgreSQL**, Using SQLAlchemy to interact with the database
5. **Uploading Data to PostgreSQL**, Storing data in the defines tables and verifying the upload results

## 🗃️ **Dataset**
Data sources : (https://github.com/fatwaman/ETL_Excel_with_Pandas/blob/main/Shift%20Report%20Tipe%20A%20Februari%202026.xlsx)

## ✅ **Step by Step**
1. Import required library
   ```bash
   import pandas as pd
   import numpy as np
   from datetime import datetime, timedelta, date # to define production date, create label data that we want to extract
   from sqlalchemy import create_engine
   ```
2. Data cleaning and transformation using *Jupyter Notebook*
   ```bash
   # Load data from excel file
   df = pd.read_excel

   # Drop unnecessary colums
   df.drop

   # Rename columns to make easier to work with
   df.rename

   # Change column names to lowercase
   df.columns = df.columns.str.lower()
   df.columns = df.columns.str.replace(" ", "_") # optional to replace spaces with underscores for easier access
   df.columns = df.columns.str.strip()

   # Fill in empty values (NaN)
   df['column_name'] = df['column_name'].ffill()

   # Create new column for categorization
   df['new_column'] = np.where(df['product'].str.contains("Feed", case=False, na=False), "Cat-1", "Cat-2")

   # Pivot Data
   df_melted_prod = df.melt(id_vars=['column_1', 'column_2', 'column_3'],
                    value_vars=['value_1', 'value_2', 'value_3'],
                    var_name='value_category',
                    value_name='value')
   ```
3. *Running PostgreSQL with Docker Run PostgreSQL* using Docker with the following commnad in your terminal:
   ```bash
   docker run -d --name "container name" \
   -e POSTGRES_USER="admin" \
   -e POSTGRES_PASSWORD="admin123" \
   -e POSTGRES_DB="database_name" \
   -p 5432:5432 \
   postgres
   ```
4. Create engine to connect to PostgreSQL database using SQLAlchemy
   ```bash
   engine = create_engine(f"postgresql+psycopg2://{DB_USER}:{DB_PASS}@{DB_HOST}:{DB_PORT}/{DB_NAME}")
   ```
5. Import data to PostgreSQL
   ```bash
   df_finalize.to_sql(name="table_name", con=engine, if_exists="replace", index=False)
   ```
 Your data is successfully ingested into PostgreSQL🎉




