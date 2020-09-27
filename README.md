# Conneting to BigQuery from Jupyter/IPython notebook
I already had the Anaconda installation of JupyterLab on my Windows 7 computer. I wanted to connect to BigQuery and fetch data using SQL queries into a pandas dataframe. Following are the steps which worked for me:

## STEP 1: Create a service account on Google Cloud API  
There is extensive documentation on Google Cloud website. I followed below instructions:
https://cloud.google.com/docs/authentication/getting-started  
- On Google Cloud Console, create or select a project.  
- Go to the Create service account key page.  
  https://console.cloud.google.com/apis/credentials/serviceaccountkey  
- From the Service account list, select New service account.  
  In the Service account name field, enter a name.  
  From the Role list, select Project > Owner.  
  Click Create. A JSON file that contains your key downloads to your computer. Save it in your project folder. We will need this file in step 2

## STEP 2: Setting the environment variable

Install google-cloud-bigquery by opening Anaconda prompt and try the following:
> conda install -c conda-forge google-cloud-bigquery

[Note: if you see “EnvironmentNotWritableError”, run the Anaconda prompt in Admin mode (right click and select Run as Administrator) and try the above command again.]

Above command will download and install a bunch of packages. Would need to restart Anaconda Navigator if it was already open. You should be able to see “google-cloud-bigquery” and lot of other packages under Environments > base (root) in Anaconda Navigator

Install google-cloud-storage
> conda install -c conda-forge google-cloud-storage

GOOGLE_APPLICATION_CREDENTIALS': 'd:\\Users\\ved\\Data Science\\My Project 65719-247605f2d810.json'

$env:GOOGLE_APPLICATION_CREDENTIALS="d:\Users\ved\Data Science\My Project 65719-247605f2d810.json"

set GOOGLE_APPLICATION_CREDENTIALS="d:\Users\ved\Data Science\My Project 65719-247605f2d810.json"

After creating the environment variable, need to restart the computer
Check if the environment variable is created, by running magic command %env in the IPython notebook

conda install -c conda-forge pandas-gbq

if bigquery is not being imported, i.e. from google.cloud import bigquery is giving ImportError such as:
ImportError: cannot import name 'collections_abc' from 'six.moves' (unknown location)
This is a known issue https://github.com/googleapis/google-cloud-python/issues/9965
upgrading six to to the latest version 1.12.0 to 1.15.0 fixed this issue
> conda install -c anaconda six

https://cloud.google.com/bigquery/docs/bigquery-storage-python-pandas#conda

AttributeError: 'ClientOptions' object has no attribute 'scopes'
This is known issue https://github.com/googleapis/google-cloud-python/issues/10471
downgrading google-cloud-core 1.4.0 to 1.3.0 fixed this issue
conda install -c conda-forge google-cloud-core=1.3.0
