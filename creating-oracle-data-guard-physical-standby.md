##  3Creating a Physical Standby Database {#GUID-B511FB6E-E3E7-436D-94B5-071C37550170} 

You can manually create a physical standby database in maximum performance mode using asynchronous redo transport and real-time apply, the default Oracle Data Guard configuration. 

> **note:** 

A multitenant container database is the only supported architecture in Oracle Database 21c and later releases. While the documentation is being revised, legacy terminology may persist. In most cases, "database" and "non-CDB" refer to a CDB or PDB, depending on context. In some contexts, such as upgrades, "non-CDB" refers to a non-CDB from a previous release. 

See the following main topics: 

  * [ Creating a Physical Standby of a CDB ](creating-oracle-data-guard-physical-standby.md#GUID-D5F0EB8F-8534-462A-911C-216E6DC7CE69)

  * [ Preparing the Primary Database for Standby Database Creation ](creating-oracle-data-guard-physical-standby.md#GUID-3DA747A3-1976-4A70-A96A-56281872D4D9)

  * [ Step-by-Step Instructions for Creating a Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-A86D403B-B3F5-4B05-9255-5BADD513F9CF)

  * [ Post-Creation Steps ](creating-oracle-data-guard-physical-standby.md#GUID-3A0F8F9C-EF69-4BE4-9F7B-98AED4BEC4AC)

  * [ Using DBCA to Create a Data Guard Standby ](creating-oracle-data-guard-physical-standby.md#GUID-7C55FE8A-50C3-4601-9ADA-98BE6D65F1DD)

  * [ Creating a PDB in a Primary Database ](creating-oracle-data-guard-physical-standby.md#GUID-20D2DEAC-0355-4FBB-ADB8-37BF5F4328E2)




> **note:** See Also: 

  * [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN11117) for information about creating and using server parameter files 

  * Enterprise Manager online help system for information about using the Oracle Data Guard broker graphical user interface (GUI) to automatically create a physical standby database 

  * [ Creating a Standby Database with Recovery Manager ](creating-data-guard-standby-database-using-RMAN.md#GUID-82731D59-A20F-45DD-A235-267B3B0E38C5) for information about alternative methods of creating a physical standby database that automate much of the process by using Oracle Recovery Manager (RMAN) and either backup based duplication or active duplication over a network 

  * [ *Oracle Data Guard Broker* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR090) for information about configuring a database so that it can be managed by Oracle Data Guard broker 




###  About Creating a Physical Standby in a Multitenant Environment {#GUID-D5F0EB8F-8534-462A-911C-216E6DC7CE69} 

Learn about how physical standby databases work in a multitenant environment. 

Be aware of the following when you create a multitentant container database (CDB) as a physical standby. 

  * You must have the ` CDB_DBA ` role. 

  * Oracle recommends that the standby database have its own keystore. 

  * If you execute a switchover or failover operation, the entire CDB undergoes the role change. If you used the ` ENABLED_PDBS_ON_STANDBY ` intialization parameter, then be aware of the possibility that not every PDB is present in both the primary and the standby databases. 

  * The database role is defined at the CDB level, not at the individual PDB level, unless Oracle Data Guard for Pluggable Database (DG PDB) is configured using Oracle Data Guard broker. 

  * Any DDL related to role changes must be executed in the root container because a role is associated with an entire CDB. Individual pluggable databases (PDBs) do not have their own roles unless Oracle Data Guard for Pluggable Database (DG PDB) is configured using Oracle Data Guard broker . 

  * The ` ALTER DATABASE RECOVER MANAGED STANDBY ` command functions only in the root container; it is not allowed in a PDB. 

  * A role is associated with an entire CDB; individual PDBs do not have their own roles do not have their own roles (unless configured with Oracle Data Guard for Pluggable Database (DG PDB)). Therefore, the following role change DDL associated with physical standbys affect the entire CDB: 

` ALTER DATABASE SWITCHOVER TO target_db_name `

` ALTER DATABASE ACTIVATE PHYSICAL STANDBY `

  * The ` ALTER PLUGGABLE DATABASE [OPEN|CLOSE] ` SQL statement is supported on the standby, provided you have already opened the root container. 

  * The ` ALTER PLUGGABLE DATABASE RECOVER ` statement is not supported on the standby. (Standby recovery is always at the CDB level.) 

  * Many DBA views have analogous CDB views that you should use instead. 

  * The redo must be shipped to the root container of the standby database. 

The following is an example of how to determine whether redo is being shipped to the root container. Suppose your primary database has the following settings: 
    
        ```
    LOG_ARCHIVE_DEST_2='SERVICE=boston ASYNC VALID_FOR=(ONLINE_LOGFILES,
    PRIMARY_ROLE) DB_UNIQUE_NAME=boston'
    
    ```

Redo is being shipped to ` boston ` . The container ID ( ` CON_ID ` ) for the root container is always 1, so you must make sure that the ` CON_ID ` is 1 for the service ` boston ` . To do this, check the service name in the ` tnsnames.ora ` file. For example: 
    
        ```
    boston = (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=boston-server)(PORT=1521))
    (CONNECT_DATA=(SERVICE_NAME=boston.us.example.com))
    
    ```

The service name for ` boston ` is ` boston.us.example.com ` . 

On the standby database, query the ` CDB_SERVICES ` view to determine the ` CON_ID ` . For example: 
    
        ```
    SQL> SELECT NAME, CON_ID FROM CDB_SERVICES;
    
    NAME                                 CON_ID
    ---------------------------------------------
    boston.us.example.com                 1
    
    ```

The query result shows that the ` CON_ID ` for ` boston ` is 1. 




> **note:** See Also: 

  * [ *Oracle Database Advanced Security Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ASOAG10284) for more information about creating keystores 




###  Preparing the Primary Database for Standby Database Creation {#GUID-3DA747A3-1976-4A70-A96A-56281872D4D9} 

Before you create a standby database you must first ensure the primary database is properly configured. 

Perform the following tasks on the primary database to prepare for physical standby database creation: 

  * [ Enable an Appropriate Logging Mode ](creating-oracle-data-guard-physical-standby.md#GUID-AE2B1237-57A1-4745-A04C-4246A831A963)

  * [ Configure Redo Transport Authentication ](creating-oracle-data-guard-physical-standby.md#GUID-0DC30726-3471-4588-BFE0-9CA0736328E2)

  * [ Configure the Primary Database to Receive Redo Data ](creating-oracle-data-guard-physical-standby.md#GUID-CA9076B9-3360-4A36-87AF-3390B391CEFF)

  * [ Set Primary Database Initialization Parameters ](creating-oracle-data-guard-physical-standby.md#GUID-0FD317D6-1A49-4DB3-A141-D00CB3089E84)

  * [ Enable Archiving ](creating-oracle-data-guard-physical-standby.md#GUID-0379019D-0995-4EB7-8861-B07B708C7B4C)




> **note:** 

Perform these preparatory tasks only once. After you complete these steps, the database is prepared to serve as the primary database for one or more standby databases. 

####  Enable an Appropriate Logging Mode {#GUID-AE2B1237-57A1-4745-A04C-4246A831A963} 

As part of preparing the primary database for standby database creation, you must enable a logging mode appropriate to the way you plan to use the Oracle Data Guard configuration. 

The default logging mode of a database that is not part of an Oracle Data Guard configuration allows certain data loading operations to be performed in a nonlogged manner. This default mode is not appropriate to a database with a standby, because it leads to the loaded data being missing from the standby, which requires manual intervention to fix. 

In addition to the default logging mode, there are three other modes that are appropriate for a primary database: 

  * ` FORCE LOGGING ` mode prevents any load operation from being performed in a nonlogged manner. This mode can slow down the load process, because the loaded data must be copied into the redo logs. ` FORCE LOGGING ` mode is enabled using the following command: 
    
        ```
    SQL> ALTER DATABASE FORCE LOGGING;
    ```

  * ` STANDBY NOLOGGING FOR DATA AVAILABILITY ` mode causes the load operation to send the loaded data to each standby through its own connection to the standby. The commit is delayed until all the standbys have applied the data as part of running managed recovery in an Active Data Guard environment. It is enabled with the following command: 
    
        ```
    SQL> ALTER DATABASE SET STANDBY NOLOGGING FOR DATA AVAILABILITY;
    ```

  * ` STANDBY NOLOGGING FOR LOAD PERFORMANCE ` is similar to the previous mode except that the loading process can stop sending the data to the standbys if the network cannot keep up with the speed at which data is being loaded to the primary. In this mode it is possible that the standbys may have missing data, but each standby automatically fetches the data from the primary as a normal part of running managed recovery in an Active Data Guard environment. It is enabled with the following command: 
    
        ```
    SQL> ALTER DATABASE SET STANDBY NOLOGGING FOR LOAD PERFORMANCE;
    
    
    ```




When you issue any of these statements, the primary database must at least be mounted (and it can also be open). The statement can take a considerable amount of time to complete, because it waits for all unlogged direct write I/O to finish. 

> **note:** 

When you enable ` STANDBY NOLOGGING FOR DATA AVAILABILITY ` or ` STANDBY NOLOGGING FOR LOAD PERFORMANCE ` on the primary database, any standbys that are using multi-instance redo apply functionality will stop applying redo with the error ` ORA-10892 ` . You must first restart redo apply and allow the affected standbys to progress past the NOLOGGING operation period and then enable multi-instance redo apply. 

**Related Topics**

  * [ Specifying FORCE LOGGING Mode ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=MULTI-GUID-ACEA5B83-D70F-4F30-A85B-3030248F597B)



> **note:** See Also:  *Oracle Database Administrator’s Guide*  For more information about the ramifications of specifying ` FORCE LOGGING ` mode 

####  Configure Redo Transport Authentication {#GUID-0DC30726-3471-4588-BFE0-9CA0736328E2} 

Oracle Data Guard uses Oracle Net sessions to transport redo data and control messages between the members of an Oracle Data Guard configuration. 

These redo transport sessions are authenticated using either the Secure Sockets Layer (SSL) protocol or a remote login password file. 

SSL is used to authenticate redo transport sessions between two databases if: 

  * The databases are members of the same Oracle Internet Directory (OID) enterprise domain and it allows the use of current user database links 

  * The ` LOG_ARCHIVE_DEST_ ` *n* , and ` FAL_SERVER ` database initialization parameters that correspond to the databases use Oracle Net connect descriptors configured for SSL 

  * Each database has an Oracle wallet or supported hardware security module that contains a user certificate with a distinguished name (DN) that matches the DN in the OID entry for the database 




If the SSL authentication requirements are not met, then each member of an Oracle Data Guard configuration must be configured to use a remote login password file and every physical standby database in the configuration must have an up-to-date copy of the password file from the primary database. 

> **note:** 

As of Oracle Database 12 *c* Release 2 (12.2.0.1) password file changes done on a primary database are automatically propagated to standby databases. The only exception to this is far sync instances. Updated password files must still be manually copied to far sync instances because far sync instances receive redo, but do not apply it. Once the password file is up-to-date at the far sync instance, the redo is automatically propagated to any standby databases that are configured to receive redo logs from that far sync instance. The password file is updated on the standby when the redo is applied. 

> **note:** See Also: 

  * [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN-GUID-26EC8D1B-7A2F-4F0A-9588-D92CBD610858) and [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10184) for more information about remote login password files 

  * [ *Oracle Database Security Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DBSEG070) for more information about SSL 

  * [ *Oracle Database Net Services Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG001) for more information about Oracle Net Services 




####  Configure the Primary Database to Receive Redo Data {#GUID-CA9076B9-3360-4A36-87AF-3390B391CEFF} 

It is a best practice to configure the primary database to receive redo if this is the first time a standby database is added to the configuration. 

The primary database can then quickly transition to the standby role and begin receiving redo data, if necessary. 

To create a standby redo log, use the SQL ` ALTER ` ` DATABASE ` ` ADD ` ` STANDBY ` ` LOGFILE ` statement. For example: 
    
    
    ```
    SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/oracle/dbs/slog1.rdo') SIZE 500M;
     
    SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/oracle/dbs/slog2.rdo') SIZE 500M;
    
    ```

See [ Configuring an Oracle Database to Receive Redo Data ](oracle-data-guard-redo-transport-services.md#GUID-AF64C4FE-2D16-4197-953A-25930F36CFA8) for a discussion of how to determine the size of each log file and the number of log groups, as well as other background information about managing standby redo logs. 

####  Set Primary Database Initialization Parameters {#GUID-0FD317D6-1A49-4DB3-A141-D00CB3089E84} 

On the primary database, you define initialization parameters that control redo transport services while the database is in the primary role. 

There are additional parameters you need to add that control the receipt of the redo data and apply services when the primary database is transitioned to the standby role. 

The following example shows the primary role initialization parameters that you maintain on the primary database. This example represents an Oracle Data Guard configuration with a primary database located in Chicago and one physical standby database located in Boston. The parameters shown in this example are valid for the Chicago database when it is running in either the primary or the standby database role. The configuration examples use the names shown in the following table: 

Database  |  DB_UNIQUE_NAME  |  Oracle Net Service Name   
---|---|---  
Primary  |  chicago  |  chicago   
Physical standby  |  boston  |  boston   
      
    
    ```
    DB_NAME=chicago
    DB_UNIQUE_NAME=chicago
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,boston)'
    CONTROL_FILES='/arch1/chicago/control1.ctl', '/arch2/chicago/control2.ctl'
    LOG_ARCHIVE_DEST_1=
     'LOCATION=USE_DB_RECOVERY_FILE_DEST 
      VALID_FOR=(ALL_LOGFILES,ALL_ROLES)
      DB_UNIQUE_NAME=chicago'
    LOG_ARCHIVE_DEST_2=
     'SERVICE=boston ASYNC
      VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) 
      DB_UNIQUE_NAME=boston'
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    
    ```

These parameters control how redo transport services transmit redo data to the standby system and the archiving of redo data on the local file system. Note that the example specifies asynchronous ( ` ASYNC ` ) network transmission to transmit redo data on the ` LOG_ARCHIVE_DEST_2 ` initialization parameter. These are the recommended settings and require standby redo log files (see [ Configure the Primary Database to Receive Redo Data ](creating-oracle-data-guard-physical-standby.md#GUID-CA9076B9-3360-4A36-87AF-3390B391CEFF) ). 

The following shows the additional standby role initialization parameters on the primary database. These parameters take effect when the primary database is transitioned to the standby role. 
    
    
    ```
    FAL_SERVER=boston
    DB_FILE_NAME_CONVERT='/boston/','/chicago/'
    LOG_FILE_NAME_CONVERT='/boston/','/chicago/' 
    STANDBY_FILE_MANAGEMENT=AUTO
    
    ```

Specifying the initialization parameters shown above sets up the primary database to resolve gaps, converts new data file and log file path names from a new primary database, and archives the incoming redo data when this database is in the standby role. With the initialization parameters for both the primary and standby roles set as described, none of the parameters need to change after a role transition. 

The following table provides a brief explanation about each parameter setting shown in the previous two examples. 

Parameter  |  Recommended Setting   
---|---  
` DB_NAME ` |  On a primary database, specify the name used when the database was created. On a physical standby database, use the ` DB_NAME ` of the primary database.   
` DB_UNIQUE_NAME ` |  Specify a unique name for each database. This name stays with the database and does not change, even if the primary and standby databases reverse roles.   
` LOG_ARCHIVE_CONFIG ` |  The ` DG_CONFIG ` attribute of this parameter must be explicitly set on each database in an Oracle Data Guard configuration to enable full Oracle Data Guard functionality. Set ` DG_CONFIG ` to a text string that contains the ` DB_UNIQUE_NAME ` of each database in the configuration, with each name in this list separated by a comma.   
` CONTROL_FILES ` |  Specify the path name for the control files on the primary database. It is recommended that a second copy of the control file is available so an instance can be easily restarted after copying the good control file to the location of the bad control file.   
` LOG_ARCHIVE_DEST_n ` |  Specify where the redo data is to be archived on the primary and standby systems.  <ul>

<li>
  * ` LOG_ARCHIVE_DEST_1 ` archives redo data generated by the primary database from the local online redo log files to the local archived redo log files in /arch1/chicago/.  </li> <li>
  * ` LOG_ARCHIVE_DEST_2 ` is valid only for the primary role. This destination transmits redo data to the remote physical standby destination ` boston ` .  </li> </ul> Note:  If a fast recovery area was configured (with the ` DB_RECOVERY_FILE_DEST ` initialization parameter) and you have not explicitly configured a local archiving destination with the ` LOCATION ` attribute, Oracle Data Guard automatically uses the ` LOG_ARCHIVE_DEST_1 ` initialization parameter (if it has not already been set) as the default destination for local archiving. Also, see [ LOG_ARCHIVE_DEST_n Parameter Attributes ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B04FC22C-F0B0-440F-BAF4-182EE547FCC1) for complete ` LOG_ARCHIVE_DEST_ ` *n* information.   
` REMOTE_LOGIN_PASSWORDFILE ` |  This parameter must be set to ` EXCLUSIVE ` or ` SHARED ` if a remote login password file is used to authenticate administrative users or redo transport sessions.   
` LOG_ARCHIVE_FORMAT ` |  Specify the format for the archived redo log files using a thread (%t), sequence number (%s), and resetlogs ID (%r).   
` FAL_SERVER ` |  Specify the Oracle Net service name of the FAL server (typically this is the database running in the primary role). When the Chicago database is running in the standby role, it uses the Boston database as the FAL server from which to fetch (request) missing archived redo log files if Boston is unable to automatically send the missing log files.   
` DB_FILE_NAME_CONVERT ` |  Specify the path name and filename location of the standby database data files followed by the primary location. This parameter converts the path names of the primary database data files to the standby data file path names. This parameter is used only to convert path names for physical standby databases. Multiple pairs of paths may be specified by this parameter.   
` LOG_FILE_NAME_CONVERT ` |  Specify the location of the standby database online redo log files followed by the primary location. This parameter converts the path names of the primary database log files to the path names on the standby database. Multiple pairs of paths may be specified by this parameter.   
` STANDBY_FILE_MANAGEMENT ` |  Set to ` AUTO ` so when data files are added to or dropped from the primary database, corresponding changes are made automatically to the standby database.   
  
> **note:** 

Review the initialization parameter file for additional parameters that may need to be modified. For example, you may need to modify the dump destination parameters if the directory location on the standby database is different from those specified on the primary database. 

####  Enable Archiving {#GUID-0379019D-0995-4EB7-8861-B07B708C7B4C} 

If archiving is not enabled, then you must put the primary database in ` ARCHIVELOG ` mode and enable automatic archiving. 

Issue the following SQL statements: 
    
        ```
    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    
    ```

> **note:** The standby database must be in ` ARCHIVELOG ` mode for standby redo log archival to be performed. 

See [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN008) for information about archiving. 

###  Step-by-Step Instructions for Creating a Physical Standby Database {#GUID-A86D403B-B3F5-4B05-9255-5BADD513F9CF} 

These are the tasks you perform to create a physical standby database. 

The information in this topic is written at a level of detail that requires you to already have a thorough understanding of the following topics: 

    * Database administrator authentication 

    * Database initialization parameters 

    * Managing redo logs, data files, and control files 

    * Managing archived redo logs 

    * Fast recovery areas 

    * Oracle Net configuration 

[ Table 3-1 ](creating-oracle-data-guard-physical-standby.md#GUID-A86D403B-B3F5-4B05-9255-5BADD513F9CF__BACHCEHE) provides a checklist of the tasks that you perform to create a physical standby database and the database or databases on which you perform each task. 

**Table: Creating a Physical Standby Database** 

Task  |  Database   
---|---  
[ Create a Backup Copy of the Primary Database Data Files ](creating-oracle-data-guard-physical-standby.md#GUID-C40DD1FB-1DB5-4674-839D-D17ECA99FB93) |  Primary   
[ Create a Control File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269) |  Primary   
[ Create a Parameter File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA) |  Primary   
[ Copy Files from the Primary System to the Standby System ](creating-oracle-data-guard-physical-standby.md#GUID-4834C26E-6493-4C5A-91F7-680FD43FADF7) |  Primary   
[ Set Up the Environment to Support the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-2FD8DBAB-C439-4F03-9A94-01C6FEF33392) |  Standby   
[ Start the Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569) |  Standby   
[ Verify the Physical Standby Database Is Performing Properly ](creating-oracle-data-guard-physical-standby.md#GUID-AAA6D97B-A345-4825-A320-B662BB16E2ED) |  Standby   
  
> **note:** RMAN can also be used to directly to create a standby database. See My Oracle Support note 1075908.1 Step by Step Guide on Creating Physical Standby Using RMAN... at [ ` http://support.oracle.com ` ](https://support.oracle.com/knowledge/Oracle%20Database%20Products/1075908_1.md?) for additional information 

####  Creating a Physical Standby Task 1: Create a Backup Copy of the Primary Database Data Files {#GUID-C40DD1FB-1DB5-4674-839D-D17ECA99FB93} 

You can use any backup copy of the primary database to create the physical standby database, as long as you have the necessary archived redo log files to completely recover the database. 

Oracle recommends that you use the Recovery Manager utility ( ` RMAN ` ). 

**Related Topics**

    * [ Backing Up the Database,  *Oracle Database Backup and Recovery User’s Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV-GUID-93BAB347-063F-439E-BDF3-109AB8D1F8E7)

####  Creating a Physical Standby Task 2: Create a Control File for the Standby Database {#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269} 

Create the control file for the standby database. The primary database does not have to be open, but it must at least be mounted. 

You must create a control file for the standby database. You cannot use a single control file for both the primary and standby databases. They each must have their own file. 

Example 3-1 Creating the Control File for the Standby Database 

The ` ALTER DATABASE ` command designates the database that you want to operate in the standby role. In this example, that standby database is named ` boston ` : 
    
        ```
    SQL> ALTER DATABASE CREATE STANDBY CONTROLFILE AS '/tmp/boston.ctl';
    
    ```

> **note:** 

If a control file backup is taken on the primary, and restored on a standby (or vice-versa), then the location of the snapshot control file on the restored system is configured to be the default. The default value for the snapshot control file name is platform-specific, and dependent on the Oracle home. Manually reconfigure it to the correct value by using the RMAN command ` CONFIGURE SNAPSHOT CONTROLFILE ` . For more information see [ *Oracle Globally Distributed Database Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF-GUID-B5094E73-C26C-4FED-AE39-8C2E9540050A)

####  Creating a Physical Standby Task 3: Create a Parameter File for the Standby Database {#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA} 

Create a parameter file ( ` PFILE ` ) from the server parameter file ( ` SPFILE ` ) used by the primary database. 

To create a parameter file for the standby database, perform the following steps: 

    1. On the primary database, issue a SQL statement to create a copy of the primary database parameter file. 

In the following example, 
        
                ```
        SQL> CREATE PFILE='/tmp/initboston.ora' FROM SPFILE;
        
        ```

    2. Modify the parameter values in the copy parameter file as needed to use this copy as the parameter file for the standby database. 

Although most of the initialization parameter settings in the parameter file are also appropriate for the physical standby database, some modifications must be made. 

Example 3-2 Modifying Initialization Parameters for a Physical Standby Database 

This example shows the parameters created earlier on the primary that must be changed. The parameters that must be changed for the physical standby database are shown in bold typeface. Any parameter, such as DB_NAME, that is not shown in bold typeface should retain the same value on the standby database that it has on the primary database. 
    
        ```
    .
    .
    .
    DB_NAME=chicago
    DB_UNIQUE_NAME=boston
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,boston)'
    CONTROL_FILES='/arch1/boston/control1.ctl', '/arch2/boston/control2.ctl'
    DB_FILE_NAME_CONVERT='/chicago/','/boston/'
    LOG_FILE_NAME_CONVERT='/chicago/','/boston/'
    LOG_ARCHIVE_FORMAT=log%t_%s_%r.arc
    LOG_ARCHIVE_DEST_1=
     'LOCATION=USE_DB_RECOVERY_FILE_DEST
      VALID_FOR=(ALL_LOGFILES,ALL_ROLES) 
      DB_UNIQUE_NAME=boston'
    LOG_ARCHIVE_DEST_2=
     'SERVICE=chicago ASYNC
      VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) 
      DB_UNIQUE_NAME=chicago'
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    STANDBY_FILE_MANAGEMENT=AUTO
    FAL_SERVER=chicago
    .
    .
    .
    ```

Ensure the ` COMPATIBLE ` initialization parameter is set to the same value on both the primary and standby databases. If the values differ, then redo transport services may be unable to transmit redo data from the primary database to the standby databases. 

It is always a good practice to use the ` SHOW PARAMETERS ` command to verify that no other parameters need to be changed. 

The following table provides a brief explanation about the parameter settings shown in that have different settings from the primary database. 

Parameter  |  Recommended Setting   
---|---  
` DB_UNIQUE_NAME ` |  Specify a unique name for this database. This name uniquely identifies this database, and does not change even if the primary and standby databases reverse roles.   
` CONTROL_FILES ` |  Specify the path name for the control files on the standby database. The example in this topic shows how to specify the path name for two control files. Oracle recommends that you ensure a copy of the control file is available, so that if a control file is corrupted, an instance can be easily restarted after copying the good control file to the location of the bad control file.   
` DB_FILE_NAME_CONVERT ` |  Specify the path name and filename location of the primary database data files, followed by the standby location. The ` CONTROL_FILES ` parameter converts the path names of the primary database data files to the standby data file path names.   
` LOG_FILE_NAME_CONVERT ` |  Specify the location of the primary database online redo log files followed by the standby location. This parameter converts the path names of the primary database log files to the path names on the standby database.   
` LOG_ARCHIVE_DEST_ ` *n*  |  Specify where the redo data is to be archived. In the example in this topic, the following destinations are specified:  <ul> <li>
    * ` LOG_ARCHIVE_DEST_1 ` archives redo data received from the primary database to archived redo log files in ` /arch1/boston/ ` .  </li> <li>
    * ` LOG_ARCHIVE_DEST_2 ` is currently ignored, because this destination is valid only for the primary role. If a switchover occurs, and this instance becomes the primary database, then this parameter specification provides the path to transmit redo data to the remote Chicago destination.  </li> </ul> Note:  If a fast recovery area was configured (using the ` DB_RECOVERY_FILE_DEST ` initialization parameter), and you have not explicitly configured a local archiving destination with the ` LOCATION ` attribute, then Oracle Data Guard automatically uses the ` LOG_ARCHIVE_DEST_1 ` initialization parameter (if it has not already been set) as the default destination for local archiving.   
` FAL_SERVER ` |  Specify the Oracle Net service name of the FAL (fetch archive log) server for a standby database. Typically, this service name is for the database running in the primary role. When the Boston database is running in the standby role, it uses the Chicago database as the FAL server from which to fetch (request) missing archived redo log files, if Chicago is unable to automatically send the missing log files.   
  
> **note:** 

Review the initialization parameter file for additional parameters that may need to be modified. For example, you may need to modify the dump destination parameters if the directory location on the standby database is different from those specified on the primary database. 

**Related Topics**

      * [ LOG_ARCHIVE_DEST_n Parameter Attributes ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SBYDB-GUID-B04FC22C-F0B0-440F-BAF4-182EE547FCC1)

####  Creating a Physical Standby Task 4: Copy Files from the Primary System to the Standby System {#GUID-4834C26E-6493-4C5A-91F7-680FD43FADF7} 

Ensure that all required directories are created. Use an operating system copy utility to copy binary files from the primary system to their correct locations on the standby system. 

Copy these binary files to the correct locations on the standby system: 

      1. The primary Oracle Database backup. 

See [ Create a Backup Copy of the Primary Database Data Files ](creating-oracle-data-guard-physical-standby.md#GUID-C40DD1FB-1DB5-4674-839D-D17ECA99FB93)

      2. the standby control file. 

See [ Create a Control File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269)

      3. Standby database initialization parameter file. 

See [ Create a Parameter File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA)

####  Creating a Physical Standby Task 5: Set Up the Environment to Support the Standby Database {#GUID-2FD8DBAB-C439-4F03-9A94-01C6FEF33392} 

Set up the environment by creating a Microsoft Windows-based service, a password file, and an ` SPFILE ` , and then setting up the Oracle Net environment. 

To set up the environment, perform the following steps: 

      1. If the standby database is going to be hosted on a Microsoft Windows system, then use the ` ORADIM ` utility to create a Windows service. 

For example: 
            
                        ```
            oradim –NEW –SID boston –STARTMODE manual
            
            ```

The ` ORADIM ` utility automatically determines the username for which this service should be created and prompts for a password for that username (if that username needs a password). 

See [ *Oracle Database Administrator’s Reference for Microsoft Windows*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NTQRF-GUID-862FBD9C-EB31-4C58-9D81-567110FE8DA5) for more information about using the ` ORADIM ` utility. 

      2. Copy the remote login password file from the primary database system to the standby system and rename it appropriately (for example, rename the copied orapwchicago to orapwboston). 

This step is optional if operating system authentication is used for administrative users, and if SSL is used for redo transport authentication. If that is not the case, then copy the remote login password file from the primary database to the appropriate directory on the physical standby database system. 

Any subsequent changes to the password file on the primary are automatically propagated to the standby. Changes to a password file can include when administrative privileges ( ` SYSDG ` , ` SYSOPER ` , ` SYSDBA ` , and so on) are granted or revoked, and when passwords of any user with administrative privileges is changed. Updated password files must still be manually copied to far sync instances because far sync instances receive redo, but do not apply it. Once the password file is up-to-date at the far sync instance, the redo containing the password update at the primary is automatically propagated to any standby databases that are set up to receive redo from that far sync instance. The password file is updated on the standby when the redo is applied. 

      3. Configure and start a listener on the standby system if one is not already configured. 

See [ Configuring and Administering Oracle Net Listener ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG-GUID-C3C40DBA-4282-41E1-9562-4B8B10947C4E) in  *Oracle Database Net Services Administrator's Guide*  . 

      4. Create Oracle Net service names. 

On both the primary and standby systems, use Oracle Net Manager to create a network service name for the primary and standby databases that are to be used by redo transport services. The Net service names in this example are ` chicago ` and ` boston ` . 

The Oracle Net service name must resolve to a connect descriptor that uses the same protocol, host address, port, and service that you specified when you configured the listeners for the primary and standby databases. The connect descriptor must also specify that a dedicated server be used. 

See [ Understanding Database Services ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG-GUID-153861C1-16AD-41EC-A179-074146B722E6) in  *Oracle Database Net Services Administrator's Guide*  for more information about service names. 

      5. On an idle standby database, use the SQL ` CREATE ` statement to create a server parameter file for the standby database from the text initialization parameter file that was edited in Task 3. 

For example: 
            
                        ```
            SQL> CREATE SPFILE FROM PFILE='initboston.ora';
            
            ```

      6. If the primary database has a database encryption wallet, then copy it to the standby database system and configure the standby database to use this wallet. 

> **note:** 

The database encryption wallet must be copied from the primary database system to each standby database system whenever the primary encryption key is updated. 

Encrypted data in a standby database cannot be accessed unless the standby database is configured to point to a database encryption wallet or hardware security module that contains the current primary encryption key from the primary database. 

####  Creating a Physical Standby Task 6: Start the Physical Standby Database {#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569} 

These are the steps to start the physical standby database and Redo Apply. 

      1. On the standby database, issue the following SQL statement to start and mount the database: 
            
                        ```
            SQL> STARTUP MOUNT;
            
            ```

      2. Restore the backup of the data files taken from the primary database data files, and copied to the standby system. 
      3. On the standby database, issue the following command to start Redo Apply: 
            
                        ```
            SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE - 
            > DISCONNECT FROM SESSION;
            
            ```

The statement includes the ` DISCONNECT FROM SESSION ` option, so that Redo Apply runs in a background session. 

####  Creating a Physical Standby Task 7: Verify the Physical Standby Database Is Performing Properly {#GUID-AAA6D97B-A345-4825-A320-B662BB16E2ED} 

After you create the physical standby database and set up redo transport services, you may want to verify database modifications are being successfully transmitted from the primary database to the standby database. 

To verify that redo is being transmitted from the primary database and applied to the standby database, connect to the standby database, and query the ` V$DATAGUARD_PROCESS ` view. 

Example 3-3 Querying V$DATAGUARD_PROCESS to Verify Redo Transmission from Primary to Secondary Database 
        
                ```
        SQL> SELECT ROLE, THREAD#, SEQUENCE#, ACTION FROM V$DATAGUARD_PROCESS;
        
        
        ROLE                        THREAD#  SEQUENCE# ACTION
        ------------------------ ---------- ---------- ------------
        RFS ping                          1          9 IDLE
        recovery apply applier            0          0 IDLE
        recovery apply applier            0          0 IDLE
        managed recovery                  0          0 IDLE
        recovery logmerger                1          9 APPLYING_LOG
        RFS archive                       0          0 IDLE
        RFS async                         1          9 IDLE
        ```

The ` recovery logmerger ` role shows that redo is being applied at the standby. 

> **note:** 

Use the ` V$DATAGUARD_PROCESS ` view instead of the ` V$MANAGED_STANDBY ` view. ` V$MANAGED_STANDBY ` was deprecated in Oracle Database 12c Release 2 (12.2.0.1) and can be desupported in a future release. 

###  Creating a Physical Standby: Post-Creation Steps {#GUID-3A0F8F9C-EF69-4BE4-9F7B-98AED4BEC4AC} 

After the physical standby database is running, you can upgrade the data protection mode, and you can enable Flashback Database. 

      * Upgrade the data protection mode 

[ Oracle Data Guard Protection Modes ](oracle-data-guard-protection-modes.md#GUID-16298839-9B8A-4E58-8EC9-27912E57F1CE) provides information about configuring the different data protection modes. 

      * Enable Flashback Database 

Flashback Database removes the need to re-create the primary database after a failover. Flashback Database enables you to return a database to its state at a time in the recent past much faster than traditional point-in-time recovery, because it does not require restoring data files from backup nor the extensive application of redo data. You can enable Flashback Database on the primary database, the standby database, or both. 

> **note:** See Also: 
      * [ Converting a Failed Primary Into a Standby Database Using Flashback Database ](examples-of-using-oracle-data-guard.md#GUID-1163448F-6B18-4A44-AA8D-7CDF0D1360FB) and [ Using Flashback Database After Issuing an Open Resetlogs Statement ](examples-of-using-oracle-data-guard.md#GUID-BAA5ED38-29A0-4E8D-8435-AA083D19C13E) for scenarios showing how to use Flashback Database in an Oracle Data Guard environment 

      * [ *Oracle Database Backup and Recovery User's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV71000) for more information about Flashback Database 

###  Using DBCA to Create a Data Guard Standby {#GUID-7C55FE8A-50C3-4601-9ADA-98BE6D65F1DD} 

The Database Configuration Assistant (DBCA) can also be used as a simple command-line method to create an Oracle Data Guard physical standby database. 

The DBCA command qualifier used to create the physical standby database is ` createDuplicateDB ` . 

The basic ` createDuplicateDB ` command has the following syntax: 
        
                ```
        dbca -createDuplicateDB 
            -gdbName global_database_name 
            -primaryDBConnectionString easy_connect_string_to_primary
            -sid database_system_identifier
            [-createAsStandby 
                [-dbUniqueName db_unique_name_for_standby]]
            [-customScripts scripts_list]
        ```

For more information about ` createDuplicateDB ` options, including the use of custom scripts, see [ *Oracle Database Administrator’s Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN-GUID-7F4B1A64-5B08-425A-A62E-854542B3FD4E) . 

In the following two examples the primary database is ` chicago ` and it resides on the primary system ` myprimary.domain ` . Each example creates a physical standby on the system on which the command is executed, ` boston ` . The ` initParams ` parameter is used in the examples to show how other DBCA parameters can be used in the standby creation command. In these examples, ` initParams ` is used to explicitly set the ` INSTANCE_NAME ` of the standby to match the ` DB_UNIQUE_NAME ` , ` boston ` . 

This first example creates the standby database without any custom scripts being executed afterward. 
        
                ```
        dbca –silent -createDuplicateDB -primaryDBConnectionString  myprimary.domain:1523/chicago.domain 
        -gdbName chicago.domain -sid boston -initParams instance_name=boston –createAsStandby
        ```
        
                ```
        Enter SYS user password:
        Listener config step
        33% complete
        Auxiliary instance creation
        66% complete
        RMAN duplicate
        100% complete
        Look at the log file " /u01/app/oracle/product/12.2.0/dbhome_1/cfgtoollogs/dbca/chicago/chicago.log" for further details.
        
        ```

The following example is exactly the same as the previous example, except that it runs a SQL script named ` /tmp/test.sql ` which can be used to perform post-creation operations. 
        
                ```
        dbca -silent -createDuplicateDB -primaryDBConnectionString  myprimary.domain:1523/chicago.domain
        -gdbName chicago.domain -sid boston -initParams instance_name=boston -createAsStandby  -customScripts /tmp/test.sql
        ```
        
                ```
        Enter SYS user password:
        
        Listener config step
        25% complete
        Auxiliary instance creation
        50% complete
        RMAN duplicate
        75% complete
        Running Custom Scripts
        100% complete
        Look at the log file " /u01/app/oracle/product/12.2.0/dbhome_1/cfgtoollogs/dbca/chicago/chicago.log" for further details.
        ```

> **note:** Even though it is required to have a listener running on the physical standby system, it is not necessary to configure the Oracle Net service names for the databases on either system to execute these commands. In these examples, the Easy Connect naming method was used to create a connection to the primary database, ` Chicago ` , to complete creation of the standby, ` Boston ` . Before adding the new standby to the Data Guard configuration you would first configure Oracle Net service name descriptors on both systems, as described in Step 4 in [ Creating a Physical Standby Task 5: Set Up the Environment to Support the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-2FD8DBAB-C439-4F03-9A94-01C6FEF33392) . 

When these commands complete without any errors, the physical standby ` Boston ` is ready to be added to your Data Guard configuration. As part of adding it, you would need to define the Data Guard parameters in ` Chicago ` and ` Boston ` as shown in [ Creating a Physical Standby Task 3: Create a Parameter File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-06FDC24C-6E90-420B-BB63-3B6BAC4D8AFA) . Optionally, if you have an Oracle Data Guard broker configuration, you could use the broker ` ADD DATABASE ` command to add the new standby to your configuration (see [ *Oracle Data Guard Broker*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-EA723918-2509-4BBC-871B-163885DE3C73) ). 

Starting with Oracle Database 23ai, DBCA supports PDB operations in Dataguard environment. The following DBCA PDB creation operations are now supported: 

      * • PDB from Default (PDB$SEED) 
      * • PDB from PDB flow (Local clone of a PDB) 
      * • PDB from Remote PDB (Remote PDB clone) 

###  Creating a PDB in a Primary Database {#GUID-20D2DEAC-0355-4FBB-ADB8-37BF5F4328E2} 

In an Oracle Data Guard configuration, a pluggable database (PDB) on a primary database is created in the same way that a PDB on a regular database is created. 

This topic lists key points about how Data Guard and the Oracle Multitenant option interact when any type of PDB creation is performed. For detailed information and examples, see My Oracle Support note 2049127.1 at [ ` http://support.oracle.com ` ](http://support.oracle.com) . 

The steps to create a PDB on a regular database are documented in the [ *Oracle Multitenant Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=MULTI-GUID-C95E8613-C556-4818-BE64-B2129AC2F8A2) . Before following those steps, be sure to read the following: 

      * You can specify a subset of PDBs to be replicated on a physical standby of a multitenant container database (CDB). To do so, use the ` ENABLED_PDBS_ON_STANDBY ` initialization parameter to specify a list of PDBs or use the ` STANDBYS ` clause on the ` CREATE PLUGGABLE DATABASE ` statement, or both. 

The ` ENABLED_PDBS_ON_STANDBY ` parameter (see [ ENABLED_PDBS_ON_STANDBY ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN-GUID-A2EC2381-D9C0-4612-87C9-A225C7BCE8C4) ) is valid only on a physical standby; it is ignored by primary databases. (It can be set on a primary database for use if that database ever becomes a standby database.) It can be used to specify which PDBs should or should not be enabled on a physical standby database. If the parameter is not specified, then all PDBs in the CDB are created on the standby unless the ` STANDBYS ` clause is used. 

The ` STANDBYS ` clause has no effect on whether recovery is later enabled or disabled. The ` STANDBYS ` clause of the SQL ` CREATE PLUGGABLE DATABASE ` statement has the following syntax: 
            
                        ```
            CREATE PLUGGABLE DATABASE … STANDBYS={('cdb_name', 'cdb_name', ...) \| NONE \| ALL [EXCEPT ('cdb_name', 'cdb_name', ...)]}
            ```

        * ` cdb_name  ` is the ` DB_UNIQUE_NAME ` for the physical standbys in which the PDB is to be included. 

        * ` NONE ` excludes the PDB from being created in any of the standby CDBs. When a PDB is excluded from all standby CDBs, the PDB's data files are offline and marked as unnamed on all of the standby CDBs. Any new standby CDBs that are instantiated after the PDB has been created must disable the PDB for recovery explicitly to exclude it from the standby CDB. It is possible to enable a PDB on a standby CDB after it was excluded on that standby CDB. 

        * ` ALL ` (the default) includes the PDB being created in all standby CDBs. 

        * ` EXCEPT  cdb_name  ` includes the PDB being created in all standby CDBs except for those CDBs listed in this clause by their ` DB_UNIQUE_NAME ` . 

When a list of CDB names is provided on the ` STANDBYS ` clause, the list must be enclosed in parentheses and each name must be enclosed within single quotation marks. The value of ` DB_UNIQUE_NAME ` can be up to 30 characters and is case insensitive. The following characters are valid in a database name: alphanumeric characters, underscore (_), number sign (#), and dollar sign ($). 

The standbys in the list must be pre-populated with the required files unless the PDB is being created from seed, or a local clone with the standby running Active Data Guard, or a remote clone using the ` STANDBY_PDB_SOURCE_FILE_DBLINK ` parameter. 

      * When you create a PDB as a local clone from a different PDB or from the seed PDB within the same primary CDB, note the following: 
        * When a new PDB is created from seed, the standby automatically instantiates the files. 

        * In an Active Data Guard environment, when a PDB is cloned in the same CDB, Data Guard automatically instantiates the files. 

        * In a non-Active Data Guard environment, when a PDB is cloned in the same CDB, use the ` STANDBYS ` clause on the ` CREATE PLUGGABLE DATABASE ` statement and enable recovery later. 

      * To automatically maintain standby databases when performing PDB remote clones or plugins, use the following initialization parameters: 
        * ` STANDBY_PDB_SOURCE_FILE_DBLINK ` —For use with remote cloning. Specifies the name of the database link used to clone the PDB at the primary. When the redo for the PDB clone operation is applied at the standby, it uses that database link to connect to the source database to repeat the clone process to the destination standby database. This parameter is available only in Active Data Guard because it accesses the database dictionary for specific information about the link. 

The source PDB for the cloning operation must be in read only mode for the duration of both the clone to the primary and the clone to the standby. 

On remote clones that do *not* use the ` STANDBY_PDB_SOURCE_FILE_DBLINK ` parameter, the SQL ` CREATE PLUGGABLE DATABASE ` statement used to create the PDB must use the ` STANDBYS=NONE ` clause. 

        * ` STANDBY_PDB_SOURCE_FILE_DIRECTORY ` —For use with plugins. Specifies the directory location where files of a PDB being used for repeated plugins are stored. When the redo for the PDB plugin operation is applied at the standby, it searches the directory location for the data files and after finding them, copies them to the location the standby requires them to be based on the settings of the ` DB_CREATE_FILE_DEST ` and ` DB_FILE_NAME_CONVERT ` parameters. 

> **note:** The files must be image copies of the source PDB data files. The copies should be created after creation of the manifest XML file for the source PDB. 

These parameters should be reset after they have been used and are no longer needed. There is no runtime impact from setting and resetting these parameters because doing so does not require that instances be restarted. 

See My Oracle Support note 2274735.1 at [ ` http://support.oracle.com ` ](http://support.oracle.com) for detailed information about using these parameters. 

      * To create a PDB from an XML file, copy the data files specified in the XML file to the standby database. 

When a PDB is plugged into a primary database from an XML file, all standby databases in the configuration must be given access to the source PDB's files. (The only exceptions to this are standbys that have recovery deferred by use of either the ` STANDBYS ` clause on the ` CREATE PLUGGABLE DATABASE ` statement or the ` ENABLED_PDBS_ON_STANDBY ` initialization parameter.) 

To provide standbys in the configuration with access to the source PDB's files, either use the ` STANDBY_PDB_SOURCE_FILE_DIRECTORY ` initialization parameter or copy the files of the source PDB to the standby site, typically the same set of PDB files that are to be plugged into the primary database. The files should be copied to the standby prior to the plugin operation being performed at the primary to minimize disruptions to managed standby recovery. Ensure the files are copied to the appropriate location at the standby where managed standby recovery can find them, as follows: 

        * If data files reside in standard operating system file systems, then the location of the files at the standby database are based on the value of the ` DB_FILE_NAME_CONVERT ` parameter. For more details about setting primary database initialization parameters, see [ Set Primary Database Initialization Parameters ](creating-oracle-data-guard-physical-standby.md#GUID-0FD317D6-1A49-4DB3-A141-D00CB3089E84)

        * If data files reside in ASM, then use the ASMCMD utility to copy the files to the following location at the standby database: 
                
                                ```
                db_create_file_dest/db_unique_name/GUID/datafile
                
                ```

The ` GUID ` parameter is the global unique identifier assigned to the PDB; once assigned, it does not change. To find the value of the ` GUID ` parameter, query the ` V$CONTAINERS ` view before unplugging the PDB from its original source container. The following example shows how to find the value of the ` GUID ` parameter for the PDB whose PDB container ID in the source container is ` 3 ` : 
                
                                ```
                SELECT guid
                  FROM V$CONTAINERS
                 WHERE con_id=3;
                 
                GUID
                 
                D98C12257A951FC4E043B623F00A7AF5
                
                ```

In this example, if the value of the ` DB_CREATE_FILE_DEST ` parameter is ` +DATAFILE ` and the value of the ` DB_UNIQUE_NAME ` parameter is ` BOSTON ` , then the data files are copied to: 
                
                                ```
                +DATAFILE/BOSTON/D98C12257A951FC4E043B623F00A7AF5/datafile
                
                ```

The path name of the data files on the standby database must be the same as the resultant path name when you create the PDB on the primary, unless the ` DB_FILE_NAME_CONVERT ` database initialization parameter has been configured on the standby. In that case, the path name of the data files on the standby database is the path name on the primary with ` DB_FILE_NAME_CONVERT ` applied. 

> **note:** See Also: 
      * [ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF55686) for more information about the SQL statement ` CREATE ` ` PLUGGABLE ` ` DATABASE `
