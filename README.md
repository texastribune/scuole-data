<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [scuole-data](#scuole-data)
  - [District boundaries and campus coordinates](#district-boundaries-and-campus-coordinates)
    - [District boundaries](#district-boundaries)
    - [Campus coordinates](#campus-coordinates)
      - [Older update instructions](#older-update-instructions)
  - [District and campus models](#district-and-campus-models)
  - [AskTED](#askted)
  - [TAPR](#tapr)
    - [Cleaning the TAPR data](#cleaning-the-tapr-data)
    - [Making sure TAPR header data matches](#making-sure-tapr-header-data-matches)
  - [Cohorts](#cohorts)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# scuole-data

This repository contains data sets used in the [scuole](https://github.com/texastribune/scuole) project.  
  
The instructions below guide you through the process of retrieving and cleaning the requisite data files before you start the update process in the [scuole](https://github.com/texastribune/scuole).

Data should be updated according to the following timeline:  
* January after the latest TAPR data is released at the end of the previous year  
* May after 8th Grade Cohort data is published  
* At other regular intervals to ensure AskTed directory info is updated (though this should be automated)  

You can update these data independently from each other, provided you follow the instructions in [scuole](https://github.com/texastribune/scuole) in sequence.  

As you proceed through these steps, please update the vintage of each dataset in the [schools data catalog (see the columns with the Schools Explorer header)](https://docs.google.com/spreadsheets/d/1gsUuhcZF7NiPQ225jX4W68pxwZLbcMwCa-OIYPhVOt0/edit?gid=0#gid=0). As of January 2025, most data in this repo was last updated mid-2023.

## District boundaries and campus coordinates

TEA provides district boundaries and campus coordinates on [their open data site](https://schoolsdata2-tea-texas.opendata.arcgis.com/), which you can get to via the [School District Locator page](https://tea.texas.gov/texas-schools/general-information/school-district-locator). The latest district boundaries are listed under "Current Districts" and campus coordinates under "Current Campuses". Depending on what year you are updating for, you'll go to "Archived Schools Data" and click the school year you are updating for.

Put the GeoJSON boundaries and coordinates in the respective folder: `tapr/reference/district/shapes/` or `tapr/reference/campus/shapes/`.

### District boundaries
These files are updated approximately once every year, and should be downloaded as GeoJSONs. We don't display the actual shapes on the page because they're not accurate enough and may be misleading. They are useful for determining nearby districts and geolocating.

### Campus coordinates
These files are updated approximately once every year, and should be downloaded as GeoJSONs. Campus coordinates data can be a bit dated, and likely includes some "zombie schools" which have closed or otherwise have zero enrollment.

## District and campus models

Each year, there's a possibility that campuses and districts change names, are added, or are removed. We rely on the `reference.csv` in each year's TAPR folder to create a `entities.csv` file that will create models for districts and campuses.

Instructions on how to take the `reference.csv` and create a new `entities.csv` are in the [`format_new_entities` Jupyter Notebook](https://github.com/texastribune/scuole-data/blob/carla_readme_changes/format_new_entities.ipynb) — we should be doing this every year.

We do some district and campus name re-formatting in the Jupyter Notebook (i.e. Cayuga H S --> Cayuga High School). Abbreviations, the Regex for those abbreviations, and the string to replace them with are in `campus_name_abbrev_guidelines.xlsx`.

When you update entitites in [the scoule database](https://github.com/texastribune/scuole#updating-entities), you will be erasing the existing district and campus models and then re-adding every district and campus based on that `entities.csv`. This should take care of districts and schools that get renamed/removed/added.

## AskTED
**Released: as information is updated**

[AskTED](https://tealprod.tea.state.tx.us/tea.askted.web/Forms/Home.aspx) provides superintendents, principals and directory information for all schools and districts. The `scuole` repo downloads data from AskTED directly and updates them in our database, so there's no need to manually download and add them to `scuole-data`. 

Instructions on what commands to run to update AskTED are in the [`scuole` README](https://github.com/texastribune/scuole#updating-askted-data).

As of 2023, the AskTED data is found on their [site](https://tealprod.tea.state.tx.us/tea.askted.web/Forms/Home.aspx) if you click on [`Download School and District File with Site Address`](https://tealprod.tea.state.tx.us/Tea.AskTed.Web/Forms/DownloadSite.aspx). This ensures that we have school and district address for the site of the school and district office rather than the mailing addresses (which can be P.O. Boxes).

If the links above don't lead you to the correct data, then AskTED might have changed the link to the data. If so, then the commands to [update askTED data in the scuole database](https://github.com/texastribune/scuole#updating-askted-data) will fail, so make sure the links are updated with the correct headers names.

## TAPR
**Released: Annually in late November/early December**

All stats are collected by the [Texas Education Agency](http://tea.texas.gov/).

To download TAPR data, go to the [Texas Academic Performance Report homepage](https://rptsvr1.tea.texas.gov/perfreport/tapr/) and find the most recent release. Click the `Data Download` link and go to the `Advanced TAPR Data (Numerators, Denominators & Rates)` option. 

This app requires sheets for `College, Career, and Military Readiness (CCMR), TSIA, College Prep`, `AP/IB, SAT/ACT`, `Attendance, Chronic Absenteeism, Graduation (RHSP/DAP & FHSP), and Dropout Rates`, `Longitudinal Rate (4-Year, 5-Year, & 6-Year)`, `Staff & Student Information` for Campus, District, Region and State. The `Reference Information, Accountability Rating and Special Education Determination Status` sheet is only required for Campus, District and Region.

For districts and campuses we need to download one extra file that contains the full A-F rankings. Go back one page and select 20xx Accountability instead. (The year will change depending on what year you're working on!). Download the `Accountability Summary` for Campus and Districts.

A detailed walkthrough on how to download and format the TAPR data is available [on this Confluence page](https://texastribune.atlassian.net/wiki/spaces/APPS/pages/163844/How+to+update+Public+Schools+2019).

After downloading each file, you will save it in their respective folders (Campus, District, Region, State) in their respective years in the `tapr` directory as the following spreadsheets. An example of the directory for Campus from 2021-22 found [here](tapr/2021-2022/campus/).

| TAPR data file | File name    | What it contains |
| ----------- | ----------- | ----------- |
| Accountability Summary      | accountability.csv       | A - F scores       |
| Reference Information, Accountability Rating and Special Education Determination Status      | reference.csv       | District and Campus Identifiers       |
| Staff, Student, and Annual Graduates   | staff-and-student-information.csv        | Student and Teacher data       |
| Attendance, Chronic Absenteeism, Graduation (RHSP/DAP & FHSP), and Dropout Rates   | attendance.csv        | Absenteeism and Dropout rates       |
| Longitudinal Rate (4-Year, 5-Year, & 6-Year)   | longitudinal-rate.csv        | Four year graduation rates       |
| College, Career, and Military Readiness (CCMR), TSIA, College Prep   | postsecondary-readiness-and-non-staar-performance-indicators.csv        | Texas Success Initiative Assessment (TSIA) scores       |
| AP/IB, SAT/ACT  | ap-ib-sat-act.csv        | AP/IB, SAT, ACT scores       |

### Cleaning the TAPR data

The TAPR data usually needs a cleaning before we run it in the [scuole](https://github.com/texastribune/scuole) database. During the last couple of years, this is the cleaning we have needed to do:

- The campus, district and region codes should have a set number of digits, usually padded with leading zeroes. They are:
  - campus (9 digits)
  - district (6 digits)
  - region (2 digits)
  - county (3 digits)
- There's an unnecessary apostrophe added in the "DISTRICT", "COUNTY", "REGION" and "CAMPUS" columns

You have the option [this Jupyter notebook](https://github.com/texastribune/scuole-data/blob/master/Editing%20campus%2C%20district%2C%20region%20and%20county%20codes.ipynb) in this repository that uses the zfill() function to fill in all of the leading zeroes and also removes unnecessary apostrophes in their respective columns. Be sure to run it for the district, campus, region and state datasets. In addition, there is [this Jupyter notebook](https://github.com/texastribune/scuole-data/blob/master/delete_apostrophes.ipynb) that you only have to run once (although not sure if it runs the zfill() function for campuses and districts).

Also, for the 2021-22 TAPR data, the SAT and ACT headers had random letters that were lowercased. Make sure the column headers are capitalized. This step is written in both Python notebooks.

If you're unlucky to run into any other formatting errors, first of all (sorry!), second of all, try to write a solution in a Python notebook and add it to this README so it can be reproduced the following year and properly documented.

### Making sure TAPR header data matches

The headers for the data should match the schema found in [`schema_v2.py`](https://github.com/texastribune/scuole/blob/master/scuole/stats/schemas/tapr/schema_v2.py) which is what we use to map the data. If after uploading all of the data into the [scuole](https://github.com/texastribune/scuole) database, you notice there are fields missing. It could be because the header in the spreadsheet do not match the schema found in [`schema_v2.py`](https://github.com/texastribune/scuole/blob/master/scuole/stats/schemas/tapr/schema_v2.py). There are a lot of headers and columns so it might get tedious to check each and every one, especially since they don't tend to change year-to-year. But it might be worth checking if you see data missing. The [scuole](https://github.com/texastribune/scuole) database just skips those headers if it doesn't see a matching header and shows N/A in your local database.

Every year, TEA publishes a `Master reference of TAPR elements` like [this one from 2022](https://rptsvr1.tea.texas.gov/perfreport/tapr/2022/download/taprref.html). It's usually found in the TAPR Advanced Data Download for that year that you use to download the data and called `Master Reference (HTML)`. You can also download it in an Excel format for campuses, districts, regions and state. 

It's also good to remember that for some datasets, TAPR releases the **latest data** while others are **a year behind**. For example, if we were updating the 2021-22 TAPR data, the latest data would be for 2021-22 (or Class of 2022) and the previous year would be 2020-21 (or Class of 2021) Here's is a handy breakdown:

| Data     | TAPR Data File | Year |
| ----------- | ----------- | ----------- |
| A-F scores     | Reference Information, Accountability Rating and Special Education Determination       | Latest year       |
| Student Demographics   | Staff, Student, and Annual Graduates       | Latest year       |
| At-risk, economically disadvantages and limited English proficiency students   | Staff, Student, and Annual Graduates       | Latest year       |
| Enrollments in special programs (bilingual/ESL, gifted & talented, special ed)   | Staff, Student, and Annual Graduates       | Latest year       |
| Four-year graduation rates   | Longitudinal Rate (4-Year, 5-Year, & 6-Year)       | Previous year       |
| Dropout rates   | Attendance, Graduation (RHSP/DAP & FHSP), and Dropout Rates       | Previous year       |
| Chronic absenteeism   | Attendance, Graduation (RHSP/DAP & FHSP), and Dropout Rates       | Previous year       |
| AP/IB participation   | AP/IB, SAT/ACT       | Previous year       |
| AP/IB performance   | AP/IB, SAT/ACT       | Previous year       |
| SAT score   | AP/IB, SAT/ACT       | Previous year       |
| ACT score   | AP/IB, SAT/ACT       | Previous year       |
| College-ready graduates (TSIA scores)   | College, Career, and Military Readiness (CCMR); and Other Postsecondary Indicators       | Previous year       |
| Teacher demographics   | Staff, Student, and Annual Graduates       | Latest year       |
| Degree held by teachers   | Staff, Student, and Annual Graduates       | Latest year       |
| Students per teacher   | Staff, Student, and Annual Graduates       | Latest year       |
| Teacher salaries   | Staff, Student, and Annual Graduates       | Latest year       |

## Cohorts
**Released: Annually in late April/early May**

The Texas Higher Ed Coordinating Board (THECB) provides data for the Higher Ed Outcomes section of the app. To obtain and clean the data:

1) First, [download the latest year of the data from THECB](http://www.txhighereddata.org/index.cfm?objectId=4E600400-D970-11E8-BB650050560100A9). The latest year should be 11 years from the current year.

2) Then, create a folder in the `cohorts/` folder. Name it the year (YYYY) to which the data corresponds. 

3) Open the spreadsheet- you can do this in Google Sheets if you don't have Excel. Unhide the `Master Raw Data` worksheet in the `.xlsx` file from THECB by going to `Format -> Sheet -> Unhide --> Master Raw Data` (Excel) or `Hamburger Menu -> Unhide` (Google Sheets). 

4) For the last two years, however, the `Master Raw Data` worksheet has not been for the corresponding year. Check to see if it matches the year. If not, contact the Texas Higher Ed Coordinating Board right away so they can get you a spreadsheet of that `Master Raw Data`. **Warning:** They tend to take their sweet time when it comes to communication. **Note 1/31/25** Cohort data as published on THECB does not include Master Raw Data for the 2013 Cohort. I needed to send a PIR request to `publicinformationrequest@highered.texas.gov`.

5) Copy and paste that data into a new spreadsheet. Change the headers to match the list of fields in the loader, or copy and paste the headers from a previous year's data. Be sure the data matches the header, and save it as `regionState.csv`.

6) Copy and paste the data found in `Region Cty Gender`, `Region Cty Econ` and `Region Cty Ethnicity` into individual csv files. Name them `countyGender.csv`, `countyEcon.csv` and `countyEthnicity.csv`, respectively. Change the headers to match the list of fields in the loader, or copy and paste the headers from a previous year's data.

7) Be sure to remove any notes found at the top and bottom of all `.xlsx` files.

8) Make sure all counts are integers and all percents are floats (which requires changing them in Excel 😬).

9) When updating the latest cohorts data, I noticed that they're adding asterisk (*) and N/As into the datasets. We don't want that! We want them to be empty. I created a Jupyter notebook called [`clean_cohorts_data.ipynb`](clean_cohorts_data.ipynb) to help with any cleanup of that.

**Note (from 2023 or prior?):** According to a spokesperson at THECB, they are planning on building a cohorts dashboard in the near future, and they don't know how they will make the spreadsheets available to download (if at all). That's why we should probably be on the lookout for any changes in the [THECB website](http://www.txhighereddata.org/index.cfm?objectId=4E600400-D970-11E8-BB650050560100A9) and plan for any changes in our updating process in the future depending on how the data is released. Contact Mike Eddleman at `Mike.Eddleman@highered.texas.gov` or any other spokesperson at THECB when the time comes for any details.
