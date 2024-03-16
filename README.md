# GeoData_WS2023_2_Final_Group_H
# Task 1.1: Selecting Weather Stations in Bayern (Bavaria)

This task involves selecting weather stations that meet specific criteria: they must be located in Bayern, still active as of 2023, and started before 1950. We will use Pandas to read the station description file from the historical KL data collection and make necessary modifications to achieve this.

Steps:
Data Source:

I use the station description file provided by DWD (Deutscher Wetterdienst). The file can be accessed here. https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/annual/kl/historical/KL_Jahreswerte_Beschreibung_Stationen.txt
Modifications I made to the Notebook:

I based my solution on the Jupyter notebook gdms0180_DWD_NRW_Annual_Temp_vs_Altitude/gnb0181_DWD_NRW_Annual_Temp_vs_Altitude_V001.ipynb.
The directory was changed from recent to historical to fetch historical data.
The following lines of code were added to select stations meeting the specified criteria:

inBAY = (df_stations['state'] == "Bayern").values

isActive2023 = (df_stations['date_to'] > "2023").values 

isActive1950 = (df_stations['date_from'] < "1950").values 

station_ids_selected = df_stations[inBAY & isActive2023 & isActive1950].index

print(f"Stations located in Bayern and still active in 2023 and started before 1950: \n{list(station_ids_selected)}")

This code selects stations in Bayern that were active in 2023 and started before 1950.

I created a geometrical point for each station using latitude and longitude values.
Code used:

geometry = gpd.points_from_xy(df_stations.longitude, df_stations.latitude)
df_stations['geometry'] = geometry

Additionally, I changed CRS (Coordinate Reference System) values to EPSG:25832 (CRS for Germany) using the to_crs function.
Results:
38 stations were detected that met the specified criteria
 
# Sub-Task 1.2: Creating a Geopackage Layer and Annotated Map in QGIS
In this task, we utilize geopandas in a Jupyter notebook to create a geopackage layer containing stations that match the criteria specified in Sub-Task 1.1. We then load this layer into QGIS, overlay it with the Bayern WMS service using the topographic map collection (DTK500) as a background map, and create a nicely designed and annotated map.

**Steps:**

*Creating Geopackage Layer:*
We used geopandas to create a geopackage layer from the dataframe containing stations meeting the criteria from Sub-Task 1.1.
The function to_file was used to save the dataframe as a geopackage file named output.gpkg.

*Loading Data into QGIS:*
In QGIS, we added the geopackage layer containing the selected stations.
We also added the Bayern WMS service, specifically the topographic map collection (DTK500), as a background map.

*Creating Annotated Map:*
Using the 'Print Layout' option in QGIS, we designed a map with the selected stations overlaid on the background map.
Station IDs and names were added as labels to the map for easy identification.
    
*Results:*
The final map provides a visual representation of the selected stations in Bayern, overlayed on the topographic map background.
Each station is labeled with its ID and name for easy reference.(see below)
    
*Additional Notes:*
    We used EPSG:25832 as the coordinate reference system for consistency.
Ensure that the station names and IDs are clearly visible and properly positioned on the map for easy interpretation.

# Sub-Task 1.3: Automatically Downloading Annual Temperature Data

In this task, I extended our Jupyter notebook to automatically download the annual temperature data from the KL data collection for the selected station IDs. Using the requests library to fetch the data from the provided URL.

**Steps:**

Defining URL and Local Directory:

I specified the URL of the KL data collection for annual temperature data, which is: https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/annual/kl/historical/.
Additionally, we defined a local directory where the temperature data files will be stored.
                                   
Downloading Data for Selected Stations:

I iterated over the selected station IDs obtained from Sub-Task 1.1.
For each station, we constructed the filename for the temperature data and generated the full URL for downloading.
Using requests.get(), we fetched the temperature data file from the URL.
The downloaded file was saved to the local directory using open() in binary write mode.
A print statement confirms successful download for each station.
                                                                                                 
**Results:**
                                                                                                 
