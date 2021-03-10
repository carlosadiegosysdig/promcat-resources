# Apache
The [Apache HTTP Server Project](https://httpd.apache.org/) is an effort to develop and maintain an open-source HTTP server for modern operating systems including UNIX and Windows.

# Metrics
The Apache metrics are extracted in two different ways:
from the [apache exporter](https://github.com/Lusitaniae/apache_exporter)
and parsing the logs via [Grok exporter](https://hub.docker.com/r/palobo/grok_exporter).

## Metrics gathered with Apache exporter
- apache_accesses_total Current total apache accesses
- apache_connections Apache connection statuses
- apache_cpuload The current percentage CPU used by each worker and in total by all workers combined
- apache_duration_total Total duration of all registered requests
- apache_exporter_build_info A metric with a constant '1' value labeled by version, revision, branch, and goversion from which apache_exporter was built.
- apache_scoreboard Apache scoreboard statuses
- apache_sent_kilobytes_total Current total kbytes sent
- apache_up Could the apache server be reached
- apache_uptime_seconds_total Current uptime in seconds
- apache_version Apache server version
- apache_workers Apache worker statuses

## Metrics gathered with Apache exporter
- apache_http_response_codes_total
- apache_http_response_bytes_total
- apache_http_last_request_seconds

> More metrics can be customized through the Grok exporter.
The metrics configured here are based on this [blog post of Robust Percepction](https://www.robustperception.io/getting-metrics-from-apache-logs-using-the-grok-exporter).

# Number of time series generated
Each Apache instance generate ~25 time series.

For further information, consult [Apache documentation](https://httpd.apache.org/).

# Attributions
Configuration files and dashboards maintained by [Sysdig team](https://sysdig.com/).

Exporter [Apache_exporter](https://github.com/Lusitaniae/apache_exporter) MIT License.