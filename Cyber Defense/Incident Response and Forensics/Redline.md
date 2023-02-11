# Data Collection                            

Now that you have the overview for Redline, let's move to the Data Collection stage.

There are three ways or options to collect data using Redline: 



![img](https://assets.tryhackme.com/additional/redline101/capture2.png)

1. **Standard Collector** - this method configures the script to gather a minimum amount of data  for the analysis. This is going to be our preferred method to collect  data in this room. It is also usually the fastest method to collect the  data you need. It takes only a few minutes to complete.
2. **Comprehensive Collector** - this method configures the script to gather the most data from your  host for further analysis. This method takes up to an hour or more. You  will choose this method if you prefer the full analysis of the system.
3. **IOC Search Collector** (Windows only) - this method collects data that matches with the [Indicators of Compromise (IOCs)](https://www.crowdstrike.com/cybersecurity-101/indicators-of-compromise/) that you created with the help of [IOC Editor](https://www.fireeye.com/services/freeware/ioc-editor.html). You will choose this method if you want to run the data collection  against known IOCs that you have gathered either through threat  intelligence (data feed or narrative report), incident response, or  malware analysis. You imported them into [IOC Editor](https://www.fireeye.com/services/freeware/ioc-editor.html). We'll look at the IOC Editor a bit further in the next task.

In this task, we will be using the Standard Collector method.

- From Redline, click on "Create a Standard Collector".
- You will have an option to choose the target platform. In our case, we will select **Windows**.

![img](https://assets.tryhackme.com/additional/redline101/targetos.png)

- Under the *Review Script Configuration,* click on “Edit your script”, this is one of the crucial steps since you will be presented with the set of data to choose to collect from the host. There will be five tabs, which include **Memory, Disk, System, Network**, and **Other.**

Let's dive into some details:

**Memory:**

![img](https://assets.tryhackme.com/additional/redline101/memoryyy.png)

You can configure the script to collect memory data such as process  listings, drivers enumeration (Windows hosts only), and hook detection (versions before Windows 10).

**Note**: For this exercise, uncheck **Hook Detection** and make sure **Aquire Memory Image** is also unchecked. 

Be sure to make changes to the settings in each tab as necessary to mirror the settings illustrated in the task content. 

**Disk:** 

This is where you can collect the data on Disks partitions and Volumes along with File Enumeration.

![img](https://assets.tryhackme.com/additional/redline101/diskkk.png)

**System:**

The system will provide you with machine information:

- Machine and operating system (OS) information
- Analyze system restore points (Windows versions before 10 only)
- Enumerate the registry hives (Windows only)
- Obtain user accounts (Windows and OS X only)
- Obtain groups (OS X only)
- Obtain the prefetch cache (Windows only) 

![img](https://assets.tryhackme.com/additional/redline101/systemm.png)

**Network:**

Network Options supports Windows, OS X, and Linux platforms. You can configure the script to collect network information and browser history, which is essential when investigating  the browser activities, including malicious file downloads and  inbound/outbound connections. 



![img](https://assets.tryhackme.com/additional/redline101/networkkk.png)**
**

**Other:**

![img](https://assets.tryhackme.com/additional/redline101/other.png)

With this option, you can collect the data on different services and tasks running on the system, including the hashes.

- Now we are ready to proceed to the next important step. After choosing your data options - click OK. And then click on*"Browse"* under *"Save Your Collector To"*. Why is this an important step? Because you will need to create a folder where your analysis file will be saved and the script for collecting  the data you need. In our case, we are saving it to the *Analysis* folder.

![img](https://assets.tryhackme.com/additional/redline101/32.png)**
**

**Note**: You can choose any folder you wish but make sure that the folder is EMPTY. Complete this dialog by clicking the OK button. 

- After you choose to save the collector to the folder, you will see the Collector Instructions. 

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/f8ddaa960778e373c108ea2fa3ebe67f.png)

- If you go into the folder, you will notice the bat file under the name **"RunRedlineAudit"**. This is the executable script to collect data from the host. The script needs to be *run as Administrator* to be able to collect the data we need.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/ef6666436c67665ba44f03f149a89320.png)

- Running the script will open a command prompt window; this indicates that the  script is running successfully. It will close automatically when the  data collection process finishes. 

![img](https://assets.tryhackme.com/additional/redline101/1213.png)**
**

**Note**: This process may take between 15-20 minutes to complete.

- After the script is finished, you will notice a new file created - **AnalysisSession1** (in the **Sessions** folder) with the *.mans* extension. This file is what we need to be able to import into Redline for  investigation. Just double-click on the file to import the audit data.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/93f84b334ff930f8fdbb3bb316d010c3.png)

**Tip**: If you run the script multiple times, the naming convention of the  analysis file will increment by 1. For example, if you run the script  two times, you will see **AnalysisSession1** and **AnalysisSession2**. 

# The Redline Interface                            

![img](https://i.ibb.co/CH6zS38/red1.png)

When the data is imported, you will be presented with this view:

![img](https://i.ibb.co/8YhfzHb/redlineee.png)

On the left panel, you will see different types of *Analysis Data;* this is where you will perform information gathering and investigation process.

- **System Infomation:** this is where you will see the information about the machine, BIOS (Windows only), operating system, and user information.
- **Processes:** processes will contain different attributes such as Process Name, PID, Path,  Arguments, Parent process, Username, etc. When you expand the Processes  tab, there will be four sections: Handles, Memory Sections, Strings, and Ports.

A **handle** is a connection from a process to an object or resource in a Windows  operating system. Operating systems use handles for referencing internal objects like files, registry keys, resources, etc.

**Memory Sections** will let you investigate unsigned memory sections used by some processes.  Many processes usually use legitimate dynamic link libraries (DLLs),  which will be signed. This is particularly interesting because if you  see any unsigned DLLs then it will be worth taking a closer look. 

**Strings** - you will see the information on the captured strings.

**Ports** - this is one of the critical sections to pay attention to. Most malware  often initiates the outbound or inbound connections to communicate to  their command and control server (C2) to do some malicious activities like exfiltrating the data or grabbing a payload to the machine. This situation is where you can review the  suspicious connections from ports and IP addresses. Pay attention to the system processes as well. The threat actors like to avoid detection by  hiding under the system processes. For example, explorer.exe or  notepad.exe shouldn't be on the list of processes with outbound  connections. 

Some of the other important sections you need to pay attention to are:

- File System (**not included in this analysis session**)
- Registry
- Windows Services
- Tasks (Threat actors like to create scheduled tasks for persistence)
- Event Logs (this another great place to look for the suspicious Windows  PowerShell events as well as the Logon/Logoff, user creation events, and others)
- ARP and Route Entries (**not included in this analysis session**)
- Browser URL History (**not included in this analysis session**)
- File Download History

The **Timeline** will help you to better understand when the compromise happened and  what steps the malicious actor took to escalate the attack. The **Timeline** will also record every action on the file if it got create, changed, modified, accessed. 

![img](https://i.ibb.co/mN1GyRf/files.png)

If you know when the host compromise or suspicious activity occurred, you can use **TimeWrinkles™** to filter out the timeline to only the events that took place around that time. 

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/0d3564554dec83b1e948cf2e78a41b26.png)

![img](https://i.ibb.co/k4gB91m/wrinklee.png)

**TimeCrunches™** helps to reduce the excessive amount of data that is not relevant in the  table view. A TimeCrunch will hide the same types of events that  occurred within the same minute you specified.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/e38cdd16a6b56e484a6d9a4549c7348b.png)

![img](https://i.ibb.co/RTc3t0z/timeline2.png)

You can find out more about each type of data analysis using the Redline User Guide: https://www.fireeye.com/content/dam/fireeye-www/services/freeware/ug-redline.pdf.

# IOC Search Collector                           

We briefly discussed the usage of the **IOC Search Collector** in the **Data Collection** task. 

Let's take a closer look at the capabilities of this collector type. But first, let's recap what an IOC is. 

IOC stands for **Indicators of Compromise**; they are artifacts of the potential compromise and host intrusion on  the system or network that you need to look for when conducting threat  hunting or performing incident response. IOCs can be MD5, SHA1, SHA256  hashes, IP address, C2 domain, file size, filename, file path, a registry key, etc.

One of the great tools you can use is [IOC Editor, ](https://www.fireeye.com/services/freeware/ioc-editor.html)created by FireEye, to create IOC files. You can refer to this link to learn how to use the IOC Editor: https://www.fireeye.com/content/dam/fireeye-www/services/freeware/ug-ioc-editor.pdf. 

**Note**: According to the [IOC Editor](https://www.fireeye.com/services/freeware/ioc-editor.html) download page Windows 7 is the latest operating system officially supported. It is the same version installed in the attached VM. There is another tool called [OpenIOC Editor](https://fireeye.market/apps/211404) by FireEye, which supports Windows 10 that is worth taking a look at. 

**Tip**: Before proceeding you can close Redline to free up some system resources while using IOC Editor. 

You can create a text file containing IOCs, modify them, and share it with other people in the InfoSec industry.

In this example, we will look at an IOC of a keylogger created with IOC Editor. 

**Note**: Below, you may follow along with the screenshots and don't have to create the IOC file in this task. You will create an IOC file using IOC Editor and perform an IOC Search in the next task. 



![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/b26d9e80ac55821643531c3a0436f633.png)

**Keylogger indicators in IOC Editor**:

![img](https://i.ibb.co/02VS0M6/keylogger2.png)

A brief explanation of the above image:

- The **Name** of the IOC file is Keylogger, Keylogger.ioc. (this field you can edit)
- The **Author** is RussianPanda. (this field you can edit)
- **GUID**, **Created**, and **Modified** are fields you can **NOT** edit, and IOC Editor populates the information.
- Under **Description**, you can add a summary explaining the purpose of the IOC file.

The actual IOCs will be added under, you guessed it, **Add**. 

Here are the values from the image above:

- **File Strings** - `psylog.exe`
- **File Strings** - `RIDEV_INPUTSINK`
- **File MD5** - `791ca706b285b9ae3192a33128e4ecbb`
- **File Size** - `35400`

Refer to the gif below to get an idea of adding specific IOCs to the IOC file.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/01db4361981d214c2692aa10d59961d1.gif)

Once you select an item you can enter the value for the item directly. 

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/81e9ffdb97a2ce98e8b9cec57a2be261.png)

You can also add it within the **Properties**. 

All the fields are read-only except for **Content** and **Comment**. To add a value to the item enter it under **Content**. 

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/5a0e549950f7ca673699d51a2ff14bc9.png)

Once you enter the value click Save to save it.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/9d95abf1f3d62f3fe7d2eb6352b86235.png)

**Note**: You can right-click on an item for additional options. See below.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5de58e2bfac4a912bcc7a3e9/room-content/f5173beaf331e84b7672daf6be726092.png)

We'll leave that for you to explore on your own. 

Now that we've created and saved the IOC file, let's move on and go back to the **IOC Search Collector** in the **Redline** tool.

**Note**: If you closed Redline now is the time to relaunch the application. You can close IOC Editor, again, to free up system resources. 

**IOC Search Collector** will ignore the data that doesn't match an IOC you have gathered. Although, you can always choose to collect  additional data. As the Redline User Guide states, the quality of the  IOC analysis will depend on the data you have available in the analysis  session

![img](https://i.ibb.co/SwvyRyq/ioc.png)

To create an IOC Search Collector, click "Browse..." and choose the location of the .ioc file. Redline will automatically detect the .ioc file and place it in  the Indicators section, as shown below.

**IOC Search Collector**:

![img](https://i.ibb.co/2S2t1sB/keylogger.png)



**Unsupported Search Terms:** These terms will not show any successful hits in Redline, which means Redline doesn't recognize specific search terms. 

**Supported Search Terms:** The terms that Redline will recognize and search for.

After you are finished reviewing the configured IOCs, click "Next". Now click on "Edit your script" to configure what data  will be collected for the analysis. For this example, Keylogger file IOC Search, the following parameters were selected. 

![img](https://i.ibb.co/g7JkhPr/keylogger3.png)

**Note**: When you configure your own IOC Search, you will choose different settings for your script compared to the settings above. 

When done editing the script, click "OK". 

In the "Save Your Collector To" section, click "Browse" and choose an  empty folder where your analysis file will be saved along with the **RunRedlineAudit.bat** file. 

After executing the .bat file in the same manner as before, let's now wait for the analysis to finish.

After the analysis is finished, you will see the .mans file (AnalysisSession1 in our example). Double-click the file to open it in Redline. 

![img](https://i.ibb.co/xhJwX1f/analysis.png)

If Redline fails to generate the IOC Report automatically, you can manually generate it by clicking "Create a New IOC Report" and importing your .ioc file.

When the report generation completes, you should see the "Hits". You can expand the list by clicking on the entries in each row.

![img](https://i.ibb.co/bvVVdj5/keyllogger4.png)

From the screenshot, you can see that there was one hit on "chrome.dll", this is a false positive. Let's review the details below. 

![img](https://i.ibb.co/d0RCvdH/hits2.png)

As you can see, the DLL file matched with the string "RIDEV_INPUTSINK" that we had in our .ioc  file. It's important to gather granulated and accurate artifacts to add  to your IOC file to avoid false positives. 

The screenshot below is of a file with the most amount of "Hits", which means it is most likely the file we are looking for. 

![img](https://i.ibb.co/tzvyS6w/hits3.png)