# Workflow Overview

Before diving into Autopsy and analyzing data, there are a few steps  to perform, such as identifying the data source and what Autopsy actions to perform with the data source. 

Your basic workflow:

1. Create the case for the data source you will investigate
2. Select the data source you wish to analyze
3. Configure the ingest modules to extract specific artifacts from the data source
4. Review the artifacts extracted by the ingest modules
5. Create the report

Below is a visual of **step #1**. 

When you start Autopsy, there will be 3 options. To start a new case, click on `New Case`.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-newcase1.png)

The next screen is titled **Case Information,** and this is where information about the case is populated.



![img](https://assets.tryhackme.com/additional/autopsy/autopsy-newcase3.png)

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-autfile2.png)

- **Case Name**: The name you wish to give to the case
- **Base Directory**: The root directory that will store all the files specific to the case (the full path will be displayed)
- **Case Type**: Specify whether this case will be local (**Single-user**) or hosted on a server where multiple analysts can review (**Multi-user**)

**Note**: In this room, the focus is on **Single-User**.

The screen that follows is titled, **Optional Information** and it can be left blank for our purposes. In an actual forensic environment, you should fill out this information. When you're done, click `Finish`. 

In this room, you will import a case. To open a case, you will select is `Open Case`. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-opencase.png)

Autopsy case files have an `.aut` file extension. Navigate to the case folder and select the .aut file you wish to open. 



![img](https://assets.tryhackme.com/additional/autopsy/autopsy-autfile2.png)

Next, Autopsy will process the case files open the case. 

You can identify the name of the case at the top left corner of the Autopsy window. In the image below, the name of this case is Tryhackme. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-casename2.png)

**Note**: If Autopsy is unable to locate the disk image, a warning box will  appear. At this point, you can point Autopsy to the location of the disk image it's attempting to find, or you can click NO; you can still  analyze the data from the Autopsy case. 

 ![img](https://assets.tryhackme.com/additional/autopsy/autopsy-missing-image3.png)

# Data Sources

Below is a screenshot of the `Add Data Source` Windows dialog box. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-datasources.png)

In this room, we will focus primarily on the first option, `Disk Image or VM file`. 

Supported Disk Image Formats:

- **Raw Single** (For example: *.img, *.dd, *.raw, *.bin)
- **Raw Split** (For example: *.001, *.002, *.aa, *.ab, etc)
- **EnCase** (For example: *.e01, *.e02, etc)
- **Virtual Machines** (For example: *.vmdk, *.vhd)

If there are multiple image files (e.i. E01, E02, E03, etc.) Autopsy only  needs you to point to the first image file, and Autopsy will handle the  rest. 

**Note**: Refer to the Autopsy [documentation](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/ds_page.html) to understand the other data sources that can be added to a case. 

Below is a screenshot of an E01 disk image added to a sample case as a data source. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-add-datasource.png)

Specify the time zone and click `Next`. 

**Note**: Orphan files are deleted files that no longer have a parent folder. In  FAT file systems, it can be time-sensitive to read and analyze. 

# Ingest Modules                            

Essentially **Ingest Modules** are Autopsy plug-ins. Each Ingest Module is designed to analyze and retrieve specific data from the drive. 

Below is a screenshot of the `Configure Ingest Modules` window. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-configure-modules.png)

By default, the Ingest Modules are configured to run on **All Files, Directories, and Unallocated Space**. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-run-modules-on.png)

The other two options are:

1. All Files and Directories (Not Unallocated Space)
2. Create/edit file ingest filters...

**Note**: We will not cover ingest filters in this room. 

If all the Ingest Modules are deselected, and `Next` is selected, Autopsy will still process the data source and update the local database. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-no-ingest2.png)

**Note**: Autopsy adds metadata about files to the local database, not the actual file contents. 

When Autopsy is done, you will see the following: 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-done-processing.png)

To complete this process, click `Finish`. 

In the below image, since the Ingest Modules were deselected, there aren't any results in the Results node. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-no-ingest3.png)

The results of any Ingest Module you select to run against a data source  will populate the Results node in the Tree view, which is the left pane  of the Autopsy user interface. 

You can run Ingest Modules at any time while the case is open. To do so, right-click on the data source and select `Run Ingest Modules`. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-right-click-modules.png)

As Ingest Modules run, alerts may appear in the **Ingest Inbox**. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-ingest-inbox.png)

Below is an example of the Ingest Inbox after a few Ingest Modules have completed running. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-inbox2.png)

Drawing the attention back to the Configure Ingest Modules window, notice that some Ingest Modules have **per-run settings** and some do not.

For example, the **Recent Activity** Ingest Module does not have per-run settings. In contrast, the **Hash Lookup** Ingest Module does. 

To learn more about Ingest Modules, read Autopsy documentation [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/ingest_page.html). 

# The User Interface                             

Let's look at the Autopsy user interface, which is comprised of 5 primary areas: 

- **Tree Viewer** (Left pane)
- **Result Viewer** (Top right pane)
- **Keyword Search** (Upper Top Right)
- **Contents Viewer** (Bottom right pane)
- **Status Area** (Lower Bottom right)

Each area will be explained briefly below. 

**Tree Viewer**

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-tree-view.png)

The **Tree Viewer** has **5 top-level nodes**:

- **Data Sources** - all the data will be organized as you would typically see it in a normal Windows File Explorer. 
- **Views** - files will be organized based on file types, MIME types, file size, etc. 
- **Results** - as mentioned earlier, this is where the results from Ingest Modules will appear. 
- **Tags** - will display files and/or results that have been tagged (read more about tagging [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/tagging_page.html))
- **Reports** - will display reports either generated by modules or the analyst. (read more about reporting [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/reporting_page.html))

