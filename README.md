# Conneting to BigQuery from Jupyter IPython notebook
## STEP 1: 

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
