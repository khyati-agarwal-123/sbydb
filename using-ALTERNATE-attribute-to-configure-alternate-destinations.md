##  HUsing the ALTERNATE Attribute to Configure Remote Alternate Destinations {#GUID-7055337B-A852-402F-8B2F-2E119CBE6437} 

As of Oracle Database 12 *c* Release 2 (12.2), the preferred method for creating alternate log archive destinations for remote standby databases and far sync instances that take over if the active destination fails is to use the ` GROUP ` and ` PRIORITY ` attributes. 

For local archiving locations ( ` LOCATION= ` …), the ` ALTERNATE ` attribute is still used to provide high availability if the original archiving directory becomes unavailable due to disk or network issues that prevent access to the archiving location. But use of the ` ALTERNATE ` attribute for remote log archive destinations ( ` SERVICE= ` …) is maintained only for backwards compatibility. The examples provided in the following sections for using this method are a follow-on for creating a far sync instance, but they also apply to cascading redo destinations. 

After you perform the steps in [ Creating and Configuring a Far Sync Instance ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3) , the far sync instance provides zero data loss capability for the configuration to the terminal standby at a remote site over the WAN. For the configuration to remain protected, but at a reduced protection level, in the event that communication with the far sync instance is lost, you can optionally configure the terminal standby to automatically become the alternate destination. This reduces the amount of data loss by allowing Oracle Data Guard to ship redo asynchronously directly from the primary to the terminal standby, temporarily bypassing the far sync instance. 

> **note:** See Also: 

  * [ Alternate Destinations ](creating-oracle-data-guard-far-sync-instance.md#GUID-5CA464E7-40B6-486C-AF95-2F04493EB2DD)




###  Configuring an Alternate Destination {#GUID-2281C292-AFB0-4515-97DA-F9C4FC01C2BA} 

To configure an alternate destination, set these parameters on the primary database. 

Primary Database chicago 
    
    
    ```
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
     
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC AFFIRM MAX_FAILURE=1 ALTERNATE=LOG_ARCHIVE_DEST_3
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS'
     
    LOG_ARCHIVE_DEST_STATE_3='ALTERNATE'
     
    LOG_ARCHIVE_DEST_3='SERVICE=boston ASYNC ALTERNATE=LOG_ARCHIVE_DEST_2
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=boston'
    ```

This configuration enables Oracle Data Guard to continue sending redo, asynchronously, to the terminal standby ` boston ` when it can no longer send the redo directly to the far sync instance ` chicagoFS ` . When the far sync instance becomes available again, Oracle Data Guard automatically resynchronizes the far sync instance ` chicagoFS ` and returns to the original configuration in which the primary sends redo to the far sync instance and the far sync instance forwards that redo to the terminal standby. When the synchronization is complete, the alternate destination ( ` LOG_ARCHIVE_DEST_3 ` in the preceding example) again becomes dormant as the alternate. 

In the above case, the ` ALTERNATE ` remote destination is set up directly between two databases using asynchronous redo transport, so in the event of a failure of the far sync instance, the protection level of the configuration falls back down to maximum performance, with data loss at failover time. For more protection from system or network failures, an additional far sync instance can be configured that provides high availability for the active far sync instance. 

In the following configuration, one far sync instance is the preferred active far sync instance and the other is the alternate far sync instance. Configuring an alternate far sync instance provides continued protection for the configuration if the preferred far sync instance fails for some reason, keeping the configuration at maximum availability. The primary automatically starts shipping to the alternate far sync instance if it detects a failure at the preferred far sync instance. If the preferred far sync instance then re-establishes itself, the primary switches back to the preferred far sync instance and puts the alternate far sync instance back into the alternate state. 

In these types of configurations, the primary uses only one of the two far sync instances to redistribute redo at any given time. 

The second high availability far sync instance would be created using the same steps as given in [ Creating and Configuring a Far Sync Instance ](creating-oracle-data-guard-far-sync-instance.md#GUID-43FE645C-1383-44F1-9127-7EFB2A402DB3) , and then become the alternate to the existing far sync instance instead of the terminal standby. When complete, ` chicago ` would have the parameters configured as follows (assuming the name ` chicagoFS1 ` as the new far sync instance name). 

Primary Database chicago 
    
    
    ```
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,chicagoFS1,boston)'
     
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    
    LOG_ARCHIVE_DEST_2='SERVICE=chicagoFS SYNC AFFIRM MAX_FAILURE=1 ALTERNATE=LOG_ARCHIVE_DEST_3
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS'
     
    LOG_ARCHIVE_DEST_STATE_3='ALTERNATE'
     
    LOG_ARCHIVE_DEST_3='SERVICE=chicagoFS1 SYNC AFFIRM ALTERNATE=LOG_ARCHIVE_DEST_2
    VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=chicagoFS1'
    
    ```

Primary Database boston 
    
    
    ```
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(chicago,chicagoFS,chicagoFS1,boston)'
    ```

Oracle Data Guard can now continue to synchronously send redo to a far sync instance, maintaining the required zero data loss protection mode of maximum availability in the event that a far sync instance fails for some reason. As before, when the failed far sync instance becomes available again, Oracle Data Guard automatically resynchronizes it and returns to the original configuration in which the primary sends redo to the first far sync instance, which then forwards that redo to the terminal standby. When the synchronization is complete, the alternate destination ( ` LOG_ARCHIVE_DEST_3 ` in the preceding example) again becomes dormant as the alternate. But if both far sync instances fail, then redo is not sent to the terminal standby ` boston ` because there is no third alternate capability. That scenario can be accomplished using the ` GROUP ` and ` PRIORITY ` attributes instead of the ` ALTERNATE ` attribute. 

> **note:** See Also: 

  * [ ALTERNATE ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-88222D10-22FD-4F3B-BBE5-B59C8ABEC725)

  * [ GROUP ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-99D83167-AA3A-4311-B2A9-6E38A353E33C)

  * [ PRIORITY ](LOG_ARCHIVE_DEST_n-parameter-attributes.md#GUID-B5AC0CAF-92FB-4CE6-B164-CC4328E389F5)



