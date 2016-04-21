---

# required metadata
title: "Common DeployR Administration Tasks"
description: "Common DeployR Administration Tasks: Starting and Stopping DeployR, Inspecting Server Logs, Database Management "
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "03/17/2016"
ms.topic: "article"
ms.prod: "deployr"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: ""
ms.custom: ""

---

# Common DeployR Administration Tasks

## Starting and Stopping DeployR

### Starting DeployR Services

#### For Linux:

To start all DeployR-related services on the main server (not nodes) at once, type the following command:

    /home/deployr-user/deployr/8.0.0/startAll.sh

To start the DeployR-related services individually on the main server (not nodes), type the following commands:

    /home/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh start
    /home/deployr-user/deployr/8.0.0/rserve/rserve.sh start
    /home/deployr-user/deployr/8.0.0/mongo/mongod.sh start

#### For Windows:

At the DOS command prompt, type:

    net start Apache-Tomcat-for-DeployR-8.0
    net start RServe7.4
    net start MongoDB-DeployR-8.0

#### For Mac OS X

To start all DeployR-related services on the main server (not nodes) at once, type the following command:

    /Users/deployr-user/deployr/8.0.0/startAll.sh

To start the DeployR-related services individually on the main server (not nodes), type the following commands:

    /Users/deployr-user/deployr/8.0.0/tomcat/tomcat7.sh start
    /Users/deployr-user/deployr/8.0.0/rserve/rserve.sh start
    /Users/deployr-user/deployr/8.0.0/mongo/mongod.sh start

### Stopping DeployR Services

To stop DeployR-related services, type the following:

#### For Linux:

To stop all services on the main server (not nodes) at once, type the following command:

    /home/deployr-user/deployr/8.0.0/stopAll.sh

To stop the services individually on the main server (not nodes), type the following commands:

    /home/deployr-user/deployr/8.0.0/tomcat/tomcat7/bin/shutdown.sh
    /home/deployr-user/deployr/8.0.0/rserve/rserve.sh stop
    /home/deployr-user/deployr/8.0.0/mongo/mongod.sh stop

#### For Windows

At the DOS command prompt, type:

    net stop Apache-Tomcat-for-DeployR-8.0
    net stop RServe7.4
    net stop MongoDB-DeployR-8.0

#### For Mac OS X

To stop all services on the main server (not nodes) at once, type the following command:

    /Users/deployr-user/deployr/8.0.0/stopAll.sh

To stop the services individually on the main server (not nodes), type the following commands:

    /Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/bin/shutdown.sh
    /Users/deployr-user/deployr/8.0.0/rserve/rserve.sh stop
    /Users/deployr-user/deployr/8.0.0/mongo/mongod.sh stop


## Inspecting Server Logs

The `catalina.out` server log file is found at the following location:

**For Linux:**

	/home/deployr-user/deployr/8.0.0/tomcat/tomcat7/logs/catalina.out

**For Windows:**

	C:\Program Files\Microsoft\DeployR\8.0\Apache_Tomcat\logs\catalina.out

**For Mac OS X:**

	/Users/deployr-user/deployr/8.0.0/tomcat/tomcat7/logs/catalina.out


