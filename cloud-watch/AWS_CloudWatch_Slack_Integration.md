# CloudWatch integration with Slack

There are several different approaches to sending CloudWatch Alarms to Slack. For example, Lambda Functions could be used to establish the connection between AWS and Slack. This whitepaper will use an AWS ChatBot to establish integration between AWS CloudWatch and Slack. The configuration will start with creating an AWS SNS Topic for the CloudWatch Alarms. Once complete, the AWS Chatbot will be made, and messages received in the SNS Topic will be sent to the chosen Slack Channel.

## AWS SNS Configuration

1. In the Services search bar, search and select **Simple Notification Service (SNS)**. On the SNS dashboard, select **Topics** and select **Create Topic**. This will route alerts to AWS Chatbot regarding the Chef Automate HA CloudWatch metrics.

1. Select **Standard** as the **SNS type**.

1. Enter a Topic name (you may want to name your topic after your PagerDuty service's name) and Display name, then select **Create topic**.

    ![SNS-Topic](images/sns-topic.png)

## AWS Chatbot Configuration

1. Navigate to Services and search for **Chatbot**.

1. In the Configure a chat client select **Slack** and select **Configure Client**.

    ![Chatbot](images/chatbot.png)

1. You will be redirected to Slack.

1. If you still need to authenticate to Slack, sign in.

1. Select the Slack Workspace to which you would like to send alerts in the top right corner.

    ![Chatbot-Slack](images/chatbot-slack.png)

1. Select Allow, and you will be redirected to the AWS Chatbot configuration.

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

1. Select any Automate HA alarms already created and select Actions – **Edit**.

1. Confirm the configured Conditions are correct and select **Next**.

1. For the Alarm state trigger, select **In alarm**.

1. Under Notification, choose Select an existing SNS topic.

1. Select the SNS topic created earlier in the Send a notification dropdown.

    ![CloudWatch](images/cloudwatch.png)

1. Select **Add Notification**.

1. In the new notification, select **OK** under the **Alarm state trigger**.

1. Under Notification, choose **Select an existing SNS topic**.

1. Select the PagerDuty SNS topic created earlier in the Send a notification dropdown.

    ![CloudWatch2](images/cloudwatch2.png)

1. Select **Next** and then **Update alarm**.

1. Amazon CloudWatch and Slack have been integrated. When your alarm threshold is met, an incident will be triggered within the Slack Channel.

1. The incident will automatically resolve once that alarm is back in an OK state.
