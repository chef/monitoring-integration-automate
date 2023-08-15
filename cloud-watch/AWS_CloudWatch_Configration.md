# AWS CloudWatch centerlise monitoring at AutomateHA cluster

### Description
As an Automate HA customer, I need to have monitoring rules set for the available metrics on the AWS Cloudwatch console which can be used for tracking and getting notified for Automate HA infrastructure-related health.

### Sending Linux logs to AWS Cloudwatch
Amazon CloudWatch is a monitoring service for AWS cloud resources and the applications you run on AWS. You can use Amazon CloudWatch to collect and track metrics, collect and monitor log files, set alarms, and automatically react to changes in your AWS resources. You can use Amazon CloudWatch to gain system-wide visibility into resource utilization, application performance, and operational health.

**Configuration for sending OS logs to CloudWatch involves:**

* Create IAM Role with relevant permission and attach to Linux instance.
* Install the CloudWatch agent in the instance.
* Prepare the configuration file in the instance.
* Start the CloudWatch agent service in the instance.
* Monitor the logs using CloudWatch web console.

### Step 1: Configure your IAM role or user for CloudWatch Logs
The CloudWatch Logs agent supports IAM roles and users. If your instance already has an IAM role associated with it, make sure that you include the IAM policy below. If you don't already have an IAM role assigned to your instance, you can use your IAM credentials for the next steps or you can assign an IAM role to that instance.

**To configure your IAM role or user for CloudWatch Logs**

1. Open the IAM console at Amazon Console.
2. In the navigation pane, choose Roles.
3. Choose the role by selecting the role name (do not select the check box next to the name).
4. Choose Attach Policies, Create Policy.
5. Choose the JSON tab and type the following JSON policy document.

        {
        "Version": "2012-10-17",
        "Statement": [
            {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": [
                "*"
            ]
        }
        ]
        }

6. When you are finished, choose Review policy. The Policy Validator reports any syntax errors.
7. On the Review Policy page, type a Name and a Description (optional) for the policy that you are creating. Review the policy Summary to see the permissions that are granted by your policy. Then choose Create policy to save your work.
8. Close the browser tab or window, and return to the Add permissions page for your role. Choose Refresh, and then choose the new policy to attach it to your role.
9. Choose Attach Policy.

***Note:** *Creating the AWS Role is a one time process and this can be attached to any Linux instance you want to send the logs to CloudWatch. And we are doing it for all of the instances we discuss here.*

### Step 2: Install and configure CloudWatch Logs agent on an existing Amazon EC2 instance

1. Connect to your Amazon EC2 instance.
2. Install the awslogs package.

        sudo yum install -y awslogs

3. Edit the /etc/awslogs/awslogs.conf file to configure the logs to track. For more information about editing this file, see [CloudWatch Logs agent reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)

4. By default, the /etc/awslogs/awscli.conf points to the us-east-1 Region. To push your logs to a different Region, edit the awscli.conf file and specify that Region.

5. Start the awslogs service.

        sudo systemctl start awslogsd
        sudo systemctl enable awslogsd.service

6. You should see the newly created log group and log stream in the CloudWatch console after the agent has been running for a few moments.
