# MAW Framework

## MAW Input Files (Grace, 3 hours)

- variety of inputs that MAW can take (csv, dataframes and others); Note from Grace: need to check with Anuj about whether json and sql will be accepted.
- input fields (required and optional)
- describe the use of standardized names for computing efficiency. Note from Grace: need to check the staandardized name of datetime with Anuj. Currently using DATETIME as placeholder here. 

The Mobility Analysis Workflow (MAW) framework is designed to analyze location-based data, such as Global Positioning System (GPS) and cellular data, to extra meaninful insights about device carriers' mobility patterns. More specifically, mobility patterns tell us about the locations where a device carrier spends time, how long the device stays at each location, and the routes and modes taken between locations. Such analysis is possible because the data collected commonly contain coordinates (latitude and longitude), timestamps of the coordinates being observed to track movement over time and space. Typically, the data is recorded in a tabulated form, where each row represents a sighting of the device at a particular location, indicated by the coordinates field, and time, indicated by the timestamp field. 

The MAW workflow is designed to process such data, particularly in the following format: comma-separated values (CSV) files, xlsx files, pandas dataframes, and other structured data fromat such as Json and SQL databases. As mentioned above, the input data requires specific fields to function correctly, which include the following: (1) device unique identifier, (2) latitude when observed, (3) longitude when observed, (4) timestamp when observed. The device unique identifier is used to distinguish one device from another, while the latitude and longitude fields provide the location of the device at a particular time. The timestamp field indicates the time when the device was observed at the given coordinates. Additionally, the input data can include optional fields such as an uncertainty radius, which provides information about the accuracy of the coordinates.

However, these fields do not have a uniform name across different datasets and vendors who collect these data. Thus, the MAW framework uses standardized names for input fields to ensure consistency and efficiency in data processing, helping to automate the data processing steps and reduce the chances of errors or misinterpretations. Specifically, the standardized names for input fields are as follows: (1) USER_ID for device unique identifier, (2) ORIG_LAT for latitude when observed, (3) ORIG_LONG for longitude when observed, and (4) DATETIME for timestamp when observed. By using these standardized names, the MAW framework can easily identify and process the relevant data fields, regardless of the original naming conventions. Overall, the MAW framework is designed to be flexible and user-friendly, allowing users to analyze their location-based data efficiently and derive meaningful insights about mobility patterns.

## MAW Output Files (Anurag, 2 hours)

The MAW code aims to provide insights into user trajectories and mobility patterns through its various algorithms and workflows. This program is designed to facilitate analysis, allowing users to utilize it according to their specific requirements. The program generates four output files, as described below:

## 1. Raw Output File

This file contains the same number of rows as the input file. Its purpose is to provide comprehensive output information that users can utilize for their own studies.

| UID | Orig_lat | Orig_long | datetime | Stay_lat | Stay_long | Orig_unc | If_stay_point | Stay_unc | Stay_ID |
|-----|----------|-----------|----------|----------|-----------|----------|---------------|----------|---------|
|     |          |           |          |          |           |          |               |          |         |
|     |          |           |          |          |           |          |               |          |         |

**Column Descriptions:**
- **UID:** Unique identifier corresponding to a user.
- **Orig_lat:** Latitude of a row item in the input file.
- **Orig_long:** Longitude of a row item in the input file.
- **datetime:** Timestamp when the row item was captured.
- **Stay_lat:** Latitude of the stay location associated with the row item.
- **Stay_long:** Longitude of the stay location associated with the row item.
- **Orig_unc:** Uncertainty (in meters) for the row item. This parameter is optional in the input file and will not be present in the output file if not provided.
- **If_stay_point:** Identifier indicating if the row item is a stay point (1 means a stay point).
- **Stay_unc:** Uncertainty (in meters) for the row item if it is part of a stay.
- **Stay_ID:** Unique identifier that helps users recognize the stay cluster to which the row item belongs.

## 2. Stay Output File

This file contains information related to stay points only, enabling users to conduct in-depth analyses and derive insights on stay location patterns. The number of rows in this file may differ from the input file.

| UID | Stay_ID | Stay_lat | Stay_long | starttime | endtime | Size | Stay_dur |
|-----|---------|----------|-----------|-----------|---------|------|----------|
|     |         |          |           |           |         |      |          |
|     |         |          |           |           |         |      |          |

**Column Descriptions:**
- **UID:** Unique identifier corresponding to a user.
- **Stay_ID:** Unique identifier that helps users recognize the stay cluster to which the row item belongs.
- **Stay_lat:** Latitude of the stay location associated with the row item.
- **Stay_long:** Longitude of the stay location associated with the row item.
- **starttime:** Timestamp of the first entry in the stay cluster for the user.
- **endtime:** Timestamp of the last entry in the stay cluster for the user.
- **Size:** Number of row items included in the stay cluster for a given Stay_ID.
- **Stay_dur:** Total duration the user spent in the stay cluster (endtime - starttime).

