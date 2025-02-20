This README contains obsolete technical instructions removed from [README.md](README.md).

# Older update instructions
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

### cleaning the TAPR data
removed 2/20/25

You have the option this Jupyter notebook in this repository that uses the zfill() function to fill in all of the leading zeroes and also removes unnecessary apostrophes in their respective columns. Be sure to run it for the district, campus, region and state datasets. In addition, there is this Jupyter notebook that you only have to run once (although not sure if it runs the zfill() function for campuses and districts).