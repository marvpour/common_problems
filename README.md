# Common Problems

Spark Data Handing

### 1. Cleanup

Find the sample dataset of request logs in `data/DataSample.csv`. We consider records with identical `geoinfo` and `timest` as suspicious. Please clean up the sample dataset by filtering out those questionable request records.

### 2. Label

Assign each *request* (from `data/DataSample.csv`) to the closest (i.e., minimum distance) *POI* (from `data/POIList.csv`).

Note: a *POI* is a geographical Point of Interest.
