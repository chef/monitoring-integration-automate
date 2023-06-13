# DataDog centralised logs management

In case we want to send the logs as well to data dog of all instances, this cionfiguration needs to be added to esnure all logs are getting sent to data dog.

+ Type of configuration required for Automate HA setup, in order to ensure that all the components of Automate HA are sending the relevant logs metrics to data dog

| Config file (Final DD agent location) | Details   |
| :--- | :-- |
|/etc/datadog-agent/conf.d/journald.d/syslog.yaml|**Journalctl logs** In case we want to send the logs as well to data dog, this cionfiguration needs to be added to esnure all journal ctl logs are getting sent to data dog. Reference location for actual values:  [journald.yaml](YML_Files/journald.yaml)|
|/etc/datadog-ag/centonf.d/systemd.d/conf.yaml|**System logs** Enable this config for system level logs. Reference location for actual values: [syslog.yaml](YML_Files/syslog.yaml)|
|/etc/datadog-agent/conf.d/systemd.d/conf.yaml|**Systemd logs** Enable this config for systemd logs. Reference location for actual values: [systemd.yaml](YML_Files/systemd.yaml)|

To check this log:
1. Click on "Logs" menu.
2. Select the appropriate use case or "Search" for all logs, from sub-menu.

   ![DataDog-menu](Images/DataDog-Log-menu.png)

3. Filter the logs with your instace tags or hostname, etc.

See below screenshot for reference.

![DataDog-menu](Images/DataDog-Logs.png)
