# PySpark Example

## Environment setup

You can run `docker-compose up` and follow the prompt to open the Jupyter Notebook UI (looks like `http://127.0.0.1:8888/?token=<SOME_TOKEN>`).

The given `data/` directory mounts as a Docker volume at `~/data/` for easy access:

```python
import os
from pyspark.sql import SparkSession

spark = SparkSession.builder.master('local').getOrCreate()
df = spark.read.options(
    header='True',
    inferSchema='True',
    delimiter=',',
).csv(os.path.expanduser('~/data/DataSample.csv'))
```

## Common Problems

### 1. Cleanup

Find the sample dataset of request logs in `data/DataSample.csv`. We consider records with identical `geoinfo` and `timest` as suspicious. Please clean up the sample dataset by filtering out those questionable request records.

### 2. Label

Assign each *request* (from `data/DataSample.csv`) to the closest (i.e., minimum distance) *POI* (from `data/POIList.csv`).

Note: a *POI* is a geographical Point of Interest.
