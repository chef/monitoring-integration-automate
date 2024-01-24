## Filebeat Installation and Configuration:

### Prerequisites

* Deployment and configuration of Logstash, Elasticsearch and Kibana.
* Chef Automate HA configured with access to Logstash over configure TCP Port (In this configuration TCP 5044)

Steps:

1. Log into each Chef Automate HA node

2. Run the following to download and extract Filebeat

```sh
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.8.2-linux-x86_64.tar.gz

tar xzvf filebeat-8.8.2-linux-x86_64.tar.gz
```

## Filebeat Configuration

1. Modify the **filebeat.yml** file.

```sh
sudo nano /etc/filebeat/filebeat.yml
```

2. Under the **Filebeat Inputs** ensure the following is set:

* type is set to journald
* id is set to everything
* enable is set to true.

![Filebeat-input-Conf](images/filebeat-inputs.png)

3. Under the **Logstash Outputs**, enter the Logstash host and port to which the logs should be sent.

 ![Filebeat-Logstash-Conf](images/filebeat-logstash.png)

4. Save and close **filebeat.yml**

5. Enable the Filebeat System module

```sh
sudo filebeat modules enable system
```

6. Modify the Filebeat module file

```sh
sudo nano /etc/filebeat/modules.d/system.yml
```

7. Enable and ensure the path to the log files are correct.

![Filebeat-System-Conf](images/filebeat-system.png)

8. Save and close **system.yml**

9. Setup Filebeat ingest pipelines by running the following command

```sh
sudo filebeat setup --pipelines --modules system
```

10. Start and enable Filebeat

```sh
sudo systemctl start filebeat
```

```sh
sudo systemctl enable filebeat
```

## Visualize Logs in Kibana

1. Open kibana on browser:

   * Click on **Explore on my own**.

     ![kibana-index-pattern](images/Kibana-Start.png)

     You will land on Kibana Home page.

     ![kibana-index-pattern](images/Kibana-Home.png)

   * Click on **Dicover** tab on left side.

   * ![kibana-index-pattern](images/Kibana-discover-firstime.png)

   * Click on **Create DataView**.

2. Create an **Index Pattern** in Kibana to view the Logs sent to Logstash.

![kibana-index-pattern](images/DataView%20setup.png)

![kibana-index-pattern](images/index-pattern.png)

3. In Kibana, navigate to **Analytics/ Discovery** and select the Index Pattern created in the prior step. The logs from the Chef Automate Servers will be displayed.

![kibana-dashboard](images/Kibana-discover-page.png)

![kibana-dashboard](images/kibana-dashboard.png)

* We can add multiple filters and customize the data coming.

We can further build the Automate Service based dashboards for quick looks.

## Custom filtering of Logs at filebeat

**Description:** In case we would like to send only some specific service level logs of Automate and Chef Infra server to our centralized logging server instead of all the logs, this is this section that can be referred.

1. Modify the **filebeat.yml** file.

```sh
sudo nano /etc/filebeat/filebeat.yml
```

2. Under the **Filebeat Inputs** add the following processor configuration:

```sh
 processors:
    - drop_event:
        when.not.regexp.message: '^<filtered service>'
```

As an example:

To send only authn-service logs to Logstash:

![kibana-filter-example](images/filebeat-authn.png)

To send only automate-dex logs to Logstash:

![kibana-filter-example2](images/filebeat-dex.png)

3. In Kibana, navigate to Discover and the filtered results will be visible:

![kibana-filter-example3](images/kibana-filter-authn.png)

4. Click into the logs to confirm the filter is working as expected.

authn-service
![kibana-filter-example3](images/kibana-filter-authn-detail.png)

automate-dex
![kibana-filter-example3](images/kibana-filter-dex-detail.png)
