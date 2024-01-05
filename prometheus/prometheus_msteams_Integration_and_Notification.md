# MS Teams Integration and Notification

The scope here is to connect MS Teams to Prometheus in order to:

* Notify on-call responders based on alerts triggered from prometheus.
* See incidents and escalations.
* Get daily reminders as to who is on-call.
   
## Configure AlertManager for MS Teams
Before performing the following steps, please ensure alertmanager is installed and configured to run as a service. Refer to the [alertmanager installation guide](./Prometheus_Monitor_configuration_and_alerting.md)

## Prometheus and MS Teams Integration

The following steps provides the guidance to prepare MS Teams receiver for the alert manager to send alerts. Refer to [prometheus-msteams documentation](https://github.com/prometheus-msteams/prometheus-msteams/releases) for more details.

### Configure MS Teams
* Create Team

* Create Team Channel
  
    Create a channel in Ms-teams where you want to send alerts. 

* Create Incoming Webhook

  - Click on connectors(found connectors in options of the channel), and then search for ‘incoming webhook’ connector.
  ![Incoming Webhook Connector](./images/msteam-1.png)

  - Create a webhook of this channel. Incoming webhook is used to send notification from external services to track the activities.
  ![Select Channel](./images/msteam-2.png)
  ![Assign Incoming Webhook Name](./images/msteam-3.png)
  - Click Create
  - Copy the Webhook url and click done.
    This webhook url will be used in the next step.

#### Install and Configure Docker image
```
apt install docker.io
```

* The prometheus-msteams proxy container is configured on prometheus server with port number 2000. Please ensure firewall is opened for this port.

* Updated TEAMS_INCOMING_WEBHOOK_URL with the url link generated above.

* Run the following command to start docker container.

```
docker run -d -p 2000:2000 \
    --name="promteams" \
    -e TEAMS_INCOMING_WEBHOOK_URL="https://progresssoftware.webhook.office.com/webhookb2/19e1c444-fa6a-4d9a-b339-c527940dbaac@db266a67-cbe0-4d26-ae1a-d0581fe03535/IncomingWebhook/43f87f7d1724404394e28b24c50deb51/890e3fec-12ba-4296-8e94-6018f68218961" \
    -e TEAMS_REQUEST_URI=alertmanager \
    quay.io/prometheusmsteams/prometheus-msteams
```

## Configure Alertmanager

* Add the following configuration in alartmanager.yml file. Refer to the [alertmanager.yml](./alertmanager.yml) file for full config.
```
vi /etc/alertmanager/alertmanager.yml
```

```
route:
  routes:
    - match:
        severity: L2
      receiver: prometheus-msteams
      group_by: ['...']

receivers:
  - name: 'prometheus-msteams'
    webhook_configs: # https://prometheus.io/docs/alerting/configuration/#webhook_config
    - send_resolved: true
      url: 'http://localhost:2000/alertmanager'
```

Restart Alertmanager and Prometheus server

```
systemctl restart alertmanager.service
systemctl restart prometheus.service
```

## MS Team Alert Example
The below screenshot shows an example of prometheus alerts in MSTeams.
![Alert Example](./images/msteam-5.png) 