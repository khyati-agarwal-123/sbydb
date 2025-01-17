## 1Introduction to Oracle Data Guard {#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7}

Oracle Data Guard ensures high availability, data protection, and disaster recovery for enterprise data.<br>Oracle Data Guard provides a comprehensive set of services that create, maintain, manage, and monitor one or more standby databases to enable production Oracle databases to survive disasters and data corruptions. Oracle Data Guard maintains these standby databases as copies of the production database. Then, if the production database becomes unavailable because of a planned or an unplanned outage, Oracle Data Guard can switch any standby database to the production role, minimizing the downtime associated with the outage. Oracle Data Guard can be used with traditional backup, restoration, and cluster techniques to provide a high level of data protection and data availability. Oracle Data Guard transport services are also used by other Oracle features such as Oracle Streams and Oracle GoldenGate for efficient and reliable transmission of redo from a source database to one or more remote destinations.<br>With Oracle Data Guard, administrators can optionally improve production database performance by offloading resource-intensive backup and reporting operations to standby systems.<br>See the following topics which describe the highlights of Oracle Data Guard:

* [Oracle Data Guard Configurations](introduction-to-oracle-data-guard-concepts.md#GUID-AB9DF863-2C7E-4767-81F2-56AD0FA30B49)

* [Oracle Data Guard Services](introduction-to-oracle-data-guard-concepts.md#GUID-5765C9D9-0611-4A8F-9CC0-5A818CE127A7)

* [Oracle Data Guard Broker](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E)

* [Oracle Data Guard Protection Modes](introduction-to-oracle-data-guard-concepts.md#GUID-65DA4E2E-3666-4972-895C-EA45D043B127)

* [Client Failover](introduction-to-oracle-data-guard-concepts.md#GUID-B26CA33C-999A-434D-A26D-3179AC3F8718)

* [Oracle Data Guard and Complementary Technologies](introduction-to-oracle-data-guard-concepts.md#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923)

* [Oracle Active Data Guard Supports Oracle Sharding](introduction-to-oracle-data-guard-concepts.md#GUID-858549EF-B72F-4545-A5D1-C632C8ECC961)

* [Summary of Oracle Data Guard Benefits](introduction-to-oracle-data-guard-concepts.md#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B)




### Oracle Data Guard Configurations {#GUID-AB9DF863-2C7E-4767-81F2-56AD0FA30B49}

An **Oracle Data Guard configuration** can contain one primary database and up to thirty destinations. 

The members of an Oracle Data Guard configuration are connected by Oracle Net and may be dispersed geographically. There are no restrictions on where the members of an Oracle Data Guard configuration are located as long as they can communicate with each other. For example, you can have a standby database in the same data center as the primary database, along with two standbys in another data center.

You can manage primary and standby databases using either the SQL command-line interface or the Oracle Data Guard broker interfaces. The broker provides a command-line interface (DGMGRL) and a graphical user interface that is integrated in Oracle Enterprise Manager Cloud Control.

#### Primary Database {#GUID-0472F1B0-C2AA-4E9D-8773-1A7FBE27DC82}

An Oracle Data Guard configuration contains one production database, also referred to as the primary database, that functions in the primary role.

The primary database is the database that is accessed by most of your applications.

The primary database can be either a single-instance Oracle database or an Oracle Real Application Clusters (Oracle RAC) database. 

#### Standby Databases {#GUID-C49AC6F4-C89B-4487-BC18-428D65865B9A}

A standby database is a transactionally consistent copy of the primary database.

Using a backup copy of the primary database, you can create up to thirty standby databases and incorporate them into an Oracle Data Guard configuration. Oracle Data Guard automatically maintains each standby database by transmitting redo data from the primary database and then applying the redo to the standby database. 

Similar to a primary database, a standby database can be either a single-instance Oracle database or an Oracle RAC database. 

The types of standby databases are as follows:

* **Physical standby database**<br>Provides a physically identical copy of the primary database, with on-disk database structures that are identical to the primary database on a block-for-block basis. The database schema, including indexes, are the same. A physical standby database is kept synchronized with the primary database, through Redo Apply, which recovers the redo data received from the primary database and applies the redo to the physical standby database. <br>A physical standby database can receive and apply redo while it is open for read-only access. A physical standby database can therefore be used concurrently for data protection and reporting.<br>A physical standby database can be used to install eligible one-off patches, patch set updates (PSUs), and critical patch updates (CPUs), in rolling fashion. For more information about this functionality, see the My Oracle Support note 1265700.1 at [`http://support.oracle.com`](http://support.oracle.com). 

* **Logical standby database**<br>Contains the same logical information as the production database, although the physical organization and structure of the data can be different. The logical standby database is kept synchronized with the primary database through SQL Apply, which transforms the data in the redo received from the primary database into SQL statements and then executes the SQL statements on the standby database. <br>The flexibility of a logical standby database lets you upgrade Oracle Database software (patch sets and new Oracle Database releases) and perform other database maintenance in rolling fashion with almost no downtime. The transient logical database rolling upgrade process can also be used with existing physical standby databases. 

* **Snapshot Standby Database** <br>A snapshot standby database is a fully updatable standby database.<br>Like a physical or logical standby database, a snapshot standby database receives and archives redo data from a primary database. Unlike a physical or logical standby database, a snapshot standby database does not apply the redo data that it receives. The redo data received by a snapshot standby database is not applied until the snapshot standby is converted back into a physical standby database, after first discarding any local updates made to the snapshot standby database.<br>A snapshot standby database is best used in scenarios that require a temporary, updatable snapshot of a physical standby database. For example, you can use the Oracle Real Application Testing option to capture the database workload on a primary and then replay it for test purposes on the snapshot standby. Because redo data received by a snapshot standby database is not applied until it is converted back into a physical standby, the time needed to recover from a primary database failure is directly proportional to the amount of redo data that needs to be applied.




> **note:** See Also: 

* [*Oracle Database Testing Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RATUG101) for more information about Oracle Real Application Testing and the license required to use it 




#### Far Sync Instances {#GUID-9F8CAE44-ABA6-4CA4-A881-1E483C8E62A1}

An Oracle Data Guard far sync instance is a remote Oracle Data Guard destination that accepts redo from the primary database and then ships that redo to other members of the Oracle Data Guard configuration.

A far sync instance manages a control file, receives redo into standby redo logs (SRLs), and archives those SRLs to local archived redo logs, but that is where the similarity with standbys ends. A far sync instance does not have user data files, cannot be opened for access, cannot run redo apply, and can never function in the primary role or be converted to any type of standby database. 

Far sync instances are part of the Oracle Active Data Guard Far Sync feature, which requires an Oracle Active Data Guard license.

> **note:** See Also: 

* [Far Sync ](creating-oracle-data-guard-far-sync-instance.md#GUID-8AD7FBA2-42B0-46CF-852B-1AF0CB4A36E8)




#### Zero Data Loss Recovery Appliance {#GUID-6BA2BB48-5E25-43BA-8667-9392133FCD66}

Zero Data Loss Recovery Appliance (Recovery Appliance) is an enterprise-level backup solution that provides a single repository for backups of all of your Oracle databases. 

Recovery Appliance offloads most Oracle Database backup and restore processing to a centralized backup system. It enables you to achieve significant efficiencies in storage utilization, performance, and manageability of backups.

> **note:** See Also: 

* [*Zero Data Loss Recovery Appliance Administrator's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=AMAGD-GUID-B57EDE37-6F78-409D-B29D-4D9F942174BC)




#### Configuration Example {#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF}

[Figure 1-1](introduction-to-oracle-data-guard-concepts.md#GUID-0DA5AED1-4D4B-446D-ADD2-7F463A688AEF__I1036091) shows a typical Oracle Data Guard configuration that provides data protection for the entire database. It contains a primary database that transmits redo data to a standby database. The standby database is remotely located from the primary database for disaster recovery and backup operations. You can configure the standby database at the same location as the primary database. However, for disaster recovery purposes, Oracle recommends you configure standby databases at remote locations. 

Figure 1-1 Typical Oracle Data Guard Configuration

![Description of Figure 1-1 follows](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img/sbydb042.gif)<br>[Descriptionof "Figure 1-1 Typical Oracle Data Guard Configuration"](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img_text/sbydb042.md)

**Related Topics**

* [Overview of Broker Configurations](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-FBEC1B54-CEE8-4634-89AD-55FDDB0EA023)



### Data Guard for Pluggable Databases {#GUID-B214B581-EDBE-45BF-A3EC-1795CB9B816F}

Oracle Data Guard uses Redo Transport Services and Apply Services to manage the transmission of redo data, the application of redo data, and changes to the database roles.

Starting with Oracle Database Release 21c, you can provide protection for one or more pluggable databases (PDBs) within a multitenant container database (CDB). This configuration consists of two primary databases. The primary database containing the PDBs that must be protected is called the **source database**. Redo data from the source database is shipped to the **target database**, which is also a primary database. Use Oracle Data Guard broker to configure data protection for PDBs. 

* Oracle Database 23ai introduces several new features for Data Guard for Pluggable Databases (DG PDB). They include:

* Simplified setup and management of DG PDB operations.

* The Database Configuration Assistant (DBCA) now supports PDB operations in a Data Guard environment. It is possible to create a PDB from Default (`PDB$SEED`), a local clone of a PDB, and a remote PDB clone. 
* New `DBMS_DG` APIs are introduced for performing common DG PDB operations. 
* All temp file related DDLs supported on a normal CDB are supported for a DG PDB. 

* Functionality with Clusterware for PDB service manipulation and application continuity has been added.

For more information on implementing and managing Data Guard for Pluggable databases, please see [Oracle Data Guard Broker](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-D5023ABB-EAA8-4E85-98D4-654CDB9F9A12). 

* With the introduction of Oracle Database 23ai, a PDB can be the source PDB for GoldenGate Per-PDB Capture and a Data Guard DG PDB. When there is a role transition, the target PDB in a different primary CDB becomes the new source PDB, GoldenGate Per-PDB Capture connects to the new source, finish mining the redo stream from the old source REDO stream, and switches to the new REDO stream and continue mining there. All these will be done with minimal user intervention.




> **note:** Saving or discarding `STATE` using the `ALTER PLUGGABLE DATABASE` command is not supported in PER PDB Gata Guard configurations. 

### Oracle Data Guard Services {#GUID-5765C9D9-0611-4A8F-9CC0-5A818CE127A7}

Oracle Data Guard uses Redo Transport Services and Apply Services to manage the transmission of redo data, the application of redo data, and changes to the database roles.

* [Redo Transport Services](introduction-to-oracle-data-guard-concepts.md#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57)<br>Control the automated transfer of redo data from the production database to one or more archival destinations.

* [Apply Services](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5)<br>Redo data is applied directly from standby redo log files as they are filled using real-time apply. If standby redo log files are not configured, then redo data must first be archived at the standby database before it is applied. 

* [Role Transitions](introduction-to-oracle-data-guard-concepts.md#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA)<br>Change the role of a database from a standby database to a primary database, or from a primary database to a standby database using either a switchover or a failover operation.




#### Redo Transport Services {#GUID-05D621EE-51AF-41E4-9CFC-45AEBAB08F57}

**Redo transport services** control the automated transfer of redo data from the production database to one or more archival destinations. 

Redo transport services perform the following tasks:

* Transmit redo data from the primary system to the standby systems in the configuration

* Manage the process of resolving any gaps in the archived redo log files due to a network failure

* Automatically detect missing or corrupted archived redo log files on a standby system and automatically retrieve replacement archived redo log files from the primary database or another standby database. 




#### Apply Services {#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5}

**Apply services** automatically apply the redo data on the standby database to maintain consistency with the primary database. 

The redo data is transmitted from the primary database and written to the standby redo log on the standby database. Apply services also allows read-only access to the data.

The main difference between physical and logical standby databases is the manner in which apply services apply the archived redo data:

* For physical standby databases, Oracle Data Guard uses **Redo Apply** technology, which applies redo data on the standby database using standard recovery techniques of an Oracle database, as shown in [Figure 1-2](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5__I1043408). 

Figure 1-2 Automatic Updating of a Physical Standby Database 

![Description of Figure 1-2 follows](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img/sbydb054.gif)<br>[Descriptionof "Figure 1-2 Automatic Updating of a Physical Standby Database "](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img_text/sbydb054.md)

With Oracle Database Release 21c or later, when protecting one or more PDBs within a CDB, Redo Apply applies redo data received from the source database to the target database. In this configuration, both source database and target database are primary databases.

* For logical standby databases, Oracle Data Guard uses **SQL Apply** technology, which first transforms the received redo data into SQL statements and then executes the generated SQL statements on the logical standby database, as shown in [Figure 1-3](introduction-to-oracle-data-guard-concepts.md#GUID-D284E617-3F4F-4128-98F5-A3EB88772BF5__I1043983). 

Figure 1-3 Automatic Updating of a Logical Standby Database

![Description of Figure 1-3 follows](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img/sbydb032.gif)<br>[Descriptionof "Figure 1-3 Automatic Updating of a Logical Standby Database"](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img_text/sbydb032.md)




#### Role Transitions {#GUID-EFAC6E17-BCE5-417C-9124-1595DBF0ABFA}

Using Oracle Data Guard, you can change the role of a database using either a switchover or a failover operation.

An Oracle database operates in one of two roles: primary or standby. 

A **switchover** is a role reversal between the primary database and one of its standby databases. A switchover ensures no data loss. This is typically done for planned maintenance of the primary system. During a switchover, the primary database transitions to a standby role, and the standby database transitions to the primary role. 

A **failover** is when the primary database is unavailable. Failover is performed only in the event of a failure of the primary database, and the failover results in a transition of a standby database to the primary role. The database administrator can configure Oracle Data Guard to ensure no data loss. 

The role transitions described in this documentation are invoked manually using SQL statements. You can also use the Oracle Data Guard broker to simplify role transitions and automate failovers using Oracle Enterprise Manager Cloud Control or the DGMGRL command-line interface, as described in [Oracle Data Guard Broker](introduction-to-oracle-data-guard-concepts.md#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E). 

### Oracle Data Guard Broker {#GUID-538B9DDD-1553-479D-8E1D-0B5C6848403E}

The Oracle Data Guard broker is a distributed management framework that automates the creation, maintenance, and monitoring of Oracle Data Guard configurations.

You can use either the Oracle Enterprise Manager Cloud Control graphical user interface (GUI) or the Oracle Data Guard command-line interface (DGMGRL) to:

* Create and enable Oracle Data Guard configurations, including setting up redo transport services and apply services

* Manage an entire Oracle Data Guard configuration from any system in the configuration

* Manage and monitor Oracle Data Guard configurations that contain Oracle RAC primary or standby databases

* Simplify switchovers and failovers by allowing you to invoke them using either a single key click in Oracle Enterprise Manager Cloud Control or a single command in the DGMGRL command-line interface. 

* Enable Oracle Data Guard fast-start failover to fail over *automatically* when the primary database becomes unavailable. When fast-start failover is enabled, the Oracle Data Guard broker determines if a failover is necessary and initiates the failover to the specified target standby database automatically, with no need for DBA intervention. 




In addition, Oracle Enterprise Manager Cloud Control automates and simplifies:

* Creating a physical or logical standby database from a backup copy of the primary database 

* Adding new or existing standby databases to an existing Oracle Data Guard configuration 

* Monitoring log apply rates, capturing diagnostic information, and detecting problems quickly with centralized monitoring, testing, and performance tools




> **note:** See Also: 

[*Oracle Data Guard Broker*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR001) for more information 

#### Using Oracle Enterprise Manager Cloud Control {#GUID-4B4BB1D1-1E38-40B9-B716-A0324A83966E}

Oracle Enterprise Manager Cloud Control provides a web-based interface for viewing, monitoring, and administering primary and standby databases in an Oracle Data Guard configuration.

Oracle Enterprise Manager Cloud Control is sometimes referred to simply as Cloud Control.

Enterprise Manager's easy-to-use interfaces, combined with the broker's centralized management and monitoring of the Oracle Data Guard configuration, enhance the Oracle Data Guard solution for high availability, site protection, and data protection of an enterprise. 

Using Enterprise Manager, you can perform all management operations either locally or remotely. You can view home pages for Oracle databases, including primary and standby databases and instances, create or add existing standby databases, start and stop instances, monitor instance performance, view events, schedule jobs, and perform backup and recovery operations.

#### Using the Oracle Data Guard Command-Line Interface {#GUID-997A3B25-B3A0-460C-990B-1E5BE2AF0622}

The Oracle Data Guard command-line interface (DGMGRL) enables you to control and monitor an Oracle Data Guard configuration from the DGMGRL prompt or within scripts. 

You can perform most of the activities required to manage and monitor the databases in the configuration using DGMGRL. See [*Oracle Data Guard Broker*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR495) for complete DGMGRL reference information and examples. 

### Oracle Data Guard Protection Modes {#GUID-65DA4E2E-3666-4972-895C-EA45D043B127}

Oracle Data Guard provides three distinct modes of data protection.

In some situations, a business cannot afford to lose data regardless of the circumstances. In other situations, the availability of the database may be more important than any potential data loss in the unlikely event of a multiple failure. Finally, some applications require maximum database performance at all times, and can therefore tolerate a small amount of data loss if any component fails. The following are brief descriptions of the protection modes available for each of these situations:

Maximum Protection

This protection mode ensures that no data loss occurs if the primary database fails. To provide this level of protection, the redo data needed to recover a transaction must be written to both the online redo log and to the standby redo log on at least one synchronized standby database before the transaction commits. To ensure that data loss cannot occur, the primary database shuts down, rather than continue processing transactions, if it cannot write its redo stream to at least one synchronized standby database.

All three protection modes require that specific redo transport options be used to send redo data to at least one standby database.

Maximum Availability

This protection mode provides the highest level of data protection that is possible without compromising the availability of a primary database. With Oracle Data Guard, transactions do not commit until all redo data needed to recover those transactions has either been received in memory or written to the standby redo log (depending upon configuration) on at least one synchronized standby database. If the primary database cannot write its redo stream to at least one synchronized standby database, it operates as if it were in maximum performance mode to preserve primary database availability until it is again able to write its redo stream to a synchronized standby database.

This protection mode ensures zero data loss except in the case of certain double faults, such as failure of a primary database after failure of the standby database.

Maximum Performance

This is the default protection mode. It provides the highest level of data protection that is possible without affecting the performance of a primary database. This is accomplished by allowing transactions to commit as soon as all redo data generated by those transactions has been written to the online log. Redo data is also written to one or more standby databases, but this is done asynchronously with respect to transaction commitment, so primary database performance is unaffected by delays in writing redo data to the standby database(s).

This protection mode offers slightly less data protection than maximum availability mode and has minimal impact on primary database performance.

> **note:** See Also: 

* [ Oracle Data Guard Protection Modes ](oracle-data-guard-protection-modes.md#GUID-16298839-9B8A-4E58-8EC9-27912E57F1CE) for more detailed descriptions of these modes and for information about setting the protection mode of a primary database 




### Client Failover {#GUID-B26CA33C-999A-434D-A26D-3179AC3F8718}

A high availability architecture requires a fast failover capability for databases and database clients. Client failover encompasses failure notification, stale connection cleanup, and transparent reconnection to the new primary database.

Oracle Database provides the capability to integrate database failover with failover procedures that automatically redirect clients to a new primary database within seconds of a database failover.

> **note:** See Also: 

* [*Oracle Data Guard Broker*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR3546) for information about configuration requirements specific to Oracle Data Guard for Fast Application Notification (FAN), Fast Connection Failover (FCF), and role-specific database services 

* Application Checklist for Continuous Service for MAA Solutions" technical brief at<br>[`http://www.oracle.com/goto/maa`](https://www.oracle.com/docs/tech/application-checklist-for-continuous-availability-for-maa.pdf)




#### Application Continuity {#GUID-37CFE42A-D731-4A07-977B-09BE3D309E0C}

Application Continuity is an Oracle Database feature that enables rapid and nondisruptive replays of requests against the database after a recoverable error that made the database session unavailable. 

Application Continuity is supported for Oracle Data Guard switchovers to physical standby databases. It is also supported for fast-start failover to physical standbys in maximum availability data protection mode. To use Application Continuity, the primary and standby databases must be licensed for Oracle Real Application Clusters (Oracle RAC) or Oracle Active Data Guard. 

> **note:** See Also: 

* [*Oracle Real Application Clusters Administration and Deployment Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RACAD-GUID-BD699AEB-9F85-42A8-8687-5A979918938D) for information about Application Continuity 




### Oracle Data Guard and Complementary Technologies {#GUID-9EEFD8E5-901F-4092-85F4-2E0069AC5923}

Oracle Database provides several unique technologies that complement Oracle Data Guard to help keep business critical systems running with greater levels of availability and data protection than when using any one solution by itself. 

The following list summarizes some Oracle high-availability technologies:

* Oracle Real Application Clusters (Oracle RAC)<br>Oracle RAC enables multiple independent servers that are linked by an interconnect to share access to an Oracle database, providing high availability, scalability, and redundancy during failures. Oracle RAC and Oracle Data Guard together provide the benefits of both system-level, site-level, and data-level protection, resulting in high levels of availability and disaster recovery without loss of data:

* Oracle RAC addresses system failures by providing rapid and automatic recovery from failures, such as node failures and instance crashes. It also provides increased scalability for applications.

* Oracle Data Guard addresses site failures and data protection through transactionally consistent primary and standby databases that do not share disks, enabling recovery from site disasters and data corruption.

Many different architectures using Oracle RAC and Oracle Data Guard are possible depending on the use of local and remote sites and the use of nodes and a combination of logical and physical standby databases. See [ Oracle Data Guard and Oracle Real Application Clusters](configuring-data-guard-standby-databases-in-oracle-RAC.md#GUID-084DF97B-5524-401A-BAA4-65BA279F5B11) for Oracle RAC and Oracle Data Guard integration. 

* Oracle Real Application Clusters One Node (Oracle RAC One Node)<br>Oracle RAC One Node provides enhanced high availability for noncluster databases, protecting them from both planned and unplanned downtime. Oracle RAC One Node provides the following:

* Always-on noncluster database services

* Better consolidation for database servers

* Enhanced server virtualization

* Lower cost development and test platform for full Oracle RAC

In addition, Oracle RAC One Node facilitates the consolidation of database storage, standardizes your database environment, and, when necessary, enables you to upgrade to a full, multinode Oracle RAC database without downtime or disruption.

Oracle Data Guard and Oracle Data Guard broker are fully integrated with Oracle Real Application Clusters One Node (Oracle RAC One Node).

* Flashback Database<br>The Flashback Database feature provides fast recovery from logical data corruption and user errors. By allowing you to flash back in time, previous versions of business information that might have been erroneously changed or deleted can be accessed once again. This feature:

* Eliminates the need to restore a backup and roll forward changes up to the time of the error or corruption. Instead, Flashback Database can *roll back* an Oracle database to a previous point-in-time, without restoring data files. 

* Provides an alternative to delaying the application of redo to protect against user errors or logical corruptions. Therefore, standby databases can be more closely synchronized with the primary database, thus reducing failover and switchover times. 

* Avoids the need to completely re-create the original primary database after a failover. The failed primary database can be flashed back to a point in time before the failover and converted to be a standby database for the new primary database.

See [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV71000)for information about Flashback Database, and [Specifying a Time Delay for the Application of Archived Redo Log Files](oracle-data-guard-redo-apply-services.md#GUID-B891A164-1FDC-4AB3-838F-E6A91E7DA0B7) for information describing the application of redo data. 

* Recovery Manager (RMAN)<br>RMAN is an Oracle utility that simplifies backing up, restoring, and recovering database files. Like Oracle Data Guard, RMAN is a feature of the Oracle database and does not require separate installation. Oracle Data Guard is well integrated with RMAN, allowing you to:

* Use the Recovery Manager `DUPLICATE` command to create a standby database from backups of your primary database. 

* Take backups on a physical standby database instead of the production database, relieving the load on the production database and enabling efficient use of system resources on the standby site. Moreover, backups can be taken while the physical standby database is applying redo. 

* Help manage archived redo log files by automatically deleting the archived redo log files used for input after performing a backup.

See [ Creating a Standby Database with Recovery Manager](creating-data-guard-standby-database-using-RMAN.md#GUID-82731D59-A20F-45DD-A235-267B3B0E38C5). 

* Oracle Global Data Services (GDS)<br>Oracle Global Data Services (GDS) applies the Oracle RAC service model to pools of globally distributed databases, providing dynamic load balancing, failover, and centralized service management for a set of replicated databases that offer common services. The set of databases can include Oracle RAC and single-instance Oracle databases interconnected through Oracle Data Guard, Oracle GoldenGate, or any other replication technology.<br>GDS is integrated with Oracle Data Guard broker. This allows role-specific global services to be automatically started and stopped as appropriate when role transitions occur within an Oracle Data Guard broker configuration. <br>GDS allows the specification of a replication lag limit for a global service. If the lag limit is exceeded at a given replica, the global service is temporarily stopped at that replica and new client requests are routed to a replica that satisfies the lag limit. The global service is automatically restarted at the original replica when the replication lag becomes less than the lag limit. 




### Oracle Active Data Guard Supports Oracle Sharding {#GUID-858549EF-B72F-4545-A5D1-C632C8ECC961}

Oracle Sharding allows you to horizontally partition data across multiple independent Oracle databases and route database connection requests to databases that contain appropriate data. Oracle Data Guard and Oracle Sharding are integrated technologies.

Sharding splits data into multiple independent databases (shards) that do not share any physical resources. Sharding is usually combined with data replication, such as that provided by Oracle Data Guard. Oracle Data Guard provides fast single-master replication of an entire Oracle Database. In an Oracle Data Guard configuration there is an updateable primary database and one or more standby databases which can be open for read-only access. Replication can improve performance and scalability of a sharded database and provide high-availability and disaster recovery. Replication topology in a sharded database is specified using the `-repl` option on the GDSCTL `create shardcatalog` command. The default replication topology is Oracle Data Guard. 

Oracle Sharding supports two methods of sharding: system-managed and composite. The sharding method is specified with the GDSCTL command `CREATE SHARDCATALOG`. 

Shards that belong to a shardgroup are usually located in the same data center. An entire shardgroup can be fully replicated to one or more shardgroups in the same or different data centers.

System-Managed Sharding With Oracle Data Guard Replication

In system-managed sharding, the logical unit of replication is a group of shards called shardgroup. In system-managed sharding, a shardgroup contains all data stored in the sharded database. The data is automatically distributed across shards using partitioning by consistent hash (consistent hash is a partitioning strategy commonly used in scalable distributed systems). The partitioning algorithm evenly and randomly distributes data across shards. The following figure illustrates how Oracle Data Guard replication is used with system-managed sharding. There is a primary shardgroup – Shardgroup 1 and two standby shardgroups - Shardgroup 2 and Shardgroup 3. 

Figure 1-4 System-Managed Sharding With Oracle Data Guard Replication

![Description of Figure 1-4 follows](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img/admin_3v_136a.png)<br>[Descriptionof "Figure 1-4 System-Managed Sharding With Oracle Data Guard Replication"](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img_text/admin_3v_136a.md)Shardgroup 1 consists of Oracle Data Guard primary databases (shards 1, 2, 3). Shardgroup 2 consists of local standby databases (shards 4, 5, 6) which are located in the same datacenter and configured for synchronous replication. And Shardgroup 3 consists of remote standbys (shards 7, 8, 9) located in a different datacenter and configured for asynchronous replication. The default replication topology is Oracle Data Guard. To open each standby in the configuration in read-only mode, you must enable Oracle Active Data Guard. This is done using the `-deploy_as ACTIVE_STANDBY` option on the GDSCTL `add shardgroup` command. 

The sharded database shown in the previous figure consists of three sets of replicated shards: {1, 4, 7}, {2, 5, 8} and {3, 6, 9}. Each set of replicated shards is managed as an Oracle Data Guard broker configuration with fast-start failover enabled. 

To deploy replication, you only need to specify the properties of shardgroups (region, role, etc) and add shards to them. Oracle Sharding automatically configures Oracle Data Guard and starts a fast-start failover observer for each set of replicated shards. It also provides load balancing of read-only workloads, role-based global services, and replication lag and locality-based routing.

To deploy the configuration shown in the previous figure, execute the following GDSCTL commands:
```
CREATE SHARDCATALOG –database host00:1521:shardcat –region dc1, dc2

ADD GSM -gsm gsm1 -listener 1571 –catalog host00:1521:shardcat –region dc1
ADD GSM -gsm gsm2 -listener 1571 –catalog host00:1521:shardcat –region dc2

ADD SHARDGROUP -shardgroup shardgroup1 -region dc1 -deploy_as primary
ADD SHARDGROUP -shardgroup shardgroup2 -region dc1 -deploy_as standby
ADD SHARDGROUP -shardgroup shardgroup3 -region dc2 -deploy_as standby

CREATE SHARD -shardgroup shardgroup1 -destination host01 -credential oracle_cred  -netparamfile /home/oracle/netca_dbhome.rsp
CREATE SHARD -shardgroup shardgroup1 -destination host02 -credential oracle_cred  -netparamfile /home/oracle/netca_dbhome.rsp
CREATE SHARD -shardgroup shardgroup1 -destination host03 -credential oracle_cred  -netparamfile /home/oracle/netca_dbhome.rsp
……
CREATE SHARD -shardgroup shardgroup3  -destination host09 -credential oracle_cred -netparamfile /home/oracle/netca_dbhome.rsp

DEPLOY
```


Composite Sharding With Oracle Data Guard Replication

Composite sharding combines features of system-managed sharding and user-managed sharding. In composite sharding the logical unit of replication is a group of shards called a shardgroup (as in system-managed sharding). Also in composite sharding, a sharded database consists of multiple shardspaces (as in user-managed sharding), however, each shardspace, instead of replicated shards, contains replicated shardgroups as shown in the following figure.

Figure 1-5 Composite Sharding With Oracle Data Guard Replication

![Description of Figure 1-5 follows](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img/admin_3v_133c.png)<br>[Descriptionof "Figure 1-5 Composite Sharding With Oracle Data Guard Replication"](https://docs.oracle.com/en/database/oracle/oracle-database/23/sbydb/img_text/admin_3v_133c.md)

To deploy the configuration shown in the previous figure, execute the following GDSCTL commands:
```
CREATE SHARDCATALOG -sharding composite –database host00:1521:cat –region dc1, dc2, dc3

ADD GSM -gsm gsm1 -listener 1571 –catalog host00:1521:cat –region dc1
ADD GSM -gsm gsm2 -listener 1571 –catalog host00:1521:cat –region dc2
ADD GSM -gsm gsm3 -listener 1571 –catalog host00:1521:cat –region dc3

ADD SHARDSPACE -shardspace shardspace_a
ADD SHARDSPACE -shardspace shardspace_b

ADD SHARDGROUP -shardgroup shardgroup_a1 –shardspace shardspace_a -region dc1
-deploy_as primary
ADD SHARDGROUP -shardgroup shardgroup_a2 –shardspace shardspace_a -region dc1     -deploy_as standby
ADD SHARDGROUP -shardgroup shardgroup_a3 –shardspace shardspace_a -region dc3     -deploy_as standby
ADD SHARDGROUP -shardgroup shardgroup_b1 –shardspace shardspace_a -region dc1
-deploy_as primary
ADD SHARDGROUP -shardgroup shardgroup_b2 –shardspace shardspace_a -region dc1     -deploy_as standby
ADD SHARDGROUP -shardgroup shardgroup_b3 –shardspace shardspace_a -region dc2     -deploy_as standby

CREATE SHARD -shardgroup shardgroup_a1 -destination host01 –credential orcl_cred  -netparamfile /home/oracle/netca_dbhome.rsp
……
CREATE SHARD -shardgroup shardgroup_b3 -destination host09 -credential orcl_cred      -netparamfile /home/oracle/netca_dbhome.rsp

DEPLOY
```


**Related Topics**

* [*Oracle Database Administrator’s Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN-GUID-0F39B1FB-DCF9-4C8A-A2EA-88705B90C5BF)
* [*Oracle Database Global Data Services Concepts and Administration Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=GSMUG-GUID-829434C7-608D-40D4-81C8-A77A63FCD7EB)
* [*Oracle Globally Distributed Database Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SHARD)



### Summary of Oracle Data Guard Benefits {#GUID-C65BF458-4BF7-4723-BAFC-5860B5B88A2B}

Oracle Data Guard provides an efficient and comprehensive disaster recovery and high availability solution. 

Oracle Data Guard offers these benefits:

* High availability<br>Oracle Data Guard’s easy-to-manage switchover and failover capabilities allow role reversals between primary and standby databases, minimizing the downtime of the primary database for planned and unplanned outages.

* Complete data protection<br>Oracle Data Guard can ensure zero data loss, even in the face of unforeseen disasters. A standby database provides a safeguard against unplanned outages of all types, including data corruption and administrative error. Because the redo data received from a primary database is validated at a standby database, physical corruptions that can occur at a primary database are not propagated to the standby database. Additional validation performed at a standby database also prevents logical intra-block corruptions and lost-write corruptions from propagating to the standby. Similarly, administrative errors such as accidental file deletions by a storage administrator are not propagated to a standby database. A physical standby database can also be used to protect against user errors either by delaying the redo apply or by using Flashback Database to rewind the standby and extract a good copy of the data.

* Efficient use of system resources<br>The standby database tables that are updated with redo data received from the primary database can be used for other tasks such as backups, reporting, summations, and queries, thereby reducing the primary database workload necessary to perform these tasks, saving valuable CPU and I/O cycles.

* Flexibility in data protection to balance availability against performance requirements <br>Oracle Data Guard offers maximum protection, maximum availability, and maximum performance modes to help enterprises balance data availability against system performance requirements.

* Automatic gap detection and resolution<br>If connectivity is lost between the primary and one or more standby databases (for example, due to network problems), then redo data being generated on the primary database cannot be sent to those standby databases. After a connection is reestablished, the missing archived redo log files (referred to as a gap) are automatically detected by Oracle Data Guard, which then automatically transmits the missing archived redo log files to the standby databases. The standby databases are synchronized with the primary database, without manual intervention by the DBA.

* Centralized and simple management<br>The Oracle Data Guard broker provides a graphical user interface and a command-line interface to automate management and operational tasks across multiple databases in an Oracle Data Guard configuration. The broker also monitors all of the systems within a single Oracle Data Guard configuration.

* Integration with Oracle Database<br>Oracle Data Guard is a feature of Oracle Database Enterprise Edition and does not require separate installation.

* Automatic role transitions<br>When fast-start failover is enabled, the Oracle Data Guard broker automatically fails over to a synchronized standby site in the event of a disaster at the primary site, requiring no intervention by the DBA. In addition, applications are automatically notified of the role transition.


