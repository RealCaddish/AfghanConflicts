# Afghanistan Conflicts
## Mapping conflict sites in Afghanistan's various provinces from 2001 - 2019. 

This project aims to map data provided by Uppsala University's Department of Peace and Conflict Research. The goal of this project is to create an interactive map visualizing the pinpoints of various conflict events in Afghanistan between 2001-2019. However, it should be noted that this dataset has conflict data from as early as the Afghan Civil War in 1989. 

The dataset is hosted via the Humanitarian Data Exchange and is entitled, *"Afghanistan - Data on Conflict Events."* Lead authors Ralph Sundberg and Erik Melander published the dataset in the Journal of Peace Research in 2013: 

Sundberg, Ralph, and Erik Melander, 2013, *Introducing the UCDP Georeferenced Event Dataset*, Journal of Peace Research, vol.50, no.4, 523-532.

Högbladh Stina, 2019, *UCDP GED Codebook version 19.1*, Department of Peace and Conflict Research, Uppsala University.

A link to the dataset can be found here:

https://data.humdata.org/dataset/ucdp-data-for-afghanistan
 

 ## Table of Contents
 

 1. Goals
 2. Workflow
 3. Citations


# Goals 

The online map geolocates the lat/lon values provided in the dataset within various provinces. The data should be cleaned and trimmed so that we can view conflict sites only constrained to the United States' Afghanistan War from the years 2001 - 2019. Additionally, any unnecessary or irrelevant fields will be dropped. 

The online map contains a tooltip to help users understand various aspects of the conflict data at a given location. 

The basemap will be a neutral dark tone with provinces outlined on top of this layer. Lastly, urban area polygons and conflict point locations will be added on top of the basemap in order respectively. 

The tooltip will supply the following data:

- Conflict Time and Date
- Link to the News Article
- \# of Civilians Killed
- \# of Government Personnel Killed

Other data may be added in the future. 


# Workflow 

I'm going to primarily use the command line utilizing GDAL/OGR command utilities for data processing and trimming but may incorporate QGIS and MapShaper. 

The resulting dataset will then be ported into an HTML document with CSS and Javascript to create a webpage. jQuery and Leaflet are the primary libaries used for these tasks. 


## 1. Create shapefile using ogr2ogr 

Below are the steps for creating a shapefile with GDAL/OGR out of a csv file. The afghanConflicts.csv will be used to extract lat and long values to create a shapefile. 

First, create a projection file in the terminal with the following command: 

`$ ogr2ogr -f "ESRI Shapefile" afghan_conflicts.dbf afghanConflicts.csv`

This creates a database file we'll need later to build the shapefiles. 

Next, create a .vrt file that will be used to create points from the latitude and longitude fields in the afghanConflicts.csv file. The following syntax in employed to make the .vrt file in an open file in VSCode: 

`<OGRVRTDataSource>
  <OGRVRTLayer name="afghanConflicts">
  <SrcDataSource>afghanConflicts.csv</SrcDataSource>
  <SrcLayer>afghanConflicts</SrcLayer>
  <GeometryType>wkbPoint</GeometryType>
  <LayerSRS>+proj=stere +lat_0=90 +lat_ts=71 +lon_0=-39 +k=1 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs</LayerSRS>
  <GeometryField encoding="PointFromColumns" x="longitude" y="latitude"/>
  </OGRVRTLayer>
</OGRVRTDataSource>`


We now have our database file and our .vrt that can be used to build a Shapefile in OGR. To make the shapefile, we'll create a directory to store the associated shapefiles within the command. The syntax looks like this:

`ogr2ogr -f "ESRI Shapefile" afghanConflicts afghan_xy.vrt`

This will create a directory within the data folder that will store these shapefiles. Then, use QGIS to verify that the Shapefile was made correctly:


![Image of Conflicts on QGIS](images/conflicts_screenshot.JPG)

hmmm.. this still needs work. 