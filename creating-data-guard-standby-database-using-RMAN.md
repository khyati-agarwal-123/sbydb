## ECreating a Standby Database with Recovery Manager {#GUID-82731D59-A20F-45DD-A235-267B3B0E38C5}

These topics describe how to use Oracle Recovery Manager to create a standby database.

* [Prerequisites](creating-data-guard-standby-database-using-RMAN.md#GUID-BFD2D916-17D6-4CC1-8BC0-2F333C54516D)

* [Overview of Standby Database Creation with RMAN](creating-data-guard-standby-database-using-RMAN.md#GUID-CAEE94C8-6C1A-4021-B93F-E9C65384418A)

* [Using the DUPLICATE Command to Create a Standby Database](creating-data-guard-standby-database-using-RMAN.md#GUID-D1694762-4B53-4828-B094-874DB1A6FC81)




### Prerequisites {#GUID-BFD2D916-17D6-4CC1-8BC0-2F333C54516D}

Before you create a standby database with RMAN, you should be familiar with database duplication.

Because you use the `DUPLICATE` command to create a standby database with RMAN, you should also familiarize yourself with the `DUPLICATE` command . 

Also familiarize yourself with how to create a standby database *before* you attempt to create a standby database with RMAN. 

> **note:** See Also: 

* [*Oracle Database Backup and Recovery User’s Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV010) for information about database duplication 

* [*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF126) for a description of the RMAN `DUPLICATE` command 

* [Creating a Physical Standby Database](creating-oracle-data-guard-physical-standby.md#GUID-B511FB6E-E3E7-436D-94B5-071C37550170)

* [Creating a Logical Standby Database](creating-oracle-data-guard-logical-standby.md#GUID-3666CA35-D993-44B6-8D70-A2B8B9EC8B2E)




### Overview of Standby Database Creation with RMAN {#GUID-CAEE94C8-6C1A-4021-B93F-E9C65384418A}

Read this section for information about the purpose and basic concepts involved in standby database creation with RMAN.

#### Purpose of Standby Database Creation with RMAN {#GUID-9864EED2-AE6E-4378-BA7C-1FB3971649E5}

You can use either manual techniques or the RMAN `DUPLICATE` command to create a standby database from backups of your primary database. 

You can use either manual techniques or the RMAN `DUPLICATE` command to create a standby database from backups of your primary database. Creating a standby database with RMAN has the following advantages over manual techniques: 

* RMAN can create a standby database by copying the files currently in use by the primary database. No backups are required.

* RMAN can create a standby database by restoring backups of the primary database to the standby site. Thus, the primary database is not affected during the creation of the standby database.

* RMAN automates renaming of files, including Oracle Managed Files (OMF) and directory structures.

* RMAN restores archived redo log files from backups and performs media recovery so that the standby and primary databases are synchronized.




#### Basic Concepts of Standby Creation with RMAN {#GUID-C055CE69-9410-4B71-92B6-93C9B6F63197}

The procedure for creating a standby database with RMAN is almost the same as for creating a duplicate database.

You need to amend the duplication procedures described in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89930) to account for issues specific to a standby database. 

To create a standby database with the `DUPLICATE` command you must connect as target to the primary database and specify the `FOR STANDBY` option. You cannot connect to a standby database and create an additional standby database. RMAN creates the standby database by restoring and mounting a control file. RMAN can use an existing backup of the primary database control file, so you do not need to create a control file backup especially for the standby database. 

A standby database, unlike a duplicate database created by `DUPLICATE` *without* the `FOR` `STANDBY` `OPTION`, does not get a new DBID. Therefore, do not register the standby database with your recovery catalog. 

##### Active Database and Backup-Based Duplication {#GUID-DDC82FDB-86B6-4FF0-8C1A-7F3611C7F4F9}

When you are using RMAN to create a standby, you must choose between active and backup-based duplication.

If you specify `FROM ACTIVE DATABASE`, then RMAN copies the data files directly from the primary database to the standby database. The primary database must be mounted or open. 

If you not specify `FROM ACTIVE DATABASE`, then RMAN performs backup-based duplication. RMAN restores backups of the primary data files to the standby database. All backups and archived redo log files needed for creating and recovering the standby database must be accessible by the server session on the standby host. RMAN restores the most recent data files unless you execute the `SET UNTIL` command. 

##### DB_UNIQUE_NAME Values in an RMAN Environment {#GUID-AC0FF3B1-456C-4EF5-BD3E-08014927B7B4}

A standby database, unlike a duplicate database created by `DUPLICATE` without the `FOR STANDBY` option, does not get a new DBID. 

When you use RMAN in an Oracle Data Guard environment, connect it to a recovery catalog. The recovery catalog can store the metadata for all primary and standby databases in the environment. Do not explicitly register the standby database in the recovery catalog.

A database in an Oracle Data Guard environment is uniquely identified by means of the `DB_UNIQUE_NAME` parameter in the initialization parameter file. The `DB_UNIQUE_NAME` must be unique across all the databases with the same DBID for RMAN to work correctly in an Oracle Data Guard environment. 

> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89379) for a conceptual overview of RMAN operation in an Oracle Data Guard environment 

##### Recovery of a Standby Database {#GUID-A75EBEE4-CFD7-4205-B3D0-24AFFC1331AF}

By default, RMAN does not recover the standby database after creating it. 

RMAN leaves the standby database mounted, but does not place the standby database in manual or managed recovery mode. RMAN disconnects and does not perform media recovery of the standby database.

For RMAN to recover the standby database after creating it, the standby control file must be usable for the recovery. The following conditions must be met:

* The end recovery time of the standby database must be greater than or equal to the checkpoint SCN of the standby control file.

* An archived redo log file containing the checkpoint SCN of the standby control file must be available at the standby site for recovery.




One way to ensure these conditions are met is to issue the `ALTER` `SYSTEM` `ARCHIVE` `LOG` `CURRENT` statement after backing up the control file on the primary database. This statement archives the online redo log files of the primary database. Then, either back up the most recent archived redo log file with RMAN or move the archived redo log file to the standby site. 

Use the `DORECOVER` option of the `DUPLICATE` command to specify that you want RMAN to recover the standby database. RMAN performs the following steps after creating the standby database files: 

1. RMAN begins media recovery. If recovery requires archived redo log files, and if the log files are not already on disk, then RMAN attempts to restore backups.

2. RMAN recovers the standby database to the specified time, system change number (SCN), or log file sequence number, or to the latest archived redo log file generated if none of the preceding are specified.

3. RMAN leaves the standby database mounted after media recovery is complete, but does *not* place the standby database in manual or managed recovery mode. 




###### Standby Database Redo Log Files {#GUID-730789B8-813C-433F-BF49-D34B4077DE77}

RMAN automatically creates the standby redo log files on the standby database. 

After the log files are created, the standby database maintains and archives them according to the normal rules for log files.

If you use backup-based duplication, then the only option when naming the standby redo log files on the standby database is the file names for the log files, as specified in the standby control file. If the log file names on the standby must be different from the primary file names, then one option is to specify file names for the standby redo logs by setting `LOG_FILE_NAME_CONVERT` in the standby initialization parameter file. 

Note the following restrictions when specifying file names for the standby redo log files on the standby database:

* You must use the `LOG_FILE_NAME_CONVERT` parameter to name the standby redo log files if the primary and standby databases use different naming conventions for the log files. 

* You cannot use the ```SET` `NEWNAME` or ```CONFIGURE` `AUXNAME` commands to rename the standby redo log files. 

* You cannot use the `LOGFILE` clause of the `DUPLICATE` command to specify file names for the standby redo log files. 

* For the standby redo log file names on the standby database to be the same as the primary redo log file names, you must specify the `NOFILENAMECHECK` clause of the `DUPLICATE` command. Otherwise, RMAN signals an error even if the standby database is created on a different host. 




##### Password Files for the Standby Database {#GUID-70DD3082-8678-4A06-92F0-7121ACAEBBE9}

If you are using active database duplication, then RMAN always copies the password file to the standby host because the password file on the standby database must be an exact copy of the password file on the target database. 

If you are using active database duplication, then RMAN always copies the password file to the standby host because the password file on the standby database must be an exact copy of the password file on the target database. In this case, the `PASSWORD FILE` clause is not necessary. RMAN overwrites any existing password file for the auxiliary instance. With backup-based duplication you must copy the password file used on the primary to the standby, for Oracle Data Guard to ship logs. 

### Using the DUPLICATE Command to Create a Standby Database {#GUID-D1694762-4B53-4828-B094-874DB1A6FC81}

The procedure for creating a standby database is basically identical to the RMAN duplication procedure.

See [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV168) for information about the RMAN duplication procedure. 

#### Using Active Database Duplication to Create a Standby Database or Far Sync Instance {#GUID-C713FD5C-3375-45E4-A78F-4063006DAC5B}

You can use active database duplication to create either a standby database or a far sync instance.

**Creating a Standby Database with Active Database Duplication**

To use the RMAN `DUPLICATE` command to create a standby database from files that are active in the primary database, you must specify both the `FOR STANDBY` and `FROM ACTIVE DATABASE` options. You can specify other options as well, such as `DORECOVER`. For example:
```
DUPLICATE TARGET DATABASE FOR STANDBY FROM ACTIVE DATABASE DORECOVER;
```


**Creating a Far Sync Instance with Active Database Duplication**

As of Oracle Database 12*c* Release 12.2 (12.2.0.1), you can also use the RMAN `DUPLICATE` command to create a far sync instance from files that are active in the primary database. To do so, substitute `FARSYNC` in place of `STANDBY` on the command line (do not specify the `DORECOVER` option; it is not allowed for far sync instances). For example: 
```
DUPLICATE TARGET DATABASE FOR FARSYNC FROM ACTIVE DATABASE;
```


**Steps to Create a Standby Database or Far Sync Instance Using Active Database Duplication**

The following steps create a standby database from active database files, but you could also use these steps to create a far sync instance from active database files. The steps assume that the standby host (or far sync instance), and primary database host have the same directory structure. 

1. Prepare the auxiliary database instance as explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV167).

Because you are using active database duplication, you must create a password file for the auxiliary instance and establish Oracle Net connectivity. This is a temporary password file which is overwritten during the duplicate operation.

2. Decide how to provide names for the standby control files, data files, online redo logs, and tempfiles. This step is explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV298).

In this scenario, the standby database files are named the same as the primary database files.

3. Start and configure RMAN as explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV299).
4. Execute the `DUPLICATE` command.

