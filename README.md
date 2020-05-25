# Thanos
A collection of .yml files for running up Prometheus with Thanos
on Kubernetes with external storage in S3.

https://thanos.io/quick-tutorial.md/

This setup will give us high availability as well as allow us to store
an 'infinite' amount of data from _Prometheus_.

In this example, all pods run in namespace _monitoring_.

## Components
A short decription of the Thanos components in use in this example.

### Sidecar
Thanos component _sidecar_ runs on the same pod as an instance of Prometheus.
For long time storage, _sidecar_ uploads data to an _S3_ bucket (On service
_Wasabi_ in my configuration.

https://thanos.io/components/sidecar.md/


### Querier
Thanos component _querier_ relays _promQL_ queries to a number of _sidecar_
and _store_ instances. For visualizing data in something like _Grafana_, you must use
the _Querier_ service as data store.

https://thanos.io/components/query.md/


### Store
To be able to query Prometheus data on the external storage, we need to run pods
with Thanos _store_. _Querier_ will relay _promQL_ queries to the relevant _sidecar_
and _store_ instances.

https://thanos.io/components/store.md/


### Compactor
Thanos _compactor_ runs periodically to compact, and downsample the Prometheus data on S3.
Data is downsampled to 5 minutes after 40 hours and to 1h after 10 days.
Only a **SINGLE** instance of _compactor_ should be running. Otherwise data will easily
be corrupted when several instances compact the same data. _Compactor_ can be run as
a _cron job_.

https://thanos.io/components/compact.md/