## 3. Trajectory Output File

This file provides information about the trajectory pattern of a user for a given date. The number of rows in this file may differ from the input file.

| UID | Date | Trajectory string | Starttime | Endtime | Stay_indicators |
|-----|------|-------------------|-----------|---------|-----------------|
|     |      |                   |           |         |                 |
|     |      |                   |           |         |                 |

**Column Descriptions:**
- **UID:** Unique identifier corresponding to a user.
- **Date:** Date for which the trajectory information is captured.
- **Trajectory string:** List of (lat, long) tuples for a given user, sorted by datetime for the specified date. For example, if (a,b), (c,d), and (e,f) are the (lat, long) for a user on a given date, the trajectory string will be [(a,b), (c,d), (e,f)]. Each tuple can represent either a stay point or a moving point.
- **Starttime:** Timestamp of the first entry in the trajectory string.
- **Endtime:** Timestamp of the last entry in the trajectory string.
- **Stay_indicators:** A mapping of the trajectory string indicating whether each (lat, long) is a stay point (1) or not (0). For example, if (a,b) and (c,d) are stay points and (e,f) is not, the indicator list will be [1, 1, 0].

## 4. Trip Output File

This file provides information about trips for a given user. A trip is defined as a journey from one stay location to another, including non-stay (transient) points in between. (Further details on the definition of a trip are to be determined.)

| UID | Date | Trip string | Starttime | Endtime |
|-----|------|-------------|-----------|---------|
|     |      |             |           |         |
|     |      |             |           |         |

**Column Descriptions:**
- **UID:** Unique identifier corresponding to a user.
- **Date:** Date for which the trajectory information is captured.
- **Trip string:** List of (lat, long) tuples where the first and last tuples are stay points, and the others are moving points. For example, if [(a,b), (c,d), (e,f), (g,h), (i,j)] is a trip string, (a,b) and (i,j) are stay points, and the others are moving points.
- **Starttime:** End time of the first stay point (a,b) in the trip string.
- **Endtime:** Start time of the last stay point (i,j) in the trip string.

Note from Grace: after the above output files are defined, we might need to discuss the standardization of the output field names with Anuj. Currently I see STAY_LAT, STAY_LONG, STAY_UNC, STAY. It seems that STAY_ID is not present in incremental clustering file.

## MAW core data processing functionalities

overall goal: not to invent any algorithms; the goal is to make sure that the algorithms finally developed by escience people are consistent with the algirthms stated in the two papers cited below. 

### DP: Data partition (optional)

partition datasets into different sets with different uncertainty radius (if
available)

### OSC: Addressing Oscillation (Grace, two days)

review this paper
(https://www.sciencedirect.com/science/article/pii/S0968090X17303637?via%3Dihub#s0035)
and the algorithm itself. test.

### TIME4OSC: Identifying time window for oscillation removal (Grace, 1 day)

review this paper
https://www.sciencedirect.com/science/article/pii/S0968090X17303637?via%3Dihub#f0015,
section 4.3.2. Create plot such as Figure 10

### TRACESEG: Identifying stays from low-variance (GPS) data (Anurag: two days)

trace segmentation algorithm. review this paper
https://www-sciencedirect-com.offcampus.lib.washington.edu/science/article/pii/S0968090X18316085?via%3Dihub#fn4,
section 4.2.1 as well as Appendix A.1. and the algorithm itself. Test algorithm.

### INCREMENTAL: Identifying stays from high-variance (cellular) data (Anurag: 1 day)

incremental clustering algorithm. review the above paper, section 4.2.2. And
test algorithm.

### STAYINT: Integrate stays (Grace: two days)

refer the above paper section 4.3 and the algorithms itself

### STAYCAL: Update stays (Anurag: 4 hours)

update stays and duration.

### STAY_INC_THRESHOLDS: Identifying suitable spatial thresholds for stay identification (incremental clustering) (km) (Anurag: 1 day)

create plots that shows number of stays per user per day. see Figure 7 in this
paper:
https://www.sciencedirect.com/science/article/pii/S0968090X17303637?via%3Dihub#f0015

## MAW core analysis functionalities

### S_USER: Single user analysis (Anurag: 1 day)

show a user's raw trajectory on a map with stays identified along with duration
information. See MAW paper figure 1, with the underlying map. bubble. 

it shall take a single user's data only. If a user inputs multiple users' data,
an error shall be returned.

### A_USER: Aggregate user analysis 

shows many users' stays on a map with stays being aggregated as POIs

### HOME: Identifying home locations (Grace: 1 day)

pls read section 5.2.1 in paper "extracting stays..."

identifying home locations require multiple days' data (a minimum of five days within a month).
Below is the rule: the home location is identified as the tract containing the
stay with the most frequent visits during the night time (22:00 pm to 6:00 am
the next day), with the condition that it has to be visited at least X times for
a time period. See homeloc_threshold.png for the criteria. After 21 days, it
requires at least once a week.
