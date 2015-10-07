# scuole-data
A repository of data sets used in the [scuole](https://github.com/texastribune/scuole) project.

## Financial Allocation Study for Texas (fast)

Summary data for districts and campus via the [Financial Allocation Study for Texas](http://fastexas.org/) through their [data downloads](http://fastexas.org/results/downloads.php). FAST data used for clean campus and district names.

## Common Core of Data (ccd)

Summary data for districts and campus via the [National Center of Education Statistics](https://nces.ed.gov/) [Common Core of Data](https://nces.ed.gov/ccd/ccddata.asp) program. CCD data is used for addresses and coordinates.

## Texas Academic Performance Reports (tapr)

Our primary source of statistics for state, regions, districts and campuses is from the [Texas Education Agency's](http://tea.texas.gov/) [Texas Academic Performace Reports](http://ritter.tea.state.tx.us/perfreport/tapr/). TEA releases this data every fall (2014-2015 data is projected to be released November 19th, 2015). The data is downloaded from the [Advanced TAPR Data (Numerators, Denominators & Rates)](http://ritter.tea.state.tx.us/perfreport/tapr/2014/download/DownloadData.html) data download page. For state, region, district and region, we load the `Postsecondary Readiness & Non-STAAR Performance Indicators`, `Staff & Student Information ` and `Reference Information for Campus, District or Region ` sheets listed under the `Other Indicators` list on the data download page.

The data is referenced and mapped in the schema using the [Master reference of TAPR elements](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/taprref.html). The reference tables can be found here:

- [Campus student information](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cstud.html)
- [Campus staff information](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cstaff.html)
- [Campus college admissions, college-ready graduates](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/ccad.html)
- [Campus APIB, RHSP, annual dropout, attendance, advanced courses, higher education](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cothr.html)

Note: These are tables for 2012-2013. We use the campus tables to collect the codes used in the TAPR tables and later remove the prefixes for state (S), region (R), district (D), campus (C) and the suffixes -- if there are any -- indicating year Usually year suffixes are included for fields like college ready graduates where there are two graduation times within the school year, but not for demographic data which is representative of the entire year. Prefixes and suffixes are both handled in the data loaders.

### Reference

`reference` is a folder within the `tapr` folder where we store the data that isn't attached to a year (name, rating, etc.)- though it should be updated yearly.

- [Campus reference](http://ritter.tea.state.tx.us/perfreport/tapr/2013/download/cref.html)

### District boundaries

TEA also provides shape files for each district that can be found on the [TEA Data Download](http://tea.texas.gov/Texas_Schools/General_Information/School_District_Locator/Data_Download/) page. We don't display the actual shapes on the page because they're not accurate enough and may be misleading. They are useful for determining nearby districts and geolocating.

We convert the TEA provided shapefile into a geojson file using [`ogr2ogr`](http://www.gdal.org/ogr2ogr.html) with the command:

```sh
$ ogr2ogr -f GeoJSON -t_srs crs:84 SchoolDistricts.geojson [tea-provided-file-name].shp
```
