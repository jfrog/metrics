# JFrog Metrics

This project integrates JFrog metrics data into various vendors like Splunk Infrastructure Monitoring, Datadog etc.

The goal of this project is to make it easy for JFrog customers to monitor the health and operation of JFrog Platform and its related services. Adopting the Metrics API enables JFrog to provide a richer dataset on Artifactory and Xray and make it easily consumable via predefined dashboards.

## Setup

7.7 and above versions of JFrog Artifactory support OpenMetrics. To enable those metrics, make the following configuration changes to the [Artifactory System YAML](https://www.jfrog.com/confluence/display/JFROG/Artifactory+System+YAML)

```text
artifactory:
    metrics:
        enabled: true
```

Once this configuration is done and the application is restarted, metrics will be available in [Open Metrics Format](https://prometheus.io/docs/instrumenting/exposition_formats/#text-based-format)

### Artifactory Metrics

The [Artifactory Metrics](https://www.jfrog.com/confluence/display/JFROG/Artifactory+REST+API#ArtifactoryRESTAPI-GettheOpenMetricsforArtifactory) REST API returns the following metrics

| Metric                                    | Description                                                        |
|-------------------------------------------|--------------------------------------------------------------------|
| sys_cpu_totaltime_seconds                 | Total CPU used by the artifactory process                          |
| app_disk_total_bytes                      | Total Disk used by the application (Home directory)                |
| app_disk_free_bytes                       | Total Disk free                                                    |
| jfrt_artifacts_gc_duration_seconds        | Time taken by a GC run                                             |
| jfrt_artifacts_gc_binaries_total          | Number of binaries removed by a GC run                             |
| jfrt_artifacts_gc_size_cleaned_bytes      | Space reclaimed by a GC run                                        |
| jfrt_artifacts_gc_current_size_bytes      | Space occupied by Binaries after a GC run (Only for FULL GC runs)  |
| jfrt_runtime_heap_freememory_bytes        | Available free memory for JVM                                      |
| jfrt_runtime_heap_maxmemory_bytes         | Maximum memory configured for JVM                                  |
| jfrt_runtime_heap_totalmemory_bytes       | Total memory configured for JVM memory                             |
| jfrt_runtime_heap_processors_total        | Total number of processors for JVM memory                          |
| jfrt_db_connections_active_total          | Total number of active total DB connections                        |
| jfrt_db_connections_idle_total            | Total number of idle DB connections                                |
| jfrt_db_connections_max_active_total      | Total number of maximum DB connections                             |
| jfrt_db_connections_min_idle_total        | Total number of min idle DB connections                            |
| jfrt_http_connections_available_total     | Total number of available outbound HTTP connections                |
| jfrt_http_connections_leased_total        | Total number of available leased HTTP connections                  |
| jfrt_http_connections_pending_total       | Total number of available pending HTTP connections                 |
| jfrt_http_connections_max_total           | Total number of maximum HTTP connections                           |


### Xray Metrics

The [Xray Metrics](https://www.jfrog.com/confluence/display/JFROG/Xray+REST+API#XrayRESTAPI-Metrics) REST API returns the following metrics

| Metric                                    | Description                                                                                          |
|-------------------------------------------|------------------------------------------------------------------------------------------------------|
| jfxr_db_sync_started_before_seconds       | Seconds that passed since the last Xray DB sync started running                                      |
| jfxr_data_artifacts_total                 | Total number of Xray scanned artifacts by package type (Note: Package type is a label package_type)  |
| jfxr_data_components_total                | Total number of Xray scanned components by package type (Note: Package type is a label package_type  |
| jfxr_performance_server_up_time_seconds   | Seconds that passed since Xray server has started on the particular node                             |

