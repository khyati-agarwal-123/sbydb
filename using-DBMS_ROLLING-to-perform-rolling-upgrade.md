##  14Using DBMS_ROLLING to Perform a Rolling Upgrade {#GUID-70C09F5B-90BE-4C8C-96A5-45A52E05D380} 

The Rolling Upgrade Using Oracle Active Data Guard feature provides a streamlined method of performing rolling upgrades. 

It is implemented using the ` DBMS_ROLLING ` PL/SQL package, which enables you to upgrade the database software in an Oracle Data Guard configuration in a rolling fashion. The Rolling Upgrade Using Oracle Active Data Guard feature requires a license for the Oracle Active Data Guard option. 

You can use this feature to perform database version upgrades starting with the first patchset of Oracle Database 12 *c* . You cannot use it to upgrade from any version earlier than the first Oracle Database 12 *c* patchset. This means that the manual Transient Logical Standby upgrade procedure must still be used when upgrading from the initial Oracle Database 12 *c* release to the first patchset of Oracle Database 12 *c* . 

The ` DBMS_ROLLING ` package performs Oracle Data Guard switchovers to minimize downtime of the primary database service. Prior to using ` DBMS_ROLLING ` , the Oracle Data Guard environment must be properly configured to accommodate switchovers. The setup requirements differ depending on whether Oracle Data Guard broker is active during execution of ` DBMS_ROLLING ` : 

  * If the broker is going to be active during ` DBMS_ROLLING ` , then see [ *Oracle Data Guard Broker*  ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-AC6A6D2B-1112-422B-8A41-02FAC40E2078) for information about setting up the broker for a switchover. 

  * If the broker is not going to be active during ` DBMS_ROLLING ` , then see [ Role Transitions Involving Logical Standby Databases ](managing-oracle-data-guard-role-transitions.md#GUID-CE785178-FD22-4A81-9248-DF5ED8E91635) . The absence of Oracle Data Guard broker means that ` LOG_ARCHIVE_DEST_ ` *`n`* parameters must be properly configured on the target primary database so that redo shipping resumes after the switchover. 




Additionally, you can use this feature immediately for other database maintenance tasks beginning with Oracle Database 12 *c* Release 1 (12.1). The database where maintenance is performed must be operating at a minimum of Oracle 12.1. Such maintenance tasks include: 

  * Adding partitioning to non-partitioned tables 

  * Changing BasicFiles LOBs to SecureFiles LOBs 

  * Changing ` XMLType ` stored as ` CLOB ` to ` XMLtype ` stored as binary XML 

  * Altering tables to be OLTP-compressed 




See the following topics: 

  * [ Concepts New to Rolling Upgrades ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-647FF758-5B53-44AE-BB80-B6FA506F09C7)

  * [ DBMS_ROLLING Upgrades in a Multitenant Environment ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-BC4E1CA8-4431-4FC0-8437-106C934E88EA)

  * [ Overview of Using DBMS_ROLLING ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-D42FC23B-C6F4-44D1-A46A-202AB845BB80)

  * [ Planning a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-CD45406F-0A0F-4C9E-95FF-42C908BE62BE)

  * [ Performing a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6)

  * [ Monitoring a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-A1348F5A-AFBC-4090-822F-23DF87CE7D3C)

  * [ Rolling Back a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5B34621B-FABE-4C50-A2C0-ED249844934D)

  * [ Handling Role Changes That Occur During a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-1019E58C-1370-48A6-BC67-FA42074C0351)

  * [ Examples of Rolling Upgrades ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9)




###  Concepts New to Rolling Upgrades {#GUID-647FF758-5B53-44AE-BB80-B6FA506F09C7} 

To upgrade the database software in an Oracle Data Guard configuration in a rolling fashion, you first designate a physical standby as the future primary database. 

Conceptually, the rolling upgrade process splits the Oracle Data Guard configuration into two groups: the leading group (LG) and the trailing group (TG). 

Databases in the leading group are upgraded first; hence the name leading group. The leading group contains the designated future primary database, and the physical standbys that you can configure to protect the designated future primary. The future primary is first converted into a logical standby database and then the new database software is installed on it and the upgrade process is run. Other standby databases in the leading group also must have their software upgraded at this point. 

The trailing group contains the original primary database and standby databases that protect the original primary during the rolling upgrade process. While the databases in the leading group are going through the upgrade process, user applications can still be connected to the original primary and making changes. The trailing group databases continue running the old database software until all the databases in the leading group are upgraded and the future primary has caught up with the original primary by applying the changes that were generated at the original primary database during the upgrade window. At this point a switchover is done to transfer the primary role to the designated future primary database, and the user applications are switched over to the new primary database. New software is then installed on the databases that are part of the trailing group, and they are reinstated into the configuration as standbys to the new primary database. 

The standbys in the respective groups are called the Leading Group Standbys (LGS) and Trailing Group Standbys (TGS). Other than the designated future primary, all other standbys in the leading group can only be physical standbys. The trailing group can contain both physical and logical standbys; they are called Trailing Group Physical (TGP) and Trailing Group Logical (TGL) in cases where it is necessary to make a distinction between the standby types. The designated future primary is also called the Leading Group Master (LGM) and the original primary database is called the Trailing Group Master (TGM). 

The ` DBMS_ROLLING ` package increases the robustness of the rolling upgrade process as follows: 

  * It can handle failures during the rolling upgrade process. The original primary or the TGM database can fail. You can initiate a regular failover operation to any other physical standby in the trailing group, and then designate the new primary database as the TGM. 

  * It allows data protection of the LGM (the designated future primary) during the rolling upgrade process. You can set up physical standbys for the LGM database, and thus protect it during the upgrade process and also achieve Zero Data Loss after the upgrade. After the LGM has been successfully upgraded, a failure in the LGM can be accommodated by failing over to any of its physical standby databases. You can then designate the failover target database to take over the role of the LGM. 




[ Table 14-1 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-647FF758-5B53-44AE-BB80-B6FA506F09C7__CJAFAAHA) compares the characteristics of TGP standbys versus LGP standbys before and after a switchover operation. 

**Table: Trailing Group Physicals (TGP) Versus Leading Group Physicals (LGP)** 

Standby Type  |  Before Switchover  |  After Switchover  |  Notes   
---|---|---|---  
Trailing Group Physical (TGP)  |  Low apply lag  Lower data loss risk  |  High apply lag  Higher data loss risk  |  Can fail over to the primary role  Must flash back like the original primary   
Leading Group Physical (LGP)  |  High apply lag  Higher data loss risk  |  Low apply lag  Lower data loss risk  |  Can fail over to the transient logical standby role  Does not have to flash back like the original primary   
  
> **note:** See Also: 

  * [ *Oracle Database PL/SQL Packages and Types Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS73534) for a description of the ` DBMS_ROLLING ` PL/SQL package 

  * [ Unsupported Tables During Rolling Upgrades ](data-type-ddl-support-on-logical-standby-databases.md#GUID-13DDE926-B175-488D-9037-F707A4D2256D) for information about how to determine whether any of the tables involved in the upgrade contain data types that are unsupported when performing an upgrade using the ` DBMS_ROLLING ` PL/SQL package 

  * [ Additional PL/SQL Package Support Available Only in the Context of DBMS_ROLLING Upgrades ](data-type-ddl-support-on-logical-standby-databases.md#GUID-F54904C7-F666-4CC3-B1CB-493525476D2F) for information about PL/SQL packages that are supported only in the context of a ` DBMS_ROLLING ` upgrade 




####  DBMS_ROLLING Upgrades in a Multitenant Environment {#GUID-BC4E1CA8-4431-4FC0-8437-106C934E88EA} 

There are additional requirements and considerations when performing rolling upgrades in a multitenant environment. 

**Requirements Specific to DBMS_ROLLING Upgrades** 

The additional requirements when you use ` DBMS_ROLLING ` to perform a rolling upgrade in a multitenant environment are as follows. 

  * The TNS services referenced in the ` LOG_ARCHIVE_DEST_n ` parameters must be services that resolve to the root container of the destination database. The process assisting ` DBMS_ROLLING ` performs numerous operations which can only execute from the root container. 

  * All container databases on the transient logical standby must be plugged in and opened prior to calling ` DBMS_ROLLING.SWITCHOVER ` . This eliminates the possibility that the logical standby apply engine will halt because it cannot apply to a given PDB. 




See [ Example 14-6 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9__CJAFHDDH) for an example rolling upgrade using ` DBMS_ROLLING ` . 

**Additional Considerations for DBMS_ROLLING Upgrades** 

Installing, upgrading, or patching of applications installed in application containers is not supported while a ` DBMS_ROLLING ` upgrade is in progress. 

  * If DDL is executed to start the install, upgrade, or patching of an application container while a ` DBMS_ROLLING ` upgrade is in progress, then an error is returned. (The ` DBMS_ROLLING ` upgrade continues.) 

  * If an upgrade to an application container is in progress, then an attempt to start a ` DBMS_ROLLING ` upgrade results in an error. (The application container upgrade continues.) 

  * If a ` DBMS_ROLLING ` upgrade is performed and database compatibility is set to 12.2 or higher, then replication of application containers is supported. Except for ` DBMS_ROLLING ` upgrades, logical standby does not offer any support for application containers; such containers are skipped and a message is written to the alert log indicating that application containers are being skipped. 

  * The CDB that you are upgrading using ` DBMS_ROLLING ` can contain pluggable databases (PDBs) with different character sets. 




####  Coexistence of Oracle GoldenGate with Data Guard TLS Rolling Upgrades {#GUID-D617FB46-8701-41EF-B074-660245638099} 

Starting with Oracle Database 23ai, Application Continuity and the draining of database sessions are supported when performing a rolling upgrade using ` DBMS_ROLLING ` . This is the strategic approach for applying non-rolling patches and for performing major upgrades for the Oracle Autonomous Database and on-premises database deployments. It supports the upgrade from one major release to a newer major release, for example, from Oracle 19c to Oracle 23ai 

Previously, applications could not failover the session properly after the primary role transition from the former to the new primary database using ` DBMS_ROLLING.SWITCHOVER() ` . Now, application sessions implementing Application Continuity can keep working seamlessly, without downtime. 

The Transient Logical Standby (TLS) used by Oracle Active Data Guard Rolling Upgrades ( ` DBMS_ROLLING ` ) replicates the Oracle GoldenGate capture structures, making it possible to keep the GoldenGate capture processes running while the ` DBMS_ROLLING ` switchover occurs. 

Now, GoldenGate replication will appear to run uninterrupted while the capture sessions are migrated from the former to the new primary, with no more outage than is incurred by the rolling switchover and session reconnect. This is transparent to the users and particularly important for Autonomous Database on Dedicated Exadata Infrastructure, where ` DBMS_ROLLING ` is used underneath for upgrade operations. This new functionality does not alter how the ` DBMS_ROLLING ` package is executed. 

####  Data Guard Broker Support for DBMS_ROLLING Upgrades {#GUID-5DDF9197-33A4-4279-8058-46B3B8C0FAE4} 

As of Oracle Database 12 *c* Release 2 (12.2.0.1), Data Guard broker can remain on during a ` DBMS_ROLLING ` rolling upgrade; there is no longer any need to disable it. 

Keep the following in mind regarding Data Guard broker support for ` DBMS_ROLLING ` upgrades: 

  * Broker support is enabled by default during execution of the ` DBMS_ROLLING.BUILD_PLAN ` procedure if the broker is enabled at the time of the call. When broker support is enabled, the broker sets up the redo transport destinations as necessary from the original primary database as well as from the rolling upgrade target. 

  * The fast-start failover feature must be disabled before starting a ` DBMS_ROLLING ` upgrade. 

  * Any attempt to enable fast-start failover while a rolling upgrade is in progress is rejected. 

  * While a rolling upgrade is in progress, role changes are permissible only to the standby databases that are protecting the current primary database. The broker reports the role of the rolling upgrade target as ` Transient Logical Standby ` during a ` SHOW CONFIGURATION ` command as well as reporting the configuration status as ` ROLLING DATABASE MAINTENANCE IS IN PROGRESS ` . If there are standby databases protecting both the original primary and the upgrade target, then this topology is reflected when the ` SHOW CONFIGURATION ` command is issued from the current primary as well as from the upgrade target (before it has taken over as the primary database). 

  * The broker prevents a role change to a standby that is not protecting the current primary. This ensures that before the switchover phase, role changes to the Trailing Group Standby are allowed, and after the switchover phase, role changes are only allowed to the Leading Group Standbys. 

  * During the start of the upgrade process, if the upgrade target is an Oracle RAC database, then the broker automatically reduces the target standby to one instance and allows the upgrade to proceed. Without the broker, the start of the upgrade is rejected if it is found that the target has multiple instances running. 

  * The broker notifies Oracle Clusterware and Global Data Services as appropriate during the course of the rolling upgrade. 

  * Although role transitions are typically performed using the broker, the switchover step in a rolling upgrade should continue to be performed using the ` DBMS_ROLLING.SWITCHOVER ` procedure. 

  * Information about the status of a rolling upgrade being done using the PL/SQL package ` DBMS_ROLLING ` , is displayed in the output of the broker commands ` SHOW CONFIGURATION ` and ` SHOW DATABASE ` . 

The ` SHOW CONFIGURATION ` command shows Transient logical standby database as the role of the upgrade target, and ` ROLLING DATABASE MAINTENANCE IN PROGRESS ` as the configuration status. An example of this output is as follows: 
    
        ```
    Configuration - DRSolution
    
      Protection Mode: MaxPerformance
      Members:
      North_Sales - Primary database
        South_Sales - Transient logical standby database
    
    Fast-Start Failover: DISABLED
    
    Configuration Status:
       ROLLING DATABASE MAINTENANCE IN PROGRESS
    ```

The ` SHOW DATABASE ` command shows a ` WARNING ` with an appropriate ` ORA ` error for the upgrade target and the trailing or leading standbys, depending on the current rolling upgrade progress. An example of this output is as follows: 
    
        ```
    Database - South_Sales
    
      Role: Physical standby database
      Intended State: APPLY-ON
      Transport Lag: ***
      Apply Lag: ***
      Average Apply Rate: ***
      Real Time Query: OFF
      Instance(s):
        South
      Database Warning(s):
        ORA-16866: database converted to transient logical standby database for rolling
        database maintenance
    Database Status:
    WARNING
    ```

  * Starting with Oracle Database 21c, the ` JSON ` data type can be used with logical standby databases in the context of ` DBMS_ROLLING ` . The ` COMPATIBLE ` parameter must be set to 21.0 or higher. 



> **note:** See Also: 

  * [ DBMS_ROLLING ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS-GUID-097F1B39-E623-43B5-BA30-DF377BFE05CF)

  * [ SHOW CONFIGURATION ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-1A5D2697-D931-4A48-B54C-CAE223B37733)

  * [ SHOW DATABASE ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-9293F2C8-A7D2-442E-B147-A05A54DDE20E)




###  Overview of Using DBMS_ROLLING {#GUID-D42FC23B-C6F4-44D1-A46A-202AB845BB80} 

There are three stages to the rolling upgrade process using the ` DBMS_ROLLING ` PL/SQL Package: specification, compilation, and execution. 

  1. Specification  : Specify how the rolling upgrade will be implemented by designating a future primary database. This creates the initial leading and trailing groups. At this point the leading group contains only the LGM, but additional standby databases optionally can be added to protect the LGM. 

Use the following procedures during the specification phase: 

     * ` DBMS_ROLLING.INIT_PLAN `

     * ` DBMS_ROLLING.SET_PARAMETER `

  2. Compilation  : This is initiated by calling the ` DBMS_ROLLING.BUILD_PLAN ` procedure. The ` BUILD_PLAN ` procedure communicates with all databases participating in the rolling upgrade and assembles a rolling upgrade plan. The ` BUILD_PLAN ` procedure is also called to alter an existing rolling upgrade plan. Alterations are necessary after changes to ` DBMS_ROLLING ` parameters and after changes to the topology as a result of failover. All participating databases must be reachable during execution of the ` BUILD_PLAN ` procedure because numerous validations are required to ensure a successful rolling upgrade. 

  3. Execution  : Execution of the rolling upgrade has five stages. 

Stage 1:  The ` DBMS_ROLLING.START_PLAN ` procedure starts the execution of the rolling upgrade. This converts the LGM database to a logical standby and starts the SQL Apply process at the LGM. 

Stage 2:  Upgrade the database software at the databases that are part of the leading group. Next, run the upgrade scripts at the LGM. After this is done, restart SQL Apply processes at the LGM database. (See [ *Oracle Database Upgrade Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD-GUID-99E10023-B2E5-4B35-BD01-E21D5B912D50) for information about upgrade scripts.) Leading group physical standbys are also addressed during this stage by re-mounting them using the higher version binaries. These databases are upgraded via recovery of the redo from the LGM. 

Stage 3:  After the apply lag reaches a given threshold (set to 10 minutes by default, but can be configured during the specification stage), the ` DBMS_ROLLING.SWITCHOVER ` procedure proceeds with the switchover operation. When the switchover is complete, the LGM becomes the primary database. 

Stage 4:  The LGM is now the primary database running the new database software and the databases in the leading group are protecting it. The TGM is mounted and the databases in the trailing group are still running the older version of the database software. Prepare the TGM and TGS databases for upgrade by upgrading the database software and re-mounting the databases on the higher version binaries. (See [ *Oracle Database Upgrade Guide* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD-GUID-99E10023-B2E5-4B35-BD01-E21D5B912D50) for information about upgrade scripts.) 

Stage 5:  Execute the ` DBMS_ROLLING.FINISH_PLAN ` procedure at the current primary database (originally the LGM). It reinstates all the databases in the trailing group to become the standbys of the current primary database, and restarts the apply processes. The ` FINISH_PLAN ` procedure waits for all databases in the trailing group to be upgraded to the new release (although the database software for the trailing group databases was changed in Stage 4, the data dictionary of the trailing group databases, except for any logical standbys in the trailing group, are updated based on media recovery of the redo generated during the upgrade at the LGM database). 




After the rolling upgrade has been successfully executed, remove the rolling upgrade specification by calling the ` DBMS_ROLLING.DESTROY_PLAN ` procedure. 

###  Planning a Rolling Upgrade {#GUID-CD45406F-0A0F-4C9E-95FF-42C908BE62BE} 

Planning is essential to a successful rolling upgrade experience. During the planning phase, various upgrade parameters are specified and used to build an upgrade plan. 

The upgrade parameters specified during the planning phase help to identify all the operational details unique to the upgrade environment. The upgrade plan performs site-specific validations to identify potential configuration and resource problems that could disrupt the rolling upgrade. 

The tasks necessary to define upgrade parameters and build an upgrade plan are as follows: 

  * Initialize the upgrade parameters 

  * View the current upgrade parameter values 

  * Modify the upgrade parameter values, as necessary 

  * Build the upgrade plan 

  * View the current upgrade plan 

  * Revise the upgrade plan, as necessary 




The rest of this section describes each of these steps in detail. They must be performed in the order presented. 

  1. Plan parameters must be initialized to system-generated default values before they can be customized. To initialize plan parameters, call the ` DBMS_ROLLING.INIT_PLAN ` procedure. This procedure identifies the ` DB_UNIQUE_NAME ` of the future primary database (the leading group master or LGM). The LGM is converted into a logical standby database as part of the ` START_PLAN ` procedure call. The following is a sample call to the ` INIT_PLAN ` procedure in which ` boston ` is identified as the future primary database: 
    
        ```
    DBMS_ROLLING.INIT_PLAN(future_primary=>'boston');
    
    ```

The ` INIT_PLAN ` procedure returns an initial set of system-generated plan parameters. It adds each physical and logical standby database specified in the ` DG_CONFIG ` init.ora parameter as a participant in the rolling upgrade. Other databases (such as downstream databases serving GoldenGate downstream deployment or snapshot standbys) are excluded automatically. 

By default, standby databases other than the future primary are configured to protect the primary database, and are configured as mandatory participants in the rolling upgrade. 

Once the database-related parameters have been defined, the ` INIT_PLAN ` procedure defines operational parameters with system-supplied defaults. In most cases, the plan parameters are ready for plan validation, but be sure to review each parameter. 

Plan parameters are persisted in the database until the ` DESTROY_PLAN ` procedure is called to remove all states related to the rolling upgrade. 

  2. After the ` INIT_PLAN ` procedure has completed, query the ` DBA_ROLLING_PARAMETERS ` view to see the plan parameters and their current values. Plan parameters are either global or local in scope. Global parameters are attributes of the rolling upgrade as a whole and are independent of the database participants. Global parameters have a ` NULL ` value in the ` SCOPE ` column. Local parameters have a specific database name in the ` SCOPE ` column, with which they are associated. The following is a sample query: 
    
        ```
    SQL> select scope, name, curval from dba_rolling_parameters order by scope, name;
    
    SCOPE          NAME                         CURVAL
    -----------    ----------------             ------------------
    seattle        INVOLVEMENT                  FULL
    seattle        MEMBER                       NONE
    boston         INVOLVEMENT                  FULL
    boston         MEMBER                       TRAILING
    oakland        INVOLVEMENT                  FULL
    oakland        MEMBER                       TRAILING
    atlanta        INVOLVEMENT                  FULL
    atlanta        MEMBER                       LEADING
                   ACTIVE_SESSIONS_TIMEOUT     3600
                   ACTIVE_SESSIONS_WAIT        0
                   BACKUP_CONTROLFILE          rolling_change_backup.f
                   DGBROKER                    0
                   DICTIONARY_LOAD_TIMEOUT     3600
                   DICTIONARY_LOAD_WAIT        0
                   DICTIONARY_PLS_WAIT_INIT    300
                   DICTIONARY_PLS_WAIT_TIMEOUT 3600
                   EVENT_RECORDS               10000
                   FAILOVER                    0
                   GRP_PREFIX                  DBMSRU_
                   IGNORE_BUILD_WARNINGS       0
                   IGNORE_LAST_ERROR           0
                   LAD_ENABLED_TIMEOUT         600
                   LOG_LEVEL                   INFO
                   READY_LGM_LAG_TIME          600
                   READY_LGM_LAG_TIMEOUT       60
                   READY_LGM_LAG_WAIT          0
                   SWITCH_LGM_LAG_TIME         600
                   SWITCH_LGM_LAG_TIMEOUT      60
                   SWITCH_LGM_LAG_WAIT         1
                   SWITCH_LGS_LAG_TIME         60
                   SWITCH_LGS_LAG_TIMEOUT      60
                   SWITCH_LGS_LAG_WAIT         0
                   UPDATED_LGS_TIMEOUT         10800
                   UPDATED_LGS_WAIT            1
                   UPDATED_TGS_TIMEOUT         10800
                   UPDATED_TGS_WAIT            1
    35 rows selected.
    ```

In the sample output, the databases ` atlanta ` , ` boston ` , ` oakland ` , and ` seattle ` were all discovered through the ` DG_CONFIG ` , and assigned parameters in the current plan. 

> **note:** See Also: 
     * [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN23889) for more information about the ` DBA_ROLLING_PARAMETERS ` view 

  3. To modify any existing rolling upgrade parameter, use the ` DBMS_ROLLING.SET_PARAMETER ` PL/SQL procedure. 

Starting with Oracle Database Release 21c, operations that are not supported by transient logical standbys can be blocked from running on the primary database. The ` BLOCK_UNSUPPORTED ` parameter of the ` DBMS_ROLLING.SET_PARAMETER ` procedure is used to specify whether unsupported operations must be blocked. The default setting is to block unsupported operations. 

The following is an example of using the ` SET_PARAMETER ` procedure: 
    
        ```
    DBMS_ROLLING.SET_PARAMETER(
      scope IN VARCHAR2,
      name IN VARCHAR2,
      value IN VARCHAR2);
    
    ```

The scope identifies either a ` DB_UNIQUE_NAME ` value for local parameters or ` NULL ` for global parameters. It is not necessary to provide a scope of ` NULL ` for parameters that are not specific to a database. 

The name is the name of the parameter to modify. 

The value identifies the value for the specified parameter. A value of NULL reverts the parameter back to its system-supplied default if one exists. 

> **note:** See Also: 
     * [ *Oracle Database PL/SQL Packages and Types Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=ARPLS74605) for a complete list of all available rolling upgrade parameters 

The following examples illustrate sample usage of some rolling upgrade parameters. 

  4. After all the necessary parameters are specified, build an upgrade plan. An upgrade plan is a custom generated set of instructions which guides the Oracle Data Guard configuration through a rolling upgrade. (The plan that is generated differs based on whether or not the configuration is managed by Data Guard broker.) 

To build an upgrade plan, use the ` DBA_ROLLING.BUILD_PLAN ` PL/SQL procedure. This procedure requires the configuration to be exactly as described by the plan parameters with all of the instances started and reachable through the network. 

The procedure is called as follows: 
    
        ```
    DBMS_ROLLING.BUILD_PLAN;
    
    ```

There are no arguments to specify because the procedure gets all its input from the ` DBA_ROLLING_PARAMETERS ` view. The procedure validates plan parameters and performs site-specific validations of resources such as log transport and flash recovery area settings. In general, configuration settings that do not meet the criteria of best-practice values are treated as warnings and recorded in the ` DBA_ROLLING_EVENTS ` view. By default, the ` IGNORE_BUILD_WARNINGS ` parameter is set to ` 1 ` , meaning warnings do not prevent an upgrade plan from reaching a usable state. Set this parameter to ` 0 ` for stricter rule enforcement when plans are built. 

> **note:** 

The validations performed during plan generation are specific to rolling upgrades. They are not a substitute for the recommended practice of running the Pre-Upgrade Information Tool to evaluate upgrade readiness. 

After generating the plan, move on to the following steps to view it, diagnose any problems with it, and revise it if necessary. 

  5. After the ` BUILD_PLAN ` procedure successfully returns, the complete upgrade plan is viewable in the ` DBA_ROLLING_PLAN ` view. Each record in the view identifies a specific instruction that is scheduled for execution. 

The following output is an example of how a rolling upgrade plan would appear: 
    
        ```
    SQL> SELECT instid, target, phase, description FROM DBA_ROLLING_PLAN;
    
    INSTID TARGET       PHASE   DESCRIPTION
    ------ ------------ ------- -----------------------------------------------------
         1 seattle      START   Verify database is a primary
         2 seattle      START   Verify MAXIMUM PROTECTION is disabled
         3 boston       START   Verify database is a physical standby
         4 boston       START   Verify physical standby is mounted
         5 oakland      START   Verify database is a physical standby
         6 oakland      START   Verify physical standby is mounted
         7 atlanta      START   Verify database is a physical standby
         8 atlanta      START   Verify physical standby is mounted
         9 seattle      START   Verify server parameter file exists and is modifiable
        10 boston       START   Verify server parameter file exists and is modifiable
        11 oakland      START   Verify server parameter file exists and is modifiable
        12 atlanta      START   Verify server parameter file exists and is modifiable
        13 seattle      START   Verify Data Guard Broker configuration is disabled
        14 boston       START   Verify Data Guard Broker configuration is disabled
        15 oakland      START   Verify Data Guard Broker configuration is disabled
        16 atlanta      START   Verify Data Guard Broker configuration is disabled
        17 seattle      START   Verify flashback database is enabled
        18 seattle      START   Verify available flashback restore points
        19 boston       START   Verify flashback database is enabled
        20 boston       START   Verify available flashback restore points
        21 oakland      START   Verify flashback database is enabled
        22 oakland      START   Verify available flashback restore points
        23 atlanta      START   Verify flashback database is enabled
        24 atlanta      START   Verify available flashback restore points
        25 boston       START   Scan LADs for presence of atlanta destination
        26 boston       START   Test if atlanta is reachable using configured TNS service
        27 boston       START   Stop media recovery
        28 oakland      START   Stop media recovery
        29 atlanta      START   Stop media recovery
        30 boston       START   Drop guaranteed restore point DBMSRU_INITIAL
        31 boston       START   Create guaranteed restore point DBMSRU_INITIAL
        32 oakland      START   Drop guaranteed restore point DBMSRU_INITIAL
        33 oakland      START   Create guaranteed restore point DBMSRU_INITIAL
        34 atlanta      START   Drop guaranteed restore point DBMSRU_INITIAL
        35 atlanta      START   Create guaranteed restore point DBMSRU_INITIAL
        36 seattle      START   Drop guaranteed restore point DBMSRU_INITIAL
        37 seattle      START   Create guaranteed restore point DBMSRU_INITIAL
    
    INSTID TARGET       PHASE   DESCRIPTION
    ------ ------------ ------- ----------------------------------------------------------
        38 boston       START   Start media recovery
        39 boston       START   Verify media recovery is running
        40 oakland      START   Start media recovery
        41 oakland      START   Verify media recovery is running
        42 atlanta      START   Start media recovery
        43 atlanta      START   Verify media recovery is running
        44 seattle      START   Verify user_dump_dest has been specified
        45 seattle      START   Backup control file to rolling_change_backup.f
        46 boston       START   Verify user_dump_dest has been specified
        47 boston       START   Backup control file to rolling_change_backup.f
        48 oakland      START   Verify user_dump_dest has been specified
        49 oakland      START   Backup control file to rolling_change_backup.f
        50 atlanta      START   Verify user_dump_dest has been specified
        51 atlanta      START   Backup control file to rolling_change_backup.f
        52 seattle      START   Get current redo branch of the primary database
        53 boston       START   Wait until recovery is active on the primary's redo branch
        54 boston       START   Stop media recovery
        55 seattle      START   Execute dbms_logstdby.build
        56 boston       START   Convert into a transient logical standby
        57 boston       START   Open database
        58 boston       START   Configure logical standby parameters
        59 boston       START   Start logical standby apply
        60 boston       START   Get redo branch of transient logical standby
        61 boston       START   Get reset scn of transient logical redo branch
        62 atlanta      START   Stop media recovery
        63 atlanta      START   Flashback database
        64 seattle      START   Disable log file archival to atlanta
        65 boston       START   Enable log file archival to atlanta
        66 boston       START   Wait for log archive destination to atlanta to reach a valid state
        67 atlanta      START   Wait until transient logical redo branch has been registered
        68 atlanta      START   Start media recovery
        69 atlanta      START   Wait until v$dataguard_stats has been initialized
        70 atlanta      START   Wait until recovery has started on the transient redo branch
        71 seattle      START   Log pre-switchover instructions to events table
        72 boston       START   Record start of user upgrade of boston
        73 boston       SWITCH  Verify database is in OPENRW mode
        74 boston       SWITCH  Record completion of user upgrade of boston
    
    INSTID TARGET       PHASE   DESCRIPTION
    ------ ------------ ------- ---------------------------------------------------------
        75 boston       SWITCH  Scan LADs for presence of seattle destination
        76 boston       SWITCH  Scan LADs for presence of oakland destination
        77 boston       SWITCH  Scan LADs for presence of atlanta destination
        78 boston       SWITCH  Test if seattle is reachable using configured TNS service
        79 boston       SWITCH  Test if oakland is reachable using configured TNS service
        80 boston       SWITCH  Test if atlanta is reachable using configured TNS service
        81 seattle      SWITCH  Enable log file archival to boston
        82 boston       SWITCH  Enable log file archival to atlanta
        83 boston       SWITCH  Start logical standby apply
        84 atlanta      SWITCH  Start media recovery
        85 atlanta      SWITCH  Wait until upgrade redo has been fully recovered
        86 boston       SWITCH  Wait until apply lag has fallen below 600 seconds
        87 seattle      SWITCH  Log post-switchover instructions to events table
        88 seattle      SWITCH  Switch database to a logical standby
        89 boston       SWITCH  Wait until end-of-redo has been applied
        90 oakland      SWITCH  Wait until end-of-redo has been applied
        91 seattle      SWITCH  Disable log file archival to oakland
        92 boston       SWITCH  Switch database to a primary
        93 oakland      SWITCH  Stop media recovery
        94 seattle      SWITCH  Synchronize plan with new primary
        95 seattle      FINISH  Verify only a single instance is active
        96 seattle      FINISH  Verify database is mounted
        97 seattle      FINISH  Flashback database
        98 seattle      FINISH  Convert into a physical standby
        99 oakland      FINISH  Verify database is mounted
       100 oakland      FINISH  Flashback database
       101 boston       FINISH  Verify database is open
       102 boston       FINISH  Save the DBID of the new primary
       103 boston       FINISH  Save the logminer session start scn
       104 seattle      FINISH  Wait until transient logical redo branch has been registered
       105 oakland      FINISH  Wait until transient logical redo branch has been registered
       106 seattle      FINISH  Start media recovery
       107 oakland      FINISH  Start media recovery
       108 seattle      FINISH  Wait until apply/recovery has started on the transient branch
       109 oakland      FINISH  Wait until apply/recovery has started on the transient branch
       110 seattle      FINISH  Wait until upgrade redo has been fully recovered
    
    INSTID TARGET       PHASE   DESCRIPTION
    ------ ------------ ------- ------------------------------------------------
       111 oakland      FINISH  Wait until upgrade redo has been fully recovered
       112 seattle      FINISH  Drop guaranteed restore point DBMSRU_INITIAL
       113 boston       FINISH  Drop guaranteed restore point DBMSRU_INITIAL
       114 oakland      FINISH  Drop guaranteed restore point DBMSRU_INITIAL
       115 atlanta      FINISH  Drop guaranteed restore point DBMSRU_INITIAL
    
    115 rows selected.
    
    SQL> 
    
    ```

The columns in this view display the following information: 

     * ` INSTID ` \- The Instruction ID, which is the order in which the instruction is to be performed. Instructions are typically performed in groups. 

     * ` PHASE ` \- Every instruction in the upgrade plan is associated with a particular phase. A phase is a logical grouping of instructions which is performed by a procedure in the ` DBMS_ROLLING ` PL/SQL package. When a ` DBMS_ROLLING ` procedure is invoked, all of the associated instructions in the upgrade plan for that phase are executed. Possible phases are as follows: 

       * ` START ` : Consists of activities related to setup such as taking restore points, instantiation of the transient logical standby database, and configuration of LGS databases. Activities in this phase are initiated when the ` DBMS_ROLLING.START_PLAN ` procedure is called. See Step 1 in  " [ Performing a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6) "  . 

       * ` SWITCH ` : Consists of activities related to the switchover of the transient logical standby into the new primary database. Activities in this phase are initiated when the ` DBMS_ROLLING.SWITCHOVER ` procedure is called. See Step 3 in  " [ Performing a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6) " 

       * ` FINISH ` : Consists of activities related to configuring standby databases for recovery of the upgrade redo. Activities in this phase are initiated when the ` DBMS_ROLLING.FINISH_PLAN ` procedure is called. See Step 5 in  " [ Performing a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6) "  . 

     * ` EXEC_STATUS ` \- The overall status of the instruction. 

     * ` PROGRESS ` \- The progress of an instruction's execution. A value of ` REQUESTING ` indicates an instruction is being transmitted to a target database for execution. A value of ` EXECUTING ` indicates the instruction is actively being executed. A value of ` REPLYING ` indicates completion information is being returned. 

     * ` DESCRIPTION ` \- The specific operation that is scheduled to be performed. 

     * ` TARGET ` \- The site at which a given instruction is to be performed. 

     * ` EXEC_INFO ` \- Additional contextual information related to the instruction. 

> **note:** See Also: 
     * [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN23890) for more information about the ` DBA_ROLLING_PLAN ` view 

  6. Upgrade plans need to be revised after any change to the rolling upgrade or database configuration. A configuration change could include any of the following: 

     * init.ora parameter file changes at any of the databases participating in the rolling upgrade 

     * database role changes as a result of failover events 

     * rolling upgrade parameter changes 

To revise an active upgrade plan, call the ` BUILD_PLAN ` procedure again. In some cases, the ` BUILD_PLAN ` procedure may raise an error if a given change cannot be accepted. For example, setting the ` ACTIVE_SESSIONS_WAIT ` parameter has no effect if the switchover has already occurred. 

It is recommended that the ` BUILD_PLAN ` procedure be called to process a group of parameter changes rather than processing parameters individually. 




Example 14-1 Setting Switchover to Enforce Apply Lag Requirements 

The following example demonstrates how to configure the plan to wait for the apply lag to fall below 60 seconds before switching over to the future primary: 
    
    
    ```
    DBMS_ROLLING.SET_PARAMETER('SWITCH_LGM_LAG_WAIT', '1');
    DBMS_ROLLING.SET_PARAMETER('SWITCH_LGM_LAG_TIME', '60');
    ```

Example 14-2 Resetting Logging Back to Its Default Value 

The following example demonstrates resetting the ` LOG_LEVEL ` global parameter back to its default value. 
    
    
    ```
    DBMS_ROLLING.SET_PARAMETER (
      name=>'LOG_LEVEL', 
      value=>NULL);
    ```

Example 14-3 Designating a Database as an Optional Participant 

The following example demonstrates setting the ` INVOLVEMENT ` local parameter of database ` atlanta ` to indicate that errors encountered on the database should not impede the overall rolling upgrade. 
    
    
    ```
    DBMS_ROLLING.SET_PARAMETER (
      scope=>'atlanta', 
      name=>'involvement', 
      value=>'optional');
    ```

Example 14-4 Setting a Database to Protect the Transient Logical Standby 

The following example demonstrates setting the ` MEMBER ` local parameter of database ` atlanta ` to indicate it should protect the transient logical standby database during the rolling upgrade. 
    
    
    ```
    DBMS_ROLLING.SET_PARAMETER (
      scope=>'atlanta', 
      name=>'member', 
      value=>'leading');
    
    ```

Example 14-5 Blocking Operations Not Supported by Transient Logical Standbys 

The following example demonstrates setting the ` BLOCK_UNSUPPORTED ` parameter of the primary database ` seattle ` to 1. This setting blocks operations that are not supported by transient logical standbys from being run on the primary database. Unsupported operations can include DMLs and DDLs to tables contained in the ` DBA_ROLLING_UNSUPPORTED ` view, DDLs that are not replicated by transient logical standbys, and invocations to PL/SQL procedures that contain unsupported tables. The ` COMPATIBLE ` parameter must be set to 21.0 or higher to use this parameter. 
    
    
    ```
    DBMS_ROLLING.SET_PARAMETER (
          scope=>'seattle',
          name=>'BLOCK_UNSUPPORTED',
          value=>'1');
    ```

###  Performing a Rolling Upgrade {#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6} 

Follow these steps to perform a rolling upgrade using the ` DBMS_ROLLING ` PL/SQL package. 

[ Table 14-2 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6__CJAJCACI) provides a summary of the steps. These steps assume that you have first successfully built an upgrade plan as described in  " [ Planning a Rolling Upgrade ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-CD45406F-0A0F-4C9E-95FF-42C908BE62BE) "  . 

**Table: Steps to Perform Rolling Upgrade Using DBMS_ROLLING** 

Step  |  Description  |  PHASE   
---|---|---  
Step 1  |  Call the ` DBMS_ROLLING.START_PLAN ` procedure to configure the future primary and physical standbys designated to protect the future primary.  |  START   
Step 2  |  Manually upgrade the Oracle Database software at the future primary database and standbys that protect it.  |  SWITCH PENDING   
Step 3  |  Call the ` DBMS_ROLLING.SWITCHOVER ` procedure to switch roles between the current and future primary database.  |  SWITCH   
Step 4  |  Manually restart the former primary and remaining standby databases on the higher version of Oracle Database.  |  FINISH PENDING   
Step 5  |  Call the ` DBMS_ROLLING.FINISH_PLAN ` procedure to convert the former primary to a physical standby, and to configure the remaining standby databases for recovery of the upgrade redo.  |  FINISH   
  
Activities that take place during each step belong to a specific phase of the rolling upgrade as shown in the PHASE column of [ Table 14-2 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-5652ED69-A95F-454A-9208-E2483B9A9BA6__CJAJCACI) . A rolling upgrade operation is at a single phase at any given time. The current phase of a rolling upgrade is reported in the ` PHASE ` column of the ` DBA_ROLLING_STATUS ` view. 

The rest of this section describes each of the upgrade steps in detail. 

  1. Call the ` DBMS_ROLLING.START_PLAN ` procedure to configure the future primary and physical standbys designated to protect the future primary. 

The ` DBMS_ROLLING.START_PLAN ` procedure is the formal start of the rolling upgrade. The goal of the ` START_PLAN ` procedure is to configure the transient logical standby database and any physical standby databases that have been designated to protect it. When invoked, the ` START_PLAN ` procedure executes all instructions in the upgrade plan with a ` PHASE ` value of ` START_PLAN ` . The types of instructions that are performed include: 

     * Backing up the control file for each database to a trace file 

     * Creating flashback database guaranteed restore points 

     * Building a LogMiner dictionary at the primary database 

     * Recovering the designated physical standby into a transient logical standby database 

     * Loading the LogMiner dictionary into the logical standby database 

     * Configuring LGS databases with the transient logical standby database 

Call the ` START_PLAN ` procedure as follows (no arguments are required): 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.START_PLAN;
    
    ```

  2. Manually upgrade the Oracle Database software at the future primary database and standbys that protect it. 

After the ` START_PLAN ` procedure has completed, you must manually upgrade the Oracle Database software at the future primary database and standbys which protect the future primary database. This involves the following steps: 

    1. Upgrade the Oracle Database software of the transient logical (LGM) and leading group standbys (LGS). 

    2. Start media recovery on the LGS databases. 

    3. Upgrade the transient logical standby database either manually or using the AutoUpgrade utility. 

    4. Re-open the transient logical standby in read/write mode. 

The transient logical standby and LGS databases are a functional group. The LGS databases must be restarted on the higher version actively running media recovery before the transient logical standby is upgraded. If the LGS databases are not configured first, then the upgrade of the transient logical is not protected. At the conclusion of this step, the upgrade of the transient logical is complete, and media recovery is running on all LGS databases. 

It is recommended that you wait until all LGS databases have been fully upgraded before performing the switchover. An LGS database is fully upgraded when its associated record in the ` DBA_ROLLING_DATABASES ` view reports a value of ` YES ` in the ` UPDATED ` column. 

  3. Call the ` DBMS_ROLLING.SWITCHOVER ` procedure to switch roles between the current and future primary database. 

The ` SWITCHOVER ` procedure switches roles between the current and future primary databases. The procedure times the switchover to occur when apply lag is minimal which minimizes outage time of the primary service. The ` SWITCHOVER ` procedure executes all instructions in the upgrade plan with a ` PHASE ` value of ` SWITCHOVER ` . The types of instructions that are performed can include: 

     * Waiting for the apply lag at the Leading Group Master (LGM), which is currently the transient logical standby, to fall below a threshold value 

     * Waiting for the apply lag at LGS databases to fall below a threshold value 

     * Switching the primary to the logical standby role 

     * Switching the Leading Group Master (LGM), which is currently a logical standby, to the primary role 

     * Enabling log archive destinations at the Leading Group Master (LGM) after it has become the new primary 

Call the ` SWITCHOVER ` procedure as follows (no arguments are required): 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SWITCHOVER;
    
    ```

If a switchover error occurs after the switchover of the primary to the standby role but before the transient logical could be successfully converted into the primary role, then continue to execute the ` SWITCHOVER ` procedure at the former primary site until successful completion. 

  4. At this point, you must manually restart and mount the former primary and remaining standby databases on the higher version of Oracle Database. Mounting the standby databases is especially important because the ` DBMS_ROLLING ` package needs to communicate with the standby database to continue the rolling upgrade. 

  5. The overall goal of the ` FINISH_PLAN ` procedure is to configure the former primary and TGP standbys as physical standbys which recover through the upgrade redo. When invoked, the ` FINISH_PLAN ` procedure executes all instructions in the upgrade plan with a ` PHASE ` value of ` FINISH ` . The types of instructions that are performed include: 

     * Flashback of the former primary and TGP standbys 

     * Conversion of the former primary into a physical standby 

     * Startup of media recovery on the new redo branch 

Call the ` FINISH_PLAN ` procedure as follows (no arguments are required): 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.FINISH_PLAN;
    ```




###  Monitoring a Rolling Upgrade {#GUID-A1348F5A-AFBC-4090-822F-23DF87CE7D3C} 

There are several views you can query for information about the databases involved in a rolling upgrade. 

  * ` DBA_ROLLING_STATUS `

Provides information about the overall status of the upgrade. 

  * ` DBA_ROLLING_DATABASES `

Provides information about the role, protection, and recovery state of each database involved in the rolling upgrade. 

  * ` DBA_ROLLING_STATISTICS `

Provides statistics such as start and finish times, how long services were offline, and so on. 




> **note:** See Also: 

  * [ *Oracle Database Reference* ](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=REFRN002) for descriptions of these views 




###  Rolling Back a Rolling Upgrade {#GUID-5B34621B-FABE-4C50-A2C0-ED249844934D} 

To roll back a rolling upgrade procedure, you can call the ` DBMS_ROLLING.ROLLBACK_PLAN ` procedure. 

The procedure is called as follows: 
    
    
    ```
    DBMS_ROLLING.ROLLBACK_PLAN;
    
    ```

The ` ROLLBACK_PLAN ` procedure has the following requirements: 

  * The ` ROLLBACK_PLAN ` procedure can only be called if the ` DBMS_ROLLING.SWITCHOVER ` procedure has not been previously called. 

  * Before you can use the ` ROLLBACK_PLAN ` procedure you must set the transient logical standby database back to a mounted state because a flashback database is imminent. 

  * If the Oracle Database software was already upgraded, then you must restart the resultant physical standbys on the older version, and start media recovery. 




> **note:** For ` DBMS_ROLLING.ROLLBACK_PLAN ` to execute successfully, the transient logical standby must be in a mounted state. 

###  Handling Role Changes That Occur During a Rolling Upgrade {#GUID-1019E58C-1370-48A6-BC67-FA42074C0351} 

If a situation arises in which a rolling upgrade is underway and you need to perform a failover in your Oracle Data Guard configuration before the rollover completes, you can do so only in these circumstances. 

  * The failover was not performed while a ` DBMS_ROLLING ` procedure was in progress. 

  * The failover was between a primary database and a physical standby database, and was a no-data-loss failover. 

  * The failover was between a transient logical standby database and a physical standby of the transient logical standby database. 




A role change is a significant event that inevitably invalidates instructions in the upgrade plan, which was tailored for a different configuration. To resume the rolling upgrade, a new plan must be created. You must set the ` FAILOVER ` parameter to indicate that the configuration has changed. This parameter is detected on the next invocation of the ` BUILD_PLAN ` procedure, and the existing plan is amended accordingly. 

After the revised plan is built, you can resume the rolling upgrade. 

###  Examples of Rolling Upgrades {#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9} 

These examples show a variety of rolling upgrade scenarios. 

At some point in all of the scenarios, the same basic rolling upgrade steps are used. These steps are shown in [ Example 14-6 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9__CJAFHDDH) . The rest of the examples refer back to this example where appropriate rather than reiterating the same steps. 

Some of the examples in this section instruct you to resume the rolling upgrade, which means that you should continue where you left off. Resuming a rolling upgrade involves identifying the current phase of the rolling upgrade and reperforming either the PL/SQL procedure associated with the phase or the activities relevant to the phase. The current phase of the rolling upgrade is shown in the ` PHASE ` column of the ` DBA_ROLLING_STATUS ` view. 

> **note:** 

The scenarios provided in this section are only meant to be hypothetical examples. You can use the Rolling Upgrade Using Oracle Active Data Guard feature to perform database upgrades beginning with the first Oracle Database 12 *c* patchset. 

Example 14-6 Basic Rolling Upgrade Steps 

  1. Start the rolling upgrade: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.START_PLAN;
    
    ```

  2. Upgrade the transient logical standby and its protecting standbys. 

    1. Mount LGP standbys using the higher Oracle Database software version. 

    2. Start media recovery on Leading Group Physicals (LGP). 

    3. Open the Leading Group Master (LGM), which is the transient logical standby, in upgrade mode using the higher Oracle Database software version. 

    4. Upgrade the Leading Group Master (LGM), which is the transient logical standby, either manually or using the AutoUpdate utility. 

    5. Restart the Leading Group Master (LGM), which is the transient logical standby, in read/write mode. 

  3. Switchover to the Leading Group Master (LGM): 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SWITCHOVER;
    
    ```

  4. Restart the databases in the trailing group. This includes the original primary database and all its protecting standbys in the trailing group (TGP). 

    1. Mount the former primary using the higher Oracle Database version. 

    2. Mount the physical standbys of the former primary using the higher Oracle Database version. 

  5. Finish the rolling upgrade: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.FINISH_PLAN;
    ```




Example 14-7 Rolling Upgrade Between Two Databases 

The following example demonstrates a rolling upgrade on a two-site configuration consisting of a primary database and a physical standby database. In this example, ` seattle ` is the current primary and ` boston ` is the future primary. The ` seattle ` database is automatically chosen as the Trailing Group Master (TGM) and participates in the operation. By default, there is nothing that needs to be set for ` seattle ` . 

  1. Initialize the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.INIT_PLAN(future_primary=>'boston');
    
    ```

  2. Build the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  3. Perform the rolling upgrade as described in [ Example 14-6 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9__CJAFHDDH) . 




Example 14-8 Rolling Upgrade Between Three Databases 

The following example demonstrates a rolling upgrade on a three-site configuration consisting of a primary databases and two physical standby databases. In this example, ` seattle ` is the primary, ` boston ` is the future primary, and ` oakland ` is a physical standby of ` seattle ` . 

  1. Initialize the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.INIT_PLAN (future_primary => 'boston');
    
    ```

  2. Build the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  3. Perform the rolling upgrade as described in [ Example 14-6 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9__CJAFHDDH) . 




Example 14-9 Rolling Upgrade Between Four Databases 

The following example demonstrates a rolling upgrade on a four-site configuration consisting of a primary database and three physical standby databases. In this example, ` seattle ` is the primary database, ` boston ` is the future primary, ` oakland ` is a physical standby of ` seattle ` , and ` atlanta ` is a physical standby of ` boston ` . 

  1. Initialize the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.INIT_PLAN (future_primary => 'boston');
    
    ```

  2. Configure ` atlanta ` as a standby in the leading group: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(scope=>'atlanta',name=>'member',
     value=>'leading');
    
    ```

  3. Build the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  4. Perform the rolling upgrade as described in [ Example 14-6 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9__CJAFHDDH) . 




Example 14-10 Rolling Upgrade on a Reader Farm 

The following example demonstrates a rolling upgrade on a reader farm configuration consisting of one primary database and nine physical standby databases. In this example, eight physical standby databases are split into two groups of four in order for physical standbys to be available as Oracle Active Data Guard standbys before and after the switchover. In this example, ` seattle ` is the primary, ` boston ` is the future primary, databases ` rf[a-d] ` are physical standbys of ` seattle ` , and databases ` rf[e-h] ` are physical standbys of ` boston ` . The rolling upgrade is configured so that the switchover to the new primary waits until the apply lag among the reader farm group of the future primary database is less than 60 seconds. 

  1. Initialize the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.INIT_PLAN ( future_primary => 'boston');
    
    ```

  2. Configure the reader farm group to protect the future primary: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(scope=>'rfe',name=>'member',
    value=>'leading');
    
    ```
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(scope=>'rff',name=>'member',
    value=>'leading');
    
    ```
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(scope=>'rfg',name=>'member',
    value=>'leading');
    
    ```
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(scope=>'rfh',name=>'member',
    value=>'leading');
    
    ```

  3. Set a maximum permitted apply lag of 60 seconds on the future primary's reader farm: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(name=>'SWITCH_LGS_LAG_WAIT',
     value=>'1');
    
    ```

  4. Build the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  5. Perform the rolling upgrade as described in [ Example 14-6 ](using-DBMS_ROLLING-to-perform-rolling-upgrade.md#GUID-3D3BDB04-0981-4E3A-88FD-B1C0B9BDD0E9__CJAFHDDH) . 




Example 14-11 Rolling Upgrade for Application Testing 

The following example demonstrates using rolling upgrade on a four-site configuration to configure a transient logical standby and a physical of the transient logical standby in order to validate an application on the higher version database. The primary database is ` seattle ` , ` boston ` is the future primary, ` oakland ` is a physical standby of ` seattle ` , and ` atlanta ` is physical standby of ` boston ` . So in this example, ` seattle ` and ` oakland ` make up the trailing group, and ` boston ` and ` atlanta ` make up the leading group. At the end of testing, ` boston ` and ` atlanta ` are restored back to their original physical standby roles in order to resume protection of ` seattle ` . 

  1. Initialize the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.INIT_PLAN (future_primary => 'boston');
    
    ```

  2. Configure ` atlanta ` to protect the future primary: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(scope=>'atlanta',name=>'member',
     value=>'leading');
    
    ```

  3. Build the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  4. Start the rolling upgrade: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.START_PLAN;
    
    ```

  5. Upgrade ` boston ` and ` atlanta ` : 

    1. Mount ` atlanta ` using the higher database version. 

    2. Start media recovery on ` atlanta ` . 

    3. Open ` boston ` in upgrade mode using the higher database version. 

    4. Upgrade database ` boston ` either manually or using the Database Upgrade Assistant (DBUA). 

    5. Restart ` boston ` in read/write mode. 

  6. Test the application, as necessary. 

  7. Rollback the configuration: 

    1. Restart ` boston ` in mounted mode 

    2. Roll back the upgrade: 
        
                ```
        SQL> EXECUTE DBMS_ROLLING.ROLLBACK_PLAN;
        
        ```

  8. Start media recovery on ` boston ` and ` atlanta ` using the older database version: 

    1. Mount ` boston ` and ` atlanta ` using the lower database version. 

    2. Start media recovery on ` boston ` and ` atlanta ` . 




> **note:** For ` DBMS_ROLLING.ROLLBACK_PLAN() ` to execute successfully, the pre-requisite is that it needs the transient logical standby to be in mounted state. 

Example 14-12 Resuming a Rolling Upgrade After a Failover to a New Primary 

The following example demonstrates a no-data-loss failover of a physical standby to the primary role followed by the reconfiguration of the rolling upgrade plan on a three-site configuration. In this example, ` seattle ` is the primary, ` boston ` is the future primary, and ` oakland ` is a physical standby of ` seattle ` . Database ` oakland ` is failed over to become the new primary. (The Trailing Group is ( ` seattle ` , ` oakland ` ) and the Leading Group is ` boston ` .) 

  1. Recover remaining redo on ` oakland ` , and fail over to the new primary role: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY FINISH;
    SQL> ALTER DATABASE FAILOVER TO OAKLAND;
    SQL> ALTER DATABASE OPEN;
    ```

  2. Configure log archive destinations on ` oakland ` , as necessary: 
    
        ```
    SQL> ALTER SYSTEM SET LOG_ARCHIVE_DEST_2='service="boston" reopen=5
      2  LGWR ASYNC NET_TIMEOUT=180 valid_for=(ONLINE_LOGFILE, PRIMARY_ROLE)
      3  DB_UNIQUE_NAME="oakland"';
    ```
    
        ```
    
    SQL> ALTER SYSTEM ARCHIVE LOG CURRENT;
    ```

  3. Set a parameter to indicate that a failover took place: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(name=>'failover', value=>'1');
    
    ```

  4. Revise the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  5. Resume the rolling upgrade. 




Example 14-13 Resuming a Rolling Upgrade After a Failover to a New Transient Logical 

The following example demonstrates a failover of a physical standby to the transient logical role, followed by the reconfiguration of the rolling upgrade plan on a five-site configuration. In this example, ` seattle ` is the primary, ` boston ` is the future primary, ` oakland ` is a physical standby of ` seattle ` , and ` atlanta ` and ` miami ` are physical standbys of ` boston ` . Database ` atlanta ` is failed over to become the new transient logical standby. 

  1. Recover remaining redo on ` atlanta ` and failover to the new transient logical role: 
    
        ```
    SQL> ALTER DATABASE RECOVER MANAGED STANDBY FINISH;
    
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;
    
    SQL> ALTER DATABASE OPEN;
    
    ```

  2. Configure log archive destinations on ` atlanta ` , as necessary: 
    
        ```
    SQL> alter system set log_archive_dest_2='service="seattle" reopen=5
      2  LGWR ASYNC NET_TIMEOUT=180 valid_for=(ONLINE_LOGFILE, PRIMARY_ROLE)
      3  DB_UNIQUE_NAME="atlanta"';
    
    SQL> alter system set log_archive_dest_3='service="oakland" reopen=5
      2  LGWR ASYNC NET_TIMEOUT=180 valid_for=(ONLINE_LOGFILE, PRIMARY_ROLE)
      3  DB_UNIQUE_NAME="atlanta"';
    
    SQL> alter system set log_archive_dest_4='service="miami" reopen=5
      2  LGWR ASYNC NET_TIMEOUT=180 valid_for=(ONLINE_LOGFILE, ALL_ROLES)
      3  DB_UNIQUE_NAME="atlanta"';
    
    ```

  3. Specify ` atlanta ` as the new transient logical standby database: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.SET_PARAMETER(name=>'failover', value=>'1');
    
    ```

  4. Revise the upgrade plan: 
    
        ```
    SQL> EXECUTE DBMS_ROLLING.BUILD_PLAN;
    
    ```

  5. Resume the rolling upgrade. 