Annual temperature data files were automatically downloaded for each selected station ID.
Any errors encountered during the download process were logged and printed for reference.
                                                                          
**Additional Notes:**
                                                                          
Ensure that the local directory specified for storing the temperature data files exists and has appropriate permissions.

# Sub-Task 1.4: Creating Warming Stripes Plots

In this task, I used the dataframe containing the temperature time series merged columnwise together with Seaborn to plot warming stripes. Additionally, I created a similar plot for the selected stations, including the annual temperature data of 2022, with the time series running from 1950 to 2022.

**Steps:** 

Copying Relevant Code:

We copied the relevant code from the notebook gdms0155_DWD_NRW_5_Warming_Stripes/gdms155_DWD_NRW_5_Warming_Stripes.ipynb.
This code provides functionality to create warming stripes plots using Seaborn.
    
DataFrame Index Adjustment:

I ensured that the index of the dataframe captured only the year component of the timestamps. This is crucial for plotting warming stripes over time.
    
Generating Warming Stripes:

I utilized Seaborn to generate warming stripes plots for the temperature time series.
One plot was created for the merged temperature time series columnwise.
Additionally, I created another plot for the selected stations, including the annual temperature data of 2022. The time series ranged from 1950 to 2022.
                                     
**Results:**
                                     
Three different warming stripes plots were generated:
Warming stripes plot for the merged temperature time series.
Warming stripes plot for the selected stations, including the annual temperature data of 2022.
Another warming stripes plot for the selected stations, excluding the annual temperature data of 2022.

# Task 2.1: Georeferencing the Burial Mounds Map

In this task, I georeferenced the provided photo of the burial mounds in Uedemer Hochwald map using QGIS and the Georefencer tool. Georeferencing involved assigning spatial coordinates to the image so that it aligns accurately with real-world geographic data.

## Steps Taken:

1. **Preparation**:
   - Opened the QGIS project file and imported the provided photo of the burial mounds map.
   - Accessed the Georefencer tool, located under the Raster menu in QGIS.

2. **Transformation Settings**:
   - Selected 'Thin Plate Spline' as the transformation type. This method is suitable for accurately aligning images with complex distortions.
   - Chose 'Nearest neighbour' as the resampling method. This method preserves the pixel values of the original image, ensuring clarity and precision.
   - Set the target CRS (Coordinate Reference System) to EPSG:25832, which corresponds to the Universal Transverse Mercator (UTM) projection for the study area.

3. **Adding Ground Control Points (GCPs)**:
   - Identified distinct features visible on both the photo of the burial mounds map and reference layers within the QGIS project.
   - Added a total of 12 ground control points along paths, path or road intersections, and other identifiable landmarks.
   - Ensured that GCPs were evenly distributed across the image to facilitate accurate referencing.

4. **Georeferencing Process**:
   - Placed GCPs on the photo of the burial mounds map by clicking on corresponding locations in the reference layers within QGIS.
   - Used identifiable landmarks such as paths, path junctions, and other features to ensure accurate alignment.
   - Applied the chosen transformation settings to georeference the image based on the specified GCPs and target CRS.

5. **Evaluation of Results**:
   - After georeferencing, visually inspected the aligned photo of the burial mounds map to ensure its accuracy and alignment with real-world geographic features.
   - Checked the residuals to assess the accuracy of the georeferencing process. Residuals represent the differences between the original GCP locations and their corresponding locations in the georeferenced dataset.

6. **Addition to QGIS Project**:
   - Added the georeferenced photo of the burial mounds map to the QGIS project for further analysis and comparison with other spatial data.

## Observation:

The georeferencing process successfully aligned the provided photo of the burial mounds map with real-world geographic data, enabling accurate spatial analysis and visualization. (see below).

# Task 2.2: Hillshade Model and Georeferenced Map Comparison

In this task, I created a hillshade model from the Digital Terrain Model (DTM) layer and overlaid the georeferenced map on top of it for comparison.

## Steps Taken:

