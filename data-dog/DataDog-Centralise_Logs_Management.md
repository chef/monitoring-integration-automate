# Datadog centralized logs management

## Prerequisites

- Follow this [documentation](data-dog/DataDog-Installation_and_Configration.md) to download, install, and configure the Datadog agent on the Automate HA infrastructure nodes.

- Ensure that the agent is running successfully on all the nodes.

## Log Management Configuration

If we want to send the logs to the Datadog of all instances, this configuration needs to be added to ensure all logs are sent to the Datadog server.

- The type of configuration required for Automate HA setup ensures that all the components of Automate HA are sending the relevant log metrics to the Datadog.

| Config file (Final DD agent location) | Details   |
| :--- | :-- |
|/etc/datadog-agent/conf.d/journald.d/conf.yaml|**journalctl logs** If we want to send the logs to the Datadog, this configuration needs to be added to ensure all journal ctl logs are sent to the Datadog. Reference location for actual values:  [journald.yaml](YML_Files/journald.yaml)|
|/etc/datadog-agent/conf.d/journald.d/syslog.yaml|**System logs** Enable this config for system level logs. Reference location for actual values: [syslog.yaml](YML_Files/syslog.yaml)|
|/etc/datadog-agent/conf.d/systemd.d/conf.yaml|**Systemd logs** Enable this config for systemd logs. Reference location for actual values: [systemd.yaml](YML_Files/systemd.yaml)|

- Once the above configuration is added, restart the Datadog agent by running the below commands on all the nodes:

```sh
sudo service datadog-agent restart`
```

## Verify Logs on the Datadog Console

To check this log:

1. Log in to your Datadog console

1. Select the `Logs` menu.

1. Select the appropriate use case or "Search" for all logs from the sub-menu.

   ![Datadog-menu](Images/DataDog-Log-menu.png)

1. Filter the logs with your instance tags or hostname, etc.

    See the screenshot below for the reference:

    ![Datadog-menu](Images/DataDog-Logs.png)

## Custom Centralized Logging with Datadog

To have custom centralized logging with Datadog; we need to add the particular service at the [Journald.yaml](YML_Files/journald.yaml) and only that specific service log will come.

For example, To get logs for the chef automate service -  "authn-service" includes the same unit at /etc/datadog-agent/conf.d/journald.d/conf.yaml file, same as below:

```sh
- type: journald
    path: /run/log/journal
    log_processing_rules:
    - type: include_at_match
    name: include_authnservice
    ## Regexp can be anything
    pattern: .*authn-service.*
```

![Authn_service_log](Images/Authn_service_log_live.png)

Another example: To get logs for chef-server service -  "automate-cs-nginx" include the same unit at /etc/datadog-agent/conf.d/journald.d/conf.yaml file, same as below:

```sh
- type: journald
    path: /run/log/journal
    log_processing_rules:
    - type: include_at_match
    name: include_automate-cs-nginx
    ## Regexp can be anything
    pattern: .*automate-cs-nginx.*
```

![Automate-cs-nginx-log](Images/automate-cs-nginx.png)

**Note:** You can include multiple service patterns in the above `.yaml` file.
