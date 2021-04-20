# Jfrog Metrics Integration in Datadog

## Installation

JFrog Metrics Integration uses OpenMetrics check which is included in the [Datadog Agent](https://app.datadoghq.com/account/settings#agent) package. So, a separate installation is not required

## Artifactory and Xray Setup

1. [Enable Metrics for Artifactory](https://github.com/jfrog/metrics#setup)
2. [Create admin access tokens for Artifactory and Xray](https://www.jfrog.com/confluence/display/JFROG/Access+Tokens#AccessTokens-GeneratingAdminTokens)

## Datadog Configuration
Follow the instructions below to configure this check for an Agent running on a host. For containerized environments, see the Containerized section.

These values override the configuration specified below
```text
ARTIFACTORY_HOST_NAME_OR_IP   -> IP address or DNS of Artifactory 
ARTIFACTORY_ADMIN_TOKEN       -> Admin token for Artifactory
XRAY_ADMIN_TOKEN              -> Admin token for Xray
```
### Host
To configure this check for an Agent running on a host:

1. Edit the openmetrics.d/conf.yaml file at the root of your [Agent's configuration directory](https://docs.datadoghq.com/agent/guide/agent-configuration-files/?tab=agentv6v7#agent-configuration-directory) to start collecting your Artifactory and Xray Metrics. See the [sample openmetrics.d/conf.yaml](https://github.com/DataDog/integrations-core/blob/master/openmetrics/datadog_checks/openmetrics/data/conf.yaml.example) for all available configuration options
    ```text
    instances:
      - prometheus_url: http://<ARTIFACTORY_HOST_NAME_OR_IP>:80/artifactory/api/v1/metrics
        scheme: http
        headers:
          Authorization: "Bearer <ARTIFACTORY_ADMIN_TOKEN>"
        static_configs:
          - targets: ["<ARTIFACTORY_HOST_NAME_OR_IP>:80"]
        namespace: jfrog.artifactory
        metrics:
          - sys_cpu_totaltime_seconds
          - jfrt*
          - app*
      - prometheus_url: http://<ARTIFACTORY_HOST_NAME_OR_IP>:80/xray/api/v1/metrics
        scheme: http
        headers:
          Authorization: "Bearer <XRAY_ADMIN_TOKEN>"
        namespace: jfrog.xray
        metrics:
          - app*
          - db*
          - go*
          - queue*
          - sys*
          - jfxr*
    ```
2. [Restart the Agent](https://docs.datadoghq.com/agent/guide/agent-commands/?tab=agentv6v7#restart-the-agent)

### Containerized
For containerized environments, see the [Autodiscovery Integration Templates](https://docs.datadoghq.com/agent/kubernetes/integrations/?tab=kubernetes) for guidance on applying the parameters specified above.

If you are using Agent v6.8+ follow the instructions below. See the dedicated Agent guide for [installing community integrations](https://docs.datadoghq.com/agent/guide/community-integrations-installation-with-docker-agent/?tab=agentabovev68) to install checks with the [Agent prior to version 6.8](https://docs.datadoghq.com/agent/guide/community-integrations-installation-with-docker-agent/?tab=agentpriorto68) or the [Docker Agent](https://docs.datadoghq.com/agent/guide/community-integrations-installation-with-docker-agent/?tab=docker)

1. [Download and launch the Datadog Agent](https://app.datadoghq.com/account/settings#agent)
2. Configure your integration like [any other packaged integration](https://docs.datadoghq.com/getting_started/integrations/)


### Validation
[Run the Agent's status subcommand](https://docs.datadoghq.com/agent/guide/agent-commands/?tab=agentv6v7#service-status) and look for OpenMetrics under the Checks section.
 
