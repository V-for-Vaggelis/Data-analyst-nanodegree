# Traffic patterns in the city center of Thessaloniki
## by Athanasakis Evangelos

## Dataset

The dataset used in this project, includes traffic data of 9 roads located in the city center of Thessaloniki from the years 2015 to 2020. These 9 particular roads, are located around the reference air-quality monitoring station which is the reason they are chosen. The data source is a project called TrafficThess for which you can find more information <a href="https://www.trafficthess.imet.gr/">here</a>. The data were accessed through the "historical data" option in this website. Originally the website provides one dataset for each road seperately, where the variables of interest are statistical parameters of the speeds of several taxis crossing the road and each observation differs by a 15 minute interval (or more) from the previous.The dataset provided here is an engineered version of all those datasets merged together. In detail, there were originally 9 datasets from which we used the variables mentioned below:
* timestamps : Time of the recording
* id : A number that is given by the TrafficThess project to each road
* road name : Name of the road in Greek characters
* average speed (u): The average speed of all the taxis within the time recording interval
* free flow speed (u<sub>f</sub>): The speed at which a vehicle can move in the road at zero congestion conditions

In order to merge the 9 roads into 1 dataset we performed the following operations:
1. Upsampled the data for all the roads so that we have hourly observations
2. Estimated the traffic performance index (tpi) for each road, which scales from 0 (no traffic congestion) to 1 (full congestion) and is defined as (u<sub>f</sub> - u)/u<sub>f</sub>
3. Named each column with name of the road and the id in parentheses
4. "Downscaled" any tpi values that are greater than 1 to 1. In fact an outlier analysis was performed and it was determined that most outliers fall within the road's free flow speed limit, so removing outliers would be a loss of information. Also speed limits often get exceeded but that does not mean that those observations are errors.

Our goals through analyzing this dataset are:
1. To provide insights in the relationships between traffic conditions in different roads in order to answer how a single traffic congestion variable could be created to represent the whole area and serve as a variable to be compared with the air-pollution data provided by the station. For example, would averaging the 9 roads be a good choice? Or are there extreme differences between them.
2. to provide neat visualizations that communicate the traffic patterns in the city center of Thessaloniki. We can create extra categorical features that correspond to hour, day, and month to help capture and visualize patterns in the traffic data


## Summary of Findings

1. A faceted plot of colormaps was produced to explore the distribution of missing values. The data provided by TrafficThess have lots of missing values but their patterns are usually continuous (there are no large chunks of missing values). This means, that a check can be applied to drop any roads with too many missing values (could be more than 50%) and k-nn imputation could be used to impute the rest.
2. A faceted plot of histograms was produced for each road. The congestion levels at a road typically follow a left-skewed distribution. In some cases a second small mode seems to appear at the left. This probably has to do with the way we handle outliers during our preliminary wrangling
3. A faceted set of scatterplots between different roads was produced using a subset of one month's data. The roads appear to have moderate correlations with each other.
4. Average daily profiles for each road were produced by averaging all of the data by hour. The average daily profiles show similar behavior, which means that averaging all roads is a good approximation to deduct congestion levels around an area. The ideal radius of averaging should be researched. Congestion levels usually show 2 peaks during the day: 12:00 - 03:00 and 18:00 - 20:00.
5. Average congestion by month and day was studied by averaging and producing barplots. Winter months seem to have the largest congestions and summer the smallest. Also weekends have smaller congestions than the rest of the days.
6. Some heatmaps were produced in order to study the congestion levels by 2 time-related variables at once. Some interesting insights come to light:
  * Days and months that showed small congestions before, show the smallest congestions in total, when they occur at the same time (for example Sundays in summer months) and the other way around
  * The maximum congestion occurs at Thursday and Tuesday afternoons.
  * In December, noons and afternoons are much more congested than in other months.


## Key Insights for Presentation

The visualizations mentioned below should be enough to communicate our findings:
1. _Barplot of average tpi by day._ Corrections to be made:
  * Set the y-axis limit to 1 to demonstrate that the tpi can scale up to 1
  * Rotate the x-axis labels so that they are vertical
  * Add more descriptive y label name
  * Provide proper ordering in the days
2. _Barplot of average tpi by month._ with the same corrections
3. _Heatmap of average tpi by hour by day._ Corrections to be made:
  * Replace y-axis ticks by well foramted hours, for example 0 should be 00:00 and rotate them if needed.
  * Rotate x-axis labels vertically.
  * Provide proper ordering in the days
