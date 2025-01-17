## DOracle Data Guard and Oracle Real Application Clusters {#GUID-084DF97B-5524-401A-BAA4-65BA279F5B11}

An Oracle Data Guard configuration can consist of any combination of single-instance and Oracle Real Application Clusters (Oracle RAC) multiple-instance databases. <br>This appendix summarizes the configuration requirements and considerations that apply when using Oracle Data Guard with Oracle RAC databases. It contains the following sections:

* [Configuring Standby Databases in an Oracle RAC Environment](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-3140A293-DDD8-4559-8493-B6C21646E90F)

* [Configuration Considerations in an Oracle RAC Environment](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-E754F226-C5EC-4A93-A506-C699581AA2AF)




### Configuring Standby Databases in an Oracle RAC Environment {#GUID-3140A293-DDD8-4559-8493-B6C21646E90F}

You can configure a standby database to protect a primary database using Oracle Real Application Clusters (Oracle RAC). 

The following table describes the possible combinations of instances in the primary and standby databases:

Instance Combinations | Single-Instance Standby Database | Multi-Instance Standby Database  
---|---|---  
**Single-instance primary database** |  Yes |  Yes  
**Multi-instance primary database** |  Yes |  Yes  

In each scenario, each instance of the primary database transmits its redo data to an instance of the standby database. As of Oracle Database 12c release 2 (12.2.0.1) you can also perform multi-instance redo apply.

#### Setting Up Multi-Instance Redo Apply {#GUID-00B72AF1-0453-4FBD-901F-A3764631BE1F}

As of Oracle Database 12`c` Release 2 (12.2.0.1), a new ` INSTANCES [ ALL | *`integer`*]` clause is available on the SQL `ALTER DATABASE RECOVER MANAGED STANDBY DATABASE` command. 

To use In-Memory Column Store with multi-instance redo apply in an Active Data Guard environment, set the `enable_imc_with_mira` initialization parameter to `TRUE`. 

The following restrictions apply:

* The clause is applicable only for Oracle Real Application Clusters (Oracle RAC) or Oracle RAC One Node databases.

* You cannot use multi-instance redo apply when you enable `STANDBY NOLOGGING FOR DATA AVAILABILITY` or `STANDBY NOLOGGING FOR LOAD PERFORMANCE` on the primary database. 




The `ALL` option causes redo apply to run on all instances in an Oracle RAC standby database that are in an open or mounted state at the time recovery is started. All instances must be in the same state — either open or mounted. A mix of states is not allowed. 

The *`integer`*  option restricts the number of instances that redo apply uses to the number you specify. For integer, specify an integer value from 1 to the number of instances in the standby database. The database chooses the instances on which to perform Redo Apply; you cannot specify particular instances. 

The `V$RECOVERY_PROGRESS` view is only populated on the instance where recovery was started (where the MRP0 process resides). 

If you omit the `INSTANCES `clause, then recovery happens on only one instance where the command was issued. 

Because recovery processes ship redo among instances, redo apply performance is directly related to network bandwidth and latency.

#### Setting Up a Multi-Instance Primary with a Single-Instance Standby {#GUID-EBCDC4F9-B0C7-49C7-B04C-AD4DF60F3D26}

This figure illustrates an Oracle RAC database with two primary database instances (a multi-instance primary database) transmitting redo data to a single-instance standby database.

Figure D-1 Transmitting Redo Data from a Multi-Instance Primary Database

![Description of Figure D-1 follows](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img/sbydb056.gif)<br>[Descriptionof "Figure D-1 Transmitting Redo Data from a Multi-Instance Primary Database"](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img_text/sbydb056.md)

In this case, Instance 1 of the primary database archives redo data to local archived redo log files 1, 2, 3, 4, 5 and transmits the redo data to the standby database destination, while Instance 2 archives redo data to local archived redo log files 32, 33, 34, 35, 36 and transmits the redo data to the same standby database destination. The standby database automatically determines the correct order in which to apply the archived redo log files.

Although [Figure D-1](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-EBCDC4F9-B0C7-49C7-B04C-AD4DF60F3D26__I635256) does not show standby redo logs, it is a best practice to configure standby redo logs at the standby for both instances of the primary. The redo from the primary online redo log files at Instance 1 and Instance 2 would then be received first in the standby redo logs for Instance 1 and Instance 2, respectively, and then archived. 

**To Configure a Primary Database in an Oracle RAC Environment**

Before you create a standby database you must first ensure the primary database is properly configured. To do so, you must perform some preparatory steps, after which the database is prepared to serve as the primary database for one or more standby databases.

**To Configure a Single Instance Standby Database**

To specify the location of the archived redo log files and standby redo log files, define the `LOG_ARCHIVE_DEST_`*n* and `LOG_ARCHIVE_FORMAT` parameters. 

> **note:** See Also: 

