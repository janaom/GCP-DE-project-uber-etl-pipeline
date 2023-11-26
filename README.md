

Video can be found on the [mage.ai](https://docs.mage.ai/guides/community-examples)

You can create an Uber Data Model on lucid.app

## GCS

Create a new bucket, upload the csv file. Change the permissions of the bucket (permissions -> edit access control -> Fine-grained).
Then edit the access of the csv file:

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/66511ae3-9543-4d96-b6d9-22f8a6ac1483)

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/38b135a9-1b16-4e15-b082-0aecc5942c30)


# Compute Engine

Create a new instance, e.g. Machine type: e2-standard-4

SSH into your VM and run these commands to install Python and pip 

`sudo apt-get install update`

`sudo apt-get install python3-distutils`

`sudo apt-get install python3-apt`

`sudo apt-get install wget`

`wget https://bootstrap.pypa.io/get-pip.py`

`sudo python3 get-pip.py`

Run this to install pandas: `sudo pip3 install pandas`

Run this to install Google Cloud Library: `sudo pip3 install google-cloud`

`sudo pip3 install google-cloud-bigquery`


Run this to install [Mage](https://github.com/mage-ai/mage-ai#%EF%B8%8F-install) on your VM: `sudo pip3 install mage-ai`

To start a new Mage project: `mage start de-uber-project` (you will see: `Checking port 6789...`)

Create a new firewall rule for the port 6789

![image](https://github.com/janaom/GCP_DE_project_uber_etl_pipeline/assets/83917694/579a044b-dd87-4f45-8807-bec24a173fce)

To open Mage UI: External IP address:6789


