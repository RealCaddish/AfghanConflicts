# Afghanistan Conflicts
## Mapping conflict sites in Afghanistan's various provinces from 2001 - 2019. 

This project aims to map data provided by <a href='https://ucdp.uu.se/'>Uppsala University's Department of Peace and Conflict Research. </a> The goal of this project is to create an interactive map visualizing the pinpoints of various conflict events in Afghanistan between 2001-2019. However, it should be noted that this dataset has conflict data from as early as the Afghan Civil War in 1989. 

The dataset is hosted via the Humanitarian Data Exchange and is entitled, *"Afghanistan - Data on Conflict Events."* Lead authors Ralph Sundberg and Erik Melander published the dataset in the Journal of Peace Research in 2013: 

Sundberg, Ralph, and Erik Melander, 2013, *Introducing the UCDP Georeferenced Event Dataset*, Journal of Peace Research, vol.50, no.4, 523-532.

Högbladh Stena, 2019, *UCDP GED Codebook version 19.1*, Department of Peace and Conflict Research, Uppsala University.

A link to the dataset can be found here:

https://data.humdata.org/dataset/ucdp-data-for-afghanistan
 

 ## Table of Contents
 

 1. Goals
 2. Workflow
 3. Webmapping
 4. Citations


# 1. Goals 

The online map geolocates the lat/lon values provided in the dataset within various provinces. The data should be cleaned and trimmed so that we can view conflict sites only constrained to the United States' Afghanistan War from the years 2001 - 2019. Additionally, any unnecessary or irrelevant fields will be dropped. 

The online map contains a tooltip to help users understand various aspects of the conflict data at a given location. 

The basemap will be a neutral dark tone with provinces outlined on top of this layer. Lastly, urban area polygons and conflict point locations will be added on top of the basemap in order respectively. 

The tooltip will supply the following data:

- Conflict Time and Date
- Link to the News Article
- \# of Civilians Killed
- \# of Government Personnel Killed

Other data may be added in the future. 


# 2. Workflow 

I'm going to primarily use the command line utilizing GDAL/OGR command utilities for data processing and trimming but may incorporate QGIS and MapShaper. 

The resulting dataset will then be ported into an HTML document with CSS and JavaScript to create a webpage. jQuery and Leaflet are the primary libraries used for these tasks. 


## 2.1 Create the Conflict Points Shapefile with ogr2ogr 

Below are the steps for creating a shapefile with GDAL/OGR out of a csv file. The afghanConflicts.csv will be used to extract lat and long values to create a shapefile. 

First, create a projection file in the terminal with the following command: 

```
$ ogr2ogr -f "ESRI Shapefile" afghan_conflicts.dbf afghanConflicts.csv
```

This creates a database file we'll need later to build the shapefiles. 

Next, create a .vrt file that will be used to create points from the latitude and longitude fields in the afghanConflicts.csv file. The following syntax in employed to make the .vrt file in an open file in VSCode: 

```
<OGRVRTDataSource>
    <OGRVRTLayer name="afghanConflicts">
    <SrcDataSource>afghanConflicts.csv</SrcDataSource>
    <SrcLayer>afghanConflicts</SrcLayer>
    <GeometryType>wkbPoint</GeometryType>
    <LayerSRS>+proj=stere +lat_0=90 +lat_ts=71 +lon_0=-39 +k=1 +x_0=0 
    +y_0=0 +datum=WGS84 +units=m +no_defs</LayerSRS>
    <GeometryField encoding="PointFromColumns" x="longitude" 
    y="latitude"/>
    </OGRVRTLayer>
</OGRVRTDataSource>
```


We now have our database file and .vrt that can be used to build a Shapefile in OGR. To make the shapefile, we'll create a directory to store the associated shapefiles within the command. The syntax looks like this:

```
$ogr2ogr -f "ESRI Shapefile" afghanConflicts afghan_xy.vrt
```

This will create a directory within the data folder that will store these shapefiles. Then, use QGIS to verify that the Shapefile was made correctly:


![Image of Conflicts on QGIS](images/conflict_points.JPG)


# 2.2 Afghan Sovereignty Shapefile with ogr2ogr

First, let's use OGR to verify the CRS used for the administrative boundaries from Natural Earth:

```
$ ogrinfo -so ne_10m_admin_0_sovereignty.shp ne_10m_admin_0_sovereignty 
```

This outputs the following information:

![Image of sovereignty information](images/sovereignty_info.JPG)

We can see that the administrative boundaries are already in WGS 84 (EPSG: 4326) so we won't need to reproject it to a different CRS. 

Next, we can select out only Afghanistan as the only administrative unit we'll want for this project. 

```
$ ogr2ogr -f "ESRI Shapefile" -where "SOVEREIGNT='Afghanistan'" afghanistan.shp ne_10m_admin_0_sovereignty/ne_10m_admin_0_sovereignty.shp
```

Now verify in QGIS: 

![Image Verifying Afghanistan Shapefile](images/afghanistan.JPG)

# 2.3 Afghanistan Provinces 

We'll do a similar process to the provinces shapefile from Natural Earth: 

```
$ ogr2ogr -f "ESRI Shapefile" -where "admin='Afghanistan'" afghanistan.shp ne_10m_admin_0_sovereignty/ne_10m_admin_0_sovereignty.shp
```
![Image Verifying Afghan Provinces](images/afghan_provinces.JPG)

Progress! 

To recap, we now have our administrative boundaries (both Afghanistan and its subsequent provinces) properly selected out and saved using OGR. Lastly, let's add urban areas of Afghanistan to this. 

# 2.4 Afghanistan Urban Areas

For the urban areas in Afghanistan, I've decided to extract them via QGIS because I haven't found a good solution yet using ogr2ogr. Below is a screenshot of the selected urban areas of Afghanistan from Natural Earth:

![Image Verifying Afghan Urban Areas](images/afghan_urban_areas.JPG)

Super! Now we have our urban areas, provinces, and state boundaries. Next, we'll do some quick cleaning of the conflict data and then convert our shapefiles to JSON for webmapping. 

# 2.5 Python for Conflict Data Analysis 

See .ipynb for analysis of the conflict data. 

# 2.6 Converting Shapefiles to JSON for the Webmapping with OGR

Let's convert shapefiles to JSON now with OGR. I'll begin in the order taken from the previous steps starting with the recently cleaned conflict data, 'clean_conflicts.shp':

```
$ ogr2ogr -f "GeoJSON" clean_conflicts.json clean_conflicts.shp
```

Let's do the same for the rest of our datasets. I created a subdirectory for storing our json files named 'json' naturally. 

Afghanistan provinces:

```
$ ogr2ogr -f "GeoJSON" ../json/afghan_provinces.json afghan_provinces.shp
```
Afghanistan polygon:

```
$ ogr2ogr -f "GeoJSON" ../json/afghanistan.json afghanistan.shp
```

Afghanistan urban areas:

```
$ ogr2ogr -f "GeoJSON" ../json/afghan_urban_areas.json afghan_urban_areas.shp
```

# 3. Webmapping 

See index.html file for webmapping 


# 4. Citations 

Högbladh Stina, 2019, “UCDP GED Codebook version 19.1”, Department of Peace and Conflict Research, Uppsala University

Sundberg, Ralph, and Erik Melander, 2013, “Introducing the UCDP Georeferenced Event Dataset”, Journal of Peace Research, vol.50, no.4, 523-532

<a href='https://data.humdata.org/dataset/ucdp-data-for-afghanistan'>Link to Data on Conflicts in Afghanistan </a>


