---
title: "Run a Scan"
chapter: true
weight: 1
---

#### Running a Scan 

For this task, you will integrate your JIRA bug tracker with the applicaiton and run a FortiDevSec scan locally using the official Docker image.

* First, nagivate to your app's dashboard in FortiDevSec, and click the JIRA plugin icon.

    ![fds-dash-click-plugin](/images/fds-dash-click-plugin.png)

* Click the **Add Jira Plugin** switch, and fill in the values as appropriate.

<dl>
  <dt>Jira Server</dt>
  <dd>Cloud</dd>
  <dt>URL</dt>
  <dd>This is the domain name of the URL where you may view your project. i.e. https://my-project.atlassian.net
  <dt>Email ID</dt>
  <dd>The email address associated with your JIRA account.</dd>
  <dt>API Key</dt>
  <dd>The API key you set up in chapter 1</dd>
</dl>

* Once you've entered in the information, click **Fetch Details**, and your project should appear next to **Jira Projects**.

    ![jira-plugin-modal](/images/jira-plugin-modal.png)

* Select it, and click **OK**. The modal will disappear, and you should now see the JIRA plugin is **Configured**.

    ![jira-plugin-configured](/images/jira-plugin-configured.png)

* Now, to run the SAST scan, the fdevsec.yaml file needs to be configured to tell FortiDevSec what kind of scan we want to run. If this is not specified, FortiDevSec will default to a SAST scan. We can also optionally specify the languages in the codebase that we want FortiDevSec to scan. If this parameter is left blank FortiDevSec will automatically detect the languages present. After adding the required details, the file should look like:

```sh
version: v1

id:
  org: <Org ID>
  app: <App ID>

scanners:
  - sast
  - secret
  - sca
  - container
```

* Ensure docker is running. If using Windows, the Docker Desktop may need to be started.

```sh
docker --version
> Docker version 20.10.21, build baeda1f
```

* Run the following command to start the scan:

```sh
docker run --rm --mount type=bind,source="$PWD",target=/scan registry.fortidevsec.forticloud.com/fdevsec_sast:latest
```

* You should see the following output once the scan is complete:

    ![sast-run-done-output](/images/sast-run-done-output.png)

* Now, open your browser and navigate to the FortiDevSec console and click on the application.

    ![fds-dashboard-check-scan-1](/images/fds-dashboard-check-scan-1.png)

* You'll be taken to the application dashboard, which displays scan details such as type, languages, the Git commit the scan was done on, as well as vulnerability information.

    ![sast-run-done-app-dash](/images/sast-run-done-app-dash.png)

* Click **VIEW ALL** under the VULNERABILITIES heading on the main tab.

    ![click-view-all](/images/click-view-all.png)

* After a second or two, the filters sidebar on the left will populate. This lets you quickly get a high level overview of the scope of vulnerabilities that FortiDevSec has found in the application.

    ![click-view-all-none-confirmed](/images/click-view-all-none-confirmed.png)

* Now let's head over to JIRA and observe how status updates sync back to FortiDevSec as updated. From the main project issue page, choose an issue, and click the Status dropdown, and click **IN PROGRESS**.

    ![jira-todo-dropdown](/images/jira-todo-dropdown.png)
    ![jira-in-progress](/images/jira-in-progress.png)

* Now, switch back over to FortiDevSec and from the application's vulnerability page, click **Sync**, and the syncing process will begin.

    ![click-sync-sast](/images/click-sync-sast.png)

* While the syncing process is happening, the button will appear as in this image:

    ![jira-syncing-sast](/images/jira-syncing-sast.png)

* Once that process is complete, we can see that the status of our issue in FortiDevSec has changed from **New** to **Confirmed**.

    ![filter-confirmed-sast](/images/filter-confirmed-sast.png)
    ![confirmed-sast-tab](/images/confirmed-sast-tab.png)

