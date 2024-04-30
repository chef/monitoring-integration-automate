# Datadog Integration with Microsoft Teams for Notification and Alerts

Webhooks are a great way to integrate MS Teams with external apps, Datadog in our case. It is an HTTP POST request sent to a callback URL to provide messages and notifications, allowing connections to Web Services and MS Teams. With this, you can:

1. Respond to messages when mentioned (outgoing webhooks).

1. Send proactive messages to configured channels in MS teams (incoming webhooks and connectors).

Here, we are going to concentrate on **incoming webhooks and connectors**. You can forward all the notifications and alerts for Chef Automate in Datadog to Microsoft Teams.

## Step 1: Configure your MS Teams Connectors

1. Login to MS Teams

1. *Select Teams --> Select the Team --> Add channel*. Here, we named the channel "Datadog Alerts".

    ![Create Teams Channel](Images/Create-Teams-Channel.png)

1. Create *connector*.

    ![Create Connector](Images/Creating-Connectors.png)

    **Note:** In the newer version of Microsoft Teams, the connectors menu option has been moved to the location below. Kindly follow the same if you have a similar version of Microsoft Teams:

1. Go to *"manage channel" > Find the "Connectors" option*.

1. Select the *Edit* button from the Connector section.

    See the below screenshot for reference.

    ![MS Teams Manage Channel](Images/DataDog-Teams-ManageChannel.png)

    ![MS Teams Connector Edit Menu](Images/DataDog-Teams-ConnectorMenu.png)

1. Search for Datadog, select add, and then select Configure

    ![Configure Datadog](Images/Configure-Datadog-Teams.png)

1. Configure the webhook URL here; it's a self-explanatory step; once done, select save. You will need this URL later to integrate into the Datadog application.

    ![Configure the Webhook URL](Images/Datadog-webhook-url.png)

## Step2: Setting up a Datadog account in Microsoft Teams

1. On Microsoft Teams, select **Apps** in the vertical toolbar.

1. Search for "Datadog".

1. Select **Add** to install the Datadog App.

    ![Add Datadog App](Images/Add-Datadog-on-Teams.png)

1. Now go to your Datadog application, navigate to [Microsoft Teams Title](https://app.datadoghq.com/account/settings?_gl=1*2j0s4k*_gcl_au*MjYxMTI0MTE2LjE2ODYyMDIwNDQ.*_ga*MjEyMDkyMzE0Mi4xNjg2MjAyMDQ0*_ga_KN80RDFSQK*MTY4Njk0NDc1MS4xMi4xLjE2ODY5NDQ3NzguMzMuMC4w*_fplc*RVglMkIxRzJRUmRCWjB2em8lMkZ0Z0Q1U05FN1l0UVNHUDYwU2tsQ3VIeTVFZ2Naa1kzY3lhYnBhSWU5bzNPYWREWWlWS245VlJqdndKN1ZNZlR5bk1rRERhbExMMzByNCUyRlMlMkY2a0dJdHAyNWxrQnYwNHZHa1U2VkhnSUJrWjNpdkElM0QlM0Q.#integrations/microsoft-teams)

    Or, in the Datadog app, from the left-hand bar, select *Integrations --> Integrations*; search for Microsoft Teams, install, and then select *Configure*.

1. Select *Add Account*, which will redirect you to Microsoft Authentication. Follow the prompts and select *OK*.

    ![Adding MS Teams Account & Authorization](Images/Authorize-Integration-Datadog-MSteams.png)

    **Note:** It usually takes a few hours for MS teams to build the MS teams groups within Datadog for setting up notifications.

## Step 3: Configure Webhook in Datadog Application

1. Log in to https://app.datadoghq.com/ with your credentials.

1. Under the page, select *Integrations --> Integrations*.

    ![Create Integration](Images/Create-Integrations.png)

1. Search for Microsoft Teams and select *Configure*.

    ![Configure Teams](Images/Datadog-Integrations.png)

1. Add MS Teams channel with the webhook URL.

    ![Add Teams Channel](Images/Add-Teams-Channel.png)

**Note:** We will need this teams channel name, which sets up notifications and alerts to be sent to Microsoft Teams webhook in the next step.

## Step 4: Setting up notification settings for Microsoft Teams

1. Login to [https://app.datadoghq.com/](https://app.datadoghq.com/) with your credentials.

1. From the left-hand side, select *Service Management --> Incidents --> Settings --> Integrations*; make the following changes for Datadog to send notifications to Microsoft Teams.

    Here, under "Microsoft Teams" section, make the following appends:

1. Key in the following information from what you had created in your Microsoft Teams Account:

    * Tenant

    * Team

    * Channel

Once done, Select **Save**.

![Creating Notification Settings](Images/Creating-Notification-Settings.png)

## Step 5: Setting up Notifications/Alerts

It is time to set monitors for your Automate HA infrastructure that can be sent as notifications/alerts over Teams and Email. The monitoring section teaches you how to create new monitors. To set up context for this section, let's take a simple monitor created to notify 4XX errors on the OpenSearch Service.

In the Datadog app, go to Monitors --> New Monitor

1. Select the detection method (we have selected "Threshold Alert" here)

1. Define what metrics you need to monitor

1. Define your alert conditions

![Setting up Alert Conditions](Images/Setting-up-Alert.png)

### Notification Settings

To create notifications/alerts to be sent to the designated Teams webhook (with MS Teams Channel) you created in Step2.

In Step2, the webhook was associated with Microsoft Teams Channel **msteams-automateha-alerts**

To set up notifications, you can take a cue from "Use Message Template Variables" as highlighted below.

![Setting up Notification](Images/Setting-up-Notifications.png)

Once done, you can also test if the notifications reach their desired targets by selecting **Test Notifications**. After confirming that you received the alerts in your teams channel (as shown below), select **save** to make it perpetual.

![Setup Notifications](Images/Teams-Notification.png)

## References

You can use the following official references:

* [Integrate MS Teams in Datadog for Notifications](https://docs.datadoghq.com/integrations/microsoft_teams/#send-monitor-notifications-to-a-microsoft-teams-channel)

* [Configure Monitors in Datadog](https://docs.datadoghq.com/monitors/configuration/?tab=thresholdalert#notify-your-team)