1. **Creation of Hillshade Model**:
   - Accessed the DTM layer within the QGIS project.
   - Used the Processing Toolbox to create a hillshade model from the DTM layer.
   - Generated the hillshade model to highlight the terrain features and elevation variations in the landscape.

2. **Overlay Georeferenced Map**:
   - Imported the georeferenced map, which was previously aligned with real-world geographic data using identifiable ground control points (GCPs).
   - Placed the georeferenced map layer on top of the hillshade model layer in the QGIS canvas for comparison.

3. **Visual Comparison**:
   - Visually inspected the overlaid layers to compare the representation of terrain features in the hillshade model and the georeferenced map.
   - Observed any discrepancies or similarities in the depiction of landscape characteristics such as elevation, slopes, and landforms.

4. **Analysis and Observations**:
   - Analyzed the results of the comparison to assess the accuracy and alignment of the georeferenced map with the terrain features represented in the hillshade model.
   - Made observations about the visibility and clarity of the burial mounds and other landscape features in both datasets.


## Observations:

Looking at the burial mounds they are clearly visible on the hillshade model, and can be identified by the “bumps” on the map. On the geo referenced map they are represented by a circular icon with spokes perturbing outwards. 
The burial mounds are visible in both maps, and are generally located in the same area, meaning the maps are aligned. Closely looking along the roads and junctions, the map seems to align well with the terrain shown on the hillshade model. 
The geo referenced map, in some areas, doesn’t line -up with the hillshade model, as the hillshade depicts the presence of burial mounds in places the georeferenced map doesn’t have the icons, in these areas the representation is less reliable. 

# Task 2.3: Measuring Typical Mound Heights Relative to Surrounding Environment

## Description:
In Task 2.3, I utilized a contour layer extracted from the Digital Terrain Model (DTM) map to facilitate the detection of burial mounds. By identifying areas where the elevation differs from the surrounding environment, it became easier to pinpoint the location of burial mounds, which typically exhibit a circular shape on the map due to their elevated position.

## Steps Taken:

1. **Extraction of Contour Layer:**
   - I extracted a contour layer from the DTM map to highlight areas with varying elevations, making it easier to detect burial mounds.

2. **Using the Identify Features Tool:**
   - I employed the 'Identify Features' tool in QGIS to check the elevation of the area. The elevation range for the area of interest, i.e. the area where the burial mounds are located, was 30m to 40m above sea level.

3. **Observations:**
   - Upon analysis, I observed that the elevation difference between the surroundings and the top of the burial mounds ranged from 0.5m to 2m. This relatively small height difference is characteristic of burial mounds, which are often constructed to be more protruding than their immediate surroundings.

# Task 2.4: Identifying and Digitizing Rectangular Structures

In this task, I identified and digitized rectangular structures northeast of the burial mounds using QGIS. The goal was to capture and analyze these structures, which may represent archaeological or other features of interest within the study area.

## Steps Taken:

1. **Observation of Rectangular Structures**:
   - Examined the hillshade model in the direction East-North-East of the burial mounds area.
   - Identified visible rectangular structures that were distinct from natural terrain features.

2. **Digitization Process**:
   - Used the QGIS Digitizing tools to draw polygons around the identified rectangular structures.
   - Activated the Toggle Editing mode for the layer containing the hillshade model.
   - Selected the Add Polygon Feature tool and clicked to create vertices outlining the shape of each structure.
   - Closed the polygons to complete the digitization process.

3. **Styling and Visualization**:
   - Set the style of the digitized polygons to distinguish them from surrounding features.
   - Chose a red color to represent the polygons and adjusted transparency as needed for clarity.
   - Visualized the digitized polygons overlaid on the hillshade model to assess their spatial distribution and relationship to other landscape features.

4. **Analysis and Interpretation**:
   - Analyzed the digitized rectangular structures to infer their potential origin and significance.
   - Considered factors such as historical context, archaeological evidence, and landscape patterns to interpret the nature of the identified features.

## Observation:

