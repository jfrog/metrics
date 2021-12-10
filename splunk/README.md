# Jfrog Metrics Integration in Splunk

## Table of Contents
1. [Splunk Setup](#splunk-setup)
   * [Pre requisite setup](#pre-requisite-setup)
2. [Environment Configuration](#environment-configuration)
3. [Fluentd Installation](#fluentd-installation)
4. [Fluentd Configuration for Splunk](#fluentd-configuration-for-splunk)
    * [Configuration for Artifactory Metrics](#configuration-for-artifactory-metrics)
    * [Configuration for Xray Metrics](#configuration-for-xray-metrics)
5. [Artifactory and Xray Setup](#artifactory-and-xray-setup)
6. [References](#references)

## Splunk Setup

### Splunkbase App

#### Pre requisite setup

1. Steps [here](https://github.com/jfrog/log-analytics-splunk#splunk-setup)

### Configure Splunk for Metrics

Queries for rendering dashboards will need to use Index of Metrics type with a specific name, steps to configure are listed below.

Administrator will need to configure the Index of Metrics type to accept metrics data. Steps are below.

#### Create index jfrog_splunk_metics
````text
1. Open Splunk web console as adminstrator
2. Click on "Settings" in dropdown select "Indexes"
3. Click on "New Index"
4. Enter Index name as "jfrog_splunk_metrics"
5. Click on 'Metrics' for the 'Index Data Type'
5. Click "Save"
````

#### Configure new HEC token to receive Metrics

Our metrics integration uses the [Splunk HEC](https://dev.splunk.com/enterprise/docs/dataapps/httpeventcollector/) to send data to Splunk.

Administrator will need to configure the HEC to accept data (enabled) and also create a new token. Steps are below.

````text
1. Open Splunk web console as adminstrator
2. Click on "Settings" in dropdown select "Data inputs"
3. Click on "HTTP Event Collector"
4. Click on "New Token"
5. Enter a "Name" in the textbox
6. (Optional) Enter a "Description" in the textbox
7. Click on the green "Next" button
8. Select App Context of "JFrog Platform Log Analytics and Metrics" in the dropdown
9. Add "jfrog_splunk_metrics" index to store the JFrog platform metrics data into.
10. Click on the green "Review" button
11. If good, Click on the green "Done" button
12. Save the generated token value
````

## Environment Configuration

1. Refer the [Environment Configuration](https://github.com/jfrog/log-analytics-splunk#environment-configuration)

## Fluentd Installation

1. Click [here](https://github.com/jfrog/log-analytics-splunk#fluentd-installation) for fluentd installation steps

## Fluentd Configuration for Splunk

1. Refer the [documentation](https://github.com/jfrog/log-analytics-splunk#fluentd-configuration-for-splunk) for fluentd configuration, in addition below are the configurations needed,

### Configuration for Artifactory Metrics

````text
cd $JF_PRODUCT_DATA_INTERNAL
wget https://raw.githubusercontent.com/jfrog/log-analytics-splunk/master/fluent.conf.rt
````

Fill in the JPD_URL, USER, JFROG_API_KEY fields in the source directive of the downloaded `fluent.conf.rt` with the details given below

```text
<source>
  @type jfrog_metrics
  @id metrics_hec_rt
  tag jfrog.metrics.rt
  metric_prefix 'jfrog.artifactory'
  interval 60s
  jpd_url JPD_URL
  username USER
  apikey JFROG_API_KEY
</source>
```

Override the match directive(last section) of the downloaded `fluent.conf.rt` with the details given below

```
<match jfrog.**>
  @type splunk_hec
  data_type metric
  hec_host HEC_HOST
  hec_port HEC_PORT
  hec_token METRICS_TOKEN
  index METRICS_INDEX
  source ${tag}
  metric_name_key metric_name
  metric_value_key value
  # buffered output parameter
  flush_interval 120s
  # ssl parameter
  use_ssl false
  ca_file /path/to/ca.pem
</match>
```

### Configuration for Xray Metrics

````text
cd $JF_PRODUCT_DATA_INTERNAL
wget https://raw.githubusercontent.com/jfrog/log-analytics-splunk/master/fluent.conf.xray
````

Fill in the JPD_URL, USER, JFROG_API_KEY fields in the source directive of the downloaded `fluent.conf.xray` with the details given below

```text
<source>
  @type jfrog_metrics
  @id metrics_hec_rt
  tag jfrog.metrics.xr
  metric_prefix 'jfrog.xray'
  interval 60s
  jpd_url JPD_URL
  username USER
  apikey JFROG_API_KEY
</source>
```

Override the match directive(last section) of the downloaded `fluent.conf.xray` with the details given below

```
<match jfrog.**>
  @type splunk_hec
  data_type metric
  hec_host HEC_HOST
  hec_port HEC_PORT
  hec_token METRICS_TOKEN
  index METRICS_INDEX
  source ${tag}
  metric_name_key metric_name
  metric_value_key value
  # buffered output parameter
  flush_interval 120s
  # ssl parameter
  use_ssl false
  ca_file /path/to/ca.pem
</match>
```

## Artifactory and Xray Setup

1. [Enable Metrics for Artifactory and Xray](https://github.com/jfrog/metrics#setup)

## References

1. [FluentD Installation - OS/Virtual Machine](https://github.com/jfrog/log-analytics-splunk#os--virtual-machine)
2. [FluentD Installation - Docker](https://github.com/jfrog/log-analytics-splunk#docker)
3. [FluentD Installation - Kubernetes with Helm](https://github.com/jfrog/log-analytics-splunk#kubernetes-deployment-with-helm)
4. [FluentD Installation - Kubernetes without Helm](https://github.com/jfrog/log-analytics-splunk#kubernetes-deployment-without-helm)