The following example illustrates how to use `DUPLICATE` for active duplication. This example requires the `NOFILENAMECHECK` option because the primary database files have the same names as the standby database files. The `SET` clauses for `SPFILE` are required for log shipping to work properly. The `db_unique_name` must be set to ensure that the catalog and Oracle Data Guard can identify this database as being different from the primary. Optionally, specify the `DORECOVER` option to recover the database after standby creation (`DORECOVER` is not a valid option for far sync instances).
```
DUPLICATE TARGET DATABASE
FOR STANDBY
FROM ACTIVE DATABASE
DORECOVER
SPFILE
SET "db_unique_name"="foou" COMMENT "Is a duplicate"
SET LOG_ARCHIVE_DEST_2="service=inst3 ASYNC REGISTER
VALID_FOR=(online_logfile,primary_role)"
SET FAL_SERVER="inst1" COMMENT "Is primary"
NOFILENAMECHECK;
```


RMAN automatically copies the server parameter file to the standby host, starts the auxiliary instance with the server parameter file, restores a backup control file, and copies all necessary database files and archived redo logs over the network to the standby host. RMAN recovers the standby database, but does not place it in manual or managed recovery mode.

If you were creating a far sync instance rather than a standby, the command would be the same except that `STANDBY` is replaced with `FARSYNC`, as follows:
```
DUPLICATE TARGET DATABASE
FOR FARSYNC
FROM ACTIVE DATABASE
SPFILE
SET "db_unique_name"="foou" COMMENT "Is a duplicate"
SET LOG_ARCHIVE_DEST_2="service=inst3 ASYNC REGISTER
VALID_FOR=(online_logfile,primary_role)"
SET FAL_SERVER="inst1" COMMENT "Is primary"
NOFILENAMECHECK;
```


