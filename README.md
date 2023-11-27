



## GCS 🪣

Create a new bucket, upload the csv file. Change the permissions of the bucket (permissions -> edit access control -> Fine-grained).
Then edit the access of the csv file:

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/66511ae3-9543-4d96-b6d9-22f8a6ac1483)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/38b135a9-1b16-4e15-b082-0aecc5942c30)


## Compute Engine 💽

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

## Mage 🧙

### load_uber_data

Open Mage UI, select Data loader -> Python -> API

Copy URL of your csv file, add it to the mage code: @data_loader url = ' '

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/9d125cee-63f2-4802-a7a4-b75f0be9bc63)

Run the block, load the data from GCS

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/e5e01438-4a74-4e12-b0bc-f037f6b42d74)

### uber_transformation

Transform the data: select Transformer -> Python -> Generic (no template)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/39995cd8-b34d-42b7-b605-566e5b4efbbb)

Run uber_transformation block

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/76893241-b06a-4862-ae7a-1bdf22a85859)

### uber_bigquery_load

Select Data exporter -> Python -> Google BigQuery

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/c714ee7d-5de3-4e8a-b83e-40e12c1ebd0f)

Go to API & Services on GCP, create a new Service account from Credentials, assign BigQuery Admin role. Create a new key in json format for this SA.
Copy/paste info from your json key to `io_config.yaml`

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/27f5b21a-769c-47ea-b03a-89cb20cec313)

## BigQuery ❔

Create a Dataset in BQ, run uber_bigquery_load block. You will load all tables to BQ.

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/829677c2-d407-4726-830a-a1d86ceea3d6)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/91b1924e-412b-471d-ae28-4d8b0769cfee)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/b265a460-45fa-423a-b5ee-50cc6277a293)

Create a new table by running sql code from `sql_query.sql`

## Looker 📈

Open lookerstudio.google.com. Connect Looker to your BQ.

Create a dashboard. Here is an example of my [Looker Dashboard](https://lookerstudio.google.com/s/twWLPhtdgPI).
