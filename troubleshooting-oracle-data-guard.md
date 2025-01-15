##  ATroubleshooting Oracle Data Guard {#GUID-1AF3825C-C58B-4362-ADD5-A21FEF75912F} 

These are some of the problems that can occur on a standby database, and the troubleshooting procedures to address them. 

  * [ Common Problems ](troubleshooting-oracle-data-guard.md#GUID-07FDFFCC-6FFB-458B-B7A3-F0833EE3BF8B)

  * [ Log File Destination Failures ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944)

  * [ Handling Logical Standby Database Failures ](troubleshooting-oracle-data-guard.md#GUID-3BC98594-CFA0-4EDC-92B9-52F19088B224)

  * [ Problems Switching Over to a Physical Standby Database ](troubleshooting-oracle-data-guard.md#GUID-05C15D55-8CD8-4CB3-ADD3-642AED0FB3A3)

  * [ Problems Switching Over to a Logical Standby Database ](troubleshooting-oracle-data-guard.md#GUID-0D1E5C2A-A8E9-4866-89F4-47AA1D86BB7C)

  * [ What to Do If SQL Apply Stops ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A)

  * [ Network Tuning for Redo Data Transmission ](troubleshooting-oracle-data-guard.md#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7)

  * [ Slow Disk Performance on Standby Databases ](troubleshooting-oracle-data-guard.md#GUID-5F933817-AD03-43D7-878D-E4B115448DAD)

  * [ Avoiding ORA-01555 errors in an Active Data Guard Environment ](unresolvable-reference.md)

  * [ Log Files Must Match to Avoid Primary Database Shutdown ](troubleshooting-oracle-data-guard.md#GUID-82348347-C02B-4179-A325-3192E6F94737)

  * [ Troubleshooting a Logical Standby Database ](troubleshooting-oracle-data-guard.md#GUID-B1551FC6-1B9F-49A5-A462-124697A449C3)




###  Common Problems {#GUID-07FDFFCC-6FFB-458B-B7A3-F0833EE3BF8B} 

These are some of the common problems you may encounter when using a standby database. 

  * [ Renaming Data Files with the ALTER DATABASE Statement ](troubleshooting-oracle-data-guard.md#GUID-AFDF503B-600D-4B7E-A476-2E7E0CB489C0)

  * [ Standby Database Does Not Receive Redo Data from the Primary Database ](troubleshooting-oracle-data-guard.md#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B)

  * [ You Cannot Mount the Physical Standby Database ](troubleshooting-oracle-data-guard.md#GUID-A0AF4052-4B12-496A-AC20-820E184C5A05)




####  Renaming Data Files with the ALTER DATABASE Statement {#GUID-AFDF503B-600D-4B7E-A476-2E7E0CB489C0} 

You cannot rename the data file on the standby site when the ` STANDBY_FILE_MANAGEMENT ` initialization parameter is set to ` AUTO ` . 

When you set the ` STANDBY_FILE_MANAGEMENT ` initialization parameter to ` AUTO ` , use of the following SQL statements is  not allowed: 

  * ` ALTER DATABASE RENAME `

  * ` ALTER DATABASE ADD/DROP LOGFILE `

  * ` ALTER DATABASE ADD/DROP STANDBY LOGFILE MEMBER `

  * ` ALTER DATABASE CREATE DATAFILE AS `




If you attempt to use any of these statements on the standby database, an error is returned. For example: 
    
    
    ```
    SQL> ALTER DATABASE RENAME FILE '/disk1/oracle/oradata/payroll/t_db2.log' to 'dummy';
    
    alter database rename file '/disk1/oracle/oradata/payroll/t_db2.log' to 'dummy' 
    * 
    ERROR at line 1: 
    ORA-01511: error in renaming log/datafiles 
    ORA-01270: RENAME operation is not allowed if STANDBY_FILE_MANAGEMENT is auto
    
    ```

See [ Adding a Data File or Creating a Tablespace ](managing-oracle-data-guard-physical-standby-databases.md#GUID-C60E4DB4-F424-454D-B4DF-7A97B2F09540) to learn how to add data files to a physical standby database. 

####  Standby Database Does Not Receive Redo Data from the Primary Database {#GUID-E0F5B423-208F-4F7E-8CA4-0350D3A7CF0B} 

If the standby site is not receiving redo data, query the ` V$ARCHIVE_DEST ` view and check for error messages. 

For example, enter the following query: 
    
    
    ```
    SQL> SELECT DEST_ID "ID", -
    > STATUS "DB_status", -
    > DESTINATION "Archive_dest", -
    > ERROR "Error" -
    > FROM V$ARCHIVE_DEST WHERE DEST_ID <=5;
    
    ID DB_status Archive_dest                   Error   
    -- --------- ------------------------------ ------------------------------------
     1  VALID    /vobs/oracle/work/arc_dest/arc                          
     2  ERROR    standby1                       ORA-16012: Archivelog standby database identifier mismatch  
     3  INACTIVE                            
     4  INACTIVE                    
     5  INACTIVE                                           
    5 rows selected.
    
    ```

If the output of the query does not help you, then check the following list of possible issues. If any of the following conditions exist, then redo transport services fail to transmit redo data to the standby database: 

  * The service name for the standby instance is not configured correctly in the  ` tnsnames.ora ` file for the primary database. 

  * The Oracle Net service name specified by the ` LOG_ARCHIVE_DEST_ ` *n* parameter for the primary database is incorrect. 

  * The ` LOG_ARCHIVE_DEST_STATE_ ` *n* parameter for the standby database is not set to the value ` ENABLE. `

  * The ` listener.ora ` file has not been configured correctly for the standby database. 

  * The listener is not started at the standby site. 

  * The standby instance is not started. 

  * You have added a standby archiving destination to the primary SPFILE or text initialization parameter file, but have not yet enabled the change. 

  * Redo transport authentication has not been configured properly. See section 3.1.2 for redo transport authentication configuration requirements. 

  * You used an invalid backup as the basis for the standby database (for example, you used a backup from the wrong database, or did not create the standby control file using the correct method). 




####  You Cannot Mount the Physical Standby Database {#GUID-A0AF4052-4B12-496A-AC20-820E184C5A05} 

You cannot mount the standby database if the standby control file was not created with the ` ALTER DATABASE CREATE [LOGICAL] STANDBY CONTROLFILE ... ` statement or RMAN command. 

You cannot use the following types of control file backups: 

  * An operating system-created backup 

  * A backup created using an ` ALTER DATABASE ` statement *without* the ` PHYSICAL STANDBY ` or ` LOGICAL STANDBY ` option 




###  Log File Destination Failures {#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944} 

If you specify ` REOPEN ` for a ` MANDATORY ` destination, redo transport services stall the primary database when redo data cannot be successfully transmitted. 

The ` REOPEN ` attribute is required when you use the ` MAX_FAILURE ` attribute. [ Example A-1 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944__I635145) shows how to set a retry time of 5 seconds and limit retries to 3 times. 

Example A-1 Setting a Retry Time and Limit 
    
    
    ```
    LOG_ARCHIVE_DEST_1='LOCATION=/arc_dest REOPEN=5 MAX_FAILURE=3'
    
    ```

Use the ` ALTERNATE ` attribute of the ` LOG_ARCHIVE_DEST_ ` *n* parameter to specify alternate archive destinations. An alternate archiving destination can be used when the transmission of redo data to a standby database fails. If transmission fails and the ` REOPEN ` attribute was not specified or the ` MAX_FAILURE ` attribute threshold was exceeded, redo transport services attempts to transmit redo data to the alternate destination on the next archival operation. 

Use the ` NOALTERNATE ` attribute to prevent the original archive destination from automatically changing to an alternate archive destination when the original archive destination fails. 

[ Example A-2 ](troubleshooting-oracle-data-guard.md#GUID-397ED7EA-1A1C-4224-82A3-746448BE6944__I635159) shows how to set the  initialization parameters so that a single, mandatory, local destination automatically fails over to a different destination if any error occurs. 

Example A-2 Specifying an Alternate Destination 
    
    
    ```
    LOG_ARCHIVE_DEST_1='LOCATION=/disk1 MANDATORY ALTERNATE=LOG_ARCHIVE_DEST_2'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_2='LOCATION=/disk2 MANDATORY'
    LOG_ARCHIVE_DEST_STATE_2=ALTERNATE
    
    ```

If the ` LOG_ARCHIVE_DEST_1 ` destination fails, the archiving process automatically switches to the ` LOG_ARCHIVE_DEST_2 ` destination at the next log file switch on the primary database. 

###  Handling Logical Standby Database Failures {#GUID-3BC98594-CFA0-4EDC-92B9-52F19088B224} 

An important tool for handling logical standby database failures is the ` DBMS_LOGSTDBY.SKIP_ERROR ` procedure. 

Depending on how important a table is, you might want to do one of the following: 

  * Ignore failures for a table or specific DDL 

  * Associate a stored procedure with a filter so at runtime a determination can be made about skipping the statement, executing this statement, or executing a replacement statement 




Taking one of these actions prevents SQL Apply from stopping. Later, you can query the ` DBA_LOGSTDBY_EVENTS ` view to find and correct any problems that exist. See [ *Oracle Database PL/SQL Packages and Types Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS363) for more information about using the ` DBMS_LOGSTDBY ` package with PL/SQL callout procedures. 

###  Problems Switching Over to a Physical Standby Database {#GUID-05C15D55-8CD8-4CB3-ADD3-642AED0FB3A3} 

These are some of the problems that can cause a switchover to be unsuccessful, and possible solutions. 

  * [ Switchover Fails Because Redo Data Was Not Transmitted ](troubleshooting-oracle-data-guard.md#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C)

  * [ Switchover Fails with the ORA-01102 Error ](troubleshooting-oracle-data-guard.md#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13)

  * [ Redo Data Is Not Applied After Switchover ](troubleshooting-oracle-data-guard.md#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631)

  * [ Roll Back After Unsuccessful Switchover and Start Over ](troubleshooting-oracle-data-guard.md#GUID-97282862-4EA0-42D9-97EF-34E75A29F87F)




####  Switchover Fails Because Redo Data Was Not Transmitted {#GUID-616EF287-25CE-4F6D-A0C3-9A4DEDA3A97C} 

If the switchover does not complete successfully, you can query the ` SEQUENCE# ` column in the ` V$ARCHIVED_LOG ` view to see if the last redo data transmitted from the original primary database was applied on the standby database. 

If the last redo data was not transmitted to the standby database, you can manually copy the archived redo log file containing the redo data from the original primary database to the old standby database and register it with the SQL ` ALTER DATABASE REGISTER LOGFILE ` *`file_specification`* statement. If you then start apply services, the archived redo log file is applied automatically. Query the ` SWITCHOVER_STATUS ` column in the ` V$DATABASE ` view. A switchover to the primary role is now possible if the ` SWITCHOVER_STATUS ` column returns ` TO PRIMARY ` or ` SESSIONS ACTIVE ` . 
    
    
    ```
    SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;
    
    SWITCHOVER_STATUS 
    ----------------- 
    TO PRIMARY 
    1 row selected 
    
    ```

To continue with the switchover, follow the instructions in [ Performing a Switchover to a Physical Standby Database ](managing-oracle-data-guard-role-transitions.md#GUID-AAD70601-D248-4309-B8DD-F461EE31A5FF) for physical standby databases or [ Performing a Switchover to a Logical Standby Database ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B) for logical standby databases, and try again to switch the target standby database to the primary role. 

####  Switchover Fails with the ORA-01102 Error {#GUID-2BE05596-2E74-4FEE-84AB-81777B5B9E13} 

This is an explanation of why you may receive an ` ORA-01102 ` error during a switchover, and what action to take. 

Suppose the standby database and the primary database reside on the same site. After the ` ALTER DATABASE SWITCHOVER TO ` *target_db_name* statement is successfully executed, shut down and restart the physical standby database and the primary database. 

> **note:** 

It is not necessary to shut down and restart the physical standby database if it has not been opened read-only since the instance was started. 

However, the startup of the second database fails with ` ORA-01102 ` error "cannot mount database in ` EXCLUSIVE ` mode." 

This could happen during the switchover if you did not set the ` DB_UNIQUE_NAME ` parameter in the initialization or server parameter file that is used by the standby database (the original primary database). If the ` DB_UNIQUE_NAME ` parameter of the standby database is not set, the standby and the primary databases both use the same mount lock and cause the ` ORA-01102 ` error during the startup of the second database. 

Action: Add ` DB_UNIQUE_NAME= ` *unique_database_name* to the initialization or server parameter file used by the standby database, and shut down and restart the standby and primary databases. 

####  Redo Data Is Not Applied After Switchover {#GUID-ACDE1813-1DBD-48E1-8C42-0A2E4ED36631} 

If the archived redo log files are not applied to the new standby database after the switchover, it could be because some environment or initialization parameters were not properly set after the switchover. 

Action: 

  * Check the ` tnsnames.ora ` file at the new primary site and the ` listener.ora ` file at the new standby site. There should be entries for a listener at the standby site and a corresponding service name at the primary site. 

  * Start the listener at the standby site if it has not been started. 

  * Check if the ` LOG_ARCHIVE_DEST_ ` *n* initialization parameter was set to properly transmit redo data from the primary site to the standby site. For example, query the ` V$ARCHIVE_DEST ` fixed view at the primary site as follows: 
    
        ```
    SQL> SELECT DEST_ID, STATUS, DESTINATION FROM V$ARCHIVE_DEST;
    
    ```

If you do not see an entry corresponding to the standby site, you need to set ` LOG_ARCHIVE_DEST_ ` *n* and ` LOG_ARCHIVE_DEST_STATE_ ` *n* initialization parameters. 

  * Verify that the ` LOG_ARCHIVE_FORMAT ` initialization parameter is set correctly at the standby site. 

  * At the standby site, set the ` DB_FILE_NAME_CONVERT ` and ` LOG_FILE_NAME_CONVERT ` initialization parameters. Set the ` STANDBY_FILE_MANAGEMENT ` initialization parameter to ` AUTO ` to enable the standby site to automatically add new data files that are created at the primary site. 




####  Roll Back After Unsuccessful Switchover and Start Over {#GUID-97282862-4EA0-42D9-97EF-34E75A29F87F} 

For physical standby databases in situations where an error occurred and it is not possible to continue with the switchover, it might still be possible to revert the new physical standby database back to the primary role. 

Take the following steps. 

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
    SQL> ALTER DATABASE SWITCHOVER TO target_db_name [FORCE];
    
    ```

If this statement is successful, then the database runs in the primary database role, and you do not need to perform any more steps. 

If this statement is unsuccessful, then continue with Step 5. 

  5. When the switchover to change the role from primary to physical standby was initiated, a trace file was written in the log directory. This trace file contains the SQL statements required to re-create the original primary control file. Locate the trace file and extract the SQL statements into a temporary file. Execute the temporary file from SQL*Plus. This reverts the new standby database back to the primary role. 
  6. Shut down the original physical standby database. 
  7. Create a new standby control file. This is necessary to resynchronize the primary database and physical standby database. Copy the physical standby control file to the original physical standby system. [ Create a Control File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269) describes how to create a physical standby control file. 
  8. Restart the original physical standby instance. 

If this procedure is successful and archive gap management is enabled, then the FAL processes start and re-archive any missing archived redo log files to the physical standby database. Force a log switch on the primary database and examine the alert logs on both the primary database and physical standby database to ensure the archived redo log file sequence numbers are correct. 

See [ Manual Gap Resolution ](oracle-data-guard-redo-transport-services.md#GUID-97EB433B-4125-4082-9276-2454349BC4F8) for information about archive gap management and [ Setting Archive Tracing ](setting-LOG_ARCHIVE_TRACE-initialization-parameter.md#GUID-515A27B2-5163-42E1-A78D-B6E979BA54EB) for information about locating the trace files. 

  9. Try the switchover again. 

At this point, the Oracle Data Guard configuration has been rolled back to its initial state, and you can try the switchover operation again (after correcting any problems that might have led to the initial unsuccessful switchover). 




###  Problems Switching Over to a Logical Standby Database {#GUID-0D1E5C2A-A8E9-4866-89F4-47AA1D86BB7C} 

A switchover operation involving a logical standby database usually consists of two phases: preparing and committing. The exceptions to this are for rolling upgrades of Oracle software using a logical standby database or if you are using Oracle Data Guard broker. 

If you experience failures in the context of doing a rolling upgrade using a logical standby database or during a switchover operation initiated by Oracle Data Guard broker, then go directly to [ Failures During the Commit Phase of a Switchover Operation ](troubleshooting-oracle-data-guard.md#GUID-A97E4093-B9C0-48E7-BCBE-77DB570A16C2) . 

> **note:** 

Oracle recommends that Flashback Database be enabled for all databases in an Oracle Data Guard configuration. The steps in this section assume that you have Flashback Database enabled on all databases in your Oracle Data Guard configuration. 

####  Failures During the Prepare Phase of a Switchover Operation {#GUID-7B380888-6FEF-4B79-93B9-0D25821E704E} 

If a failure occurs during the preparation phase of a switchover operation, then cancel the switchover and retry the switchover operation from the very beginning. 

#####  Failure While Preparing the Primary Database {#GUID-3A7971C1-3593-4F68-873C-26A2CCA33258} 

If you encounter failure while executing the ` ALTER DATABASE PREPARE TO SWITCHOVER TO LOGICAL STANDBY ` statement, you can cancel the prepare phase of a switchover. 

To do so, issue the following SQL statement at the primary database: 
    
    
    ```
    SQL> ALTER DATABASE PREPARE TO SWITCHOVER TO LOGICAL STANDBY CANCEL;
    
    ```

You can now retry the switchover operation from the beginning. 

#####  Failure While Preparing the Logical Standby Database {#GUID-7A07B541-BD0D-440D-92A3-EC305F3D9821} 

If you encounter failure while executing the ` ALTER DATABASE PREPARE TO SWITCHOVER TO PRIMARY ` statement, you need to cancel the prepare operation at the primary database and at the target standby database. 

Take the following steps: 

  1. At the primary database, cancel the statement you had issued to prepare for the switchover: 
    
        ```
    SQL> ALTER DATABASE PREPARE TO SWITCHOVER TO LOGICAL STANDBY CANCEL;
    
    ```

  2. At the logical standby database that was the target of the switchover, cancel the statement you had issued to prepare to switch over: 
    
        ```
    SQL> ALTER DATABASE PREPARE TO SWITCHOVER TO PRIMARY CANCEL;
    
    ```

You can now retry the switchover operation from the beginning. 




####  Failures During the Commit Phase of a Switchover Operation {#GUID-A97E4093-B9C0-48E7-BCBE-77DB570A16C2} 

Although committing to a switchover involves a single SQL statement, internally a number of operations are performed. 

The corrective actions that you need to take depend on the state of the commit to switchover operation when the error was encountered. 

#####  Failure to Convert the Original Primary Database {#GUID-46D11649-5971-4A47-BA25-6FFAD2D4B83E} 

If you encounter failures while executing the ` ALTER DATABASE COMMIT TO SWITCHOVER TO LOGICAL STANDBY ` statement, there are corrective steps you can take. 

  1. Check the ` DATABASE_ROLE ` column of the ` V$DATABASE ` fixed view on the original primary database: 
    
        ```
    SQL> SELECT DATABASE_ROLE FROM V$DATABASE;
    
    ```

     * If the column contains a value of ` LOGICAL STANDBY ` , the switchover operation has completed, but has failed during a post-switchover task. In this situation, Oracle recommends that you shut down and reopen the database. 

     * If the column contains a value of ` PRIMARY ` , proceed to Step 2. 

  2. Perform the following query on the original primary: 
    
        ```
    SQL> SELECT COUNT(*) FROM SYSTEM.LOGSTDBY$PARAMETERS -
    > WHERE NAME = 'END_PRIMARY';
    
    ```

     * If the query returns a 0, the primary is in a state identical to that it was in before the commit to switchover command was issued. You do not need to take any corrective action. You can proceed with the commit to switchover operation or cancel the switchover operation as outlined in [ Failure While Preparing the Logical Standby Database ](troubleshooting-oracle-data-guard.md#GUID-7A07B541-BD0D-440D-92A3-EC305F3D9821) . 

     * If the query returns a 1, the primary is in an inconsistent state, and you need to proceed to Step 3. 

  3. Take corrective action at the original primary database to maintain its ability to be protected by existing or newly instantiated logical standby databases. 

You can either fix the underlying cause of the error raised during the commit to switchover operation and reissue the SQL statement ( ` ALTER DTABASE COMMIT TO SWITCHOVER TO LOGICAL STANDBY ` ) or you can take the following steps: 

    1. From the alert log of the instance where you initiated the commit to switchover command, determine the SCN needed to flash back to the original primary. This information is displayed after the ` ALTER DATABASE COMMIT TO SWITCHOVER TO LOGICAL STANDBY ` SQL statement: 
        
                ```
        LOGSTDBY: Preparing the COMMIT TO SWITCHOVER TO LOGICAL STANDBY DDL at scn [flashback_scn].
        
        ```

    2. Shut down all instances of the primary database: 
        
                ```
        SQL> SHUTDOWN IMMEDIATE;
        
        ```

    3. Mount the primary database in exclusive mode: 
        
                ```
        SQL> STARTUP MOUNT;
        
        ```

    4. Flash back the database to the SCN taken from the alert log: 
        
                ```
        SQL> FLASHBACK DATABASE TO BEFORE SCN ;
        
        ```

    5. Open the primary database: 
        
                ```
        SQL> STARTUP;
        
        ```

    6. Lower the database guard at the original primary database: 
        
                ```
        SQL> ALTER DATABASE GUARD NONE;
        
        ```

At this point the primary is in a state identical to that it was in before the commit switchover command was issued. You do not need to take any corrective action. you can proceed with the commit to switchover operation or cancel the switchover operation as outlined in [ Failure While Preparing the Primary Database ](troubleshooting-oracle-data-guard.md#GUID-3A7971C1-3593-4F68-873C-26A2CCA33258) . 




#####  Failure to Convert the Target Logical Standby Database {#GUID-8EF3068B-4A91-43A9-8004-5B6418CE9482} 

If you encounter failures while executing the ` ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY ` statement, there are corrective steps you can take. 

  1. Check the ` DATABASE_ROLE ` column of the ` V$DATABASE ` fixed view on the target standby database: 
    
        ```
    SQL> SELECT DATABASE_ROLE FROM V$DATABASE;
    
    ```

     * If the column contains a value ` PRIMARY ` , the switchover operation has completed, but has failed during a post-switchover task. In this situation, you must perform the following steps: 

       1. Shut down and reopen the database. 

       2. Issue an ` ALTER DATABASE GUARD NONE ` command to remove write restrictions to the database. 

     * If the column contains a value of ` LOGICAL STANDBY ` , proceed to Step 2. 

  2. Perform the following query on the target logical standby: 
    
        ```
    SQL> SELECT COUNT(*) FROM SYSTEM.LOGSTDBY$PARAMETERS -
    > WHERE NAME = 'BEGIN_PRIMARY';
    
    ```

     * If the query returns a 0, the logical standby is in a state identical to that it was in before the commit to switchover command was issued. You do not need to take any corrective action. You can proceed with the commit to switchover operations or cancel the switchover operation as outlined in [ Failure While Preparing the Logical Standby Database ](troubleshooting-oracle-data-guard.md#GUID-7A07B541-BD0D-440D-92A3-EC305F3D9821) . 

     * If the query returns a 1, then the logical standby is in an inconsistent state. Proceed to Step 3. 

  3. Take corrective action at the logical standby to maintain its ability to either become the new primary or become a bystander to a different new primary. 

You can either fix the underlying cause of the error raised during the commit to switchover operation and reissue the SQL statement ( ` ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY ` ) or you can take the following steps to flash back the logical standby database to a point of consistency just prior to the commit to switchover attempt: 

    1. From the alert log of the instance where you initiated the commit to switchover command, determine the SCN needed to flash back to the logical standby. This information is displayed after the ` ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY ` SQL statement: 
        
                ```
        LOGSTDBY: Preparing the COMMIT TO SWITCHOVER TO PRIMARY DDL at scn [flashback_scn].
        
        ```

    2. Shut down all instances of the target standby database: 
        
                ```
        SQL> SHUTDOWN IMMEDIATE;
        
        ```

    3. Mount the target logical standby database: 
        
                ```
        SQL> STARTUP MOUNT;
        
        ```

    4. Flash back the target logical standby to the desired SCN: 
        
                ```
        SQL> FLASHBACK DATABASE TO BEFORE SCN ;
        
        ```

    5. Open the database (in case of an Oracle RAC, open all instances); 
        
                ```
        SQL> STARTUP OPEN;
        
        ```




At this point the target standby is in a state identical to that it was in before the commit to switchover command was issued. You do not need to take any further corrective action. You can proceed with the commit to switchover operation. 

###  What to Do If SQL Apply Stops {#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A} 

When an unsupported statement or package is encountered, SQL Apply stops. 

Apply services cannot apply unsupported DML statements, DDL statements, and Oracle supplied packages to a logical standby database running SQL Apply. 

If SQL Apply has stopped because of an unsupported statement or package, you can take the actions described in [ Table A-1 ](troubleshooting-oracle-data-guard.md#GUID-E2C9EEB9-CDCE-46B4-A7ED-D60B7FCD657A__G640603) to correct the situation and start SQL Apply on the logical standby database again. 

**Table A-1 Fixing Typical SQL Apply Errors** 

If...  |  Then...   
---|---  
You suspect an unsupported statement or Oracle supplied package was encountered  |  Find the last statement in the  ` DBA_LOGSTDBY_EVENTS ` view. It shows the statement and error that caused SQL Apply to fail. If an incorrect SQL statement caused SQL Apply to fail, transaction information, as well as the statement and error information, can be viewed. The transaction information can be used with LogMiner tools to understand the cause of the problem.   
An error requiring database management occurred, such as running out of space in a particular tablespace  |  Fix the problem and resume SQL Apply using the  ` ALTER DATABASE START LOGICAL STANDBY APPLY ` statement.   
An error occurred because a SQL statement was entered incorrectly, such as an incorrect standby database filename being entered in a tablespace statement  |  Enter the correct SQL statement and use the  ` DBMS_LOGSTDBY.SKIP_TRANSACTION ` procedure to ensure the incorrect statement is ignored the next time SQL Apply is run. Then, restart SQL Apply using the ` ALTER DATABASE START LOGICAL STANDBY APPLY ` statement.   
An error occurred because skip parameters were incorrectly set up, such as specifying that all DML for a given table be skipped but ` CREATE ` , ` ALTER ` , and ` DROP TABLE ` statements were not specified to be skipped  |  Issue the  ` DBMS_LOGSTDBY.SKIP('TABLE','schema_name','table_name',null) ` procedure, then restart SQL Apply.   
  
See [ Views Relevant to Oracle Data Guard ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A) for information about querying the ` DBA_LOGSTDBY_EVENTS ` view to determine the cause of failures. 

###  Network Tuning for Redo Data Transmission {#GUID-5B879B01-2433-4B08-8D58-E91DBA58FEF7} 

For optimal performance, set the Oracle Net SDU parameter to its maximum value of 65535 bytes in each Oracle Net connect descriptor used by redo transport services. 

The following example shows a database initialization parameter file segment that defines a remote destination ` netserv ` : 
    
    
    ```
    LOG_ARCHIVE_DEST_3='SERVICE=netserv'
    
    ```

The f  ollowing example shows the definition of that service name in the  ` tnsnames.ora ` file: 
    
    
    ```
    netserv=(DESCRIPTION=(SDU=32768)(ADDRESS=(PROTOCOL=tcp)(HOST=host) (PORT=1521)) (CONNECT_DATA=(SERVICE_NAME=srvc)))
    
    ```

The following example shows the definition in the ` listener.ora ` file: 
    
    
    ```
    LISTENER=(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL=tcp)
    (HOST=host)(PORT=1521))))
    
    SID_LIST_LISTENER=(SID_LIST=(SID_DESC=(SDU=32768)(SID_NAME=sid)
    (GLOBALDBNAME=srvc)(ORACLE_HOME=/oracle)))
    
    ```

If you archive to a remote site using a high-latency or high-bandwidth network link, you can improve performance by using the ` SQLNET.SEND_BUF_SIZE ` and ` SQLNET.RECV_BUF_SIZE ` Oracle Net profile parameters to increase the size of the network send and receive I/O buffers. 

See [ *Oracle Database Net Services Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG0141) for information about other ways to change the Oracle NET SDU parameter. 

###  Slow Disk Performance on Standby Databases {#GUID-5F933817-AD03-43D7-878D-E4B115448DAD} 

If asynchronous I/O on the file system itself is showing performance problems, try mounting the file system using the Direct I/O option or setting the ` FILESYSTEMIO_OPTIONS=SETALL ` initialization parameter. 

The maximum I/O size setting is 1 MB. 

###  Log Files Must Match to Avoid Primary Database Shutdown {#GUID-82348347-C02B-4179-A325-3192E6F94737} 

If you have configured a standby redo log on one or more standby databases in the configuration, ensure the size of the standby redo log files on each standby database exactly matches the size of the online redo log files on the primary database. 

At log switch time, if there are no available standby redo log files that match the size of the new current online redo log file on the primary database: 

  * The primary database shuts down if it is operating in maximum protection mode, 

*or* 

  * The RFS process on the standby database creates an archived redo log file on the standby database and writes the following message in the alert log: 
    
        ```
    No standby log files of size <#> blocks available.
    
    ```




For example, if the primary database uses two online redo log groups whose log files are 100K, then the standby database should have 3 standby redo log groups with log file sizes of 100K. 

Also, whenever you add a redo log group to the primary database, you must add a corresponding standby redo log group to the standby database. This reduces the probability of adverse effects on the primary database because a standby redo log file of the required size is not available at log switch time. 

###  Troubleshooting a Logical Standby Database {#GUID-B1551FC6-1B9F-49A5-A462-124697A449C3} 

These troubleshooting tips can help you recover from errors. 

  * [ Recovering from Errors ](troubleshooting-oracle-data-guard.md#GUID-1D92D18C-488C-4896-9592-33FBB4BEEAB2)

  * [ Troubleshooting SQL*Loader Sessions ](troubleshooting-oracle-data-guard.md#GUID-0AB63229-F105-44EF-AB5E-29E19946DC2F)

  * [ Troubleshooting Long-Running Transactions ](troubleshooting-oracle-data-guard.md#GUID-0C23E281-5ED2-4411-851C-B609EA1C350E)

  * [ Troubleshooting ORA-1403 Errors with Flashback Transactions ](troubleshooting-oracle-data-guard.md#GUID-B25EE583-E622-4F0C-AA50-84582DFDD5DD)




####  Recovering from Errors {#GUID-1D92D18C-488C-4896-9592-33FBB4BEEAB2} 

Logical standby databases maintain user tables, sequences, and jobs. To maintain other objects, you must reissue the DDL statements seen in the redo data stream. 

If SQL Apply fails, an error is recorded in the ` DBA_LOGSTDBY_EVENTS ` table. The following sections demonstrate how to recover from two such errors. 

#####  DDL Transactions Containing File Specifications {#GUID-16CE2533-7190-4717-88E0-BDC53DBD887B} 

DDL statements are executed the same way on the primary database and the logical standby database. 

If the underlying file structure is the same on both databases, then the DDL executes on the standby database as expected. 

If an error was caused by a DDL transaction containing a file specification that did not match in the logical standby database environment, perform the following steps to fix the problem: 

  1. Use the ` ALTER SESSION DISABLE ` ` GUARD ` statement to bypass the database guard so you can make modifications to the logical standby database: 
    
        ```
    SQL> ALTER SESSION DISABLE GUARD;
    
    ```

  2. Execute the DDL statement, using the correct file specification, and then reenable the database guard. For example: 
    
        ```
    SQL> ALTER TABLESPACE t_table ADD DATAFILE '/dbs/t_db.f' SIZE 100M REUSE;
    SQL> ALTER SESSION ENABLE GUARD;
    
    ```

  3. Start SQL Apply on the logical standby database and skip the failed transaction. 
    
        ```
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE -
    > SKIP FAILED TRANSACTION;
    
    ```




In some situations, the problem that caused the transaction to fail can be corrected and SQL Apply restarted without skipping the transaction. An example of this might be when available space is exhausted. (Do not let the primary and logical standby databases diverge when skipping DDL transactions. If possible, manually execute a compensating transaction in place of the skipped transaction.) 

The following example shows SQL Apply stopping, the error being corrected, and then restarting SQL Apply: 
    
    
    ```
    SQL> SET LONG 1000
    SQL> ALTER SESSION SET NLS_DATE_FORMAT  = 'DD-MON-YY HH24:MI:SS';
    
    Session altered.
    
    SQL> SELECT EVENT_TIME, COMMIT_SCN, EVENT, STATUS FROM DBA_LOGSTDBY_EVENTS;
    
    EVENT_TIME              COMMIT_SCN
    ------------------ ---------------
    EVENT
    -------------------------------------------------------------------------------
    STATUS
    -------------------------------------------------------------------------------
    22-OCT-03 15:47:58
    
    ORA-16111: log mining and apply setting up
    
    22-OCT-03 15:48:04          209627
    insert into "SCOTT"."EMP"
    values
       "EMPNO" = 7900,
       "ENAME" = 'ADAMS',
       "JOB" = 'CLERK',
       "MGR" IS NULL,
       "HIREDATE" = TO_DATE('22-OCT-03', 'DD-MON-RR'),
       "SAL" = 950,
       "COMM" IS NULL,
       "DEPTNO" IS NULL
    ORA-01653: unable to extend table SCOTT.EMP by %200 bytes in tablespace T_TABLE
    
    ```

In the example, the ` ORA-01653 ` message indicates that the tablespace was full and unable to extend itself. To correct the problem, add a new data file to the tablespace. For example: 
    
    
    ```
    SQL> ALTER TABLESPACE t_table ADD DATAFILE '/dbs/t_db.f' SIZE 60M;
    Tablespace altered.
    
    ```

Then, restart SQL Apply: 
    
    
    ```
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
    Database altered.
    
    ```

When SQL Apply restarts, the transaction that failed is reexecuted and applied to the logical standby database. 

#####  Recovering from DML Failures {#GUID-A146A62B-550F-4520-9001-C11C043E3A64} 

Do not use the ` SKIP_TRANSACTION ` procedure to filter DML failures because it will skip not only the DML seen in the events table, but all the DML associated with the transaction as well. 

DML failures usually indicate a problem with a specific table. For example, assume the failure is an out-of-storage error that you cannot resolve immediately. The following steps demonstrate one way to respond to this problem. 

  1. Bypass the table, but not the transaction, by adding the table to the skip list: 
    
        ```
    SQL> EXECUTE DBMS_LOGSTDBY.SKIP('DML','SCOTT','EMP');
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
    
    ```

From this point on, DML activity for the ` SCOTT.EMP ` table is not applied. After you correct the storage problem, you can fix the table, provided you set up a database link to the primary database that has administrator privileges to run procedures in the ` DBMS_LOGSTDBY ` package. 

  2. Using the database link to the primary database, drop the local ` SCOTT.EMP ` table and then re-create it, and pull the data over to the standby database. 
    
        ```
    SQL> ALTER DATABASE STOP LOGICAL STANDBY APPLY;
    SQL> EXECUTE DBMS_LOGSTDBY.INSTANTIATE_TABLE('SCOTT','EMP','PRIMARYDB');
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
    
    ```

  3. To ensure a consistent view across the newly instantiated table and the rest of the database, wait for SQL Apply to catch up with the primary database before querying this table. Refer to [ Adding or Re-Creating Tables On a Logical Standby Database ](managing-oracle-data-guard-logical-standby-databases.md#GUID-DD88E3D6-D6DB-4175-BB59-F048B9A87BAA) for a detailed example. 



####  Troubleshooting SQL*Loader Sessions {#GUID-0AB63229-F105-44EF-AB5E-29E19946DC2F} 

Oracle SQL*Loader provides a method of loading data from different sources into the Oracle Database. 

This topic analyzes some of the features of the SQL*Loader utility as it pertains to SQL Apply. 

Regardless of the method of data load chosen, the SQL*Loader control files contain an instruction on what to do to the current contents of the Oracle table into which the new data is to be loaded, via the keywords of ` APPEND ` and ` REPLACE ` . The following examples show how to use these keywords on a table named ` LOAD_STOK ` : 

  * When using the ` APPEND keyword ` , the new data to be loaded is appended to the contents of the ` LOAD_STOK ` table: 
    
        ```
    LOAD DATA
    INTO TABLE LOAD_STOK APPEND
    
    ```

  * When using the ` REPLACE ` keyword, the contents of the ` LOAD_STOK ` table are deleted prior to loading new data. Oracle SQL*Loader uses the ` DELETE ` statement to purge the contents of the table, in a single transaction: 
    
        ```
    LOAD DATA
    INTO TABLE LOAD_STOK REPLACE
    
    ```




Rather than using the ` REPLACE ` keyword in the SQL*Loader script, Oracle recommends that prior to loading the data, you issue the SQL*Plus ` TRUNCATE TABLE ` command against the table on the primary database. This has the same effect of purging both the primary and standby databases copy of the table in a manner that is both fast and efficient because the ` TRUNCATE TABLE ` command is recorded in the online redo log files and is issued by SQL Apply on the logical standby database. 

The SQL*Loader script may continue to contain the ` REPLACE ` keyword, but it now attempts to ` DELETE ` zero rows from the object on the primary database. Because no rows were deleted from the primary database, there is no redo recorded in the redo log files. Therefore, no ` DELETE ` statement is issued against the logical standby database. 

Issuing the ` REPLACE ` keyword without the SQL statement ` TRUNCATE TABLE ` provides the following potential problems for SQL Apply when the transaction needs to be applied to the logical standby database. 

  * If the table currently contains a significant number of rows, then these rows need to be deleted from the standby database. Because SQL Apply is not able to determine the original syntax of the statement, SQL Apply must issue a ` DELETE ` statement for each row purged from the primary database. 

For example, if the table on the primary database originally had 10,000 rows, then Oracle SQL*Loader issues a single ` DELETE ` statement to purge the 10,000 rows. On the standby database, SQL Apply does not know that all rows are to be purged, and instead must issue 10,000 individual ` DELETE ` statements, with each statement purging a single row. 

  * If the table on the standby database does not contain an index that can be used by SQL Apply, then the ` DELETE ` statement issues a Full Table Scan to purge the information. 

Continuing with the previous example, because SQL Apply has issued 10,000 individual ` DELETE ` statements, this could result in 10,000 Full Table Scans being issued against the standby database. 




####  Troubleshooting Long-Running Transactions {#GUID-0C23E281-5ED2-4411-851C-B609EA1C350E} 

One of the primary causes for long-running transactions in a SQL Apply environment is full table scans. 

Additionally, long-running transactions could be the result of SQL statements being replicated to the standby database, such as when creating or rebuilding an index. 

**Identifying Long-Running Transactions** 

If SQL Apply is executing a single SQL statement for a long period of time, then a warning message similar to the following is reported in the alert log of the SQL Apply instance: 
    
    
    ```
    Mon Feb 17 14:40:15 2003
    WARNING: the following transaction makes no progress
    WARNING: in the last 30 seconds for the given message!
    WARNING: xid =
    0x0016.007.000017b6 cscn = 1550349, message# = 28, slavid = 1
    knacrb: no offending session found (not ITL pressure)
    
    ```

Note the following about the warning message: 

  * This warning is similar to the warning message returned for interested transaction list (ITL) pressure, with the exception being the last line that begins with ` knacrb ` . The final line indicates: 

    * A Full Table Scan may be occurring 

    * This issue has nothing to do with interested transaction list (ITL) pressure 

  * This warning message is reported only if a single statement takes more than 30 seconds to execute. 




It may not be possible to determine the SQL statement being executed by the long-running statement, but the following SQL statement may help in identifying the database objects on which SQL Apply is operating: 
    
    
    ```
    SQL> SELECT SAS.SERVER_ID -
    >  , SS.OWNER -
    >  , SS.OBJECT_NAME -
    >  , SS.STATISTIC_NAME -
    >  , SS.VALUE -
    >  FROM V$SEGMENT_STATISTICS SS -
    >  , V$LOCK L -
    >  , V$STREAMS_APPLY_SERVER SAS -
    >  WHERE SAS.SERVER_ID = &SLAVE_ID -
    >  AND L.SID = SAS.SID -
    >  AND L.TYPE = 'TM' -
    >  AND SS.OBJ# = L.ID1;
    
    ```

Additionally, you can issue the following SQL statement to identify the SQL statement that has resulted in a large number of disk reads being issued per execution: 
    
    
    ```
    SQL> SELECT SUBSTR(SQL_TEXT,1,40) -
    >  , DISK_READS -
    >  , EXECUTIONS -
    >  , DISK_READS/EXECUTIONS -
    >  , HASH_VALUE -
    >  , ADDRESS -
    >  FROM V$SQLAREA -
    >  WHERE DISK_READS/GREATEST(EXECUTIONS,1) > 1 -
    >  AND ROWNUM < 10 -
    >  ORDER BY DISK_READS/GREATEST(EXECUTIONS,1) DESC;
    
    ```

Oracle recommends that all tables have primary key constraints defined, which automatically means that the column is defined as ` NOT NULL ` . For any table where a primary-key constraint cannot be defined, define an index on an appropriate column that is defined as ` NOT NULL ` . If a suitable column does not exist on the table, then the table should be reviewed and, if possible, skipped by SQL Apply. The following steps describe how to skip all DML statements issued against the ` FTS ` table on the ` SCOTT ` schema: 

  1. Stop SQL Apply: 
    
        ```
    SQL> ALTER DATABASE STOP LOGICAL STANDBY APPLY;
    Database altered
    
    ```

  2. Configure the skip procedure for the ` SCOTT.FTS ` table for all DML transactions: 
    
        ```
    SQL> EXECUTE DBMS_LOGSTDBY.SKIP(stmt => 'DML' , -
    >  schema_name => 'SCOTT' , -
    >  object_name => 'FTS');
    PL/SQL procedure successfully completed
    
    ```

  3. Start SQL Apply: 
    
        ```
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
    Database altered
    ```




**Troubleshooting ITL Pressure** 

Interested transaction list (ITL) pressure is reported in the alert log of the SQL Apply instance. The following shows an example of the warning messages. 
    
    
    ```
    Tue Apr 22 15:50:42 2003
    WARNING: the following transaction makes no progress
    WARNING: in the last 30 seconds for the given message!
    WARNING: xid =
    0x0006.005.000029fa cscn = 2152982, message# = 2, slavid = 17
    ```

**Real-Time Analysis** 

The messages shown in the above output indicate that the SQL Apply process ( ` slavid ` ) #17 has not made any progress in the last 30 seconds. To determine the SQL statement being issued by the Apply process, issue the following query: 
    
    
    ```
    SQL> SELECT SA.SQL_TEXT -
    >  FROM V$SQLAREA SA -
      >  , V$SESSION S -
      >  , V$STREAMS_APPLY_SERVER SAS -
      >  WHERE SAS.SERVER_ID = &SLAVEID -
      >  AND S.SID = SAS.SID -
      >  AND SA.ADDRESS = S.SQL_ADDRESS
    
    SQL_TEXT
    ------------------------------------------------------------
    insert into "APP"."LOAD_TAB_1" p("PK","TEXT")values(:1,:2)
    
    ```

An alternative method to identifying ITL pressure is to query the ` V$LOCK ` view, as shown in the following example. Any session that has a request value of 4 on a ` TX ` lock, is waiting for an ITL to become available. 
    
    
    ```
    SQL> SELECT SID,TYPE,ID1,ID2,LMODE,REQUEST -
    > FROM V$LOCK -
    > WHERE TYPE = 'TX'
    
    SID        TY ID1        ID2        LMODE      REQUEST
    ---------- -- ---------- ---------- ---------- ----------
             8 TX     327688         48          6          0
            10 TX     327688         48          0          4
    
    ```

In this example, ` SID 10 ` is waiting for the ` TX ` lock held by ` SID 8 ` . 

**Post-Incident Review** 

Pressure for a segment's ITL is unlikely to last for an extended period of time. In addition, ITL pressure that lasts for less than 30 seconds is not reported in the standby databases alert log. Therefore, to determine which objects have been subjected to ITL pressure, issue the following statement: 
    
    
    ```
    SQL> SELECT OWNER, OBJECT_NAME, OBJECT_TYPE -
    >  FROM V$SEGMENT_STATISTICS -
    >  WHERE STATISTIC_NAME = 'ITL waits' -
    >  AND VALUE > 0 -
    >  ORDER BY VALUE;
    
    ```

This statement reports all database segments that have had ITL pressure at some time since the instance was last started. 

> **note:** 

This SQL statement is not limited to a logical standby databases in the Oracle Data Guard environment. It is applicable to any Oracle database. 

**Resolving ITL Pressure** 

To increase the ` INITRANS ` integer for a particular database object, it is necessary to first stop SQL Apply. 

> **note:** See Also: 

[ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF52296) for more information about specifying the ` INITRANS ` integer, which is the initial number of concurrent transaction entries allocated within each data block allocated to the database object 

The following example shows the necessary steps to increase the ` INITRANS ` for table ` load_tab_1 ` in the schema ` app ` . 

  1. Stop SQL Apply: 
    
        ```
    SQL> ALTER DATABASE STOP LOGICAL STANDBY APPLY;
    Database altered.
    
    ```

  2. Temporarily bypass the database guard: 
    
        ```
    SQL> ALTER SESSION DISABLE GUARD;
    Session altered.
    
    ```

  3. Increase the ` INITRANS ` on the standby database. For example: 
    
        ```
    SQL> ALTER TABLE APP.LOAD_TAB_1 INITRANS 30;
    Table altered
    
    ```

  4. Reenable the database guard: 
    
        ```
    SQL> ALTER SESSION ENABLE GUARD;
    Session altered
    
    ```

  5. Start SQL Apply: 
    
        ```
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
    Database altered.
    
    ```




Also, consider modifying the database object on the primary database, so that in the event of a switchover, the error does not occur on the new standby database. 

####  Troubleshooting ORA-1403 Errors with Flashback Transactions {#GUID-B25EE583-E622-4F0C-AA50-84582DFDD5DD} 

If SQL Apply returns the ` ORA-1403: No Data Found ` error, then it may be possible to use Flashback Transaction to reconstruct the missing data. 

This strategy relies upon the ` UNDO_RETENTION ` initialization parameter specified on the standby database instance. 

Under normal circumstances, the ` ORA-1403 ` error is not seen in a logical standby database environment. The error occurs when data in a table that is being managed by SQL Apply is modified directly on the standby database and then the same data is modified on the primary database. When the modified data is updated on the primary database and is subsequently received on the logical standby database, SQL Apply verifies the original version of the data is present on the standby database before updating the record. When this verification fails, the ` ORA-1403: No Data Found ` error is returned. 

**The Initial Error** 

When SQL Apply verification fails, the error message is reported in the alert log of the logical standby database and a record is inserted in the ` DBA_LOGSTDBY_EVENTS ` view. The information in the alert log is truncated, while the error is reported in it's entirety in the database view. For example: 
    
    
    ```
    LOGSTDBY stmt: UPDATE "SCOTT"."MASTER"
      SET
        "NAME" = 'john'
      WHERE 
        "PK" = 1 and 
        "NAME" = 'andrew' and 
        ROWID = 'AAAAAAAAEAAAAAPAAA'
    LOGSTDBY status: ORA-01403: no data found
    LOGSTDBY PID 1006, oracle@staco03 (P004)
    LOGSTDBY XID 0x0006.00e.00000417, Thread 1, RBA 0x02dd.00002221.10
    ```

**The Investigation** 

The first step is to analyze the historical data of the table that caused the error. This can be achieved using the ` VERSIONS ` clause of the ` SELECT ` statement. For example, you can issue the following query on the primary database: 
    
    
    ```
    SELECT VERSIONS_XID
          , VERSIONS_STARTSCN
          , VERSIONS_ENDSCN
          , VERSIONS_OPERATION
          , PK
          , NAME
       FROM SCOTT.MASTER
            VERSIONS BETWEEN SCN MINVALUE AND MAXVALUE
      WHERE PK = 1
      ORDER BY NVL(VERSIONS_STARTSCN,0);
    
    VERSIONS_XID     VERSIONS_STARTSCN VERSIONS_ENDSCN V  PK NAME
    ---------------- ----------------- --------------- - --- -------
    03001900EE070000           3492279         3492290 I   1 andrew
    02000D00E4070000           3492290                 D   1 andrew
    
    ```

Depending upon the amount of undo retention that the database is configured to retain ( ` UNDO_RETENTION ` ) and the activity on the table, the information returned might be extensive and you may need to change the versions between syntax to restrict the amount of information returned. From the information returned, you can see that the record was first inserted at SCN 3492279 and then was deleted at SCN 3492290 as part of transaction ID 02000D00E4070000. Using the transaction ID, query the database to find the scope of the transaction. This is achieved by querying the ` FLASHBACK_TRANSACTION_QUERY ` view. 

> **note:** Please note that the ` UNDO_RETENTION ` parameter is not inheritable in a CDB database. In addition, setting the ` UNDO_RETENTION ` parameter for Active Data Guard does not have any effect since the physical standby server does not run any DMLs. To accommodate the queries on Active Data Guard, specify the minimum retention on the primary database instance using the ` UNDO_RETENTION ` parameter and monitor ` V$UNDOSTAT ` ’s ` TUNED_UNDORETENTION ` on the primary server. 
    
    
    ```
    SELECT OPERATION
         , UNDO_SQL
      FROM FLASHBACK_TRANSACTION_QUERY
     WHERE XID = HEXTORAW('02000D00E4070000');
    
    OPERATION  UNDO_SQL
    ---------- ------------------------------------------------
    DELETE     insert into "SCOTT"."MASTER"("PK","NAME") values
               ('1','andrew');
    BEGIN
    
    ```

There is always one row returned representing the start of the transaction. In this transaction, only one row was deleted in the master table. The ` UNDO_SQL ` column, when executed, restores the original data into the table. 
    
    
    ```
    SQL> INSERT INTO "SCOTT"."MASTER"("PK","NAME") VALUES ('1','ANDREW');SQL> COMMIT;
    
    ```

When you restart SQL Apply, the transaction is applied to the standby database: 
    
    
    ```
    SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
    ```
