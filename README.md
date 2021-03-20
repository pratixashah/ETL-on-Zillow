# ETL Project

## Data Resources:

**1. CSVs: Sales Data(https://www.zillow.com/research/data/)**

**2. Google Sheets: Inventory Data(https://www.zillow.com/research/data/)**

**3. Web Scraping: School's Data(www.greatschools.org)**


# ETL Process:

## Extraction:

**1. CSVs(Data Resources/ZHVI dir):** 
- Zillow website provides ZHVI(Zillow Home Value Index) CSVs files. 
- It categorise Home value on Numbers of Bed Room. Here 5 files are being used each per Bed Room count (i.e. Zip_zhvi_bdrmcnt_1.csv -> data for 1 Bed Room homes). 
- Zillow provides monthly data from year 1996 in form of one cloumn per month.

|  Home Category  |       File Name        |  Data Size (Columns * Rows)  |File Size|
| --------------- | ---------------------- | -----------------------------| --------|
| 1 Bed Room      | Zip_zhvi_bdrmcnt_1.csv |      310 * 18459             | 35.3 MB |
| 2 Bed Room      | Zip_zhvi_bdrmcnt_2.csv |      310 * 26755             | 56.0 MB |
| 3 Bed Room      | Zip_zhvi_bdrmcnt_3.csv |      310 * 28800             | 63.0 MB |
| 4 Bed Room      | Zip_zhvi_bdrmcnt_4.csv |      310 * 26580             | 58.7 MB |
| 5 Bed Room      | Zip_zhvi_bdrmcnt_5.csv |      310 * 22074             | 48.8 MB |

**2. Google Sheets(https://drive.google.com/drive/folders/1SCwfsJ8WD_295HeEOx8iBrM8mtwEzM7y):** 
- It provides monthly Sales and Newly Pending Inventory Listings's Raw Data.
- It also provides Rentals data(market rate rent across a given region) with Zillow Observed Rent Index (ZORI is a repeat-rent index that is weighted to the rental housing stock to ensure representativeness across the entire market, not just those homes currently listed for-rent. The index is dollar-denominated by computing the mean of listed rents that fall into the 40th to 60th percentile range for all homes and apartments in a given region, which is once again weighted to reflect the rental housing stock)

|      Data         |             File Name             |  Data Size (Columns * Rows)  |
| ----------------- | --------------------------------- | -----------------------------|
| Sales Inventory   |  US_Sale_Inventory_Monthly        |            44 * 98           |
| Pending Inventory |  US_Pending_Inventory_Monthly     |            41 * 97           |
| Rentals           |  ZORI_AllHomesPlusMultifamily_ZIP |            89 * 2069         |

**3. Web Scraping: School's Data:** 
- It scrapes listing of around 250 public schools in San Jose distributed across more than 10 pages
- It includes detail like district name, school type, rating, grades, student per teacher ratio, total students enrolled, zip code etc
- The listing on website provides 25 schools on one page
- It extracts rating, school name from the first column of listing
- It scrapes zip code from address frm first column as well.
- 
## Transform:

**1. CSVs(Data Resources/ZHVI dir):** 
- Renames column RegionName to Zip_Code.
- Drops columns RegionID, SizeRank, RegionType. StateName
- Extractes monthly data columns from year 2015 
- Adds Column Bedroom Count with respective bedroom count
- Reorders columns
- Replaces N/A with 0
- Mergerd all 5 files in to one DataFrame with index Bedroom Count and Zip Code
- Final Output

|    All Homes    | Data Size (Columns * Rows) |
| --------------- | ------------------------ |
|  One DataFrame  | 79 * 122753 |

**2. Google Sheets(https://drive.google.com/drive/folders/1SCwfsJ8WD_295HeEOx8iBrM8mtwEzM7y):** 

- **1.Sales Inventory:**
- Drops columns RegionID, SizeRank, RegionType
- Drops row having US summary
- Extractes monthly data columns from year 2018
- Renames column StateName to State
- Extracts Region Name from Region Name with State
- Final Output -> DataFrame with Region Name and State as an index 

- **2.Pending Inventory:**
- Drops columns RegionID, SizeRank, RegionType
- Renames column StateName to State
- Extracts Region Name from Region Name with State
- Final Output -> DataFrame with Region Name and State as an index 

- **3.Rentals:**
- Drops columns RegionID, SizeRank
- Extractes monthly data columns from year 2018
- Converts columns' format from yyyy-mm to mm/dd/yyyy
- Renames column RegionName to Zip_Code
- Extracts State from MsaName(Metropolitan Statistical Area Name) to separate column named State
- Extracts MsaName from MsaName and state
- Final Output -> DataFrame with Zip Code as an index

**3. Web Scraping: School's Data:** 
- It replaces Non rated school's rating and N/A areas to 0
- Final Result -> DataFrame with School name and Zip Code as an index

## Load:
- It loads all the transformed data in to PostgreSQL database named zillow_db
- At first, It checks whether zillow_db database is already exists or not
- If its not then, it will create a New database with name zillow_db
- It will connects to zillow_db database to create all the tables
- One by one it will create tables and provides aknowledgement for the same

|    Table Name     |         Data               |    Data Size (Columns * Rows)    |
|-------------------| -------------------------- | ---------------------------------|
|  sales            |   Sales                    |    79 * 122753                   |
|  states           |   State Names              |    59 * 2                        |
|  inventory_sales  |   Sales Inventory          |    44 * 98                       |
|  inventory_pending|   Newly Pending Inventory  |    41 * 97                       |
|  rentals          |   Rental Inventory         |    89 * 2069                     |
|  schools          |   Schools' listing         |    6 * 246                       |





 
