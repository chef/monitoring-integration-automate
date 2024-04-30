# PagerDuty Integration and Notification

Connect PagerDuty to Datadog to:

1. Notify on-call responders based on alerts triggered from Datadog.

1. See incidents and escalations.

1. Get daily reminders as to who is on-call.

What you’ll need:

1. Admin or Standard role permissions for your Datadog account (unless your organization has created a custom role).

1. Admin role in PagerDuty Account.

## Datadog and PagerDuty Integration

The connection between Datadog and PagerDuty must be created first.

1. In Datadog, select the **Integrations** Tab from the left-hand navigation.

1. In the search field, type in **PagerDuty** and select the **Install** button.

      ![Install](Images/install.png)

1. Select the **Configuration** tab and then select **Alert with PagerDuty**.

1. On the PagerDuty authorization screen, there are two options for integrating with Datadog.

      * Enter your PagerDuty account using the **email address** and **password** and click **Authorize Integration**.

      or

      * Use Single Sign On by providing your PagerDuty **Subdomain** and selecting **Sign In Using Your Identity Provider**.

      ![Integration](Images/authorize.png)

1. Once authenticated, you may choose to integrate with a **PagerDuty Service** or **Global Event Routing**.

1. Select **Finish Integration**

## PagerDuty Configuration

1. Create PagerDuty Team

      * In PagerDuty, navigate to **People** and **Teams**.

      * Select **+ New Team**.

      * Provide a unique **Name** for the Team and optional **Tags**.

      * Select **Save**.

      ![Team](Images/createteam.png)

1. Create Escalation Policy

      * In PagerDuty, navigate to **People** and **Escalation Policies**.

      * Select **+ New Escalation Policy**.

      * Provide a unique **Name** for the Escalation Policy and an optional **Description**.

      * Select the **Team** that was created in the previous step.

      * Provide optional **Tags**.

      * Enter the required **Users* and/or **Schedules** to be notified.

      * Select **Save**.

      ![Escalation](Images/createpolicy.png)

1. Create Service

      * In PagerDuty, navigate to **Services**.

      * Select **+ new Service**

      * Provide a unique **Name** and optional **Description**

      * Select **Next**.

      ![Service1](Images/service1.png)

      * Select the **Select an Existing Escalation Policy** and choose the **Escalation Policy** created in the previous step.

      * Select **Next**.

      ![Service2](Images/service2.png)

      * Choose the required **Reduce Noise** option

      * Select **Next**.

      ![Service3](Images/service3.png)

      * Under Integration, select **Datadog**.

      * Select **Create Service**.

      ![Service4](Images/service4.png)

1. The Service will open up in the Integration tab:

      * Copy the **Integration Key**.

      ![Integration](Images/integrationkey.png)

## Datadog Configuration

1. In Datadog, select the **Integrations** Tab from the left-hand navigation:

      * Search for **PagerDuty** in the search field.

      * Select the **CONFIGURE** button.

      ![Integration2](Images/ddintegration.png)

1. Select the **Set Up Manually** button.

      ![Setup](Images/setup.png)

1. Enter the **Service Name** and the copied **Integration Key** obtained in the previous step.

      ![Setup2](Images/servicename.png)

1. Select **Save**.

## Test Integration

1. In Datadog, navigate to **Monitors**.

1. Create a new or update an existing Monitor.

1. In the **Notify Your Team** section, enter the Service by using the following syntax.

      ```sh
      @pagerduty-< Service Name >
      Example: @pagerduty-Chef-Demo-Service
      ```

      ![Monitor1](Images/monitor1.png)

1. Select **Test Notifications**.

1. Select **Run Test**.

      ![Monitor2](Images/monitor2.png)

1. The Alert will be sent to the address configured in PagerDuty.
