# Case-Study: How Does a Bike-Share Navigate Speedy Success
In this case study, I am assuming the role of a junior data analyst for the fictional, Chicago-based company Cyclistic.  This report will outline the methodology and analysis of company data making use of R and Tableau to identify and present insights.

## The Scenario
Following the successful launch of their rideshare services, the Chicago-based bike-share company Cyclistic, has expanded their operations to a fleet numbering 5,824 bicycles across 692 stations scattered throughout the city.  Leading up to today, the company’s marketing strategy has been focused on reaching a broader audience of new customers. Their approach involved introducing flexible pricing plans to cater to casual users, who only need single trip or day passes, and annual members, making use of their services year-round.

Cyclistic’s financial analysts have concluded that annual members are more profitable than casual riders, and it is believed that there is an opportunity to convert casual users into annual members.  Thus, we are tasked with the goal of designing data-driven marketing strategies to convert casual users to annual members. 

### Key Insight Objectives
To assess what strategies might yield positive results, it is essential to understand the differences in the use cases of annual members and casual riders.  We aim to address the following questions so that insights can be developed:

* How do different types of riders use Cyclistic’s bikes?
* What changes would convince casual users to become annual members?
* What reasons do casual users have for remaining casual users?
* How can digital media be utilized to influence casual users to become members?

Through analysis of the data, we seek to identify patterns in the habits of the two types of users that will allow us to draw informed insight on the questions above.

## Data Preparations
### Data Sources
Historical trip data of the fictional company Cyclistic is made available from [divvy_tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html).  For the purposes of this study, the last 12 months of trip data have been used (dated from July 2023 to June 2024).  This data is made publicly available by Motivate International Inc. under this [licence agreement](https://divvybikes.com/data-license-agreement).

### Our first look at the data
The data is comprised of 12 .Csv files each containing the trip data from a month of operations. Each row entry contains up to 13 columns of fields, each column described below:

Column name         |        Description
------------------- | ---------------------------------------------------
_ride_id_           |        Unique ride identifier
_rideable_type_     |     	 Type of bike: Classic, Docked, Electric
_started_at_        |        Date and time at start of trip
_ended_at_          |        Date and time at end of trip
_start_station_name_|	  	   Trip starting station name
_start_station_id_  |  	     Trip stating station identifier
_end_station_name_  |  	   	 Trip ending station name
_end_station_id_    |   	   Trip ending station identifier
_start_lat_         |        Latitude at start of trip 
_start_lng_         |        Longitude at start of trip
_end_lat_           |        Latitude at end of trip
_end_lng_           |        Longitude at end of trip  
_member_casual_     |    	 	 Type of rider: Member or Casual 

### Data Integrity
Before making use of any data, it is important to ensure that it can be relied upon.

1. Reliability
   * The City of Chicago releases data on bike stations [here](https://data.cityofchicago.org/Transportation/Divvy-Bicycle-Stations-Map/bk89-9dk7), so it can be seen that the data is in use and should not be incomplete or inaccurate.
2.Originality
   *   The data is sourced from its original creator, so we can expect it to be first party. data.
3. Comprehensive and Current:
   * Some personal data is omitted from the resource to ensure identifiable information cannot be accessed but otherwise the listed data is comprehensive.  To ensure the relevance of information, all data is the most recently made available and is sourced from Divvy Bikes and the City of Chicago.
4. Cited
   * Data cited above.

## Data Processing
### Data Cleaning (performed in R)
Before performing any analysis, the data is checked for any inconsistencies in formatting or errors in entries:
1. _ride_id_
   * id's are unique
   * Id's comprised of only letters and nunbers
   * No NA/NULL values
2. _rideable_type_
   * only one of three types of bikes: classic, electric, docked
3. _started_at_, _ended_at_
   * Reformatted to ensure consistent date formatting
   * NA/NULL entries removed since these would be caused by some error<sup>**[1]**<sup>
4. _start_station_name_, _start_station_id_, _end_station_name_, _end_station_id_
   * No NA/NULL values
5. _start_lat_, _end_lat_
    * Latitude values are bounded by the limits [-90,90]
    * NA/NULL entries removed since these would be caused by some error<sup>**[2]**<sup>
6. _start_lng_, _end_lng_
    * longitude values are bounded by the limits [-180,180]
    * NA/NULL entries removed since these would be caused by some error<sup>**[2]**<sup>
7. _member_casual_
   * Only one of two membership types: member, casual

* **[1]**: if a ride does not record when it has started or ended it is safe to assume that some error has occured and the entry should be removed since it is likely to contain incorrect information
* **[2]**: NA coordinates were found to occur in some entries.  Further investigation revealed that where the coordinates were recorded as NA, the trip length was always exactly 1 day and 59 minutes, signifying some timeout feature ending trips after a set time without recording the coordinates.  These entries were removed as they have inaccurate records.

## Data Manipulation
Two additional columns were added to the data:
* _tripDuration_ representing the time elapsed from the start to the finish of the bike trip.
* _distTravelled_ representing the magnitude of distance travelled from start to finish.

Outlier entries where the values calculated above where uncharacteristically high were removed as these were likely due to the rider not returning to a station after completing their trip.

## Data Analysis
Now that the data has been reviewed for its integrity and has been processed we can now proceed with the analysis of the data.  Please refer to the R markdown analysis, to see how calculations were performed.

### Basic data Insights

### When Are Riders Using the Service?
The following data visualisations seperates rides by a specified time-frame so that we can see how often the service is utilised at different times of the year.

Separating usage by months of the year:
![Ride-Share Usage by Months of the Year](https://github.com/user-attachments/assets/617f2087-8461-4500-baed-e159e151f54c)
We can see that both groups of users follow the same trend of prefering to use the bike-share during the hotter months of the year, with the number of casual rides dropping very low during the colder months.

Separating usage by Days of the week:
![Ride-Share Usage by Days of the Week](https://github.com/user-attachments/assets/58039c2c-07e0-4724-8600-dd7ae69369c9)


![Ride-Share Usage by Hours of the Day](https://github.com/user-attachments/assets/a0e34066-67e6-4d3b-a036-3b800fedc71c)




![Bike Type Proportions - Member](https://github.com/user-attachments/assets/ce48fd8f-7046-45ef-b8cc-97a77674d3ff)
![Bike Type Proportions - Casual](https://github.com/user-attachments/assets/20277bdb-b56e-49c8-b157-84b60d63a8e2)
![Average Trip Length](https://github.com/user-attachments/assets/ec36af4a-76a7-47fe-ba64-596c543415e6)
![Average Distance Travelled](https://github.com/user-attachments/assets/b8830e67-a3cf-42ed-b7fb-46cd4aa47ef9)

![Most Popular Stations - Member Users](https://github.com/user-attachments/assets/64d10a9c-8194-421c-861b-2118bc6c0990)
![Most Popular Stations - Casual Users](https://github.com/user-attachments/assets/aad61426-065c-45a9-b3f3-dadba8882230)
