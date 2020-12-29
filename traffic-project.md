## Forecasting Street Speed using Alternative Data

Joint Work: Lingbo Ji, Yaowei Zong

[Paper](/pdf/TrafficPaper.pdf)
<br>
[Powerpoint](/pdf/TrafficSlides.pdf)
<br>
[Code](https://github.com/ls5122/TrafficForecasting)

**Overview** Forecasting of street speed is a pivotal component of many traffic analysis and route prediction algorithms. Street speed over a geographic region is highly non-linear being effected by both seasonal and environmental factors. To test our hypothesis of using alternative data sources to supplement forecasting street speed we train a baseline model against one with additional weather data and traffic collision counts. We train and evaluate our model over several New York City streets. Our results indicate significant improvement over the baseline particularly due to the pandemic lockdown in early 2020. 

### 1. System Design

To test the effectiveness of our approach we predict street speeds with additional weather data and motor vehicle collision counts by utilizing Fbprophet <sup>[1](#prophet)</sup>, a lightweight time series forecasting toolkit. Our models are trained on New York City street speed data from 2019 and are used to forecast speeds for 2020. Our system architecture is displayed below. Initially the raw data is taken from three different sources. Hourly street speed data <sup>[2](#uber)</sup> , weather data <sup>[3](#weather)</sup>, and motor vehicle crashes <sup>[4](#crashes)</sup>. After the data sources are placed into HDFS we apply data cleaning and pre-processing using MapReduce to construct curated data sets that are ready for analysis. Then using Spark we construct DataFrames over the data sources and apply additional steps to fit the structure for forecasting. Finally using our time series forecasting tool, Fbprophet in conjunction with Pyspark we construct forecasts over our testing period for a fixed number of streets. These can then be stored in HDFS or otherwise the average RMSE is calculated against the true street speed values. 


<img src="images/TrafficSchema.png?raw=true"/>


### 2. Methods 


A Fbprophet time series forecasting model is decomposable as noted in the equation below. The  trend g(t) refers to the overall behavior of the time series and can be linear or logisitic.  The seasonality s(t) refers to the effect of time related components such as time of day, week,year. Holiday effects refer to effects of specific days on our prediction and is depicted as h(t), this concept can be extended to include additional external regressors such as our alternative environmental data. Finally e denotes the additional components not accounted for by the model components, this is assumed to come from a normal distribution.

``
y(t) = g(t) + s(t) + h(t) + e
``

Fbprophet is an out of the box time series forecasting model in the sense that it does not require users to calculate features or apply rigorous preprocessing. To conduct forecasting the time series just needs to be put into the format usable by the model. There are however several hyperparameters regarding the components previously described. Due to the nature of vehicle traffic the trend can only be linear while both seasonality and our added regressors can both be either additive or multiplicative. When seasonality or regressors are additive it means that the model learns the fluctuations from the trend line through addding or subtracting. In contrast multiplicative regressors measure fluctuations from the trend line through positive or negative dilation.

**Model 1** Our baseline model utilizes only trend and seasonality with no added weather features or traffic collision counts. We construct a Fbprophet model trained on daily speed data from 1/1/2019 to 12/31/2019 and forecast street speeds over 1/1/2020 to 3/31/2020. The error for a single street is the Root Mean Squared Error (RMSE) and the error over the dataset is the average RMSE.

**Model 2**  Our second model utilizes both trend and seasonality but also incorporates the additional features rain, snow, freezing, visibility, crashes. These features are added as regressors and street speed is forecast along an identical time horizon as Model 1. We utilize the average RMSE to evaluate the efficacy of using the additional features. 


### 3. Results

<img src="images/HyperparameterErrors.JPG?raw=true"/>

First we tuned our hyperparameters on a validation set of 15 seperate street speed time series. Our results indicated that using a linear trend with multiplicative seasonality and multiplicative added regressors would be optimal. Then, utilizing Pyspark to interface between the raw data and Fbprophet we ran our baseline model against the one with added regressors on 295 different street speed time series. The average RMSE was **19.55** versus **16.55** when comparing the baseline model to our proposed solution. Traffic collisions served as a proxy for the COVID-19 lockdown which began in mid-March 2020 while also providing a global indicator of traffic across NYC. The figures below display the baseline and updated model respectively. 


<img src="images/TrafficBaseline.JPG?raw=true"/>


<img src="images/street_speed.png?raw=true"/>


Our hypothesis was correct in that utilizing external environmental features like weather and traffic collisions counts is an improvement over the baseline time series forecasts. We believe the COVID-19 lockdown is responsible for larger error on the baseline and that our model anticipates that. Fbprophet is a lightweight time series forecasting tool that has allowed us to test our hypothesis regarding alternative data use for the traffic forecasting problem. Despite limiting model complexity, utilizing Fbprophet for forecasting along with Apache Spark for data infrastructure has allowed us to quickly prototype and test our ideas.  

<a name="prophet">[1]</a>: J. Taylor and B. Letham,    “Forecasting at scale, ”Peer J Preprints, vol. 5, p.e3190v2, Sep. 2017. [Online]. Available: https://doi.org/10.7287/peerj.preprints.3190v2

<a name="uber">[2]</a>: Uber historical speeds, hourly time series, ”Uber Movement, Dec.2020. [Online]. Available: https://movement.uber.com/cities/newyork/downloads/speed

<a name="weather">[3]</a>: “NYC local climatological data” National Center for Environmental Information, Dec. 2020. [Online]. Available: https://www.ncdc.noaa.gov/cdo-web/datasets/LCD/stations/WBAN:94728/detail


<a name="crashes">[4]</a>: Motor  vehicle  collisions - crashes,”  NYC  Open  Data,  Dec.  2020. [Online]. Available: https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95



