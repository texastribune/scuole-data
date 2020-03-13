# scuole-data

A repository of data sets used in the [scuole](https://github.com/texastribune/scuole) project.

## Common Core of Data (ccd)

Summary data for districts and campus via the [National Center of Education Statistics](https://nces.ed.gov/) [Common Core of Data](https://nces.ed.gov/ccd/ccddata.asp) program.

## AskTED Data

AskTED provides superintendents, principals and directory information for all schools and districts. The `scuole` repo downloads data from AskTED directly and updates them in our database, so there's no need to manually download and update them in `scuole-data`. 

Instructions on what commands to run to update AskTED are in the [`scuole` repo README](https://github.com/texastribune/scuole).

We download data from the [directory page](http://mansfield.tea.state.tx.us/TEA.AskTED.Web/Forms/DownloadFile.aspx) and the [personnel page](http://mansfield.tea.state.tx.us/TEA.AskTED.Web/Forms/DownloadFile2.aspx).

## TAPR Data

A walkthrough on how to update the TAPR data is available [on this Confluence page](https://texastribune.atlassian.net/wiki/spaces/APPS/pages/163844/How+to+update+Public+Schools+2019).

All stats collected by the [Texas Education Agency](http://tea.texas.gov/). This data is released annually in late November/early December.

To download TAPR data, go to the [Texas Academic Performance Report homepage](https://rptsvr1.tea.texas.gov/perfreport/tapr/) and find the most recent release. Click the `Data Download` link and go to the `Advanced TAPR Data (Numerators, Denominators & Rates)` option.

This app requires sheets for `Postsecondary Readiness & Non-STAAR Performance Indicators`, `Longitudinal Rate (4-Year, 5-Year, & 6-Year)`, and `Staff & Student Information` for Campus, District, Region and State. The `Reference Information, Accountability Rating and Special Education Determination Status` sheet is only required for Campus, District and Region.

The data is referenced and mapped in the schema using the [Master reference of TAPR elements](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/taprref.html). The reference tables can be found here:

- [Campus student information](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cstud.html)
- [Campus staff information](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cstaff.html)
- [Campus college admissions, college-ready graduates](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/ccad.html)
- [Campus APIB, RHSP, annual dropout, attendance, advanced courses, higher education](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cothr.html)

Note: These are tables for 2012-2013. We use the campus tables to collect the codes used in the TAPR tables and later remove the prefixes for state (S), region (R), district (D), campus (C) and the suffixes -- if there are any -- indicating year Usually year suffixes are included for fields like college ready graduates where there are two graduation times within the school year, but not for demographic data which is representative of the entire year. Prefixes and suffixes are both handled in the data loaders. Codes do not change from year to year.

### TAPR Campus and district names

Each year, there's a possibility that campuses and districts change names, are added, or are removed. We rely on the `entities.csv` file in each year's TAPR folder to create models for districts and campuses.

Instructions on how to create a new `entities.csv` are in the `new_entities` Jupyter Notebook — we should be doing this every year.

We do some district and campus name re-formatting in the Jupyter Notebook (i.e. Cayuga H S --> Cayuga High School). Abbreviations, the Regex for those abbreviations, and the string to replace them with are in `campus_name_abbrev_guidelines.xlsx`.

For campuses and districts that have changed their names have been removed or are new in the current year, email Lauren Callahan at `lauren dot callahan at tea dot texas dot gov` and ask for a CSV or Excel spreadsheet of:

- Campuses and districts that don't exist anymore
- New campuses and districts
- Campuses and districts that have changed their name

## District boundaries

TEA also provides shape files for each district that can be found on the [TEA Data Download](http://tea.texas.gov/Texas_Schools/General_Information/School_District_Locator/School_District_Locator/) page. We don't display the actual shapes on the page because they're not accurate enough and may be misleading. They are useful for determining nearby districts and geolocating.

The latest shapefile `campuses_03-10-2020.shp` (fetched March 10, 2020) is for the 2018-2019 school year. It was downloaded as an `.sd` file, and unzipped with Unarchiver (a Mac program). The unzipped version yields a `schools.gdb` folder, which can be opened in QGIS ([instructions](https://gis.stackexchange.com/questions/26285/installing-file-geodatabase-gdb-support-in-qgis)). 

We convert the TEA provided shapefile into a geojson file using [`ogr2ogr`](http://www.gdal.org/ogr2ogr.html) with the command:

For school districts:

```sh
$ ogr2ogr -f GeoJSON -t_srs crs:84 districts.geojson [tea-provided-file-name].shp
```

For campuses:

```sh
$ ogr2ogr -f GeoJSON -t_srs crs:84 campuses.geojson [tea-provided-file-name].shp
```

## Cohorts

The Texas Higher Ed Coordinating Board (THECB) and TEA provide data for the Higher Ed Outcomes section of the app every year in late April/early May. To obtain and clean the data:

1) First, [download the latest year of the data from THECB](http://www.txhighereddata.org/index.cfm?objectId=F2CBE4A0-C90B-11E5-8D610050560100A9). The latest year should be 11 years from the current year.

2) Then, create a folder in the `cohorts/` folder. Name it the year (YYYY) to which the data corresponds. 

3) Open the spreadsheet. Unhide the `Master Raw Data` worksheet in the `.xlsx` file from THECB by going to `Format -> Sheet -> Unhide --> Master Raw Data`. 

4) Copy and paste that data into a new spreadsheet. Change the headers to match the list of fields in the loader, or copy and paste the headers from a previous year's data. Be sure the data matches the header, and save it as `regionState.csv`.

5) Copy and paste the data found in `Region Cty Gender`, `Region Cty Econ` and `Region Cty Ethnicity` into individual csv files. Name them `countyGender.csv`, `countyEcon.csv` and `countyEthnicity.csv`, respectively. Change the headers to match the list of fields in the loader, or copy and paste the headers from a previous year's data.

** Be sure to remove any notes found at the top and bottom of all `.xlsx` files and make sure all counts are integers and all percents are floats (this might require changing their format in excel 😬). **
