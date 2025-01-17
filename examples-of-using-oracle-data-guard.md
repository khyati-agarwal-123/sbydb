## 15Oracle Data Guard Scenarios {#GUID-07C5867F-C55F-4B5B-804F-1800CE9985EF}

These scenarios present different situations you might encounter while administering your Oracle Data Guard configuration. Each of them can be adapted to your specific environment.

> **note:** 

A multitenant container database is the only supported architecture in Oracle Database 21c and later releases. While the documentation is being revised, legacy terminology may persist. In most cases, "database" and "non-CDB" refer to a CDB or PDB, depending on context. In some contexts, such as upgrades, "non-CDB" refers to a non-CDB from a previous release.

* [Configuring Logical Standby Databases After a Failover](examples-of-using-oracle-data-guard.md#GUID-CBC9D920-C2D0-4342-ACEF-C16289D1774A)

* [Converting a Failed Primary Into a Standby Database Using Flashback Database](examples-of-using-oracle-data-guard.md#GUID-1163448F-6B18-4A44-AA8D-7CDF0D1360FB)

* [Using Flashback Database After Issuing an Open Resetlogs Statement](examples-of-using-oracle-data-guard.md#GUID-BAA5ED38-29A0-4E8D-8435-AA083D19C13E)

* [Recovering After the NOLOGGING Clause Is Specified](examples-of-using-oracle-data-guard.md#GUID-BD9E85AB-D812-4659-9C85-26CDFF115A8A)

* [Creating a Standby Database That Uses OMF or Oracle ASM](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540)

* [Recovering From Lost-Write Errors on a Primary Database](examples-of-using-oracle-data-guard.md#GUID-8F4E7807-6013-480F-8780-088F5639732F)

* [Using the DBCOMP Procedure to Detect Lost Writes and Other Inconsistencies](examples-of-using-oracle-data-guard.md#GUID-A2FA9BA2-07F9-492E-8113-3FE8521EB3E3)

* [Converting a Failed Primary into a Standby Database Using RMAN Backups](examples-of-using-oracle-data-guard.md#GUID-B734C79F-A2BB-43A5-89D7-7733089FB577)

* [Changing the Character Set of a Primary Without Re-Creating Physical Standbys](examples-of-using-oracle-data-guard.md#GUID-FCDB44CE-FBFE-43BB-88E3-74267B38FE70)

* [Actions Needed On a Standby After a PDB PITR or PDB Flashback On a Primary](examples-of-using-oracle-data-guard.md#GUID-8D948A24-A3B7-4E4F-917A-00B047CF3CAF)




### Configuring Logical Standby Databases After a Failover {#GUID-CBC9D920-C2D0-4342-ACEF-C16289D1774A}

These are the steps required on a logical standby database after the primary database has failed over to another standby database.

After a failover has occurred, a logical standby database cannot act as a standby database for the new primary database until it has applied the final redo from the original primary database. This is similar to the way the new primary database applied the final redo during the failover. The steps to be performed depend on whether the new primary database was a physical standby or a logical standby database prior to the failover:

* [When the New Primary Database Was Formerly a Physical Standby Database](examples-of-using-oracle-data-guard.md#GUID-C058F492-C55E-4EF4-929C-CA8AF206F349)

* [When the New Primary Database Was Formerly a Logical Standby Database](examples-of-using-oracle-data-guard.md#GUID-424FC856-08A5-4F68-9EEC-93F0EF575A3A)




#### When the New Primary Database Was Formerly a Physical Standby Database {#GUID-C058F492-C55E-4EF4-929C-CA8AF206F349}

These steps demonstrate how to configure a logical standby database to support a new primary database that was a physical standby database before it assumed the primary role. 

In this scenario, SAT is the logical standby database and NYC is the primary database.

1. On the SAT database, issue the following statement to configure the FAL_SERVER parameter to enable automatic recovery of log files.<br>```
SQL> ALTER SYSTEM SET FAL_SERVER='';
SQL> ALTER SYSTEM SET FAL_CLIENT='';
```


2. Call the PREPARE_FOR_NEW_PRIMARY routine to verify that the logical standby database is capable of serving as a standby database to the new primary database. During this step, local copies of log files that pose a risk for data divergence are deleted from the local database. These log files are then requested for re-archival directly from the new primary database.<br>On the SAT database, issue the following statement:
```
SQL> EXECUTE DBMS_LOGSTDBY.PREPARE_FOR_NEW_PRIMARY( -
>  former_standby_type => 'PHYSICAL' -
>  dblink => 'nyc_link');
```


> **note:** 

If the `ORA-16109` message is returned and the 'LOGSTDBY: prepare_for_new_primary failure -- applied too far, flashback required.' warning is written in the alert.log, perform the following steps: 

1. Flash back the database to the SCN as stated in the warning and then 

2. Repeat this step before continuing.

See [Flashing Back a Logical Standby Database to a Specific Applied SCN](examples-of-using-oracle-data-guard.md#GUID-A6260CFC-7ABA-4478-9211-BC665077725B) for an example of how to flash back a logical standby database to an Apply SCN. 

3. On the SAT database, issue the following statement to start SQL Apply:<br>```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
```





#### When the New Primary Database Was Formerly a Logical Standby Database {#GUID-424FC856-08A5-4F68-9EEC-93F0EF575A3A}

These steps demonstrate how to configure a logical standby database to support a new primary database that was a logical standby database before it assumed the primary role. 

In this scenario, SAT is the logical standby database and NYC is the primary database.

1. Ensure the new primary database is ready to support logical standby databases. On the NYC database, ensure the following query returns a value of `NONE`. Otherwise the new primary database has not completed the work required to enable support for logical standby databases. For example: 
```
SQL> SELECT PENDING_ROLE_CHANGE_TASKS FROM V$DATABASE;
```


A value of `NONE` must be returned before you attempt to reinstate an old primary database. 

2. On the SAT database, issue the following statement to configure the FAL_SERVER parameter to enable automatic recover of log files:<br>```
SQL> ALTER SYSTEM SET FAL_SERVER='';
SQL> ALTER SYSTEM SET FAL_CLIENT='';
```


3. Call the `PREPARE_FOR_NEW_PRIMARY` routine to verify the logical standby database is capable of being a standby to the new primary. During this step, local copies of log files which pose a risk for data divergence are deleted from the local database. These log files are then requested for re-archival directly from the new primary database. <br>On the SAT database, issue the following statement:
```
SQL> EXECUTE DBMS_LOGSTDBY.PREPARE_FOR_NEW_PRIMARY( -
> former_standby_type => 'LOGICAL' -
> dblink => 'nyc_link');
```


> **note:** 

If the `ORA-16109` message is returned and the `LOGSTDBY: prepare_for_new_primary failure -- applied too far, flashback required` warning is written in the alert.log file, perform the following steps: 

1. Flash back the database to the SCN as stated in the warning and then 

2. Repeat this step before continuing.

See [Flashing Back a Logical Standby Database to a Specific Applied SCN](examples-of-using-oracle-data-guard.md#GUID-A6260CFC-7ABA-4478-9211-BC665077725B) for an example of how to flash back a logical standby database to an Apply SCN. 

4. On the SAT database, issue the following statements to start SQL Apply:
```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY NEW PRIMARY nyc_link;
```


This statement must always be issued without the real-time apply option enabled. To enable real-time apply on the logical standby database, wait for the above statement to complete successfully, and then issue the following statements:
```
SQL> ALTER DATABASE STOP LOGICAL STANDBY APPLY;
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
```





### Converting a Failed Primary Into a Standby Database Using Flashback Database {#GUID-1163448F-6B18-4A44-AA8D-7CDF0D1360FB}

After a failover occurs, the original primary database can no longer participate in the Oracle Data Guard configuration until it is repaired and established as a standby database in the new configuration. 

To do this, use the Flashback Database feature to recover the failed primary database to a point in time before the failover occurred, and then convert it into a physical or logical standby database in the new configuration. The following sections describe:

* [Flashing Back a Failed Primary Database into a Physical Standby Database ](examples-of-using-oracle-data-guard.md#GUID-C7C39BEC-A841-48D1-BE4B-9BB49C65B9C9)

* [Flashing Back a Failed Primary Database into a Logical Standby Database](examples-of-using-oracle-data-guard.md#GUID-324EBFC6-5358-419A-AD4A-E457273E6560)

> **note:** 

Flashback Database must be enabled on the original primary database before the failover. See [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV71000) for more information. 

* [Flashing Back a Logical Standby Database to a Specific Applied SCN](examples-of-using-oracle-data-guard.md#GUID-A6260CFC-7ABA-4478-9211-BC665077725B)

> **note:** See Also: 

[*Oracle Data Guard Broker*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR3438) for information about automatic reinstatement of the failed primary database as a new standby database (as an alternative to using Flashback Database) 




#### Flashing Back a Failed Primary Database into a Physical Standby Database {#GUID-C7C39BEC-A841-48D1-BE4B-9BB49C65B9C9}

These steps bring the old primary database back into the Oracle Data Guard configuration as a physical standby database.

The following steps assume that a failover has been performed to a physical standby database and that Flashback Database was enabled on the old primary database at the time of the failover. 

1. On the new primary database, issue the following query to determine the SCN at which the old standby database became the new primary database:
```
SQL> SELECT TO_CHAR(STANDBY_BECAME_PRIMARY_SCN) FROM V$DATABASE;
```


2. Shut down the old primary database (if necessary), mount it, and flash it back to the value for `STANDBY_BECAME_PRIMARY_SCN` that was determined in the previous step. 
```
SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
SQL> FLASHBACK DATABASE TO SCN standby_became_primary_scn;
```


3. To convert the database to a physical standby database, issue the following statement on the old primary database:
```
SQL> ALTER DATABASE CONVERT TO PHYSICAL STANDBY;
```


4. To start transporting redo to the new physical standby database, perform the following steps on the new primary database:

1. Issue the following query to see the current state of the archive destinations:
```
SQL> SELECT DEST_ID, DEST_NAME, STATUS, PROTECTION_MODE, DESTINATION, -
> ERROR,SRL FROM V$ARCHIVE_DEST_STATUS;
```


2. If necessary, enable the destination:
```
SQL> ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_n=ENABLE;
```


3. Perform a log switch to ensure the standby database begins receiving redo data from the new primary database, and verify it was sent successfully. Issue the following SQL statements on the new primary database:
```
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> SELECT DEST_ID, DEST_NAME, STATUS, PROTECTION_MODE, DESTINATION,-
> ERROR,SRL FROM V$ARCHIVE_DEST_STATUS;
```


On the new standby database, you may also need to change the `LOG_ARCHIVE_DEST_`*n* initialization parameters so that redo transport services do not transmit redo data to other databases. 

5. Start Redo Apply on the new physical standby database:
```
SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;
```


Redo Apply automatically stops each time it encounters a redo record that is generated as the result of a role transition, so Redo Apply needs to be restarted one or more times until it has applied beyond the SCN at which the new primary database became the primary database. Once the failed primary database is restored and is running in the standby role, you can optionally perform a switchover to transition the databases to their original (pre-failure) roles. See "[Performing a Switchover to a Physical Standby Database](managing-oracle-data-guard-role-transitions.md#GUID-AAD70601-D248-4309-B8DD-F461EE31A5FF)" for more information. 




#### Flashing Back a Failed Primary Database into a Logical Standby Database {#GUID-324EBFC6-5358-419A-AD4A-E457273E6560}

These steps bring the old primary database back into the Oracle Data Guard configuration as a new logical standby database without having to formally instantiate it from the new primary database.

These steps assume that the Oracle Data Guard configuration has already completed a failover involving a logical standby database and that Flashback Database has been enabled on the old primary database.

1. Determine the flashback SCN and the recovery SCN. The flashback SCN is the SCN to which the failed primary database is flashed back. The recovery SCN is the SCN to which the failed primary database is recovered. Issue the following query on the new primary to identify these SCNs:
```
SQL> SELECT merge_change# AS FLASHBACK_SCN, processed_change# AS RECOVERY_SCN -
> FROM DBA_LOGSTDBY_HISTORY -
> WHERE stream_sequence# = (SELECT MAX(stream_sequence#)-1 -
> FROM DBA_LOGSTDBY_HISTORY);
```


2. Flash back the failed primary database to the flashback SCN identified in the previous step:
```
SQL> FLASHBACK DATABASE TO SCN flashback_scn;
```


3. Convert the failed primary into a physical standby, and remount the standby database in preparation for recovery:
```
SQL> ALTER DATABASE CONVERT TO PHYSICAL STANDBY;
```


4. Configure the `FAL_SERVER` parameter to enable automatic recovery of log files. Issue the following statement on the physical standby (failed primary):
```
SQL> ALTER SYSTEM SET FAL_SERVER='';
```


5. Remove any archive logs created at the time of or, after the failover operation, from the failed primary database. If the failed primary database was isolated from the standby, it could have divergent archive logs that are not consistent with the current primary database. To ensure these divergent archive logs are never applied, they must be deleted from backups and the fast recovery area. You can use the following RMAN command to delete the relevant archive logs from the fast recovery area:
```
RMAN> DELETE FORCE ARCHIVELOG FROM SCN FLASHBACK_SCN;
```


Once deleted, these divergent logs and subsequent transactions can never be recovered.<br>6. Recover until the recovery SCN identified in Step 1:
```
SQL> RECOVER MANAGED STANDBY DATABASE UNTIL CHANGE recovery_scn;
```


7. Enable the database guard:
```
SQL> ALTER DATABASE GUARD ALL;
```


8. Activate the physical standby to become a primary database:
```
SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;
```


9. Open the database:
```
SQL> ALTER DATABASE OPEN;
```


10. Create a database link to the new primary, and start SQL Apply:
```
SQL> CREATE PUBLIC DATABASE LINK mylink -
> CONNECT TO system IDENTIFIED BY password -
> USING 'service_name_of_new_primary_database';

SQL> ALTER DATABASE START LOGICAL STANDBY APPLY NEW PRIMARY mylink;
```


The role reversal is now complete.




#### Flashing Back a Logical Standby Database to a Specific Applied SCN {#GUID-A6260CFC-7ABA-4478-9211-BC665077725B}

One of the benefits of a standby database is that Flashback Database can be performed on the standby database without affecting the primary database service. Flashing back a database to a specific point in time is a straightforward task, however on a logical standby database, you may want to flash back to a time just before a known transaction was committed. Such a need can arise when configuring a logical standby database with a new primary database after a failover. These steps describe how to use Flashback Database and SQL Apply to recover to a known applied SCN.

1. Once you have determined the known SCN at the primary (APPLIED_SCN), issue the following query to determine the corresponding SCN at the logical standby database, to use for the flashback operation:
```
SQL> SELECT DBMS_LOGSTDBY.MAP_PRIMARY_SCN (PRIMARY_SCN => APPLIED_SCN) -
> AS TARGET_SCN FROM DUAL;
```


2. Issue the following SQL statements to flash back the logical standby database to the specified SCN, and open the logical standby database with the RESETLOGS option:
```
SQL> SHUTDOWN;
SQL> STARTUP MOUNT EXCLUSIVE;
SQL> FLASHBACK DATABASE TO SCN ;
SQL> ALTER DATABASE OPEN RESETLOGS;
```


3. Issue the following query to confirm SQL Apply has applied less than or up to the `APPLIED_SCN`.
```
SQL> SELECT APPLIED_SCN FROM V$LOGSTDBY_PROGRESS;
```





### Using Flashback Database After Issuing an Open Resetlogs Statement {#GUID-BAA5ED38-29A0-4E8D-8435-AA083D19C13E}

If an error occurrs on the primary database in an Oracle Data Guard configuration in which the standby database is using real-time apply, then the same error is applied on the standby database. if Flashback Database is enabled, you can revert the primary and standby databases back to their pre-error condition.

To do so, issue the `FLASHBACK DATABASE` and `OPEN RESETLOGS` statements on the primary database, and then issuing a similar `FLASHBACK STANDBY DATABASE` statement on the standby database before restarting apply services. (If Flashback Database is not enabled, you need to re-create the standby database, as described in [ Creating a Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B511FB6E-E3E7-436D-94B5-071C37550170) and [ Creating a Logical Standby Database ](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E), after the point-in-time recovery was performed on the primary database.) 

#### Flashing Back a Physical Standby Database to a Specific Point-in-Time {#GUID-09129494-CD98-44B6-821F-A4C2CB5AEE08}

These steps describe how to avoid re-creating a physical standby database after you issue the `OPEN RESETLOGS` statement on the primary database. 

With Oracle Database Release 19c, if the standby is in mount mode and flashback database is enabled, you may not need to do anything. In the common case, if there is sufficient flashback data available, the standby is automatically flashed back so that the standby continues to follow the primary.

If the standby is open and flashback database is enabled, you can place the standby in mount mode and then start standby recovery. The MRP, in an attempt to let the standby follow the primary, triggers an automatic flashback of the standby. If this operation succeeds, as reported in the alert log, you need to follow up with opening the Active Data Guard instance.

If automatic flashback is not triggered, or automatic flashback did not result in the standby following the primary, you can perform the steps described in this section.

1. Determine the SCN before the RESETLOGS operation occurred. For example, on the primary database, use the following query to obtain the value of the system change number (SCN) that is 2 SCNs before the `RESETLOGS` operation occurred on the primary database:
```
SQL> SELECT TO_CHAR(RESETLOGS_CHANGE# - 2) FROM V$DATABASE;
```


2. On the standby database, obtain the current SCN with the following query:
```
SQL> SELECT TO_CHAR(CURRENT_SCN) FROM V$DATABASE;
```


3. If the value of `CURRENT_SCN` is larger than the value of resetlogs_change# - 2, issue the following statement to flash back the standby database.
```
SQL> FLASHBACK STANDBY DATABASE TO SCN resetlogs_change# -2;
```


* If the value of `CURRENT_SCN` is less than the value of the resetlogs_change# - 2, skip to Step 4. 

* If the standby database's SCN is far enough behind the primary database's SCN, and the new branch of redo from the `OPEN RESETLOGS` statement has been registered at the standby, then apply services can continue through the `OPEN RESETLOGS` statement without stopping. In this case, flashing back the database is unnecessary because apply services do not stop upon reaching the `OPEN RESETLOGS` statement in the redo data. 

4. To start Redo Apply on the physical standby database, issue the following statement: 
```
SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;
```


The standby database is now ready to receive and apply redo from the primary database.




#### Flashing Back a Logical Standby Database to a Specific Point-in-Time {#GUID-A2191DBB-AA91-4C42-8F01-7B7E9529F465}

These steps describe how to avoid re-creating a logical standby database after you have flashed back the primary database and opened it by issuing an `OPEN RESETLOGS` statement. 

> **note:** 

If SQL Apply detects the occurrence of a resetlogs operation at the primary database, it automatically mines the correct branch of redo, if it is possible to do so without having to flashback the logical standby database. Otherwise, SQL Apply stops with an error `ORA-1346: LogMiner processed redo beyond specified reset log scn`. In this section, it is assumed that SQL Apply has already stopped with such an error. 

1. On the primary database, use the following query to obtain the value of the system change number (SCN) that is 2 SCNs before the `RESETLOGS` operation occurred on the primary database:
```
SQL> SELECT TO_CHAR(RESETLOGS_CHANGE# - 2) AS FLASHBACK_SCN FROM V$DATABASE;
```


2. Determine the target SCN for flashback operation at the logical standby. 

In this step, the `FLASHBACK_SCN` value for `PRIMARY_SCN` is from Step 1. 
```
SQL> SELECT DBMS_LOGSTDBY.MAP_PRIMARY_SCN (PRIMARY_SCN => FLASHBACK_SCN) -
> AS TARGET_SCN FROM DUAL;
```


3. Issue the following SQL statements to flash back the logical standby database to the specified SCN, and open the logical standby database with the `RESETLOGS` option:
```
SQL> SHUTDOWN;
SQL> STARTUP MOUNT EXCLUSIVE;
SQL> FLASHBACK DATABASE TO SCN ;
SQL> ALTER DATABASE OPEN RESETLOGS;
```


4. Confirm that a log file from the primary's new branch is registered before SQL apply is started. Issue the following query on the primary database:
```
SQL> SELECT resetlogs_id FROM V$DATABASE_INCARNATION WHERE status = 'CURRENT';
```


Issue the following query on the standby database:
```
SQL> SELECT * FROM DBA_LOGSTDBY_LOG WHERE resetlogs_id = resetlogs_id_at_primary;
```


If one or more rows are returned, it confirms that there are registered logfiles from the primary's new branch.

5. Start SQL Apply:
```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
```





### Recovering After the NOLOGGING Clause Is Specified {#GUID-BD9E85AB-D812-4659-9C85-26CDFF115A8A}

Some SQL statements allow you to specify a `NOLOGGING` clause so that the operation is not logged in the online redo log file. 

In actuality, when you specify `NOLOGGING`, a redo record is still written to the online redo log file, but there is no data associated with the record. This specification can result in log application or data access errors at the standby site. Manual recovery might be required to resume applying log files. Depending on whether you have a logical standby or physical standby, you can avoid these errors by doing the following: 

* Logical standbys<br>Specify the `FORCE LOGGING` clause in the `CREATE DATABASE` or `ALTER DATABASE` statements. 

* Physical standbys<br>Specify a logging mode that is appropriate to the way in which you plan to use your Data Guard configuration. <br>See [Enable an Appropriate Logging Mode](creating-oracle-data-guard-physical-standby.md#GUID-AE2B1237-57A1-4745-A04C-4246A831A963). 




You can see the current logging mode in the `V$DATABASE.FORCE_LOGGING` column (for CDBs), or the `DBA_PDBS.FORCE_LOGGING` column (for PDBs). 

#### Recovery Steps for Logical Standby Databases {#GUID-BB804A80-7F19-4087-BAFA-97552C1DAAEF}

On logical standby databases, when SQL Apply encounters a redo record for an operation performed with the `NOLOGGING` clause on a table that is not being skipped, it stops with the following error: 
```
ORA-16211 unsupported record found in the archived redo log
```


To recover after the `NOLOGGING` clause is specified, re-create one or more tables from the primary database, as described in [Adding or Re-Creating Tables On a Logical Standby Database](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA). 

> **note:** 

In general, use of the `NOLOGGING` clause is not recommended. Therefore, if you know in advance that operations using the `NOLOGGING` clause will be performed on certain tables in the primary database, then you might want to prevent the application of SQL statements associated with these tables to the logical standby database. You can do this by using the `DBMS_LOGSTDBY.SKIP` procedure. 

#### Recovery Steps for Physical Standby Databases {#GUID-DDCEF91D-3D6A-45EC-B0ED-8BEBC32FC18B}

When the redo is applied to the physical standby database, a portion of the data file is marked as being unrecoverable. When you either fail over to the physical standby database, or open the standby database for read-only access, and attempt to read the range of blocks that are marked as `UNRECOVERABLE`, you see error messages similar to the following: 
```
ORA-01578: ORACLE data block corrupted (file # 1, block # 2521)
ORA-01110: data file 1: '/oracle/dbs/stdby/tbs_1.dbf'
ORA-26040: Data block was loaded using the NOLOGGING option
```


When you use `STANDBY NOLOGGING FOR LOAD PERFORMANCE` mode it is still possible for a query executed on a physical standby to report a corrupt block due to a nologging operation. However, when managed recovery is running on that standby as part of an Active Data Guard configuration, it automatically fetches a replacement block from the primary. (Managed recovery is started with the `ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT` statement.) Managed recovery attempts to fetch all such blocks shortly after they are first created and continues attempting to fix them until it succeeds. Whenever managed recovery is started, it again begins fixing any outstanding blocks from earlier recovery sessions. This automatic recovery applies only to corrupt blocks caused by redo generated with the `STANDBY NOLOGGING FOR LOAD PERFORMANCE` mode enabled. Blocks corrupted due to conventional nonlogged operations must use the following procedure. These steps describe a simple approach that recovers all nonlogged blocks. (See follow-on sections for other approaches such as determining whether a backup is required after unrecoverable operations, and recovering parts of a physical standby database.) 

1. Stop recovery on the standby:
```
SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
```


2. Recover the nonlogged blocks by connecting RMAN to the standby and issue the following command:
```
RMAN> RECOVER DATABASE NONLOGGED BLOCK;
```


If the presence of unrecoverable blocks is only found after a switchover, then you can use these same two steps, but the primary database must be just mounted ( not open) and, RMAN must be connected to the primary.<br>It is possible that the RMAN `RECOVER` command may not be able to recover all the nonlogged blocks. The reasons that this might happen are detailed in the alert log of the database from which the RMAN command was executed. The most common reason is that a block has been modified recently at the primary and not yet written to its corresponding data file. This may mean that the block sent to the standby is too old to replace the unrecoverable block on the standby. To resolve this, issue the `RECOVER` command again at a later time after the block has been written out to the data file. 




The following is an example of an alert log entry for an execution of the `RECOVER` command which left some blocks unrecovered: 
```
Started Nonlogged Block Replacement recovery on file 7 (ospid 13005 rcvid 11319003446180375696)
Finished Nonlogged Block Replacement recovery on file 7. 5 blocks remain
Statistics for replacement block source database (service=dg3tns)
(Use of it stopped due to error 12942 received from it)
Blocks requested 5, blocks received 0.

Reason replacement blocks accepted or rejected               Blocks Last block
-------------------------------------------------------- ---------- ----------
Not received: Rejected by sender. Wrong state or SCN              5         21
```


In this case, the command was run on a standby and the primary did not send any blocks but instead reported the following Oracle error: .
```
ORA-12942: database incarnation at source does not match
```


An examination of the alert logs would reveal that the primary had performed `FLASHBACK DATABASE` and `OPEN RESETLOGS` commands, but the standby had not been flashed back. This means the standby would now be on an orphaned branch of redo and therefore the primary could not supply data blocks that would be known to be the correct version. 

> **note:** When a Data Guard configuration has more than one standby, an RMAN `RECOVER` command that is run at a standby attempts to fetch blocks from the primary only. When the `RECOVER` command is run at the primary it attempts to fetch blocks from only the one standby that it determines to be most likely to yield good blocks, which typically means the standby that is closest to being in synch with the primary. 

#### Determining If a Backup Is Required After Unrecoverable Operations {#GUID-55881B7E-55C1-4342-94DA-A75739504980}

If you performed unrecoverable operations on your primary database, then you need to determine if a new backup operation is required.

To do so, take the following steps:

1. Query the `V$DATAFILE` view on the primary database to determine the **system change number (SCN)** or the time at which the Oracle database generated the most recent invalidated redo data.
2. Issue the following SQL statement on the primary database to determine if you need to perform another backup:
```
SQL> SELECT UNRECOVERABLE_CHANGE#,-
> TO_CHAR(UNRECOVERABLE_TIME, 'mm-dd-yyyy hh:mi:ss') -
> FROM V$DATAFILE;
```


3. If the query in the previous step reports an unrecoverable time for a data file that is more recent than the time when the data file was last backed up, then make another backup of the data file in question.



See [*Oracle Database Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN30050) for more information about the `V$DATAFILE` view. 

#### Recovery Steps for Part of a Physical Standby Database {#GUID-C342EF3D-EAB7-4B2D-A638-903345C6A3EC}

The RMAN `RECOVER ... NONLOGGED BLOCK` command can be used to recover blocks that belong to a set of data files or a set of tablespaces or just a single pluggable database (PDB) as well as the multitenant container database (CDB). 

This ability may be useful if it is acceptable to have nonlogged blocks remain in some data files, for example because it is explicitly known that the nonlogged blocks were created as a result of loading rows into an object that has now been dropped.

The view `V$NONLOGGED_BLOCK` usually lists the ranges of known invalid blocks for each data file and the entries are maintained as part of media recovery. However, there are times when the information is not complete. Typically this is after upgrading from a release prior to Oracle Database 12*c* Release 1 (12.1) or after restoring an operating system backup of a data file. The next time media recovery is run, the stale entries are removed and any newly invalidated blocks are recorded but any prior invalid blocks do not have entries in `V$NONLOGGED_BLOCK`. The `FIRST_NONLOGGED_SCN` column in the `V$DATAFILE` view can still be used to see that there is at least one invalid block in a data file even when there are no `V$NONLOGGED_BLOCK` entries for a data file. 

The RMAN command `VALIDATE ... NONLOGGED BLOCK` can be used to bring the entries in `V$NONLOGGED_BLOCK` back into synchronization with the data files. It does this by determining if the existing ranges are complete and if not, it scans the necessary data files to identify any invalid blocks and make sure they are captured by an entry in `V$NONLOGGED_BLOCK`. The `VALIDATE ... NONLOGGED BLOCK` command has the same options as the `RECOVER ... NONLOGGED BLOCK` command to validate just a set of data files or a set of tablespaces or a PDB, as well as the CDB. 

> **note:** Entries in the `V$NONLOGGED_BLOCK` view represent a superset of the invalid blocks, and some normal blocks that are close to an invalid block may be included. For example, if there is an entry for file 7 that starts at block 100 and has 50 blocks in it, then none, some, or all of those 50 blocks are invalid. After the `VALIDATE` command is run, there are no ranges that have no invalid blocks in them and the first and last block of a range are also be invalid blocks. However, there are limits to the total number of ranges that can be held in the control file so sometimes it may be necessary to merge ranges for the same file, causing regular blocks to be included in a range. 

If only offline data files are to be validated or recovered then the database to which they belong can be open at the time the RMAN command is run.

### Creating a Standby Database That Uses OMF or Oracle ASM {#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540}

When you create standby databases, there are additional steps that must be performed if the primary database uses Oracle Managed Files (OMF) or Oracle Automatic Storage Management (Oracle ASM).

The discussion in this section is presented at a level of detail that assumes you already know how to create a physical standby database and are an experienced user of the RMAN, OMF, and Oracle ASM features.

Perform the following tasks to prepare for standby database creation:

1. Enable forced logging on the primary database.

2. Enable archiving on the primary database.

3. Set all necessary initialization parameters on the primary database.

4. Create an initialization parameter file for the standby database.

5. If the primary database is configured to use OMF, then Oracle recommends that the standby database be configured to use OMF, too. To do this, set the `DB_CREATE_FILE_DEST` and `DB_CREATE_ONLINE_LOG_DEST_`*n* initialization parameters to appropriate values. Maintenance and future role transitions are simplified if the same disk group names are used for both the primary and standby databases. 

> **note:** 

If OMF parameters are set on the standby, then new files on that standby are always created as OMF, regardless of how they were created on the primary. Therefore, if both the `DB_FILE_NAME_CONVERT` and `DB_CREATE_FILE_DEST` parameters are set on the standby, the `DB_CREATE_FILE_DEST` parameter takes precedence. 

6. Set the `STANDBY_FILE_MANAGEMENT` initialization parameter to `AUTO`. 

7. Configure Oracle Net, as required, to allow connections to the standby database.

8. Configure redo transport authentication as described in [Configure Redo Transport Authentication](creating-oracle-data-guard-physical-standby.md#GUID-0DC30726-3471-4588-BFE0-9CA0736328E2). 

9. Start the standby database instance without mounting the control file.




Perform the following tasks to create the standby database:

1. If the standby database is going to use Oracle ASM, create an Oracle ASM instance if one does not already exist on the standby database system.

2. Use the RMAN `BACKUP` command to create a backup set that contains a copy of the primary database's data files, archived log files, and a standby control file. 

3. Use the RMAN `DUPLICATE FOR STANDBY` command to copy the data files, archived redo log files and standby control file in the backup set to the standby database's storage area. <br>The `DUPLICATE FOR STANDBY` command performs the actual data movement at the standby instance. If the backup set is on tape, the media manager must be configured so that the standby instance can read the backup set. If the backup set is on disk, the backup pieces must be readable by the standby instance, either by making their primary path names available through Network File Storage (NFS), or by copying them to the standby system and using RMAN `CATALOG BACKUPPIECE` command to catalog the backup pieces before restoring them. 




After you successfully complete these steps, continue with the steps in [Verify the Physical Standby Database Is Performing Properly](creating-oracle-data-guard-physical-standby.md#GUID-AAA6D97B-A345-4825-A320-B662BB16E2ED), to verify the configuration of the physical standby database. 

To create a logical standby database, continue with the standby database creation process described in [ Creating a Logical Standby Database ](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E), but with the following modifications: 

1. For a logical standby database, setting the `DB_CREATE_FILE_DEST` parameter does not force the creation of OMF filenames. However, if this parameter was set on the primary database, it must also be set on the standby database.
2. After creating a logical standby control file on the primary system, do not use an operating system command to copy this file to the standby system. Instead, use the RMAN `RESTORE CONTROLFILE` command to restore a copy of the logical standby control file to the standby system.
3. If the primary database uses OMF files, use RMAN to update the standby database control file to use the new OMF files created on the standby database. To perform this operation, connect only to the standby database, as shown in the following example:
```
> RMAN TARGET sys@lstdby

target database Password: password

RMAN> CATALOG START WITH '+stby_diskgroup';
RMAN> SWITCH DATABASE TO COPY;
```





After you successfully complete these steps, continue with the steps in [Open the Logical Standby Database](creating-oracle-data-guard-logical-standby.md#GUID-68777C01-83F6-4CCF-9BA2-547ED180138D) to start, recover, and verify the logical standby database. 

> **note:** See Also: 

* [ Creating a Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B511FB6E-E3E7-436D-94B5-071C37550170)

* [ Creating a Logical Standby Database ](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E)

* [ Creating a Standby Database with Recovery Manager](creating-data-guard-standby-database-using-RMAN.md#GUID-82731D59-A20F-45DD-A235-267B3B0E38C5)

* [*Oracle Database Administrator's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ADMIN003) for information about OMF 

* [*Oracle Automatic Storage Management Administrator's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=OSTMG036) for more information about Oracle ASM 

* [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89346) for information about RMAN 




### Recovering From Lost-Write Errors on a Primary Database {#GUID-8F4E7807-6013-480F-8780-088F5639732F}

During media recovery in an Oracle Data Guard configuration, a physical standby database can be used to detect lost-write data corruption errors on the primary database.

This is done by comparing SCNs of blocks stored in the redo log on the primary database to SCNs of blocks on the physical standby database. If the SCN of the block on the primary database is lower than the SCN on the standby database, then there was a lost-write error on the primary database.

In such a situation, if lost write detection (set with the [`DB_LOST_WRITE_PROTECT`](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN10268) initialization parameter) is enabled at both the primary and standby, then a recovery attempt at the standby results in an `ORA-752` error. If lost write detection is not enabled, then a recovery attempt results in an `ORA-600` `[3020]` error. However, not all `ORA-600` `[3020]` errors are due to lost writes at the primary. Therefore, before following the guidelines given in this section, work with your Oracle Support representative to determine whether the root cause for the `ORA-600` `[3020]` error was indeed a lost write that occurred on the primary. Also see "Resolving ORA-752 or ORA-600 [3020] During Standby Recovery" in the My Oracle Support Note 1265884.1 at [`http://support.oracle.com`](http://support.oracle.com). 

> **note:** 

Because lost-write errors are detected only when a block is read into the cache by a primary and the corresponding redo is later compared to the block on the standby, there may be undetected stale blocks on both the primary and the standby that have not yet been read and verified. These stale blocks do not affect operation of the current database because until those blocks are read, all blocks that have been used up to the SCN of the currently applied redo on the standby to do queries or updates were verified by the standby.

When a primary lost-write error is detected on the standby, one or more block error messages similar to the following for each stale block are printed in the alert file of the standby database:
```
Tue Dec 12 19:09:48 2006
STANDBY REDO APPLICATION HAS DETECTED THAT THE PRIMARY DATABASE
LOST A DISK WRITE OF BLOCK 26, FILE 7
NO REDO AT OR AFTER SCN 389667 CAN BE USED FOR RECOVERY.
.
.
.
```


The alert file then shows that an `ORA-00752` error is raised on the standby database and the managed recovery is cancelled: 
```
Slave exiting with ORA-752 exception
Errors in file /oracle/log/diag/rdbms/dgstwrite2/stwrite2/trace/stwrite2_pr00_23532.trc:
ORA-00752: recovery detected a lost write of a data block
ORA-10567: Redo is inconsistent with data block (file# 7, block# 26)
ORA-10564: tablespace TBS_2
ORA-01110: data file 7: '/oracle/dbs/btbs_21.f'
ORA-10561: block type 'TRANSACTION MANAGED DATA BLOCK', data object# 57503
.
.
.
```


The standby database is then recovered to a consistent state, without any corruption to its data files caused by this error, at the SCN printed in the alert file:
```
Recovery interrupted!
Recovered data files to a consistent state at change 389569
```


This last message may appear significantly later in the alert file and it may have a lower SCN than the block error messages. Also, the primary database may operate without visible errors even though its data files may already be corrupted.

The recommended procedure to recover from such errors is a failover to the physical standby, as described in the following steps.

Steps to Failover to a Physical Standby After Lost-Writes Are Detected on the Primary

1. Shut down the primary database. All data at or after the SCN printed in the block error messages is lost.

2. Issue the following SQL statement on the standby database to convert it to a primary:
```
SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Database altered.

Tue Dec 12 19:15:23 2006
alter database activate standby database
ALTER DATABASE ACTIVATE [PHYSICAL] STANDBY DATABASE (stwrite2)
RESETLOGS after incomplete recovery UNTIL CHANGE 389569
Resetting resetlogs activation ID 612657558 (0x24846996)
Online log /oracle/dbs/bt_log1.f: Thread 1 Group 1 was previously cleared
Online log /oracle/dbs/bt_log2.f: Thread 1 Group 2 was previously cleared
Standby became primary SCN: 389567
Tue Dec 12 19:15:23 2006
Setting recovery target incarnation to 3
Converting standby mount to primary mount.
ACTIVATE STANDBY: Complete - Database mounted as primary (stwrite2)
Completed: alter database activate standby database
```


3. Back up the new primary. Performing a backup immediately is a necessary safety measure, because you cannot recover changes made after the failover without a complete backup copy of the database. As a result of the failover, the original primary database can no longer participate in the Oracle Data Guard configuration, and all other standby databases now receive and apply redo data from the new primary database.

4. Open the new primary database.

5. An optional step is to recreate the failed primary as a physical standby. You can do this using the database backup taken at the new primary in step 3. (You cannot use flashback database or the Oracle Data Guard broker to reinstantiate the old primary database in this situation.) Be aware that a physical standby created using the backup taken from the new primary has the same data files as the old standby. Therefore, any undetected lost writes that the old standby had before it was activated are not detected by the new standby, since the new standby compares the same blocks. Any new lost writes that happen on either the primary or the standby are detected.




> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89476) for more information about enabling lost-write detection 

### Using the DBCOMP Procedure to Detect Lost Writes and Other Inconsistencies {#GUID-A2FA9BA2-07F9-492E-8113-3FE8521EB3E3}

You can use the PL/SQL procedure, `DBMS_DBCOMP.DBCOMP`, to detect lost writes and also to detect inconsistencies between a primary database and physical standby databases. 

The `DBMS_DBCOMP.DBCOMP` procedure compares the same data blocks on the primary and physical standby databases. The procedure can be executed at any time. (It does not require that the `DB_LOST_WRITE_PROTECT` initialization parameter be enabled.) 

You can monitor the progress of an on-going block comparison operation by querying the `V$SESSION_LONGOPS` view. 

> **note:** Logical standby databases, far sync instances, and cascaded standbys cannot be the target database for the `DBMS_DBCOMP.DBCOMP` procedure. 

The `DBMS_DBCOMP.DBCOMP` procedure assumes that there is one primary database and one or more physical standby databases. The databases should be at least mounted before block comparison. 

In the following example situations, assume that there is a primary database with a unique name of `dgmain`, and that physical standby databases are named `dgmainb`, `dgmainc`, `dgmaind`, and so on. 

Example 15-1 Primary and All Standbys Are Mounted or Open and DBCOMP Is Executed From the Primary

In this situation, when the `DBCOMP` procedure is executed from the primary database, the specified data files are compared block by block between the primary and every physical standby database. For example, suppose that you perform the following query: 
```
SQL> exec sys.dbms_dbcomp.dbcomp(‘1’,’BlockCompare’,:retval);
```


The generated output files are `BlockCompare_dgmainb_1` and `BlockCompare_dgmainc_d_1`. 

Example 15-2 Primary and All Standbys Are Mounted or Open and DBCOMP Is Executed From a Standby

In this situation, when the `DBCOMP` procedure is executed from one of the standby databases (for example, `dgmainb`), the specified data files are compared only between the primary and that particular standby database. Other standby databases are not considered. For example, suppose that you perform the following query: 
```
SQL> exec sys.dbms_dbcomp.dbcomp(‘1’,’BlockCompare’,:retval);
```


The generated output file is `BlockCompare_dgmain_1`. 

Example 15-3 Primary Is Mounted or Open, But Not All Standbys Are, and DBCOMP is Executed From the Primary

In this situation, when the `DBCOMP` procedure is executed on the primary, the specified data files are compared between the primary database and the mounted or open physical standby databases. For those standby databases that are neither mounted nor open, no action is taken. 

Example 15-4 Primary Is Mounted or Open, But Not All Standbys Are, and DBCOMP is Executed From a Standby

In this situation, the specified data files are compared between the primary and the standby from which the `DBCOMP` procedure is executed. 

Example 15-5 Primary is Not Mounted, But Multiple Standbys Are Mounted or Open

Because the primary database is neither mounted nor open, the `DBCOMP` procedure cannot find an appropriate pair of primary and physical standby databases to compare. An `ORA` error message is not returned, but a message similar to the following is printed out in the corresponding output file: `Remote database is not in the primary role`. 

Example 15-6 Primary Is Mounted or Open, But No Standbys Are Mounted or Open

Because no appropriate pair of primary and physical standby databases are found, a message is printed out in the corresponding output file, but no `ORA` error is returned. 

**Related Topics**

* [*Oracle Database PL/SQL Packages and Types Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS-GUID-4A248C6E-F52E-4841-B26B-139DD33012B1)



### Converting a Failed Primary into a Standby Database Using RMAN Backups {#GUID-B734C79F-A2BB-43A5-89D7-7733089FB577}

To convert a failed primary database, Oracle recommends that you enable the Flashback Database feature on the primary and follow one of these procedures, as appropriate.

* [Flashing Back a Failed Primary Database into a Physical Standby Database ](examples-of-using-oracle-data-guard.md#GUID-C7C39BEC-A841-48D1-BE4B-9BB49C65B9C9)

* [Flashing Back a Failed Primary Database into a Logical Standby Database](examples-of-using-oracle-data-guard.md#GUID-324EBFC6-5358-419A-AD4A-E457273E6560)




The procedures in these sections describe the fastest ways to convert a failed primary into either a physical or logical standby. However, if Flashback Database was not enabled on the failed primary, you can still convert the failed primary into either a physical or logical standby using a local backup of the failed primary, as described in the following topics: 

* [Converting a Failed Primary into a Physical Standby Using RMAN Backups](examples-of-using-oracle-data-guard.md#GUID-433907DE-DF34-4CE6-989F-8F32F983FD99)

* [Converting a Failed Primary into a Logical Standby Using RMAN Backups](examples-of-using-oracle-data-guard.md#GUID-D93BA2CD-A50B-43A5-91C7-A719998057FC)




#### Converting a Failed Primary into a Physical Standby Using RMAN Backups {#GUID-433907DE-DF34-4CE6-989F-8F32F983FD99}

These steps describe how to convert a failed primary into a physical standby by using RMAN backups. 

This procedure requires that the `COMPATIBLE` initialization parameter of the old primary be set to at least 11.0.0. 

1. On the new primary database, issue the following query to determine the SCN at which the old standby database became the new primary database:
```
SQL> SELECT TO_CHAR(STANDBY_BECAME_PRIMARY_SCN) FROM V$DATABASE;
```


2. Restore the database with a backup taken before the old primary had reached the SCN at which the standby became the new primary (`standby_became_primary_scn)`. Then, perform a point-in-time recovery to recover the old primary to that same point. <br>Issue the following RMAN commands:
```
RMAN> RUN
{
SET UNTIL SCN ;
RESTORE DATABASE;
RECOVER DATABASE;
}
```


With user-managed recovery, you can first restore the database manually. Typically, a backup taken a couple of hours before the failover would be old enough. You can then recover the failed primary using the following command:
```
SQL> RECOVER DATABASE USING BACKUP CONTROLFILE UNTIL CHANGE -
>  ;
```


Unlike a reinstantiation that uses Flashback Database, this procedure adds one to `standby_became_primary_scn`. For data files, flashing back to an SCN is equivalent to recovering up until that SCN plus one. 

3. Perform the following steps on the old primary database:

1. Issue the following statement on the old primary database:
```
SQL> ALTER DATABASE CONVERT TO PHYSICAL STANDBY;
```


2. Shut down and restart the database:
```
SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
```


4. Issue the following command:
```
SQL> ALTER DATABASE OPEN READ ONLY;
```


The goal of this step is to synchronize the control file with the database by using a dictionary check. After this command, check the alert log for any actions suggested by the dictionary check. Typically, no user action is needed if the old primary was not in the middle of adding or dropping data files during the failover.

5. If you have purchased a license for the Oracle Active Data Guard option and would like to operate your physical standby database in active query mode, skip this step. Otherwise, bring your standby database to the mount state.<br>For example:
```
SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
```


6. Before the new standby database was created, the new primary database probably stopped transmitting redo to the remote destination. To restart redo transport services, perform the following steps on the new primary database:

1. Issue the following query to see the current state of the archive destinations:
```
SQL> SELECT DEST_ID, DEST_NAME, STATUS, PROTECTION_MODE, DESTINATION, -
> ERROR,SRL FROM V$ARCHIVE_DEST_STATUS;
```
```
```


2. If necessary, enable the destination:
```
SQL> ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_n=ENABLE;
```


3. Perform a log switch to ensure the standby database begins receiving redo data from the new primary database, and verify it was sent successfully. 

> **note:** 

This is an important step in order for the old primary to become a new standby following the new primary. If this step is not done, the old primary may recover to an incorrect database branch. The only way to correct the problem then is to convert the old primary again.

At the SQL prompt, enter the following statements:
```
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> SELECT DEST_ID, DEST_NAME, STATUS, PROTECTION_MODE, DESTINATION, -
> ERROR,SRL FROM V$ARCHIVE_DEST_STATUS;
```


On the new standby database, you may also need to change the `LOG_ARCHIVE_DEST_`*n* initialization parameters so that redo transport services do not transmit redo data to other databases. This step can be skipped if both the primary and standby database roles were set up with the `VALID_FOR` attribute in one server parameter file (SPFILE). By doing this, the Oracle Data Guard configuration operates properly after a role transition. 

7. Start Redo Apply on the new physical standby database, as follows:
```
SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;
```


Once the failed primary database is restored and is running in the standby role, you can optionally perform a switchover to transition the databases to their original (pre-failure) roles. See "[Performing a Switchover to a Physical Standby Database](managing-oracle-data-guard-role-transitions.md#GUID-AAD70601-D248-4309-B8DD-F461EE31A5FF)" for more information. 




#### Converting a Failed Primary into a Logical Standby Using RMAN Backups {#GUID-D93BA2CD-A50B-43A5-91C7-A719998057FC}

These steps describe how to convert a failed primary into a logical standby using RMAN backups. 

1. On the new primary database, issue the following query to determine the SCN to which you want to recover the failed primary database:
```
SQL> SELECT APPLIED_SCN RECOVERY_SCN FROM V$LOGSTDBY_PROGRESS;
```


Also on the new primary database, determine the SCN to use in dealing with archive logs, as follows:

1. Ensure all standby redo logs have been archived. Issue the following query, looking for a value of `NONE` to be returned. Depending on the size of the database and the number of logs needing to be archived, it could take some time before a status of `NONE` is returned. 
```
SQL> SELECT PENDING_ROLE_CHANGE_TASKS FROM V$DATABASE;
```


2. After a status of `NONE` has been returned, run the following query to retrieve the SCN for dealing with archive logs as part of this recovery: 
```
SQL> SELECT VALUE ARCHIVE_SCN FROM SYSTEM.LOGSTDBY$PARAMETERS -
> WHERE NAME='STANDBY_BECAME_PRIMARY_SCN';
```


2. Remove any archive logs created at the time of, or after the failover operation, from the failed primary database. If the failed primary database was isolated from the standby, it could have divergent archive logs that are not consistent with the current primary database. To ensure these divergent archive logs are never applied, they must be deleted from backups and the fast recovery area. You can use the following RMAN command to delete the relevant archive logs from the fast recovery area:
```
RMAN> DELETE FORCE ARCHIVELOG FROM SCN ARCHIVE_SCN;
```


Once deleted, these divergent logs and subsequent transactions can never be recovered.<br>3. On the new primary database, issue the following query to determine the minimum set of log files that must be copied to the failed primary database before recovering from a backup:
```
SQL> SELECT file_name FROM DBA_LOGSTDBY_LOG WHERE next_change# > ARCHIVE_SCN;
```


Retrieve the required standby logs, copy the backup set to the new standby and restore it to the new standby fast recovery area. Because these logs are coming from standby redo logs, they are not part of the standby's standard archives. The RMAN utility is able to use a partial file name to retrieve the files from the correct location.<br>The following is a sample use of the RMAN `BACKUP` command: 
```
RMAN> BACKUP AS COPY DEVICE TYPE DISK FORMAT '/tmp/test/%U'
> ARCHIVELOG LIKE '%';
```


The following is a sample use of the RMAN `RESTORE` command: 
```
RMAN> CATALOG START WITH '/tmp/test';
RMAN> RESTORE ARCHIVELOG FROM SEQUENCE 33 UNTIL SEQUENCE 35;
```


4. Restore a backup of all the original primary's data files and recover to `RECOVERY_SCN + 1`. Oracle recommends that you leverage the current control file. 

1. Start up the database in restricted mode to protect it from rogue transactions until the `GUARD ALL` command can be issued after the database has been opened. 

2. Use the backup to restore the data files of the failed primary database.

3. Turn off flashback database, if it is enabled (necessary for the `USING BACKUP CONTROLFILE` clause). 

4. Perform point-in-time recovery to `RECOVERY_SCN +1` in SQL*Plus. 

Whether you are using a current control file or a backup control file, you must specify the `USING BACKUP CONTROLFILE `clause to allow you to point to the archive logs being restored. Otherwise, the recovery process could attempt to access online redo logs instead of the logs retrieved in Step 3. When prompted for the sequences retrieved in Step 3, ensure you specify the file names of the restored archive log copies, as follows: 
```
SQL> RECOVER DATABASE UNTIL CHANGE RECOVERY_SCN + 1 USING BACKUP CONTROLFILE;
```


5. Open the database with the RESETLOGS option:
```
SQL> ALTER DATABASE OPEN RESETLOGS;
```


6. Enable Database Guard
```
SQL> ALTER DATABASE GUARD ALL;
```


7. Create a database link to the new primary database and start SQL Apply:
```
SQL> CREATE PUBLIC DATABASE LINK myLink -
> CONNECT TO SYSTEM IDENTIFIED BY password -
> USING 'service name of new primary database';
```
```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY NEW PRIMARY myLink;
```


At this point, you can disable restricted session (`ALTER SYSTEM DISABLE RESTRICTED SESSION`) or, if you need to restart the database to re-enable Flashback from Step 4c, let this restart turn off `RESTRICTED SESSION`. 




### Changing the Character Set of a Primary Without Re-Creating Physical Standbys {#GUID-FCDB44CE-FBFE-43BB-88E3-74267B38FE70}

Oracle Data Guard allows you to change both the database character set and the national character set of a primary database without requiring you to recreate any physical standby databases in the configuration. 

You can continue to use your physical standby database with minimal disruption while performing character set conversion of a primary database.

The character set migration process consists of preparatory steps such as scanning for possible issues and identifying methods to solve them. During the execution of these preparatory steps the Oracle Data Guard configuration can operate unchanged and no extra steps are required to maintain the physical standby. After the preparatory steps are completed, the actual conversion is performed which may involve changes to both system data (metadata) and user data. Several procedures specific to Oracle Data Guard must be run as part of the conversion. The steps to run these procedures are interspersed with the steps performed by the Database Migration Assistant for Unicode (DMU) or other appropriate character set migration tool.

For a detailed description of the steps involved in this process, see My Oracle Support note 1124165.1 at [`http://support.oracle.com`](http://support.oracle.com). 

### Actions Needed On a Standby After a PDB PITR or PDB Flashback On a Primary {#GUID-8D948A24-A3B7-4E4F-917A-00B047CF3CAF}

After you perform a PDB Point-in-Time Recovery (PITR) or PDB Flashback on a primary, you can either restore the PDB or flashback the PDB on the standby to let the standby follow the primary.

With Oracle Database Release 19c, if the standby is in mount mode and flashback database is enabled, you may not need to do anything. In the common case, if there is sufficient flashback data, the standby is automatically flashed back so that the standby continues to follow the primary.

In all other cases, that is if the standby is open, refer to the "Recovery After PDB PITR or PDB Flashback in an Active Data Guard Environment" section.

**Related Topics**

* [Recovery After PDB PITR or PDB Flashback in an Active Data Guard Environment](examples-of-using-oracle-data-guard.md#GUID-45590356-133B-47DC-AD19-3AA37348CD4D)



### Recovery After PDB PITR or PDB Flashback in an Active Data Guard Environment {#GUID-45590356-133B-47DC-AD19-3AA37348CD4D}

In an Active Data Guard environment, PDB recovery isolation ensures that media recovery of a CDB on the standby is not impacted when one or more PDBs are not consistent with the rest of the CDB.

About PDB Recovery Isolation

> **note:** PDB recovery isolation is enabled only in an Active Data Guard environment. 

When Active Data Guard media recovery discovers that a hot cloning, point-in-time recovery, or flashback database operation was performed on a PDB, it performs the following steps:

* Temporarily marks the PDB as disabled and continues with CDB media recovery.

* Automatically recovers the PDB in a separate background session. This is called PDB recovery isolation.

* Enables the PDB after the PDB recovery isolation completes.

* Merges the PDB recovery isolation session with the Active Data Guard media recovery session.




For PDB recovery isolation to be performed, the CDB must be open. Multiple PDB recovery isolation sessions can work in parallel, one for each PDB. When Active Data Guard media recovery is stopped and then restarted, the PDB recovery isolation is automatically restarted.

Although PDB recovery isolation is an automatic process, you can manually stop and start PDB recovery isolation. If you manually stop PDB recovery isolation, you must subsequently restart it. Else PDB recovery isolation remains disabled.

To manually stop PDB recovery isolation:

* Use the `ALTER PLUGGABLE DATABASE` command with the `RECOVER MANAGED STANDBY CANCEL` clause. <br>For example, to stop PDB recovery isolation for the PDB `my_pdb`: 
```
ALTER PLUGGABLE DATABASE my_pdb RECOVER MANAGED STANDBY DATABASE CANCEL;
```


You can also use the `ALTER DATABASE` command if you are connected to the PDB or if the current container is set to the PDB. 




When PDB recovery isolation is stopped, the value is the `RECOVERY_STATUS` column in `V$PDBS` or `V$CONTAINER` is `DISABLED`. 

To manually start PDB recovery isolation:

* Use the `ALTER PLUGGABLE DATABASE` command with the `RECOVER MANAGED STANDBY` clause.

For example, to start PDB recovery isolation for the PDB `my_pdb`:
```
ALTER PLUGGABLE DATABASE my_pdb RECOVER MANAGED STANDBY DATABASE;
```


You can also use the `ALTER DATABASE` command if you are connected to the PDB or if the current container is set to the PDB. 

If you start PDB recovery isolation when media recovery is not running on the standby, the recovery stops at the current standby database checkpoint SCN. You cannot enable media recovery for the standby until the PDB recovery isolation completes.<br>When PDB recovery isolation is stopped, the value of the `RECOVERY_STATUS` column in `V$PDBS` or `V$CONTAINER` is `DISABLED AUTOMATIC RECOVER`. 


