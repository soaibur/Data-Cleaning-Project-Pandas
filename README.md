# Data Cleaning with Pandas
The dataset is collected from https://www.oica.net/wp-content/uploads/pc_sales_2021.xlsx. It contains data on passenger car sales across different countries for the years 2019, 2020, and 2021. I am going to create a tidy dataframe with four columns: Country, 2019, 2020, 2021. The .xlsx file was saved as .csv file for convenience.

## Import Pandas
```bash
import pandas as pd
```

## Read CSV file without header
I am reading it without header because I do not know whether this file has proper column names. 
```bash
df = pd.read_csv("pc_sales_2021.csv", header=None)
df.head()
```
The output shows that the desired heading row is at 2nd position.
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/34d91899-a5ad-43c7-bce5-b580fb75a6f9)

## Turn a specific row into header row and drop that row
```bash
df.columns = df.iloc[2]
df = df.drop(2)
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/9c7b340e-a579-458c-aebe-be41e3c13fe1)

## Drop the 0th row 
It is just a headline!
```bash
df = df.drop(0)
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/43f0fd59-cee0-40a2-9107-55f928598a08)

## Drop last two columns
Percent growth columns are not necessary.
```bash
df = df.drop(["2021/\n2020","2021/2019"], axis = 1)
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/7b14a730-6a0f-48d8-a4e3-a427cacf46aa)

## Drop rows where all values are NaN
Get rid of the completely blank rows.
```bash
df = df.dropna(how='all')
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/91755228-6ed8-4e41-88e6-7045432292f9)

## Check other null values 
```bash
df.isnull().any()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/36584e6c-d8ee-4b09-b205-2d304fc3821b)

Great! No missing value.

## Rename columns : Country, 2019, 2020, 2021
```bash
df = df.rename(columns={'REGIONS/COUNTRIES': 'Country', 'Q1-Q4 2019': '2019', 'Q1-Q4 2020': '2020', 'Q1-Q4 2021': '2021'})
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/8fac134d-ab52-4195-9f69-b33546458a69)

## View the whole table and make a list of entries under 'Country' that are not country
I was careful of the whitespaces during making this list.
```bash
pd.set_option('display.max_rows', None)
df
non_countries= ["EUROPE", "EU 27 countries + EFTA + UK", "OTHER COUNTRIES", "RUSSIA, TURKEY & OTHER EUROPE", "OTHER COUNTRIES/REGIONS ", "AMERICA", "NAFTA", "CENTRAL & SOUTH AMERICA", "ASIA/OCEANIA/MIDDLE EAST", "ASEAN",  "AFRICA", "ALL COUNTRIES/REGIONS", "TOTAL OICA MEMBERS "]
```

## Delete the rows that have any of the items of 'non_countries' list
```bash
df = df[~df['Country'].isin(non_countries)]
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/b7f12388-0495-4007-ab0a-5f3c055421fe)

## Capitalize only the first letter of each country
```bash
df.loc[:, 'Country'] = df["Country"].str.title()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/a9e893d6-4276-4900-bb3a-ac4cdd6a3cb8)

## Remove all leading and trailing spaces & everything other than capital letters, small letters, and blank spaces (in country column)
This time I want to look at the whole data frame.
```bash
df.loc[:, 'Country'] = df['Country'].str.replace(r'[^a-zA-Z\s]', '', regex=True)
df = df.applymap(lambda x: x.strip() if isinstance(x, str) else x)
df
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/6d86ee17-0c56-4df3-b9aa-c6681796ebf8)

There is an issue with the USA. o of 'of' has becocme capitalized.

## Rename United States Of America
```bash
df.loc[df['Country'] == 'United States Of America', 'Country'] = 'United States of America'
df
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/4cae0fe2-bafd-47a9-80b3-c0489a5d0c33)

Now it is okay!

## Remove commas
Removing commas from numbers is required for analysis. 
```bash
df['2019'] = df['2019'].str.replace(',', '')
df['2020'] = df['2020'].str.replace(',', '')
df['2021'] = df['2021'].str.replace(',', '')
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/e5c67fe6-d49b-44dc-a0dc-100eed34a2f2)

## Change Datatypes
```bash
df = df.astype({"2019":"int", "2020":"int", "2021":"int"})
df.dtypes
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/7f3026fb-d9f2-4647-a397-1ce5724fdfc9)

Now all the numbers are integers. 

## Sort by Country (Ascending)
```bash
df.sort_values(by=['Country'])
df.head()
```
![image](https://github.com/soaibur/Data-Cleaning-Project-Pandas/assets/140079359/e37c651a-7ca8-45c5-ac2a-c62bdcaf582d)

## Download the cleaned CSV file
```bash
df.to_csv('cleandata.csv', index=False)
```
index=False has excluded the index column from the CSV file.




***THIS IS THE END***
