# Slack Integration and notification

**Description**

As an Automate HA customer, need to integrate Data Dog with a slack tool to ensure getting notified over the set option in case of any higher severity issue.

Connecting Slack to Datadog to help your team collaborate by:

* Sharing graphs in private or public Slack channels.
* Receiving alerts and notifications from Datadog within Slack.
* Muting triggering monitors and declaring incidents from Slack.

**What you’ll need:**

  Admin or Standard role permissions for your Datadog account (unless your organization has created custom roles)


1. In the Slack App:

Select your workspace and chose/create the slack channel for datadog alerts to be triggered in.

2. In Datadog UI:

* Navigate to Integration -> select slack

* Click on Configure -> connect slack account

   ![Connecting slack channel](Images/Slack_integration.png)

* Select your intended worksapce from drop down in the top right corner.

* Click on add channel -> type the name of the channel if it is not showing and then click on 'save'.

   ![Selecting slack workspace](Images/Slack_workspace.png)


3. In Slack:

* Under apps -> datadog > click on 'add this app to a channel'

* A new message should pop up in the channel saying 'datadog was added to this channel'.


4. Test the Integration:

* In datadog app, go to monitor -> edit -> add the name of the slack channel that is created for notification purpose below the notification message as shown below.

  ![Selecting slack channel](Images/Slack_channel_name_selection.png)

* Click on Test notification at the bottom of the screen.

* A sample notification should be triggered in the channel confirming the Datadog-slack integration is successful.
