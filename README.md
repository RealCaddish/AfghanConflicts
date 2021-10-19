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

