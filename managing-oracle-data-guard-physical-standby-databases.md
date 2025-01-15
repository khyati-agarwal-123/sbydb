##  10Managing Physical and Snapshot Standby Databases {#GUID-B140C38B-DE01-4252-8422-7154018DDFEC} 

This chapter discusses the various ways that physical and snapshot standby databases need to be managed. 

See the following topics: 

  * [ Starting Up and Shutting Down a Physical Standby Database ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D7026A24-78EF-4C0D-84E5-41D767516565)

  * [ Opening a Physical Standby Database ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4)

  * [ Primary Database Changes That Require Manual Intervention at a Physical Standby ](managing-oracle-data-guard-physical-standby-databases.md#GUID-62007456-DD41-431D-B0E7-56C99FEB2277)

  * [ Recovering Through the OPEN RESETLOGS Statement ](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A)

  * [ Monitoring Primary, Physical Standby, and Snapshot Standby Databases ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486)

  * [ Replicating Restore Points from Primary to Standby ](managing-oracle-data-guard-physical-standby-databases.md#GUID-45D7D60E-ED0F-4B3E-9D32-EB5394196CAF)

  * [ Tuning Redo Apply ](managing-oracle-data-guard-physical-standby-databases.md#GUID-EEDAC95A-828F-4117-AF57-D8ABA702D42D)

  * [ Tuning Databases in an Active Data Guard Environment with SQL Tuning Advisor ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2FF52A94-828C-4BF1-A5D0-C70A05A8198A)

  * [ Using Oracle Diagnostic Pack to Tune Oracle Active Data Guard Standbys ](managing-oracle-data-guard-physical-standby-databases.md#GUID-1E2A26E3-B0E2-4ED1-A50D-891505C86E7D)

  * [ Managing a Snapshot Standby Database ](managing-oracle-data-guard-physical-standby-databases.md#GUID-CE30E2D6-5B53-4389-8B02-FC0F341C8C1A)




See [ *Oracle Data Guard Broker* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR001) to learn how the Oracle Data Guard broker simplifies the management of physical and snapshot standby databases. 

###  Starting Up and Shutting Down a Physical Standby Database {#GUID-D7026A24-78EF-4C0D-84E5-41D767516565} 

This section describes how to start up and shut down a physical standby database. 

####  Starting Up a Physical Standby Database {#GUID-76930886-6460-45AE-86CA-84DCB41E5BE5} 

Use the SQL*Plus ` STARTUP ` command to start a physical standby database. 

The SQL*Plus ` STARTUP ` command starts, mounts, and opens a physical standby database in read-only mode when it is invoked without any arguments. 

After it has been mounted or opened, a physical standby database can receive redo data from the primary database. 

See [ Applying Redo Data to Physical Standby Databases ](oracle-data-guard-redo-apply-services.md#GUID-2B839172-947E-48A4-9FFD-33CC6907809F) for information about Redo Apply and [ Opening a Physical Standby Database ](managing-oracle-data-guard-physical-standby-databases.md#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4) for information about opening a physical standby database in read-only mode. 

> **note:** 

When Redo Apply is started on a physical standby database that has not yet received redo data from the primary database, an ` ORA-01112 ` message may be returned. This indicates that Redo Apply is unable to determine the starting sequence number for media recovery. If this occurs, manually retrieve an archived redo log file from the primary database and register it on the standby database, or wait for redo transport to begin before starting Redo Apply. 

####  Shutting Down a Physical Standby Database {#GUID-B51873F4-F50C-4757-9DF9-D43635BFAD7F} 

Use the SQL*Plus ` SHUTDOWN ` command to stop Redo Apply and shut down a physical standby database. 

Control is not returned to the session that initiates a database shutdown until shutdown is complete. 

If the primary database is up and running, defer the standby destination on the primary database and perform a log switch before shutting down the physical standby database. 

###  Opening a Physical Standby Database {#GUID-D5FB88EC-799D-40E7-80E1-19474E3167E4} 

A physical standby database can be opened for read-only access and used to offload queries from a primary database. 

> **note:** 

A physical standby database that is opened in read-only mode is subject to the same restrictions as any other Oracle database opened in read-only mode. For more information, see [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN11154) . 

If a license for the Oracle Active Data Guard  option has been purchased, Redo Apply can be active while the physical standby database is open, thus allowing queries to return results that are identical to what would be returned from the primary database. This capability is known as the real-time query  feature.  See [ Real-time query ](managing-oracle-data-guard-physical-standby-databases.md#GUID-07CB190C-C248-4FF5-AB64-EAA9C6D42677) for more details. 

If a license for the Oracle Active Data Guard option has not been purchased, a physical standby database cannot be open while Redo Apply is active, so the following rules must be observed when opening a physical standby database instance or starting Redo Apply: 

  * Redo Apply must be stopped before any physical standby database instance is opened. 

  * If one or more physical standby instances are open, those instances must be stopped or restarted in a mounted state before starting Redo Apply. 




> **note:** See Also: 

  * [ *Oracle Database Licensing Information* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DBLIC-GUID-AB56CEE3-955E-4E56-8B44-6075E889C283) for more information about Oracle Active Data Guard 




####  Real-time Query {#GUID-07CB190C-C248-4FF5-AB64-EAA9C6D42677} 

The ` COMPATIBLE ` database initialization parameter must be set to 11.0 or higher to use the real-time query feature of the Oracle Active Data Guard option. 

A physical standby database instance cannot be opened if Redo Apply is active on a mounted instance of that database. Use the following SQL statements to stop Redo Apply, open a standby instance read-only, and restart Redo Apply: 
    
    
    ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;
    ```

> **note:** 

If Redo Apply is active on an open instance, additional instances can be opened without having to stop Redo Apply. 

Redo Apply cannot be started on a mounted physical standby instance if any instance of that database is open. The instance must be opened before starting Redo Apply on it. 

**Example: Querying V$DATABASE to Check the Standby's Open Mode** 

This example shows how the value of the ` V$DATABASE.OPEN_MODE ` column changes when a physical standby is open in real-time query mode. 

  1. Start up and open a physical standby instance, and perform the following SQL query to show that the database is open in read-only mode: 
    
        ```
    SQL> SELECT open_mode FROM V$DATABASE;
     
    OPEN_MODE
    --------------------
    READ ONLY
    
    ```

  2. Issue the following SQL statement to start Redo Apply: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;
     
    Database altered.
    
    ```

  3. Now that the standby is in real-time query mode (the standby is open in read-only mode and Redo Apply is active), the ` V$DATABASE.OPEN_MODE ` column changes to indicate the following: 
    
        ```
    SQL> SELECT open_mode FROM V$DATABASE;
     
    OPEN_MODE
    --------------------
    READ ONLY WITH APPLY
    ```




#####  Monitoring Apply Lag in a Real-time Query Environment {#GUID-C225DCBC-E9FB-4E86-9D19-A5D20C3A8AA5} 

If you are using real-time query to offload queries from a primary database to a physical standby database, you can monitor the apply lag to ensure that it is within acceptable limits. 

The current apply lag is the difference, in elapsed time, between when the last applied change became visible on the standby and when that same change was first visible on the primary. This metric is computed to the nearest second. 

To obtain the apply lag, query the ` V$DATAGUARD_STATS ` view. For example: 
    
    
    ```
    SQL> SELECT name, value, datum_time, time_computed FROM V$DATAGUARD_STATS -
    > WHERE name like 'apply lag';
         
        NAME         VALUE            DATUM_TIME              TIME_COMPUTED
        ---------    -------------    -------------------     -------------------
        apply lag    +00 00:00:00     05/27/2009 08:54:16     05/27/2009 08:54:17
    
    ```

The ` apply ` ` lag ` metric is computed using data that is periodically received from the primary database. The ` DATUM_TIME ` column contains a timestamp of when this data was last received by the standby database. The ` TIME_COMPUTED ` column contains a timestamp taken when the ` apply ` ` lag ` metric was calculated. The difference between the values in these columns should be less than 30 seconds. If the difference is larger than this, the ` apply ` ` lag ` metric may not be accurate. 

To obtain a histogram that shows the history of apply lag values since the standby instance was last started, query the ` V$STANDBY_EVENT_HISTOGRAM ` view. For example: 
    
    
    ```
    SQL> SELECT * FROM V$STANDBY_EVENT_HISTOGRAM WHERE NAME = 'apply lag' - 
    > AND COUNT > 0;
    
    NAME             TIME       UNIT         COUNT        LAST_TIME_UPDATED
    ---------     ---------   --------    -----------    ------------------------
    apply lag         0        seconds        79681          06/18/2009 10:05:00
    apply lag         1        seconds         1006          06/18/2009 10:03:56
    apply lag         2        seconds           96          06/18/2009 09:51:06
    apply lag         3        seconds            4          06/18/2009 04:12:32
    apply lag         4        seconds            1          06/17/2009 11:43:51
    apply lag         5        seconds            1          06/17/2009 11:43:52
    
    6 rows selected
    
    ```

To evaluate the apply lag over a time period, take a snapshot of ` V$STANDBY_EVENT_HISTOGRAM ` at the beginning of the time period and compare that snapshot with one taken at the end of the time period. 

#####  Configuring Apply Lag Tolerance in a Real-time Query Environment {#GUID-622B6596-54F7-4736-8A8B-0C5E308AC3F5} 

The ` STANDBY_MAX_DATA_DELAY ` session parameter can be used to specify a session-specific apply lag tolerance, measured in seconds, for queries issued by non-administrative users to a physical standby database that is in real-time query mode. 

This capability allows queries to be safely offloaded from the primary database to a physical standby database, because it is possible to detect if the standby database has become unacceptably stale. 

If ` STANDBY_MAX_DATA_DELAY ` is set to the default value of ` NONE ` , than queries issued to a physical standby database are executed regardless of the apply lag on that database. 

If ` STANDBY_MAX_DATA_DELAY ` is set to a nonzero value, then queries issued to a physical standby database are executed only if the apply lag is less than or equal to ` STANDBY_MAX_DATA_DELAY ` . Otherwise, an ` ORA-3172 ` error is returned to alert the client that the apply lag is too large. 

If ` STANDBY_MAX_DATA_DELAY ` is set to 0, a query issued to a physical standby database is guaranteed to return the exact same result as if the query were issued on the primary database, unless the standby database is lagging behind the primary database, in which case an ` ORA-3172 ` error is returned. 

Use the ` ALTER SESSION ` SQL statement to set ` STANDBY_MAX_DATA_DELAY ` . For example: 
    
    
    ```
    SQL> ALTER SESSION SET STANDBY_MAX_DATA_DELAY=2
    ```

#####  Forcing Redo Apply Synchronization in a Real-time Query Environment {#GUID-194B1448-84FB-4357-A6AD-72A77701F918} 

To ensure that all redo data received from the primary database has been applied to a physical standby database, you can use a SQL ` ALTER SESSION ` statement. 

Issue the following SQL statement: 
    
    
    ```
    SQL> ALTER SESSION SYNC WITH PRIMARY;
    
    ```

This statement blocks until all redo data received by the standby database at the time that this command is issued has been applied to the physical standby database. An ` ORA-3173 ` error is returned immediately, and synchronization does not occur, if the redo transport status at the standby database is not ` SYNCHRONIZED ` or if Redo Apply is not active. 

To ensure that Redo Apply synchronization occurs in specific cases, use the ` SYS_CONTEXT('USERENV','DATABASE_ROLE') ` function to create a standby-only trigger (enabled on the primary but that only takes certain actions if it is running on a standby). For example, you could create the following trigger that would execute the ` ALTER SESSION SYNC WITH PRIMARY ` statement for a specific user connection at logon: 
    
    
    ```
    CREATE TRIGGER adg_logon_sync_trigger
     AFTER LOGON ON user.schema
      begin
        if (SYS_CONTEXT('USERENV', 'DATABASE_ROLE')  IN ('PHYSICAL STANDBY')) then
          execute immediate 'alter session sync with primary';
        end if;
      end;
    ```

#####  Real-time Query Restrictions {#GUID-25EF4F01-3DF0-4F21-99B8-95178CA003BE} 

This list discusses restrictions related to real-time query mode. 

  * The apply lag control and Redo Apply synchronization mechanisms described above require that the client be connected and issuing queries to a physical standby database that is in real-time query mode. 

  * The following additional restrictions apply if ` STANDBY_MAX_DATA_DELAY ` is set to 0 or if the ` ALTER SESSION SYNC WITH PRIMARY ` SQL statement is used: 

    * The standby database must receive redo data via the SYNC transport. 

    * The redo transport status at the standby database must be SYNCHRONIZED and the primary database must be running in either maximum protection mode or maximum availability mode. 

    * Real-time apply must be enabled. 

  * Oracle Active Data Guard achieves high performance of real-time queries in an Oracle RAC environment through the use of cache fusion. This allows the Oracle Data Guard apply instance and queries to work out of cache and not be slowed down by disk I/O limitations. 

A consequence of this is that an unexpected failure of the apply instance leaves buffers in inconsistent states across all the open Oracle RAC instances. If the database is open on any other instances, one of the open instances performs Active Data Guard instance recovery to bring the database to a consistent state and all the open instances remain open. 

> **note:** See Also: 
    * [ *Oracle Data Guard Broker* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR260) for more information about how the broker handles apply instance failures 

    * The My Oracle Support note 1357597.1 at [ ` http://support.oracle.com ` ](http://support.oracle.com) for additional information about apply instance failures in an Oracle Active Data Guard Oracle RAC standby 




#####  Automatic Block Media Recovery {#GUID-67B644A3-161B-4A83-A0B2-923BF51AD932} 

If corrupt data blocks are encountered when a database is accessed, they can be automatically replaced with uncorrupted copies of those blocks. 

This requires the following conditions: 

  * The physical standby database must be operating in real-time query mode, which requires an Oracle Active Data Guard license. 

  * The physical standby database must be running real-time apply. 




Automatic block media recovery works in two directions depending on whether the corrupted blocks are encountered on the primary or on the standby. 

**Corrupted Blocks On the Primary** 

If corrupt data blocks are encountered at a primary database, then the primary automatically searches for good copies of those blocks on a standby and, if they are found, has them shipped back to the primary. 

The primary requires a ` LOG_ARCHIVE_DEST_ ` *n* to the standby only (a physical standby, a cascading physical standby, or a far sync instance). The primary does not require a ` LOG_ARCHIVE_DEST_ ` *n* to any terminal destinations; it is able to automatically ascertain their service names. 

**Corrupted Blocks On a Standby** 

If corrupt data blocks are encountered at a standby, then the standby automatically initiates communication with the primary and requests uncorrupted copies of those blocks. For the primary to be able to ship the uncorrupted blocks to the standby, the following database initialization parameters must be configured on the standby. This is true even if the primary does not directly service the standby (for example, in cascading configurations). 

  * The ` LOG_ARCHIVE_CONFIG ` parameter is configured with a ` DG_CONFIG ` list and a ` LOG_ARCHIVE_DEST_ ` *n* parameter is configured for the primary database. 

or 

  * The ` FAL_SERVER ` parameter is configured and its value contains an Oracle Net service name for the primary database. 




**Additional Automatic Block Media Repair Considerations** 

  * Automatic repair is supported with any Oracle Data Guard protection mode. However, the effectiveness of repairing a corrupt block at the primary using the non-corrupt version of the block from the standby depends on how closely the standby apply is synchronized with the redo generated by the primary. 

  * When an automatic block repair has been performed, a message is written to the database alert log. 

  * If automatic block repair is not possible, an ` ORA-1578 ` error is returned. 




#####  Manual Block Media Recovery {#GUID-B2A61261-304D-4C14-AEF3-DA6E1B4368B3} 

The RMAN ` RECOVER BLOCK ` command is used to manually repair a corrupted data block. 

This command searches several locations for an uncorrupted copy of the data block. By default, one of the locations is any available physical standby database operating in real-time query mode. The ` EXCLUDE STANDBY ` option of the RMAN ` RECOVER BLOCK ` command can be used to exclude physical standby databases as a source for replacement blocks. 

> **note:** See Also: 

[ *Oracle Database Backup and Recovery Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF140) for more information about the RMAN ` RECOVER BLOCK ` command 

#####  Tuning Queries on a Physical Standby Database {#GUID-12950D5A-FE9F-4444-A6DE-26FCC5653225} 

Queries on a physical standby database can be tuned for optimal performance. 

For details about tuning queries, see the *Active Data Guard Best Practices* technical brief available on the Oracle Maximum Availability Architecture (MAA) home page at: 

[ ` http://www.oracle.com/goto/maa ` ](http://www.oracle.com/goto/maa)

**Force Full Database Caching Mode** 

The use of force full database caching mode can potentially improve performance because queries are executed faster. 

The enabling and disabling of force full database caching mode  is not recorded in redo, so the status of in-memory caching is not necessarily the same on all members of a Data Guard configuration. 

For more information about the Force Full Database In-Memory Caching feature, including guidelines on how and when to enable and disable it, see [ *Oracle Database Performance Tuning Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=TGDBA95384) . 

#####  Adding Temp Files to a Physical Standby {#GUID-40A7D7C1-72B8-4397-B61B-446028102E2D} 

If you are using a standby to offload queries from the primary database, then the standby must be configured with the minimum of one temp tablespace with at least one temporary data file. 

If the nature of the workload requires more temp table space than is automatically created when the standby is first created, then you may need to manually add additional space. 

To add temporary files to the physical standby database, perform the following tasks: 

  1. Identify the tablespaces that contain temporary files. Do this by entering the following command on the standby database: 
    
        ```
    SQL> SELECT TABLESPACE_NAME FROM DBA_TABLESPACES
    2> WHERE CONTENTS = 'TEMPORARY';
     
    TABLESPACE_NAME
    --------------------------------
    TEMP1
    TEMP2
    
    ```

  2. For each tablespace identified in the previous query, add a new temporary file to the standby database. The following example adds a new temporary file called ` TEMP1 ` with size and reuse characteristics that match the primary database temporary files: 
    
        ```
    SQL> ALTER TABLESPACE TEMP1 ADD TEMPFILE
    2> '/arch1/boston/temp01.dbf'
    3> SIZE 40M REUSE;
    
    ```




######  Automatic Temp File Creation {#TASK_T5H_1ZL_X5B} 

In previous resleases, Oracle Data Guard did not automatically replicate temp file configuration changes from a primary to a standby database because the database did not generate redo for such operations (add/remove temp file; increase/decrease temp file size). As a consequence, DBAs needed to design and program their own automation and replication logic to keep temp file configurations in-sync between primary and standby databases.With the introduction of Oracle Dabase 23ai, automatic temp file creation for the standby database is possible. In addition, all temp file related DDLs currently supported for on a normal CDB level physical standby are supported for a Data Guard PDB (DG PDB). DDLs must be executed from within a DG PDB container. 

####  Using SQL and PL/SQL on Active Data Guard Standbys {#GUID-B1C66720-3EA6-45A3-986E-6E510381EC79} 

Starting with Oracle Database Release 19c, you can perform SQL and PL/SQL operations in Active Data Guard standby databases. 

**Related Topics**

  * [ Performing DML Operations on Active Data Guard Standby Databases ](managing-oracle-data-guard-physical-standby-databases.md#GUID-8AAD002C-ED06-4349-8BB5-EC8DB30B2628)
  * [ Running Top-level PL/SQL Operations on Active Data Guard Standby Databases ](managing-oracle-data-guard-physical-standby-databases.md#GUID-143121BA-3F4C-4953-B0E7-68BE9EE700C9)
  * [ Automatic Recompliation of Modified PL/SQL Objects ](managing-oracle-data-guard-physical-standby-databases.md#GUID-21156FB5-6BBF-4D0B-B914-D69DC3C61A2C)



#####  Performing DML Operations on Active Data Guard Standby Databases {#GUID-8AAD002C-ED06-4349-8BB5-EC8DB30B2628} 

You can run DML operations on Active Data Guard standby databases. This enables you to run read-mostly applications, which occasionally execute DMLs, on the standby database. 

DML operations on a standby can be transparently redirected to and run on the primary database. This includes DML statements that are part of PL/SQL blocks. The Active Data Guard session waits until the corresponding changes are shipped to and applied to the Active Data Guard standby. Read consistency is maintained during the DML operation and the standby database on which the DML is run can view its uncommitted changes. However, all the other standby database instances can view these changes only after the transaction is committed. 

> **note:** 

Avoid running too may DML operations on Active Data Guard standby databases. Because the operations are actually performed on the primary, too many DMLs may impact the performance of the primary. 

> **note:** DML operations in Oracle XA transactions are not supported on Active Data Guard standby databases. 

Automatic redirection of DML operations to the primary can be configured at the system level or the session level. The session level setting overrides the system level setting. 

To configure automatic redirection of DML operations for all standby sessions in an Active Data Guard environment: 

  * Set the ` ADG_REDIRECT_DML ` initialization parameter to ` TRUE ` . 



To configure automatic redirection of DML operations for the current session, use the following command: 

  * ` ALTER SESSION ENABLE ADG_REDIRECT_DML; `



Example 10-1 Performing DML Operations on a Physical Standby Database 

The physical standby database in an Active Data Guard setup contains a table named ` employees ` . You can insert rows into this table by running DML on a physical standby database in the Active Data Guard environment. 

On the standby database, enable DML redirection for the current session: 
    
    
    ```
    SQL> ALTER SESSION ENABLE ADG_REDIRECT_DML;
    ```

Add a row to the ` employees ` table using the following command: 
    
    
    ```
    SQL> INSERT INTO employees VALUES (.......);
    ```

At this point, the changed data is visible only to the standby database on which the command was run. After the insert operation is committed on the primary database, the changes are shipped back and applied to all the standby databases. 

#####  Running Top-level PL/SQL Operations on Active Data Guard Standby Databases {#GUID-143121BA-3F4C-4953-B0E7-68BE9EE700C9} 

Top-level PL/SQL blocks that you run on Active Data Guard standby databases can be redirected to and run on the primary database, if they do not contain bind variables. 

To redirect top-level PL/SQL operations that are run on a standby to the primary, configure automatic redirection using the following command on the standby database: 

  * ` ALTER SESSION ENABLE ADG_REDIRECT_PLSQL; `



You can configure automatic redirection for top-level PL/SQL operations only at the session level. 

#####  Automatic Recompliation of Modified PL/SQL Objects {#GUID-21156FB5-6BBF-4D0B-B914-D69DC3C61A2C} 

PL/SQL objects that are run on standby instances can be recompiled, if they are invalid. 

PL/SQL objects become invalid when their dependent objects are modified or dropped. Starting with Oracle Database Release 19c, invalidated PL/SQL objects run on a standby database can be automatically recomplied by setting the ` ADG_REDIRECT_DML ` initialization parameter to ` TRUE ` . The DDL corresponding to these PL/SQL objects is redirected to and executed on the primary database. The standby session waits until the operation is completed. 

Example 10-2 Automatically Recompiling Modified PL/SQL Objects 

On the primary database in an Active DataGuard environment, a procedure named ` insert_empl ` is created. This procedure is used to update the ` employees ` table. 
    
    
    ```
    CREATE OR REPLACE PROCEDURE update_sal (emp_id IN NUMBER,sal IN NUMBER)
    AS BEGIN
       UPDATE employees SET salary=sal WHERE employee_id=emp_id);
    END;
    ```

The structure of the ` employees ` table is subsequently modified using an ` ALTER TABLE ` command. This invalidates the ` update_sal ` procedure. 

On a standby database in the Active DataGuard environment, you want to use the ` update_sal ` procedure to update the salary of an employee. Run the following commands on the physical standby: 
    
    
    ```
    SQL> ALTER SESSION ENABLE ADG_REDIRECT_DML;
    SQL> exec update_sal(105,6000);
    ```

The user running these commands has been granted permission to execute the ` update_sal ` procedure. 

####  Using Temporary Tables on Active Data Guard Instances {#GUID-A182664E-59F7-4C96-953F-406D0FB6573D} 

You can create both global temporary tables and private temporary tables on an Active Data Guard instance. 

A temporary table holds data that exists only for the duration of a transaction or session. Data in a temporary table is private to the session. Each session can only see and modify its own data. 

> **note:** See Also: 

  * [ *Oracle Database Concepts*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=CNCPT-GUID-23B23DCF-7482-4585-9C63-AC073C5DE224) for descriptions of global temporary tables, private temporary tables, and the differences between them 




#####  Global Temporary Tables on Active Data Guard Instances {#GUID-DAF341CA-69C9-497D-A50C-FDC89ED9C1D8} 

DML and DDL operations are allowed on temporary tables on Oracle Active Data Guard instances. 

**DML Operations** 

When a global temporary table is changed by a DML operation, the change itself does not generate redo because it is only a temporary table. But the undo generated for the change does in turn generate redo. Redo generation on a read-only database (such as an Active Data Guard standby) is not allowed. However, DML operations on global temporary tables are allowed on Oracle Active Data Guard standbys because the temporary undo feature allows the undo for changes to a global temporary table to be stored in the temporary tablespace as opposed to the undo tablespace. And undo stored in the temporary tablespace does not generate redo. 

This feature benefits Oracle Data Guard in the following ways: 

  * Read-mostly reporting applications that use global temporary tables for storing temporary data can be offloaded to an Oracle Active Data Guard instance. 

  * When temporary undo is enabled on the primary database, undo for changes to a global temporary table are not logged in the redo and thus, the primary database generates less redo. Therefore, the amount of redo that Oracle Data Guard must ship to the standby is also reduced, thereby reducing network bandwidth consumption and storage consumption. 




To enable temporary undo on the primary database, use the ` TEMP_UNDO_ENABLED ` initialization parameter. On an Oracle Active Data Guard standby, temporary undo is always enabled by default so the ` TEMP_UNDO_ENABLED ` parameter has no effect. 

**Restrictions** 

  * The temporary undo feature requires that the database initialization parameter ` COMPATIBLE ` be set to 12.0.0 or higher. 

  * The temporary undo feature on Oracle Active Data Guard instances does not support temporary ` BLOB ` s or temporary ` CLOB ` s. 

  * Distributed transactions on an Oracle Active Data Guard instance are not permitted if they involve changes to local objects. For example, you cannot commit a transaction that modifies a global temporary table on the Oracle Active Data Guard instance and also updates a remote table on another database using a database link. You must commit or roll back any outstanding DML operations to global temporary tables on the Active Data Guard instance before issuing a remote DML operation, or vice versa. This also includes implicit writes to global temporary tables made by operations such as ` EXPLAIN PLAN ` statements. 




> **note:** See Also: 

  * [ *Oracle Database Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN13740) for more information about temporary undo 

  * [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10326) for more information about the ` TEMP_UNDO_ENABLED ` initialization parameter 




**DDL Operations** 

Global temporary tables can be created on, and dropped from, Active Data Guard standby databases. The DDL for these operations is transparently redirected to the primary database. The Active Data Guard session then waits until the corresponding changes are shipped and applied to the Active Data Guard standby. The following is an example of creating a global temporary table: 
    
    
    ```
    SQL> CREATE GLOBAL TEMPORARY TABLE tab2(c1 number, c2 varchar(10)) ON COMMIT PRESERVE ROWS;  
    
    Table created. 
    SQL>
    ```

> **note:** 

For the DDL redirection to succeed, managed standby recovery at the Active Data Guard standby database must be started with the real-time apply option and the Active Data Guard standby database must be in sync with the primary database. 

> **note:** 

Data definition language (DDL) operations on global temporary tables (for example, ` CREATE ` and ` DROP ` ) can still be issued from the primary database. DDL changes are visible on the standby when it catches up with the primary database. 

#####  Private Temporary Tables on Active Data Guard Instances {#GUID-92A71AA4-BEEB-4C2B-B663-FC6641059E39} 

You can create private temporary tables on Oracle Active Data Guard instances even though they are read-only. 

The reason private temporary tables can be created in read-only databases is that their metadata is stored in memory, rather than on disk. The lifetime of a private temporary table is only during the session which created it and it gets dropped automatically when the session ends. This functionality allows reporting applications to run on Active Data Guard standby databases. 

> **note:** 

  * [ *Oracle Database Administrator’s Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN-GUID-9B373086-0760-4B18-9688-BACFF07EC74B) for information about creating private temporary tables 



####  IM Column Store in an Active Data Guard Environment {#GUID-9796284E-DF94-4F88-8CF6-7F966830D3AA} 

As of Oracle Database 12 *c* Release 2 (12.2.0.1), the Oracle Database In-Memory column store (IM column store) is supported on a standby database in an Active Data Guard (ADG) environment. 

A reporting workload executing on an Active Data Guard standby database can use the IM column store. Using the IM column store improves the execution performance of the workload because it can take full advantage of accessing data in a compressed columnar format, in memory. Additionally, it is possible to populate a completely different set of data in the IM column store on the primary and standby databases, effectively doubling the size of the IM column store available to the application. To enable IM column store support with multi-instance redo apply, set the initialization parameter ` enable_imc_with_mira ` to TRUE. 

Note the following restrictions: 

  * In-Memory Expressions are captured based only on the queries executed on the primary database. 

  * In-Memory Information Lifecycle Management (ILM) polices based on access criteria are triggered based only on access recorded on the primary database. 




> **note:** 

  * In-Memory FastStart is not supported on a standby database in an ADG environment. 

  * In-Memory Join-Groups are not supported on a standby database in an ADG environment. 




**Related Topics**

  * [ *Oracle Database In-Memory Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=INMEM-GUID-1B891BD1-66E8-40CD-BCE0-C3532FC36222)
  * [ *Oracle Database Reference*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN-GUID-124E6841-C552-4DA8-96D4-83CE16B6B6AB)



####  In-Memory External Tables in an Active Data Guard Environment {#GUID-37A7C87D-61C3-4C8C-A592-1C5B42BE021A} 

Oracle Active Data Guard supports In-Memory external tables. 

In-Memory external tables, both on a primary and standby database, are loaded in parallel. Queries that are run in an Active Data Guard environment use the In-Memory external table segments with parallel query. Using ` INMEMORY ` or ` NO INMEMORY ` results in the In-Memory external table segment on an active standby database being released. 

> **note:** 

Using the ` FOR SERVICE ` subclause of the ` INMEMORY...DISTRIBUTE ` clause when populating an ` INMEMORY ` object in the IM column store is not supported for the primary database, standby database, or both. 

> **note:** See Also: 

[ *Oracle Database In-Memory Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=INMEM-GUID-37E2E218-33A2-490E-891D-B627906DE16B#GUID-37E2E218-33A2-490E-891D-B627906DE16B)

####  Using Sequences in Oracle Active Data Guard {#GUID-DFDC929C-7446-413B-A25B-7F11284D900F} 

In an Oracle Active Data Guard environment, sequences created by the primary database with the default ` CACHE ` and ` NOORDER ` options can be accessed from standby databases as well. 

When a standby database accesses such a sequence for the first time, it requests that the primary database allocate a range of sequence numbers. The range is based on the cache size and other sequence properties specified when the sequence was created. Then the primary database allocates those sequence numbers to the requesting standby database by adjusting the corresponding sequence entry in the data dictionary. When the standby has used all the numbers in the range, it requests another range of numbers. 

The primary database ensures that each range request from a standby database gets a range of sequence numbers that do not overlap with the ones previously allocated for both the primary and standby databases. This generates a unique stream of sequence numbers across the entire Oracle Data Guard configuration. 

Because the standby's requests for a range of sequences involve a round-trip to the primary, be sure to specify a large enough value for the ` CACHE ` keyword when you create a sequence to be used on an Oracle Active Data Guard standby. Otherwise, performance could suffer. 

Also, be sure the terminal standby has a ` LOG_ARCHIVE_DEST_ ` *n* parameter defined that points back to the primary. 

**Example: Assigning a Range of Sequence Values In a Multi-standby Configuration** 

This example shows how a range of sequence values can be assigned to a database when it references ` NEXTVAL ` on the sequence either for the first time or after it uses up all of the previously assigned sequence values. In this example, there are two standby databases. 

  1. On the primary database, issue the following SQL statements to create a global temporary table named ` gtt ` , and a sequence named ` g ` with a cache size of 10: 
    
        ```
    SQL> CREATE GLOBAL TEMPORARY TABLE gtt (a int);
     
    Table created. 
     
    SQL> CREATE SEQUENCE g CACHE 10;
     
    Sequence created.
    
    ```

  2. On the first standby database, issue the following SQL statements: 
    
        ```
    SQL> INSERT INTO gtt VALUES (g.NEXTVAL);
     
    1 row created.
     
    SQL> INSERT INTO gtt VALUES (g.NEXTVAL);
     
    1 row created.
     
    SQL> SELECT * FROM gtt;
     
             A
    ----------
             1
             2
    
    ```

Because the sequence cache size was set to 10 (in Step 1) and because this is the first time the sequence was accessed, the results of the ` SELECT ` statement show that the first standby database is assigned sequence numbers 1 to 10. 

  3. On the primary database, issue the following SQL statements: 
    
        ```
    SQL> SELECT g.NEXTVAL FROM dual;
     
       NEXTVAL
    ----------
            11
     
    SQL> SELECT g.NEXTVAL FROM dual;
     
       NEXTVAL
    ----------
            12
    
    ```

The results of the ` SELECT ` statements show that the primary database is assigned the next range of sequence values, 11-20. 

  4. On the second standby database, issue the following SQL statements: 
    
        ```
    SQL> INSERT INTO gtt VALUES (g.NEXTVAL);
     
    1 row created.
     
    SQL> INSERT INTO gtt VALUES (g.NEXTVAL);
     
    1 row created.
      
    SQL> SELECT * FROM gtt;
     
             A
    ----------
            21
            22
    
    ```

The results of the ` SELECT ` statement show that the second standby is assigned the next range of sequence values, 21-30. 

> **note:** Sequences created with the ` ORDER ` or ` NOCACHE ` options cannot be accessed on an Oracle Active Data Guard standby. 




#####  Session Sequences {#GUID-8A73B0A3-D77D-4CF6-ABCB-6293BB26B6D1} 

A session sequence is a special type of sequence that is specifically designed to be used with global temporary tables that have session visibility. 

Unlike the existing regular sequences (referred to as "global" sequences for the sake of comparison), a session sequence returns a unique range of sequence numbers only within a session, but not across sessions. Another difference is that session sequences are not persistent. If a session goes away, so does the state of the session sequences that were accessed during the session. 

Session sequences support most of the sequence properties that are specified when the sequence is defined. However, the ` CACHE ` / ` NOCACHE ` and ` ORDER ` / ` NOORDER ` options are not relevant to session sequences and are ignored. 

Session sequences must be created by a read/write database but can be accessed on any read/write or read-only databases (either a regular database temporarily open read-only or a standby database). 

**Creating and Altering Session Sequences** 

To create a session sequence, issue the following SQL statement: 
    
    
    ```
    SQL> CREATE SEQUENCE … SESSION;
    
    ```

To alter an existing session sequence to be a regular ("global") sequence, issue the following SQL statement: 
    
    
    ```
    SQL> ALTER SEQUENCE … GLOBAL;
    
    ```

To alter a regular sequence to be a session sequence, issue the following SQL statement: 
    
    
    ```
    SQL> ALTER SEQUENCE … SESSION;
    ```

**Example: Using Session Sequences** 

This example shows how session sequence values are unique to each database session. 

  1. On the primary database, issue the following SQL statements to create a global temporary table named ` gtt ` and a session sequence named ` s ` : 
    
        ```
    SQL> CREATE GLOBAL TEMPORARY TABLE gtt (a int);
     
    Table created.
     
    SQL> CREATE SEQUENCE s SESSION;
     
    Sequence created.
    
    ```

  2. On the standby database, issue the following SQL statements: 
    
        ```
    SQL> INSERT INTO gtt VALUES (s.NEXTVAL);
     
    1 row created.
     
    SQL> INSERT INTO gtt VALUES (s.NEXTVAL);
     
    1 row created.
     
     
    SQL> SELECT * FROM gtt;
     
             A
    ----------
             1
             2
    
    ```

  3. From another session of the same standby database, issue the following SQL statements: 
    
        ```
    SQL> INSERT INTO gtt VALUES (s.NEXTVAL);
     
    1 row created.
     
    SQL> INSERT INTO gtt VALUES (s.NEXTVAL);
     
    1 row created.
     
     
    SQL> SELECT * FROM gtt;
     
             A
    ----------
             1
             2
    
    ```

The results of the ` SELECT ` statement show that the sequence values assigned are the same as those assigned for the first session in the previous step. This is because sequence values are unique to each database session. 




####  Using the Result Cache on Physical Standby Databases {#GUID-05508445-ED4C-4C5B-91EC-07B8C010BC4F} 

In an Active Data guard environment, using the result cache on physical standby databases enables you to run queries, without performance impact. 

By default, query results on physical standbys are not stored in the result cache. To improve the performance for recurring queries that are run on the standby, and that can be mapped to cached results, you can enable the use of result cache. Identify objects that are frequently used in queries and enable the use of result cache for these objects. A query result is stored in the result cache only if all the dependent objects of the query are enabled for the result cache. The ` STANDBY ENABLE ` clause in the ` CREATE TABLE ` command and ` ALTER TABLE ` command is used to specify that a table is enabled for the result cache on physical standbys. Query results from views are not stored in the result cache. 

If a query involves multiple tables, the result cache is used while executing this query only if result cache usage has been enabled for all the tables included in the query. 

> **note:** 

Enabling the result cache for standbys works for tables only. Executing a query against a view does not use the result cache even though the underlying base tables are enabled for the result cache. 

Each PDB in a CDB can have its own result cache. You can enable the result cache for one or more PDBs in a CDB. 

You must enable the use of result cache for each table that will be used in a query. 

To enable the use of result cache for queries that are run against the physical standby database: 

  * If the table already exists, modify the table definition using an ` ALTER TABLE ` command with the ` STANDBY ENABLE ` clause. 
  * If the table is being created, include the ` STANDBY ENABLE ` clause in the ` CREATE TABLE ` command. 



Example 10-3 Enabling the Result Cache for an Existing Table 

The following example modifies the definition of the ` employee ` table and enables the use of the standby result cache for this table. 
    
    
    ```
    ALTER TABLE employee RESULT_CACHE (STANDBY ENABLE);
    ```

Example 10-4 Enabling the Result Cache for Queries Containing Hints 

The following example shows how to enable the use of result cache on standby during table creation time. 
    
    
    ```
    CREATE TABLE EMPLOYEE (emp_id number, ename varchar2(50), sal number)
          RESULT_CACHE (STABLE ENABLE);
    ```

Other ` RESULT_CACHE ` attributes have the same meaning on the standby as on the primary. 

###  Primary Database Changes That Require Manual Intervention at a Physical Standby {#GUID-62007456-DD41-431D-B0E7-56C99FEB2277} 

Most structural changes made to a primary database are automatically propagated through redo data to a physical standby database, but there are some changes that require manual intervention. 

The following table lists primary database structural and configuration changes that require manual intervention at a physical standby database. 

**Table: Primary Database Changes That Require Manual Intervention at a Physical Standby** 

Primary Database Change  |  Action Required on Physical Standby Database   
---|---  
[ Adding a Data File or Creating a Tablespace ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540) |  No action is required if the ` STANDBY_FILE_MANAGEMENT ` database initialization parameter is set to ` AUTO ` . If this parameter is set to ` MANUAL ` , the new data file must be copied to the physical standby database.   
[ Dropping Tablespaces and Deleting Data Files ](managing-oracle-data-guard-physical-standby-databases.md#GUID-76C5E1AD-2A05-4BC8-AB26-5C68E2AB991D) |  Delete data file from primary and physical standby database after the redo data containing the ` DROP ` or ` DELETE ` command is applied to the physical standby.   
[ Using Transportable Tablespaces with a Physical Standby Database ](managing-oracle-data-guard-physical-standby-databases.md#GUID-88158C9B-51FB-4F13-93FC-97D737C86BF0) |  Move tablespace between the primary and the physical standby database.   
[ Renaming a Data File in the Primary Database ](managing-oracle-data-guard-physical-standby-databases.md#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0) |  Rename the data file on the physical standby database.   
[ Add or Drop a Redo Log File Group ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE) |  Evaluate the configuration of the redo log and standby redo log on the physical standby database and adjust as necessary.   
[ NOLOGGING or Unrecoverable Operations ](managing-oracle-data-guard-physical-standby-databases.md#GUID-3A0E6CB4-EE5B-4D16-BCFE-40236BC312D7) |  Use the RMAN command ` RECOVER ... NONLOGGED BLOCK ` to replace the invalid blocks on the standby with the changed blocks from the primary.   
[ Refresh the Password File ](managing-oracle-data-guard-physical-standby-databases.md#GUID-FB7AFCB2-8CB3-4F43-B664-261BAFD5B07B) |  As of Oracle Database 12 *c* Release 2 (12.2.0.1), password file changes done on the primary database are automatically propagated to standby databases. The only exception to this is far sync instances. Updated password files must still be manually copied to far sync instances because far sync instances receive redo, but do not apply it. After the password file is up-to-date at the far sync instance, the redo containing the password update at the primary is automatically propagated to any standby databases that are set up to receive redo from that far sync instance. The password file is updated on the standby when the redo is applied.   
[ Reset the TDE Master Encryption Key ](managing-oracle-data-guard-physical-standby-databases.md#GUID-B9B956BC-885F-49AF-A4FF-E900998109ED) |  Replace the database encryption wallet on the physical standby database with a fresh copy of the database encryption wallet from the primary database.   
[ Initialization Parameters ](oracle-initialization-parameters-used-by-oracle-data-guard.md#GUID-A1CF9B00-F0F5-4F3B-A0E0-9D6ABF775EC9) |  Evaluate whether a corresponding change must be made to the initialization parameters on the physical standby database.   
  
####  Adding a Data File or Creating a Tablespace {#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540} 

The ` STANDBY_FILE_MANAGEMENT ` database initialization parameter controls whether the addition of a data file to the primary database is automatically propagated to physical standby databases. 

  * If the ` STANDBY_FILE_MANAGEMENT ` database parameter on the physical standby database is set to ` AUTO ` , any new data files created on the primary database are automatically created on the physical standby database. 
  * If the ` STANDBY_FILE_MANAGEMENT ` database parameter on the physical standby database is set to ` MANUAL ` , a new data file must be manually copied from the primary database to the physical standby databases after it is added to the primary database. If the file is copied and named correctly (taking into account ` DB_FILE_NAME_CONVERT ` ) before the standby applies the redo, then no further action is required. Otherwise recovery will create a control file entry with a name including "UNNAMED" and stop. This entry must be renamed to the correct name before recovery will start again. 

> **note:** 

On a physical standby for which the Oracle Active Data Guard option has been enabled, you cannot use the manual copy method. Instead, once the UNNAMED entry has been added by recovery, you must create an empty data file by running the following SQL statement on the standby, and then restart recovery: 
    
        ```
    SQL> ALTER DATABASE CREATE DATAFILE [filename | filenumber] -
        AS [NEW | new_filename];
    ```

You must specify which one to rename by either its current ` filename ` or the ` filenumber ` . Also specify either the new filename or ` NEW ` . The ` NEW ` keyword lets Oracle automatically choose a name, if Oracle Managed Files (OMF) is enabled. 




####  Dropping Tablespaces and Deleting Data Files {#GUID-76C5E1AD-2A05-4BC8-AB26-5C68E2AB991D} 

When a tablespace is dropped or a data file is deleted from a primary database, the corresponding data file(s) must be deleted from the physical standby database. 

The following example shows how to drop a tablespace: 
    
    
    ```
    SQL> DROP TABLESPACE tbs_4;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    
    ```

To verify that deleted data files are no longer part of the database, query the ` V$DATAFILE ` view. 

Delete the corresponding data file on the standby system after the redo data that contains the previous changes is applied to the standby database. For example: 
    
    
    ```
    % rm /disk1/oracle/oradata/payroll/s2tbs_4.dbf
    
    ```

On the primary database, after ensuring the standby database applied the redo information for the dropped tablespace, you can remove the data file for the tablespace. For example: 
    
    
    ```
    % rm /disk1/oracle/oradata/payroll/tbs_4.dbf
    
    ```

#####  Using DROP TABLESPACE INCLUDING CONTENTS AND DATAFILES {#GUID-10EE6961-A52F-4101-A911-B8692636CC37} 

You can issue the SQL ` DROP TABLESPACE INCLUDING CONTENTS AND DATAFILES ` statement on the primary database to delete the data files on both the primary and standby databases. 

To use this statement, the ` STANDBY_FILE_MANAGEMENT ` initialization parameter must be set to ` AUTO ` . For example, to drop the tablespace at the primary site: 
    
    
    ```
    SQL> DROP TABLESPACE tbs_4 INCLUDING CONTENTS AND DATAFILES;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```

####  Using Transportable Tablespaces with a Physical Standby Database {#GUID-88158C9B-51FB-4F13-93FC-97D737C86BF0} 

You can use the Oracle transportable tablespaces feature to move a subset of an Oracle database and plug it in to another Oracle database, essentially moving tablespaces between the databases. 

To move or copy a set of tablespaces into a primary database when a physical standby is being used, perform the following steps: 

  1. Generate a transportable tablespace set that consists of data files for the set of tablespaces being transported and an export file containing structural information for the set of tablespaces. 

  2. Transport the tablespace set: 

    1. Copy the data files and the export file to the primary database. 

    2. Copy the data files to the standby database. 

The data files must have the same path name on the primary and standby databases unless the ` DB_FILE_NAME_CONVERT ` database initialization parameter has been configured. If ` DB_FILE_NAME_CONVERT ` has *not* been configured and the path names of the data files are not the same on the primary and standby databases, issue the ` ALTER DATABASE RENAME FILE ` statement to rename the data files. Do this after Redo Apply has failed to apply the redo generated by plugging the tablespace into the primary database. The ` STANDBY_FILE_MANAGEMENT ` initialization parameter must be set to ` MANUAL ` before renaming the data files, and then reset to the previous value after renaming the data files. 

  3. Plug in the tablespace. 

Invoke the  Data Pump utility to plug the set of tablespaces into the primary database. Redo data is generated and applied at the standby site to plug the tablespace into the standby database. 




For more information about transportable tablespaces, see [ *Oracle Database Administrator's Guide.* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN11394)

####  Renaming a Data File in the Primary Database {#GUID-2D3E5488-237C-467A-AB8B-7C09B6E43CE0} 

When you rename one or more data files in the primary database, the change is not propagated to the standby database. It must be done manually. 

To rename the same data files on the standby database, you must manually make the equivalent modifications on the standby database because the modifications are not performed automatically, even if the ` STANDBY_FILE_MANAGEMENT ` initialization parameter is set to ` AUTO ` . 

The following steps describe how to rename a data file in the primary database and manually propagate the changes to the standby database. 

  1. To  rename the data file in the primary database, take the tablespace offline: 
    
        ```
    SQL> ALTER TABLESPACE tbs_4 OFFLINE;
    
    ```

  2. Exit from the SQL prompt and issue an operating system command, such as the following UNIX ` mv ` command, to rename the data file on the primary system: 
    
        ```
    % mv /disk1/oracle/oradata/payroll/tbs_4.dbf 
    /disk1/oracle/oradata/payroll/tbs_x.dbf
    
    ```

  3. Rename the data file in the primary database and bring the tablespace back online: 
    
        ```
    SQL> ALTER TABLESPACE tbs_4 RENAME DATAFILE -
    > '/disk1/oracle/oradata/payroll/tbs_4.dbf' -
    >  TO '/disk1/oracle/oradata/payroll/tbs_x.dbf';
    
    SQL> ALTER TABLESPACE tbs_4 ONLINE;
    
    ```

> **note:** 

An alternative to these first three steps is to use the ` ALTER ` ` DATABASE ` ` MOVE ` ` DATAFILE ` command to rename a datafile. This command lets you rename a datafile while allowing read/write access to the datafile. Adequate storage area is a prerequisite for moving a datafile because during the execution of the ` MOVE ` ` DATAFILE ` command, the database maintains both the original and the renamed datafiles as two separate files. See [ Moving the Location of Online Data Files ](getting-started-with-oracle-data-guard.md#GUID-8DA7295A-53C7-4B52-9E0A-8A513640D265) for more information. 

  4. Connect to the standby database and stop Redo Apply: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
    
    ```

  5. Shut down the standby database: 
    
        ```
    SQL> SHUTDOWN;
    
    ```

  6. Rename the data file at the standby site using an operating system command, such as the UNIX ` mv ` command: 
    
        ```
    % mv /disk1/oracle/oradata/payroll/tbs_4.dbf /disk1/oracle/oradata/payroll/tbs_x.dbf
    
    ```

  7. Start and mount the standby database: 
    
        ```
    SQL> STARTUP MOUNT;
    
    ```

  8. Rename the data file in the standby control file. To rename a data file, you must set the ` STANDBY_FILE_MANAGEMENT ` database initialization parameter to ` MANUAL ` . You can then reset the parameter to its previous value after renaming the data file. 
    
        ```
    SQL> ALTER DATABASE RENAME FILE '/disk1/oracle/oradata/payroll/tbs_4.dbf' -
    > TO '/disk1/oracle/oradata/payroll/tbs_x.dbf';
    
    ```

  9. On the standby database, restart Redo Apply: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE -
    > DISCONNECT FROM SESSION;
    
    ```




If you do not rename the corresponding data file at the standby system, and then try to refresh the standby database control file, the standby database attempts to use the renamed data file, but will not find it. Error messages similar to the following are written to the alert log: 
    
    
    ```
    ORA-00283: recovery session canceled due to errors
    ORA-01157: cannot identify/lock datafile 4 - see DBWR trace file
    ORA-01110: datafile 4: '/Disk1/oracle/oradata/payroll/tbs_x.dbf'
    ```

> **note:** 

An alternative to steps 4-9 is to use the ` ALTER ` ` DATABASE ` ` MOVE ` ` DATAFILE ` command to rename a datafile at the standby. See [ Moving the Location of Online Data Files ](getting-started-with-oracle-data-guard.md#GUID-8DA7295A-53C7-4B52-9E0A-8A513640D265) for more information. 

####  Add or Drop a Redo Log File Group {#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE} 

The configuration of the redo log and standby redo log on a physical standby database should be reevaluated and adjusted as necessary after adding or dropping a log file group on the primary database. 

Take the following steps to add or drop a log file group or standby log file group on a physical standby database: 

  1. Stop Redo Apply. 
  2. If the ` STANDBY_FILE_MANAGEMENT ` initialization parameter is set to ` AUTO ` , change the value to ` MANUAL ` . 
  3. Add or drop a log file group. 

> **note:** 

An online logfile group must always be manually cleared before it can be dropped from a physical standby database. For example: 
    
        ```
    ALTER DATABASE CLEAR LOGFILE GROUP 3;
    
    ```

An online logfile group that has a status of ` CURRENT ` or ` CLEARING_CURRENT ` cannot be dropped from a physical standby database. An online logfile group that has this status can be dropped after a role transition. 

  4. Restore the ` STANDBY_FILE_MANAGEMENT ` initialization parameter and the Redo Apply options to their original states. 
  5. Restart Redo Apply. 



In Oracle RAC environments, keep the following in mind: 

  * When an online redo log group is added to a primary database, you must manually add an online redo log group to the standby database. It is not done automatically. 

  * When a new redo thread is added to a primary database, a new redo thread is automatically added to the standby. By default, the new thread is configured with 2 log groups of 100 MB each. This cannot be changed or overridden. 

  * When a new log group is added to an existing redo thread, a new log group is not automatically added to its existing thread. 




####  NOLOGGING or Unrecoverable Operations {#GUID-3A0E6CB4-EE5B-4D16-BCFE-40236BC312D7} 

When you perform a DML or DDL operation using the ` NOLOGGING ` or ` UNRECOVERABLE ` clause, blocks on the standby may be marked as invalid (also known as nonlogged blocks). 

See [ Precedence of FORCE LOGGING Settings ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN-GUID-27E9B63B-03E4-43E5-BA59-AB0EDDDE5CE6) for details about when an operation is actually performed in a nonlogged fashion. 

See [ Recovering After the NOLOGGING Clause Is Specified ](examples-of-using-oracle-data-guard.md#GUID-BD9E85AB-D812-4659-9C85-26CDFF115A8A) for information about recovering after the ` NOLOGGING ` clause is used. 

See the [ *Oracle Database Administrator's Guide.* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN11096) for information about specifying ` FORCE LOGGING ` mode. 

####  Refresh the Password File {#GUID-FB7AFCB2-8CB3-4F43-B664-261BAFD5B07B} 

If the ` REMOTE_LOGIN_PASSWORDFILE ` database initialization parameter is set to ` SHARED ` or ` EXCLUSIVE ` , then the password file on a physical standby database is automatically replaced with a fresh copy from the primary database. 

The file is replaced after administrative privileges are granted or revoked, or the password of a user with administrative privileges is changed. The only exception to this is far sync instances. Updated password files must still be manually copied to far sync instances because far sync instances receive redo, but do not apply it. When a password file is manually updated at a far sync instance, the redo containing the same password changes from the primary database is automatically propagated to any standby databases that are set up to receive redo from that far sync instance. The password file is updated on the standby when the redo is applied. 

####  Reset the TDE Master Encryption Key {#GUID-B9B956BC-885F-49AF-A4FF-E900998109ED} 

The database encryption wallet on a physical standby database must be replaced with a fresh copy of the database encryption wallet from the primary database whenever the TDE master encryption key is reset on the primary database. 

Failure to refresh the database encryption wallet on the physical standby database prevents access to encrypted columns on the physical standby database that are modified after the master encryption key is reset on the primary database. 

For online tablespaces and databases, as of Oracle Database 12 *c* Release 2 (12.2.0.1), you can encrypt, decrypt, and re-key both new and existing tablespaces, and existing databases within an Oracle Data Guard environment. 

For offline tablespaces and databases, as of Oracle Database 12 *c* Release 2 (12.2.0.1), you can encrypt and decrypt both new and existing tablespaces, and existing databases within an Oracle Data Guard environment. 

In online conversion, the encryption, decryption, or re-keying on the standby is automatic after it is performed on the primary. An online encryption, decryption, or re-keying cannot be performed directly on a standby database. 

In an offline conversion, the encryption or decryption must be performed manually on both the primary and standby. An offline conversion affects the data files on the particular primary or standby database only. Both the primary and physical standby should be kept at the same state. You can minimize downtime by encrypting (or decrypting) the tablespaces on the standby first, switching over to the primary, and then encrypting (or decrypting) the tablespaces on the primary. 

> **note:** See Also: 

  * [ *Oracle Database Advanced Security Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ASOAG-GUID-79072DFC-4DC6-4920-9B2A-D649123D15C7)




###  Recovering Through the OPEN RESETLOGS Statement {#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A} 

Oracle Data Guard allows recovery on a physical standby database to continue after the primary database has been opened with the ` RESETLOGS ` option. 

When an ` ALTER DATABASE OPEN RESETLOGS ` statement is issued on the primary database, the incarnation of the database changes, creating a new branch of redo data. 

When a physical standby database receives a new branch of redo data, Redo Apply automatically takes the new branch of redo data. For physical standby databases, no manual intervention is required if the standby database did not apply redo data past the new resetlogs SCN (past the start of the new branch of redo data). The following table describes how to resynchronize the standby database with the primary database branch. 

If the standby database. . .  |  Then. . .  |  Perform these steps. . .   
---|---|---  
Has not applied redo data past the new resetlogs SCN (past the start of the new branch of redo data) and the new redo branch from ` OPEN RESETLOGS ` has been registered at the standby  |  Redo Apply automatically takes the new branch of redo.  |  No manual intervention is necessary. The managed redo process (MRP) automatically resynchronizes the standby database with the new branch of redo data.  Note: To check whether the new redo branch has been registered at the standby, perform the following query at the primary and standby and verify that the results match: 
    
    
    ```
    SELECT resetlogs_id, resetlogs_change# FROM V$DATABASE_INCARNATION WHERE status='CURRENT'
    ```  
  
Has applied redo data past the new resetlogs SCN (past the start of the new branch of redo data) and Flashback Database is enabled on the standby database  |  The standby database is recovered *in the future* of the new branch of redo data.  | 

<li>
  1. Follow the procedure in [ Flashing Back a Physical Standby Database to a Specific Point-in-Time ](examples-of-using-oracle-data-guard.md#GUID-09129494-CD98-44B6-821F-A4C2CB5AEE08) to flash back a physical standby database.  </li> <li>
  2. Restart Redo Apply to continue application of redo data onto new reset logs branch.  </li> 
The managed redo process (MRP) automatically resynchronizes the standby database with the new branch.   
Has applied redo data past the new resetlogs SCN (past the start of the new branch of redo data) and Flashback Database is not enabled on the standby database  |  The primary database has diverged from the standby on the indicated primary database branch.  |  Re-create the physical standby database following the procedures in [ Creating a Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B511FB6E-E3E7-436D-94B5-071C37550170) .   
Is missing intervening archived redo log files from the new branch of redo data  |  The MRP cannot continue until the missing log files are retrieved.  |  Locate and register missing archived redo log files from each branch.   
Is missing archived redo log files from the end of the previous branch of redo data.  |  The MRP cannot continue until the missing log files are retrieved.  |  Locate and register missing archived redo log files from the previous branch.   
  
See [ *Oracle Database Backup and Recovery User's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89752) for more information about database incarnations, recovering through an ` OPEN RESETLOGS ` operation, and Flashback Database. 

###  Automatic Flashback of a Mounted Standby After a Primary RESETLOGS Operation {#GUID-252097AC-3070-43B6-88D8-919AE27F97AD} 

A standby database that is in a mounted state can automatically follow the primary database after a ` RESETLOGS ` operation on the primary. This simplifies standby management after a ` RESETLOGS ` operation on the primary. 

When flashback or point-in-time recovery is performed either on a primary database or a PDB in the primary database, the primary database or PDB is moved to a previous point in time and the primary is then opened with the ` RESETLOGS ` option. A new incarnation of the primary or the PDB in the primary is created. For the standby to automatically follow the primary, the MRP performs the following actions: 

  * detects the new incarnation 

  * flashes back the standby or the PDB on the standby to the same point in time as that of the primary or the PDB on the primary 

  * restarts the standby recovery and moves the standby to the new branch of redo 




The flashback operation will succeed only when the standby database has sufficient flashback data. 

If you do not want the standby to automatically follow the primary, either keep the standby database in ` OPEN ` mode or stop the MRP process on the standby. 

If the standby database is open in read only mode, the corresponding error messages are recorded in the alert log. When you restart the MRP after closing the physical standby, the recovery process automatically flashes back the standby database and continues to apply the new branch of redo. 

**Related Topics**

  * [ Actions Needed On a Standby After a PDB PITR or PDB Flashback On a Primary ](examples-of-using-oracle-data-guard.md#GUID-8D948A24-A3B7-4E4F-917A-00B047CF3CAF)
  * [ Flashing Back a Physical Standby Database to a Specific Point-in-Time ](examples-of-using-oracle-data-guard.md#GUID-09129494-CD98-44B6-821F-A4C2CB5AEE08)



###  Monitoring Primary, Physical Standby, and Snapshot Standby Databases {#GUID-0682EE24-B495-48E8-9E1F-52AC0E6FE486} 

This table summarizes common primary database management actions and where to find information related to these actions. 

> **note:** Starting with Oracle Database Release 21c, the ` V$MANAGED_STANDBY ` view is deprecated and may be desupported in future releases. Use the ` V$DATAGUARD_PROCESS ` view instead. 

When you use Oracle Data Guard broker to provide data protection for one or more pluggable database (PDBs) in a source database, redo data is shipped to the target database. You must connect to the target PDB within the target database and then query the views listed in the table. 

**Table: Sources of Information About Common Primary Database Management Actions ** 

Primary Database Action  |  Primary Site Information  |  Standby Site Information   
---|---|---  
Enable or disable a redo thread  |  <ul>

<li>
  * Alert log  </li> <li>
  * ` V$THREAD ` </li> </ul> |  Alert log   
Display database role, protection mode, protection level, switchover status, fast-start failover information, and so forth  |  ` V$DATABASE ` |  ` V$DATABASE `  
Add or drop a redo log file group  |  <ul> <li>
    * Alert log  </li> <li>
    * ` V$LOG ` </li> <li>
    * ` STATUS ` column of ` V$LOGFILE ` </li> </ul> |  Alert log   
` CREATE CONTROLFILE ` |  Alert log  |  Alert log   
Monitor Redo Apply  |  <ul> <li>
      * Alert log  </li> <li>
      * ` V$ARCHIVE_DEST_STATUS ` </li> </ul> |  <ul> <li>
        * Alert log  </li> <li>
        * ` V$ARCHIVED_LOG ` </li> <li>
        * ` V$LOG_HISTORY ` </li> <li>
        * ` V$MANAGED_STANDBY ` </li> </ul>  
Change tablespace status  |  <ul> <li>
          * ` V$RECOVER_FILE ` </li> <li>
          * ` DBA_TABLESPACES ` </li> <li>
          * Alert log  </li> </ul> |  <ul> <li>
            * ` V$RECOVER_FILE ` </li> <li>
            * ` DBA_TABLESPACES ` </li> </ul>  
Add or drop a data file or tablespace  |  <ul> <li>
              * ` DBA_DATA_FILES ` </li> <li>
              * Alert log  </li> </ul> |  <ul> <li>
                * ` V$DATAFILE ` </li> <li>
                * Alert log  </li> </ul>  
Rename a data file  |  <ul> <li>
                  * ` V$DATAFILE ` </li> <li>
                  * Alert log  </li> </ul> |  <ul> <li>
                    * ` V$DATAFILE ` </li> <li>
                    * Alert log  </li> </ul>  
Unlogged or unrecoverable operations  |  <ul> <li>
                      * ` V$DATAFILE ` </li> <li>
                      * ` V$DATABASE ` </li> </ul> |  Alert log   
Monitor redo transport  |  <ul> <li>
                        * ` V$ARCHIVE_DEST_STATUS ` </li> <li>
                        * ` V$ARCHIVED_LOG ` </li> <li>
                        * ` V$ARCHIVE_DEST ` </li> <li>
                        * Alert log  </li> </ul> |  <ul> <li>
                          * ` V$ARCHIVED_LOG ` </li> <li>
                          * Alert log  </li> </ul>  
Issue ` OPEN RESETLOGS ` or ` CLEAR UNARCHIVED LOGFILES ` statements  |  Alert log  |  Alert log   
Change initialization parameter  |  Alert log  |  Alert log   
  
####  Using Views to Monitor Primary, Physical, and Snapshot Standby Databases {#GUID-567E9883-B1BE-431D-86E9-872BAB6A899F} 

You can use dynamic performance views to monitor primary, physical standby, and snapshot standby databases. 

The following dynamic performance views are discussed: 

                            * [ V$DATABASE ](managing-oracle-data-guard-physical-standby-databases.md#GUID-BCAA4A5D-8A26-434D-B2E9-291BF7FC147D)

                            * [ V$MANAGED_STANDBY ](managing-oracle-data-guard-physical-standby-databases.md#GUID-31B49D22-414F-4802-9A48-8D8C55CD8CB6)

                            * [ V$ARCHIVED_LOG ](managing-oracle-data-guard-physical-standby-databases.md#GUID-E97EB4D8-48E0-4355-A878-9CA3E80C0AE8)

                            * [ V$LOG_HISTORY ](managing-oracle-data-guard-physical-standby-databases.md#GUID-6FF2CE1A-C0D8-4821-BF75-B219F9AA283A)

                            * [ V$DATAGUARD_STATUS ](managing-oracle-data-guard-physical-standby-databases.md#GUID-F60AAF04-10FE-4568-BA5E-792D150FEA28)

                            * [ V$ARCHIVE_DEST ](managing-oracle-data-guard-physical-standby-databases.md#GUID-0DA8663E-65C4-47EB-8D47-4712F8201646)

> **note:** See Also: 

[ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN003) for complete reference information about views 

#####  V$DATABASE {#GUID-BCAA4A5D-8A26-434D-B2E9-291BF7FC147D} 

You can use the ` V$DATABASE ` view to display information about data protection, switchover status, and fast-start failover status. 

The following query displays the data protection mode, data protection level, database role, and switchover status for a primary, physical standby or snapshot standby database: 
                                                    
                                                                                                        ```
                                                    SQL> SELECT PROTECTION_MODE, PROTECTION_LEVEL, –
                                                    > DATABASE_ROLE ROLE, SWITCHOVER_STATUS –
                                                    > FROM V$DATABASE;
                                                    
                                                    ```

The following query displays fast-start failover status: 
                                                    
                                                                                                        ```
                                                    SQL> SELECT FS_FAILOVER_STATUS "FSFO STATUS", -
                                                    > FS_FAILOVER_CURRENT_TARGET TARGET, -
                                                    > FS_FAILOVER_THRESHOLD THRESHOLD, -
                                                    > FS_FAILOVER_OBSERVER_PRESENT "OBSERVER PRESENT" –
                                                    > FROM V$DATABASE;
                                                    ```

#####  V$DATAGUARD_PROCESS {#GUID-3B82374E-5646-4029-8AD3-94BAE5FCB17D} 

The ` V$DATAGUARD_PROCESS ` view displays one row for each Oracle Data Guard process that is currently running. 

The ` V$DATAGUARD_PROCESS ` view replaces the ` V$MANAGED_STANDBY ` view which is deprecated as of Oracle Database 12 *c* Release 2 (12.2.0.1) and may be desupported in a future release. 

The following is an example query of this view: 
                                                    
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

#####  V$MANAGED_STANDBY {#GUID-31B49D22-414F-4802-9A48-8D8C55CD8CB6} 

You can use the ` V$MANAGED_STANDBY ` view to query Redo Apply and redo transport status on a physical standby database. 

> **note:** This view is deprecated as of Oracle Database 12 *c* Release 2 (12.2.0.1) and may be desupported in a future release. The ` V$DATAGUARD_PROCESS ` view should be used instead. 

The following is a sample query of the ` V$MANAGED_STANDBY ` view: 
                                                    
                                                                                                        ```
                                                    SQL> SELECT PROCESS, STATUS, THREAD#, SEQUENCE#,-
                                                    > BLOCK#, BLOCKS FROM V$MANAGED_STANDBY;
                                                     
                                                    PROCESS STATUS       THREAD#    SEQUENCE#  BLOCK#     BLOCKS
                                                    ------- ------------ ---------- ---------- ---------- ----------
                                                    RFS     ATTACHED     1          947        72         72
                                                    MRP0    APPLYING_LOG 1          946        10         72
                                                    
                                                    ```

The sample output shows that a remote file server (RFS) process completed archiving a redo log file with a sequence number of 947 and that Redo Apply is actively applying an archived redo log file with a sequence number of 946. Redo Apply is currently recovering block number 10 of the 72-block archived redo log file. 

#####  V$ARCHIVED_LOG {#GUID-E97EB4D8-48E0-4355-A878-9CA3E80C0AE8} 

You can use the ` V$ARCHIVED_LOG ` view to query information about archived redo log files that have been received by a physical or snapshot standby database from a primary database. 

For example, issue the following query: 
                                                    
                                                                                                        ```
                                                    SQL> SELECT THREAD#, SEQUENCE#, FIRST_CHANGE#, -
                                                    > NEXT_CHANGE# FROM V$ARCHIVED_LOG;
                                                     
                                                    THREAD#    SEQUENCE#  FIRST_CHANGE# NEXT_CHANGE#
                                                    ---------- ---------- ------------- ------------
                                                    1          945        74651         74739
                                                    1          946        74739         74772
                                                    1          947        74772         74795
                                                    
                                                    ```

The sample output shows that three archived redo log files have been received from the primary database. 

#####  V$LOG_HISTORY {#GUID-6FF2CE1A-C0D8-4821-BF75-B219F9AA283A} 

You can use the ` V$LOG_HISTORY ` view to query archived log history information. 

For example, issue the following query: 
                                                    
                                                                                                        ```
                                                    SQL> SELECT THREAD#, SEQUENCE#, FIRST_CHANGE#, -
                                                    > NEXT_CHANGE# FROM V$LOG_HISTORY;
                                                    
                                                    ```

#####  V$DATAGUARD_STATUS {#GUID-F60AAF04-10FE-4568-BA5E-792D150FEA28} 

You can use the ` V$DATAGUARD_STATUS ` view to display messages generated by Oracle Data Guard events that caused a message to be written to the alert log or to a server process trace file. 

For example, issue the following query : 
                                                    
                                                                                                        ```
                                                    SQL> SELECT MESSAGE FROM V$DATAGUARD_STATUS;
                                                    
                                                    ```

#####  V$ARCHIVE_DEST {#GUID-0DA8663E-65C4-47EB-8D47-4712F8201646} 

You can query the ` V$ARCHIVE_DEST ` view to show the status of each redo transport destination, and for redo transport destinations that are standby databases, the SCN of the last primary database redo applied at that standby database. 

For example, issue the following query: 
                                                    
                                                                                                        ```
                                                    SQL> SELECT DEST_ID, STATUS, APPLIED_SCN FROM V$ARCHIVE_DEST WHERE TARGET='STANDBY';
                                                       
                                                    DEST_ID    STATUS    APPLIED_SCN
                                                    ---------- --------- -----------
                                                    2          VALID     439054
                                                    3          VALID     439054 
                                                    ```

###  Replicating Restore Points from Primary to Standby {#GUID-45D7D60E-ED0F-4B3E-9D32-EB5394196CAF} 

Restore points that are created on a primary database are automatically replicated to the standby database. The restore points created on the standby database are called *replicated restore points* . Irrespective of whether a restore point on the primary database is a guaranteed restore point or a normal restore point, the corresponding replicated restore point is always a normal restore point. 

Oracle Database automatically replicates restore points from a primary database to the standby database when the following conditions are met: 

                            * ` COMPATIBLE ` initialization parameter for both the primary database and the standby database is set to 19.0.0 or higher 

                            * primary database is open 

A restore point that is created on a primary database when the primary is in mount mode is not replicated. This restriction is because the restore point information is replicated though the redo. 

The naming convention for a replicated restore point uses the name of the restore point on the primary database suffixed with ` _PRIMARY ` . If a replicated restore point with the same name exists on the standby database, then a replicated restore point is not created. For example, when you create a restore point named ` PRE_MYTBS ` on the primary database, the replicated restore point is named ` PRE_MYTBS_PRIMARY ` . When you delete a restore point on the primary, the corresponding replicated restore point on the standby is also deleted. 

The managed redo process (MRP) manages the creation and maintenance of replicated restore points. If restore points are created on the primary database when MRP is not running, then these restore points are replicated to the standby database after MRP is started. 

To determine if a restore point is automatically replicated, query the ` V$RESTORE_POINT ` view. 

###  Tuning Redo Apply {#GUID-EEDAC95A-828F-4117-AF57-D8ABA702D42D} 

Oracle technical briefs are available that describe how to optimize Redo Apply and media recovery performance. 

In particular, see Oracle Maximum Availability Architecture (MAA) home page at: 

[ ` http://www.oracle.com/goto/maa ` ](http://www.oracle.com/goto/maa)

> **note:** See Also: 

My Oracle Support note 454848.1 at [ ` http://support.oracle.com ` ](http://support.oracle.com) for information about the installation and use of the Standby Statspack, which can be used to collect Redo Apply performance data from a physical standby database 

###  Tuning Databases in an Active Data Guard Environment with SQL Tuning Advisor {#GUID-2FF52A94-828C-4BF1-A5D0-C70A05A8198A} 

In an Active Data Guard environment, SQL Tuning Advisor can tune a standby workload on a primary database. 

Using database links, you can issue SQL Tuning Advisor statements on one database, but execute the statements on a different database. 

Tuning a Standby Database Workload on a Primary Database 

In some cases, a standby database can assume a reporting role in addition to its data protection role. The standby database can have its own workload of queries, some of which may require tuning. In this scenario, you tune a standby database workload by issuing every tuning statement on the standby database, but SQL Tuning Advisor performs its analysis on the primary database by using a standby-to-primary database link. 

The following are the tasks that must be performed to tune a standby database workload on a primary database. The tasks must be performed at the standby database in the order given, using the ` DBMS_SQLTUNE ` PL/SQL package: 

                            1. Execute the ` DBMS_SQLTUNE.CREATE_TUNING_TASK ` statement to fetch the data from the primary database needed to create a task. Because the standby is a read-only database, the data about the task is written remotely to the primary database. A database link parameter is required in this step to write to the primary. (A database link parameter is optional in subsequent steps, because it is tied to the task created in this step.) 

                            2. Execute the ` DBMS_SQLTUNE.EXECUTE_TUNING_TASK ` statement. Initially, the data required to execute a task is fetched from the remote primary database. The tuning analysis process to find the possible recommendations is executed. Because the standby is a read-only database, when the results are available they are stored remotely at the primary. 

                            3. Execute the ` DBMS_SQLTUNE.REPORT_TUNING_TASK ` statement. The data needed to construct a report is stored remotely at the primary database. The data is fetched remotely from the primary and constructed locally at the standby. 

                            4. Execute the ` DBMS_SQLTUNE.ACCEPT_SQL_PROFILE ` statement. The profile data is written to the remote primary database because the standby is read-only. 

                            5. The SQL Profiles are made available at the standby using Redo Apply. 

> **note:** See Also: 
                            * [ *Oracle Database PL/SQL Packages and Types Reference*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS-GUID-421606DC-588B-4BCD-8ED8-192F8B93C070) for more information about the ` DBMS_SQLTUNE ` package 

                            * [ *Oracle Database SQL Tuning Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=TGSQL-GUID-642877C5-40BE-4736-8302-17F849933038) for more information about local and remote SQL tuning 

###  Using Oracle Diagnostic Pack to Tune Oracle Active Data Guard Standbys {#GUID-1E2A26E3-B0E2-4ED1-A50D-891505C86E7D} 

The Oracle Diagnostic Pack can be used with an Oracle Active Data Guard standby database that is open in read-only mode. 

This enables you to capture performance data to the Automatic Workload Repository (AWR) for an Oracle Active Data Guard standby and to run Automatic Database Diagnostic Monitor (ADDM) analysis on the AWR data. For details about how to perform these operations, see [ *Oracle Database Performance Tuning Guide*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=TGDBA-GUID-309C107F-DC42-4119-9904-9504E9748B84) . 

###  Managing a Snapshot Standby Database {#GUID-CE30E2D6-5B53-4389-8B02-FC0F341C8C1A} 

A snapshot standby database is a fully updatable standby database. It receives and archives redo data from a primary database, but does not apply it. 

Redo data received from the primary database is applied when a snapshot standby database is converted back into a physical standby database, after discarding all local updates to the snapshot standby database. 

A snapshot standby database typically diverges from its primary database over time because redo data from the primary database is not applied as it is received. Local updates to the snapshot standby database cause additional divergence. The data in the primary database is fully protected however, because a snapshot standby can be converted back into a physical standby database at any time, and the redo data received from the primary is then applied. 

A snapshot standby database provides disaster recovery and data protection benefits that are similar to those of a physical standby database. Snapshot standby databases are best used in scenarios where the benefit of having a temporary, updatable snapshot of the primary database justifies increased time to recover from primary database failures. 

####  Converting a Physical Standby Database into a Snapshot Standby Database {#GUID-63245504-B67C-4DF2-B8E5-752C0A67FEE7} 

These steps describe how to convert a physical standby database into a snapshot standby database. 

                            1. Stop Redo Apply, if it is active. 
                            2. Ensure that the database is mounted, but not open. 
                            3. Ensure that a fast recovery area has been configured. It is not necessary for flashback database to be enabled. 
                            4. Issue the following SQL statement to perform the conversion: 
                                                        
                                                                                                                ```
                                                        SQL> ALTER DATABASE CONVERT TO SNAPSHOT STANDBY;
                                                        
                                                        ```

                            5. Open the snapshot standby in read/write mode by issuing the following SQL statement: 
                                                        
                                                                                                                ```
                                                        SQL> ALTER DATABASE OPEN READ WRITE;
                                                        ```

> **note:** 

A physical standby database that is managed by the Oracle Data Guard broker can be converted into a snapshot standby database using either DGMGRL or Oracle Enterprise Manager Cloud Control. See [ *Oracle Data Guard Broker* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR3432) for more details. 

####  Using a Snapshot Standby Database {#GUID-C1C48B71-88F5-4006-9CAE-0FD040888FAE} 

A snapshot standby database can be opened in read-write mode and is fully updatable. 

A snapshot standby database has the following characteristics: 

                            * A snapshot standby database cannot be the target of a switchover or failover. A snapshot standby database must first be converted back into a physical standby database before performing a role transition to it. 

                            * A snapshot standby database cannot be the only standby database in a Maximum Protection Oracle Data Guard configuration. 

> **note:** 

Flashback Database is used to convert a snapshot standby database back into a physical standby database. Any operation that cannot be reversed using Flashback Database technology prevents a snapshot standby from being converted back to a physical standby. 

For information about some of the limitations of Flashback Database, see [ *Oracle Database Backup and Recovery User's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV543) . 

####  Converting a Snapshot Standby Database into a Physical Standby Database {#GUID-36B87563-A971-43B4-8320-52BBA27EE749} 

These steps describe how to convert a snapshot standby database into a physical standby database. 

                            1. On an Oracle Real Applications Cluster (Oracle RAC) database, shut down all but one instance. 
                            2. Ensure that the database is mounted, but not open. 
                            3. Issue the following SQL statement to perform the conversion: 
                                                        
                                                                                                                ```
                                                        SQL> ALTER DATABASE CONVERT TO PHYSICAL STANDBY;
                                                        
                                                        ```

Redo data received while the database was a snapshot standby database is automatically applied when Redo Apply is started. 

> **note:** 

A snapshot standby database must be opened at least once in read-write mode before it can be converted into a physical standby database. 
