# msteams Integration and Notification
Connect msteams to prometheus to:

* Notify on-call responders based on alerts triggered from prometheus.
* See incidents and escalations.
* Get daily reminders as to who is on-call.

What you’ll need:

 * Admin or Standard role permissions for your prometheus account (unless your organization has created custom role)
 * Admin role in msteams Account 

## prometheus and msteams Integration

The connection between prometheus and msteams must be created first.

1. In prometheus select the **Integrations** Tab from the left-hand navigation.

2. In the search field, type in **msteams** and select the **Install** button.

![Install](images/install.png)

3. Select the **Configuration** tab and then click **Alert with msteams**.

4. On the msteams authorization screen there are two options to integrate with prometheus.

* Enter your msteams account using the **email address** and **password** and click **Authorize Integration**.


   or
* Use Single Sign On by providing your msteams **Subdomain** and click **Sign In Using Your Identity Provider**.

![Integration](images/authorize.png)

4. Once authenticated, you may choose to integrate with a **msteams Service** or **Global Event Routing**.

5. Click **Finish Integration**

## msteams Configuration

1. Create msteams Team

-  In msteams, navigate to **People** and **Teams**
- Click **+ New Team**
- Provide a unique **Name** for the Team and optional **Tags**. 
- click **Save**

![Team](images/createteam.png)

2. Create Escalation Policy
- In msteams, navigation to **People** and **Escalation Policies**.
- Click **+ New Escalation Policy**.
- Provide a unique **Name** for the Escalation Policy and optional **Description**.
- Select the **Team** that was created in the previous step.
- Provide optional **Tags**.
- Enter the required **Users* and/or **Schedules** to be notified.
- Click **Save**.

![Escalation](images/createpolicy.png)

3. Create Service
- In msteams, navigate to **Services**.
- Click **+ new Service**
- Provide a unique **Name** and optional **Description**
- Click **Next**.

![Service1](images/service1.png)

- Click the **Select an Existing Escalation Policy** and choose the **Escalation Policy** created in the previous step.
- Click **Next**.

![Service2](images/service2.png)

- Choose the required **Reduce Noise** option
- Click **Next**.

![Service3](images/service3.png)

- Under Integration, select **prometheus**.
- Click **Create Service**.

![Service4](images/service4.png)

4. The Service will open up in the Integration tab.
- Copy the **Integration Key**.

![Integration](images/integrationkey.png)


## prometheus Configuration

1. In prometheus select the **Integrations** Tab from the left-hand navigation.
- Search for **msteams** in the search field.
- Click on the **CONFIGURE** button.

![Integration2](images/ddintegration.png)

2. Click the **Set Up Manually** button

![Setup](images/setup.png)

3. Enter the **Service Name** and the copied **Integration Key** obtained in the previous step.

![Setup2](images/servicename.png)

4. Click **Save**.

## Test Integration

1. In prometheus, navigate to **Monitors**.
2. Create a new or update an existing Monitor
3. In the **Notify your team** section, enter the Service by using the following syntax.

@msteams-< Service Name >

Example: @msteams-Chef-Demo-Service

![Monitor1](images/monitor1.png)

4. Click **Test Notifications**.
5. Click **Run Test**.

![Monitor2](images/monitor2.png)

6. The Alert will be sent to address configured in msteams.
