# ETL-Using-Python
ETL From Scratch with Python (Scrapping Wikipedia)

This project demonstrate extract, transform, and load (ETL) process using Python. The data is extracted from [Wikipedia](https://id.wikipedia.org/wiki/Daftar_miliarder_Forbes). Then it's tranformed and cleaned by using python library such as Pandas. Finally, the cleaned data is loaded in the Postgres Database (local & cloud). You can see the full project in the ipynb file above.

The flowchart of the project: 
<img src='/flowchart.jpg'>

## Project Step
The step for the project are
1. Extract the Data from Wikipedia
2. Transform the Data
3. Load the Data to the Local Database (Cloud Optional)
4. Read the Data from the Local Database (Cloud Optional)


### Extract the Data
To start the project, we need to import Pandas and Logging. Pandas are used to extract the data from the Wikipedia and load the data in the Databases. While logging provides the facility to work with the framework for releasing log messages from the Python programs. To extract the data:

    def scrape(url):
      logging.info(f"Scraping website with url: '{url}' ...")
      return pd.read_html(url, header=None)
      
The data that will used are from Wikipedia website `https://id.wikipedia.org/wiki/Daftar_miliarder_Forbes`. It contains the list of billionaire from the Forbes Magazine. 

### Transform the Data
The data from the Wikipedia will be transformed into these feature:
- Nama
- Nomor urut
- Kekayaan bersih USD juta
- Usia
- Kebangsaan
- Sumber kekayaan
- Tahun

To transform the money format from `$91.5 miliar` to 91500.0 we will use the Regex function.

    def transform_money_format(string_money):
        half_clean_string = string_money.lower().replace(" ", "")
        return re.sub(r"[?\[M\]miliar|\[$\]]", "", half_clean_string)
        
### Load the Data
To load the data from the Pandas dataframe to the PostgreSQL database we will use _sqlalchemy_ module. It will create the engine needed to store the data into the database. To actually load the data to the database, we will use `df.to_sql` function. All of these function will be glued together in the new function that called *write_to_postgres*. Don't forget to specify the table name for the local database.

        def write_to_postgres(df, db_name, table_name, connection_string):
            engine = create_engine('postgresql+psycopg2://postgres:120997@localhost/postgres')
            logging.info(f"Writing dataframe to database: '{db_name}', table: '{table_name}' ...")
            df.to_sql(name = table_name, con=engine, if_exists="replace", index=False)
            
While to load the data to the cloud databases, simply change the `CONNECTION_STRING` to the desired cloud databses. You should specify: 
- DB_NAME
- DB_USER
- DB_PASSWORD 
- DB_HOST
- DB_PORT

### Read the Data
To read the data that already stored in the local/cloud databases you are also need to make the SQL engine to connect to the databases. To actually read from the datases, it will use the `pd.read_sql_table` function. All of these will be written inside the *read_from_postgres* function.

        def read_from_postgres(db_name, table_name, connection_string):
            engine = create_engine('databaseinfo)
            logging.info(f"Reading postgres database: ...")
            return pd.read_sql_table("Bernard-Evan-Kanigara_orang_terkaya_forbes", con=engine)

The result in the local PostgreSQL database:

<img src="https://camo.githubusercontent.com/707cada21eed2d184a53bb96b774beb1b2508e493ef60411b0c1cd7fd2cbbc34/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f6576616e6b616e69676172612f45544c2d5573696e672d507974686f6e2f6d61696e2f64617461626173655f6c6f6b616c2e504e47" alt="Result Table" width="1000"/>

## Contact
- LinkedIn: [Bernard Evan Kanigara](https://www.linkedin.com/in/bernard-evan-kanigara/)
- Email: bernardevankanigara@gmail.com

Thank you!
