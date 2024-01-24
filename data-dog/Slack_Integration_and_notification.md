# Slack Integration and Notification

## Description

As an Automate HA customer, need to integrate Data Dog with a slack tool to ensure getting notified over the set option in case of any higher severity issue.

Connecting Slack to Datadog to help your team collaborate by:

* Sharing graphs in private or public Slack channels.

* Receiving alerts and notifications from Datadog within Slack.

* Muting triggering monitors and declaring incidents from Slack.

## What you’ll Need

Admin or Standard role permissions for your Datadog account (unless your organization has created custom roles)

1. In the Slack App select your workspace and chose/create the slack channel for datadog alerts to be triggered in.

1. In Datadog UI:

    * Navigate to Integration -> select slack

    * Select **Configure** -> connect slack account

        ![Connecting slack channel](Images/Slack_integration.png)

    * Select your intended workspace from drop down in the top right corner.

    * Select **Add Channel** -> type the name of the channel if it is not showing and then select **Save**.

        ![Selecting slack workspace](Images/Slack_workspace.png)

1. In Slack:

    * Under apps -> datadog > Select **add this app to a channel**

    * A new message should pop up in the channel saying **datadog was added to this channel**.

1. Test the Integration:

    * In datadog app, go to monitor -> edit -> add the name of the slack channel that is created for notification purpose below the notification message as shown below.

        ![Selecting slack channel](Images/Slack_channel_name_selection.png)

    * Select **Test Notification** at the bottom of the screen.

    * A sample notification should be triggered in the channel confirming the Datadog-slack integration is successful.
