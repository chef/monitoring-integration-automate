# PagerDuty Integration and Notification
Connect PagerDuty to Datadog to:

* Notify on-call responders based on alerts triggered from Datadog.
* See incidents and escalations.
* Get daily reminders as to who is on-call.

What you’ll need:

 * Admin or Standard role permissions for your Datadog account (unless your organization has created custom role)
 * Admin role in PagerDuty Account

## Data Dog and PagerDuty Integration

The connection between Data Dog and PagerDuty must be created first.

1. In Data Dog select the **Integrations** Tab from the left-hand navigation.

2. In the search field, type in **PagerDuty** and select the **Install** button.

![Install](Images/install.png)

3. Select the **Configuration** tab and then click **Alert with PagerDuty**.

4. On the PagerDuty authorization screen there are two options to integrate with Data Dog.

* Enter your PagerDuty account using the **email address** and **password** and click **Authorize Integration**.


   or
* Use Single Sign On by providing your PagerDuty **Subdomain** and click **Sign In Using Your Identity Provider**.

![Integration](Images/authorize.png)

4. Once authenticated, you may choose to integrate with a **PagerDuty Service** or **Global Event Routing**.

5. Click **Finish Integration**

## PagerDuty Configuration

1. Create PagerDuty Team

-  In PagerDuty, navigate to **People** and **Teams**
- Click **+ New Team**
- Provide a unique **Name** for the Team and optional **Tags**.
- click **Save**

![Team](Images/createteam.png)

2. Create Escalation Policy
- In PagerDuty, navigation to **People** and **Escalation Policies**.
- Click **+ New Escalation Policy**.
- Provide a unique **Name** for the Escalation Policy and optional **Description**.
- Select the **Team** that was created in the previous step.
- Provide optional **Tags**.
- Enter the required **Users* and/or **Schedules** to be notified.
- Click **Save**.

![Escalation](Images/createpolicy.png)

3. Create Service
- In PagerDuty, navigate to **Services**.
- Click **+ new Service**
- Provide a unique **Name** and optional **Description**
- Click **Next**.

![Service1](Images/service1.png)

- Click the **Select an Existing Escalation Policy** and choose the **Escalation Policy** created in the previous step.
- Click **Next**.

![Service2](Images/service2.png)

- Choose the required **Reduce Noise** option
- Click **Next**.

![Service3](Images/service3.png)

- Under Integration, select **Data Dog**.
- Click **Create Service**.

![Service4](Images/service4.png)

4. The Service will open up in the Integration tab.
- Copy the **Integration Key**.

![Integration](Images/integrationkey.png)


## Data Dog Configuration

1. In Data Dog select the **Integrations** Tab from the left-hand navigation.
- Search for **PagerDuty** in the search field.
- Click on the **CONFIGURE** button.

![Integration2](Images/ddintegration.png)

2. Click the **Set Up Manually** button

![Setup](Images/setup.png)

3. Enter the **Service Name** and the copied **Integration Key** obtained in the previous step.

![Setup2](Images/servicename.png)

4. Click **Save**.

## Test Integration

1. In Data Dog, navigate to **Monitors**.
2. Create a new or update an existing Monitor
3. In the **Notify your team** section, enter the Service by using the following syntax.

@pagerduty-< Service Name >

Example: @pagerduty-Chef-Demo-Service

![Monitor1](Images/monitor1.png)

4. Click **Test Notifications**.
5. Click **Run Test**.

![Monitor2](Images/monitor2.png)

6. The Alert will be sent to address configured in PagerDuty.