Refer to the Autopsy documentation on the **Tree Viewer** for more information [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/tree_viewer_page.html). 

**Result Viewer**

**Note**: Don't confuse the Results node (from the Tree Viewer) with the Result Viewer. 

When a volume, file, folder, etc., are selected from the Tree Viewer,  additional information about the selected item is displayed in the  Result Viewer. 

For example, the Sample case's data source is selected, and now additional information is visible in the Results Viewer. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-table-view.png)

If a volume is selected, the Result Viewer's information will change to  reflect the information in the local database for the selected volume. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-table-view2.png)

Notice that the Result Viewer pane has 3 tabs: **Table**, **Thumbnail**, and **Summary**. The 2 above screenshots reflect the information displayed in the Table tab. 

The Thumbnail tab works best with image or video files. If the view of the above data is changed from Table to Thumbnail, not much information will be displayed. See below.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-thumbnail-view.png)

Volume nodes can be expanded, and an analyst can navigate the volume's contents, as they would a typical Windows system. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-volume.png)

In the **Views** tree node, files are categorized by File Types - **By Extension, By** **MIME Type**, **Deleted Files**, and **By** **File Size**.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-views.png) 

**Tip**: When it comes to **File Types**, pay attention to this section. An adversary can rename a file with a  misleading file extension. So the file will be 'miscategorized' **By** **Extension** but will be categorized appropriately by **MIME Type**. 

Expand **By Extension** and more children nodes appear, categorizing files even further (see below).

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-byextension.png)

Refer to the Autopsy documentation on the **Result Viewer** for more information [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/result_viewer_page.html). 

**Contents Viewer**

From the Table tab in the Result Viewer, if you click any folder/file,  additional information is displayed in the Contents Viewer pane. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-contents-view.png)

In the above image, 3 columns might not be quickly understood what they represent. 

- **S** = **Score**

The **Score** will show a red exclamation point for a folder/file marked/tagged as  notable and a yellow triangle pointing downward for a folder/file that  is marked/tagged as suspicious. These items can be marked/tagged by an  Ingest Module or by the analyst.

- **C** = **Comment**

If a yellow page is visible in the Comment column, it will indicate that there is a comment for the folder/file. 

- **O** = **Occurrence** 

In a nutshell, this column will indicate how many times this file/folder has been seen in past cases (this will require the [Central Repository](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/central_repo_page.html))

Refer to the Autopsy documentation on the **Contents** **Viewer** for more information [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/content_viewer_page.html). 

**Keyword Search**

At the top right, you will find **Keyword Lists** and **Keyword Search**.

With **Keyword Search,** an analyst can perform an AD-HOC keyword search. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-keyword-search.png)

In the image above, the analyst is searching for the word 'secret.' Below are the search results.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-keyword-search2.png)

 

Refer to the Autopsy [documentation](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/ad_hoc_keyword_search_page.html) for more information on how to perform keyword searches with either option. 

**Status Area**

Lastly, the **Status Area** is at the bottom right. 

When Ingest Modules are running, a progress bar (along with the percentage  completed) will be displayed in this area. If you click on the bar, more detailed information regarding the Ingest Modules is provided. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-statusbar2.png)

If the `X` (directly next to the progress bar) is clicked on, a prompt will appear confirming if you wish to end cancel the Ingest Modules. 

Refer to the Autopsy documentation on the UI overview [here](http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/uilayout_page.html). 

# Visualization Tools                            

You may have noticed that other parts of the user interface weren't discussed as of yet. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-top-bar.png)

Please refer to the Autopsy documentation for the following visualization tool:

- **Images/Videos** - http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/image_gallery_page.html
- **Communications** - http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/communications_page.html
- **Timeline** - http://sleuthkit.org/autopsy/docs/user-docs/4.12.0/timeline_page.html

**Note**: Within the attached VM, you will **NOT** be able to practice with some of the visualization tools, except for **Timeline**. 

Below is a screenshot of the **Timeline**.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline.png)

The **Timeline** tool is composed of 3 areas:

1. **Filters** - narrow the events displayed based on the filter criteria
2. **Events** - the events are displayed here based on the **View Mode**
3. **Files/Contents** - additional information on the event(s) is displayed in this area

There are 3 view modes:

1. **Counts** - the number of events is displayed in a bar chart view
2. **Details** - information on events is displayed, but they are clustered and collapsed, so the UI is not overloaded
3. **List** - the events are displayed in a table view

In the above screenshot, the View Mode is **Counts**. Below is a screenshot of the **Details** View Mode. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline-details.png)

The numbers (seen above) indicate the number of clustered/collapsed events for a specific time frame. 

For example, for /Windows, there are 130,257 events between 2009-06-10 and 2010-03-18. See the below image. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline-clustered.png)

To expand a cluster, click on the `green icon with the plus sign`. See the below example.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-cluster-expand.png)

To collapse the events, click on the `red icon with a minus sign`. 

Click `the map marker icon with a plus sign` if you wish to pin a group of events. This will move (pin) the events to an isolated section of the Events view. 

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline-clustered2.png)

To unpin the events, click on the `map marker with the minus sign`. 

The last group of icons to cover are the `eye` icons. If you wish to hide a group of events from the Events view, click on the `eye with a minus sign`. 

In the below screenshot, the clustered events for /Boot were hidden and were placed in `Hidden Descriptions` (in the Filters area).

 ![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline-hidden.png)

If you wish to reverse that action and unhide the events, right-click and select `Unhide and remove from list`. See the below example.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline-unhide.png)

Last but not least, below is a screenshot of the **List** View Mode.

![img](https://assets.tryhackme.com/additional/autopsy/autopsy-timeline-list.png)