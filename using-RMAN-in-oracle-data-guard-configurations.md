## 12Using RMAN to Back Up and Restore Files {#GUID-516D9C60-F2EF-4822-BB87-5D3BE877B69B}

You can create backup strategies using Oracle Recovery Manager (RMAN) with Oracle Data Guard and standby databases.

> **note:** 

A multitenant container database is the only supported architecture in Oracle Database 21c and later releases. While the documentation is being revised, legacy terminology may persist. In most cases, "database" and "non-CDB" refer to a CDB or PDB, depending on context. In some contexts, such as upgrades, "non-CDB" refers to a non-CDB from a previous release.

RMAN can perform backups with minimal effect on the primary database and quickly recover from the loss of individual data files, or the entire database. RMAN and Oracle Data Guard can be used together to simplify the administration of an Oracle Data Guard configuration.<br>See the following topics:

* [About RMAN File Management in an Oracle Data Guard Configuration](using-RMAN-in-oracle-data-guard-configurations.md#GUID-7B0909F0-2DEA-4A16-8E91-A4687F634D4E)

* [About RMAN Configuration in an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-8D525402-9629-44EC-A3F1-F6B8C3307E8C)

* [Recommended RMAN and Oracle Database Configurations](using-RMAN-in-oracle-data-guard-configurations.md#GUID-33B70E30-2454-484C-9110-A5AC452E63CB)

* [Backup Procedures](using-RMAN-in-oracle-data-guard-configurations.md#GUID-410FEFAD-7805-405A-8C02-B9D8F74C60F4)

* [Registering and Unregistering Databases in an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-9C54A243-6B84-4755-957E-AFC57B9BC3B6)

* [Reporting in an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-D13E9A5E-5DBB-4DF4-8534-F1E16FA33618)

* [Performing Backup Maintenance in an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-4B1696A8-1A90-426C-A620-D5F313365E54)

* [Recovery Scenarios in an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-44539CC5-0BAB-47F8-B51F-BFD0A1D6DA40)

* [Additional Backup Situations](using-RMAN-in-oracle-data-guard-configurations.md#GUID-211E08A4-C19E-4159-80B2-429431314278)

* [Restoring and Recovering Files Over the Network](using-RMAN-in-oracle-data-guard-configurations.md#GUID-F0EB4F8E-7D2F-4674-9A5B-AABC368D8F11)

* [RMAN Support for CDBs In an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-72F008A1-527F-41DB-89CD-623FA06D0420)




> **note:** 

Because a logical standby database is not a block-for-block copy of the primary database, you cannot use a logical standby database to back up the primary database.

> **note:** See Also: 

* [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89346) for more information about RMAN concepts and about using RMAN in an Oracle Data Guard environment 

* [*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF001) for detailed information about RMAN commands 




### About RMAN File Management in an Oracle Data Guard Configuration {#GUID-7B0909F0-2DEA-4A16-8E91-A4687F634D4E}

RMAN uses a recovery catalog to track filenames for all database files in an Oracle Data Guard environment. 

A recovery catalog is a database schema used by RMAN to store metadata about one or more Oracle databases. The catalog also records where the online redo logs, standby redo logs, tempfiles, archived redo logs, backup sets, and image copies are created. 

#### Interchangeability of Backups in an Oracle Data Guard Environment {#GUID-075FF361-A398-4533-9E7B-9255A288A662}

RMAN commands use the recovery catalog metadata to behave transparently across different physical databases in the Oracle Data Guard environment.

For example, you can back up a tablespace on a physical standby database and restore and recover it on the primary database. Similarly, you can back up a tablespace on a primary database and restore and recover it on a physical standby database.

> **note:** 

Backups of logical standby databases are not usable at the primary database.

Backups of standby control files and nonstandby control files are interchangeable. For example, you can restore a standby control file on a primary database and a primary control file on a physical standby database. This interchangeability means that you can offload control file backups to one database in an Oracle Data Guard environment. RMAN automatically updates the filenames for database files during restore and recovery at the databases.

#### Association of Backups in an Oracle Data Guard Environment {#GUID-73A75929-1F78-46F1-9CBE-EC2611388A6B}

The recovery catalog tracks the files in the Oracle Data Guard environment by associating every database file or backup file with a `DB_UNIQUE_NAME`. 

The database that creates a file is associated with the file. For example, if RMAN backs up the database with the unique name of `standby1`, then `standby1` is associated with this backup. A backup remains associated with the database that created it unless you use the `CHANGE` `... RESET DB_UNIQUE_NAME` to associate the backup with a different database. 

#### Accessibility of Backups in an Oracle Data Guard Environment {#GUID-61ABB2C2-0813-4DD9-94F4-BB292CB21194}

By default, in an Oracle Data Guard environment, the recovery catalog considers disk backups as accessible only to the database with which it is associated, whereas tape backups created on one database are accessible to all databases.

You can override the default behavior by using the following RMAN command:
```
SET BACKUP FILES FOR DEVICE TYPE device_specifier TO [NOTACCESSIBLE | ACCESSIBLE]
```


The accessibility setting is at the session level which means that after the RMAN session is disconnected, the association of backups reverts back to default behavior.

If a backup file is not associated with any database, then the row describing it in the recovery catalog view shows `null` for the `SITE_KEY` column. By default, RMAN associates files whose `SITE_KEY` is `null` with the target database. 

RMAN commands such as `BACKUP`, `RESTORE`, and `CROSSCHECK` work on any accessible backup. For example, for a `RECOVER COPY` operation, RMAN considers only image copies that are associated with the database as eligible to be recovered. RMAN considers the incremental backups on disk and tape as eligible to recover the image copies. In a database recovery, RMAN considers only the disk backups associated with the database and all files on tape as eligible to be restored. 

To illustrate the differences in backup accessibility, assume that databases `prod` and `standby1` reside on different hosts. RMAN backs up data file 1 on `prod` to `/prmhost/disk1/df1.dbf` on the production host and also to tape. RMAN backs up data file 1 on `standby1` to `/sbyhost/disk2/df1.dbf` on the standby host and also to tape. If RMAN is connected to database `prod`, then you cannot use RMAN commands to perform operations with the `/sbyhost/disk2/df1.dbf` backup located on the standby host. However, RMAN does consider the tape backup made on `standby1` as eligible to be restored. 

> **note:** 

You can FTP a backup from a standby host to a primary host or vice versa, connect as `TARGET` to the database on this host, and then [`CATALOG`](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF109) the backup. After a file is cataloged by the target database, the file is associated with the target database. 

### About RMAN Configuration in an Oracle Data Guard Environment {#GUID-8D525402-9629-44EC-A3F1-F6B8C3307E8C}

In an Oracle Data Guard configuration, the process of backing up control files, data files, and archived logs can be offloaded to the standby system, thereby minimizing the effect of backups on the production system. 

These backups can be used to recover the primary or standby database.

RMAN uses the `DB_UNIQUE_NAME` initialization parameter to distinguish one database site from another database site. Thus, it is critical that the uniqueness of `DB_UNIQUE_NAME` be maintained in an Oracle Data Guard configuration. 

Only the primary database must be explicitly registered using the RMAN `REGISTER DATABASE` command. You do this after connecting RMAN to the recovery catalog and primary database as target. 

Use the RMAN `CONFIGURE` command to set the RMAN configurations. When the `CONFIGURE` command is used with the `FOR DB_UNIQUE_NAME` option, it sets the RMAN site-specific configuration for the database with the DB_UNIQUE_NAME you specify. 

For example, after connecting to the recovery catalog, you could use the following commands at an RMAN prompt to set the default device type to `SBT` for the `BOSTON` database that has a DBID of 1625818158. The RMAN `SET DBID` command is required only if you are not connected to a database as target. 
```
SET DBID 1625818158;
CONFIGURE DEFAULT DEVICE TYPE TO SBT FOR DB_UNIQUE_NAME BOSTON;
```


### About RMAN Support for Oracle Data Guard In a Multitenant Environment {#GUID-72F008A1-527F-41DB-89CD-623FA06D0420}

RMAN supports point-in-time recovery (PITR) of a multitenant container database (CDB) at a standby. Individual pluggable databases (PDBs) do not have their own individual standbys.

This is in addition to the support RMAN provides for complete database recovery and complete data file recovery at a standby. 

To perform a CDB PITR at a standby, connect to the CDB as root and issue the RMAN `BACKUP`, `RESTORE`, and `RECOVER` commands as necessary. 

Be aware that when a CDB PITR is performed on a standby, any pluggable databases (PDBs) that were in a disabled state before the CDB PITR become enabled. To return a PDB to a disabled state, connect to it, ensure it is closed (the `OPEN_MODE` column in the `V$PDBS` view shows a value of `MOUNTED`), and then execute the SQL statement `ALTER PLUGGABLE DATABASE DISABLE RECOVERY`. 

The `ALTER PLUGGABLE DATABASE DISABLE RECOVERY` statement takes all data files belonging to the PDB offline and disables recovery for the PDB. The data files that belong to the PDB are not part of any recovery session until the PDB is enabled again. Any new data files created while recovery is disabled are created as unnamed files and are marked offline. 

To bring all data files that belong to a PDB back online and enable it for recovery, connect to it, ensure it is closed (the `OPEN_MODE` column in the `V$PDBS` view shows a value of `MOUNTED`), and issue the SQL statement `ALTER PLUGGABLE DATABASE ENABLE RECOVERY`. 

To check whether recovery is enabled or disabled on a PDB, query the `V$PDBS` view as follows: 
```
SQL> SELECT RECOVERY_STATUS FROM V$PDBS;
```


Flashing Back a PDB

As of Oracle Database 12*c* Release 2 (12.2.0.1), you can use the `FLASHBACK PLUGGABLE DATABASE` command (available through SQL or Recovery Manager) to perform a flashback operation on a specific PDB. You can flashback to a specific restore point — an alias for system change number (SCN)— in the past without affecting other PDBs in a CDB. (Performing such an operation on a PDB is analogous to `FLASHBACK DATABASE` in a non-CDB.) 

You can also flashback a PDB on a standby. In effect, flashing back a PDB on a standby rewinds the data files for the PDB to a previous point in time, as if restoring a backup of the PDB. Flashing back a PDB on a standby allows the standby to quickly follow the primary after you perform a PDB PITR/flashback operation on the primary, as described in [Actions Needed On a Standby After a PDB PITR On a Primary](examples-of-using-oracle-data-guard.md#GUID-8D948A24-A3B7-4E4F-917A-00B047CF3CAF). 

> **note:** 

Files that are brought online or offline as a result of an `ALTER PLUGGABLE DATABASE [ENABLE | DISABLE]` operation remain in that state even if you flashback the database to a point before the operation was performed. 

> **note:** See Also: 

* [Actions Needed On a Standby After a PDB PITR On a Primary](examples-of-using-oracle-data-guard.md#GUID-8D948A24-A3B7-4E4F-917A-00B047CF3CAF) for information about actions needed on a standby after a PDB PITR on a primary 

* [Actions Needed on Standby After TSPITR or Tablespace Plugin at Primary](using-RMAN-in-oracle-data-guard-configurations.md#GUID-2B3CB228-F8AD-4FE0-8C3B-BEED91DFB424) for information about actions needed on a standby after TSPITR or tablespace plugin at the primary 




### Recommended RMAN and Oracle Database Configurations {#GUID-33B70E30-2454-484C-9110-A5AC452E63CB}

These configurations can simplify backup and recovery operations.

* [Oracle Database Configurations on Primary and Standby Databases](using-RMAN-in-oracle-data-guard-configurations.md#GUID-B7FFF73A-B8C0-49DD-8404-F6FE9F784C41)

* [RMAN Configurations at the Primary Database](using-RMAN-in-oracle-data-guard-configurations.md#GUID-2AEB88E7-075F-47F4-BBCE-77B274A50683)

* [RMAN Configurations at a Standby Database Where Backups are Performed](using-RMAN-in-oracle-data-guard-configurations.md#GUID-FBB66066-8FD4-4328-9AF2-0D7625803FDB)

* [RMAN Configurations at a Standby Where Backups Are Not Performed](using-RMAN-in-oracle-data-guard-configurations.md#GUID-F4CD9CD2-1006-409F-92D8-909114931B06)




Configuration Assumptions

These configurations make the following assumptions:

* The standby database is a physical standby database, and backups are taken only on the standby database. See [Standby Databases Too Geographically Distant to Share Backups](using-RMAN-in-oracle-data-guard-configurations.md#GUID-75F78440-2A7C-4FDC-AFF9-CABB440CB651) for procedural changes if backups are taken on both primary and standby databases. 

* An RMAN recovery catalog is required so that backups taken on one database server can be restored to another database server. It is not sufficient to use only the control file as the RMAN repository because the primary database has no knowledge of backups taken on the standby database.<br>The RMAN recovery catalog organizes backup histories and other recovery-related metadata in a centralized location. The recovery catalog is configured in a database and maintains backup metadata. A recovery catalog does not have the space limitations of the control file and can store more historical data about backups.<br>A catalog server, physically separate from the primary and standby sites, is recommended in an Oracle Data Guard configuration because a disaster at either site will not affect the ability to recover the latest backups.

> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV8015) for more information about managing a recovery catalog 

* Oracle Secure Backup software or 3rd-party media management software is configured with RMAN to make backups to tape.




#### Oracle Database Configurations on Primary and Standby Databases {#GUID-B7FFF73A-B8C0-49DD-8404-F6FE9F784C41}

These Oracle Database configurations are recommended on every primary and standby database in the Oracle Data Guard environment.

* Configure a fast recovery area for each database (the recovery area is local to a database).<br>The fast recovery area is a single storage location on a file system or Oracle Automatic Storage Management (Oracle ASM) disk group where all files needed for recovery reside. These files include the control file, archived logs, online redo logs, flashback logs, and RMAN backups. As new backups and archived logs are created in the fast recovery area, older files (which are either outside of the retention period, or have been backed up to tertiary storage) are automatically deleted to make room for them. In addition, notifications can be set up to alert the DBA when space consumption in the fast recovery area is nearing its predefined limit. The DBA can then take action, such as increasing the recovery area space limit, adding disk hardware, or decreasing the retention period.<br>Set the following initialization parameters to configure the fast recovery area:
```
DB_RECOVERY_FILE_DEST =
DB_RECOVERY_FILE_DEST_SIZE =
```


> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV8183) for more information about configuring a fast recovery area 

* Use a server parameter file (SPFILE) so that it can be backed up to save instance parameters in backups.

* Enable Flashback Database on primary and standby databases.<br>When Flashback Database is enabled, Oracle Database maintains flashback logs in the fast recovery area. These logs can be used to roll the database back to an earlier point in time, without requiring a complete restore. 

> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV595) for more information about enabling Flashback Database 




#### RMAN Configurations at the Primary Database {#GUID-2AEB88E7-075F-47F4-BBCE-77B274A50683}

To simplify ongoing use of RMAN, you can set a number of persistent configuration settings for each database in the Oracle Data Guard environment. 

These settings control many aspects of RMAN behavior. For example, you can configure the backup retention policy, default destinations for backups to tape or disk, default backup device type, and so on. You can use the `CONFIGURE` command to set and change RMAN configurations. The following RMAN configurations are recommended at the primary database: 

1. Connect RMAN to the primary database and recovery catalog.
2. Configure the retention policy for the database as *n* days:
```
CONFIGURE RETENTION POLICY TO RECOVERY WINDOW OF  DAYS;
```


This configuration lets you keep the backups necessary to perform database recovery to any point in time within the specified number of days. <br>Use the `DELETE` `OBSOLETE` command to delete any backups that are not required (per the retention policy in place) to perform recovery within the specified number of days. 

3. Specify when archived logs can be deleted with the `CONFIGURE ARCHIVELOG DELETION POLICY` command. For example, to delete logs after ensuring that they *shipped* to all destinations, use the following configuration:
```
CONFIGURE ARCHIVELOG DELETION POLICY TO SHIPPED TO ALL STANDBY;
```


To delete logs after ensuring that they were *applied* on all standby destinations, use the following configuration: 
```
CONFIGURE ARCHIVELOG DELETION POLICY TO APPLIED ON ALL STANDBY;
```


4. Configure the connect string for the primary database and all standby databases, so that RMAN can connect remotely and perform resynchronization when the `RESYNC CATALOG FROM DB_UNIQUE_NAME` command is used. When you connect to the target instance, you must provide a net service name. This requirement applies even if the other database instance from where the resynchronization is done is on the local host. The target and remote instances must use the same `SYSDBA` (or `SYSBACKUP`) password, which means that both instances must already have password files. You can create the password file with a single password so you can start all the database instances with that password file. For example, if the TNS alias to connect to a standby in Boston is `boston_conn_str`, you can use the following command to configure the connect identifier for the `BOSTON` database site:
```
CONFIGURE DB_UNIQUE_NAME BOSTON CONNECT IDENTIFIER 'boston_conn_str';
```


Note that the `'boston_conn_str'` does not include a username and password. It contains only the Oracle Net service name that can be used from any database site to connect to the `BOSTON` database site. <br>After connect identifiers are configured for all standby databases, you can verify the list of standbys by using the `LIST DB_UNIQUE_NAME OF DATABASE` command. 




> **note:** See Also: 

* [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV006) for more information about RMAN configurations 

* [*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF113) for more information about the RMAN `CONFIGURE` command 




#### RMAN Configurations at a Standby Database Where Backups are Performed {#GUID-FBB66066-8FD4-4328-9AF2-0D7625803FDB}

These RMAN configurations are recommended at a standby database where backups are done.

1. Connect RMAN to the standby database (where backups are performed) as target, and to the recovery catalog.
2. Enable automatic backup of the control file and the server parameter file:
```
CONFIGURE CONTROLFILE AUTOBACKUP ON;
```


3. Skip backing up data files for which there already exists a valid backup with the same checkpoint:
```
CONFIGURE BACKUP OPTIMIZATION ON;
```


4. Configure the tape channels to create backups as required by media management software:
```
CONFIGURE CHANNEL DEVICE TYPE SBT PARMS '';
```


5. Because the archived logs are backed up at the standby database, Oracle recommends that you configure the `BACKED UP` option for the log deletion policy:
```
CONFIGURE ARCHIVELOG DELETION POLICY BACKED UP n TIMES TO DEVICE TYPE SBT;
```





> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV89439) for more information about enabling deletion policies for archived redo logs 

#### RMAN Configurations at a Standby Where Backups Are Not Performed {#GUID-F4CD9CD2-1006-409F-92D8-909114931B06}

These RMAN configurations are recommended at a standby database where backups are *not* done. 

1. Connect RMAN to the standby database as target, and to the recovery catalog.
2. Enable automatic deletion of archived logs once they are applied at the standby database (this is also applicable to all terminal databases when the cascading or far sync instance features are in use):
```
CONFIGURE ARCHIVELOG DELETION POLICY TO APPLIED ON ALL STANDBY;
```





### Backup Procedures {#GUID-410FEFAD-7805-405A-8C02-B9D8F74C60F4}

You can use RMAN scripts and procedures to back up Oracle Database in an Oracle Data Guard configuration.

See the following topics:

* [Using Disk as Cache for Tape Backups](using-RMAN-in-oracle-data-guard-configurations.md#GUID-104006BF-5611-44C9-B892-87D140474DD3)

* [Performing Backups Directly to Tape](using-RMAN-in-oracle-data-guard-configurations.md#GUID-9102A173-7F1B-46B2-A750-E2599F74305F)




> **note:** 

Oracle's Maximum Availability Architecture (MAA) best practices recommend that backups be taken at both the primary and the standby databases to reduce MTTR, in case of double outages and to avoid introducing new site practices upon switchover and failover.

Backups of Server Parameter Files

All backup operations can be offloaded to a single standby database, except backups of the SPFILE. Backups of the SPFILE can only be restored to the database from which they were backed up.

For databases that are not backed up, Oracle recommends that you at least back up the SPFILE to a known local disk location. If the SPFILE backups need to be further backed up to tape, you can copy them to the database site where backups to tape have been configured. The SPFILE backups can then be cataloged at that database using the following RMAN command:
```
CATALOG START WITH '';
```


Then back up the SPFILE backups to tape:
```
BACKUP BACKUPSET ALL;
```


When the SPFILE needs to be restored for a specific database, the appropriate SPFILE backup is restored from disk or tape.

#### Using Disk as Cache for Tape Backups {#GUID-104006BF-5611-44C9-B892-87D140474DD3}

The fast recovery area on the standby database can serve as a disk cache for tape backup.

Disk is used as the primary storage for backups, with tape providing long term, archival storage. Incremental tape backups are taken daily and full tape backups are taken weekly. The commands used to perform these backups are described in the following sections.

Note that when the fast recovery area is full, an archived redo log that is reclaimable according to the fast recovery area rules may be automatically deleted by the fast recovery area to make space for other files.

**Related Topics**

* [Guaranteed Restore Points and Fast Recovery Area Space Usage](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV-GUID-36048C35-BCC7-4D1F-8E2A-C2254D732EEE)



##### Commands for Daily Tape Backups Using Disk as Cache {#GUID-88CA9C62-4AFF-4395-B47D-05A145E7EFAE}

When deciding on your backup strategy, Oracle recommends that you take advantage of daily incremental backups. 

Data file image copies can be rolled forward with the latest incremental backups, thereby providing up-to-date data file image copies at all times. RMAN uses the resulting image copy for media recovery just as it would use a full image copy taken at that system change number (SCN), without the overhead of performing a full image copy of the database every day. An additional advantage is that the time-to-recover is reduced because the image copy is updated with the latest block changes and fewer redo logs are required to bring the database back to the current state.

To implement daily incremental backups, a full database backup is taken on the first day, followed by an incremental backup on day two. Archived redo logs can be used to recover the database to any point in either day. For day three and onward, the previous day's incremental backup is merged with the data file copy and a current incremental backup is taken, allowing fast recovery to any point within the last day. Redo logs can be used to recover the database to any point during the current day.

The script to perform daily backups looks as follows (the last line, `DELETE ARCHIVELOG ALL` is only needed if the fast recovery area is not used to store logs): 
```
RESYNC CATALOG FROM DB_UNIQUE_NAME ALL;
RECOVER COPY OF DATABASE WITH TAG 'OSS';
BACKUP DEVICE TYPE DISK INCREMENTAL LEVEL 1 FOR RECOVER OF COPY WITH TAG 'OSS' DATABASE;
BACKUP DEVICE TYPE DISK DATAFILECOPY ALL NOT BACKED UP AT ANY SCN;
BACKUP DEVICE TYPE SBT ARCHIVELOG ALL;
BACKUP BACKUPSET ALL;
DELETE ARCHIVELOG ALL;
```


The standby control file is automatically backed up at the conclusion of the backup operation because the control file auto backup is enabled.

Explanations for what each command in the script does are as follows:

* `RESYNC CATALOG FROM DB_UNIQUE_NAME ALL`<br>Resynchronizes the information from all other database sites (primary and other standby databases) in the Oracle Data Guard setup that are known to recovery catalog. For `RESYNC CATALOG FROM DB_UNIQUE_NAME` to work, RMAN must be connected to the target using the Oracle Net service name and all databases must use the same password file. 

* `RECOVER COPY OF DATABASE WITH TAG 'OSS'`<br>Rolls forward level 0 copy of the database by applying the level 1 incremental backup taken the day before. In the example script just shown, the previous day's incremental level 1 was tagged `OSS`. This incremental is generated by the `BACKUP DEVICE TYPE DISK ... DATABASE` command. On the first day this command is run there is no roll forward because there is no incremental level 1 yet. A level 0 incremental is created by the `BACKUP DEVICE TYPE DISK ... DATABASE` command. Again on the second day there is no roll forward because there is only a level 0 incremental. A level 1 incremental tagged `OSS` is created by the `BACKUP DEVICE TYPE DISK ... DATABASE` command. On the third and following days, the roll forward is performed using the level 1 incremental tagged `OSS` created on the previous day. 

* `BACKUP DEVICE TYPE DISK INCREMENTAL LEVEL 1 FOR RECOVER OF COPY WITH TAG 'OSS' DATABASE`<br>Create a new level 1 incremental backup. On the first day this command is run, a base datafile copy is created. On the second and following days, this is a level 1 incremental.

* `BACKUP DEVICE TYPE DISK DATAFILECOPY ALL NOT BACKED UP AT ANY SCN`

Backs up only the data files that have not yet been backed up.

* `BACKUP DEVICE TYPE SBT ARCHIVELOG ALL`<br>Backs up archived logs to tape according to the deletion policy in place. <br>It is recommended that you set the recovery window for SBT to a value higher than the recovery window for disk.

* `BACKUP BACKUPSET ALL`<br>Backs up any backup sets created as a result of incremental backup creation.

* `DELETE ARCHIVELOG ALL`<br>Deletes archived logs according to the log deletion policy set by the `CONFIGURE ARCHIVELOG DELETION POLICY` command. If the archived logs are in a fast recovery area, then they are automatically deleted when more open disk space is required. Therefore, you only need to use this command if you explicitly want to delete logs each day. 




##### Commands for Weekly Tape Backups Using Disk as Cache {#GUID-66665A86-0B71-41FE-BED0-B9992A405C35}

To back up all recovery-related files to tape, use the RMAN `BACKUP RECOVERY FILES` command once a week. 

This ensures that all current incremental, image copy, and archived log backups on disk are backed up to tape.

#### Performing Backups Directly to Tape {#GUID-9102A173-7F1B-46B2-A750-E2599F74305F}

Oracle's Media Management Layer (MML) API lets third-party vendors build a media manager, software that works with RMAN and the vendor's hardware to allow backups to sequential media devices such as tape drives.

A media manager handles loading, unloading, and labeling of sequential media such as tapes. You must install Oracle Secure Backup or third-party media management software to use RMAN with sequential media devices.

Take the following steps to perform backups directly to tape, by default:

1. Connect RMAN to the standby database (as the target database) and recovery catalog.
2. Execute the `CONFIGURE` command as follows:
```
CONFIGURE DEFAULT DEVICE TYPE TO SBT;
```





In this scenario, full backups are taken weekly, with incremental backups taken daily on the standby database.

> **note:** See Also: 

[*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV132) for more information about how to configure RMAN for use with a media manager 

##### Commands for Daily Backups Directly to Tape {#GUID-110EF96F-1018-44FE-A7D4-6BED2786424B}

The RMAN commands used to perform daily backups directly to tape resynchronize the information from all other databases in the Oracle Data Guard environment.

Take the following steps to perform daily backups directly to tape:

1. Connect RMAN to the standby database (as target database) and to the recovery manager.
2. Execute the following RMAN commands:
```
RESYNC CATALOG FROM DB_UNIQUE_NAME ALL;
BACKUP AS BACKUPSET INCREMENTAL LEVEL 1 DATABASE PLUS ARCHIVELOG;
DELETE ARCHIVELOG ALL;
```





These commands also create a level 1 incremental backup of the database, including all archived logs. On the first day this script is run, if no level 0 backups are found, then a level 0 backup is created.

The `DELETE ARCHIVELOG ALL` command is necessary only if all archived log files are not in a fast recovery area. 

##### Commands for Weekly Backups Directly to Tape {#GUID-859B3851-D933-44E0-A114-5430EFD1F662}

One day a week, perform a weekly backup directly to tape.

Take the following steps:

1. Connect RMAN to the standby database (as target database) and to the recovery catalog.
2. Execute the following RMAN commands:
```
RESYNC CATALOG FROM DB_UNIQUE_NAME ALL;
BACKUP AS BACKUPSET INCREMENTAL LEVEL 0 DATABASE PLUS ARCHIVELOG;
DELETE ARCHIVELOG ALL;
```





These commands resynchronize the information from all other databases in the Oracle Data Guard environment, and create a level 0 database backup that includes all archived logs.

The `DELETE ARCHIVELOG ALL` command is necessary only if all archived log files are not in a fast recovery area. 

### Registering and Unregistering Databases in an Oracle Data Guard Environment {#GUID-9C54A243-6B84-4755-957E-AFC57B9BC3B6}

Only the primary database must be explicitly registered using the `REGISTER DATABASE` command. Do this after connecting RMAN to the recovery catalog and primary database as `TARGET`. 

A new standby is automatically registered in the recovery catalog when you connect to a standby database or when the `CONFIGURE DB_UNIQUE_NAME` command is used to configure the connect identifier. 

To unregister information about a specific standby database, you can use the `UNREGISTER DB_UNIQUE_NAME` command. When a standby database is completely removed from an Oracle Data Guard environment, the database information in the recovery catalog can also be removed after you connect to another database in the same Oracle Data Guard environment. The backups that were associated with the database that was unregistered are still usable by other databases. You can associate these backups with any other existing database by using the `CHANGE BACKUP RESET DB_UNIQUE_NAME` command. 

When the `UNREGISTER DB_UNIQUE_NAME` command is used with the `INCLUDING BACKUPS` option, the metadata for all the backup files associated with the database being unregistered is also unregistered from the recovery catalog. 

### Reporting in an Oracle Data Guard Environment {#GUID-D13E9A5E-5DBB-4DF4-8534-F1E16FA33618}

Use the RMAN `LIST`, `REPORT`, and `SHOW` commands with the `FOR DB_UNIQUE_NAME` clause to view information about a specific database. 

For example, after connecting to the recovery catalog, you could use the following commands to display information for a database with a DBID of 1625818158 and to list the database in the Oracle Data Guard environment. The `SET DBID` command is required only if you are not connected to a database as `TARGET`. The last three commands list archive logs, database file names, and RMAN configuration information for a database with a `DB_UNIQUE_NAME` of `BOSTON`. 
```
SET DBID 1625818158;
LIST DB_UNIQUE_NAME OF DATABASE;
LIST ARCHIVELOG ALL FOR DB_UNIQUE_NAME BOSTON;
REPORT SCHEMA FOR DB_UNIQUE_NAME BOSTON;
SHOW ALL FOR DB_UNIQUE_NAME BOSTON;
```


### Performing Backup Maintenance in an Oracle Data Guard Environment {#GUID-4B1696A8-1A90-426C-A620-D5F313365E54}

The files in an Oracle Data Guard environment (data files, archived logs, backup pieces, image copies, and proxy copies) are associated with a database through use of the `DB_UNIQUE_NAME` parameter. 

Therefore, it is important that the value supplied for `DB_UNIQUE_NAME` be unique for each database in an Oracle Data Guard environment. This information, along with file-sharing attributes, is used to determine which files can be accessed during various RMAN operations. 

File sharing attributes state that files on disk are accessible only at the database with which they are associated, whereas all files on tape are assumed to be accessible by all databases. RMAN commands such as `BACKUP` and `RESTORE`, as well as other maintenance commands, work according to this assumption. For example, during a roll-forward operation of an image copy at a database, only image copies associated with the database are rolled forward. The incremental backups on disk associated with that database and any incremental backups on tape are used to roll forward the image copies. Similarly, during recovery operations, only disk backups associated with the database and files on tape are considered as sources for backups. 

> **note:** See Also: 

[*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF001) for detailed information about RMAN commands 

#### Changing Metadata in the Recovery Catalog {#GUID-13FC0BEC-C528-47DA-95D5-35A67B3A97A1}

The RMAN `CHANGE` command can be used with various operands to change metadata in the recovery catalog. 

For example:

* Changing File Association From One Standby Database to Another<br>Use the `CHANGE` command with the `RESET DB_UNIQUE_NAME` option to alter the association of files from one database to another within an Oracle Data Guard environment. The `CHANGE` command is useful when disk backups or archived logs are transferred from one database to another and you want to use them on the database to which they were transferred. The `CHANGE` command can also change the association of a file from one database to another database, without having to directly connect to either database using the `FOR DB_UNIQUE_NAME` and `RESET DB_UNIQUE_NAME TO` options. 

* Changing the DB_UNIQUE_NAME Initialization Parameter for a Database<br>If the value of the `DB_UNIQUE_NAME` initialization parameter changes for a database, then the same change must be made in the Oracle Data Guard environment. The RMAN recovery catalog, after connecting to that database instance, knows both the old and new value for `DB_UNIQUE_NAME`. To merge the information for the old and new values within the recovery catalog schema, you must use the RMAN `CHANGE DB_UNIQUE_NAME` command. If the value of the `DB_UNIQUE_NAME` initialization parameter changes for a database, the same change must be made in RMAN so that it is aware of the new `DB_UNIQUE_NAME`. For example, perform the following steps to change the database with `DB_UNIQUE_NAME` of `BOSTON_A` to `BOSTON_B`: 

1. In the initialization parameter file or SQL, change the `DB_UNIQUE_NAME` initialization parameter from `BOSTON_A` to `BOSTON_B`. 

2. In RMAN, connect to any database in the Oracle Data Guard environment as target database and connect to the recovery catalog. Then execute the `CHANGE` command: 
```
CHANGE DB_UNIQUE_NAME FROM BOSTON_A TO BOSTON_B;
```


* Making Backups Unavailable or Removing Their Metadata<br>Use `CHANGE` command options such as `AVAILABLE`, `UNAVAILABLE`, `KEEP`, and `UNCATALOG` to make backups available or unavailable for restore and recovery purposes, and to keep or remove their metadata. 

> **note:** See Also: 

[*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF110) for more information about the RMAN `CHANGE` command 




#### Deleting Archived Logs or Backups {#GUID-38EB0163-6F07-4F89-9A17-FC21FD779785}

Use the RMAN `DELETE` command to delete backup sets, image copies, archived logs, or proxy copies. 

To delete only files that are associated with a specific database, you must use the `FOR DB_UNIQUE_NAME` option with the `DELETE` command. 

File metadata is deleted for all successfully deleted files associated with the current target database (or for files that are not associated with any known database). If a file could not be successfully deleted, you can use the `FORCE` option to remove the file's metadata. 

When a file associated with another database is deleted successfully, its metadata in the recovery catalog is also deleted. Any files that are associated with other databases, and that could not be successfully deleted, are listed at the completion of the `DELETE` command, along with instructions for you to perform the same operation at the database with which the files are associated (files are grouped by database). The `FORCE` option cannot be used to override this behavior. If you are certain that deleting the metadata for the non-deletable files will not cause problems, you can use the `CHANGE RESET DB_UNIQUE_NAME` command to change the metadata for association of files with the database and use the `DELETE` command with the `FORCE` option to delete the metadata for the file. 

> **note:** See Also: 

[*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF121) for more information about the RMAN `DELETE` command 

#### Validating Recovery Catalog Metadata {#GUID-5C9C8A2E-E23A-4D62-BE5B-2205B5E877CB}

Use the `CROSSCHECK` command to validate and update file status in the recovery catalog schema. 

Metadata for all files associated with the current target database (or for any files that are not associated with any database), is marked `AVAILABLE` or `EXPIRED` according to the results of the `CROSSCHECK` operation. 

If a file associated with another database is successfully inspected, its metadata in the recovery catalog is also changed to `AVAILABLE`. Any files that are associated with other databases, and that could not be inspected successfully, are listed at the completion of the `CROSSCHECK` command, along with instructions for you to perform the same operation at the database with which the files are associated (files are grouped by site). If you are certain of the configuration and still want to change status metadata for unavailable files, you can use the `CHANGE RESET DB_UNIQUE_NAME` command to change metadata for association of files with the database and execute the `CROSSCHECK` command to update status metadata to `EXPIRED`. 

> **note:** See Also: 

[*Oracle Database Backup and Recovery Reference*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=RCMRF119) for more information about the RMAN `CROSSCHECK` command 

### Recovery Scenarios in an Oracle Data Guard Environment {#GUID-44539CC5-0BAB-47F8-B51F-BFD0A1D6DA40}

These are some of recovery scenarios that can occur in an Oracle Data Guard environment.

* [Recovery from Loss of Files on the Primary or Standby Database](using-RMAN-in-oracle-data-guard-configurations.md#GUID-1F1D37FE-48A6-46D4-98DB-C0E3707E3BCF)

* [Recovery from Loss of Online Redo Log Files](using-RMAN-in-oracle-data-guard-configurations.md#GUID-7D1A6CBF-6E72-4700-87D8-64E534EFAA2B)

* [Incomplete Recovery of the Primary Database](using-RMAN-in-oracle-data-guard-configurations.md#GUID-E8430B01-A852-4EAF-A711-10F3211F36A3)

* [Actions Needed on Standby After TSPITR or Tablespace Plugin at Primary](using-RMAN-in-oracle-data-guard-configurations.md#GUID-2B3CB228-F8AD-4FE0-8C3B-BEED91DFB424)




#### Recovery from Loss of Files on the Primary or Standby Database {#GUID-1F1D37FE-48A6-46D4-98DB-C0E3707E3BCF}

You can restore and recover files over the network by connecting to a physical standby database that contains the required files.

This can be useful when you want to restore lost data files, control files, or tablespaces on a primary database using the corresponding files on the physical standby database. You can also use the same process to restore files on a physical standby database by using the primary database.

> **note:** 

In releases prior to Oracle Database 12*c*, to recover from loss of files on the primary, you used the RMAN recovery catalog, and the RMAN `BACKUP`, `CATALOG DATAFILE`, and `SWITCH DATAFILE` commands. To recover from loss of files on the standby, you used the `RESTORE` and `RECOVER` commands. Those methods are no longer necessary as of Oracle Database 12*c*. 

> **note:** See Also: 

* [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV680) for more information about using RMAN to restore and recover files over the network 




#### Recovery from Loss of Online Redo Log Files {#GUID-7D1A6CBF-6E72-4700-87D8-64E534EFAA2B}

If all online log members for the current `ACTIVE` group or for an inactive group which has not yet been archived are lost, then you must fail over to the standby database. 

Refer to [ Role Transitions](managing-oracle-data-guard-role-transitions.md#GUID-44F26D5F-E036-4ACD-B8F0-224F5498FBCE) for the failover procedure. 

For information about how to recover from the loss of online redo log files in other circumstances, see [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV229). 

#### Incomplete Recovery of the Primary Database {#GUID-E8430B01-A852-4EAF-A711-10F3211F36A3}

Incomplete recovery of the primary database is normally done in cases such as when the database is logically corrupted (by a user or an application) or when a tablespace or data file was accidentally dropped from database. 

Depending on the current database checkpoint SCN on the standby database instances, you can use one of the following procedures to perform incomplete recovery of the primary database. All the procedures are in order of preference, starting with the one that is the least time consuming. 

**Using Flashback Database**

Using Flashback Database is the recommended procedure when the Flashback Database feature is enabled on the primary database, none of the database files are lost, and the point-in-time recovery is greater than the oldest flashback SCN or the oldest flashback time. See [Using Flashback Database After Issuing an Open Resetlogs Statement](examples-of-using-oracle-data-guard.md#GUID-BAA5ED38-29A0-4E8D-8435-AA083D19C13E) for the procedure to use Flashback Database to do point-in-time recovery. 

**Using the standby database instance**

This is the recommended procedure when the standby database is behind the desired incomplete recovery time, and Flashback Database is not enabled on the primary or standby databases:

1. Recover the standby database to the desired point in time. Be sure to stop the managed redo process (MRP) before issuing the following command:
```
RECOVER DATABASE UNTIL TIME 'time';
```


Alternatively, incomplete recovery time can be specified using the SCN or log sequence number:
```
RECOVER DATABASE UNTIL SCN incomplete recovery SCN';
RECOVER DATABASE UNTIL LOGSEQ incomplete recovery log sequence number THREAD thread number;
```


2. Open the standby database in read-only mode to verify the state of database.<br>If the state is not what is desired, use the LogMiner utility to look at the archived redo log files to find the right target time or SCN for incomplete recovery. Alternatively, you can start by recovering the standby database to a point that you know is before the target time, and then open the database in read-only mode to examine the state of the data. Repeat this process until the state of the database is verified to be correct. If you recover the database too far (past the SCN where the error occurred) you cannot return it to an earlier SCN.

3. Activate the standby database using the SQL `ALTER DATABASE ACTIVATE STANDBY DATABASE` statement. This converts the standby database to a primary database, creates a new resetlogs branch, and opens the database. See [Recovering Through the OPEN RESETLOGS Statement](managing-oracle-data-guard-physical-standby-databases.md#GUID-02218512-31A6-461E-96B4-0B9997DCFC4A) to learn how the standby database reacts to the new reset logs branch. 




**Using the primary database instance**

If all of the standby database instances have already been recovered past the desired point in time and Flashback Database is not enabled on the primary or standby database, then this is your only option.

Use the following procedure to perform incomplete recovery on the primary database:

1. Use LogMiner or another means to identify the time or SCN at which all the data in the database is known to be good.
2. Using the time or SCN, issue the following RMAN commands to do incomplete database recovery and open the database with the `RESETLOGS` option (after connecting to catalog database and primary instance that is in `MOUNT` state):
```
RUN
{
SET UNTIL TIME 'time';
RESTORE DATABASE;
RECOVER DATABASE;
}
ALTER DATABASE OPEN RESETLOGS;
```





After this process, all standby database instances must be reestablished in the Oracle Data Guard configuration.

#### Actions Needed on Standby After TSPITR or Tablespace Plugin at Primary {#GUID-2B3CB228-F8AD-4FE0-8C3B-BEED91DFB424}

After an RMAN tablespace point-in-time recovery (TSPITR) is performed at the primary, the recovered data files have a new system change number (SCN), and are therefore treated like new data files at the primary.

These data files cannot be automatically created at the standby.

Likewise, when a new plugged in tablespace is added to the primary database, the data files are treated like new data files at the primary.

The managed redo process (MRP) at the standby stops when the Redo Apply process encounters creation of these new files. The required new data files must be copied and restored to the standby. You can do this using either backups or a direct copy from the primary. For example, to copy all files that belong to a tablespace that has undergone an RMAN TSPITR, you can use the following RMAN command: 
```
RMAN> RESTORE TABLESPACE  FROM SERVICE
```


The number of disk channels allocated is per RMAN configurations. So, if `CONFIGURE DEVICE TYPE DISK PARALLELISM 4` is executed, then 4 disk channels are used to pull the files from the primary database. 

When the new data files are available at the standby, restart the MRP to continue applying the logs.

> **note:** See Also: 

* [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV009) for more information about RMAN TSPITR 




### Additional Backup Situations {#GUID-211E08A4-C19E-4159-80B2-429431314278}

You can modify the backup procedures for other configurations, such as when the standby and primary databases cannot share backup files; the standby instance is only used to remotely archive redo log files; or the standby database filenames are different than the primary database.

#### Standby Databases Too Geographically Distant to Share Backups {#GUID-75F78440-2A7C-4FDC-AFF9-CABB440CB651}

If the standby databases are far apart from one another, then the backups taken on them may not be easily accessible by the primary system or other standby systems. 

Perform a complete backup of the database on all systems to perform recovery operations. The fast recovery area can reside locally on the primary and standby systems; it does not have to be the same for the primary and standby databases.

In this scenario, you can still use the general strategies described in [Recovery Scenarios in an Oracle Data Guard Environment](using-RMAN-in-oracle-data-guard-configurations.md#GUID-44539CC5-0BAB-47F8-B51F-BFD0A1D6DA40), with the following exceptions: 

* Backup files created by RMAN must be tagged with the local system name, and with `RESTORE` operations that tag must be used to restrict RMAN from selecting backups taken on the same host. In other words, the `BACKUP` command must use the `TAG *`system name`* option when creating backups; the `RESTORE` command must use the `FROM TAG` *`system name`* option; and the `RECOVER` command must use the `FROM TAG` *`system name`* ARCHIVELOG TAG` *`system name`* option. 

* Disaster recovery of the standby site:

1. Start the standby instance in the `NOMOUNT` state using the same parameter files with which the standby was operating earlier. 

2. Create a standby control file on the primary instance using the SQL `ALTER DATABASE CREATE STANDBY CONTROLFILE AS` *`filename`* statement, and use the created control file to mount the standby instance. 

3. Issue the following RMAN commands to restore and recover the database files:
```
RESTORE DATABASE FROM TAG 'system name';
RECOVER DATABASE FROM TAG 'system name' ARCHIVELOG TAG 'system name';
```


4. Restart Redo Apply.




The standby instance fetches the remaining archived redo log files.

#### Standby Database Does Not Contain Data Files, Used as a FAL Server {#GUID-BA288622-6D29-444E-ACD6-7CFC293C32D5}

The FAL server can be used as a backup source for all archived redo log files, thus off-loading backups of archived redo log files to the FAL server.

Use the same procedure described in [Backup Procedures](using-RMAN-in-oracle-data-guard-configurations.md#GUID-410FEFAD-7805-405A-8C02-B9D8F74C60F4), with the exception that the RMAN commands that back up database files cannot be run against the FAL server. 

#### Standby Database File Names Are Different From Primary Database {#GUID-75A2D339-D737-44A1-B0E0-3BF8443D2FA4}

The recovery catalog can resynchronize the file names from each standby database site. 

However, if the database filenames are not the same on the primary and standby databases that were never resynchronized, then the `RESTORE` and `RECOVER` commands you use are slightly different. To obtain the actual data file names on the standby database, query the `V$DATAFILE` view and specify the `SET NEWNAME` option for all the data files in the database: 
```
RUN
{
SET NEWNAME FOR DATAFILE 1 TO 'existing file location for file#1 from V$DATAFILE';
SET NEWNAME FOR DATAFILE 2 TO 'existing file location for file#2 from V$DATAFILE';
…
…
SET NEWNAME FOR DATAFILE n TO 'existing file location for file#n from V$DATAFILE';
RESTORE {DATAFILE <n,m,…> \| TABLESPACE tbs_name_1, 2, …\| DATABASE;
SWITCH DATAFILE ALL;
RECOVER DATABASE {NOREDO};
}
```


Similarly, you use the `SET NEWNAME` option of the RMAN `DUPLICATE` command to specify new filenames during standby database creation. Or you could set the `LOG_FILE_NAME_CONVERT` and `DB_FILE_NAME_CONVERT` parameters. 

> **note:** See Also: 

[Creating a Standby Database That Uses OMF or Oracle ASM](examples-of-using-oracle-data-guard.md#GUID-6FB574E5-2F3A-4C9E-A3F0-7C9154B0F540) for information about precedence rules when both the `DB_FILE_NAME_CONVERT` and `DB_CREATE_FILE_DEST` parameters are set on the standby 

### Restoring and Recovering Files Over the Network {#GUID-F0EB4F8E-7D2F-4674-9A5B-AABC368D8F11}

As of Oracle Database 12*c*, RMAN lets you restore or recover files by connecting, over the network, to a physical standby database that contains the required files. 

You can restore an entire database, data files, control files, spfile, or tablespaces. Restoring files over the network is very useful in scenarios where you need to synchronize the primary and standby databases.

RMAN restores database files, over the network, from a physical standby database by using the `FROM` `SERVICE` clause of the `RESTORE` command. The `FROM` `SERVICE` clause provides the service name of the physical standby database from which the files must be restored. During the restore operation, RMAN creates backup sets, on the physical standby database, of the files that need to be restored and then transfers these backup sets to the target database over the network. 

> **note:** 

In releases prior to Oracle Database 12*c*, to restore and recover files over the network, you used the RMAN `BACKUP INCREMENTAL FROM SCN` command to create a backup on the primary database that started at the current SCN of the standby, and was then used to roll the standby database forward in time. That manual, multi-step method is not necessary as of Oracle Database 12*c*. 

> **note:** See Also: 

* [*Oracle Database Backup and Recovery User's Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV680) for more information about using RMAN to restore and recover files over the network 

* My Oracle Support note 2005729.1 at [`http://support.oracle.com`](http://support.oracle.com) for information about reducing transportable tablespace downtime using cross-platform incremental backups. 




### Rolling Forward a Standby With One Command {#GUID-53AF8403-7ECC-4329-966E-965FDBFB4455}

As of Oracle Database 18c, you can refresh a standby database over the network using one RMAN command, `RECOVER STANDBY DATABASE`. 

The` RECOVER STANDBY DATABASE` command restarts the standby instance, refreshes the control file from the primary database, and automatically renames data files, temp files, and online logs. It restores new data files that were added to the primary database and recovers the standby database up to the current time. 

When you use the `RECOVER STANDBY DATABASE` command to refresh a standby database, you specify either a `FROM SERVICE` clause or a `NOREDO` clause. The `FROM SERVICE` clause specifies the name of a primary service. The `NOREDO` clause specifies that backups should be used for the refresh, which allows a standby to be rolled forward to a specific time or SCN. 

The MRP must be manually stopped on the standby before any attempt is made to sync with primary database.

The following is an example of using the `RECOVER STANDBY DATABASE` command. It shows optional usage of the `PFILE` clause to specify a parameter file for the standby database (used when the spfile is not available). 
```
RECOVER STANDBY DATABASE FROM SERVICE service_name PFILE=pfile_location;
```


> **note:** See Also: 

* [*Oracle Database Backup and Recovery User’s Guide*](https://docs.oracle.com/pls/topic/lookup?ctx=en/database/oracle/oracle-database/23/sbydb&id=BRADV-GUID-828A4953-3F7E-4975-ADA4-5469D6794BB0) for more information about using the `RECOVER STANDBY DATABASE` command to refresh a physical standby database with changes made to the primary database 


