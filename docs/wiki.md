# Python


###  praca z BQ
#####  #1 
``` py
##to dziala domyslnie
%%bigquery df
SELECT * FROM tabela
```

#####  #2 
```py
#import pakietow
from google.cloud import bigquery
#inicjacja polaczenia
client = bigquery.Client()
#odpytanie o projekt
print("Client creating using default project: {}".format(client.project))
#zapytanie
query = '''SELECT * FROM `tabela`'''
#zapisanie danych do DataFrame
df = client.query(query).resoult().to_dataframe()
```

##### #3 zapytanie z parametrem #1

``` py
#definiowanie parametru
dae_ = '2024-03-01'
#definiowanie zapytania
query = f''' select * from dual where dt = "{date_}" '''
```

##### #4 zapytanie z parametrem #2
``` py
#definiowanie zapytania
query = ''' select * from dual where dt = @date_'''
#definiowanie parametrow
params = [
            bigquery.ScalarQueryParameter('date','STRING', date_)
          ]
job_config = bigquery.QueryjobConfig(query_parameter=parms)
df = client.query(query,job_config=job_config).resoult().to_dataframe()
```

###  cost estimator
```py
# cost estimator
job_config = bigquery.QueryJobConfig()
job_config.dry_run = True
job_config.use_query_cache = False
query_job = bigquery.Client().query(
  (query),
    location="EU",
    job_config=job_config
)
data_amt = query_job.total_bytes_processed
print("{} GB will be processed which is roughly {} USD cost".format(round(data_amt / 1024**3,2), round(data_amt / 1024**4 * 5,2)))
```

### zapisanie wynikow zapytania do tabeli na BQ
```py
from google.cloud import bigquery
import pandas

dataset_id = 'logistics'
dataset = client.get_dataset(dataset_id)

table_ref = dataset.table('ai_nb_test_table')
job_config = bigquery.QueryJobConfig(
    destination=table_ref
)

# Start the query, passing in the extra configuration.
query_job = client.query(query, location="EU", job_config=job_config)

query_job.result()  # Waits for the query to finish
print("Query results loaded to table {}".format(table_ref.path))
```