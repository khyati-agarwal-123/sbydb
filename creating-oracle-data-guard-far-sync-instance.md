##  5Using Far Sync Instances {#GUID-8AD7FBA2-42B0-46CF-852B-1AF0CB4A36E8} 

An Oracle Data Guard far sync instance is a remote Oracle Data Guard destination that accepts redo from the primary database and then ships that redo to other members of the Oracle Data Guard configuration. 

A far sync instance manages a control file, receives redo into standby redo logs (SRLs), and archives those SRLs to local archived redo logs, but that is where the similarity with standbys ends. A far sync instance does not have user data files, cannot be opened for access, cannot run redo apply, and can never function in the primary role or be converted to any type of standby database. 

Far sync instances are part of the Oracle Active Data Guard Far Sync feature, which requires an Oracle Active Data Guard license. 

A far sync instance consumes very little disk and processing resources, yet provides the ability to failover to a terminal destination with zero data loss, as well as offload the primary database of other types of overhead (for example, redo transport). 

All redo transport options available to a primary when servicing a typical standby destination are also available to it when servicing a far sync instance. And all redo transport options are available to a far sync instance when servicing terminal destinations (for example, performing redo transport compression, if you have a license for the Oracle Advanced Compression option). 

Many configurations have a primary database shipping redo to a standby database using asynchronous transport at the risk of some data loss at failover time. Using synchronous redo transport to achieve zero data loss may not be a viable option because of the impact on the commit response times at the primary due to network latency between the two databases. 

Creating a far sync instance close to the primary has the benefit of minimizing impact on commit response times to an acceptable threshold (due to the smaller network latency between primary and far sync instance) while allowing for higher data protection guarantees -- if the primary were to fail, and assuming the far sync instance was synchronized at the time of the failure, the far sync instance and the terminal standby would coordinate a final redo shipment from the far sync instance to the standby to ship any redo not yet available to the standby and then perform a zero-data-loss failover. 

