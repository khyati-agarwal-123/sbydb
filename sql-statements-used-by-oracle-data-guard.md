##  18SQL Statements Relevant to Oracle Data Guard {#GUID-A3B54628-8FDB-4E99-A933-9EDD59AAC2DF} 

There are many SQL and SQL*Plus statements that are useful for performing operations on standby databases in an Oracle Data Guard environment. 

See the following topics: 

  * [ ALTER DATABASE Statements ](sql-statements-used-by-oracle-data-guard.md#GUID-F690B602-6582-465B-8E86-0B0A52838D66)

  * [ ALTER SESSION Statements ](sql-statements-used-by-oracle-data-guard.md#GUID-0C30DE80-F67E-4B27-9ABE-F6FA51358739)

  * [ ALTER SYSTEM Statements ](sql-statements-used-by-oracle-data-guard.md#GUID-7CFC5C95-68EF-49E7-A95E-4926EFA2C934)




Refer to the [ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF008) for complete syntax and descriptions of all SQL statements. 

See [ Initialization Parameters ](oracle-initialization-parameters-used-by-oracle-data-guard.md#GUID-A1CF9B00-F0F5-4F3B-A0E0-9D6ABF775EC9) for a list of initialization parameters that you can set and dynamically update using the ` ALTER SYSTEM SET ` statement. 

###  ALTER DATABASE Statements {#GUID-F690B602-6582-465B-8E86-0B0A52838D66} 

This table describes ` ALTER DATABASE ` statements that are relevant to Oracle Data Guard. 

**Table: ALTER DATABASE Statements Used in Data Guard Environments** 

ALTER DATABASE Statement  |  Description   
---|---  
` ACTIVATE [PHYSICAL|LOGICAL] STANDBY DATABASE FINISH APPLY] ` |  Performs a failover. The standby database must be mounted before it can be activated with this statement.  Note:  Do not use the ` ALTER DATABASE ACTIVATE STANDBY DATABASE ` statement to failover because it causes data loss. Instead, use the following best practices:  <ul>

<li>
  * For physical standby databases, use the  ` ALTER DATABASE RECOVER MANAGED STANDBY DATABASE ` statement with the ` FINISH ` keyword to perform the role transition as quickly as possible with little or no data loss and without rendering other standby databases unusable.  </li> <li>
  * For logical standby databases, use the ` ALTER DATABASE PREPARE TO SWITCHOVER ` and ` ALTER DATABASE COMMIT TO SWITCHOVER ` statements.  </li> </ul>  
` ADD ` ` [STANDBY ` ] ` LOGFILE [THREAD *`integer`*  ] [GROUP *`integer`*  ] ` *filespec*  |  Adds one or more online redo log file groups or standby redo log file groups to the specified thread, making the log files available to the instance to which the thread is assigned.  See [ Add or Drop a Redo Log File Group ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE) for an example of this statement.   
` ADD [STANDBY ` ] ` LOGFILE ` ` MEMBER ` ' *`filename`* ' ` [REUSE] ` ` TO ` *`logfile-descriptor`*  |  Adds new members to existing online redo log file groups or standby redo log file groups.   
` [ADD ` | ` DROP ` ] ` SUPPLEMENTAL ` ` LOG ` ` DATA {PRIMARY KEY\|UNIQUE INDEX} ` ` COLUMNS ` |  This statement is for logical standby databases only.  Use it to enable full supplemental logging before you create a logical standby database. This is necessary because supplemental logging is the source of change to a logical standby database. To implement full supplemental logging, you must specify either the ` PRIMARY KEY COLUMNS ` or the ` UNIQUE INDEX COLUMNS ` keyword on this statement.   
` COMMIT TO SWITCHOVER ` |  Performs a switchover to:  <ul> <li>
    * Change the current primary database to the standby database role  </li> <li>
    * Change one standby database to the primary database role.  </li> </ul> When switching over to a physical standby database, as of Oracle Database 12 *c* Release 1 (12.1), the ` COMMIT TO SWITCHOVER ` statement has been replaced with the ` SWITCHOVER TO ` statement. The ` COMMIT TO SWITCHOVER ` statement is still supported, but Oracle recommends that you use the new ` SWITCHOVER TO ` statement.  Note:  On logical standby databases, you issue the ` ALTER DATABASE PREPARE TO SWITCHOVER ` statement to prepare the database for the switchover before you issue the ` ALTER DATABASE COMMIT TO SWITCHOVER ` statement.  See [ Performing a Switchover to a Physical Standby Database Using Old Syntax ](performing-oracle-data-guard-role-transitions.md#GUID-65C1BE5E-A2AC-4938-B03F-2495761DE500) and [ Performing a Failover to a Physical Standby Database Using Old Syntax ](performing-oracle-data-guard-role-transitions.md#GUID-AAAA9A3B-7A1A-4044-90EC-F581D87439F4) for examples of this statement.   
` CONVERT TO [[PHYSICAL|SNAPSHOT] STANDBY] DATABASE ` |  Converts a physical standby database into a snapshot standby database and vice versa.   
` CREATE [PHYSICAL|LOGICAL] STANDBY CONTROLFILE AS ' *`filename`*  ' [REUSE] ` |  Creates a control file to be used to maintain a physical or a logical standby database. Issue this statement on the primary database.  See [ Create a Control File for the Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-B471D788-AFD3-48B3-9709-D63C34DBC269) for an example of this statement.   
` DROP [STANDBY] LOGFILE ` *`logfile_descriptor`*  |  Drops all members of an online redo log file group or standby redo log file group.  See [ Add or Drop a Redo Log File Group ](managing-oracle-data-guard-physical-standby-databases.md#GUID-64E46731-490D-4228-AC87-A07CDE3C75FE) for an example of this statement.   
` DROP [STANDBY] LOGFILE MEMBER ` *`'filename'`*  |  Drops one or more online redo log file members or standby redo log file members.   
` FAILOVER TO ` *target_db_name*  |  This statement is for physical standby databases only.  It initiates a failover to the specified host database.   
` [NO]FORCE LOGGING ` |  Controls whether or not the Oracle database logs all changes in the database except for changes to temporary tablespaces and temporary segments. The ` [NO]FORCE LOGGING ` clause is required to prevent inconsistent standby databases.  The primary database must at least be mounted (and it can also be open) when you issue this statement. See [ Enable Forced Logging ](creating-oracle-data-guard-physical-standby.md#GUID-AE2B1237-57A1-4745-A04C-4246A831A963) for an example of this statement.   
` GUARD ` |  Controls user access to tables in a logical standby database. Possible values are ` ALL ` , ` STANDBY ` , and ` NONE ` . See [ Controlling User Access to Tables in a Logical Standby Database ](managing-oracle-data-guard-logical-standby-databases.md#GUID-572B9755-69F5-4BD7-9DFF-1F7EB1DE3771) for more information.   
` MOUNT ` [ ` STANDBY DATABASE ` ]  |  Mounts a standby database, allowing the standby instance to receive redo data from the primary instance.   
` OPEN ` |  Opens a previously started and mounted database:  <ul> <li>
      * Physical standby databases are opened in read-only mode, restricting users to read-only transactions and preventing the generating of redo data.  </li> <li>
      * Logical standby database are opened in read/write mode.  </li> </ul>  
` PREPARE ` ` TO ` ` SWITCHOVER ` |  This statement is for logical standby databases only.  It prepares the primary database and the logical standby database for a switchover by building the LogMiner dictionary *before* the switchover takes place. After the dictionary build has completed, issue the ` ALTER DATABASE COMMIT TO SWITCHOVER ` statement to switch the roles of the primary and logical standby databases.  See [ Performing a Switchover to a Logical Standby Database ](managing-oracle-data-guard-role-transitions.md#GUID-42B8B448-90E0-4D7D-82A3-AAA273E6E49B) for examples of this statement.   
` RECOVER MANAGED STANDBY DATABASE [ { DISCONNECT [FROM SESSION] \| PARALLEL n\| NODELAY \| UNTIL CHANGE integer }...] ` |  This statement starts and controls Redo Apply on physical standby databases  . You can use the ` RECOVER MANAGED STANDBY DATABASE ` clause on a physical standby database that is mounted, open, or closed. See Step 3 in [ Start the Physical Standby Database ](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569) and [ Applying Redo Data to Physical Standby Databases ](oracle-data-guard-redo-apply-services.md#GUID-2B839172-947E-48A4-9FFD-33CC6907809F) for examples.  Note:  Several clauses and keywords were deprecated and are supported for backward compatibility only. See [ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF52467) for more information about these deprecated clauses.   
` RECOVER ` ` MANAGED ` ` STANDBY ` ` DATABASE ` ` CANCEL ` |  The ` CANCEL ` clause cancels Redo Apply on a physical standby database after applying the current archived redo log file.  Note:  Several clauses and keywords were deprecated and are supported for backward compatibility only. See [ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF52467) for more information about these clauses.   
` RECOVER ` ` MANAGED ` ` STANDBY ` ` DATABASE ` ` FINISH ` |  The ` FINISH ` clause initiates failover on the target physical standby database and recovers the current standby redo log files. Use the ` FINISH ` clause only in the event of the failure of the primary database. This clause overrides any delay intervals specified.  Note:  Several clauses and keywords were deprecated and are supported for backward compatibility only. See [ *Oracle Database SQL Language Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SQLRF52467) for more information about these clauses.   
` REGISTER [OR REPLACE] [PHYSICAL|LOGICAL] LOGFILE ` *filespec*  |  Allows the registration of manually copied archived redo log files.  Note  : Issue this command only after manually copying the corresponding archived redo log file to the standby database. Issuing this command while the log file is in the process of being copied or when the log file does not exist may result in errors on the standby database at a later time.   
` RECOVER TO LOGICAL STANDBY ` *`new_database_name`*  |  Instructs apply services to continue applying changes to the *physical* standby database until you issue the command to convert the database to a *logical* standby database. See [ Convert to a Logical Standby Database ](creating-oracle-data-guard-logical-standby.md#GUID-715EBEC1-21B9-4DA9-80A5-ED58033F94C8) for more information.   
` RESET DATABASE TO INCARNATION ` *`integer`*  |  Resets the target recovery incarnation for the database from the current incarnation to a different incarnation.   
` SET ` ` STANDBY ` ` DATABASE ` ` TO ` ` MAXIMIZE ` { ` PROTECTION ` \| ` AVAILABILITY ` \| ` PERFORMANCE ` }  |  Use this clause to specify the level of protection for the data in your Oracle Data Guard configuration. You specify this clause from the primary database.   
` START LOGICAL STANDBY APPLY INITIAL ` [ *`scn-value`* ] ] [ ` NEW PRIMARY ` *`dblink`* ]  |  This statement is for logical standby databases only.  It starts SQL Apply on a logical standby database. See [ Starting SQL Apply ](oracle-data-guard-redo-apply-services.md#GUID-5E640AEE-12C7-4ED8-8C42-B989F7768E91) for examples of this statement.   
` {STOP\|ABORT} LOGICAL STANDBY APPLY ` |  This statement is for logical standby databases only.  Use the ` STOP ` clause to stop SQL Apply on a logical standby database in an orderly fashion. Use the ` ABORT ` clause to stop SQL Apply abruptly. See [ Performing a Failover to a Logical Standby Database ](managing-oracle-data-guard-role-transitions.md#GUID-EBD0C21B-6356-446E-8BF5-7B7D96554713) for an example of this statement.   
` SWITCHOVER TO ` *target_db_name*  |  This statement is for physical standby databases only.  It initiates a switchover on the primary database to the specified physical standby database.   
  
###  ALTER SESSION Statements {#GUID-0C30DE80-F67E-4B27-9ABE-F6FA51358739} 

This table describes the ` ALTER SESSION ` statements that are relevant to Oracle Data Guard. 

**Table: ALTER SESSION Statements Used in Oracle Data Guard Environments** 

ALTER SESSION Statement  |  Description   
---|---  
` ALTER ` ` SESSION ` [ ` ENABLE ` | ` DISABLE ` ] ` GUARD ` |  This statement is for logical standby databases only.  This statement allows privileged users to turn the database guard on and off for the current session.  See [ Modifying a Logical Standby Database ](managing-oracle-data-guard-logical-standby-databases.md#GUID-43971CB9-3337-4575-B4EF-D591878982F1) for more information.   
` ALTER SESSION SYNC WITH PRIMARY ` |  This statement is for physical standby databases only.  This statement synchronizes a physical standby database with the primary database, by blocking until all redo data received by the physical standby at the time of statement invocation has been applied.  See [ Forcing Redo Apply Synchronization in a Real-time Query Environment ](managing-oracle-data-guard-physical-standby-databases.md#GUID-194B1448-84FB-4357-A6AD-72A77701F918) for more information.   
  
###  ALTER SYSTEM Statements {#GUID-7CFC5C95-68EF-49E7-A95E-4926EFA2C934} 

This table describes the ` ALTER SYSTEM ` statements that are relevant to Oracle Data Guard. 

**Table: ALTER SYSTEM Statements Used in Oracle Data Guard Environments** 

ALTER SYSTEM Statement  |  Description   
---|---  
` ALTER SYSTEM FLUSH REDO TO target_db_name [[NO] CONFIRM APPLY] ` |  This statement flushes redo data from a primary database to a standby database and optionally waits for the flushed redo data to be applied to a physical or logical standby database.  This statement must be issued on a mounted, but not open, primary database. 
