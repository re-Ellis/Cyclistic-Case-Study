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
Historical trip data of the fictional company Cyclistic is made available from [divvy_tripdata][https://divvy-tripdata.s3.amazonaws.com/index.html].  For the purposes of this study, the last 12 months of trip data have been used (dated from July 2023 to June 2024).  This data is made publicly available by Motivate International Inc. under this [licence agreement][https://divvybikes.com/data-license-agreement].

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

The City of Chicago releases data on bike stations [here][https://data.cityofchicago.org/Transportation/Divvy-Bicycle-Stations-Map/bk89-9dk7], so it can be seen that the data is in use and should not be incomplete or inaccurate. The data is sourced from its original creator, so we can expect it to be first party data.  Some personal data is omitted from the resource to ensure identifiable information cannot be accessed but otherwise the listed data is comprehensive.  To ensure the relevance of information, all data is the most recently made available and is sourced from Divvy Bikes and the City of Chicago.

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

##Data Manipulation
Two additional columns were added to the data:
* _tripDuration_ representing the time elapsed from the start to the finish of the bike trip.
* _distTravelled_ representing the magnitude of distance travelled from start to finish,.
Outlier entries where the values calculated above where unrealistically high were removed as these were likely due to rider not returning to a station after completing their trip.
