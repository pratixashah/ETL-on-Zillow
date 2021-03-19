# ETL Project

## Data Resources:

**1. CSVs: Sales Data(https://www.zillow.com/research/data/)**
**2. Google Sheets: Inventory Data(https://www.zillow.com/research/data/)**
**3. Web Scraping: School's Data(www.greatschools.org)**

# ETL Process:

## Extraction:

**1. CSVs(Data Resources/ZHVI dir):** Zillow website provides ZHVI(Zillow Home Value Index) CSVs files. 
- It categorise on Numbers of Bed Room. Here 5 files are being used each per Bed Room count (i.e. Zip_zhvi_bdrmcnt_1.csv -> data for 1 Bed Room homes). 
- Zillow provides monthly data from year 1996 in form of one cloumn per month, total 300 columns.

- 1 Bed Room Data - Zip_zhvi_bdrmcnt_1.csv - Columns - 310 * Rows - 18459
- 2 Bed Room Data - Zip_zhvi_bdrmcnt_2.csv - Columns - 310 * Rows - 26755
- 3 Bed Room Data - Zip_zhvi_bdrmcnt_3.csv - Columns - 310 * Rows - 28800
- 4 Bed Room Data - Zip_zhvi_bdrmcnt_4.csv - Columns - 310 * Rows - 26580
- 5 Bed Room Data - Zip_zhvi_bdrmcnt_5.csv - Columns - 310 * Rows - 22074

- Renames column RegionName to Zip_Code.
- Drops columns RegionID, SizeRank, RegionType. StateName
- Extractes monthly data columns from year 2015 
- Adds Column Bedroom Count with respective bedroom count
- Reorders columns
- Replaces N/A with 0
- Mergerd all 5 files in to one DataFrame with index Bedroom Count and Zip Code





 
