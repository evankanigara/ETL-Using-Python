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
