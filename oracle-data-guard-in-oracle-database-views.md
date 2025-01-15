##  19Views Relevant to Oracle Data Guard {#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A} 

There are a number of views that are especially useful when monitoring an Oracle Data Guard environment. 

[ Table 19-1 ](oracle-data-guard-in-oracle-database-views.md#GUID-FF72163C-78B0-475F-B4CE-135EF288A32A__BACJHDCH) describes the views and indicates if a view applies to physical standby databases, logical standby databases, snapshot standby databases, or primary databases. See [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN) for complete information about views. 

**Table: Views That Are Pertinent to Oracle Data Guard Configurations** 

View  |  Database  |  Description   
---|---|---  
` DBA_LOGSTDBY_EVENTS ` |  Logical only  |  Contains information about the activity of a logical standby database. It can be used to determine the cause of failures that occur when SQL Apply is applying redo to a logical standby database.   
` DBA_LOGSTDBY_HISTORY ` |  Logical only  |  Displays the history of s  witchovers and f  ailovers for logical standby databases in an Oracle Data Guard configuration. It does this by showing the complete sequence of redo log streams processed or created on the local system, across all role transitions. (After a role transition, a new log stream is started and the log stream sequence number is incremented by the new primary database.)   
` DBA_LOGSTDBY_LOG ` |  Logical only  |  Shows the log files registered for logical standby databases.   
` DBA_LOGSTDBY_NOT_UNIQUE ` |  Logical only  |  Identifies tables that have no primary and no non-null unique indexes.   
` DBA_LOGSTDBY_PARAMETERS ` |  Logical only  |  Contains the list of parameters used by SQL Apply.   
` DBA_LOGSTDBY_SKIP ` |  Logical only  |  Lists the tables to be skipped by SQL Apply.   
` DBA_LOGSTDBY_SKIP_TRANSACTION ` |  Logical only  |  Lists the skip settings chosen.   
` DBA_LOGSTDBY_UNSUPPORTED ` |  Logical only  |  Identifies the schemas and tables (and columns in those tables) that contain unsupported data types. Use this view when you are preparing to create a logical standby database.   
` DBA_ROLLING_UNSUPPORTED ` |  Logical only  |  Displays the schemas, tables, and columns in those tables, that contain unsupported data types for a rolling upgrade operation for a logical standby database using the ` DBMS_ROLLING ` PL/SQL package. Use this view before you perform a rolling upgrade using ` DBMS_ROLLING ` to determine what is unsupported.   
` V$ARCHIVE_DEST ` |  Primary, physical, snapshot, and logical  |  Describes all of the destinations in the Oracle Data Guard configuration, including each destination's current value, mode, and status.  Note:  The information in this view does not persist across an instance shutdown.   
` V$ARCHIVE_DEST_STATUS ` |  Primary, physical, snapshot, and logical  |  Displays runtime and configuration information for the archived redo log destinations  .  Note:  The information in this view does not persist across an instance shutdown.   
` V$ARCHIVE_GAP ` |  Physical, snapshot, and logical  |  Displays information to help you identify a gap in the archived redo log files.   
` V$ARCHIVED_LOG ` |  Primary, physical, snapshot, and logical  |  Displays archive redo log information from the control file, including names of the archived redo log files.   
` V$DATABASE ` |  Primary, physical, snapshot, and logical  |  Provides database information from the control file. Includes information about fast-start failover (available only with the Oracle Data Guard broker).   
` V$DATABASE_INCARNATION ` |  Primary, physical, snapshot, and logical  |  Displays information about all database incarnations. Oracle Database creates a new incarnation whenever a database is opened with the ` RESETLOGS ` option. Records about the current and the previous incarnation are also contained in the ` V$DATABASE ` view.   
` V$DATAFILE ` |  Primary, physical, snapshot, and logical  |  Provides data file information from the control file.   
` V$DATAGUARD_CONFIG ` |  Primary, physical, snapshot, and logical  |  Lists the unique database names defined with the ` DB_UNIQUE_NAME ` and ` LOG_ARCHIVE_CONFIG ` initialization parameters.   
` V$DATAGUARD_STATS ` |  Primary, physical, snapshot, and logical  |  Displays various Oracle Data Guard statistics, including apply lag and transport lag. This view can be queried on any instance of a standby database. No rows are returned if queried on a primary database. See also [ Choosing a Target Standby Database for a Role Transition ](managing-oracle-data-guard-role-transitions.md#GUID-60804EE7-88C0-4CB7-AEE7-073EBCAFD1AC) for an example and more information.   
` V$DATAGUARD_STATUS ` |  Primary, physical, snapshot, and logical  |  Displays and records events that would typically be triggered by any message to the alert log or server process trace files.   
` V$DG_BROKER_CONFIG ` |  Primary, physical, snapshot, and logical  |  Provides a view of the entire Oracle Data Guard broker configuration from any database in the configuration.   
` V$DG_BROKER_PROPERTY ` |  Primary, physical, snapshot, and logical  |  Displays Data Guard Broker properties.   
` V$DG_BROKER_ROLE_CHANGE ` |  Primary  |  Displays information about the past role changes across a Data Guard broker configuration. The role change history is maintained in the configuration metadata. It keeps up to 10 records of the latest role changes.   
` V$FAST_START_FAILOVER_CONFIG ` |  Primary  |  Query this view on the primary database to display statistics about the fast-start failover configuration.   
` V$FS_FAILOVER_OBSERVERS ` |  Primary  |  Displays detailed information about all observers.   
` V$FS_OBSERVER_HISTOGRAM ` |  Primary  |  Displays statistics that are based on the frequency of successful pings between the observer and primary database for different time intervals.   
` V$LOG ` |  Primary, physical, snapshot, and logical  |  Contains log file information from the online redo log files.   
` V$LOGFILE ` |  Primary, physical, snapshot, and logical  |  Contains information about the online redo log files and standby redo log files.   
` V$LOG_HISTORY ` |  Primary, physical, snapshot, and logical  |  Contains log history information from the control file.   
` V$LOGSTDBY_PROCESS ` |  Logical only  |  Provides dynamic information about what is happening with SQL Apply. This view is very helpful when you are diagnosing performance problems during SQL Apply on the logical standby database, and it can be helpful for other problems.   
` V$LOGSTDBY_PROGRESS ` |  Logical only  |  Displays the progress of SQL Apply on the logical standby database.   
` V$LOGSTDBY_STATE ` |  Logical only  |  Consolidates information from the ` V$LOGSTDBY_PROCESS ` and ` V$LOGSTDBY_STATS ` views about the running state of SQL Apply and the logical standby database.   
` V$LOGSTDBY_STATS ` |  Logical only  |  Displays LogMiner statistics, current state, and status information for a logical standby database during SQL Apply. If SQL Apply is not running, the values for the statistics are cleared.   
` V$LOGSTDBY_TRANSACTION ` |  Logical only  |  Displays information about all active transactions being processed by SQL Apply on the logical standby database.   
` V$MANAGED_STANDBY ` |  Physical and snapshot  |  Displays current status information for Oracle database processes related to physical standby databases.  Note:  The information in this view does not persist across an instance shutdown.   
` V$REDO_DEST_RESP_HISTOGRAM ` |  Primary  |  Contains the response time information for destinations that are configured for ` SYNC ` transport.  Note:  The information in this view does not persist across an instance shutdown.   
` V$STANDBY_EVENT_HISTOGRAM ` |  Physical  |  Contains a histogram of apply lag values for the physical standby. An entry is made in the corresponding apply lag bucket by the Redo Apply process every second. (This view returns rows only on a physical standby database that has been open in real-time query mode.)  Note:  The information in this view does not persist across an instance shutdown.   
` V$STANDBY_LOG ` |  Physical, snapshot, and logical  |  Contains log file information from the standby redo log files. 
