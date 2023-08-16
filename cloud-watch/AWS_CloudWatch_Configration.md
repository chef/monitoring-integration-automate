# AWS CloudWatch centerlise monitoring at AutomateHA cluster

### Description
As an Automate HA customer, I need to have monitoring rules set for the available metrics on the AWS Cloudwatch console which can be used for tracking and getting notified for Automate HA infrastructure-related health.

### Sending centerlised AutomateHA instaces logs to AWS Cloudwatch
Amazon CloudWatch is a monitoring service for AWS cloud resources and the applications you run on AWS. You can use Amazon CloudWatch to collect and track metrics, collect and monitor log files, set alarms, and automatically react to changes in your AWS resources. You can use Amazon CloudWatch to gain system-wide visibility into resource utilization, application performance, and operational health.

**Configuration for sending centerlised logs to CloudWatch involves:**

* Install the CloudWatch agent in the instance.
* Prepare the configuration file in the instance.
* Start the CloudWatch agent service in the instance.
* Monitor the logs using CloudWatch web console.

### Install and configure CloudWatch Logs agent on an existing Amazon EC2 instance

1. Connect to your AutomateHA Bastion Host.
2. Navigate to respective instance.
3. Install the awslogs agent.

        wget https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py

        sudo python ./awslogs-agent-setup.py --region <aws-region>

***Note:** To send the logs data from instances to CloudWatch. There are two methods, one is creating seperate IAM role specific to cloudwatch and than attach the instances to same IAM roles and policies. And another is adding the IAM user keys at the time of aswlogs agent installation at the /etc/awslogs/awslogs.conf file. For this documetation we are choosing the second method. You can refer to IAM roles creation method at [reference link](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html)*

4. setup the /etc/awslogs/awslogs.conf file to configure the logs to track. For more information about editing this file, see [CloudWatch Logs agent reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html). To set this file, just follow the auto prompts comes at time of awslogs installation. see below screenshot for reference.
![awslogs_configration](images/awslogs_configure.png)

5. Start the awslogs service. (By default the starting of service is taken care by the installation script)

        sudo systemctl start awslogs
        sudo systemctl enable awslogs

6. You should see the newly created log group and log stream in the CloudWatch console after the agent has been running for a few moments.
7. Login to AWS console and open CloudWatch and navigate to Logs tab.
![CloudWatch_LogsGroup](images/CloudWatch_logsGroup.png)

8. Open the Log group to see log stream segregated by set name (For example instance name here).
   ![CloudWatch_LogStream](images/CloudWatch_logstream.png)
9. Open the respective instance log stream to see the centerlised AutomateHA service logs.
    ![ClodWatch_Logs](images/CloudWatch_Logs.png)

We can further create visualization and filter the logs as per requirements and set the appropriate alerts on the threshold logs.
