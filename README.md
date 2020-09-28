# Connecting to BigQuery from Jupyter/IPython notebook
I already had the Anaconda installation of JupyterLab on my Windows 7 computer. I wanted to connect to BigQuery API and fetch data from Jupyter notebook using SQL queries into a pandas dataframe. Here are the steps:

## STEP 1: Creating a service account on Google Cloud API  
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
Command line (PowerShell or command prompt) instructions for setting the environment variable  did not work for me. I had to set it manually as follows:  

Go to *Start > Computer >* right click on *Properties > Advanced system settings*  
Under *Advanced* tab, click *Environment Variables...* Under section *System variables*, click *New...*  

Variable name: GOOGLE_APPLICATION_CREDENTIALS  
Variable value: d:\Users\ved\Data Science\SQL-BigQuery-Jupyter-3d11b79a2e69.json  
[replace the path above with the location of where you saved the service accout key JSON file in step 1]  

Click OK. Close all remaining windows by clicking OK.
If your JupyterLab session is already open , then restart Anaconda Navigator and JupyterLab

Check if the environment variable was created successfully by running magic command %env in the Jupyter notebook  

**Note:** In your Jupyter notebook, if you see "DefaultCredentialsError: Could not automatically determine credentials.", it means that the GOOGLE_APPLICATION_CREDENTIALS environemnt variable was not created or set correctly.

## STEP 3: Installing the BigQuery Python client library  
For detailed instructions, please check:  
https://cloud.google.com/bigquery/docs/bigquery-storage-python-pandas  

Open Anaconda Prompt and install the following pacakges one-by-one:  
> conda install -c conda-forge google-cloud-bigquery  
> conda install -c conda-forge google-cloud-bigquery-storage  
> conda install -c conda-forge pandas  
> conda install -c conda-forge pyarrow  

**Note:** if you see “EnvironmentNotWritableError”, run the Anaconda prompt in Admin mode (right click and select Run as Administrator) and try running above commands again.  

Above commands will download and install/upgrade a bunch of packages. Would need to restart Anaconda Navigator if it was already open. You should be able to see “google-cloud-bigquery” and lot of other packages under Environments > base (root) in Anaconda Navigator.

## STEP 4: Accessing BigQuery data from Jupyter notebook 
Following is an example script for accessing stackoverflow dataset from bigquery-public-data

```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "stackoverflow" dataset
dataset_ref = client.dataset("stackoverflow", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)
```
```python
# Get a list of available tables 
tables = list(client.list_tables(dataset))

list_of_tables =[table.table_id for table in tables]
print(list_of_tables)
```
If the above runs and gives you the list of tables, then you are all set. Congratulations!!!
```
['badges', 'comments', 'post_history', 'post_links', 'posts_answers', 'posts_moderator_nomination', 'posts_orphaned_tag_wiki', 'posts_privilege_wiki', 'posts_questions', 'posts_tag_wiki', 'posts_tag_wiki_excerpt', 'posts_wiki_placeholder', 'stackoverflow_posts', 'tags', 'users', 'votes']
```  

In my case, I encountered errors and had to fix two dependency issues for the above code to work:
1. ImportError: cannot import name 'collections_abc' from 'six.moves' (unknown location)  
bigquery is not being imported propoerly due to the dependency issue with a package called six. This is a known issue:  
https://github.com/googleapis/google-cloud-python/issues/9965  
upgrading six to to the latest version (1.12.0 to 1.15.0) fixed this issue:  
  > conda install -c anaconda six  

2. AttributeError: 'ClientOptions' object has no attribute 'scopes'  
This is also a known issue https://github.com/googleapis/google-cloud-python/issues/10471  
downgrading google-cloud-core 1.4.0 to 1.3.0 fixed this issue:  
  > conda install -c conda-forge google-cloud-core=1.3.0  

## STEP 5: Running SQL queries for accessing BigQuery data  


## Notes
An alternative way of connecting to BigQuery is through pandas-gbq package, which is being led by the pandas community. For more details, check out:  
https://pandas-gbq.readthedocs.io/en/latest/  
https://cloud.google.com/bigquery/docs/pandas-gbq-migration
