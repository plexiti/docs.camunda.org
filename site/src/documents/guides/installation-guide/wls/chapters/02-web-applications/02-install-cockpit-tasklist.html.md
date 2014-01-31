---

title: 'Install camunda Cockpit and Tasklist'
category: 'Web Applications'

---


The web application archive that contains camunda Cockpit and Tasklist resides under `webapps/camunda-webapp-ee-wls-$PLATFORM_VERSION.war` in the WLS distribution archive.

In the following we explain how to install the WAR file using the WebLogic Server Administration Console:

1.  Open the WebLogic Server Administration Console.
2.  Navigate to the **Domain Structure / YOUR_DOMAIN / Deployments** page.
3.  Click the **Install** button. The first page of the Wizard opens.
4.  Using the File Browser, select the `camunda-webapp-ee-wls-$PLATFORM_VERSION.war` file from the distribution and upload it.
5.  Continue to the **Next** page.
6.  Select **Install this deployment as an application** and continue to the **Next** page.
7.  Click the **Finish** button to complete the installation.

After completing the wizard, the Cockpit and Tasklist should be accessible on the default context path **/camunda**.
In some situations, you also have to start the web application manually from the **Domain Structure / YOUR_DOMAIN / Deployments** page.

You can check whether everything went well by accessing Cockpit, Tasklist and Admin via `/camunda/app/cockpit`, `/camunda/app/tasklist` and `/camunda/app/admin` or under the context path you configured.