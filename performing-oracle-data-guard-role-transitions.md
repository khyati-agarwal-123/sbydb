##  GPerforming Role Transitions Using Old Syntax {#GUID-6E78412B-8E55-4D8B-A318-9F0612376E86} 

Prior to Oracle Database 12 *c* Release 1 (12.1), the procedures for performing switchovers and failovers to a physical standby database were different. 

These procedures are still supported, but Oracle recommends you use the new procedures described in  " [ Role Transitions Involving Physical Standby Databases ](managing-oracle-data-guard-role-transitions.md#GUID-857F6F45-DC1C-4345-BD39-F3BE7D79F1CD) "  . 

If you are using a release prior to Oracle Database 12 *c* Release 1 (12.1), then you must use the old procedures. 

The following topics are discussed: 

  * [ SQL Syntax for Role Transitions Involving Physical Standbys ](performing-oracle-data-guard-role-transitions.md#GUID-EEBC6DA6-E192-470E-8FC9-F507B004406E)

  * [ Role Transitions Involving Physical Standby Databases ](performing-oracle-data-guard-role-transitions.md#GUID-F4A18A3A-6308-4580-B1F3-9647E2669BA0)

  * [ Troubleshooting Switchovers to Physical Standby Databases ](performing-oracle-data-guard-role-transitions.md#GUID-E80C5A58-6D05-4281-81DB-6268EE1757AC)




###  SQL Syntax for Role Transitions Involving Physical Standbys {#GUID-EEBC6DA6-E192-470E-8FC9-F507B004406E} 

Oracle Database 12 *c* Release 1 (12.1) introduces new SQL syntax for performing switchover and failover operations to a physical standby database. 

Oracle Database 12 *c* Release 1 (12.1) introduces new SQL syntax for performing switchover and failover operations to a physical standby database. Do not mix syntax from the old procedures (described in this topic) and the new procedures (described in [ Role Transitions ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE) ), unless you are specifically directed to do so. 

Pre-12c Role Transition Syntax for Physical Standby Databases  |  12c Role Transition Syntax for Physical Standby Databases   
---|---  
To switchover to a physical standby database, on the primary database:  ` SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PHYSICAL STANDBY; ` On the physical standby database:  ` SQL>ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY; ` |  To switchover to a physical standby database:  ` SQL> ALTER DATABASE SWITCHOVER TO ` *target_db_name* ` [FORCE] [VERIFY] ` ;   
To failover to a physical standby database, (step 6 and step 8 in  " [ Performing a Failover to a Physical Standby Database Using Old Syntax ](performing-oracle-data-guard-role-transitions.md#GUID-AAAA9A3B-7A1A-4044-90EC-F581D87439F4) "  ):  ` SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH ` ;  and  ` SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY ` ;  |  To failover to a physical standby database, the following statement replaces the two statements previously required:  ` SQL> ALTER DATABASE FAILOVER TO ` *target_db_name* ;   
  
> **note:** See Also: 

  * [ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF00802) for more information about SQL syntax 




####  New Features When Using the Old Syntax {#GUID-C1E83316-FD7F-4944-8CC8-784A37172B95} 

As of Oracle Database 12 *c* Release 1 (12.1), the ` WITH SESSION SHUTDOWN ` clause is no longer needed to terminate active SQL sessions. 

You can issue the following statement to automatically terminate active SQL sessions: 
    
    
    ```
    SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PHYSICAL STANDBY;
    
    ```

Additionally, when you perform a switchover from an Oracle RAC primary database to a physical standby database, it is no longer necessary to shut down all but one primary database instance. All the instances are shut down automatically after the switchover is complete. 

###  Role Transitions Involving Physical Standby Databases {#GUID-F4A18A3A-6308-4580-B1F3-9647E2669BA0} 

The SQL syntax to perform switchovers and failovers to a physical standby database was different in releases prior to Oracle Database 12 *c* Release 1 (12.1). 

The following are the procedures that must be used if you are running a release prior to 12.1: 

  * [ Performing a Switchover to a Physical Standby Database Using Old Syntax ](performing-oracle-data-guard-role-transitions.md#GUID-65C1BE5E-A2AC-4938-B03F-2495761DE500)

  * [ Performing a Failover to a Physical Standby Database Using Old Syntax ](performing-oracle-data-guard-role-transitions.md#GUID-AAAA9A3B-7A1A-4044-90EC-F581D87439F4)




> **note:** See Also: 

[ Role Transitions ](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE) for information about how to prepare for switchovers and failovers 

####  Performing a Switchover to a Physical Standby Database Using Old Syntax {#GUID-65C1BE5E-A2AC-4938-B03F-2495761DE500} 

A switchover is initiated on the primary database and is completed on the target standby database. 

This topic describes how to perform a switchover to a physical standby database. 

  1. Verify that the primary database can be switched to the standby role. 

Query the ` SWITCHOVER_STATUS ` column of the ` V$DATABASE ` view on the primary database. For example: 
    
        ```
    SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
    
    SWITCHOVER_STATUS 
     ----------------- 
     TO STANDBY 
     1 row selected 
    
    ```

A value of ` TO STANDBY ` or ` SESSIONS ACTIVE ` indicates that the primary database can be switched to the standby role. If neither of these values is returned, a switchover is not possible because redo transport is either misconfigured or is not functioning properly. See [ Redo Transport Services ](oracle-data-guard-redo-transport-services.md#GUID-0EC71C7D-A80B-40AA-93E5-28BB7E24ED31) for information about configuring and monitoring redo transport. 

  2. Issue the following SQL statement on the primary database to switch it to the standby role: Issue the following SQL statement on the primary database to switch it to the standby role: 
    
        ```
    SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PHYSICAL STANDBY;
    
    ```

This statement converts the primary database into a physical standby database. The current control file is backed up to the current SQL session trace file before the switchover. This makes it possible to reconstruct a current control file, if necessary. 

  3. Mount the former primary database. For example: For example: 
    
        ```
    SQL> STARTUP MOUNT;
    
    ```

At this point in the switchover process, the original primary database is a physical standby database. 

  4. Query the ` SWITCHOVER_STATUS ` column of the ` V$DATABASE ` view on the standby database to verify that the switchover target is ready to be switched to the primary role. For example: 
    
        ```
    SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
    
    SWITCHOVER_STATUS 
    ----------------- 
    TO PRIMARY 
    1 row selected
    
    ```

A value of ` TO PRIMARY ` or ` SESSIONS ACTIVE ` indicates that the standby database is ready to be switched to the primary role. If neither of these values is returned, verify that Redo Apply is active and that redo transport is configured and working properly. Continue to query this column until the value returned is either ` TO PRIMARY ` or ` SESSIONS ACTIVE ` . 

  5. Issue the following SQL statement on the target physical standby database to switch it to the primary role: 
    
        ```
    SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY WITH SESSION SHUTDOWN;
    ```

> **note:** 

The ` WITH SESSION SHUTDOWN ` clause can be omitted from the switchover statement if the query performed in step 4 returned a value of ` TO PRIMARY ` . 

  6. Open the new primary database: 
    
        ```
    SQL> ALTER DATABASE OPEN;
    ```

  7. Start Redo Apply on the new physical standby database: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE -
    > DISCONNECT FROM SESSION;
    ```

  8. Restart Redo Apply if it has stopped at any of the other physical standby databases in your Oracle Data Guard configuration: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE -
    > DISCONNECT FROM SESSION;
    ```




####  Performing a Failover to a Physical Standby Database Using Old Syntax {#GUID-AAAA9A3B-7A1A-4044-90EC-F581D87439F4} 

These steps describe how to perform a failover to a physical standby database. 

  1. If possible, mount the primary database and flush any unsent archived and current redo from the primary database to the standby database. If this operation is successful, a zero data loss failover is possible even if the primary database is not in a zero data loss data protection mode. 

Ensure that Redo Apply is active at the target standby database. 

Mount, but do not open, the primary database. If the primary database cannot be mounted, go to step 2. 

Issue the following SQL statement at the primary database: 
    
        ```
    SQL> ALTER SYSTEM FLUSH REDO TO target_db_name;
    
    ```

For *target_db_name* , specify the ` DB_UNIQUE_NAME ` of the standby database that is to receive the redo flushed from the primary database. 

This statement flushes any unsent redo from the primary database to the standby database, and waits for that redo to be applied to the standby database. 

If this statement completes without any errors, go to step 5. If the statement completes with any errors, or if it must be stopped because you cannot wait any longer for the statement to complete, continue with step 2. 

  2. Verify that the standby database has the most recently archived redo log file for each primary database redo thread. 

To do this, query the ` V$ARCHIVED_LOG ` view on the target standby database to obtain the highest log sequence number for each redo thread. 

For example: 
    
        ```
    SQL> SELECT UNIQUE THREAD# AS THREAD, MAX(SEQUENCE#) -
    > OVER (PARTITION BY thread#) AS LAST from V$ARCHIVED_LOG;
    
        THREAD       LAST
    ---------- ----------
             1        100
    
    ```

If possible, copy the most recently archived redo log file for each primary database redo thread to the standby database if it does not exist there, and register it. This must be done for each redo thread. 

For example: 
    
        ```
    SQL> ALTER DATABASE REGISTER PHYSICAL LOGFILE 'filespec1';
    ```

  3. Query the ` V$ARCHIVE_GAP ` view on the target standby database to determine if there are any redo gaps on the target standby database. 

For example: 
    
        ```
    SQL> SELECT THREAD#, LOW_SEQUENCE#, HIGH_SEQUENCE# FROM V$ARCHIVE_GAP;
    
    THREAD#    LOW_SEQUENCE# HIGH_SEQUENCE#
    ---------- ------------- --------------
             1            90             92
    
    ```

In this example, the gap comprises archived redo log files with sequence numbers 90, 91, and 92 for thread 1. 

If possible, copy any missing archived redo log files to the target standby database from the primary database and register them at the target standby database. This must be done for each redo thread. 

For example: 
    
        ```
    SQL> ALTER DATABASE REGISTER PHYSICAL LOGFILE 'filespec1';
    ```

  4. Repeat step 3 until all gaps are resolved. (The query executed in step 3 displays information for the highest gap only, so after resolving a gap, you must repeat the query until no more rows are returned.) 

If, after performing steps 2 through step 4, you are not able to resolve all gaps in the archived redo log files (for example, because you do not have access to the system that hosted the failed primary database), then some data loss will occur during the failover. 

  5. Issue the following SQL statement on the target standby database to stop Redo Apply: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
    ```

  6. Finish applying all received redo data: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    
    ```

If this statement completes without any errors, then proceed to step 7. 

If an error occurs, some received redo data was not applied. Try to resolve the cause of the error and reissue the statement before proceeding to the next step. 

If there is a redo gap that was not resolved in step 3 and step 4, then you receive an error stating that there is a redo gap. 

If the error condition cannot be resolved, a failover can still be performed (with some data loss) by issuing the following SQL statement on the target standby database: 
    
        ```
    SQL> ALTER DATABASE ACTIVATE PHYSICAL STANDBY DATABASE;
    
    ```

Proceed to step 9 when the ` ACTIVATE ` statement completes. 

  7. Verify that the target standby database is ready to become a primary database. 

To do this, query the ` SWITCHOVER_STATUS ` column of the ` V$DATABASE ` view on the target standby database. For example: 
    
        ```
    SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
    
    SWITCHOVER_STATUS
    -----------------
    TO PRIMARY
    1 row selected
    
    ```

A value of either ` TO PRIMARY ` or ` SESSIONS ACTIVE ` indicates that the standby database is ready to be switched to the primary role. If neither of these values is returned, verify that Redo Apply is active and continue to query this view until either ` TO PRIMARY ` or ` SESSIONS ACTIVE ` is returned. 

  8. Issue the following SQL statement on the target standby database to switch the physical standby to the primary role: 
    
        ```
    SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY WITH SESSION SHUTDOWN;
    ```

> **note:** 

The ` WITH SESSION SHUTDOWN ` clause can be omitted from the switchover statement if the query of the ` SWITCHOVER_STATUS ` column performed in the previous step returned a value of ` TO PRIMARY ` . 

  9. Open the new primary database: 
    
        ```
    SQL> ALTER DATABASE OPEN;
    ```

  10. Oracle recommends that you now back up the new primary database. 
  11. Restart Redo Apply if it has stopped at any of the other physical standby databases in your Oracle Data Guard configuration: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE -
    > DISCONNECT FROM SESSION;
    ```

  12. Optionally, restore the failed primary database. 

After a failover, the original primary database can be converted into a physical standby database of the new primary database using the method described in [ Converting a Failed Primary Into a Standby Database Using Flashback Database ](examples-of-using-oracle-data-guard.md#GUID-1163448F-6B18-4A44-AA8D-7CDF0D1360FB) or [ Converting a Failed Primary into a Standby Database Using RMAN Backups ](examples-of-using-oracle-data-guard.md#GUID-B734C79F-A2BB-43A5-89D7-7733089FB577) , or it can be re-created as a physical standby database from a backup of the new primary database using the method described in [ Step-by-Step Instructions for Creating a Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-A86D403B-B3F5-4B05-9255-5BADD513F9CF) . 

Once the original primary database is running in the standby role, a switchover can be performed to restore it to the primary role. 




###  Troubleshooting Switchovers to Physical Standby Databases {#GUID-E80C5A58-6D05-4281-81DB-6268EE1757AC} 

These are some of the problems that can occur during a switchover to a physical standby database. 

> **note:** 

The following troubleshooting topics apply only when you are performing switchovers and failovers to a physical standby database using procedures available in releases prior to Oracle Database 12c Release 1 (12.1). 

  * [ Switchover Fails Because Redo Data Was Not Transmitted ](performing-oracle-data-guard-role-transitions.md#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4)

  * [ Switchover Fails with the ORA-01102 Error ](performing-oracle-data-guard-role-transitions.md#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB)

  * [ Redo Data Is Not Applied After Switchover ](performing-oracle-data-guard-role-transitions.md#GUID-F394098F-8726-45AC-A66F-B91402719D6A)

  * [ Roll Back After Unsuccessful Switchover and Start Over ](performing-oracle-data-guard-role-transitions.md#GUID-2CF7243A-24DA-42CB-86C7-D48E648D9F5D)




####  Switchover Fails Because Redo Data Was Not Transmitted {#GUID-9D024B1F-54EF-4C95-A810-1FA09B7AE3E4} 

If a switchover does not complete successfully, you can query the ` SEQUENCE# ` column in the ` V$ARCHIVED_LOG ` view to see if the last redo data transmitted from the original primary database was applied on the standby database. 

If the last redo data was not transmitted to the standby database, you can manually copy the archived redo log file containing the redo data from the original primary database to the old standby database and register it with the SQL ` ALTER DATABASE REGISTER LOGFILE ` *`file_specification`* statement. If you then start apply services, the archived redo log file is applied automatically. Query the ` SWITCHOVER_STATUS ` column in the ` V$DATABASE ` view. A switchover to the primary role is now possible if the ` SWITCHOVER_STATUS ` column returns ` TO PRIMARY ` or ` SESSIONS ACTIVE ` : 
    
    
    ```
    SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
    
    SWITCHOVER_STATUS 
    ----------------- 
    TO PRIMARY 
    1 row selected 
    
    ```

See [ Views Relevant to Oracle Data Guard ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A) for information about other valid values for the ` SWITCHO ` ` VER_STATUS ` column of the ` V$DATABASE ` view. 

To continue with the switchover, follow the instructions in [ Performing a Switchover to a Physical Standby Database Using Old Syntax ](performing-oracle-data-guard-role-transitions.md#GUID-65C1BE5E-A2AC-4938-B03F-2495761DE500) and try again to switch the target standby database to the primary role. 

####  Switchover Fails with the ORA-01102 Error {#GUID-A9CF13FE-3E89-4FB3-B2BF-6536DCAD3ADB} 

These are some of the possible causes, and solutions, if you receive an ORA-01102 error. 

Suppose the standby database and the primary database reside on the same site. After both the ` ALTER DATABASE COMMIT TO SWITCHOVER TO PHYSICAL ` ` STANDBY ` and the ` ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY ` statements are successfully executed, shut down and restart the physical standby database and the primary database. 

> **note:** 

It is not necessary to shut down and restart the physical standby database if it has not been opened read-only since the instance was started. 

However, the startup of the second database fails with an ` ORA-01102 cannot mount database in EXCLUSIVE mode ` error. 

This could happen during the switchover if you did not set the ` DB_UNIQUE_NAME ` parameter in the initialization parameter file that is used by the standby database (the original primary database). If the ` DB_UNIQUE_NAME ` parameter of the standby database is not set, the standby and the primary databases both use the same mount lock and cause the ORA-01102 error during the startup of the second database. 

Action: Add ` DB_UNIQUE_NAME= ` *unique_database_name* to the initialization parameter file used by the standby database, and shut down and restart the standby and primary databases. 

####  Redo Data Is Not Applied After Switchover {#GUID-F394098F-8726-45AC-A66F-B91402719D6A} 

If archived redo log files are not applied to the new standby database after the switchover, it may be because some environment or initialization parameters were not properly set after the switchover. 

Action: 

  * Check the  ` tnsnames.ora ` file at the new primary site and the ` listener.ora ` file at the new standby site. There should be entries for a listener at the standby site and a corresponding service name at the primary site. 

  * Start the listener at the standby site if it has not been started. 

  * Check if the ` LOG_ARCHIVE_DEST_ ` *n* initialization parameter was set to properly transmit redo data from the primary site to the standby site. For example, query the ` V$ARCHIVE_DEST ` fixed view at the primary site as follows: 
    
        ```
    SQL> SELECT DEST_ID, STATUS, DESTINATION FROM V$ARCHIVE_DEST;
    
    ```

If you do not see an entry corresponding to the standby site, you need to set ` LOG_ARCHIVE_DEST_ ` *n* and ` LOG_ARCHIVE_DEST_STATE_ ` *n* initialization parameters. 

  * Verify that the ` LOG_ARCHIVE_FORMAT ` initialization parameter is set correctly at the standby site. 

  * At the standby site, set the  ` DB_FILE_NAME_CONVERT ` and  ` LOG_FILE_NAME_CONVERT ` initialization parameters. Set the ` STANDBY_FILE_MANAGEMENT ` initialization parameter to ` AUTO ` to enable the standby site to automatically add new data files that are created at the primary site. 




####  Roll Back After Unsuccessful Switchover and Start Over {#GUID-2CF7243A-24DA-42CB-86C7-D48E648D9F5D} 

For physical standby databases in situations where an error occurred and it is not possible to continue with the switchover, it might still be possible to revert the new physical standby database back to the primary role. 

Take the following steps: 

  1. Shut down and mount the new standby database (old primary). 
  2. Start Redo Apply on the new standby database. 
  3. Verify that the new standby database is ready to be switched back to the primary role. Query the ` SWITCHOVER_STATUS ` column of the ` V$DATABASE ` view on the new standby database. For example: 
    
        ```
    SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
     
    SWITCHOVER_STATUS 
    ----------------- 
    TO_PRIMARY 
    1 row selected
    
    ```

A value of ` TO PRIMARY ` or ` SESSIONS ACTIVE ` indicates that the new standby database is ready to be switched to the primary role. Continue to query this column until the value returned is either ` TO PRIMARY ` or ` SESSIONS ACTIVE ` . 

  4. Issue the following statement to convert the new standby database back to the primary role: 
    
        ```
    SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY WITH SESSION SHUTDOWN;
    
    ```

If this statement is successful, the database runs in the primary database role, and you do not need to perform any more steps. 

If this statement is unsuccessful, then continue with Step 5. 

  5. When the switchover to change the role from primary to physical standby was initiated, a trace file was written in the log directory. This trace file contains the SQL statements required to re-create the original primary control file. Locate the trace file and extract the SQL statements into a temporary file. Execute the temporary file from SQL*Plus. This reverts the new standby database back to the primary role. 
  6. Shut down the original physical standby database. 
  7. Create a new standby control file. This is necessary to resynchronize the primary database and physical standby database. Copy the physical standby control file to the original physical standby system. [ Create a Control File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269) describes how to create a physical standby control file. 
  8. Restart the original physical standby instance. 

If this procedure is successful and archive gap management is enabled, then the FAL processes start and re-archive any missing archived redo log files to the physical standby database. Force a log switch on the primary database and examine the alert logs on both the primary database and physical standby database to ensure the archived redo log file sequence numbers are correct. 

See [ Manual Gap Resolution ](oracle-data-guard-redo-transport-services.md#GUID-97EB433B-4125-4082-9276-2454349BC4F8) for information about archive gap management and [ Setting Archive Tracing ](setting-LOG_ARCHIVE_TRACE-initialization-parameter.md#GUID-515A27B2-5163-42E1-A78D-B6E979BA54EB) for information about locating the trace files. 

  9. Try the switchover again. 

At this point, the Oracle Data Guard configuration has been rolled back to its initial state and you can try the switchover operation again (after correcting any problems that might have led to the initial unsuccessful switchover). 



