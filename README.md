<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [scuole-data](#scuole-data)
  - [Common Core of Data (ccd)](#common-core-of-data-ccd)
  - [AskTED](#askted)
  - [TAPR](#tapr)
  - [District and campus models](#district-and-campus-models)
  - [District boundaries and campus coordinates](#district-boundaries-and-campus-coordinates)
    - [District boundaries](#district-boundaries)
    - [Campus coordinates](#campus-coordinates)
    - [Converting the shapefile](#converting-the-shapefile)
  - [Cohorts](#cohorts)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# scuole-data

A repository of data sets used in the [scuole](https://github.com/texastribune/scuole) project. When updating the [scuole](https://github.com/texastribune/scuole) database, this is where you download, clean and save the data. The scripts found in [scuole](https://github.com/texastribune/scuole) will then upload each dataset directly from this folder into the database.

Before you start the update process in the [scuole](https://github.com/texastribune/scuole), make sure all of the data files needed for the update are clean and ready to go.

We try to update every year in January after the latest TAPR data is released at the end of the previous year. However, some data, like cohorts data, are updated much earlier in the year so you can update that data independently from the rest of the database. 

## TAPR
**Released: Annually in late November/early December**

All stats are collected by the [Texas Education Agency](http://tea.texas.gov/).

To download TAPR data, go to the [Texas Academic Performance Report homepage](https://rptsvr1.tea.texas.gov/perfreport/tapr/) and find the most recent release. Click the `Data Download` link and go to the `Advanced TAPR Data (Numerators, Denominators & Rates)` option. 

This app requires sheets for `College, Career, and Military Readiness (CCMR), TSIA, College Prep`, `AP/IB, SAT/ACT`, `Attendance, Chronic Absenteeism, Graduation (RHSP/DAP & FHSP), and Dropout Rates`, `Longitudinal Rate (4-Year, 5-Year, & 6-Year)`, `Staff & Student Information` for Campus, District, Region and State. The `Reference Information, Accountability Rating and Special Education Determination Status` sheet is only required for Campus, District and Region.

For districts and campuses we need to download one extra file that contains the full A-F rankings. Go back one page and select 20xx Accountability instead. (The year will change depending on what year you're working on!). Download the `Accountability Summary` for Campus and Districts.

A detailed walkthrough on how to download and format the TAPR data is available [on this Confluence page](https://texastribune.atlassian.net/wiki/spaces/APPS/pages/163844/How+to+update+Public+Schools+2019).

After downloading each file, you will save it in their respective folders (Campus, District, Region, State) in their respective years as the following csvs. An example of the directory for Campus from 2021-22 found [here](tapr/2021-2022/campus/).

| TAPR data file      | File name |
| ----------- | ----------- |
| Accountability Summary      | accountability.csv       |
| Reference Information, Accountability Rating and Special Education Determination Status      | reference.csv       |
| Staff, Student, and Annual Graduates   | staff-and-student-information.csv        |
| Attendance, Chronic Absenteeism, Graduation (RHSP/DAP & FHSP), and Dropout Rates   | attendance.csv        |
| Longitudinal Rate (4-Year, 5-Year, & 6-Year)   | longitudinal-rate.csv        |
| College, Career, and Military Readiness (CCMR), TSIA, College Prep   | postsecondary-readiness-and-non-staar-performance-indicators.csv        |
| AP/IB, SAT/ACT  | ap-ib-sat-act.csv        |

### Cleaning the TAPR data

The data is referenced and mapped in the schema using the [Master reference of TAPR elements](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/taprref.html). The reference tables can be found here:

- [Campus student information](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cstud.html)
- [Campus staff information](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cstaff.html)
- [Campus college admissions, college-ready graduates](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/ccad.html)
- [Campus APIB, RHSP, annual dropout, attendance, advanced courses, higher education](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cothr.html)

**FYI, dropout rates, graduation rates, AP/IB participation, AP/IB performance and college-ready graduates data are one year behind.**

Note: These are tables for 2012-2013. We use the campus tables to collect the codes used in the TAPR tables and later remove the prefixes for state (S), region (R), district (D), campus (C) and the suffixes -- if there are any -- indicating year. Usually year suffixes are included for fields like college ready graduates where there are two graduation times within the school year, but not for demographic data which is representative of the entire year. Prefixes and suffixes are both handled in the data loaders. Codes do not change from year to year.


## AskTED
**Released: as information is updated**

AskTED provides superintendents, principals and directory information for all schools and districts. The `scuole` repo downloads data from AskTED directly and updates them in our database, so there's no need to manually download and add them to `scuole-data`. 

Instructions on what commands to run to update AskTED are in the [`scuole` README](https://github.com/texastribune/scuole).

We download data from the [directory page](http://mansfield.tea.state.tx.us/TEA.AskTED.Web/Forms/DownloadFile.aspx) and the [personnel page](http://mansfield.tea.state.tx.us/TEA.AskTED.Web/Forms/DownloadFile2.aspx).


## District and campus models

Each year, there's a possibility that campuses and districts change names, are added, or are removed. We rely on the `entities.csv` file in each year's TAPR folder to create models for districts and campuses.

Instructions on how to create a new `entities.csv` are in the `format_new_entities` Jupyter Notebook — we should be doing this every year.

We do some district and campus name re-formatting in the Jupyter Notebook (i.e. Cayuga H S --> Cayuga High School). Abbreviations, the Regex for those abbreviations, and the string to replace them with are in `campus_name_abbrev_guidelines.xlsx`.

## District boundaries and campus coordinates

TEA provides district boundaries and campus coordinates on [their open data site](https://schoolsdata2-tea-texas.opendata.arcgis.com/), which you can get to via the [School District Locator page](https://tea.texas.gov/texas-schools/general-information/school-district-locator). The latest district boundaries are listed under "Current Districts" and campus coordinates under "Current Campuses". Depending on what year you are updating for, you'll go to "Archived Schools Data" and click the school year you are updating for.

Put the GeoJSON boundaries and coordinates in the respective folder: `tapr/reference/district/shapes/` or `tapr/reference/campus/shapes/`.

### District boundaries
This was last updated for the 2020-21 school year. TEA provided the files as GeoJSONs. We don't display the actual shapes on the page because they're not accurate enough and may be misleading. They are useful for determining nearby districts and geolocating.

### Campus coordinates
This was last updated for the 2020-21 school year. TEA provided the files as GeoJSONs.

#### Older update instructions
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
**Released: Annually in late April/early May**

The Texas Higher Ed Coordinating Board (THECB) and TEA provide data for the Higher Ed Outcomes section of the app. To obtain and clean the data:

1) First, [download the latest year of the data from THECB](http://www.txhighereddata.org/index.cfm?objectId=4E600400-D970-11E8-BB650050560100A9). The latest year should be 11 years from the current year.

2) Then, create a folder in the `cohorts/` folder. Name it the year (YYYY) to which the data corresponds. 

3) Open the spreadsheet. Unhide the `Master Raw Data` worksheet in the `.xlsx` file from THECB by going to `Format -> Sheet -> Unhide --> Master Raw Data`. 

4) Copy and paste that data into a new spreadsheet. Change the headers to match the list of fields in the loader, or copy and paste the headers from a previous year's data. Be sure the data matches the header, and save it as `regionState.csv`.

5) Copy and paste the data found in `Region Cty Gender`, `Region Cty Econ` and `Region Cty Ethnicity` into individual csv files. Name them `countyGender.csv`, `countyEcon.csv` and `countyEthnicity.csv`, respectively. Change the headers to match the list of fields in the loader, or copy and paste the headers from a previous year's data.

** Be sure to remove any notes found at the top and bottom of all `.xlsx` files and make sure all counts are integers and all percents are floats (this might require changing their format in excel 😬). **

## Common Core of Data (ccd)

Summary data for districts and campus via the [National Center of Education Statistics](https://nces.ed.gov/) [Common Core of Data](https://nces.ed.gov/ccd/ccddata.asp) program.