* [Managing Standby Redo Logs](oracle-data-guard-redo-transport-services.md#GUID-E6EC6104-3C38-482D-B807-A0E84ECFB937) for more information about standby redo logs 

* [Creating a Physical Standby Database](creating-oracle-data-guard-physical-standby.md#GUID-B511FB6E-E3E7-436D-94B5-071C37550170) for information about configuring a primary database when creating a physical standby database. 

* [Creating a Logical Standby Database](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E) for information about configuring a primary database when creating a logical standby database. 




#### Setting Up Oracle RAC Primary and Standby Databases {#GUID-65F87F2F-6602-4673-B971-4AEE3B4F7B4D}

An Oracle RAC primary database must be set up to send redo data to an Oracle RAC standby database, and an Oracle RAC standby database must be set up to receive redo data.

##### Configuring an Oracle RAC Standby Database to Receive Redo Data {#GUID-54BEB161-7125-4879-98B4-ED1F61BC3E9E}

These steps describe how to configure an Oracle RAC standby database to receive redo data from a primary database.

1. Create a standby redo log on the standby database. The redo log files in the standby redo log must reside in a location that can be accessed by all of the standby database instances, such as on a cluster file system or Oracle ASM instance. See [Managing Standby Redo Logs](oracle-data-guard-redo-transport-services.md#GUID-E6EC6104-3C38-482D-B807-A0E84ECFB937) for more information about creating a standby redo log.
2. Configure standby redo log archival on each standby database instance. The standby redo log must be archived to a location that can be accessed by all of the standby database instances, and every standby database instance must be configured to archive the standby redo log to the same location.



##### Configuring an Oracle RAC Primary Database to Send Redo Data {#GUID-AD6594AC-6F4B-4EC0-84DD-54CEF3A6C1A7}

Configure each instance of the Oracle RAC primary database to send its redo data to the Oracle RAC standby database. These are the best practices that Oracle recommends you follow when you configure an Oracle RAC primary database to send redo data to an Oracle RAC standby database.

1. Use the same `LOG_ARCHIVE_DEST_n` parameter on each primary database instance to send redo data to a given standby database. 

2. Set the `SERVICE` attribute of each `LOG_ARCHIVE_DEST_n` parameter that corresponds to a given standby database to the same net service name. 

3. The net service name should resolve to an Oracle Net connect descriptor that contains an address list, and that address list should contain connection data for each standby database instance.




[Configuring an Oracle Database to Send Redo Data](oracle-data-guard-redo-transport-services.md#GUID-45DFFA7C-3967-4B46-8C2C-DBD22A4D6A74) describes how to configure an Oracle database instance to send redo data to another database. 

### Configuration Considerations in an Oracle RAC Environment {#GUID-E754F226-C5EC-4A93-A506-C699581AA2AF}

Oracle Real Application Clusters (Oracle RAC) have specific Oracle Data Guard configuration requirements regarding the format for archived redo log filenames, and for data protection modes.

See the following topics:

* [Format for Archived Redo Log Filenames](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-1D0678E0-160E-4097-8D51-10563838FB98)

* [Data Protection Modes](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-7920893D-D3D2-44F4-A640-F68B6D516B1C)




#### Format for Archived Redo Log Filenames {#GUID-1D0678E0-160E-4097-8D51-10563838FB98}

The format for archived redo log filenames is in the form of log_%*`parameter`*. 

The %*`parameter`* can include one or more of the parameters in [Table D-1](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-1D0678E0-160E-4097-8D51-10563838FB98__G649139). 

**Table D-1 Directives for the LOG_ARCHIVE_FORMAT Initialization Parameter**

Directives | Description  
---|---  
`%a` |  Database activation ID.  
`%A` |  Database activation ID, zero filled.  
`%d` |  Database ID.  
`%D` |  Database ID, zero filled.  
`%t` |  Instance thread number.  
`%T` |  Instance thread number, zero filled.  
`%s` |  Log file sequence number.  
`%S` |  Log file sequence number, zero filled.  
`%r` |  Resetlogs ID.  
`%R` |  Resetlogs ID, zero filled.  

For example:
```
LOG_ARCHIVE_FORMAT = log%d_%t_%s_%r.arc
```


The thread parameters %t or %T are mandatory for Oracle RAC to uniquely identify the archived redo log files with the `LOG_ARCHIVE_FORMAT` parameter. 

#### Data Protection Modes {#GUID-7920893D-D3D2-44F4-A640-F68B6D516B1C}

If any instance of an Oracle RAC primary database loses connectivity with a standby database, all other primary database instances stop sending redo to the standby database for the number of seconds specified on the `LOG_ARCHIVE_DEST_`*n* `REOPEN` attribute, after which all primary database instances attempt to reconnect to the standby database. 

The following list describes the behavior of the protection modes in Oracle RAC environments:

* Maximum protection configuration<br>If a lost destination is the *last* participating `SYNC` destination, then the instance loses connectivity and is shut down. Other instances in an Oracle RAC configuration that still have connectivity to the standby destinations recover the lost instance and continue sending redo to their standby destinations. Only when every instance in an Oracle RAC configuration loses connectivity to the last standby destination is the primary database shut down. 

* Maximum availability and maximum performance configurations<br>Other instances in an Oracle RAC configuration that still have connectivity to the standby destination recover the lost instance and continue sending redo to their standby destinations. When every instance in an Oracle RAC configuration loses connectivity to the standby destination, the primary database continues operation in maximum performance mode. The maximum performance mode ensures very minimal data loss except when the entire standby fails.<br>The maximum availability protection mode ensures zero data loss except in the case of certain double faults, such as failure of the primary database after the failure of all standby databases. 