Using the hillshade model, I was able to detect the rectangular structures, they seem to be evenly shaped, so they are probably man-made. Seeing that it is on the site of a [Roman Archaeological Park](https://apx.lvr.de/en/lvr_archaeologischer_park/archaeologischer_park.html).

, its origin may be from the Roman Empire. 
The Archaeological park features full-size reconstructions of Roman houses, temples & an amphitheater.

# Task 3:OpenHygrisC Nitrate Data:
OpenHypE System Setup and Time Series Video Generation

This project aims to set up the OpenHypE system and generate a time series video depicting changes in nitrate concentration over time in North Rhine-Westphalia (NRW), Germany. The OpenHypE system utilizes data from OpenHygrisC stored in a PostgreSQL/PostGIS database, with QGIS for visualization.

**Steps taken:** <br>
*1. Data Download and Environment Setup*

The data was downloaded from OpenGeodata.NRW.
Then an Anaconda environment named OpenHype was created
I later Installed the required packages (pandas, sqlalchemy, psycopg2, geopandas, jupyter notebook). The packages were then loaded into the OpenHype environment using a YAML file for streamlined installation.

*2. Data Cleaning and Importing*

The CSV files were cleaned and the data was imported into PostgreSQL database using separate Jupyter Notebooks.

*3. Database Analysis*

I utilized SQL commands to examine and analyze data within the PostgreSQL database.
View tables, counted rows, and filtered data based on Nitrate.
I created views for efficient data retrieval and analysis.
Grouped data from multiple tables based on Nitrate and enhanced messstelle table with a geometry column.

*4. Data Visualization in QGIS*

Downloaded shapefiles for NRW (entire state, kreis, Gemeinde).
Loaded the shapefiles into QGIS for visualization and analysis.
Created shapefiles for stations and nitrate concentration using Python code.
Loaded shapefiles into QGIS.
                         
*5. Time Series Video Creation*
                         
Utilized QGIS Temporal Controller to export images for each time frame.
Then I compiled images into a time series video using an online video editing platform ([Clideo](https://clideo.com/video-maker)).

# Task 4: Hurricane Tracking in the Caribbean using QGIS
## Kwame Agard (24171)

## Introduction <br>
In this project, I aimed to create a QGIS time series animation depicting the paths of hurricanes during the 2016 Atlantic hurricane season. Growing up in the Caribbean, I have firsthand experience with hurricanes, which inspired me to delve into their tracking and patterns. This project not only serves as a personal exercise but also contributes to understanding the behavior of hurricanes for emergency management and scientific research purposes.


**What is a Hurricane?**
    
A hurricane is a type of storm known as a tropical cyclone, forming over tropical or subtropical waters in the North Atlantic and Northeast Pacific oceans. These storms rotate around areas of low pressure, generating strong winds and heavy rain.

**How are Hurricanes Formed?**
    
Several factors are required for a hurricane to develop:

- **Warm sea surface temperatures:** Sea surface temperatures of at least 26.5°C are necessary down to a depth of at least 50m, causing the overlying atmosphere to be unstable enough to sustain convection and thunderstorms.
- **Rapid cooling with height:** allows the release of the heat of condensation that powers the hurricane.
- **High humidity:** High humidity, especially in the lower-to-mid troposphere, provides the moisture that feeds the storm.
- **Low wind shear:** Low amounts of wind shear are essential, as high shear disrupts the storm's circulation.
- **Location away from the equator:** Hurricanes form more than 5 degrees latitude away from the equator, allowing the Coriolis effect to deflect winds and create circulation.


**Atlantic Hurricane Season**
    
The Atlantic hurricane season officially begins on June 1st and ends on November 30th, with a peak from late August through September. On average, there are 8 to 10 hurricanes in the Atlantic Basin per year.

**Classification of Hurricanes**
    
Hurricanes are classified based on the Saffir-Simpson Hurricane Wind Scale, which rates them from Category 1 (Minimal) to Category 5 (Catastrophic), depending on sustained wind speed:

- **Category 1 (Minimal Hurricane):**
  - Winds: 74-95 mph (64-83 kts, 119-153 km/h).
  - Well-constructed frame homes could have damage to roof, shingles, vinyl siding, and gutters.
  - Large branches of trees will snap, and shallowly rooted trees may be toppled.
  - Extensive damage to power lines and poles likely will result in power outages that could last a few to several days.

- **Category 2 (Moderate Hurricane):**
  - Winds: 96-110 mph (84-96 kts, 154-177 km/h).
  - Well-constructed frame homes could sustain major roof and siding damage.
  - Many shallowly rooted trees will be snapped or uprooted and block numerous roads.
  - Near-total power loss is expected with outages that could last from several days to weeks.

- **Category 3 (Extensive Hurricane):**
  - Winds: 111-130 mph (97-113 kts, 178-209 km/h).
  - Well-built framed homes may incur major damage or removal of roof decking and gable ends.
  - Many trees will be snapped or uprooted, blocking numerous roads.
  - Electricity and water will be unavailable for several days to weeks after the storm passes.

- **Category 4 (Extreme Hurricane):**
  - Winds: 131-155 mph (114-135 kts, 210-249 km/h).
  - Well-built framed homes can sustain severe damage with the loss of most of the roof structure and/or some exterior walls.
  - Most trees will be snapped or uprooted, and power poles downed.
  - Fallen trees and power poles will isolate residential areas.
  - Power outages will last weeks to possibly months.
  - Most of the area will be uninhabitable for weeks or months.

- **Category 5 (Catastrophic Hurricane):**
  - Winds: greater than 155 mph (135 kts, 249 km/h).
  - A high percentage of framed homes will be destroyed, with total roof failure and wall collapse.
  - Fallen trees and power poles will isolate residential areas.
  - Power outages will last for weeks to possibly months.
  - Most of the area will be uninhabitable for weeks or months.


**Importance of Analyzing Historical Hurricane Paths**
    
Studying and tracking historical hurricane paths are crucial for scientists, climatologists, emergency management officials, and residents in hurricane-prone areas. It aids in understanding climate change patterns, predicting future events, and implementing effective disaster preparedness measures.

**Information was sourced from [US National Ocean Service](https://oceanservice.noaa.gov/facts/hurricane.html) and [National Geographic](https://education.nationalgeographic.org/resource/hurricane/).**

# Methodology: Spatio-Temporal Hurricane Tracking <br>
## Introduction
    
This project involves tracking hurricanes during the 2016 Atlantic Hurricane Season using spatio-temporal analysis techniques. The goal is to visualize the paths and intensities of hurricanes over time using QGIS and shapefile data obtained from [the National Hurricane Centre website](https://www.nhc.noaa.gov/data/tcr/index.php?season=2016&basin=atl).

## Steps Taken <br>
**1. Data Acquisition** 
    
Obtained shapefile data of all storms and hurricanes for the 2016 Atlantic Hurricane Season from the National Hurricane Centre website.
    
**2. Data Visualization in QGIS**
    
Loaded a basemap using QuickMapServices and Google Satellite imagery in QGIS. Utilized Python console in QGIS to load shapefiles onto the base map. Using the following code: 
        
**3. Data Processing**

Merged all storms for the 2016 Atlantic Hurricane Season using the Data Management Tools in QGIS.
Adjusted symbology settings to represent storm intensity (stronger storms depicted with darker red symbols, size of symbols increases with intensity).
Generated labels with storm names for better visualization.

**4. Temporal Analysis and Video Creation**

Opened the attribute table for the merged layer to change the date format for compatibility with the temporal controller.
Created a custom function in the Field Calculator to format dates appropriately (see below).
Configured settings for the temporal controller in QGIS, including event duration and field selection.
Exported the animation and used [Clideo](https://clideo.com/video-maker) services to create a video from the images generated.

**Conclusion**

By following these steps, I successfully tracked hurricanes during the 2016 Atlantic Hurricane Season using spatio-temporal on QGIS. The resulting video provides valuable insights into the paths and intensities of hurricanes over time, aiding in understanding and preparedness for future events
