## Forecasting Street Speed using Alternative Data

Joint Work: Lingbo Ji, Yaowei Zong

[Paper](/pdf/TrafficPaper.pdf)
<br>
[Powerpoint](/pdf/TrafficSlides.pdf)
<br>
[Code](https://github.com/ls5122/TrafficForecasting)

**Overview** Forecasting of street speed is a pivotal component of many traffic analysis and route prediction algorithms. Street speed over a geographic region is highly non-linear being effected by both seasonal and environmental factors. To test our hypothesis of using alternative data sources to supplement forecasting street speed we train a baseline model against one with additional weather data and traffic collision counts. We train and evaluate our model over several New York City streets. Our results indicate significant improvement over the baseline particularly due to the pandemic lockdown in early 2020. 

### 1. System Design

To test the effectiveness of our approach we predict street speeds with additional weather data and motor vehicle collision counts by utilizing Fbprophet <sup>[1](#prophet)</sup>, a lightweight time series forecasting toolkit. Our models are trained on New York City street speed data from 2019 and are used to forecast speeds for 2020. Our system architecture is displayed below. Initially the raw data is taken from three different sources. Hourly street speed data <sup>[2](#uber)</sup> , weather data <sup>[3](#weather)</sup>, and motor vehicle crashes <sup>[3](#crashes)</sup>. After the data sources are placed into HDFS we apply data cleaning and pre-processing using MapReduce to construct curated data sets that are ready for analysis. Then using Spark we construct DataFrames over the data sources and apply additional steps to fit the structure for forecasting. Finally using our time series forecasting tool, Fbprophet in conjunction with Pyspark we construct forecasts over our testing period for a fixed number of streets. These can then be stored in HDFS or otherwise the average RMSE is calculated against the true street speed values. 


<img src="images/TrafficSchema.png?raw=true"/>


<a name="prophet">[1]</a>: J. Taylor and B. Letham,    “Forecasting at scale, ”Peer J Preprints, vol. 5, p.e3190v2, Sep. 2017. [Online]. Available: https://doi.org/10.7287/peerj.preprints.3190v2

<a name="uber">[2]</a>: Uber historical speeds, hourly time series, ”Uber Movement, Dec.2020. [Online]. Available: https://movement.uber.com/cities/newyork/downloads/speed

<a name="weather>[3]</a>: “NYC local climatological data” National Center for Environmental Information, Dec. 2020. [Online]. Available: https://www.ncdc.noaa.gov/cdo-web/datasets/LCD/stations/WBAN:94728/detail



         


