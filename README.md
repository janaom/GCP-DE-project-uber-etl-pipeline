# GCP Data Engineering project

The purpose of this project is to analyze the [TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page). Trip records include fields capturing pick-up and drop-off dates/times, pick-up and drop-off locations, trip distances, itemized fares, rate types, payment types, and driver-reported passenger counts. To understand the data and its fields, please refer to the data dictionary provided by the TLC (`data_dictionary_trip_records_yellow.pdf`). 


The analysis of the TLC Trip Record Data will be conducted using the following technologies and tools:



- Cloud Storage 🪣 to store and manage the trip record data

- Compute Engine 💽 to host and run the Mage 

- Mage 🧙‍♂️ for Extract, Transform, Load (ETL) processes

- BigQuery 🔍 as our data warehouse for storing and querying the transformed trip record data

- Looker📈 as our business intelligence and data visualization platform


    
💡 Additionally, a key objective of this project is to gain familiarity with the Mage tool. Mage offers an alternative approach to Airflow for managing ETL workflows and will be the primary tool used to orchestrate the data pipeline.



![image](https://github.com/janaom/GCP-DE-project-uber-etl-pipeline/assets/83917694/6cada155-6df3-4497-8410-ab0d9f4d0b09)

<img width="471" alt="image" src="https://github.com/janaom/GCP-DE-project-uber-etl-pipeline/assets/83917694/126e73f5-053a-4287-9266-88f562570c14">


# GCS 🪣

Create a new bucket, upload the csv file. Change the permissions of the bucket (permissions -> edit access control -> Fine-grained), edit the access of the csv file to make it publicly available

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/66511ae3-9543-4d96-b6d9-22f8a6ac1483)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/38b135a9-1b16-4e15-b082-0aecc5942c30)


# Compute Engine 💽

Create a new instance, e.g. Machine type: e2-standard-4

SSH into your VM and run these commands to install Python, pip, wget, pandas, Google Cloud Library, Google Cloud BigQuery library 

`sudo apt-get install update`

`sudo apt-get install python3-distutils`

`sudo apt-get install python3-apt`

`sudo apt-get install wget`

`wget https://bootstrap.pypa.io/get-pip.py`

`sudo python3 get-pip.py`

`sudo pip3 install pandas`

`sudo pip3 install google-cloud`

`sudo pip3 install google-cloud-bigquery`


Run `sudo pip3 install mage-ai` to install [Mage](https://github.com/mage-ai/mage-ai#%EF%B8%8F-install) on your VM.

To start a new Mage project: `mage start de-uber-project` (you will see: `Checking port 6789...` if you restart Mage, the port may change e.g. `Checking port 6790...`)

Create a new firewall rule for the port 6789

<img width="561" alt="image" src="https://github.com/janaom/GCP-DE-project-uber-etl-pipeline/assets/83917694/5e2344e7-69d4-4e11-aa89-d9ed23888e1f">


To open Mage UI: `External-IP-address:6789`


# Mage 🧙

## load_uber_data

Open Mage UI, select `Data loader -> Python -> API`

Copy URL of your csv file, add it to the Mage code: @data_loader url = ' '

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/9d125cee-63f2-4802-a7a4-b75f0be9bc63)

Run the block, load the data from GCS

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/e5e01438-4a74-4e12-b0bc-f037f6b42d74)

## uber_transformation

Transform the data: select `Transformer -> Python -> Generic (no template)`

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/39995cd8-b34d-42b7-b605-566e5b4efbbb)

Run uber_transformation block

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/76893241-b06a-4862-ae7a-1bdf22a85859)

## uber_bigquery_load

Select `Data exporter -> Python -> Google BigQuery`

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/c714ee7d-5de3-4e8a-b83e-40e12c1ebd0f)

Go to API & Services on GCP, create a new Service account from the Credentials section, assign the BigQuery Admin role to the SA. Create a new key in JSON format for this SA.
Copy and paste the information from your JSON key into the `io_config.yaml` file

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/27f5b21a-769c-47ea-b03a-89cb20cec313)


# BigQuery 🔍

Create a Dataset in BQ, run uber_bigquery_load block. Load all tables to BQ.

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/829677c2-d407-4726-830a-a1d86ceea3d6)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/91b1924e-412b-471d-ae28-4d8b0769cfee)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/b265a460-45fa-423a-b5ee-50cc6277a293)

Create a new table by running sql code from `sql_query.sql`


# Looker 📈

Open lookerstudio.google.com. Connect Looker to your BQ.

Create a dashboard. Here is an example of my [Looker Dashboard](https://lookerstudio.google.com/s/twWLPhtdgPI).
