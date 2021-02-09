## Analytics of r/WallStreetBets


[www.wsb-viz.com](http://www.wsb-viz.com)


### 1. System Architecture 
To gain insight on the evolving nature of the popular reddit forum 
r/WallStreetBets all forum comments were collected and analyzed to
provide up to date statistics regarding forum activity over the previous 
week. The forum is known for having a large effect on several stocks during
the beginning of 2021. This application was created to monitor potential
changes in crowdsourced behavior regarding financial products. The system
architecture displayed below is deployed using the Google Cloud Platform
and features an ingestion, buffering, storage, visualization and deployment
phase. 

<img src="images/wsb-architecture.PNG?raw=true"/>
### 2. Data Ingestion and Buffering

A Linux VM is configured to ingest comments from the forum using <a href = https://praw.readthedocs.io/en/latest/> Python Reddit Api Wrapper </a> messages
are then published to a GCP Pub/Sub topic which is a distributed scalable messaging service and ensures reliability of data ingestion. Next we utilize
the GPC Dataflow service which connects the published messages to BigQuery our data warehouse.

### 3. Data Storage

Once data is in BigQuery it is subject to automated scheduled queries. To ascertain the most popular financial products discussed during the last
week we join the reddit comments to a list of NYSE symbols. Other analytics regarding the dataset is also automated and stored in seperate tables.
This allows our analytics output to be much smaller in size than the overall data we store in BigQuery

### 4. Data Visualization

Tableau is used to connect to BigQuery and display data visualizations. This dashboard is then published on Tableau Public with data extraction automatically
refreshed when new queries are scheduled in BigQuery.


### 5. Deployment

Finally a website was constructed to host the dashboard and other information regarding this project. The website was written in React and NodeJS.

<img src="images/wsb-vizwebsite.JPG?raw=true"/>

