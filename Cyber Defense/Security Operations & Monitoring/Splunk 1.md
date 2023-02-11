#  Introduction

Typically when people think of a SIEM, they think of Splunk, and rightly so. Per the Splunk website, they boast that 91 of the Fortune 100 use Splunk. 

Splunk is not only used for security; it's used for data analysis, DevOps, etc. But before speaking more on Splunk, what is a SIEM exactly?

A **SIEM** (**Security Information and Event Management**) is a software solution that provides a central location to collect log  data from multiple sources within your environment. This data is  aggregated and normalized, which can then be queried by an analyst.

As stated by [Varonis](https://www.varonis.com/blog/what-is-siem/), there are 3 critical capabilities for a SIEM:

- Threat detection
- Investigation
- Time to respond

Some other SIEM features:

- Basic security monitoring
- Advanced threat detection
- Forensics & incident response
- Log collection
- Normalization
- Notifications and alerts
- Security incident detection
- Threat response workflow

This room is a general overview of Splunk and its core features. Having experience with Splunk will help your resume stick out from the rest. 

Splunk was named a "Leader" in [Gartner's](https://www.splunk.com/en_us/form/gartner-siem-magic-quadrant.html) 2020 Magic Quadrant for Security Information and Event Management.

Per Gartner, "*Thousands of organizations around the world use Splunk as their SIEM for security monitoring, advanced threat detection, incident investigation and  forensics, incident response, SOC automation and a wide range of  security analytics and operations use cases.*

Besides developing a good understanding of SPL (Search Process  Language), it would be a good idea to level up your regex-fu. This will  increase your ability to write complex search queries. Read more about regular expression in the Splunk documentation [here](https://docs.splunk.com/Documentation/Splunk/8.1.2/Knowledge/AboutSplunkregularexpressions). 

#  Navigating Splunk

When you access Splunk, you will see the default home screen identical to the screenshot below.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-home-screen.png)

Let's look at each section, or panel, that makes up the home screen. The top panel is the **Splunk Bar** (below image). 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-bar.png)

In the Splunk Bar, you can see system-level messages (**Messages**), configure the Splunk instance (**Settings**), review the progress of jobs (**Activity**), miscellaneous information such as tutorials (**Help**), and a search feature (**Find**). 

The ability to switch between installed Splunk apps instead of using the **Apps panel** can be achieved from the Splunk Bar, like in the image below.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-bar2.png)

Next is the **Apps Panel**. In this panel, you can see the apps installed for the Splunk instance. 

The default app for every Splunk installation is **Search & Reporting**. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-apps-panel.png)

The next section is **Explore Splunk**. This panel contains quick links to add data to the Splunk instance, add new Splunk apps, and access the Splunk documentation. 

![img](https://assets.tryhackme.com/additional/splunk-overview/explore-splunk.png)
The last section is the **Home Dashboard**. By default, no dashboards are displayed. You can choose from a range of dashboards readily available within your Splunk instance. You can  select a dashboard from the dropdown menu or by visiting the **dashboards listing page**.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-add-dashboard.gif)

You can also create dashboards and add them to the Home Dashboard. The  dashboards you create can be viewed isolated from the other dashboards  by clicking on the **Yours** tab.

Please review the Splunk documentation on Navigating Splunk [here](https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/NavigatingSplunk). 

# Splunk Apps

As mentioned in the previous task, **Search & Reporting** is a Splunk app installed by default with your Splunk instance. This app is also referred to as the Search app. If you click on the **Search & Reporting** app, you will be redirected to the **Search** app (see image below).

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-search.png)

The Search app is where you will enter your Splunk queries to search  through the data ingested by Splunk. More on Splunk queries later.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-app-navigation.png)

The above image is the navigation for the Search app. Each app will have  its own navigation menu. This menu is different from the menu/navigation within the Splunk bar, accessible throughout your entire Splunk  session. 

Let's draw our attention back to the Splunk Home page.  In the Apps panel, there is a cog icon. By clicking the cog, you will be redirected to the Manage Apps page. From this page, you can change  various settings (**properties**) for the installed apps. Let's look at the properties for the **Search & Reporting** app by clicking on `Edit properties`.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-app-properties.png)

You can change the app's display name, whether the app should check for  updates, and whether the app should be visible in the Apps panel or  not. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-app-properties2.png)

