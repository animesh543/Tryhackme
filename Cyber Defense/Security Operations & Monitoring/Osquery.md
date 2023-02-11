# Introduction

**[Osquery](https://osquery.io/)** is an [open-source](https://github.com/osquery/osquery) tool created by **[Facebook](https://engineering.fb.com/2014/10/29/security/introducing-osquery/)**. With Osquery, Security Analysts, Incident Responders, Threat Hunters, etc.,  can query an endpoint (or multiple endpoints) using SQL syntax. Osquery can be installed on multiple platforms: Windows, Linux, macOS, and FreeBSD. 

Many well-known companies, besides Facebook, either use Osquery, utilize  osquery within their tools, and/or look for individuals who know  Osquery. 

As of today (March 2021), **Github** and **AT&T** seek individuals who have experience with Osquery. 

**Github**:

![img](https://assets.tryhackme.com/additional/osquery/github-posting.png)


**AT&T:**

![img](https://assets.tryhackme.com/additional/osquery/att-posting.png)

Some of the tools (open-source and commercial) that utilize Osquery are listed below.

- **Alienvault**: The [AlienVault agent](https://otx.alienvault.com/endpoint-security/welcome) is based on Osquery. 
- **Cisco**: Cisco AMP (Advanced Malware Protection) for endpoints utilize Osquery in [Cisco Orbital](https://orbital.amp.cisco.com/help/). 

Learning Osquery will be beneficial if you are looking to enter into this field  or if you're already in the field and you're looking to level up your  skills.

**Note**: It is highly beneficial if you're already familiar with SQL queries. If not, check out this [SQL Tutorial](https://www.w3schools.com/sql/sql_intro.asp).

#  Installation

If you wish to install Osquery on your local machine or local virtual machine, please refer to the installation instructions. 

- [Install on Windows](https://osquery.readthedocs.io/en/stable/installation/install-windows/)
- [Install on Linux](https://osquery.readthedocs.io/en/stable/installation/install-linux/)
- [Install on macOS](https://osquery.readthedocs.io/en/stable/installation/install-macos/)
- [Install on FreeBSD](https://osquery.readthedocs.io/en/stable/installation/install-freebsd/)

Refer to the documentation on the Osquery daemon (osqueryd) information and all the command-line flags [here](https://osquery.readthedocs.io/en/latest/installation/cli-flags/). 

#  Interacting with the Osquery Shell                            

To interact with the Osquery interactive console/shell, open CMD (or PowerShell) and run `osqueryi`. 

As per the documentation, **osqueryi** is a modified version of the SQLite shell. 

You'll know that you've successfully entered into the interactive shell by the new command prompt.

![img](https://assets.tryhackme.com/additional/osquery/osquery_prompt.png)

One way to familiarize yourself with the Osquery interactive shell, as with any new tool, is to check its help menu. 

In Osquery, the help command (or meta-command) is `.help`. 

![img](https://assets.tryhackme.com/additional/osquery/osquery_help.png)

**Note**: As per the documentation, meta-commands are prefixed with a '`.`'.

To list all the available tables that can be queried, use the `.tables` meta-command. 

For example, if you wish to check what tables are associated with processes, you can use `.tables process`.

![img](https://assets.tryhackme.com/additional/osquery/osquery_tables.png)

In the above image, 3 tables are returned that contain the word 'process.' 

**Note**: Depending on the operating system, different tables will be returned when the .tables meta-command is executed. 

Table names are not enough to know exactly what information is contained in any given table without actually querying it. 

Knowing what columns and types, known as a **schema**, for each table are also useful. 

You can list a table's schema with the following meta-command: `.schema table_name`

![img](https://assets.tryhackme.com/additional/osquery/osquery_schema.png)

Looking at the above image, **pid** is the **column,** and **BIGINT** is the **type**. 

**Note**: Any user on a system can run and interact with osqueryi, but some  tables might return limited results compared to running osqueryi from an elevated shell. 

If you which to check the schema for another operating system, you'll need to use the `--enable_foreign` command-line flag. 

To read more about command-line flags, refer to this page, https://osquery.readthedocs.io/en/latest/installation/cli-flags/. 

Interacting with the shell to get quick schema information for a table is good but  not ideal when you want schema information for multiple tables. 

For that, the schema API online documentation can be used to view a  complete list of tables, columns, types, and column descriptions. 

# Schema Documentation                            

Head over to the schema documentation [here](https://osquery.io/schema/4.7.0/). 

![img](https://assets.tryhackme.com/additional/osquery/osquery_apischema-1.png)

The above image is a resemblance to what you'll see when you navigate to the page. 

**Note**: At the time of this writing, the current version for Osquery is 4.7.0.

A breakdown of the information listed on the schema API page is explained below.

1. A dropdown listing various versions of Osquery. Choose the version of Osquery you wish to see schema tables for.
2. The number of tables within the selected version of Osquery. (In the above image, 271 tables exist for Osquery 4.7.0)
3. The list of the tables is listed in alphabetical order for the selected version of Osquery. 
4. The name of the table and a brief description.
5. A detailed chart listing the **column**, **type**, and column **description** for each table.
6. Information to which operating system the table applies to. (In the above image, the **account_policy_data** table is available only for **macOS**)

You have enough information to confidently navigate this resource to retrieve any information you'll need. 

# Creating queries

The SQL language  implemented in Osquery is not an entire SQL language that you might be  accustomed to, but rather it's a superset of SQLite's. 

Realistically all your queries will start with a **SELECT** statement. This makes sense because, with Osquery, you are only  querying information on an endpoint or endpoints. You won't be updating  or deleting any information/data on the endpoint. 

**The exception to the rule**: The use of other SQL statements, such as **UPDATE** and **DELETE,** is possible, but only if you're creating run-time tables (views) or using an extension if the extension supports them. 

Your queries will also include a **FROM** clause and end with a **semicolon**. 

If you wish to retrieve all the information about the running processes on the endpoint: `SELECT * FROM processes;`

![img](https://assets.tryhackme.com/additional/osquery/osquery_selectall.png)

**Note**: The results for you will be different if you run this query in the attached VM or your local machine (if Osquery is installed).

The number of columns returned might be more than what you need. You can  select specific columns rather than retrieving every column in the  table. 

**Query**: `SELECT pid, name, path FROM processes;`

![img](https://assets.tryhackme.com/additional/osquery/osquery_notselectall.png)

The above query will list the process id, the process's name, and the path for all running processes on the endpoint. 

This will still return a large number of results, depending on how busy the endpoint is. 

The **count()** function can be used to get exactly how many.

**Query**: `SELECT count(*) from processes;`

![img](https://assets.tryhackme.com/additional/osquery/osquery_count.png)

The output can be limited to the first 3 in ascending order by process name, as shown below.

![img](https://assets.tryhackme.com/additional/osquery/osquery_orderby_limit.png)

Optionally, you can use a **WHERE** clause to narrow down the list of results returned based on specified criteria. 

**Query**: `SELECT pid, name, path FROM processes WHERE name='lsass.exe';`

![img](https://assets.tryhackme.com/additional/osquery/osquery_where.png)

The equal sign is not the only filtering option available in a WHERE clause. 

Below are filtering operators that can be used in a WHERE clause:

- `=` [equal]
- `<> ` [not equal]
- `>`, `>=` [greater than, greater than or equal to]
- `<`, `<=` [less than or less than or equal to] 
- `BETWEEN` [between a range]
- `LIKE` [pattern wildcard searches]
- `%` [wildcard, multiple characters]
- `_` [wildcard, one character]

Below is a screenshot from the Osquery [documentation](https://osquery.readthedocs.io/en/stable/deployment/file-integrity-monitoring/) showing examples of using wildcards when used in folder structures. 

![img](https://assets.tryhackme.com/additional/osquery/osquery_wildcard.png)

Some tables will require a **WHERE** clause, such as the **file** table, to return a value. If the required **WHERE** clause is not included in the query, then you will get an error. 

![img](https://assets.tryhackme.com/additional/osquery/osquery_fileerror.png)

The last concept to cover is **JOIN**. To join 2 or more tables, each table needs to share a column in common. 

Let's look at 2 tables to demonstrate this further. Below is the schema for the **osquery_info** table and the **processes** table. 

![img](https://assets.tryhackme.com/additional/osquery/osquery_join_example.png)

The common column in both tables is **pid**. A query can be constructed to use the **JOIN** clause to join these 2 tables **USING** the PID column. 

**Query**: `SELECT pid, name, path FROM osquery_info JOIN processes USING (pid);`

![img](https://assets.tryhackme.com/additional/osquery/osquery_join.png)

Please refer to the Osquery [documentation](https://osquery.readthedocs.io/en/stable/introduction/sql/) for more information regarding SQL and creating queries specific to Osquery. 

# Using Kolide Fleet                            

In this task, we will look at an open-source Osquery Fleet Manager known as **[Kolide Fleet](https://github.com/kolide/fleet)**. 

With Kolide Fleet, instead of using Osquery locally to query an endpoint,  you can query multiple endpoints from the Kolide Fleet UI. 

**Note**: The open-source repo of Kolide Fleet is no longer supported and was  retired on November 4th, 2020. A commercial version, known as **Kolide K2**, is available. You can view more about it [here](https://www.kolide.com/launcher). There is a more recent repo called [**fleet**](https://github.com/fleetdm/fleet), a fork of the original Kolide Fleet, and as per the creators of Kolide Fleet, "*it appears to be the first of many promising forks*." 

The attached VM has Kolide Fleet installed and configured thanks to the [**Windows Subsystem for Linux**](https://docs.microsoft.com/en-us/windows/wsl/about) (**WSL**). Steps need to be executed to start Kolide Fleet in the attached VM, though. 

Open an Ubuntu terminal and enter the following commands. (**sudo password** is `tryhackme`)

**Command**: `sudo redis-server --daemonize yes`

![img](https://assets.tryhackme.com/additional/osquery/kolide_redis_server.png)**
**

**Command**: `sudo service mysql start`

![img](https://assets.tryhackme.com/additional/osquery/kolide_mysql_start.png)**
**

**Command**:`/usr/bin/fleet serve \--mysql_address=127.0.0.1:3306 \--mysql_database=kolide  \--mysql_username=root \--mysql_password=tryhackme  \--redis_address=127.0.0.1:6379  \--server_cert=/home/tryhackme/server.cert  \--server_key=/home/tryhackme/server.key  \--auth_jwt_key=JB+wEDR4V3bbhU4OlIMcXpcBQAaZc+4r \--logging_json`

![img](https://assets.tryhackme.com/additional/osquery/kolide_server_start.png)

A text file with the above commands is on the desktop in a file titled `kolide-commands.txt`.

Open Google Chrome and navigate to [https://127.0.0.1:8080.](http://127.0.0.1:8080.) The credentials to log into Kolide Fleet are below:

**Username**: `thmosquery`

**Password**: `tryhackme1!`

If all goes well, you should be greeted with the Kolide Fleet UI, similar to the image below. 

![img](https://assets.tryhackme.com/additional/osquery/osquery_kolide.png)

Now it's time to add a host, which will be the actual Windows machine. 

Open the Windows CMD and navigate to C:\Users\Administrator\Desktop\launcher\windows. 

From within that directory, run the following command:

**Command**: `launcher.exe --hostname=127.0.0.1:8080 --enroll_secret=***ENTER-SECRET-KEY\*** --insecure`

Before executing the above command, you need to replace **ENTER-SECRET-KEY** with the **Osquery Enroll Secret**. 

If all goes well, you should see the machine successfully added to the fleet. 

![img](https://assets.tryhackme.com/additional/osquery/kolide_agent.png)

**Note**: You may need to refresh the page to see the machine added. 

Try to run a query against the new endpoint using the Kolide Fleet UI.

Click on `Query > Create New Query` (or click the database icon next to the machine name).

![img](https://assets.tryhackme.com/additional/osquery/kolide_query.png)

Let's look at a brief overview of the **New Query** page.



![img](https://assets.tryhackme.com/additional/osquery/kolide_new_query3.png)

1. If you wish to save your query, give your query a title.
2. This is the SQL command to execute when this query is run.
3. A brief description to what is the objective of the query. 
4. What host, or hosts, to run the query against. 
5. Save the query for future executions.
6. Execute the query.

You don't have to save the query to run it. You can enter the SQL command, select the host(s), and run the command. 

A few more things to mention about the UI: each column in the returned results is filterable, and information about each table is available. 

![img](https://assets.tryhackme.com/additional/osquery/kolide_query_filter2.png)

The above image shows the query returned 106 results, but the output was filtered to just 1 by filtering on the **cmdline** column to return the results for '**lsass**'.

![img](https://assets.tryhackme.com/additional/osquery/kolide_table_desc2.png)

At the far right of the UI, there is a convenient **Table Documentation** which is essentially the schema for each table. The above screenshot shows the schema for the **users** table. 

Feel free to explore **Query Packs** at your own leisure. You can read more about this [here](https://osquery.readthedocs.io/en/stable/deployment/configuration/) and [here](https://osquery.readthedocs.io/en/stable/deployment/log-aggregation/).

# Osquery extensions

Extensions add functionality/features (i.e., additional tables) that are not included in the core Osquery. Anyone can create extensions for Osquery. The official documentation on this subject is [here](https://osquery.readthedocs.io/en/latest/deployment/extensions/). 

If you perform a search, you'll find some interesting ones that can be  downloaded and implemented with Osquery with little hassle. Others might require extra steps, such as setting up additional dependencies and  compiling the extension before use. 

Below are 2 repos of Osquery extensions that you can play with. 

- https://github.com/trailofbits/osquery-extensions
- https://github.com/polylogyx/osq-ext-bin

The Polylogyx extension is available in the attached VM, and you will load  and interact with this extension in the upcoming tasks. 

# Linux and Osquery

Review the On-Demand YARA scanning [here](https://osquery.readthedocs.io/en/stable/deployment/yara/)

# Windows and Osquery                            

For this exercise, use either Kolide Fleet or the Windows CMD/PowerShell. 

**Note**: For the questions which involve the Polylogyx **osq-ext-bin** extension, you'll need to interact with Osquery via the command line. 

**To load the extension**: `osqueryi --allow-unsafe --extension "C:\Program Files\osquery\extensions\osq-ext-bin\plgx_win_extension.ext.exe"`

Wait for the command prompt to reflect the phrase `Done StartDriver`. This will indicate that the extension is fully loaded into the session.

**Tip**: If the phrase doesn't appear after a minute or so, hit the ENTER key. It should appear right after. 

Resources for Polylogx **osq-ext-bin**:

- https://github.com/polylogyx/osq-ext-bin/blob/master/README.md
- https://github.com/polylogyx/osq-ext-bin/tree/master/tables-schema
