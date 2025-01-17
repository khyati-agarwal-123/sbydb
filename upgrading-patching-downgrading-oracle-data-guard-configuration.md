## BPatching, Upgrading, and Downgrading Databases in an Oracle Data Guard Configuration {#GUID-A4FBE490-7E7E-4998-B80E-01C93C77AE2F}

These topics address how to upgrade and downgrade an Oracle database when a physical or logical standby database is present in the Oracle Data Guard configuration.

* [Before You Upgrade the Oracle Database Software](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-784C06D8-9CFA-4244-BA55-D7003CAB3F1E)

* [Patching Oracle Database with Standby First Patching](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-A5226768-DB6B-4714-BB9A-0A3EF17A01C8)

* [Upgrading Oracle Database with a Physical Standby Database in Place](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-6431EFBF-6B00-4E7D-A106-63574225BDD9)

* [Upgrading Oracle Database with a Logical Standby Database in Place](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-5AF5983E-E481-4599-88B9-311A43EB5C80)

* [Modifying the COMPATIBLE Initialization Parameter After Upgrading](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-253C94C1-8E40-4378-A0A5-2627C248D873)

* [Downgrading Oracle Database with No Logical Standby in Place](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-F087E74B-ED71-42DF-A730-2A8A5FAA1A84)

* [Downgrading Oracle Database with a Logical Standby in Place](upgrading-patching-downgrading-oracle-data-guard-configuration.md#GUID-FDB0FA2C-EA66-423C-AFD9-51F3FA4899F8)




### Before You Patch or Upgrade the Oracle Database Software {#GUID-784C06D8-9CFA-4244-BA55-D7003CAB3F1E}

Before you patch or upgrade your Oracle Database software, review the prerequisites for different use case scenarios.

* If you are using the Oracle Data Guard broker to manage your configuration, follow the instructions in *Oracle Data Guard Broker*

* Use procedures described in these topics in conjunction with other upgrade procedures and guidelines provided in *Oracle Database Upgrade Guide*. 

* Check for `NOLOGGING` operations. If `NOLOGGING` operations have been performed then you must update the standby database. <br>See [Recovering After the NOLOGGING Clause Is Specified](examples-of-using-oracle-data-guard.md#GUID-BD9E85AB-D812-4659-9C85-26CDFF115A8A)

* Make note of any tablespaces or data files that need recovery due to `OFFLINE IMMEDIATE`. Before starting an upgrade, tablespaces or data files should be recovered, and either online or offline. 

* In an Oracle Data Guard configuration, all physical and snapshot standby databases must use a copy of the password file from the primary database. Password file changes done on the primary database are automatically propagated to standby databases. Password file changes are events such as when an administrative privilege (`SYSDG`, `SYSOPER`, `SYSDBA`, and so on) is granted or revoked, and when the password of any user with administrative privileges is changed. <br>Far sync instances are an exception to the automatic updating feature. Updated password files must still be manually copied to far sync instances, because far sync instances receive redo, but do not apply it. When a password file is manually updated at a far sync instance, the redo containing the same password changes from the primary database is automatically propagated to any standby databases that are set up to receive redo from that far sync instance. The password file is updated on the standby when the redo is applied.




> **note:** 

If there are cascaded standbys in your configuration, then those cascaded standbys must follow the same rules as any other standby, but should be shut down last, and restarted in the new home first.

**Related Topics**

* [Oracle Data Guard Broker Upgrading and Downgrading](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-82682FF3-6052-471B-B581-5E91183A5432)



### Patching Oracle Database with Standby First Patching {#GUID-A5226768-DB6B-4714-BB9A-0A3EF17A01C8}

Oracle Data Guard Standby-First Patch Apply provides support for different database home software between a primary database and its physical standby database(s).

This support is provided for the purpose of applying and validating Oracle patches and patch bundles in rolling fashion with minimal risk to the primary database. For example, with Data Guard Standby-First Patch Apply you apply a database home patch first to a physical standby database. The standby is used to run read-only workload, or read-write workload if it is a snapshot standby, for testing and evaluation of the patch. After passing evaluation, the patch is then installed on the primary system with greater assurance of the effectiveness and stability of the database home patch.

Oracle Data Guard Standby-First Patch Apply is supported only for certified interim patches and patch bundles (for example, Patch Set Update, or Database Patch for Exadata) for Oracle Database 11.2.0.1 and later, on both Oracle Engineered Systems (e.g. Exadata, SuperCluster) and non-Engineered Systems. A patch and patch bundle that is Data Guard Standby-First certified states the following in the patch README:

`Data Guard Standby-First Installable`

The following types of patches are candidates to be Data Guard Standby-First certified:

* Database home interim patches

* Exadata bundle patches (e.g. Monthly and quarterly database patches for Exadata)

* Database patch set updates




Patches and patch bundles that update modules that may potentially disrupt the interoperability between primary and physical standby systems running different database home software are not certified “Data Guard Standby-First Installable” and do not state so in the patch README.

Oracle patch sets and major release upgrades do not qualify for Data Guard Standby-First Patch Apply. For example, upgrades from 11.2.0.2 to 11.2.0.3 or 11.2 to 12.1 do not qualify. Use the Data Guard transient logical standby rolling upgrade process for database patch sets and major releases.

A physical standby database can be used to install eligible one-off patches, patch set updates (PSUs), and critical patch updates (CPUs), in rolling fashion. For more information about this functionality, see the My Oracle Support note 1265700.1 at [`http://support.oracle.com`](http://support.oracle.com). 

### Upgrading Oracle Database with a Physical Standby Database in Place {#GUID-6431EFBF-6B00-4E7D-A106-63574225BDD9}

These steps show how to upgrade to Oracle Database when a physical standby database is present in the configuration.

> **note:** If the database being upgraded is a member of an Oracle Data Guard broker configuration, then before proceeding, you must disable fast-start failover and shut down the broker. For information about how to do this, see *Oracle Data Guard Broker* . 

1. Review and perform the standard preupgrade preparation tasks described in *Oracle Database Upgrade Guide*.
2. Install the new release of the Oracle software into a new Oracle home on the physical standby database and primary database systems, as described in *Oracle Database Upgrade Guide*
3. Shut down the primary database.
4. Shut down physical standby databases. 
5. Stop all listeners, agents, and other processes running in the Oracle homes that you want to upgrade (Source Oracle homes). Perform this step on all nodes in an Oracle Real Application Clusters (Oracle RAC) environment.
6. In the new Oracle home (Target Oracle home), restart all listeners, agents, and other processes that you stopped in the source Oracle home
7. Mount physical standby databases on the target Oracle home (upgraded version). 

> **note:** Caution: 

Do not open standby databases until the primary database upgrade is completed.

See [Start the Physical Standby Database](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569) for information on how to start a physical standby database. 

8. Start Redo Apply on the physical standby databases.

> **note:** By default, AutoUpgrade disables log shipping. If you have modified your AutoUpgrade configuration file to enable log shipping, then modify your AutoUpgrade configuration file to set the AutoUpgrade locally modifiable global parameter `defer_standby_log_shipping` to `no`. For example: `upg1.defer_standby_log_shipping=no`

See [Start the Physical Standby Database](creating-oracle-data-guard-physical-standby.md#GUID-3D65210D-9FE5-490E-938E-57FC2E07A569) for information on how to start Redo Apply. 

9. Upgrade the primary database. Physical standby databases are upgraded when the redo generated by the primary database as it is upgraded is applied to standbys. 
10. Open the upgraded primary database.
11. If Oracle Active Data Guard was being used before the upgrade, then you must reenable it after upgrading.

See [Real-time query](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=SBYDB-GUID-07CB190C-C248-4FF5-AB64-EAA9C6D42677)

12. (Optional) When ready. modify the `COMPATIBLE` initialization parameter. 

> **note:** 

On Microsoft Windows platforms, it is necessary to use the ORADIM utility to delete the database service (for the old database version), and to create a new database service for the new database version. You must replace the `OracleService*`SID`*` on both the primary and standby servers. 




**Related Topics**

* [Oracle Data Guard Broker Upgrading and Downgrading](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-82682FF3-6052-471B-B581-5E91183A5432)
* [Preparing to Upgrade Oracle Database, *Oracle Database Upgrade Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD-GUID-733D2563-A42D-4366-9FCD-DABD25D0E2DF)
* [Modifying the COMPATIBLE Initialization Parameter After Upgrading](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=DGBKR-GUID-253C94C1-8E40-4378-A0A5-2627C248D873)



### Upgrading Oracle Database with a Logical Standby Database in Place {#GUID-5AF5983E-E481-4599-88B9-311A43EB5C80}

These steps describe the traditional method for upgrading your Oracle Database software with a logical standby database in place.

> **note:** 

[Using SQL Apply to Upgrade the Oracle Database](using-sql-apply-to-perform-rolling-upgrade.md#GUID-290F632F-5295-47F3-AEF1-2D37C69C00D7) presents an alternative method to perform an upgrade with a logical standby database in place in a rolling fashion to minimize downtime. Use the steps from only one method to perform the complete upgrade. Do not attempt to use both methods or to combine the steps from the two methods as you perform the upgrade process. 

Perform the following steps to upgrade to Oracle Database 12*c* Release 2 (12.2) when a logical standby database is present in the configuration. These steps assume that the primary database is running in `MAXIMUM PERFORMANCE` data protection mode. 

Review and perform the steps listed in the "Preparing to Upgrade" chapter of the [*Oracle Database Upgrade Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD002)

1. Set the data protection mode to `MAXIMUM PERFORMANCE` at the primary database, if needed:
```
SQL> ALTER DATABASE SET STANDBY DATABASE TO MAXIMIZE PERFORMANCE;
```


2. On the primary database, stop all user activity and defer the remote archival destination associated with the logical standby database (for this procedure, it is assumed that `LOG_ARCHIVE_DEST_2` is associated with the logical standby database):
```
SQL> ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_2=DEFER SCOPE=BOTH;
SQL> ALTER SYSTEM ARCHIVE LOG CURRENT;
```


3. Stop SQL Apply on the logical standby database:
```
SQL> ALTER DATABASE STOP LOGICAL STANDBY APPLY;
```


4. On the primary database install the newer release of the Oracle software. .
5. On the logical standby database, install the newer release of the Oracle software.

> **note:** 

Steps 4 and 5 can be performed concurrently (in other words, the primary and the standby databases can be upgraded concurrently) to reduce downtime during the upgrade procedure.

6. On the upgraded logical standby database, restart SQL Apply. If you are using Oracle RAC, start up the other standby database instances: 
```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE;
```


7. Open the upgraded primary database and allow users to connect. If you are using Oracle RAC, start up the other primary database instances. 

Also, enable archiving to the upgraded logical standby database, as follows:
```
SQL> ALTER SYSTEM SET LOG_ARCHIVE_DEST_STATE_2=ENABLE;
```


8. Optionally, reset to the original data protection mode if you changed it in Step 1.
9. Optionally, modify the `COMPATIBLE` initialization parameter.



**Related Topics**

* [*Oracle Database Upgrade Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD-GUID-CC88F262-3370-4FFA-8604-EDA9F20F56D9)



### Modifying the COMPATIBLE Initialization Parameter After Upgrading {#GUID-253C94C1-8E40-4378-A0A5-2627C248D873}

When you upgrade to a new release of Oracle Database, certain new features might make your database incompatible with your previous release. 

Oracle Database enables you to control the compatibility of your database with the `COMPATIBLE` initialization parameter. 

After the upgrade is complete, you can increase the setting of the `COMPATIBLE` initialization parameter to the maximum level for the new Oracle Database release. When you are certain that you no longer need the ability to downgrade your database back to its original version, set the `COMPATIBLE` initialization parameter based on the compatibility level you want for your new database. 

In an Oracle Data Guard configuration, if you decide to increase the setting of the `COMPATIBLE` initialization parameter after upgrading, then it is important that you perform the following steps in the order shown (be sure the standby database has a `COMPATIBLE` setting equal to, or higher than, the primary): 

1. Increase the value of the `COMPATIBLE` initialization parameter on all standby databases in the configuration first, as follows: 

1. Ensure that apply is current on the standby database(s).

2. On one instance of each standby database, execute the following SQL statement:
```
ALTER SYSTEM SET COMPATIBLE=<value> SCOPE=SPFILE;
```


3. If Redo Apply or SQL Apply is running, then stop them.

4. Restart all instances of the standby database(s).

5. If you previously stopped Redo Apply or SQL Apply, then restart them.

2. Increase the value of the `COMPATIBLE` initialization parameter on the primary database, as follows: 

1. On one instance of the primary database, execute the following SQL statement:
```
ALTER SYSTEM SET COMPATIBLE=<value> SCOPE=SPFILE;
```


2. Restart all instances of the primary database.




> **note:** See Also: 

* [*Oracle Database Upgrade Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD12472) for more information about compatibility settings 




### Downgrading Oracle Database with No Logical Standby in Place {#GUID-F087E74B-ED71-42DF-A730-2A8A5FAA1A84}

Perform these steps to downgrade Oracle Database in an Oracle Data Guard configuration that does *not* contain a logical standby database. 

1. Ensure that all physical standby databases are mounted, but not open. Do not open the standby database(s) until all redo generated by the downgrade of the primary database has been applied.
2. Start Redo Apply, in real-time apply mode, on the physical standby database(s).
3. Downgrade the primary database using the procedure described in [*Oracle Database Upgrade Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD007), keeping the following in mind:

* At each step of the downgrade procedure where a script is executed, execute the script only at the primary database. Do not perform the next downgrade step until all redo generated by the execution of the script at the primary database has been applied to each physical standby database.

* At each step of the downgrade procedure where an action other than running a script is performed, perform the step at the primary database first and then at each physical standby database. Do not perform the next downgrade step at the primary database until the action has been performed at each physical standby database.

4. If it becomes necessary to perform a failover during a downgrade, perform the failover and then continue with the downgrade procedure at the new primary database.



### Downgrading Oracle Database with a Logical Standby in Place {#GUID-FDB0FA2C-EA66-423C-AFD9-51F3FA4899F8}

Perform these steps to downgrade Oracle Database in an Oracle Data Guard configuration that contains a logical standby database or a mixture of logical and physical standby databases.

1. Issue the following command at the primary database (database P, for the sake of this discussion) before you downgrade it:
```
SQL> ALTER DATABASE COMMIT TO SWITCHOVER TO LOGICAL STANDBY;
```


Database P is no longer in the primary database role.

2. Wait for *all* standby databases in the configuration to finish applying all available redo. To determine whether each standby database has finished applying all available redo, run the following query at each standby database:
```
SQL> SELECT SWITCHOVER_STATUS FROM V$DATABASE;

SWITCHOVER_STATUS
-----------------
TO PRIMARY
```


Do not continue on to step 3 until the query returns a value of `TO PRIMARY` for all standby databases in the configuration. 

3. Downgrade the logical standby databases using the procedures described in [*Oracle Database Upgrade Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=UPGRD007), keeping the following in mind:

* At each step of the downgrade procedure where a script is executed, execute the script only at the logical standby databases. Do not perform the next downgrade step until all redo generated by executing the script at the logical standby database that was most recently in the primary role (database P) has been applied to each physical standby database.

* At each step of the downgrade procedure where an action other than running a script is performed, first perform the step at the logical standby database that was most recently in the primary role (database P), and then perform the step at each physical standby database. Do not perform the next downgrade step at the logical standby database that was most recently in the primary role (database P) until the action has been performed at each physical standby database.

4. After the logical standby that was most recently in the primary role (database P) has been successfully downgraded, open it, and issue the following command:
```
SQL> ALTER DATABASE ACTIVATE LOGICAL STANDBY DATABASE;
```


Database P is now back in the primary role.

5. At each of the logical standby databases in the configuration, issue the following command (note that the command requires that a database link back to the primary exist in all of the logical standby databases):
```
SQL> ALTER DATABASE START LOGICAL STANDBY APPLY IMMEDIATE NEW PRIMARY
prim_db_link;
```