Tip: If you want to land into the Search app upon login automatically, you can do so by editing the `user-prefs.conf` file. 

- Windows: `C:\Program Files\Splunk\etc\apps\user-prefs\default\user-prefs.conf`
- Linux:` /opt/splunk/etc/apps/user-pref/default/user-prefs.conf`

**Before:**

![img](https://assets.tryhackme.com/additional/splunk-overview/user-prefs1.png)

**After:**

![img](https://assets.tryhackme.com/additional/splunk-overview/user-prefs2.png)

**Note**: The above paths' base location will be different if you changed your Splunk install location. 

**Tip**: Best practice is for any modifications to Splunk confs, you should  create a directory and place custom conf settings there. When Splunk is  upgraded the defaults are overwritten. For this room editing the  defaults is OK. 

In order for the user preferences changes to take effect, the **splunkd** service has to be restarted from a command-line prompt, using the following two commands: `net stop splunkd` and `net start splunkd`.

Lastly, you can install more Splunk apps to the Splunk instance to further  expand Splunk's capabilities. You can either click on `+ Find More Apps` in the Apps panel or `Splunk Apps` in the Explore Splunk panel. 

![img](https://assets.tryhackme.com/additional/splunk-overview/more-splunk-apps.png)

To install apps into the Splunk instance, you can either install directly from within Splunk or download it from [Splunkbase](https://splunkbase.splunk.com/) and manually upload it to add it to your Splunk instance. 

**Note:** You must have an account on [Splunk.com](https://www.splunk.com/) to download and install Splunk apps. 

If you wish to install the app manually, click the `Install app from file` button. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-install-app.png)

Just browse to the location of the app and upload it.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-install-app2.png)

You can also download the app (**tgz** file) from Splunkbase. You then unzip the file and place the entire directory into the Apps location for your Splunk instance. 

**Note**: If you performed the install steps from the Linux section within this room and manually copied an App to the Apps  location for your Splunk instance, you might need to change the file  ownership and group to `splunk` or else your Splunk instance might not restart properly. 

Back to Windows, if you wish to remove an app (or an add-on), you can do so via the command-line. 

Below is the command to perform this task on Windows.

```
C:\Program Files\Splunk\bin>splunk.exe remove app app-name -auth splunk-username:splunk-password
```

**Note**: The syntax is similar on Linux machines. 

If the command were successful, you would see the following output: `App 'app-name' removed`

Refer to the following Splunk documentation [here](https://docs.splunk.com/Documentation/Splunk/8.1.2/Admin/Managingappobjects) for more information about managing Splunk apps. 

# Adding Data

Splunk can ingest any data. As per the Splunk documentation, when  data is added to Splunk, the data is processed and transformed into a  series of individual events. 

The sources of the data can be event logs, website logs, firewall logs, etc. 

Data sources are grouped into categories. Below is a chart listing from the  Splunk documentation detailing each data source category.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-data-sources.png)

Please refer to the Splunk documentation [here](https://docs.splunk.com/Documentation/Splunk/8.1.2/Data/Getstartedwithgettingdatain#Use_apps_to_get_data_in) for more information regarding the specific data source you want to add Splunk. 

In this room, we're going to focus on **Sysmon Logs**.

When we click on the `Add Data` link (from the Splunk home screen), we're presented with the following screen. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-add-data.png)

Looking at the guides, if we click on **Operating System,** we should see `Windows event logs`. But the only option available is `Forward data to Splunk indexers`. This is not what we want.

Let's ignore the guides and look at the bottom options: **Upload, Monitor,** and **Forward**. 

**Note**: The above screenshot is what you'll see if you installed Splunk locally on your end. The Splunk instance in the attached room will only show **Upload**, **Monitor**, and **Forward**. (see below)

 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-add-data-2.png)

Since we want to look at Windows event logs and Sysmon logs from this host system, we want **Monitor**.

There are many options to pick from on the following screen. `Local Event Logs` is the one we want.

![img](https://assets.tryhackme.com/additional/splunk-overview/local-event-logs1.png)

Look at the list of `Available item(s)`. Do you see PowerShell logs listed? How about Sysmon? I didn't either.

Another way we can add data to the Splunk instance is from `Settings > Data Inputs`. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-data-inputs.gif)

# Splunk Queries                            

y now, you should have installed the Splunk app/add-on and added a data source to Splunk. 

Now is the fun part, querying the data that is now residing in Splunk. 

If you have completed the [Windows Event Log](https://tryhackme.com/room/windowseventlogs) and [Sysmon](https://tryhackme.com/room/sysmon) rooms, you can remember that you queried the various logs using either **Event Viewer**, the **command-line**, or **PowerShell** and used filtering techniques to narrow down the information we're looking for. 

Thankfully, with a SIEM (such as Splunk), we can create queries to find the data  we're looking for across various data sources in one tool. 

Enter an asterisk `*` in the Search bar and change the timeframe to search from `Last 24 hours` to `All time`. This will retrieve all the historical data within Splunk. 

Even though we haven't discussed **Filters** yet but essentially `Last 24 hours` and `All time` are filters. We're instructing Splunk to output all the events from the historical data within the last 24 hours from the point in time we  submit our query. 

 

Click on the magnifying glass to initiate the search. 

**Note**: The output you see *might* be different for you.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-search-results-new.png)

If you want to focus on a specific **source** or **sourcetype**, you can specify that within the Search bar. (see below image)

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-search-sources-new.png)

This information is also available if you click on **source** or **sourcetype** under **Selected Fields**. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sourcetype-new.png)

Let's look at **s****ource**.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-source-count-new.png)

From the above image, we see the names (**values**) of each source and the number of events (**count**), and the percentage value (**%**) of all the events for each source. 

In the above image, the top 10 values are visible.

Let's start our query with Sysmon as the **source**. The query will look like this:

```
source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
```

We'll use this one, instead of **WinEventLog:Microsoft-Windows-Sysmon/Operational**, since it has more events we can sift through. 

I'll select the first event that appeared for me for demonstration purposes. Expanding on the event, the details of the event are more readable. 

**Before**:

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-1.png)

