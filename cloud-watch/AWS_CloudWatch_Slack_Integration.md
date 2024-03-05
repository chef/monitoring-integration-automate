# CloudWatch integration with Slack

There are several different approaches on sending CloudWatch Alarms to Slack. As an example, the use of Lambda Functions could be leveraged to make the connection between AWS and Slack. This whitepaper will leverage AWS ChatBot to establish integration between AWS CloudWatch and Slack. The configuration will start with creating an AWS SNS Topic for the CloudWatch Alarms.  Once complete the AWS Chatbot will be created which will send messages received in the SNS Topic to the chosen Slack Channel.

## AWS SNS Configuration

1. In the Services search bar, search and select **Simple Notification Service (SNS)**. On the SNS dashboard, select **Topics** and click **Create Topic**. This will be used to route alerts to AWS Chatbot regarding the Chef Automate HA CloudWatch metrics.

1. Select **Standard** as the **SNS type**.

1. Enter a Topic name (you may want to name your topic after your PagerDuty service's name) and Display name, then click **Create topic**.

    ![SNS-Topic](images/sns-topic.png)

## AWS Chatbot Configuration

1. Navigate to Services and search of **Chatbot**.

1. In the Configure a chat client select **Slack** and click **Configure Client**.

    ![Chatbot](images/chatbot.png)

1. You will be redirected to Slack.

1. If not already authenticated to Slack, sign in.

1. Select the Slack Workspace you would like to send alerts to in the top right corner.

    ![Chatbot-Slack](images/chatbot-slack.png)

1. Select Allow and you will be redirected back to AWS Chatbot configuration.

1. Select Configure new channel.

    ![Chatbot-channel](images/config-channel.png)

1. Provide a Configuration name.

    ![Chatbot-channel2](images/config-channel2.png)

1. Select the Slack Channel to send alerts to.

    ![Chatbot-channel3](images/config-channel3.png)

1. Under permissions, provide a **Role name** and keep the defaults for Policy.

    ![Chatbot-channel4](images/config-channel4.png)

1. Under Notifications – optional, select the region for the SNS Topic created earlier and select the SNS Topic.

    ![Chatbot-channel5](images/config-channel5.png)

1. Select **Configure**.

### AWS CloudWatch Configuration

1. Navigate to Services and search for **CloudWatch**. Navigate to **All Alarms**.

1. Select any of the Automate HA alarms already created and select Actions – **Edit**.

1. Confirm the configured Conditions are correct and click **Next**.

1. For Alarm state trigger, select **In alarm**.

1. Under Notification, chose Select an existing SNS topic.

1. In the Send a notification to dropdown, select the SNS topic created earlier.

    ![CloudWatch](images/cloudwatch.png)

1. Select **Add Notification**.

1. In the new notification, select **OK** under **Alarm state trigger**.

1. Under Notification, chose **Select an existing SNS topic**.

1. In the Send a notification to dropdown, select the PagerDuty SNS topic created earlier.

    ![CloudWatch2](images/cloudwatch2.png)

1. Select **Next** and then **Update alarm**.

1. The integration of Amazon CloudWatch with Slack is complete. Now when your alarm threshold is met, an incident will be triggered within the Slack Channel.

1. Once that alarm is back in an OK state, the incident will automatically resolve.