>[!NOTE]
>[Look here](deployr-admin-diagnostics-troubleshooting.md#inspecting-diagnostic-log-files) for more information on other log files associated with DeployR.

By default, the DeployR server logs at the `INFO` level, which is appropriate for production environments. The server emits `[DEPLOYR-EVENT]` log statements that provide a permanent record of the following:

1.  API Call / Response Events
2.  Authentication Events
3.  HTTP Session Events
4.  Grid R Session Events

Each `[DEPLOYR-EVENT]` is rendered to the log file in a fixed format, which simplifies parsing and searching both manually or within log inspection tools.

The following section describes the different `[DEPLOYR-EVENT]` that can be found in the server log file.

### 1. API Call / Response Events

The log file captures the following events related to API calls:

-   `[API-PRE]` - denoting the start of an API call including call parameters.
-   `[RESPONSE]` - denoting the response generated for the call.
-   `[API-POST]` - denoting the end of an API call.

For example:

    [ INFO 08:29:15:218] ServerFilters DEPLOYR-EVENT[API-PRE][66.193.32.130][0FBAD0638F6BE1A859F3FF65E64FDD80][testuser][/deployr/r/project/create][format:json]

    [ INFO 08:29:15:292] ResponseBuilder DEPLOYR-EVENT[RESPONSE][0FBAD0638F6BE1A859F3FF65E64FDD80][deployr:[response:[success:true, call:/r/project/create, httpcookie:0FBAD0638F6BE1A859F3FF65E64FDD80, project:[project:PROJECT-bb21452e-9f4b-490e-9f1d-90c1ac7f9487, name:null, descr:null, longdescr:null, live:true, shared:false, cookie:null, origin:Project original., author:testuser, authors:[testuser], lastmodified:1404044955286]]]]

    [ INFO 08:29:15:295] ServerFilters DEPLOYR-EVENT[API-POST][66.193.32.130][0FBAD0638F6BE1A859F3FF65E64FDD80][testuser][/deployr/r/project/create]

### 2. Authentication Events

The log file captures the following events related to user authentications:

-   `[SECURITY][AuthenticationSuccessEvent]` - denoting a successful user authentication.
-   `[SECURITY][AuthenticationFailureBadCredentialsEvent]` - denoting a failed user authentication.

For example:

    [ INFO 08:29:20:296] SecurityEventLogger DEPLOYR-EVENT[SECURITY][AuthenticationSuccessEvent][testuser]

    [ INFO 01:52:00:694] SecurityEventLogger DEPLOYR-EVENT[SECURITY][AuthenticationFailureBadCredentialsEvent][testuser]

### 3. HTTP Session Events

The log file captures the following events related to HTTP session creation and release:

-   `[HTTP-SESSION]: CREATED` - denoting the creation of a HTTP session.
-   `[HTTP-SESSION]: RELEASED` - denoting the release of a HTTP session.

For example:

    [ INFO 01:51:58:967] HttpSessionListener DEPLOYR-EVENT[HTTP-SESSION]: CREATED [E5B10C188FE7DC25EA804CDF932AC85B]

    [ INFO 01:53:45:952] HttpSessionListener DEPLOYR-EVENT[HTTP-SESSION]: RELEASED [E5B10C188FE7DC25EA804CDF932AC85B][Mon Jun 30 01:51:58 EDT 2014][Mon Jun 30 01:53:27 EDT 2014][1]

### 4. Grid R Session Events

The log file captures the following events related to R session creation and release:

-   `[GRID-SESSION]: CREATED` - denoting the creation of an R session on the grid.
-   `[GRID-SESSION]: RELEASED` - denoting the release of an R session on the grid.

For example:

    [ INFO 08:29:15:287] LiveEngineService DEPLOYR-EVENT[GRID-SESSION]: CREATED LiveToken[E]: [66.193.32.130][0FBAD0638F6BE1A859F3FF65E64FDD80][PROJECT-bb21452e-9f4b-490e-9f1d-90c1ac7f9487][testuser] [Temporary Project false null NODE-6fb4583c-fa25-4854-b8ba-0600c30ec195 DeployR Default Node Authenticated ]

    [ INFO 08:29:16:909] LiveEngineService DEPLOYR-EVENT[GRID-SESSION]: RELEASED LiveToken[E]: [66.193.32.130][0FBAD0638F6BE1A859F3FF65E64FDD80][PROJECT-bb21452e-9f4b-490e-9f1d-90c1ac7f9487][testuser] [Temporary Project false projectClose NODE-6fb4583c-fa25-4854-b8ba-0600c30ec195 DeployR Default Node Authenticated ]

### Tracing Related Log Events

Related log events can be traced in a number of ways including but not limited to the use of the `HTTP session` identifier or a DeployR-managed `project` identifier. For example, using the `HTTP session` identifier makes the tracing of a series of log events representing an anonymous execution of a repository-managed R script simple as follows.

    [ INFO 06:49:35:859] ServerFilters DEPLOYR-EVENT[API-PRE][180.183.148.137][2DD2302B91770E89333848DBB9505D0A][anonymous][/deployr/r/repository/script/execute][preloadfileversion:, preloadobjectauthor:, storeobject:, preloadfiledirectory:, preloadobjectname:, storefile:, format:json, preloadfileauthor:, version:, preloadobjectdirectory:, preloadfilename:, author:testuser, inputs:{}, storeworkspace:, storedirectory:, directory:root, storepublic:false, filename:Histogram of Auto Sales, graphics:png, robjects:, storenewversion:false, preloadobjectversion:]

    [ INFO 06:49:35:936] LiveEngineService DEPLOYR-EVENT[GRID-SESSION]: CREATED LiveToken[E]: [180.183.148.137][2DD2302B91770E89333848DBB9505D0A][STATELESS-8c3e26c7-84a3-4ab7-ab9a-f0e0155f1cf1][anonymous] [Stateless Project false null NODE-6fb4583c-fa25-4854-b8ba-0600c30ec195 DeployR Default Node Anonymous ]

    [ INFO 06:49:36:680] LiveEngineService DEPLOYR-EVENT[GRID-SESSION]: RELEASED LiveToken[E]: [180.183.148.137][2DD2302B91770E89333848DBB9505D0A][STATELESS-8c3e26c7-84a3-4ab7-ab9a-f0e0155f1cf1][anonymous] [Stateless Project false null NODE-6fb4583c-fa25-4854-b8ba-0600c30ec195 DeployR Default Node Anonymous ]

    [ INFO 06:49:36:714] ResponseBuilder DEPLOYR-EVENT[RESPONSE][2DD2302B91770E89333848DBB9505D0A][deployr:[response:[success:true, call:/r/repository/script/execute, httpcookie:2DD2302B91770E89333848DBB9505D0A, interrupted:false, workspace:[objects:[]], project:[project:STATELESS-8c3e26c7-84a3-4ab7-ab9a-f0e0155f1cf1, name:Project: Autosaved, descr:Autosaved by DeployR., shared:false, longdescr:null, origin:Project original., author:STATELESS, authors:[STATELESS], cookie:null, lastmodified:1404125376681, live:true], repository:[files:[]], execution:[console:     cars trucks suvs, code:, interrupted:false, results:[], artifacts:[[project:STATELESS-8c3e26c7-84a3-4ab7-ab9a-f0e0155f1cf1, category:plot, length:7285, lastmodified:1404125376000, filename:histogram.png, descr:null, type:image/png, url:http://184.106.156.59:8000/deployr/r/repository/script/directory/download/STATELESS-8c3e26c7-84a3-4ab7-ab9a-f0e0155f1cf1/histogram.png]], warnings:[], execution:EXEC-08dc2db6-496b-43fd-a9b8-60abbb6af18e, timeStart:1404125375865, timeCode:176, timeTotal:255, tag:null, actor:anonymous, resultsGenerated:0, resultsAvailable:0]]]]

    [ INFO 06:49:36:717] ServerFilters DEPLOYR-EVENT[API-POST][180.183.148.137][2DD2302B91770E89333848DBB9505D0A][anonymous][/deployr/r/repository/script/execute]

This sample log output captures an `/r/repository/script/execute` API call, originating from `180.183.148.137`, executed on behalf of an `anonymous` user, on HTTP session `2DD2302B91770E89333848DBB9505D0A`. The log output also indicates the time taken on the call as well as the response data returned on call completion.

## Backing Up and Restoring Data

Follow these instructions to back up  and restore your DeployR data or to reset the database to its initial post-installation state.

### On Linux/OS X

Follow these steps to back up the data in the database used by DeployR.

1.  Log into the machine as a user with administrator privileges.

2.  Ensure all users are logged out of DeployR. As admin, you can always check the grid activity in the **Grid** tab of the Administration Console.

3.  Run the database utility script as follows:

         cd /home/deployr-user/deployr/8.0.0/deployr/tools
        ./databaseUtils.sh

4.  When prompted by the script:
    -  To reinitialize the database, choose option `2`.
    -  To back up the database, choose option `3` and enter the path in which the database backup should be saved. Verify that the backup was successful by checking the contents of the directory where you dumped the database.
    -  To restore the database, choose option `4` and enter the path to your backup folder. By default, the backup path includes a date stamped folder and ends with the folder `deployr`.

### On Windows

Follow these steps to back up the data in the database used by DeployR.

1. Log into the machine as a user with administrator privileges.

1. Ensure all users are logged out of DeployR. As admin, you can always check the grid activity in the **Grid** tab of the Administration Console.

1.  Run the database utility script as follows:

          cd C:\Program Files\Microsoft\DeployR\8.0\deployr\tools
          databaseUtils.bat

1.  When prompted by the script:
    -  To reinitialize the database, choose option `2`.
    -  To back up the database, choose option `3` and enter the path in which the database backup should be saved. Verify that the backup was successful by checking the contents of the directory where you dumped the database.
    -  To restore the database, choose option `4` and enter the path to your backup folder. By default, the backup path includes a date stamped folder and ends with the folder `deployr`.


## Opening DeployR Ports

### For Linux / Mac OS X

If you are using the IPTABLES firewall or equivalent service for your server, use the `iptables` command (or equivalent command/tool) to open the following ports:

>[!IMPORTANT]
>If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints), [AWS](deployr-admin-configure-for-aws.md) or VirtualBox, then you must also configure endpoints for these ports (or use port-forwarding for VirtualBox).

| Machine                                 | Ports                                 | Open Ports                                                                                      |
|-----------------------------------------|---------------------------------------|-------------------------------------------------------------------------------------------------|
| DeployR server machine                  | Tomcat ports:<br />- `8000` (Tomcat default port)<br />- `8001` (Tomcat HTTPS port)           | To the outside                                                                                  |
| DeployR server machine                  | - `8006` (DeployR event console port) | To the public IP of DeployR server AND to the public IP of *each* grid node machine<sup>1</sup> |
| Remote grid node machines <sup>1</sup>  | RServe ports:<br />- `8004` (RServe connection port)<br />- `8005` (RServe cancel port)          | To the public IP of the DeployR server                                                          |
| Remote MongoDB host machine<sup>2</sup> | - `8003` (MongoDB port)               | To the public IP of the DeployR server                                                          |

<sup>1</sup> Only DeployR Enterprise offers the ability to expand your Grid framework for load distribution by [installing and configuring additional grid nodes](deployr-installing-configuring.md#grid-node-install).

<sup>2</sup> Only DeployR Enterprise on Linux offers the ability to [install a remote MongoDB database](deployr-installing-configuring.md#deployr-install-with-remote-database).

### For Windows

During installation, the Windows firewall was updated to allow inbound communications to DeployR on the ports listed in the following table.

>[!IMPORTANT]
>If you are provisioning your server on a cloud service such as [Azure](deployr-admin-configure-for-azure.md#configuring-azure-endpoints), [AWS](deployr-admin-configure-for-aws.md) or VirtualBox, then you must also configure endpoints for these ports (or use port-forwarding for VirtualBox).

| Machine                                | Ports                                 | Open Ports                                                                                      |
|----------------------------------------|---------------------------------------|-------------------------------------------------------------------------------------------------|
| DeployR server machine                 | Tomcat ports:<br />- `8000` (Tomcat default port)<br />- `8001` (Tomcat HTTPS port)           | To the outside                                                                                  |
| DeployR server machine                 | - `8006` (DeployR event console port) | To the public IP of DeployR server AND to the public IP of *each* grid node machine<sup>1</sup> |
| Remote grid node machines <sup>1</sup> | RServe ports:<br />- `8004` (RServe connection port)<br />- `8005` (RServe cancel port)          | To the public IP of the DeployR server                                                          |

<sup>1</sup> Only DeployR Enterprise offers the ability to expand your Grid framework for load distribution by [installing and configuring additional grid nodes](deployr-installing-configuring.md#grid-node-install).

>**Notes:**
>-   If you have made any changes to the port numbers designated for communications between DeployR and any of its dependencies, add those port numbers instead.
>-   **For NFS ports** for external directory support, see the Configuration section of the [Managing External Directories for Big Data](deployr-admin-manage-big-data.md#setting-up-nfs-setup) document.