**After**:

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-2.png)

Some of these fields are specific to Sysmon. Refer to the Sysmon [room](https://tryhackme.com/room/sysmon) if you are not familiar with Sysmon Event IDs.  

**Note**: The **fields** will be different depending on the source/sourcetype. 

Back to our query, we can adjust our query to show events with Event ID 12, RegistryEvent (Object create and delete).

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-3.png)

**Fields** are **case-sensitive**. If you attempt to query for EventID in all lowercase, no results will be returned.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-4.png)

You can also search by **keywords**. Using the same event from above, I'll adjust the query and manually enter 'GoogleUpdate.exe.' 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-5.png)

Unlike fields, **keywords** are **not case-sensitive**. 

Instead of manually keying in the keyword, the keyword can also be added by  clicking the value you would like to add to the existing query (**Add to search**) or start a new query (**New search**). 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-6.png)

In the above image, I clicked on 'GoogleUpdate.exe,' and the options appeared.

**Note**: If you click on the icon to the far right for each choice, it will open the query in a new window. 

In the example below, I selected to **Add to search**. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sysmon-7.png)

You can use multiple keywords in your query. Splunk will use an implicit AND operator between each keyword.

Example: `* GoogleUpdate.exe chrome_installer.exe`

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-search-binaries.png)**
**

**Note**: You can try this query in the THM Splunk instance.

The above query will search across all the events (according to the  timeframe specified) and return all the events with GoogleUpdate.exe **AND** chrome_installer.exe. 

A keyword doesn't have to be a 'word' necessarily, but it can be a phrase. 

To search for a phrase, you need to surround the phrase with quotes. See the example below.

Example: `* "failed password for sneezy"`

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-failed-password.png)

The above query will return any events that contain the exact phrase.

**Note**: You can try this query in the THM Splunk instance. (Make sure you imported **tutorialdata.zip** into the Splunk instance first)

Moving along. Let's go back to the Sysmon logs and look at GoogleUpdate.exe again. 

Draw your attention to the **Interesting Fields** sidebar. This information is useful and can help adjust your query and narrow down your search results even more. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-interesting-fields.png)

Let's look at **RuleName** and see what the 8 values are. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-rulename-9.png)

We can further expand on our query with one of these values. 

**Note**: If you click on any of the Interesting Fields sidebar values, it will be automatically added to the existing query. 

Another thing to note regarding Interesting Fields. Let's say we would like to see the **RuleName** appear for each event, just like the **host**, **source**, and **sourcetype** fields (the default fields for every event). 

