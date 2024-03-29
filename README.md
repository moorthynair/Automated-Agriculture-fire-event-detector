# Automated-Agriculture-fire-event-detector
These set of codes can automatically download and process agriculture fire events occurred over an area of interest with a single click. The base information/data for analysis are retreived from the National Aeronautics and Space Administration (NASA) owned satellites. Since NASA provides data on Near Real Time (NRT) basis (Lag of 1-3 hours of the satellite overpass), one can run the code multiple times a day to track the latest fire events. 

## The purpose
Burning crop residue is generally discouraged because of the harm it does to soil biodiversity and public health owing to air pollution. The agriculture officials are mandated to monitor such illegal activity in order to take appropriate action. Automating this exercise using satellite-retrieved data will optimise the on-ground judgements and actions because performing this activity manually in an agricultural state is difficult and has a high risk of missing out on specific fire activities.
 
**Note:** We are here only retreiving NRT and past data upto 10 days to facilitate officals with immediate information for corrective actions. For retrospective data, one must access the archieve dataset provided in the NASA FIRMS website.

## Pre-requisites
**Req-1:** Generate a API from NASA FIRMS that lets you download NRT and past (upto 10 days) fire event dataset. [Click here](https://firms.modaps.eosdis.nasa.gov/api/area/) to access website and scroll down to generate one

**Req-2:** We shall be filtering the agriculture fire events using Landuse Landcover dataset. If you have one of your own , well and good or else download the MODIS LULC (MCD12Q1 V006) by access the website [clicking here](https://lpdaac.usgs.gov/products/mcd12q1v006/)

**Req-3:** All the essential python libraries for simulating the script mentioned [`requirements.txt`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/requirement.txt)

## what are the user inputs required to download fire data from NASA platform?
1. Generated API (Refer pre-requisites Req-1)
2. Area of Interest for which the fire point has to be downloaded (provide the lat/lon of the geometry bounding box: North:maxy,East:maxx,West:minx,South:miny)
3. Date for which the fire events to be downloaded
4. Date range (1 - 10 days)
5. Path to store the analysed data

## What are the user dataset to be provided for performing the analysis?
1. Shapefile of the area of interest. Explore more about India mass land shapefiles at [Survey of India](https://onlinemaps.surveyofindia.gov.in/Digital_Product_Show.aspx)
2. Landuse Landcover dataset (Refer pre-requisites Req-2. Save it in a folder named 'LU_LC_Raster' in the path input by the user) 
3. Other shapefile files such as forest boundary, industrial area boundary for filtering agriculture fire events from other fire events. See Step 5 below 

## Check out the code step by step

| Step no | Description | Script link |
| ------- | ----------- | ----------- |
| Step 1  | Download the fire data from the NASA platform. The information pertaining to fire such as geolocations, acquisition date/time, satellite, Fire Radiactive Power, confidence, daynight, etc monitored and processed from the the S-NPP, NOAA and MODIS (TERRA) satellites are downloaded in .CSV format. Read more about the products [VIIRS](https://www.earthdata.nasa.gov/learn/find-data/near-real-time/firms/viirs-i-band-375-m-active-fire-data) & [MODIS](https://www.earthdata.nasa.gov/learn/find-data/near-real-time/firms/mcd14dl-nrt). |  [`Step_1_Retreiving the fire data.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Step_1_Retreivng%20the%20fire%20data.py) |
| Step 2  | Merge the downloaded fire information to a single .csv format. 3 different satellite dervied information files downloaded are joined to a single file. | [`Step_2_merging of data.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Step_2_merging%20of%20data.py) |
| Step 3  | Convert fire point data to shapefile format. We maintain an uniform format throughout the analysis and GIS based formats are preferred. | [`Step_3_Convert fire data point to shapefile.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Step_3_Convert%20fire%20data%20point%20to%20shapefile.py)|
| Step 4  | Clip the fire points that are within the area of interest. We use 'overlay' method here to clip necessary information within the region of intereset. Upon runing the overlay function every single fire points are integrated with location information fetched from the area of intereset shapefile information (such as Panchayat, block, district related details). | [`Step_4_Clip to boundary.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Step_4_Clip%20to%20boundary.py)|
| Step 5  | Filter the fire points that are falling within the agriculture landuse. To further enhancing agriuclture fire event retrivals, one can use Landuse Landcover dataset to filter agriculture fire points from non-agriculture fire points.  one can either continue to step 5 or skip to step 6. It is suggested to perform this step as it removes all non-agriculutre fire points from the total dataset. [Run this MODIS LULC raster file processing step once](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/LU_LC%20Retreivals.py) before proceeding to the script | [`Step_5_Fine tunning by assigning land class.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Step_5_Fine%20tunning%20by%20assigning%20land%20class.py)|
| Step 6  | Saving the results/Performing Multiple runs. The first run result is saved in the 'Analysed Results' folder in .csv format with the current date as the filename. However, in order to catch the most recent data that is added to the database every 1-3 hours, one must run the script several times throughout the day. From the second simulation onward, the script is saved in the "Mutiple Run" folder with the date and run number as the filename. | [`Step_6_Save the results.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Step_6_Save%20the%20results.py)|

------

### Okay Now try running the code all together 
Access to **[`all in one code.py`](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/All%20in%20one%20code.py)**

Have a look at the final informations/folders that shall be generated in the path provided by the user after running the program [Click here](https://github.com/moorthynair/Automated-Agriculture-fire-event-detector/blob/main/Final%20Path.png)

Note that the area of interest, shapefiles used in the current analysis are pertaining to Bihar State (India). You many modify the area of interest accordingly