> **note:** If the primary is an Oracle Real Application Clusters (Oracle RAC) database, and the target standby database or far sync instance is going to be a single-instance Oracle database, then add the command `SET CLUSTER_DATABASE="FALSE"` to the previous RMAN examples. 




#### Creating a Standby Database with Backup-Based Duplication {#GUID-E9FBCFA9-20A2-49DD-8060-AA39100D7751}

You can use backup—based duplication to create either a standby database or a far sync instance.

**Creating a Standby Database with Backup-Based Duplication**

To create a standby database from backups, specify `FOR STANDBY` but do not specify `FROM ACTIVE DATABASE`. You can specify other options as well, such as `DORECOVER`. For example:
```
DUPLICATE TARGET DATABASE FOR STANDBY BACKUP LOCATION '+DATA/BACKUP' DORECOVER;
```


**Creating a Far Sync Instance with Backup-Based Duplication**

As of Oracle Database 12*c* Release 12.2 (12.2.0.1), you can also use backup-based duplication to create a Data Guard far sync instance using the RMAN `DUPLICATE` command. To do so, substitute `FARSYNC` in place of `STANDBY` on the command line (do not specify the `DORECOVER` option; it is not allowed for far sync instances). For example: 
```
DUPLICATE TARGET DATABASE FOR FARSYNC BACKUP LOCATION '+DATA/BACKUP';
```


**Steps to Create a Standby Database or Far Sync Instance from Backups:**

The following steps create a standby database from backup files, but you could also use these steps to create a far sync instance from backup files. The steps assume that the standby host (or far sync instance), and primary database host have the same directory structure. 

1. Make database backups and archived redo logs available to the auxiliary instance on the duplicate host as explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89931).
2. Prepare the auxiliary database instance as explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV167).
3. Decide how to provide names for the standby control files, data files, online redo logs, and tempfiles. This step is explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV298).

In this scenario, the standby database files are named the same as the primary database files.

4. Start and configure RMAN as explained in [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV299).
5. Execute the `DUPLICATE` command.

The following example illustrates how to use `DUPLICATE` for backup-based duplication. This example requires the `NOFILENAMECHECK` option because the primary database files have the same names as the standby database files. Optionally, specify the `DORECOVER` option to recover the database after standby creation (`DORECOVER` is not a valid option for far sync instances). 
```
DUPLICATE TARGET DATABASE
FOR STANDBY
DORECOVER
SPFILE
SET "db_unique_name"="foou" COMMENT "Is a duplicate"
SET LOG_ARCHIVE_DEST_2="service=inst3 ASYNC REGISTER
VALID_FOR=(online_logfile,primary_role)"
SET FAL_SERVER="inst1" COMMENT "Is primary"
NOFILENAMECHECK;
```


RMAN automatically copies the server parameter file to the standby host, starts the auxiliary instance with the server parameter file, and restores all necessary database files and archived redo logs to the standby host. RMAN recovers the standby database, but does not place it in manual or managed recovery mode.