You can change the value of **Selected** from **No** to **Yes**. This is visible in the above image. The value in the image is set to No. 

Let's change the value of **Selected** to **Yes** for **RuleName**. 

Before:

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-before-rulename.png)

After:

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-after-rulename.png)

The **Selected Fields** sidebar reflects the change. 



![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-rulename-selected.png)

Refer to the following Splunk documentation for more information on searching in Splunk.

- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Aboutthesearchapp
- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Startsearching
- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Aboutthetimerangepicker
- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Usefieldstosearch
- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Usefieldlookups
- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Searchwithfieldlookups
- https://docs.splunk.com/Documentation/Splunk/8.1.2/Knowledge/AboutSplunkregularexpressions

**Note**: Some of the information in the above links will overlap each other. 

The [Splunk Quick Reference Guide](https://www.splunk.com/pdfs/solution-guides/splunk-quick-reference-guide.pdf) has more tips on searching and filtering in Splunk, along with other tips.

# Sigma Rules

lorian Roth created **Sigma**. 

What is Sigma?

 As per the GitHub [repo](https://github.com/Neo23x0/sigma), "*Sigma is a generic and open signature format that allows you to describe  relevant log events in a straightforward manner. The rule format is very flexible, easy to write and applicable to any type of log file. The  main purpose of this project is to provide a structured form in which  researchers or analysts can describe their once developed detection  methods and make them shareable with others.*"

Each SIEM has its own structure/format for creating queries. It isn't easy  to share SIEM queries with other Security Teams if they don't use your  exact SIEM product. For example, you can have a repo of Splunk queries  that your team utilizes for threat exposure checks or threat hunting.  These queries (or rules) can be created in the Sigma format and shared  with teams that don't use Splunk. Sigma rules can be shared along with IOCs and YARA rules as Threat Intelligence. 

Some supported target SIEMs:

- [Splunk](https://www.splunk.com/)
- [Microsoft Defender Advanced Threat Protection](https://www.microsoft.com/en-us/microsoft-365/windows/microsoft-defender-atp)
- [Azure Sentinel](https://azure.microsoft.com/en-us/services/azure-sentinel/)
- [ArcSight](https://software.microfocus.com/en-us/products/siem-security-information-event-management/overview)
- [QRadar](https://www.ibm.com/products/qradar-siem)

Some projects/products that use Sigma:

- [MISP](https://www.misp-project.org/index.html)
- [THOR](https://www.nextron-systems.com/thor/)
- [Joe Sandbox](https://www.joesecurity.org/)

There also is a Splunk app titled [TA-Sigma-Searches](https://github.com/dstaulcu/TA-Sigma-Searches). 

Sigma rules are written in **YAML** (YAML Ain't Markup Language). 

As per the [website](https://yaml.org/), "*YAML is a human friendly data serialization standard for all programming languages*." 

The Sigma repo has signatures in the **rules** folder. **Sigmac**, the Sigma Converter, located in the **tools** folder, can generate a specific SIEM rule. 

Example:` ./sigmac -t splunk -c tools/config/generic/sysmon.yml ./rules/windows/process_creation/win_susp_whoami.yml`

Please refer to the Github [repo](https://github.com/Neo23x0/sigma) for more information, examples, install instructions, rules, etc. 

An online version of this tool created by SOC PRIME ([Florian Roth](https://socprime.com/leadership/florian-roth/)) does the conversion work for you. The tool is [Uncoder.io](https://uncoder.io/). 

This online tool is not only for Sigma -> SIEM conversion. It also allows for other conversions. I'll leave you to explore that.

Let's explore this online tool a bit.

Near the top, there is a drop-down box. This drop-down will feature Sigma rules we can convert to a Splunk query. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sigma-1.png)

Choose some Sigma rules and convert them to Elasticsearch, QRadar, Splunk, etc. 

The Sigma rule for '**User Added to Local Administrators**' is converted to a Splunk query in the example below.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-sigma-example2.gif)

The best way to get familiar and comfortable with Sigma and YAML files is  to inspect that repo and look at Sigma rules and create some of your  own. 

#  Dashboards & Visualizations                            

Dashboards are panels displaying different data (visualizations) in one view. 

Visualizations allow us to view data in a visual format, such as a chart (bar or pie, for instance) or as a single value.

Typically SOCs create a variety of dashboards, and these dashboards are displayed on large screens. Different dashboards can be created, but the  dashboards' overall objective is to provide a high-level overview of the environment. 

Circling back to the [Windows Event Log](https://tryhackme.com/room/windowseventlogs) room, it was briefly mentioned that in Event Viewer, we could use`Create Custom View` . A custom view is a filter to focus on specific data within the log. This concept is similar to a dashboard in a SIEM. 

**Note**: Dashboards are specific to Apps. If you create a dashboard for the  Search app, then the dashboard uses this particular app's context.

Follow these steps to create a dashboard in the Search app (and enable dark mode).

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-dashboard-example.gif)

We're tasked to display the top 5 Sysmon Event IDs on the dashboard for the SOC team.

First, we'll create the search query, pipe to a transform command to filter the top 5 Event IDs, and examine the results. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-query-dashboard.png)

After we confirm the query and results, we can look at **Visualizations**.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-dashboard-visuals.png)

Please refer to the Splunk documentation on Dashboards and Visualizations to understand the difference between each option [here](https://docs.splunk.com/Documentation/Splunk/8.1.2/Viz/Visualizationreference). 

After the visualization is selected, save it as a **Dashboard panel**. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-dashboard-saveas.png)

If the dashboard is already created, we can select Existing for Dashboard and select it. 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-dashboard-saveas-2.png)

After successfully saving the Dashboard Panel, you can view the dashboard.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-dashboard-new-2.png)

If you wish to add the dashboard to your home page, you can click on the ellipsis and select the option.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-dashboard-ellipsis-2.png)

Result:

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-home-dashboard.png)

Note: 

Refer to the Splunk documentation on dashboards and visualizations:

- https://docs.splunk.com/Documentation/Splunk/8.1.2/Viz/WebFramework
- https://docs.splunk.com/Documentation/Splunk/8.1.2/Viz/Aboutthismanual
- https://docs.splunk.com/Documentation/Splunk/8.1.2/Viz/CreateDashboards
- https://docs.splunk.com/Documentation/Splunk/8.1.2/Viz/AddPanels
- https://docs.splunk.com/Documentation/Splunk/8.1.2/SearchTutorial/Createnewdashboard

#  Alerts                            

**Alerts** is a feature in Splunk that enables us to monitor and respond to specific events. 

Alerts use a saved search to monitor events in real-time or on a schedule. Alerts will  trigger when a specific condition is met to take the defined course of  action. 

Let's look at '[The alerting workflow](https://docs.splunk.com/Documentation/SplunkCloud/8.1.2012/Alert/AlertWorkflowOverview)' from the Splunk documentation. 

**Search: What do you want to track?** 

- There is an external IP brute-forcing a web page. We want to be alerted  whenever this IP address is actively attacking the infrastructure. 

**Alert Type: How often do you want to check for events?**

- Since we want to be alerted whenever this IP is active, a real-time alert is what we'll configure. 

**Alert trigger conditions and throttling: How often do you want to trigger an alert?**

- If 10 failed password events under 1 minute, generate an alert. 

 **Alert action: What happens when the alert triggers?**

- Send an email or send a message in an application using a [webhook](https://docs.splunk.com/Documentation/Splunk/8.1.2/Alert/Webhooks). 

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-alert-1.png)

In **Splunk Free**, we can **not** create alerts. You can experiment with this feature in the [60-day trial](https://www.splunk.com/en_us/download/splunk-enterprise.html?utm_campaign=google_amer_en_search_brand&utm_source=google&utm_medium=cpc&utm_content=Splunk_Enterprise_Demo&utm_term=splunk enterprise&_bk=splunk enterprise&_bt=439715964910&_bm=e&_bn=g&_bg=43997960527&device=c&gclid=EAIaIQobChMIiYumpZ-V8AIViNrICh2arQ01EAAYASAAEgJw9PD_BwE) of Splunk Enterprise. 

Please reference the Splunk documentation on Alerting [here](https://docs.splunk.com/Documentation/Splunk/latest/Alert/Aboutalerts) to fully understand the different ways to configure them.

[Here](https://docs.splunk.com/Documentation/SplunkCloud/8.1.2012/Alert/Alertexamples) is the direct link to Alert examples.

# *splunk2*

The first objective is to find out what competitor website she visited. What is a good starting point?

When it comes to HTTP traffic, the source and destination IP addresses should be recorded in logs. You need Amber's IP address.

You can start with the following command, `index="botsv2" amber`, and see what events are returned. Look at the events on the first page. 

Amber's IP address is visible in the events related to PAN traffic, but it's not straightforward. 

To get her IP address, we can hone in on the PAN traffic source type specifically. 

**Command**: `index="botsv2" sourcetype="pan:traffic"`

From here, you should have Amber's IP address. You can build a new search query using this information.

It would be best if you used the **HTTP stream** source type in your new search query. 

Using Amber's IP address, construct the following search query. 

**Command**: `index="botsv2" ***IPADDR\*** sourcetype="stream:HTTP"`

You must substitute **IPADDR** with **Amber's IP address**.

After this query executes, there are many events to sift through for the answer. How else can you narrow this down?

Look at the additional fields.

Another field you can add to the search query to further shrink the returned events list is the **site** field. 

Think about it; you're investigating what competitor website Amber visited.

Expand the search query only to return the site field. Additionally, you can remove duplicate entries and display the results nicely in a table. 

**Command**: `index="botsv2" ***IPADDR\*** sourcetype="stream:HTTP" | ***KEYWORD\*** site | ***KEYWORD\*** site`

You must substitute **KEYWORD** with the Splunk commands to remove the duplicate entries and display the output in a table format. 

**Note**: The first **KEYWORD** is to remove the duplicate entries, and the second is to display the output in a table format. 

The results returned to show the URIs that Amber visited, but which website is the one that you're looking for? 

To help answer these questions: Who does Amber work for, and what industry are they in? 

The competitor is in the same industry. The competitor website now should clearly be visible in the table output.

**Extra**: You can also use the industry as a search phrase to narrow down the results to a handful of events (1 result to be exact). 

**Command**: `index="botsv2" ***IPADDR\*** sourcetype="stream:HTTP" ***\*INDUSTRY\**** | ***KEYWORD\*** site | ***KEYWORD\*** site`

**Note**: Use asterisks to surround the search term.

**Questions 2-7**

Amber found the executive contact information and sent him an email. Based on question 2, you know it's an image. 

Since you now know the competitor website, you can construct a more specific  search query isolating the results to focus on Amber's HTTP traffic to the competitor website. 

**Command**: `index="botsv2" ***IPADDR\*** sourcetype="stream:HTTP" ***COMPETITOR_WEBSITE\***`

Replace **COMPETITOR_WEBSITE** with the actual URI of the competitor website. 

You can expand on the search query to output the specific field you want in a table format for an easy-to-read format, as we did for the previous  objective. 

Based on the image, you have the CEO's last name but not his first name. Maybe you can get the name in the email communication.

You can now draw your attention to email traffic, SMTP, but you need Amber's email address. You should be able to get this on your own. :)

Once you have Amber's email address, you can build a search query to focus  on her email address and the competitor's website to find events that  show email communication between Amber and the competitor. 

**Command**: `index="botsv2" sourcetype="stream:smtp" ***AMBERS_EMAIL\*** ***COMPETITOR_WEBSITE\***`

Replace **AMBERS_EMAIL** with her actual email address. 

With the returned results from the above search query, you can answer your own remaining questions. :)

In this task, we'll attempt to tackle the 200 series questions from the BOTSv2 dataset. 

**Note**: As noted in the previous task, this guide is not the only way to query Splunk for the answers to the questions below. 

**Question 1**

Our first task is to identify the version of Tor that Amber installed. You can use a keyword search to get you started. 

What are some good keywords? Definitely **Amber**. Another would be **Tor**. Give that a go.

**Command**: `index="botsv2" amber tor`

Over 300 results are returned. You can reverse the order of results (hoping  the 1st event is the TOR installation) and see if you can get the  answer.

You should add another keyword to this search query. I'll leave that task to you.

**Command**: `index="botsv2" amber tor ***KEYWORD\***`

Replace the **KEYWORD** with another search term to help narrow down the events to the answer. 

**Questions 2 & 3**

You need to determine the public IP address for brewertalk.com and the IP address performing a web vulnerability scan against it. 

You should be able to tackle this one on your own. Use the previous search queries as your guide. 

**Questions 4 & 5**

Now that you have the attacker IP address, build your new search query with the attacker IP as the **source IP**.

**Command**: `index="botsv2" src_ip="***ATTACKER_IP\***"`

**Tip**: Change the **Sampling** to **1:100** or your query will auto-cancel and throw errors. 

Yikes! The number of events returned is over 18,000 .. but that is fine. 

Use the **Interesting Fields** to help you identify what the URI path that is being attacked is.

Once the URI path has been identified, you can use it to expand the search  query further to determine what SQL function is being abused. 

**Command**: `index="botsv2" src_ip="***ATTACKER_IP\***" uri_path="***URI_PATH\***"`

You should have over 600 events to sift through but fret not; the answer is there. 

**Questions 6 & 7**

Awesome, thus far, you have identified Amber downloaded Tor Browser (you even  know the exact version). You identified what URI path and the SQL  function attacked on brewertalk.com. 

Your task now is to identify the cookie value that was transmitted as part of an XSS attack. The  user has been identified as Kevin. 

Before diving right in, get some details on Kevin. This is the first time you hear of him.

**Command**: `index="botsv2" kevin`

Ok, now you have Kevin's first and last name. Time to figure out the cookie value from the XSS attack.

As before, you can start with a simple keyword search. 

You know that you're looking for events related to Kevin's HTTP traffic with an XSS payload, and you're focused on the cookie value. 

Honestly, you should be able to tackle this one on your own as well. Use the previous search queries as your guide. 

After you executed the search query that yields the events with the answer,  you can identify the username used for the spear phishing attack. 

Based on the question hint, you can perform a keyword search query here as well.

**Command**: `index="botsv2" ***KEYWORD\***`

As times before, replace **KEYWORD** with the actual keyword search term.

Great! You should have been able to find all the answers to the questions using basic keyword searches.

Upward and onwards! Time to tackle some of the 300 series questions.

As with the 100 series questions, there are extra questions in this task that are not from the BOTS2 dataset. 

**Questions 1 & 2**

The questions start with an individual named Mallory, her MacBook, and some encrypted files. 

As per the previous tasks, you can start with a keyword search to see what events are returned that are associated with Mallory.

**Command**: `index="botsv2" mallory`

Over 11,000 events are returned, but if you draw your attention to the **Selected Fields,** you should get the name of her MacBook.

Ok, build a new search query with just the name of her MacBook and see what you get.

**Command**: `index="botsv2" host="***NAME_MACBOOK***"`

**Note**: You don't have to run this command. Trust me; the results returned are well over 9 million events.

Looking back at the question (our objective), the focus is on a critical PowerPoint presentation. 

Add common file extensions for PowerPoint to help significantly shrink the amount of returned events. 

**Command**: `index="botsv2" host="***NAME_MACBOOK***" (*.ppt OR *.pptx)`

Nice! You should have the filename of the critical document. 

Now you need to find another file, this time a movie file. 

Use the same source type from the previous query that returned the event with the filename of the critical PowerPoint document.

Since you don't know the file extension, you can't use the same approach as before. 

What do you know? You know the file extension of the files once they have been encrypted. 

You can use that file extension in your search query.

**Command**: `index="botsv2" host="***NAME_MACBOOK\***" sourcetype="***?\***" ***\*.EXT\***`

Replace the **?** with the name of the source type and .**EXT** with the actual encryption file extension. 

After execution, you should see the results are over 1,000, but the answer should be on the first page of the results. 

**Questions 3-7**

Next task, you need to provide the name of the manufacturer of the USB drive Kevin used on Mallory's personal MacBook (kutekitten). 

You can search for the malware or search for the USB manufacturer (vendor). In either case, you need to start with the MacBook.

You know the drill, start with a simple keyword search using the name of the MacBook.

**Command**: `index="botsv2" kutekitten`

The number of returned events is over 6,000, and the 2 data sources/source types are related to [Osquery](https://osquery.io/). 

What is Osquery? 

"*Osquery exposes an operating system as a high-performance relational database.  This allows you to write SQL queries to explore operating system data.  With Osquery, SQL tables represent abstract concepts such as running  processes, loaded kernel modules, open network connections, browser  plugins, hardware events or file hashes.*"

**Tip**: Visit the Osquery [room](https://tryhackme.com/room/osqueryf8) to learn more. 

Look through some of the events and get familiar with the structure of the data. 

Back to the search, a good place to start searching for the malware is in  Mallory's user folders. Find it in the search results from the last  command. 

Once you have it, expand the search query with the user path and try different folders. 

**Command**:` index="botsv2" kutekitten "\\***/PATH\***\\***/MALLORY\\/FOLDER\***"`

Replace **\\/PATH\\/MALLORY\\/FOLDER** with Mallory's user folder path. For the search query to successfully execute, the path needs to be double escaped. 

Look at the other available interesting fields related to a path that you  can use to add as a field to the search query and look for an  interesting file that stands out once you've used the added field as  part of the query.

**Hint**: You know you have found the interesting file if the available field shows a count of 1. 

Once you think you found the file (you can confirm the file's hash in  VirusTotal), pivot, and look at the events 1 minute prior. 

To do  this, click on the date/time of the event. A new window will pop up that will allow you to view events before or after that specific point in  time.

![img](https://assets.tryhackme.com/additional/splunk-overview/splunk-time-correlation.png)

Now you need to run a new search query to focus on events within the specific time segment. 

It would be a good idea to refer to the documentation on Osquery [here](https://osquery.readthedocs.io/en/stable/introduction/using-osqueryd/) to help you with this. 

**Command**: `index="botsv2" kutekitten ***KEYWORD\*** ***KEYWORD\***`

Don't forget to replace the KEYWORD with the actual keywords you think will help you narrow down your search. 

**Note**: The events will not provide the name of the USB manufacturer; you need  to perform external research on the ID value to get that answer.

For questions 4-7, you have enough at this point to help you get the answers to those questions. :)

Continuing on, it's time to attempt to answer some of the 400 series questions from the BOTS2 dataset and then some. 

**Questions 1 & 2**

You're tasked to find the name of the attachment sent to Frothly by the  malicious APT actor. This will involve events related to emails.

You're provided with a command that will lead you to the answer. Replace the **?** and .**EXT** with the appropriate values. 

**Command**: `index="botsv2" sourcetype="stream:***?\***" *.***EXT\***`

You should be able to retrieve the password on your own at this point. :)

**Question 3****
**

For this question, you will need the attacker's IP. Remember, there was an IP address scanning brewertalk.com. 

Use that IP address and search the **TCP** stream instead of the **HTTP** stream. 

Once the events are returned, look at the **Interesting Fields**. 

**Command**: `index="botsv2" sourcetype="stream:***?\***" ***ATTACKER_IP\***`

**Question 4****
**

Next task, find an unusual file that was downloaded with winsys32.dll. 

Notice that it's mentioned that this file would be considered unusual for an  American company. This is a hint that it has something to do with  language. 

**Command**: `index="botsv2" winsys32.dll`

Look through the results; you should see a tool associated with transferring files from system to system. 

There is a source type associated with the binary. Use that to start a new search query.

**Command**: `index="botsv2" sourcetype="stream:***?\***"`

Replace the **?** with the appropriate value. 

Over 1,000 events are returned. It might be a good idea to shrink this further down. But how?

You're looking for an unusual file that was downloaded by the winsys32.dll.  Research commands that can be utilized with the tool that is specific to downloads. Once you find the command, expand your search query. 

**Command**: `index="botsv2" sourcetype="stream:***?\***" method=***COMMAND\***`

You know the drill, replace the **?** and **COMMAND** with the appropriate values.

The unusual file should be noticeable in the returned events. If not, then look at the **Interesting Fields**. 

**Questions 5 & 6**

Use the following links to examine the execution of the malware contained within the aforementioned zip file. 

- Hybrid Analysis - https://www.hybrid-analysis.com/sample/d8834aaa5ad6d8ee5ae71e042aca5cab960e73a6827e45339620359633608cf1/598155a67ca3e1449f281ac4
- VirusTotal - https://www.virustotal.com/gui/file/d8834aaa5ad6d8ee5ae71e042aca5cab960e73a6827e45339620359633608cf1/detection
- Any.run - https://app.any.run/tasks/15d17cd6-0eb6-4f52-968d-0f897fd6c3b3

These sources will help you find the answer to this question, along with the following question.

**Question 7**

I'm confident you can tackle this one solo. Below is a command to get you started.

**Command**: `index="botsv2" schtasks.exe`

The amount returned should be over 100 events. Look at the returned  results. Some entries should stand out. Next figure out what keyword(s)  and source type you need to find the answer. 

You'll need to perform additional steps for each event to determine the answer to the last question. Good luck! :)