See the following topics: 

  * [ Creating a Far Sync Instance ](creating-oracle-data-guard-far-sync-instance.md#GUID-3B796B03-DB88-4592-84CB-3E019C977CBA)

  * [ Alternate Destinations ](creating-oracle-data-guard-far-sync-instance.md#GUID-5CA464E7-40B6-486C-AF95-2F04493EB2DD)

  * [ Configuring Alternate Destinations ](creating-oracle-data-guard-far-sync-instance.md#GUID-A06EDCC8-7AE3-423E-826A-4C1A1B5FA987)

  * [ Supported Protection Modes for Far Sync Instances ](creating-oracle-data-guard-far-sync-instance.md#GUID-54F9001B-A5F2-4134-A6BA-521DC7E41E57)




###  Creating a Far Sync Instance {#GUID-3B796B03-DB88-4592-84CB-3E019C977CBA} 

Creating a far sync instance is similar to creating a physical standby except that data files do not exist at the far sync instance. 

Therefore, on a far sync instance there is no need to copy data files or restore data files from a backup. Once the far sync instance has been created, the configuration is modified to send redo synchronously from the primary database to the far sync instance in Maximum Availability mode and the far sync instance then forwards the redo asynchronously in real time. Lastly, the original asynchronous standby (referred to as the terminal standby) is configured to act as the alternate to the far sync instance in the event that communication with the far sync instance is interrupted. 

> **note:** 

In a configuration that contains a far sync instance, there must still be a direct network connection between the primary database and the remote standby database. The direct connection between the primary and the remote standby is used to perform health checks and switchover processing tasks. It is not used for redo transport unless the standby has been configured as an alternate destination in case the far sync instance fails and there is no alternate far sync configured to maintain the protection level. 

####  Creating and Configuring a Far Sync Instance {#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3} 

Take the following steps to create a far sync instance: 

  1. Create the control file for the far sync instance, as shown in the following example (the primary database does not have to be open, but it must at least be mounted): 
    
        ```
    SQL> ALTER DATABASE CREATE FAR SYNC INSTANCE CONTROLFILE AS - 
    > '/arch2/chicagoFS/control01.ctl';
    
    ```

The resulting control file enables ` chicagoFS ` to operate as a far sync instance that receives redo from primary database ` chicago ` . The path and file name shown are just an example; you could use any path or file name that you want. 

  2. Create a parameter file (PFILE) from the server parameter file (SPFILE) used by the primary database. Although most of the initialization settings in the parameter file are also appropriate for the far sync instance, some modifications must be made. For example, on a far sync instance, the ` DB_FILE_NAME_CONVERT ` and ` LOG_FILE_NAME_CONVERT ` parameters must be set, and the ` DB_UNIQUE_NAME ` of the far sync instance and the location of the far sync instance control file must be modified. [ Example 5-1 ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3__CJADCDJE) shows sample parameter file content for a far sync instance with a ` DB_UNIQUE_NAME ` of ` chicagoFS ` . 
  3. Create a server parameter file (SPFILE) from the edited parameter file (PFILE) to facilitate any subsequent changes to parameter values. If you do not use an SPFILE, then a warning is returned in the ` SHOW ` ` CONFIGURATION ` output when the far sync instance is added to an Oracle Data Guard broker configuration. 
  4. Use an operating system copy utility to copy the far sync instance control file created in Step 1 and the server parameter file (SPFILE) created in Step 3 from the primary system to the appropriate locations on the far sync instance system. 
  5. Create standby redo logs in the same way they are created for a regular standby. See [ Managing Standby Redo Logs ](oracle-data-guard-redo-transport-services.md#GUID-E6EC6104-3C38-482D-B807-A0E84ECFB937) . 

Because the ` LOG_FILE_NAME_CONVERT ` parameter was specified on the far sync instance (see [ Example 5-1 ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3__CJADCDJE) ), the standby redo logs are created automatically when redo transport begins from the primary, if they were created on the primary as described in [ Configure the Primary Database to Receive Redo Data ](creating-oracle-data-guard-physical-standby.md#GUID-CA9076B9-3360-4A36-87AF-3390B391CEFF) . 

> **note:** 

Standby redo log files used at the far sync instance cannot be shared with other databases. Therefore, all relevant considerations discussed in [ Standby Database Directory Structure Considerations ](getting-started-with-oracle-data-guard.md#GUID-B59DBCF7-13A7-4F49-97E9-BF1BEF803699) for standby redo log files also apply at the far sync instance. 

  6. If the far sync instance is to be hosted on a Windows system, use the ORADIM utility to create a Windows service. For example: 
    
        ```
    oradim –NEW –SID ChicagoFS –STARTMODE manual
    
    ```

After the far sync instance is created and running you can change the ` STARTMODE ` to ` auto ` to enable automatic startup of the far sync instance 

The ORADIM utility automatically determines the username for which this service should be created and prompts for a password for that username (if that username needs a password). See [ *Oracle Database Platform Guide for Microsoft Windows* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NTQRF05011) for more information about using the ORADIM utility. 

  7. This step is optional if operating system authentication is used for administrative users and if SSL is used for redo transport authentication. If not, then copy the primary database's remote login password file to the appropriate directory on the far sync instance. The password file must be recopied whenever an administrative privilege ( ` SYSDG ` , ` SYSOPER ` , ` SYSDBA ` , and so on) is granted or revoked, and after the password of any user with administrative privileges is changed. 

As of Oracle Database 12 *c* Release 2 (12.2.0.1), when a password file is manually updated at a far sync instance, the redo containing the same password changes from the primary database is automatically propagated to any standby databases that are set up to receive redo from that far sync instance. The password file is updated on the standby when the redo is applied. 

  8. On the far sync instance site, use Oracle Net Manager to configure a listener for the far sync instance. 

See [ *Oracle Database Net Services Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG219) for more information about the listener. 

  9. On the primary system, use Oracle Net Manager to create a network service name for the far sync instance ( ` chicagoFS ` ) that is to be used by redo transport services. 

On the far sync instance system, use Oracle Net Manager to create a network service name for the primary ( ` chicago ` ) and the terminal standby ( ` boston ` ) to be used by redo transport services. 

The Oracle Net service name must resolve to a connect descriptor that uses the same protocol, host address, port, and service that you specified when you configured the listeners for the primary database, the far sync instance, and the terminal standby database. The connect descriptor must also specify that a dedicated server be used. 

See the [ *Oracle Database Net Services Administrator's Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=NETAG175) for more information about service names. 

  10. Start the far sync instance in mount mode. 
  11. Verify that the far sync instance is operating properly. 

For information about validating a configuration after you create a far sync instance, see [ Validating a Configuration ](oracle-data-guard-redo-transport-services.md#GUID-A0AAA20B-4DF8-4698-A7E0-3BED5A2C11E9) . 

  12. Increase the protection mode of the configuration to Maximum Availability. On the primary database, execute the following command: 
    
        ```
    SQL> ALTER DATABASE SET STANDBY TO MAXIMIZE AVAILABILITY;
    
    ```

> **note:** See Also: 
     * [ Supported Protection Modes for Far Sync Instances ](creating-oracle-data-guard-far-sync-instance.md#GUID-54F9001B-A5F2-4134-A6BA-521DC7E41E57) for more information about far sync and protection modes 

     * [ Oracle Data Guard Protection Modes ](oracle-data-guard-protection-modes.md#GUID-16298839-9B8A-4E58-8EC9-27912E57F1CE) for more information about configuring different data protection modes 

     * [ *Oracle Database Backup and Recovery Reference*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF-GUID-E13D8A02-80F9-49A2-9C31-92DD3A795CE4) for more information about creating a Far Sync instance using RMAN. 




Example 5-1 Some of the Initialization Parameters Used for Far Sync Instances 

Primary  Database chicago 
    
    
    ```
    DB_UNIQUE_NAME=chicago
     
    CONTROL_FILES='/arch1/chicago/control01.ctl'
    
    DB_FILE_NAME_CONVERT='/boston/','/chicago/'
    
    LOG_FILE_NAME_CONVERT='/boston/','/chicago/' 
     
    FAL_SERVER=boston
     
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,boston)'
     
    LOG_ARCHIVE_DEST_1='LOCATION=USE_DB_RECOVERY_FILE_DEST
    VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=chicago'
     
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS'
    
    ```

Far Sync Instance chicagoFS 
    
    
    ```
    DB_UNIQUE_NAME=chicagoFS
     
    CONTROL_FILES='/arch2/chicagoFS/control01.ctl'
    
    DB_FILE_NAME_CONVERT='/chicago/','/chicagoFS/','/boston/','/chicagoFS/'
     
    LOG_FILE_NAME_CONVERT='/chicago/','/chicagoFS/','/boston/','/chicagoFS/'
    
    FAL_SERVER=chicago
     
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,boston)'
     
    LOG_ARCHIVE_DEST_1='LOCATION= USE_DB_RECOVERY_FILE_DEST
    VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=chicagoFS'
     
    LOG_ARCHIVE_DEST_2='SERVICE=boston ASYNC
    VALID_FOR=(STANDBY_LOGFILES,STANDBY_ROLE) DB_UNIQUE_NAME=boston'
    
    ```

Physical Standby boston 
    
    
    ```
    DB_UNIQUE_NAME=boston
     
    CONTROL_FILES='/arch3/boston/control01.ctl'
    
    DB_FILE_NAME_CONVERT='/chicago/','/boston/'
    
    LOG_FILE_NAME_CONVERT='/chicago/','/boston/'
     
    FAL_SERVER='chicagoFS','chicago'
     
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,boston)'
     
    LOG_ARCHIVE_DEST_1='LOCATION= USE_DB_RECOVERY_FILE_DEST
    VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=boston'
     
    LOG_ARCHIVE_DEST_2='SERVICE=chicago ASYNC
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicago'
    
    ```

###  Alternate Destinations {#GUID-5CA464E7-40B6-486C-AF95-2F04493EB2DD} 

After you perform the steps in [ Creating and Configuring a Far Sync Instance ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3) , the far sync instance provides zero data loss capability for the configuration by forwarding the redo to the terminal standby at a remote site over the WAN. For the configuration to remain protected in the event the far sync instance is not reachable, you must configure alternate redo transport paths to the standby databases. This is accomplished using the ` GROUP ` and ` PRIORITY ` attributes of the ` LOG_ARCHIVE_DEST_  n  ` parameter. (As of Oracle Database 12 *c* Release 2 (12.2.0.1), the ` GROUP ` and ` PRIORITY ` attributes have replaced the ` ALTERNATE ` attribute for remote redo destinations.) 

The number of possible alternate remote destinations has been increased with the concept of log archive destination groups. A log archive destination group specifies multiple archive destinations that can be used to distribute redo to multiple destinations, either from a far sync instance or through cascading. The destinations in the group can then be prioritized so that only one destination is active at a time on the primary database. Other destinations are available to become active if the active destination becomes unavailable. To expand the number of possible archive destinations for your database, you can specify multiple groups. 

> **note:** See Also: 

  * [ Using the ALTERNATE Attribute to Configure Remote Alternate Destinations ](using-ALTERNATE-attribute-to-configure-alternate-destinations.md#GUID-7055337B-A852-402F-8B2F-2E119CBE6437) for information about configuring alternate remote destinations using the old ` ALTERNATE ` syntax. 




####  Assigning Log Archive Destinations to a Group {#GUID-DB84EA69-8B64-4230-88AB-E83D5298C009} 

Use the ` GROUP ` attribute of the ` LOG_ARCHIVE_DEST_  n  ` initialization parameter to assign log archive destinations to groups. 

If log archive destination groups are used, then as long as at least one destination within the group remains available, at least one destination remains enabled and active. Log archive destinations that are not assigned to a group behave the same as log archive destinations did prior to Oracle Database 12 *c* Release 2 (12.2.0.1). 

There can be up to 30 log archive destinations in a group. Log archive destination groups are referenced by their group number, which is assigned when the group is created. Groups are numbered from 1 through 8. A log archive destination group contains a set of remote ( ` SERVICE= ` …) destinations. (Local archival ( ` LOCATION= ` …) destinations are not supported in log archive destination groups and must use the ` ALTERNATE ` attribute for alternate local archiving locations. See [ ALTERNATE ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725) . 

One log archive destination in the group is always active and the others are available for use in the event of a failure of the active log archive destination. When a failed destination again becomes available it becomes eligible if the currently active destination fails, but it does not become active immediately, unless all other group members are also unavailable. For example, the following declaration can be used to specify three far sync instances as members of the same group and having the same priority (Priority within a group is described in the next section). These are example parameter definitions and do not contain all the necessary attributes. Do not use them verbatim. In this example only the first destination is active with the second destination available to take over if destination 1 becomes unavailable. 
    
    
    ```
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1'
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3='SERVICE=chicagoFS1 SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1'
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    ```

> **note:** Because log archive destination groups replace the ` LOG_ARCHIVE_DEST_  n  ` ` ALTERNATE ` attribute, use of the ` ALTERNATE ` attribute with log archive destinations that are not in the default group (where ` GROUP ` is specified as 1 to 8) is not allowed. 

####  Assigning Priorities to Log Archive Destinations in a Group {#GUID-872B6548-DEC9-4BD1-9997-E1A4E3D2B808} 

Using the ` PRIORITY ` attribute of the ` LOG_ARCHIVE_DEST_  n  ` initialization parameter to assign destination preferences within a log archive destination group allows you to control the fail back mechanism, especially with multiple members within a group. 

In the previous section, the two far sync instance destinations did not have a priority, which means that when the alternate destination is activated after a failure of the first destination it remains as the active destination until it fails. The priority is used to determine which log archive destination within a group to make active when the database or far sync instance is started or when a destination fails. Log archive destinations become active in the following cases: The primary database is opened in read/write mode, a far sync instance is mounted, or a standby database is mounted or opened in read-only mode. The same priority value can be assigned to more than one log archive destination in a group. The priority value is an integer in the range of 1 through 8. Lower numbers indicate higher priorities. The default priority is 1 (the highest priority). 

The priority comes into play when a previously failed destination becomes available again. A set of log archive destinations assigned to the same group have the same priority, by default. Therefore, if one destination fails then a failover occurs to another member of the set. When the failed destination becomes available again, it does not become the active destination since both destinations have the same priority. If the second destination fails after the first destination has again become available, then the database fails over to the first destination or to another destination in the group at the same priority. This cycle can repeat indefinitely, provided that another destination is always available before the active destination fails. 

Continuing with the previous example, priorities can be added to the log archive destinations to control when a destination might become active. In the following example, a third far sync instance is added, but at a lower priority: 
    
    
    ```
    LOG_ARCHIVE_DEST_2=’SERVICE=chicagoFS SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3=’SERVICE=chicagoFS1 SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    LOG_ARCHIVE_DEST_4=’SERVICE=chicagoFS2 ASYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=2’
    
    LOG_ARCHIVE_DEST_STATE_4=ALTERNATE
    ```

This declaration results in the following behavior: 

  * The primary ships redo to the first of two preferred far sync instances, ` chicagoFS `

  * If ` chicagoFS ` become unavailable, then the primary ships to ` chicagoFS1 ` . 

  * If ` chicagoFS ` becomes available again, no fail back occurs. It becomes the alternate to ` chicagoFS1 ` because the priority is the same. 

  * If both ` chicagoFS ` and ` chicagoFS1 ` become unavailable, then the primary ships to ` chicagoFS2 ` (in this case via the ` ASYNC ` redo transmission mode). 

  * If either ` chicagoFS ` or ` chicagoFS1 ` become available while the primary is shipping to ` chicagoFS2 ` , then the primary fails back to that available preferred log archive destination. 




####  Shipping to Multiple Active Destinations in a Group {#GUID-3FFF0F6A-2C9D-46BD-860C-F166F8ADF9D6} 

You can also use the ` PRIORITY ` attribute to configure a group so that it ships to multiple destinations if a preferred destination fails. 

The mechanism that supports multiple active destinations within a single group is that the lowest priority ( ` PRIORITY=8 ` ) is defined to activate destinations within that group at that priority, generally used to send the redo directly to the target standby databases. The following log archive destination declaration shows how this could be configured. In this example , there is one far sync instance that forwards redo to two terminal standby databases: 
    
    
    ```
    LOG_ARCHIVE_DEST_2=’SERVICE=chicagoFS SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3=’SERVICE=boston ASYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=8’
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    LOG_ARCHIVE_DEST_4=’SERVICE=newyork ASYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=8’
    
    LOG_ARCHIVE_DEST_STATE_4=ALTERNATE
    
    ```

This declaration results in the following behavior: 

  * The primary ships redo to the preferred far sync instance, ` chicagoFS ` . 

  * If ` chicagoFS ` is unavailable, then the primary ships directly to both terminal standbys ` boston ` and ` newyork ` in ` ASYNC ` mode. 

  * While shipping to ` boston ` and ` newyork ` , if ` chicagoFS ` becomes available, then the primary stops shipping directly to ` boston ` and ` newyork ` and begins shipping instead to ` chicagoFS ` . 




####  Using Multiple Log Archive Destination Groups {#GUID-EF90DBDE-E91B-48DA-A346-B264247BE893} 

Multiple log archive destination groups can be used for site-specific high availability considerations or to distribute service over large cascaded (reader farm) configurations. 

The following declaration sets up multiple log archive destination groups with ` chicagoFS ` and ` chicagoFS1 ` in group 1 and ` chicagoFS3 ` and ` chicagoFS4 ` in group 2: 
    
    
    ```
    LOG_ARCHIVE_DEST_2=’SERVICE=chicagoFS SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3=’SERVICE=chicagoFS1 SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    LOG_ARCHIVE_DEST_4=’SERVICE=chicagoFS3 SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=2 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_4=ENABLE
    
    LOG_ARCHIVE_DEST_5=’SERVICE=chicagoFS4 SYNC 
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) GROUP=2 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_5=ALTERNATE
    
    ```

####  Determining the Availability Status of Log Archive Destinations {#GUID-0603306C-B432-4D55-BC40-E8FC5DBE1A1C} 

Oracle Data Guard keeps track of the current status of available but inactive destinations in log archive destination groups by periodically polling configured destinations to determine their availability. 

The information used to determine availability is derived from the ` MAX_FAILURE ` attribute which specifies the consecutive number of times redo transport services attempt to reestablish communication and transmit redo data to a failed destination before the primary database gives up on the destination. The default value for ` MAX_FAILURE ` is 1 when the ` GROUP ` and ` PRIORITY ` attributes are used. 

The behavior of the ` MAX_FAILURE ` attribute is different between Oracle Database 12 *c* Release 1 (12.1) and Oracle Database 12 *c* Release 2 (12.2). It is important to understand the differences. 

> **note:** See Also: 

  * [ MAX_FAILURE ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-3655C0B5-E669-4475-95B9-01A67AE6A144)




###  Configuring Alternate Destinations {#GUID-A06EDCC8-7AE3-423E-826A-4C1A1B5FA987} 

Far sync instance configurations can be set up to provide varying levels of data protection. 

The following topics expand on the examples provided in the previous section and provide examples of two additional far sync instance configurations that provide better data protection when you use far sync instances. 

  * [ Reduced Protection After a Far Sync Failure ](creating-oracle-data-guard-far-sync-instance.md#GUID-AE4BB5CA-D8F3-417F-948D-F41414748E82)

  * [ Far Sync Instance High Availability ](creating-oracle-data-guard-far-sync-instance.md#GUID-32FFD623-EB0B-40B5-9EE2-98D94EDB025C)

  * [ Maintaining Protection After a Role Change ](creating-oracle-data-guard-far-sync-instance.md#GUID-8A5DA942-04A5-4C35-A072-420EEBF92DCB)




####  Reduced Protection After a Far Sync Failure {#GUID-AE4BB5CA-D8F3-417F-948D-F41414748E82} 

With all far sync instance configurations it is important that redo continues to ship to the terminal standbys to continue to provide protection of the primary database. 

In the simplest configuration there is one far sync instance ( ` chicagoFS ` ) and one terminal standby database ( ` boston ` ). 

If the far sync instance fails, then redo should be shipped directly to the terminal standby by adding an additional log archive destination to the primary database, ` chicago ` . This does reduce the protection level because redo transmission is then in ` ASYNC ` mode instead of ` SYNC ` mode. 

Example 5-2 Configuring for Single Destination Failover 

Primary Database chicago 
    
    
    ```
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3='SERVICE=boston ASYNC NOAFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=boston GROUP=1 PRIORITY=2'
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    ```

This declaration causes the primary database to ship redo directly to the terminal standby if the far sync instance ` chicagoFS ` fails. If the far sync instance becomes available again, then it becomes the active destination and redo transmission goes to the far sync instance. 

If the far sync instance had multiple terminal standby databases, then you would use ` PRIORITY=8 ` to ensure that all of those destinations received redo directly from the primary database if the far sync instance failed. 

Example 5-3 Configuring for Multiple Standby Database Redo Destination Failover 

Primary Database chicago 

As in the previous example, modify the log archive destination on the primary database for the far sync instance to add it to a group with a Priority of 1 and then add a new log archive destination for each standby the far sync instance services at Priority 8 in ASYNC mode. 
    
    
    ```
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3='SERVICE=boston ASYNC NOAFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=boston GROUP=1 PRIORITY=8'
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    LOG_ARCHIVE_DEST_4='SERVICE=newyork ASYNC NOAFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=newyork GROUP=1 PRIORITY=8'
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    ```

This declaration causes the primary database to ship redo directly to both terminal standby databases if the far sync instance ` chicagoFS ` fails. If the far sync instance becomes available again, then it becomes the active destination and redo transmission goes to the far sync instance. 

####  Far Sync Instance High Availability {#GUID-32FFD623-EB0B-40B5-9EE2-98D94EDB025C} 

Configuring an alternate far sync instance keeps the protection level of the configuration at the configured protection level of Maximum Availability if the preferred far sync instance fails for some reason. 

In both of the preceding examples the protection level of the configuration would fall out of Maximum Availability because redo is no longer being shipped in ` SYNC ` mode. For more protection from system or network failures, an additional far sync instance can be configured that provides high availability for the active far sync instance. In this configuration one is the preferred active far sync instance and the other is the alternate far sync instance. 

The primary automatically starts shipping to the alternate far sync instance if it detects a failure at the preferred far sync instance. In these types of configurations, the primary uses only one far sync instance to redistribute redo at any given time. 

To maintain the Maximum Availability protection level, configure two far sync instances near to the primary database and set them up to protect each other. Then, if the active far sync instance becomes unavailable, the primary database can automatically begin sending redo in synchronous mode to the alternate far sync instance, thereby maintaining the elevated protection level of Maximum Availability. In this case though, the two far sync instances have the same priority and when one takes over for the other it remains the active far sync instance until it fails. To ensure that redo continues to be shipped to the terminal standby database in the event that both far sync instances fail, the terminal standby database is configured as before with ` PRIORITY=2 ` . (If there is more than one terminal standby database, then use ` PRIORITY=8 ` for them). 

The high availability far sync instance would be created using the same steps as given in [ Creating and Configuring a Far Sync Instance ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3) , and configured to forward redo to the terminal standby boston. 

Example 5-4 Parameters Used to Set Up the High Availability Far Sync Instance 

Primary Database chicago 
    
    
    ```
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,chicagoFS1,boston)'
     
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3='SERVICE=chicagoFS1 SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS1 GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    LOG_ARCHIVE_DEST_4='SERVICE=boston ASYNC NOAFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=boston GROUP=1 PRIORITY=2'
    
    LOG_ARCHIVE_DEST_STATE_4=ALTERNATE
    ```

Oracle Data Guard can now continue synchronously sending redo to a far sync instance, maintaining the required zero data loss protection mode of Maximum Availability if a far sync instance fails. If both far sync instances fail, then redo ships in ` ASYNC ` mode directly to ` boston ` , at a reduced protection level. As before, when either of the failed far sync instances becomes available again, Oracle Data Guard automatically resynchronizes it and returns to the original configuration, in which the primary sends redo to an active far sync instance, which then forwards that redo to the terminal standby. When the synchronization is complete, the alternate destination for the standby ( ` LOG_ARCHIVE_DEST_4 ` in the preceding example) again becomes dormant as the alternate. 

####  Maintaining Protection After a Role Change {#GUID-8A5DA942-04A5-4C35-A072-420EEBF92DCB} 

These examples describe how to maintain data protection after a role change. 

The configuration described in the preceding sections works well to keep the configuration running at Maximum Availability until all far sync instances fail and redo is shipped to the standby database directly. But it would be inappropriate after a role transition where ` boston ` becomes the primary database and ` chicago ` becomes the terminal standby. The far sync instances ` chicagoFS ` and ` chicagoFS1 ` would be too remote for ` boston ` to use as a synchronous destination because the network latency between two sites is sufficiently large that it would impact commit response times. To maintain the protection level of Maximum Availability for zero data loss, a second far sync instance configuration close to ` boston ` must be established, in readiness for a future role transition event. 

Using the same procedure as described in [ Creating and Configuring a Far Sync Instance ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3) , create two far sync instances named ` bostonFS ` and ` bostonFS1 ` close to the standby database ` boston ` and configure them both to ship redo to ` chicago ` in ` ASYNC ` mode when they are active. Then add them to ` boston ` so that when ` boston ` is the primary it ships redo to one of the far sync instances in ` SYNC ` mode with all the failover capabilities that were configured for ` chicago ` and its far sync instances. You need to add the new ` boston ` far sync instances to the ` LOG_ARCHIVE_CONFIG ` on both ` boston ` and ` chicago ` . 

Example 5-5 Parameters Used to Set Up Protection After a Role Change 

Primary Database boston 
    
    
    ```
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,chicagoFS1,boston, bostonFS, bostonFS1)'
     
    LOG_ARCHIVE_DEST_2='SERVICE=bostonFS SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=bostonFS GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    
    LOG_ARCHIVE_DEST_3='SERVICE=bostonFS1 SYNC AFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=bostonFS1 GROUP=1 PRIORITY=1’
    
    LOG_ARCHIVE_DEST_STATE_3=ALTERNATE
    
    LOG_ARCHIVE_DEST_4='SERVICE=chicago ASYNC NOAFFIRM
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicago GROUP=1 PRIORITY=2'
    
    LOG_ARCHIVE_DEST_STATE_4=ALTERNATE
    ```

Primary Database chicago 
    
    
    ```
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,chicagoFS1,boston, bostonFS, bostonFS1)'
    ```

Given these declarations, the far sync instance, ` bostonFS ` , receives redo from ` boston ` and ships it to ` chicago ` only when ` boston ` is the primary database. However, even if ` boston ` is not the primary database, Oracle recommends keeping far sync instance ` bostonFS ` and ` bostonFS1 ` mounted in readiness for a future role transition. 

###  Supported Protection Modes for Far Sync Instances {#GUID-54F9001B-A5F2-4134-A6BA-521DC7E41E57} 

A far sync instance is supported in either maximum performance or maximum availability mode. 

####  Far Sync Instances in Maximum Availability Mode Configurations {#GUID-1353E8E6-848C-451A-89EC-42FD178AA256} 

In maximum availability mode, the far sync instance is relatively close to the primary database to minimize network latency, and the primary services the far sync instance using ` SYNC ` transport. 

> **note:** 

There is no architectural limit to the distance that can separate the primary and far sync instance in maximum availability mode. The practical distance limit varies depending upon a given application's tolerance to the impact of network latency in a synchronous configuration. Also, it is possible to reduce the performance impact for any given distance by using the new Oracle Data Guard FastSync feature ( ` SYNC/NOAFFIRM ` ). See  " [ Performance Versus Protection in Maximum Availability Mode ](oracle-data-guard-protection-modes.md#GUID-7EF6EFEE-7E31-4F80-9C97-1B25DAA025F8__BABBFEGF) "  . 

Both ` SYNC/AFFIRM ` and ` SYNC/NOAFFIRM ` semantics are supported on the ` LOG_ARCHIVE_DEST_ ` *n* established at the primary for the far sync instance. See [ Oracle Data Guard Protection Modes ](oracle-data-guard-protection-modes.md#GUID-7EF6EFEE-7E31-4F80-9C97-1B25DAA025F8) for information about the trade-offs of using each one. 

When a primary services a far sync instance using ` SYNC ` transport, all committed redo resides on disk at the far sync instance. This allows the far sync instance to use one of the terminal standby destinations for a no data loss failover if the primary database is lost. 

The far sync instance uses ` ASYNC ` transport to ship the incoming redo to terminal standbys that can be much farther away. This extends no data loss protection to destinations that are too far away for a primary database to feasibly service directly with ` SYNC ` transport because of the degradation in transaction throughput that would result. This is a case where a far sync instance is beneficial even if there is only one standby destination in the configuration. 

####  Far Sync Instances in Maximum Performance Mode Configurations {#GUID-5FB20778-2EB5-4E96-B47E-53D67F9A4536} 

In maximum performance mode, the primary database services the far sync instance destination using ` ASYNC ` redo transport. 

This is true regardless of the physical distance between the primary and the far sync instance because high network latencies do not affect transaction throughput when a destination is serviced with ` ASYNC ` transport. 

In maximum performance mode, a far sync instance can benefit Oracle Data Guard configurations that manage more than one remote destination. Although each ` ASYNC ` destination has a near-zero effect on primary database performance, if there are many remote destinations (for example, multiple Oracle Active Data Guard standbys that form a reader farm), then the effect can become measurable. When a far sync instance is used, there is zero incremental effect for each remote destination added to the configuration. Additionally, redo transport compression can also be offloaded to the far sync instance. When a far sync instance is used, the primary only has to service the far sync instance, which then services the rest of the configuration; the greater the number of destinations, the greater the performance benefit. 
